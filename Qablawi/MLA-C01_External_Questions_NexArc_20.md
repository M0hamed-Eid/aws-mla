# Nex Arc - AWS MLA-C01 Sample Practice Questions (20 Questions)

Source: https://nex-arc-learning.com/aws/mla-c01
Free CSV: https://nex-arc-learning.com/courses/samples/aws/mla.csv

---

## Question 1

**Domain:** Data Preparation for Machine Learning

**Type:** multiple-choice

An ML engineer is configuring Amazon SageMaker Feature Store for a large-scale ML platform. The offline store will be used by multiple data science teams to build training datasets using Amazon Athena. The engineer wants to optimize query performance for the offline store data as the volume of feature data grows. The feature group contains time-series data that is frequently queried by date ranges. Which offline store configuration should the ML engineer use?

- Create the feature group with the Apache Iceberg table format and use Athena's OPTIMIZE command for compaction to improve query performance over time.
- Create the feature group with the AWS Glue (default) table format and configure custom hourly partitions based on a secondary date column.
- Create the feature group with only the online store enabled and use the BatchGetRecord API for all data science team queries to ensure optimal performance.
- Create the feature group with the AWS Glue table format and manually convert the Parquet files to Delta Lake format using AWS Glue jobs for better query performance.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. When creating feature groups with Iceberg table format, SageMaker Feature Store creates Iceberg tables using Parquet file format and registers them with the AWS Glue Data Catalog. Iceberg supports table maintenance operations like compaction, which optimizes the structural layout of the table for faster queries. As data accumulates, running the OPTIMIZE table REWRITE DATA compaction command in Athena reduces the number of files, improving query performance. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/feature-store-storage-configurations-offline-store.html
- Option 2: Incorrect. While the AWS Glue format is the default and does partition data by event time into hourly partitions, you cannot configure the partitioning scheme. The partitioning is automatically determined by Feature Store based on event time. For optimizing large-scale query performance over time, the Iceberg format with compaction provides better optimization capabilities. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/feature-store-offline.html
- Option 3: Incorrect. The online store only contains the latest record for each identifier and does not store historical data. Data science teams building training datasets require access to historical feature data, which is only available in the offline store. The BatchGetRecord API is designed for real-time inference, not for building training datasets. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/feature-store-concepts.html
- Option 4: Incorrect. SageMaker Feature Store only supports AWS Glue and Apache Iceberg table formats for the offline store. Delta Lake is not a supported table format option. Converting files manually would also break the integration with Feature Store's automatic data management and synchronization capabilities. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/feature-store-storage-configurations-offline-store.html

## Question 2

**Domain:** Data Preparation for Machine Learning

**Type:** multiple-choice

A data engineering team is building an ML data pipeline that processes CSV files uploaded daily to Amazon S3. The team uses AWS Glue ETL jobs to transform the data and write it to a target S3 location in Parquet format. The team wants the ETL job to process only newly uploaded files since the last successful job run to minimize processing time and costs. The team also needs to ensure that if data needs to be reprocessed due to downstream issues, they can restart processing from a specific point in time. Which AWS Glue feature should the team implement to meet these requirements?

- Enable job bookmarks for the AWS Glue ETL job and use the transformation_ctx parameter for the source DynamicFrame to track processed data.
- Configure the AWS Glue crawler to run incremental crawls with the CRAWL_NEW_FOLDERS_ONLY recrawl policy to track new partitions.
- Use Amazon S3 event notifications with AWS Lambda to maintain a DynamoDB table that tracks processed files, then query this table at the start of each ETL job run.
- Enable the enableUpdateCatalog option with updateBehavior set to UPDATE_IN_DATABASE in the ETL script to track processed partitions in the Data Catalog.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. AWS Glue job bookmarks track data that has already been processed during previous runs by persisting state information. When enabled, the job processes only new data on subsequent runs. The transformation_ctx parameter is used to identify state information within a job bookmark for the given operator, serving as the key to search the bookmark state. If reprocessing is needed, the bookmark can be rewound to any previous job run. Reference: https://docs.aws.amazon.com/glue/latest/dg/monitor-continuations.html
- Option 2: Incorrect. The crawler's incremental crawl feature with CRAWL_NEW_FOLDERS_ONLY is designed to identify and add new partitions to the Data Catalog, not to track which data files have been processed by an ETL job. This setting affects the crawler behavior, not the ETL job's data processing. Reference: https://docs.aws.amazon.com/glue/latest/dg/incremental-crawls.html
- Option 3: Incorrect. While this custom solution could work, it requires building and maintaining additional infrastructure and code. AWS Glue provides native job bookmark functionality specifically designed for this use case, which is the recommended approach and requires less operational overhead. Reference: https://docs.aws.amazon.com/glue/latest/dg/monitor-continuations.html
- Option 4: Incorrect. The enableUpdateCatalog option with updateBehavior is used to update the schema and add new partitions to the Data Catalog from within the ETL job, not to track which data has been processed. This feature allows schema evolution without re-running the crawler but does not provide incremental processing capabilities. Reference: https://docs.aws.amazon.com/glue/latest/dg/update-from-job.html

## Question 3

**Domain:** Data Preparation for Machine Learning

**Type:** multiple-choice

A machine learning engineer is using Amazon SageMaker Data Wrangler to transform a customer dataset containing a 'country' column with over 200 unique categorical values. The engineer needs to encode this high-cardinality categorical variable efficiently for a recommendation model while minimizing feature dimensionality. Which encoding approach in Data Wrangler will BEST meet these requirements?

- Use the Encode Categorical transform with Similarity encoding to group similar categories and create a lower-dimensional representation based on string similarity.
- Use the Encode Categorical transform with One-hot encoding and select the Vector output style to store all encoded values in a single sparse vector column.
- Use the Encode Categorical transform with Ordinal encoding to assign an integer value between 0 and the total number of categories for each country value.
- Use the Encode Categorical transform with One-hot encoding and select the Columns output style to create individual binary columns for the top 50 most frequent countries.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. Similarity encoding is specifically designed for high-cardinality categorical variables in SageMaker Data Wrangler. Unlike one-hot encoding which creates a separate column for each category (resulting in 200+ columns in this case), similarity encoding groups similar categories based on string similarity and creates a more compact representation. This efficiently handles high-cardinality columns while minimizing feature dimensionality. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-transform.html
- Option 2: Incorrect. While the Vector output style stores the one-hot encoded values in a single sparse vector column rather than creating separate columns, this does not reduce the actual dimensionality of the encoding. Each of the 200+ categories still requires a dimension in the vector. This approach is more efficient for storage but does not minimize feature dimensionality as required. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-transform.html
- Option 3: Incorrect. Ordinal encoding assigns sequential integers to categories, which implies an ordered relationship between categories. For country names, there is no inherent ordering, and using ordinal encoding would incorrectly suggest that some countries are 'greater than' or 'less than' others. This can negatively impact model performance for categorical data without natural ordering. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-transform.html
- Option 4: Incorrect. While one-hot encoding with Columns output style can work for categorical variables, creating columns for only the top 50 countries would lose information about the remaining 150+ countries. Additionally, one-hot encoding with individual columns significantly increases dimensionality rather than minimizing it. Similarity encoding is the appropriate choice for high-cardinality categorical data. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-transform.html

## Question 4

**Domain:** Data Preparation for Machine Learning

**Type:** multiple-choice

A data scientist is using Amazon SageMaker Data Wrangler to prepare features for a binary classification model. The dataset includes a categorical column called 'region' with 45 unique US state values. The data scientist wants to convert this categorical column into a numerical representation. The machine learning algorithm requires individual binary columns for each category. Which Data Wrangler transformation approach should the data scientist use?

- Use the Encode Categorical transform with One-hot encode selected, and set Output style to Columns.
- Use the Encode Categorical transform with Ordinal encode selected to map each state to a unique integer value.
- Use the Encode Categorical transform with One-hot encode selected, and set Output style to Vector.
- Use a custom PySpark transform to manually create 45 new columns using conditional logic for each state value.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. SageMaker Data Wrangler provides the Encode Categorical transform which includes One-hot encode as an option. When you select One-hot encode and set the Output style to Columns, Data Wrangler creates individual binary columns for each category value. This approach converts each unique value in the categorical column into a separate binary column, which meets the requirement of having individual binary columns for each of the 45 state values. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-transform.html
- Option 2: Incorrect. While Ordinal encode is available in Data Wrangler's Encode Categorical transform, it converts categories into integers between 0 and the total number of categories. This creates a single column with integer values rather than individual binary columns for each category. Ordinal encoding is suitable when there is an inherent order to the categories, not when individual binary columns are required. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-transform.html
- Option 3: Incorrect. While One-hot encode is correct for creating binary representations, setting Output style to Vector produces a single column containing a sparse vector rather than individual binary columns for each category. The requirement specifically states that the ML algorithm requires individual binary columns, so the Columns output style should be used instead. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-transform.html
- Option 4: Incorrect. While custom PySpark transforms are supported in Data Wrangler, manually creating 45 columns with conditional logic is unnecessary and error-prone. Data Wrangler provides a built-in Encode Categorical transform with One-hot encode that automatically handles this conversion with proper configuration. Using built-in transforms is more efficient and maintainable than custom code for standard operations. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-transform.html

## Question 5

**Domain:** Data Preparation for Machine Learning

**Type:** multiple-choice

A data science team is using Amazon SageMaker Data Wrangler to prepare a time series dataset for demand forecasting. They need to create a custom feature that calculates the rolling 7-day average sales for each product. The over 300 built-in transforms in Data Wrangler do not include this specific rolling window calculation. Which approach should the team use to implement this custom transformation in Data Wrangler?

- Add a Custom Transform step and write a PySpark SQL query using window functions to calculate the rolling 7-day average partitioned by product.
- Use the Transform Time Series built-in transform and configure it with a 7-day rolling window aggregation for the sales column.
- Export the data flow to a Jupyter notebook, add the rolling average calculation using Python code outside of Data Wrangler, then import the results back into a new flow.
- Use the Featurize Datetime transform to extract time-based features, then apply the Process Numeric transform with aggregation to calculate the average.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. Data Wrangler supports custom transformations using PySpark SQL, PySpark, or Pandas. For rolling window calculations on partitioned data, PySpark SQL with window functions is efficient and scalable. You can write a query like 'SELECT *, AVG(sales) OVER (PARTITION BY product ORDER BY date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) as rolling_avg FROM df'. Custom transforms become part of the data flow and can be included in processing jobs. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-transform.html
- Option 2: Incorrect. While Data Wrangler has Transform Time Series capabilities, this refers to specific time series transformations like featurization and does not provide configurable rolling window aggregation with partitioning by product. For custom rolling calculations with specific window sizes and partitions, a custom transform using PySpark SQL window functions is required. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-transform.html
- Option 3: Incorrect. This approach is inefficient and breaks the Data Wrangler workflow. Exporting, modifying externally, and reimporting creates unnecessary complexity and loses the benefits of maintaining transformations within a single data flow. Data Wrangler natively supports custom PySpark SQL, PySpark, and Pandas transformations that can implement rolling calculations directly. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-transform.html
- Option 4: Incorrect. Featurize Datetime extracts components from datetime columns (year, month, day, etc.) but does not perform rolling window calculations. Process Numeric provides operations like scaling and clipping but not rolling aggregations. Neither of these built-in transforms can calculate a rolling 7-day average partitioned by product. A custom transform is required. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/data-wrangler-transform.html

## Question 6

**Domain:** Data Preparation for Machine Learning

**Type:** multiple-choice

A data engineering team is building a machine learning data pipeline using AWS Glue ETL. The team needs to implement data quality validation to ensure that the customer_id column contains unique, non-null values before loading the processed data into Amazon S3. The pipeline must stop execution and prevent data from being written to the target if the data quality rules fail. Which solution will meet these requirements?

- In the AWS Glue ETL job, add an Evaluate Data Quality transform node with the rule IsPrimaryKey 'customer_id'. Configure the action to Fail job without loading to target data when data quality fails.
- In the AWS Glue ETL job, add an Evaluate Data Quality transform node with the rules IsComplete 'customer_id' and IsUnique 'customer_id'. Keep the default action setting of None when data quality fails.
- In the AWS Glue ETL job, add an Evaluate Data Quality transform node with the rule ColumnExists 'customer_id'. Configure the action to Fail job after loading data to target when data quality fails.
- In the AWS Glue ETL job, add an Evaluate Data Quality transform node with the rule Completeness 'customer_id' > 0.99. Configure the action to publish results to Amazon CloudWatch.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. The IsPrimaryKey rule type in AWS Glue Data Quality checks that a column contains unique, non-null values, which is exactly what is needed for validating the customer_id column. Setting the action to 'Fail job without loading to target data' will immediately stop the job when a data quality error occurs and prevent any data from being loaded to the target. This ensures data quality issues are caught before bad data reaches the data lake. Reference: https://docs.aws.amazon.com/glue/latest/dg/tutorial-data-quality.html
- Option 2: Incorrect. While combining IsComplete and IsUnique rules would check for non-null and unique values respectively, keeping the default action setting of None means the job will continue to run and load data to the target even when data quality rules fail. This does not meet the requirement to stop execution and prevent data from being written to the target. Reference: https://docs.aws.amazon.com/glue/latest/dg/tutorial-data-quality.html
- Option 3: Incorrect. The ColumnExists rule type only verifies that a column is present in the dataset schema. It does not validate that the column contains unique, non-null values. Additionally, the action 'Fail job after loading data to target' would still load data to the target before failing, which does not meet the requirement to prevent data from being written. Reference: https://docs.aws.amazon.com/glue/latest/dg/dqdl.html
- Option 4: Incorrect. The Completeness rule type only checks the percentage of complete (non-null) values in a column and does not verify uniqueness. Additionally, publishing results to CloudWatch is a monitoring action and does not stop the job or prevent data from being written to the target when rules fail. Reference: https://docs.aws.amazon.com/glue/latest/dg/data-quality-rule-builder.html

## Question 7

**Domain:** ML Model Development

**Type:** multiple-choice

A company operates a global e-commerce platform and wants to forecast demand for over 10,000 products across multiple regions. The company has historical sales data for the past 3 years, with related time series sharing seasonal patterns. An ML engineer needs to select a SageMaker built-in algorithm that can train a single model across all product time series and produce probabilistic forecasts. Which algorithm should the ML engineer choose?

- Use the Amazon SageMaker DeepAR algorithm to train a model jointly over all related time series.
- Use the Amazon SageMaker XGBoost algorithm with time-based features to predict future demand values.
- Use the Amazon SageMaker Linear Learner algorithm with lag features to generate point forecasts for each product.
- Use the Amazon SageMaker K-Means algorithm to group similar products and then forecast demand for each cluster.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. DeepAR is a supervised learning algorithm for forecasting scalar time series using recurrent neural networks (RNN). Unlike classical methods like ARIMA that fit a single model to each individual time series, DeepAR trains a single model jointly over all time series. When the dataset contains hundreds of related time series, DeepAR outperforms standard ARIMA and ETS methods. DeepAR produces probabilistic forecasts and can learn patterns from similar time series, which is ideal for product demand forecasting where products share seasonal patterns across regions. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/deepar.html
- Option 2: Incorrect. XGBoost is a gradient-boosted trees algorithm designed for tabular classification and regression problems. While XGBoost can be adapted for time series forecasting by creating time-based features, it does not inherently produce probabilistic forecasts or learn patterns jointly across multiple related time series like DeepAR does. XGBoost treats each observation independently and would require significant feature engineering to handle temporal dependencies. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html
- Option 3: Incorrect. Linear Learner is designed for classification and regression problems on tabular data, not specifically for time series forecasting. While lag features could be created manually, Linear Learner cannot train a single model jointly over multiple related time series, does not produce probabilistic forecasts, and lacks the recurrent neural network architecture needed to capture complex temporal dependencies and seasonality patterns inherent in time series data. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/linear-learner.html
- Option 4: Incorrect. K-Means is an unsupervised clustering algorithm that finds discrete groupings within data. It is not designed for time series forecasting and cannot produce predictions of future values. While clustering products might be useful for segmentation, it does not address the forecasting requirement. The correct approach is to use DeepAR, which is specifically designed for time series forecasting across multiple related series. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/k-means.html

## Question 8

**Domain:** ML Model Development

**Type:** multiple-choice

A machine learning team is using Amazon SageMaker Autopilot to build classification models for different business use cases. The team has three datasets: a binary classification problem for fraud detection, a multiclass classification problem with 10 product categories, and a regression problem for price prediction. The team wants to use the default objective metrics for each problem type to minimize configuration overhead. Which combination of default objective metrics does Autopilot use for these problem types?

- F1 for binary classification, Accuracy for multiclass classification, and MSE for regression.
- AUC for binary classification, F1macro for multiclass classification, and RMSE for regression.
- Accuracy for binary classification, BalancedAccuracy for multiclass classification, and MAE for regression.
- Precision for binary classification, Accuracy for multiclass classification, and R2 for regression.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. SageMaker Autopilot uses F1 as the default objective metric for binary classification problems, Accuracy as the default for multiclass classification problems, and MSE (Mean Squared Error) as the default for regression problems. When you do not specify a metric explicitly, Autopilot automatically applies these defaults based on the problem type it detects or the problem type you specify. Reference: https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobObjective.html
- Option 2: Incorrect. While AUC and F1macro are valid objective metrics that Autopilot supports, they are not the defaults. AUC is useful for imbalanced binary classification but must be explicitly specified. F1macro can be used for multiclass classification, but Accuracy is the default. RMSE is a valid regression metric but MSE is the default. Reference: https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobObjective.html
- Option 3: Incorrect. Accuracy is a valid metric for both binary and multiclass classification but is only the default for multiclass, not binary. BalancedAccuracy is supported and useful for imbalanced datasets but is not the default for multiclass classification. MAE is a valid regression metric but MSE is the default. Reference: https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobObjective.html
- Option 4: Incorrect. While Precision is a valid objective metric in Autopilot, F1 is the default for binary classification because F1 balances precision and recall. R2 is supported for regression but MSE is the default objective metric. Reference: https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobObjective.html

## Question 9

**Domain:** ML Model Development

**Type:** multiple-choice

A machine learning team is training a deep learning image classification model using Amazon SageMaker. The team configured a hyperparameter tuning job with Bayesian optimization and wants to reduce costs by enabling early stopping to terminate underperforming training jobs. The algorithm emits validation accuracy metrics after each training epoch. What must the team configure to properly enable early stopping for this tuning job?

- Set TrainingJobEarlyStoppingType to AUTO in the HyperParameterTuningJobConfig. Ensure the algorithm emits objective metrics per epoch to CloudWatch Logs.
- Set TrainingJobEarlyStoppingType to AUTO and configure the Hyperband strategy to enable the built-in early stopping mechanism.
- Configure the StoppingCondition with a MaxRuntimeInSeconds value, and SageMaker will automatically stop poorly performing jobs before reaching this limit.
- Enable BestObjectiveNotImproving completion criteria with MaxNumberOfTrainingJobsNotImproving set to a threshold value.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. To enable early stopping, you must set the TrainingJobEarlyStoppingType field to AUTO in the HyperParameterTuningJobConfig. When early stopping is enabled, SageMaker evaluates each training job by comparing the objective metric value to the median of running averages from previous training jobs. The algorithm must emit objective metrics for each epoch to support early stopping. This can reduce costs by up to 28% by terminating jobs that are unlikely to improve model accuracy. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning-early-stopping.html
- Option 2: Incorrect. Hyperband has its own advanced internal early stopping mechanism that is separate from the TrainingJobEarlyStoppingType setting. In fact, TrainingJobEarlyStoppingType must be set to OFF when using Hyperband strategy. Bayesian optimization and Hyperband use different early stopping mechanisms and cannot be combined in the same tuning job. Reference: https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HyperParameterTuningJobConfig.html
- Option 3: Incorrect. StoppingCondition with MaxRuntimeInSeconds specifies the maximum runtime that each training job can run before being stopped. This is a hard timeout limit and does not provide intelligent early stopping based on objective metric performance. Early stopping requires setting TrainingJobEarlyStoppingType to AUTO, which evaluates training job performance against previous jobs. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning-ex-tuning-job.html
- Option 4: Incorrect. BestObjectiveNotImproving is a tuning job completion criteria that stops the entire tuning job when the best objective metric is not improving across training jobs. This is different from early stopping, which terminates individual training jobs that are unlikely to outperform previous jobs. Early stopping requires TrainingJobEarlyStoppingType set to AUTO. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning-early-stopping.html

## Question 10

**Domain:** ML Model Development

**Type:** multiple-choice

A company is running a PyTorch deep learning training job on Amazon SageMaker using managed spot training with ml.g5.12xlarge instances. The training script saves model checkpoints every 500 steps. During training, the spot instance was interrupted and the job was automatically restarted by SageMaker. The ML engineer notices that after the restart, training resumed from step 0 instead of the last checkpoint. What is the MOST likely cause of this issue?

- The checkpoint_s3_uri parameter was not specified in the SageMaker estimator configuration, so checkpoints were not synchronized to Amazon S3.
- The max_wait parameter was set to a value less than the training time, causing the job to terminate before checkpoints could be saved.
- The training script was saving checkpoints to a local directory that was not /opt/ml/checkpoints, and checkpoint_local_path was not configured to match.
- The ml.g5.12xlarge instance type does not support checkpointing with managed spot training.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. When using managed spot training, you must configure checkpointing by specifying the checkpoint_s3_uri parameter in the SageMaker estimator. SageMaker copies checkpoint data from the local path '/opt/ml/checkpoints' to the specified S3 location. When a spot instance is interrupted and the job restarts, SageMaker copies the checkpoint data from S3 back to the local path so training can resume. Without checkpoint_s3_uri configured, checkpoints saved locally are lost when the instance is terminated. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/model-checkpoints.html
- Option 2: Incorrect. If max_wait was exceeded, the training job would have stopped completely with a MaxWaitTimeExceeded status, not restarted. The scenario indicates the job was automatically restarted by SageMaker after interruption, which means max_wait was sufficient. The issue is specifically about checkpoints not being available after restart. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/model-managed-spot-training.html
- Option 3: Incorrect. While this could cause checkpoint issues, the most likely cause in a standard SageMaker setup is missing checkpoint_s3_uri configuration. By default, SageMaker uses '/opt/ml/checkpoints' as the local checkpoint path. If the training script saves to this default location but checkpoint_s3_uri is not specified, the checkpoints will not be synchronized to S3 and will be lost on interruption. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/model-checkpoints-enable.html
- Option 4: Incorrect. Managed Spot Training is supported for all instance types available in Amazon SageMaker, including ml.g5 instances. There are no instance-type restrictions for checkpointing functionality. The issue is related to configuration, not instance type limitations. All GPU instance types including ml.g5, ml.p3, and ml.p4d support checkpointing with managed spot training. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/model-managed-spot-training.html

## Question 11

**Domain:** ML Model Development

**Type:** multiple-choice

An ML engineer has deployed a regression model to a SageMaker endpoint and configured data quality monitoring using DefaultModelMonitor. The baseline job has completed successfully and generated the constraints.json file. The engineer notices that certain feature constraints are too restrictive, causing excessive false positive violations during production monitoring. The engineer wants to adjust the constraints to reduce noise while still detecting meaningful drift. What should the engineer do to customize the monitoring constraints?

- Download the constraints.json file from S3, modify the constraint thresholds based on domain knowledge, upload the modified file to S3, and reference the updated file when creating the monitoring schedule.
- Re-run the baseline job with different hyperparameters to generate less restrictive constraints. Use the instance_count and instance_type parameters in DefaultModelMonitor to adjust constraint sensitivity.
- Add more data to the training dataset to widen the statistical distribution and regenerate the baseline. This will automatically produce broader constraints that are less likely to trigger false positive violations.
- Set the emit_metrics parameter to False in the constraints.json file to disable violation reporting for noisy features. This will prevent false positive alerts while still monitoring other features.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. The constraints.json file generated by the baseline job contains suggested constraints that can be customized. You can download the file, review the constraints, and modify thresholds to make them more aggressive or relaxed based on your understanding of the domain and business problem. When creating the monitoring schedule, you reference the updated constraints file. This allows you to control the number and nature of violations. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-faqs.html
- Option 2: Incorrect. The instance_count and instance_type parameters control the compute resources used for baseline and monitoring jobs, not the sensitivity of generated constraints. The baseline job uses statistical analysis of the training data to generate constraints. To customize constraints, you must manually edit the generated constraints.json file and adjust the thresholds. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-create-baseline.html
- Option 3: Incorrect. While adding more data might change the baseline statistics, this approach does not guarantee that constraints will be less restrictive. Additionally, modifying the training data could affect model training and is not the appropriate solution for adjusting monitoring thresholds. The recommended approach is to directly modify the constraints.json file based on domain expertise. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-faqs.html
- Option 4: Incorrect. The emit_metrics parameter controls whether CloudWatch metrics are emitted for specific features, but it does not disable violation detection. The monitoring job will still detect and report violations in the constraint_violations.json file. To reduce false positives, you need to modify the actual constraint thresholds such as numerical distance thresholds or completeness requirements. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-interpreting-cloudwatch.html

## Question 12

**Domain:** ML Model Development

**Type:** multiple-choice

An ML engineer is running a distributed training job on Amazon SageMaker using 4 GPUs across an ml.p3.8xlarge instance. The engineer notices that training is slower than expected and wants to determine if there is an imbalance in workload distribution across the GPUs. Which SageMaker Debugger built-in rule should the engineer configure to detect this issue?

- LoadBalancing rule to detect issues in workload distribution among multiple GPUs in the training cluster.
- LowGPUUtilization rule to check if any of the GPUs have lower utilization rates compared to others in the cluster.
- StepOutlier rule to identify variations in step duration that indicate uneven work across different GPUs.
- OverallSystemUsage rule to measure system usage per worker node and identify discrepancies between GPU utilization rates.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. The SageMaker Debugger LoadBalancing rule specifically helps detect issues in workload balancing among multiple GPUs. When configured with a training job, this rule analyzes if the work is evenly distributed across all GPUs and identifies if certain GPUs are overutilized while others are underutilized, which can cause overall training performance degradation. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-profiler-rules.html
- Option 2: Incorrect. The LowGPUUtilization rule detects if GPU utilization is low or suffers from fluctuations on each GPU. While it can indicate underutilization, it does not specifically analyze workload distribution balance across multiple GPUs. The rule evaluates each GPU independently rather than comparing workload distribution between them. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-profiler-rules.html
- Option 3: Incorrect. The StepOutlier rule helps detect outliers in step durations and returns True if there are outliers with step durations larger than a standard deviation threshold. While step duration variations might occur due to workload imbalance, this rule is designed to detect outlier steps rather than specifically analyze GPU workload distribution. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-profiler-rules.html
- Option 4: Incorrect. The OverallSystemUsage rule measures overall system usage per worker node and computes percentiles of values aggregated per node. It provides a general overview of system resource utilization but does not specifically analyze workload balancing issues across individual GPUs within a node. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-profiler-rules.html

## Question 13

**Domain:** Deployment and Orchestration of ML Workflows

**Type:** multiple-choice

An ML engineer is configuring a SageMaker Batch Transform job to process CSV data for a custom inference container. The engineer wants to maximize throughput by sending multiple records in each request to the container while keeping each request payload under 6 MB. The input files contain millions of records with each record being approximately 500 bytes. Which configuration should the ML engineer use to achieve optimal throughput?

- Set SplitType to Line, BatchStrategy to MultiRecord, and MaxPayloadInMB to 6.
- Set SplitType to None, BatchStrategy to SingleRecord, and MaxPayloadInMB to 6.
- Set SplitType to Line, BatchStrategy to SingleRecord, and MaxPayloadInMB to 1.
- Set SplitType to RecordIO, BatchStrategy to MultiRecord, and MaxPayloadInMB to 6.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. Setting SplitType to Line instructs SageMaker to split the input files by newline character boundaries into individual records. Setting BatchStrategy to MultiRecord allows SageMaker to fit as many records as possible into each request up to the MaxPayloadInMB limit. With MaxPayloadInMB set to 6 and records of approximately 500 bytes each, SageMaker can batch around 12,000 records per request, maximizing throughput. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/batch-transform.html
- Option 2: Incorrect. Setting SplitType to None means the entire input file is sent in a single request without splitting by record boundaries. BatchStrategy of SingleRecord would send one record at a time if splitting were enabled. This configuration does not split records and does not batch multiple records together, resulting in inefficient processing for many small records. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/batch-transform.html
- Option 3: Incorrect. While SplitType of Line correctly splits the input by newline characters, BatchStrategy of SingleRecord sends individual records in each request rather than batching them together. This configuration sends one 500-byte record per request instead of batching multiple records, significantly reducing throughput compared to using MultiRecord. Reference: https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TransformInput.html
- Option 4: Incorrect. SplitType of RecordIO is used for RecordIO binary data format, not CSV data. Using RecordIO with CSV files would result in errors or improper data parsing. For CSV data with line-delimited records, SplitType should be set to Line to correctly split records on newline character boundaries. Reference: https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TransformInput.html

## Question 14

**Domain:** Deployment and Orchestration of ML Workflows

**Type:** multiple-choice

A machine learning team is using AWS Step Functions to orchestrate an Amazon SageMaker pipeline for model training and deployment. The team wants to implement error handling that automatically retries the SageMaker training step when transient errors occur. The retry mechanism should wait 5 seconds before the first retry, exponentially increase wait times for subsequent retries, and stop after 3 retry attempts. Which Step Functions configuration should the ML engineer add to the Task state definition to meet these requirements?

- Add a Retry field with ErrorEquals set to 'States.ALL', IntervalSeconds set to 5, MaxAttempts set to 3, and BackoffRate set to 2.0.
- Add a Catch field with ErrorEquals set to 'States.ALL', IntervalSeconds set to 5, MaxAttempts set to 3, and BackoffRate set to 2.0.
- Add a Retry field with ErrorEquals set to 'States.ALL', TimeoutSeconds set to 5, MaxRetries set to 3, and ExponentialBackoff set to true.
- Add a Wait state before the Task state with Seconds set to 5, and add a Catch field with RetryCount set to 3 and BackoffMultiplier set to 2.0.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. The Retry field in Step Functions provides automatic retry logic for Task states. The ErrorEquals field specifies which errors trigger a retry. IntervalSeconds defines the initial wait time before the first retry (5 seconds). MaxAttempts specifies the maximum number of retry attempts (3). BackoffRate is the multiplier applied to the interval after each retry, creating exponential backoff (with a rate of 2.0, wait times would be 5, 10, and 20 seconds). This configuration meets all the specified requirements. Reference: https://docs.aws.amazon.com/step-functions/latest/dg/concepts-error-handling.html
- Option 2: Incorrect. The Catch field is used for catching errors and transitioning to a fallback state, not for implementing automatic retry logic. Catch fields do not support IntervalSeconds, MaxAttempts, or BackoffRate parameters. These parameters are only valid in Retry fields. To implement automatic retries with exponential backoff, you must use the Retry field. Reference: https://docs.aws.amazon.com/step-functions/latest/dg/concepts-error-handling.html
- Option 3: Incorrect. This configuration uses invalid parameter names. The correct parameters for a Retry field are IntervalSeconds (not TimeoutSeconds), MaxAttempts (not MaxRetries), and BackoffRate (not ExponentialBackoff). TimeoutSeconds is a separate Task state parameter for setting execution timeout, not retry intervals. Reference: https://docs.aws.amazon.com/step-functions/latest/dg/concepts-error-handling.html
- Option 4: Incorrect. A Wait state is used to pause workflow execution, not to implement retry logic. Additionally, RetryCount and BackoffMultiplier are not valid parameters in Step Functions. The Catch field is used for error handling transitions to fallback states, not for implementing retries. Proper retry logic must be implemented using the Retry field with IntervalSeconds, MaxAttempts, and BackoffRate parameters. Reference: https://docs.aws.amazon.com/step-functions/latest/dg/concepts-error-handling.html

## Question 15

**Domain:** Deployment and Orchestration of ML Workflows

**Type:** multiple-choice

A company is implementing an MLOps workflow where code commits to a CodeCommit repository should trigger a SageMaker Pipeline that includes model training. The ML engineer needs to pass the commit ID from the CodeCommit event to the SageMaker Pipeline as a parameter for version tracking. How should the ML engineer configure the EventBridge rule to pass the commit ID dynamically to the pipeline?

- Configure the EventBridge rule target with SageMakerPipelineParameters and use a JSON path expression like $.detail.commitId to extract the commit ID from the event payload and pass it as a pipeline parameter value.
- Configure the EventBridge rule with an input transformer that maps the commit ID to a static parameter, then reference this parameter in the SageMaker Pipeline definition using environment variables.
- Create a Lambda function as the EventBridge rule target that extracts the commit ID from the event and stores it in Parameter Store, then configure the SageMaker Pipeline to read from Parameter Store at execution time.
- Configure the SageMaker Pipeline to poll the CodeCommit API at the start of execution to retrieve the latest commit ID using a Processing step with custom code.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. EventBridge supports dynamic parameter values for SageMaker Pipeline targets using JSON path expressions. You can specify parameter Name/Value pairs in the SageMakerPipelineParameters, where the Value can be a JSON path that EventBridge parses from the event payload. For example, '$.detail.commitId' extracts the commit ID from the CodeCommit event detail and passes it to the StartPipelineExecutionRequest. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/pipeline-eventbridge.html
- Option 2: Incorrect. While input transformers can modify event data before passing to targets, the recommended approach for SageMaker Pipelines is to use SageMakerPipelineParameters with JSON path expressions for dynamic values. Input transformers are typically used with other target types like Lambda or Step Functions. SageMaker Pipelines have native support for dynamic parameter passing through JSON paths. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/pipeline-eventbridge.html
- Option 3: Incorrect. While this approach would work, it adds unnecessary complexity and latency. EventBridge natively supports passing dynamic values to SageMaker Pipelines using JSON path expressions in SageMakerPipelineParameters. This direct integration is simpler and more efficient than using Lambda as an intermediary. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/pipeline-eventbridge.html
- Option 4: Incorrect. Having the pipeline poll CodeCommit adds unnecessary complexity and may not retrieve the correct commit that triggered the execution if multiple commits occur in quick succession. EventBridge provides native support for passing event data to SageMaker Pipelines through SageMakerPipelineParameters with JSON path expressions, which ensures the correct commit ID is passed. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/pipeline-eventbridge.html

## Question 16

**Domain:** ML Solution Monitoring, Maintenance, and Security

**Type:** multiple-choice

A company uses Amazon SageMaker to deploy a recommendation model with auto-scaling configured. The ML operations team wants to create a CloudWatch dashboard to visualize endpoint performance during peak traffic periods. They need to track both the total number of requests hitting the endpoint and the number of requests handled per instance to understand scaling behavior. Which combination of CloudWatch metrics should the team use for this dashboard?

- Use the Invocations metric to track total requests and InvocationsPerInstance to track requests per instance
- Use the Invocations4XXErrors metric to track total requests and Invocations5XXErrors to track requests per instance
- Use the ModelLatency metric to track total requests and OverheadLatency to track requests per instance
- Use the CPUUtilization metric to track total requests and MemoryUtilization to track requests per instance

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. The Invocations metric in the AWS/SageMaker namespace tracks the total number of InvokeEndpoint requests sent to the endpoint. The InvocationsPerInstance metric shows the number of invocations per instance, which changes as the endpoint scales out with auto-scaling. When only one instance is serving requests, both metrics show the same values, but InvocationsPerInstance changes as new instances are added during scaling. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html
- Option 2: Incorrect. Invocations4XXErrors tracks client-side errors (4XX HTTP status codes) and Invocations5XXErrors tracks server-side errors (5XX HTTP status codes). Neither of these metrics tracks successful requests or requests per instance. These metrics are used for monitoring error rates, not request volumes. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html
- Option 3: Incorrect. ModelLatency measures the time that inference takes within the model container, and OverheadLatency measures SageMaker infrastructure overhead time. Neither of these metrics tracks the count of requests or requests per instance - they measure latency in microseconds. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html
- Option 4: Incorrect. CPUUtilization and MemoryUtilization are instance-level operational metrics that track resource utilization percentages on endpoint instances. These metrics do not track the number of requests or invocations hitting the endpoint. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html

## Question 17

**Domain:** ML Solution Monitoring, Maintenance, and Security

**Type:** multiple-choice

A financial services company has deployed a fraud detection ML model on an Amazon SageMaker real-time endpoint. The operations team needs to monitor the endpoint and receive notifications when the model's inference time exceeds 100 milliseconds for at least 2 out of 3 consecutive evaluation periods of 5 minutes each. The team wants to avoid false alarms from temporary latency spikes. Which CloudWatch alarm configuration meets these requirements?

- Create a CloudWatch alarm on the ModelLatency metric in the AWS/SageMaker namespace. Set the threshold to 100000 microseconds using the Average statistic. Configure EvaluationPeriods to 3, DatapointsToAlarm to 2, and Period to 300 seconds.
- Create a CloudWatch alarm on the OverheadLatency metric in the AWS/SageMaker namespace. Set the threshold to 100000 microseconds using the Average statistic. Configure EvaluationPeriods to 3, DatapointsToAlarm to 2, and Period to 300 seconds.
- Create a CloudWatch alarm on the ModelLatency metric in the AWS/SageMaker namespace. Set the threshold to 100 milliseconds using the Average statistic. Configure EvaluationPeriods to 3, DatapointsToAlarm to 3, and Period to 300 seconds.
- Create a CloudWatch alarm on the Invocations metric in the AWS/SageMaker namespace. Set the threshold to 100000 microseconds using the p99 statistic. Configure EvaluationPeriods to 2, DatapointsToAlarm to 2, and Period to 300 seconds.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. The ModelLatency metric captures the time that inference takes within the model container and is measured in microseconds. Setting an 'M out of N' alarm configuration where DatapointsToAlarm (M) is 2 and EvaluationPeriods (N) is 3 meets the requirement to trigger only when 2 out of 3 periods breach the threshold. This configuration helps avoid false alarms from temporary spikes. The threshold of 100000 microseconds equals 100 milliseconds. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html
- Option 2: Incorrect. The OverheadLatency metric measures the time SageMaker takes to respond to an invocation request excluding the model latency. This metric captures the transport time to and from the model container, not the actual inference time. To monitor when model inference time exceeds a threshold, you should use the ModelLatency metric instead. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html
- Option 3: Incorrect. While the ModelLatency metric is correct for monitoring inference time, setting DatapointsToAlarm equal to EvaluationPeriods (both 3) means the alarm will trigger only when all 3 consecutive periods breach the threshold, not 2 out of 3 as required. Additionally, ModelLatency is measured in microseconds, not milliseconds, so the threshold should be 100000, not 100. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html
- Option 4: Incorrect. The Invocations metric tracks the number of InvokeEndpoint requests sent to a model endpoint, not the latency of those requests. To monitor inference time, you should use the ModelLatency metric. The Invocations metric is typically used to monitor request volume and does not support latency-based thresholds. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html

## Question 18

**Domain:** ML Solution Monitoring, Maintenance, and Security

**Type:** multiple-choice

An ML engineer needs to quickly identify candidate instance types for deploying a PyTorch image classification model to SageMaker. The engineer has registered the model in the SageMaker Model Registry with the required metadata including framework, framework version, and sample payload. The engineer wants to get preliminary instance recommendations optimized for cost, throughput, and latency within approximately 45 minutes. Which solution will meet these requirements?

- Run a Default Inference Recommender job using the CreateInferenceRecommendationsJob API with JobType set to Default and provide the model package ARN.
- Run an Advanced Inference Recommender job using the CreateInferenceRecommendationsJob API with JobType set to Advanced and provide the model package ARN.
- Run a Default Inference Recommender job directly on the training job artifacts without registering the model in the Model Registry.
- Use SageMaker Compute Optimizer to analyze the model and automatically select the optimal instance type for deployment.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. A Default Inference Recommender job runs automated load tests on recommended instance types and provides preliminary recommendations optimized for cost, throughput, and latency. Default jobs complete quickly (typically around 45 minutes) and return the top instance types that are suitable for your model. Using a registered model package ARN from the Model Registry satisfies the prerequisites for running Inference Recommender. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender-instance-recommendation.html
- Option 2: Incorrect. Advanced Inference Recommender jobs are designed for custom load testing with specific traffic patterns and production requirements. Advanced jobs take an average of 2 hours to complete depending on the job duration and number of configurations tested. For preliminary recommendations within 45 minutes, a Default job type is more appropriate. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender-load-test.html
- Option 3: Incorrect. To use Amazon SageMaker Inference Recommender, you must either create a SageMaker model or register a model to the SageMaker Model Registry with your model artifacts. Training job artifacts cannot be directly used without proper model registration or creation. The prerequisites require a versioned model package or SageMaker model with proper configuration. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender-prerequisites.html
- Option 4: Incorrect. SageMaker does not have a service called SageMaker Compute Optimizer. AWS Compute Optimizer provides recommendations for EC2 instances but does not specifically support SageMaker inference endpoint recommendations with ML-specific load testing. SageMaker Inference Recommender is the purpose-built capability for instance type selection for ML model deployment. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender.html

## Question 19

**Domain:** ML Solution Monitoring, Maintenance, and Security

**Type:** multiple-choice

An ML engineer runs a Default SageMaker Inference Recommender job for an NLP model and receives recommendations for five different instance types. The engineer notices that ml.c5.xlarge has the lowest CostPerHour but ml.inf1.xlarge has the lowest CostPerInference and higher MaxInvocations. The model needs to serve variable traffic that averages 10,000 requests per minute but can spike to 40,000 during peak hours. Which instance type should the engineer select for cost optimization, and why?

- Select ml.inf1.xlarge because the lower CostPerInference indicates better price-performance, and the higher MaxInvocations suggests it can handle the peak traffic spikes more efficiently.
- Select ml.c5.xlarge because the lowest CostPerHour ensures minimum operational costs, and additional instances can be added via autoscaling to handle peak traffic.
- Select ml.c5.xlarge for average traffic periods and configure automatic instance type switching to ml.inf1.xlarge during detected peak traffic periods for cost savings.
- Run an Advanced Inference Recommender job to test both instance types under the specific traffic pattern before making a decision, as Default job results are not reliable for production decisions.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. For workloads with variable traffic patterns, CostPerInference is the key metric for cost optimization because it accounts for both the instance cost and throughput capacity. The ml.inf1.xlarge having lower CostPerInference means each inference request costs less, which is critical for high-volume workloads. The higher MaxInvocations indicates the instance can handle more requests, which is essential for handling traffic spikes up to 40,000 requests per minute. AWS Inferentia instances (ml.inf1) are specifically optimized for ML inference workloads. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender-interpret-results.html
- Option 2: Incorrect. While ml.c5.xlarge has lower hourly costs, focusing only on CostPerHour can be misleading for variable workloads. If the instance has lower throughput, you may need more instances to handle peak traffic, potentially increasing total costs. CostPerInference is the better metric for optimizing cost across varying traffic levels because it factors in throughput efficiency. The ml.inf1.xlarge with lower CostPerInference would likely be more cost-effective overall. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender-interpret-results.html
- Option 3: Incorrect. SageMaker endpoints do not support automatic instance type switching based on traffic patterns. You would need to maintain separate endpoints or update the endpoint configuration manually. For variable traffic with significant spikes, selecting the instance with the lowest CostPerInference and sufficient MaxInvocations provides the most efficient cost optimization without complex switching mechanisms. Autoscaling can then adjust instance count within the same instance type. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/inference-cost-optimization.html
- Option 4: Incorrect. Default Inference Recommender job results are valid for making production decisions and provide reliable metrics including CostPerInference and MaxInvocations. While Advanced jobs allow custom traffic patterns, the Default job results already indicate that ml.inf1.xlarge has better price-performance characteristics. Advanced jobs are recommended for more detailed analysis but are not required to make an informed decision from Default job results. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender-recommendation-jobs.html

## Question 20

**Domain:** ML Solution Monitoring, Maintenance, and Security

**Type:** multiple-choice

An ML engineer is configuring Amazon SageMaker training jobs that read data from and write model artifacts to Amazon S3. The company's security policy requires all data to be encrypted at rest using customer managed AWS KMS keys. The engineer needs to configure the SageMaker training job to use a specific customer managed key for encrypting output model artifacts in S3. Which SageMaker training job configuration parameter should the ML engineer specify?

- Specify the KmsKeyId parameter in the OutputDataConfig to encrypt model artifacts stored in S3 with the customer managed key.
- Specify the VolumeKmsKeyId parameter in the ResourceConfig to encrypt model artifacts stored in S3 with the customer managed key.
- Configure the S3KMSKeyId in the DataSource configuration to encrypt both input data and output artifacts with the customer managed key.
- Specify the KmsKeyId parameter in the NetworkConfig to enable end-to-end encryption for all S3 data transfers and storage.

**Correct Answer(s):** 1

**Explanation:**


- Option 1: Correct. When configuring a SageMaker training job, the OutputDataConfig contains a KmsKeyId parameter that specifies the AWS KMS key that SageMaker uses to encrypt the model artifacts at rest using Amazon S3 server-side encryption. If you do not provide a KMS key, SageMaker uses the default AWS managed key for Amazon S3 for your account. Reference: https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_OutputDataConfig.html
- Option 2: Incorrect. The VolumeKmsKeyId parameter in ResourceConfig is used to encrypt the ML storage volumes attached to the training instance (encryption of ephemeral storage), not the output model artifacts stored in S3. When the job completes, output is uploaded to S3 using the key specified in OutputDataConfig, not VolumeKmsKeyId. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/encryption-at-rest-nbi.html
- Option 3: Incorrect. The DataSource configuration in SageMaker is used to specify input data locations, not output encryption settings. To encrypt output model artifacts, you must use the KmsKeyId parameter in OutputDataConfig. Input and output encryption are configured separately in SageMaker training jobs. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/key-management.html
- Option 4: Incorrect. NetworkConfig in SageMaker is used to configure VPC settings such as subnets and security groups, not encryption settings. The KmsKeyId for encrypting S3 output must be specified in OutputDataConfig. Network encryption in transit is separate from encryption at rest configuration. Reference: https://docs.aws.amazon.com/sagemaker/latest/dg/key-management.html
