## Build Indestructible Pipelines With Flyte

We all love “undoing” when things don’t work as expected. Imagine the pain we would have to experience without such an option; we can’t just revert to or pull from what we have been working on before. 

From the perspective of machine learning or data pipelines, going back in time to access multiple versions of our work is of utmost importance because we do not want to reinvent the wheel all the time. ML is a vast area where we can churn tons of data across multiple pipelines and algorithms. Likewise, data pipelines deal with lots of data processing. In such a case, we ought to meticulously manage our work (at the least, code and data), and due to the inherent complexity of ML, this task gets even more challenging.

Here are the two use cases that explain the complexity of time travel in ML/data pipelines:

# Use Cases
## Reproducibility

We often want to **reproduce** our ML pipelines to ensure our models’ and results’ correctness, consistency, and reliability. Here are some of its challenges:

* ML may use non-deterministic algorithms, which may not always produce the same outputs given the same input. Thus, **we have to store the algorithm (and its metadata) in use**.
* Initial model weights can be randomly initialized. Likewise, hyperparameters may differ. **So, we have to make a note of the hyperparameters**.
* To ensure that every ML run is reproducible, **code, parameters, and operating conditions need to be captured**.
* ML can work differently in local and prod environments. **Containerization using Docker can help in maintaining the consistency of the results**.
* Data needs to be tracked to ensure that the same data is being used during the reproducibility phase. Thus, **data needs to be versioned and stored**. 

## Recoverability
Now, what if we want to recover our ML pipelines? If a failure pops up, we typically don’t want to start right from the first step; instead, we may want to say, fetch the last known failure point to continue running the job from that specific checkpoint with all the successful jobs restored, which assuredly, saves a lot of time! 

This technique is useful when:
* Broad system failures occur
* Third-party plugin connections fail 
* Downstream errors or failures last for an extended period, and thus, we want to recover once the downstream system stabilizes
* Failures are unavoidable

Recoverability involves caching the results of the runs that have happened to date. This again involves **caching a whole lot of data and metadata detailing the runs**. 

This is just the tip of the iceberg. Lots of such metadata needs to be taken care of to resolve these challenges. 

Imagine this story: You’re a data scientist. You come across a customer segmentation problem 🧐. You pull, analyze, visualize the data, develop a model, and run it 👾. Everything seems to work perfectly 💯! 
All of a sudden, a node on which your run is scheduled goes down, and the run stops 😓. You add a retries mechanism to retry the job a certain number of times to solve the issue 😃. The job runs smoothly now 👌. However, you ponder the time that goes to waste to run the exact same job, again and again, so you cache the data 📦.
Sadly, after a certain period, the run doesn’t stop due to an underlying issue 😤. You add timeout 🕐.
Again, an infrastructure error causes an abrupt run failure 🔴. You lose all your progress 😱. To prevent loss of data, you add a recovery mechanism 😃. 

Conclusion: You get exhausted to bring about all the changes to improve the resiliency of your code and infrastructure 😥! 

Meet FLYTE! It indeed makes our lives easier. Both reproducibility and recoverability are inherently supported within Flyte in multiple ways. The user needs to make some very trivial changes for the effect to kick off. You need not worry about the code and infrastructural maintenance at all.

In the next section, let’s see the various possibilities and the steps one has to take to reap the benefits.

# User and System Retries
Flyte’s retry feature can help in retrying a task or node if there’s a failure.

Flyte supports both user and system retries. 

## User Retry
User retry is consumed when the system behaves expectedly and detects that the prior attempt of the task failed. It can be specified as follows:

```python
@task(retries=2, ...)
def my_task() -> ...:
    ...
```

When `my_task` fails with a recoverable failure, Flyte retries this task at least two times.

**Note**: Recoverable vs. Non-Recoverable failures: Recoverable failures will be retried and counted against the task’s retry count. Non-recoverable failures will just fail, i.e., the task isn’t retried regardless of user/system retry configurations. All user exceptions are considered non-recoverable unless the exception is a subclass of `FlyteRecoverableException`. 

`retries` mechanism is handled by  [flytekit.TaskMetadata](https://docs.flyte.org/projects/flytekit/en/latest/generated/flytekit.TaskMetadata.html?highlight=retries#flytekit.TaskMetadata) .

**Note**: The number of retries must be less than or equal to 10.

## System Retry
System retry is a system-defined retry, i.e., the retry count is validated against the number of system retries. This is useful when system failures occur. 

**Note**: System failures are always regarded as recoverable.

System retry can be of two types:

* **Downstream System Retry**: When a downstream system (or service) fails, or remote service is not contactable, the failure is retried against the number of retries set [here](https://github.com/flyteorg/flytepropeller/blob/6a14e7fbffe89786fb1d8cde22715f93c2f3aff5/pkg/controller/config/config.go#L192). This does end-to-end system retry against the node whenever the task fails with a system error. This is useful when the downstream service throws a 500 error, abrupt network failure happens, etc.
* **Transient Failure Retry**: This retry mechanism offers resiliency to transient failures, which are opaque to the user. It is tracked across the entire execution for the duration of the execution. It helps Flyte entities and the additional services connected to Flyte like S3 to continue operating despite a system failure. **Indeed, all transient failures are handled gracefully by Flyte!** Moreover, in case of a transient failure retry, Flyte does not necessarily retry the entire task. “Retrying an entire task” means that the entire pod associated with Flyte task is rerun with a clean slate; instead, it just retries the atomic operation. For example, it keeps trying to persist the state until it can, exhausts the max retries, and backs off. 
To set a transient failure retry:

    * Update [MaxWorkflowRetries](https://github.com/flyteorg/flytepropeller/blob/f1b0163b0b88200b38a5d49af955490e5c98681d/pkg/controller/config/config.go#L55) in the propeller configuration
    * Or update [max-workflow-retries](https://github.com/flyteorg/flyte/blob/33f179b807093dcad2f37bde832869103bdf5182/charts/flyte/values-sandbox.yaml#L143) in helm


# Timeouts
For the system to ensure it’s always making progress, tasks must be guaranteed to end. The system defines a default timeout period for tasks. It’s also possible for task authors to specify a timeout period after which the task is marked as a failure. 

**Note**: When retry and timeout are assigned to a task, the timeout applies to every retry.

`timeout` can be initialized as follows:

```python
@task(
    timeout=datetime.timedelta(minutes="5"),
    ...
)
def my_task() -> ...:
    ...
```

The task execution will be terminated if the runtime exceeds five minutes.

`timeout` mechanism is handled by  [flytekit.TaskMetadata](https://docs.flyte.org/projects/flytekit/en/latest/generated/flytekit.TaskMetadata.html?highlight=retries#flytekit.TaskMetadata).

# Caching/Memoization
Flyte supports caching or memoization of task outputs to ensure identical invocations of a task are not repeatedly executed, which would otherwise result in longer execution times and wastage of computing resources.

Task caching is useful when the same execution needs to be repeated. For example, consider the following scenarios:

* Running a task periodically on a schedule
* Running the code multiple times when debugging workflows
* Running the commonly-shared tasks in different workflows, which receive the same inputs
* Caching can be useful when training a model; you may want to modify the hyperparameters but not the features. In such a case, re-running the workflow should suffice, where all the features should be re-usable if they are marked to be memoized.

The cache can be enabled using the following declaration:

```python
@task(cache=True, cache_version="1.0")
def my_task(...) -> ...:
    ...
```

`cache_version` field indicates that the task functionality has changed. Bumping the `cache_version` is similar to invalidating the cache.

To know more about how caching works, refer to: https://docs.flyte.org/projects/cookbook/en/latest/auto/core/flyte_basics/task_cache.html#how-the-caching-works. 

Refer to: https://docs.flyte.org/en/latest/concepts/catalog.html#how-flyte-memoizes-task-executions-on-data-catalog if you’d like to dive deeper into how memoization is handled in the data catalog service.

# Recovery Mode
Without discovery caching enabled, recovering workflows is manual and tedious. This is where recovery mode comes into the picture. In Flyte, recovery mode allows us to recover an individual execution by copying all successful node executions and running from the failed nodes. This mechanism is handy to recover from system errors and byzantine faults like loss of Kubernetes cluster, bugs in the platform or when the platform is unstable, machine failures, downstream system failures (downstream services), or simply to recover executions that failed because of retry exhaustion and should complete if tried again. 

To recover a node in Flyte, follow these two steps:

* Fetch the execution ID of your run
* If there’s a failure, recover and trigger the execution using the following command:

```bash
flytectl create execution --recover execution_id -p flytesnacks -d development
```

**To make things easier, a “recover” button has been added to the Flyte Console. Go check it out!**

Soon enough, you'll be able to script batch recovery for a list of executions in Flytekit.

**Note**: In recovery mode, users cannot change any input parameters or update the version of the execution. 

# Conclusion
Reproducibility and recoverability can help in avoiding significant errors and costs. It facilitates tracking of the results and strengthens team collaboration. Flyte is one such tool that encompasses these features. We’d love you to explore and give us some feedback. 

Join our [community](https://docs.google.com/forms/d/e/1FAIpQLScWPJZncyL-bBCWbuQ9HWuMzA8-r2RJc__CSWJoAYUQADN-BQ/viewform) and follow us on [Twitter](https://twitter.com/flyteorg) to receive updates about new features, improvements, integrations, and a lot more! 

<hr/>

Cover Image Credits: Photo by [Mike Koss](https://unsplash.com/@mckoss) on [Unsplash](https://unsplash.com/)
