## Upleveling Flyte’s Data Lineage Using Dolt

By: Max Hoffman (*Software Engineer at Dolthub*)

Dolt and Flyte joined forces to build two data integrations. Dolt is a SQL database that supports Git Versioning. Flyte is a workflow orchestrator for creating and evolving machine learning processes and mission-critical data. When combined, Dolt extends Flyte’s data lineage to storage layer versioning.

[Flytekit release (v0.19.0)](https://github.com/flyteorg/flytekit/releases/tag/v0.19.0) includes SQLAlchemy and Dolt plugins, two contributions that expand Flyte’s ability to integrate with relational databases. In this blog, we explain what Flyte is, what Dolt is, and how the two intersect to introduce a new way of thinking about reproducibility in machine learning workflows.

# What is Flyte?
At its core, Flyte is an orchestrator responsible for quarterbacking the data and compute infrastructure of an enterprise. At a 10k foot view, Flyte is a Kubernetes (K8S) cluster that accepts, executes, and records machine learning and data processing workflows. Flyte can run everything from Python functions to Spark jobs, Sagemaker jobs, and map workflows with thousand-task fan-outs. Jobs are submitted as K8S custom resource definitions (CRD), YAML files that support arbitrary language SDK pluggability. Flyte also comes with a handful of tools to support admins and task monitoring.

Flyte has invested heavily into data reproducibility and versioning. Data dependencies are hashed and recorded as metadata. Task lineage is compiled and saved on or before the execution time. Shared tasks are cached to short-circuited workflows and optimize performance. Updates to tasks themselves are versioned and recorded. In a scenario where one wants to know the changes introduced in the data, Dolt’s data versioning can help enhance Flyte’s data lineage.

## Flyte Data Typing
Flyte’s type system provides a backbone for reproducibility and versioning. Extra work upfront defining schemas comes to bear in the form of better data quality and metadata management when compiling and executing compute graphs. This comes full circle later with Dolt’s SQL versioning, the same principles applied at the row layer.

The example below, adapted from an [XGBoost tutorial in the Flyte documentation](https://docs.flyte.org/projects/cookbook/en/latest/auto/case_studies/ml_training/pima_diabetes/diabetes.html#sphx-glr-auto-case-studies-ml-training-pima-diabetes-diabetes-py) shows the type system in action.

```python
@task
def my_task(dataset: FlyteFile[typing.TypeVar("csv")]) -> pandas.DataFrame:
    return pandas.read_csv(dataset)


@workflow
def my_workflow(
    dataset: FlyteFile[
        typing.TypeVar("csv")
    ] = "https://raw.githubusercontent.com/jbrownlee/Datasets/master/pima-indians-diabetes.data.csv",
) -> int:
    df = my_task(dataset=dataset)
    return df.shape[0]
``` 

Without digging into the weeds too much, the input source (`dataset`) switches between two identities. In `my_workflow`, `dataset` is a static `FlyteFile` type:

```text
scalar {
  blob {
    metadata {
      type {
        format: "csv"
      }
    }
    uri: "https://raw.githubusercontent.com/jbrownlee/Datasets/master/pima-indians-diabetes.data.csv"
  }
}
```

At execution time, the orchestrator downloads this file, records an md5 hash and other metadata, and injects a Python path object into `my_task`:

```bash
In[1]: df = pandas.read_csv(dataset, header=None)
In[2]: df.head()
   0    1   2   3    4     5      6   7  8
0  6  148  72  35    0  33.6  0.627  50  1
1  1   85  66  29    0  26.6  0.351  31  0
2  8  183  64   0    0  23.3  0.672  32  1
3  1   89  66  23   94  28.1  0.167  21  0
4  0  137  40  35  168  43.1  2.288  33  1
```

Rigid typing is an interesting point of differentiation between Flyte and other workflow managers, and key for extending Flyte with data plugins like Dolt.

# What is Dolt?
Dolt is a novel relational database with Git versioning. Dolt can do everything MySQL does, including act as a drop-in replacement for application databases. That is one of the reasons we added Flyte’s SQLAlchemy plugin. The select statement below performs the same whether a MySQL RDS or Dolt server is running on `uri`:

```text
SQLAlchemyTask(
    name="test-task",
    query_template="select * from my_table",
    task_config=SQLAlchemyConfig(
        uri=mysql://user:password@localhost/db",
    ),
)
```
Dolt was designed from the ground up for versioning data. Dolt’s range of Git features includes cloning, branching, merging, and diffing databases. Dolt databases live in folders like git repositories, and all the `git` commands you know for version control work exactly the same on the command line in a dolt directory.

Dolt takes Flyte’s versioning one step further, letting users understand how data sources evolve over time. Two table versions can be diffed to see row-level changes. Branches can isolate experiments from others’ work. Data can be queried by commit version, timestamp, or branch name without leaving SQL. Recording execution IDs in dolt commits helps audit data changes.

# How It Works
## Setup
Using the Flyte-Dolt Plugin will require Flyte and Dolt dependencies. We will use a `flytesnacks` [tutorial](https://github.com/flyteorg/flytesnacks/tree/master/cookbook/integrations/flytekit_plugins/dolt) for this demo. To follow along, first clone `flytesnacks` and navigate to the dolt cookbook:

```bash
$ git clone https://github.com/flyteorg/flytesnacks
$ cd flytesnacks/cookbook/integrations/flytekit_plugins/dolt
```

We will need several Python dependencies, including `flytekitplugins-dolt`:

```bash
$ pip install -r requirements.txt
```

We will also need to install `dolt`:

```bash
$ sudo bash -c 'curl -L https://github.com/dolthub/dolt/releases/latest/download/install.sh | sudo bash'
```

Our workflow reads from a `foo` database, which can be initialized locally:

```bash
$ mkdir foo
$ cd foo
$ dolt init
```

And we are ready to get started! To test the plugin, run `quickstart_example.py`, a short workflow that creates, writes, and reads a dataframe:

```bash
$ python quickstart_example.py 1
Running quickstart_example.py main...
Running wf(), returns dataframe
          name  count
0  Sugar Maple      3
1        Alice      2
2       George      1
name     object
count     int64
dtype: object
```

## Diffing
This first workflow is comprised of two tasks: `populate_rabbits` and `unwrap_rabbits`:

```python
@task
def populate_rabbits(a: int) -> DoltTable:
    rabbits = [("George", a), ("Alice", a * 2), ("Sugar Maple", a * 3)]
    df = pd.DataFrame(rabbits, columns=["name", "count"])
    return DoltTable(data=df, config=rabbits_conf)


@task
def unwrap_rabbits(table: DoltTable) -> pd.DataFrame:
    return table.data
```

`populate_rabbits` creates a dataframe and stores the table in Dolt. `unwrap_rabbits` does the exact opposite, reading the table from Dolt and returning a DataFrame. Our workflow combines these two steps:

```python
@workflow
def wf(a: int) -> pd.DataFrame:
    rabbits = populate_rabbits(a=a)
    df = unwrap_rabbits(table=rabbits)
    return df
```

It might appear that this workflow accomplishes little. Any Python script can attach a `data` attribute to an object and immediately read it back. Flyte’s backend and typing system perform a lot of work under the hood to save the data to Dolt via this configuration:

```python
rabbits_conf = DoltConfig(
    db_path=os.path.join(os.path.dirname(__file__), "foo"),
    tablename="rabbits",
)
```

Running the workflow again with a different hyperparameter saves a different table:

```bash
$ python quickstart_example.py 2
Running quickstart_example.py main...
Running wf(), returns dataframe
          name  count
0        Alice      4
1       George      2
2  Sugar Maple      6
name     object
count     int64
dtype: object
```

which we can view by inspecting the `foo` database:

```bash
$ cd foo
$ dolt diff HEAD HEAD^ rabbits
diff --dolt a/rabbits b/rabbits
--- a/rabbits @ 61q42dk4ft4f16rh10m9u2as8ko1fr0g
+++ b/rabbits @ ho8q4rnqle80g3ucfbukf9gp4ichv4bl
+-----+-------------+-------+
|     | name        | count |
+-----+-------------+-------+
|  -  | Alice       | 4     |
|  -  | George      | 2     |
|  +  | Sugar Maple | 3     |
|  -  | Sugar Maple | 6     |
|  +  | Alice       | 2     |
|  +  | George      | 1     |
+-----+-------------+-------+
```

Diffing can be useful productivity or debugging tool. Histograms and distributional shifts help understand the difference between two datasets, but it is also useful to know what rowsare shared at a more granular level. Dolt’s structural sharing is the only way to get efficient row-level diffs of data.

## Branching
A second example isolates workflow runs using Dolt branches.

The configuration here is more complicated. We will create new branches for different hyperparameter values:

```python
def generate_confs(a: int) -> typing.Tuple[DoltConfig, DoltConfig, DoltConfig]:
    users_conf = DoltConfig(db_path=doltdb_path, tablename="users", branch_conf=NewBranch(f"run/a_is_{a}"))

    query_users = DoltTable(
        config=DoltConfig(
            db_path=doltdb_path,
            sql="select * from users where `count` > 5",
            branch_conf=NewBranch(f"run/a_is_{a}"),
        ),
    )

    big_users_conf = DoltConfig(
        db_path=doltdb_path,
        tablename="big_users",
        branch_conf=NewBranch(f"run/a_is_{a}"),
    )

    return users_conf, query_users, big_users_conf
```

In this example, we have `users` and `big_users` tables. The first and last config reference their respective `tablename`s. The middle config, `query_users`, is a read-only config that selects `users` whose count value is greater than 5.

The workflow has two logical steps after populating the `users` database: filtering the data using our query configuration, and then counting the number of rows returned by the query. `populate_users` and `filter_users` are identical to the previous example, [refer to the source for the full code](https://github.com/flyteorg/flytesnacks/tree/master/cookbook/integrations/flytekit_plugins/dolt/branch_example.py):

```python
@workflow
def wf(a: int) -> int:
   user_conf, query_conf, big_user_conf = generate_confs(a=a)
   users = populate_users(a=a, conf=user_conf)
   big_users = filter_users(a=a, all_users=users, filtered_users=query_conf, conf=big_user_conf)
   big_user_cnt = count_users(users=big_users)
   return big_user_cnt
```

We will run this workflow twice:

```bash
$ python branch_example.py 2
Running branch_example.py main...
Running wf(), returns int
1
<class 'int'>
$ python branch_example.py 3
Running branch_example.py main...
Running wf(), returns int
2
<class 'int'>
```

Which creates distinct branches for our two `a` values:

```bash
$ cd foo
$ dolt branch
* master
  run/a_is_2
  run/a_is_3
```

As in the first example, we can still diff the two runs:

```bash
$ dolt diff run/a_is_2 run/a_is_3
diff --dolt a/big_users b/big_users
--- a/big_users @ 0o5jmq18cmrop7u1otc9mppo2bgraecc
+++ b/big_users @ rgerfd58r0oko1tfn3d1h0kh4a5f4s0v
+-----+-------+
|     | name  |
+-----+-------+
|  +  | Alice |
+-----+-------+
diff --dolt a/users b/users
--- a/users @ t1d0htq6upsbs302s8olga9jdq34bnqe
+++ b/users @ dn31t9b8ssien91nm7lhup22jda45sdt
+-----+-----------+-------+
|     | name      | count |
+-----+-----------+-------+
|  -  | George    | 2     |
|  -  | Stephanie | 6     |
|  +  | George    | 3     |
|  +  | Stephanie | 9     |
|  +  | Alice     | 6     |
|  -  | Alice     | 4     |
+-----+-----------+-------+
```

Flyte’s caching prevents unnecessary task re-runs, but Dolt will also no-op if a workflow rerun creates no data mutations:

```bash
$ python branch_example.py 3
$ dolt log run/a_is_3
```

## Time Travel
Dolt stores data as a commit graph, just like Git. This means you can query data by branch name, as in the last example, or by timestamp or commit hash.

If we remember the `query_users` config from the last example:

```python
config=DoltConfig(
    db_path=doltdb_path,
    sql="select * from users where `count` > 5",
    branch_conf=NewBranch(f"run/a_is_{a}"),
)
```

We notice that the query is just a MySQL select statement. Dolt supports `AS OF` syntax in this statement, pointing to the table at a specific point in time. We use the command line to test how this works:

```bash
$ dolt sql --query "select * from users as of HASHOF('run/a_is_3') where `count` > 5"
+-----------+-------+
| name      | count |
+-----------+-------+
| Stephanie | 9     |
| Alice     | 6     |
+-----------+-------+
```

`HASHOF('run/a_is_3')` is a quick way to get the most recent commit of the `run/a_is_3` branch. If you wanted to pass the specific commit string, you can use these helpers:

```bash
$ dolt sql --query “select HASHOF('run/a_is_3') as commit”
+----------------------------------+
| commit                           |
+----------------------------------+
| np2l4f9lq4g5bpl3ghjhbgfo3lv3plkt |
+----------------------------------+

$ dolt log run/a_is_3 -n 1
commit np2l4f9lq4g5bpl3ghjhbgfo3lv3plkt
Author: Bojack Horseman <bojack@horseman.com>
Date:   Fri May 07 12:38:03 -0700 2021

    Generated by Flyte execution id: ex:local:local:local
```

There are several ways you could use time travel and asof queries:
- Fetch the most recent data in a specific branch
- Input the 5 most recent versions of a table
- Only input newly added rows in the last commit
- Input data from exactly one month ago

# Summary
We have discussed Flyte’s workflow automation software, Dolt’s versioned database, and why you might use Dolt in lieu of static CSV’s to version your data pipelines. We demonstrated a branching organization that creates different workspaces for different versions of an execution run, and how to perform diffs and time travel between those branches.

Dolt schemas are version controlled and gate check row entry, extending Flyte's typing assertions through the data layer. Dolt's branch and merge features are the only efficient solution for collaborating between isolated data workspaces, which when paired with Dolt's lineage history creates a new breed of data catalog. Dolt's storage layer is the only data structure that supports efficient diffing while maintaining decades of B tree optimizations, which lets you avoid manually trying to find the difference between `train_final_v5.csv` and `train_final_actually_v2.csv` when something breaks in production.

If you are interested in learning more about [Dolt](https://discord.com/invite/RFwfYpu) or [Flyte](https://docs.google.com/forms/d/e/1FAIpQLScWPJZncyL-bBCWbuQ9HWuMzA8-r2RJc__CSWJoAYUQADN-BQ/viewform) please reach out to us in our community channels! We would love to hear about your experiences with data pipelines.

