## Data Quality Enforcement Using Great Expectations and Flyte

**We’re excited to announce a new integration between Great Expectations and Flyte. We believe this is a much deeper and intuitive integration than ever before!**

**[Great Expectations](https://greatexpectations.io/)** is a Python-based open-source library for validating, documenting, and profiling your data. It helps to maintain data quality and improve communication about data between teams.

*If you haven’t worked with Great Expectations before, jump right into the [getting started tutorial](https://docs.greatexpectations.io/docs/tutorials/getting_started/intro/).*

The goodness of data validation in Great Expectations can be integrated with Flyte to validate the data moving in and out of the pipeline entities you may have defined in Flyte. This helps establish stricter boundaries around your data to ensure that everything works as expected and data does not crash your pipelines anymore unexpectedly!

# The Idea

We have entered into a data-first world. Data is often a product and is critical for running a business. As for software services, we have created multiple systems to manage the quality and ensure rigor and correctness at every step. This is not true with data systems. Data can be large, spread out, and sometimes duplicated. We have to enforce quality checks on the data. 

*It is hard and expensive to enforce quality after data is generated, and often, this may yield un-recoverable results.*

**At Flyte, we believe that data quality is not an afterthought but should be an integral part of the data definition and generation process**. We may have multiple tasks and workflows where infinite bits of data pass through the pipelines. Keeping an eye on data all the time isn’t a feasible solution. What we need is an automated mechanism that validates our data thoroughly.

This is why Flyte has a [native type system](https://docs.flyte.org/projects/cookbook/en/latest/auto/core/type_system/flyte_python_types.html) that enforces the correctness of data. However, a type system alone doesn’t suffice; we also need a comprehensive data validation tool. 

**This is where Great Expectations comes into the picture. It can help take Flyte’s data handling system to the next level.**

With the Great Expectations and Flyte integration, we can now:
- Make the Flyte pipelines more robust and resilient
- Enforce validation rules on the data
- Eliminate bad data
- Not worry about unexpected data-related crashes in the Flyte pipelines
- Prevent data quality issues
- Validate the data pre-ingestion (data going into the Flyte pipeline) and post-ingestion (data coming out of the Flyte pipeline)
- Ensure that new data isn’t out of line with the existing data

# How to Define the Integration

Great Expectations supports native execution of expectations against various [Datasources](https://docs.greatexpectations.io/docs/tutorials/getting_started/connect_to_data#configuring-datasources), such as Pandas dataframes, Spark dataframes, and SQL databases via SQLAlchemy.

We’re supporting two Flyte types (along with the primitive “string” type to support files) that should suit Great Expectations’ Datasources:
- [flytekit.types.file.FlyteFile](https://docs.flyte.org/projects/flytekit/en/latest/generated/flytekit.types.file.FlyteFile.html#flytekit.types.file.FlyteFile): FlyteFile represents an automatic persistence object in Flyte. It can represent files in remote storage, and Flyte will transparently materialize them in every task execution.
- [flytekit.types.schema.FlyteSchema](https://docs.flyte.org/projects/flytekit/en/latest/generated/flytekit.types.schema.FlyteSchema.html#flytekit.types.schema.FlyteSchema): FlyteSchema supports tabular data, which the plugin will convert into a parquet file and validate the data using Great Expectations.

Flyte types have been added because, in Great Expectations, we have the privilege to give a non-string (Pandas/Spark DataFrame) when using a [RuntimeDataConnector](https://docs.greatexpectations.io/docs/reference/datasources#runtimedataconnector-and-runtimebatchrequest) but not when using an [InferredAssetFilesystemDataConnector](https://legacy.docs.greatexpectations.io/en/latest/autoapi/great_expectations/datasource/data_connector/inferred_asset_filesystem_data_connector/index.html?highlight=InferredAssetFilesystemDataConnector#great_expectations.datasource.data_connector.inferred_asset_filesystem_data_connector.InferredAssetFilesystemDataConnector) or a [ConfiguredAssetFilesystemDataConnector](https://legacy.docs.greatexpectations.io/en/latest/autoapi/great_expectations/datasource/data_connector/configured_asset_filesystem_data_connector/index.html#great_expectations.datasource.data_connector.configured_asset_filesystem_data_connector.ConfiguredAssetFilesystemDataConnector). For the latter case, with the integration of Flyte types, we can give a Pandas/Spark DataFrame or a remote URI as the dataset.

The datasources can be well-integrated with the plugin using the following two modes:
- **Flyte Task**: A Flyte task defines the task prototype that one could use within a task or a workflow to validate data using Great Expectations.
- **Flyte Type**: A Flyte type helps attach the `GreatExpectationsType` to any dataset. Under the hood, `GreatExpectationsType` can be assumed as a combination of Great Expectations and Flyte types where every bit of data is validated against the [Expectations](https://docs.greatexpectations.io/docs/reference/expectations/expectations), much like the OpenAPI Spec or the gRPC validator.

**Note**: Expectations are unit tests that specify the validation you would like to enforce on your data, like *expect_table_row_count_to_be_between* or *expect_column_values_to_be_of_type*. 

# Plugin Parameters

- **datasource_name**: Datasource, in general, is the “name” we use in the Great Expectations config file for accessing data. ​​A Datasource brings together a way of interacting with data (like a database or Spark cluster) and some specific data (like a CSV file or a database table). Datasources enable us to build batches out of data (for validation).
- **expectation_suite_name**: Defines the data validation.
- **data_connector_name**: Tells how the data batches are to be identified.

## Optional Parameters
- **context_root_dir**: Sets the path of the Great Expectations config directory.
- **checkpoint_params**: Optional [SimpleCheckpoint](https://legacy.docs.greatexpectations.io/en/latest/autoapi/great_expectations/checkpoint/checkpoint/index.html#great_expectations.checkpoint.checkpoint.SimpleCheckpoint) class parameters.
- **batchrequest_config**: Additional batch request configuration parameters.
    - data_connector_query: Query to request a data batch
    - runtime_parameters: Parameters to be sent at run-time
    - batch_identifiers: Batch identifiers
    - batch_spec_passthrough: Reader method if your file doesn’t have an extension
- **data_asset_name**: Name of the data asset (to be used for `RuntimeBatchRequest`)
- **local_file_path**: User-given path where the dataset has to be downloaded

**Note**: You may always want to mention the `context_root_dir` parameter, as providing a path means no harm! Moreover, `local_file_path` is essential when using `FlyteFile` and `FlyteSchema`.

# Examples

Firstly, install the plugin using the following command:

```bash
pip install flytekitplugins-great_expectations
```

(or)

```bash
pip install flytekitplugins-great-expectations
```

**Note**: You should have a config file with the required Great Expectations configuration. For example, refer to [this](https://github.com/great-expectations/great_expectations/blob/develop/tests/profile/fixtures/great_expectations_titanic_0.13.yml) config file.

## Task example
Great Expectations can be written as a Flyte task. To do so, 

- Initialize Great Expectations configuration
- Validate data using the configuration

Here’s an example using `FlyteFile`:

```python
import pandas as pd
from flytekit import Resources, kwtypes, task, workflow
from flytekit.types.file import CSVFile
from flytekitplugins.great_expectations import GreatExpectationsTask

file_task_object = GreatExpectationsTask(
   name="great_expectations_task_flytefile",
   datasource_name="data",
   inputs=kwtypes(dataset=CSVFile),
   expectation_suite_name="test.demo",
   data_connector_name="data_flytetype_data_connector",
   local_file_path="/tmp",
   context_root_dir="greatexpectations/great_expectations",
)


@task(limits=Resources(mem="500Mi"))
def file_task(
   dataset: CSVFile,
) -> int:
   file_task_object(dataset=dataset)
   return len(pd.read_csv(dataset))


@workflow
def file_wf(
   dataset: CSVFile = "https://raw.githubusercontent.com/superconductive/ge_tutorials/main/data/yellow_tripdata_sample_2019-01.csv",
) -> int:
   return file_task(dataset=dataset)
```
Additional Batch Request parameters can be given using [BatchRequestConfig](https://github.com/flyteorg/flytekit/blob/b8a3c7d34e6b19722a6a968dc05506ad1eb26912/plugins/flytekit-greatexpectations/flytekitplugins/great_expectations/task.py#L22-L39).

## Data Validation Failure
If the data validation fails, the plugin will raise a Great Expectations ValidationError.

For example, this is how the error message looks:

```bash
Traceback (most recent call last):
...
great_expectations.marshmallow__shade.exceptions.ValidationError: Validation failed!
COLUMN          FAILED EXPECTATION
passenger_count -> expect_column_min_to_be_between
passenger_count -> expect_column_mean_to_be_between
passenger_count -> expect_column_quantile_values_to_be_between
passenger_count -> expect_column_values_to_be_in_set
passenger_count -> expect_column_proportion_of_unique_values_to_be_between
trip_distance -> expect_column_max_to_be_between
trip_distance -> expect_column_mean_to_be_between
trip_distance -> expect_column_median_to_be_between
trip_distance -> expect_column_quantile_values_to_be_between
trip_distance -> expect_column_proportion_of_unique_values_to_be_between
rate_code_id -> expect_column_max_to_be_between
rate_code_id -> expect_column_mean_to_be_between
rate_code_id -> expect_column_proportion_of_unique_values_to_be_between
```

**Note**: In the future, we plan to integrate Great Expectations data docs with Flyte UI. This can enhance the visualization of errors and capture the key characteristics of the dataset. 

## Type Example
Great Expectations validation can be encapsulated in Flyte’s type-system. Here’s an example using `FlyteSchema`:

```python
from flytekit import Resources, task, workflow
from flytekit.types.schema import FlyteSchema
from flytekitplugins.great_expectations import (
   BatchRequestConfig,
   GreatExpectationsFlyteConfig,
   GreatExpectationsType,
)


@task(limits=Resources(mem="500Mi"))
def schema_task(
   dataframe: GreatExpectationsType[
       FlyteSchema,
       GreatExpectationsFlyteConfig(
           datasource_name="data",
           expectation_suite_name="test.demo",
           data_connector_name="data_flytetype_data_connector",
           batch_request_config=BatchRequestConfig(data_connector_query={"limit": 10}),
           local_file_path="/tmp/test.parquet",  # noqa: F722
           context_root_dir="greatexpectations/great_expectations",
       ),
   ]
) -> int:
   return dataframe.shape[0]


@workflow
def schema_wf() -> int:
   return schema_task(dataframe=...)
```

Refer to the fully worked [task](https://docs.flyte.org/projects/cookbook/en/latest/auto/integrations/flytekit_plugins/greatexpectations/task_example.html) and [type](https://docs.flyte.org/projects/cookbook/en/latest/auto/integrations/flytekit_plugins/greatexpectations/type_example.html) examples to understand how well Great Expectations integrates with Flyte. 

**Note**: Great Expectations’ `RunTimeBatchRequest` can be used just like a simple `BatchRequest` in Flyte. Make sure to set up the data connector correctly. The plugin then automatically checks for the type of batch request and instantiates it. Check [this example](https://docs.flyte.org/projects/cookbook/en/latest/auto/integrations/flytekit_plugins/greatexpectations/type_example.html#runtimebatchrequest).

Let us know if you find this plugin useful! Join the Flyte [community](https://docs.google.com/forms/d/e/1FAIpQLScWPJZncyL-bBCWbuQ9HWuMzA8-r2RJc__CSWJoAYUQADN-BQ/viewform) or the Great Expectations [community](https://greatexpectations.io/slack) if you have any suggestions or feedback—we’d love to hear from you!




