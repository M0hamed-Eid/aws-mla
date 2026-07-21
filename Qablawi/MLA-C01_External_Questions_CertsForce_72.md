# CertsForce - AWS MLA-C01 Practice Test Questions

Source: https://www.certsforce.com/amazon-web-services/questions/mla-c01-practice-test

---

## Question 1

A company has a large collection of chat recordings from customer interactions after a product release. An ML engineer needs to create an ML model to analyze the chat data. The ML engineer needs to determine the success of the product by reviewing customer sentiments about the product.
Which action should the ML engineer take to complete the evaluation in the LEAST amount of time?

- A. Use Amazon Rekognition to analyze sentiments of the chat conversations.
- B. Train a Naive Bayes classifier to analyze sentiments of the chat conversations.
- C. Use Amazon Comprehend to analyze sentiments of the chat conversations.
- D. Use random forests to classify sentiments of the chat conversations.

**Answer:** C

**Explanation:**
Amazon Comprehend is a fully managed natural language processing (NLP) service that includes a built-in sentiment analysis feature. It can quickly and efficiently analyze text data to determine whether the sentiment is positive, negative, neutral, or mixed. Using Amazon Comprehend requires minimal setup and provides accurate results without the need to train and deploy custom models, making it the fastest and most efficient solution for this task.

## Question 2

A company has a Retrieval Augmented Generation (RAG) application that uses a vector database to store embeddings of documents. The company must migrate the application to AWS and must implement a solution that provides semantic search of text files. The company has already migrated the text repository to an Amazon S3 bucket.
Which solution will meet these requirements?

- A. Use an AWS Batch job to process the files and generate embeddings. Use AWS Glue to store the embeddings. Use SQL queries to perform the semantic searches.
- B. Use a custom Amazon SageMaker AI notebook to run a custom script to generate embeddings. Use SageMaker Feature Store to store the embeddings. Use SQL queries to perform the semantic searches.
- C. Use the Amazon Kendra S3 connector to ingest the documents from the S3 bucket into Amazon Kendra. Query Amazon Kendra to perform the semantic searches.
- D. Use an Amazon Textract asynchronous job to ingest the documents from the S3 bucket. Query Amazon Textract to perform the semantic searches.

**Answer:** C

**Explanation:**
The key requirement is semantic search over text documents that already reside in Amazon S3. AWS provides Amazon Kendra, a fully managed service specifically designed for semantic and natural language search across unstructured text.
Amazon Kendra natively supports S3 connectors, which can ingest documents directly from an S3 bucket, automatically process the text, generate embeddings, and index the content for semantic retrieval. This removes the need for the company to manage embedding generation, vector storage, or similarity search infrastructure. Queries can be expressed in natural language, making Kendra well suited for RAG-style applications.
Option A and B require building and maintaining a custom embedding pipeline and do not provide a true vector similarity search engine using SQL. SageMaker Feature Store is not intended to function as a vector database for semantic search.
Option D is incorrect because Amazon Textract is an OCR service for extracting text from scanned documents and images; it does not support semantic search.
Therefore, ingesting documents using the Amazon Kendra S3 connector and querying Kendra is the correct and AWS-recommended solution.

## Question 3

A company wants to migrate ML models from an on-premises environment to Amazon SageMaker AI. The models are based on the PyTorch algorithm. The company needs to reuse its existing custom scripts as much as possible.
Which SageMaker AI feature should the company use?

- A. SageMaker AI built-in algorithms
- B. SageMaker Canvas
- C. SageMaker JumpStart
- D. SageMaker AI script mode

**Answer:** D

**Explanation:**
SageMaker script mode allows ML engineers to bring existing training scripts written for frameworks such as PyTorch and TensorFlow directly into SageMaker with minimal changes. AWS documentation explicitly states that script mode is designed to support migration of existing ML workloads.
With script mode, the user provides a custom training script, and SageMaker handles infrastructure provisioning, distributed training, logging, and model artifact storage. This makes script mode ideal for companies that want to reuse established codebases without rewriting them.
Built-in algorithms require adopting AWS-provided implementations. SageMaker Canvas is a no-code tool, and JumpStart provides pretrained models and templates but does not focus on custom script reuse.
Therefore, Option D is the correct and AWS-recommended choice.

## Question 4

An ML engineer is tuning an image classification model that shows poor performance on one of two available classes during prediction. Analysis reveals that the images whose class the model performed poorly on represent an extremely small fraction of the whole training dataset.
The ML engineer must improve the model ' s performance.
Which solution will meet this requirement?

- A. Optimize for accuracy. Use image augmentation on the less common images to generate new samples.
- B. Optimize for F1 score. Use image augmentation on the less common images to generate new samples.
- C. Optimize for accuracy. Use Synthetic Minority Oversampling Technique (SMOTE) on the less common images to generate new samples.
- D. Optimize for F1 score. Use Synthetic Minority Oversampling Technique (SMOTE) on the less common images to generate new samples.

**Answer:** B

**Explanation:**
This problem describes severe class imbalance in an image classification task, where the minority class has poor predictive performance. In such cases, accuracy is a misleading metric, because a model can achieve high accuracy by predicting only the majority class. AWS ML best practices recommend using F1 score, which balances precision and recall and is more appropriate for imbalanced classification problems.
To improve performance on the minority image class, image augmentation is the preferred approach. Augmentation techniques—such as rotation, cropping, flipping, and brightness adjustment—create realistic new training examples while preserving semantic meaning. AWS documentation recommends augmentation for computer vision workloads to improve generalization without collecting new data.
SMOTE (Options C and D) is designed for tabular data, not image data, and generating synthetic pixel-level images using SMOTE is not appropriate or supported in typical computer vision pipelines.
Option A is incorrect because optimizing for accuracy does not address minority-class performance. Option D is incorrect because SMOTE is unsuitable for images.
Therefore, optimizing for F1 score and using image augmentation on the minority class is the correct solution.

## Question 5

A company has significantly increased the amount of data stored as .csv files in an Amazon S3 bucket. Data transformation scripts and queries are now taking much longer than before.
An ML engineer must implement a solution to optimize the data for query performance with the LEAST operational overhead.
Which solution will meet this requirement?

- A. Configure an AWS Lambda function to split the .csv files into smaller objects.
- B. Configure an AWS Glue job to drop string-type columns and save the results to S3.
- C. Configure an AWS Glue ETL job to convert the .csv files to Apache Parquet format.
- D. Configure an Amazon EMR cluster to process the data in S3.

**Answer:** C

**Explanation:**
AWS strongly recommends converting large CSV datasets into columnar formats such as Apache Parquet to improve query performance. Parquet reduces I/O by reading only the required columns and applies compression, which significantly speeds up analytics workloads.
AWS Glue ETL jobs provide a fully managed, serverless way to perform this conversion with minimal operational overhead. Once converted, the Parquet files can be queried efficiently by services such as Amazon Athena, Redshift Spectrum, and SageMaker processing jobs.
Splitting CSV files does not address inefficient storage format. Dropping columns risks data loss. Amazon EMR introduces infrastructure management overhead and is unnecessary for a straightforward format conversion.
AWS documentation clearly identifies CSV-to-Parquet conversion using Glue ETL as a best practice for scalable analytics.
Therefore, Option C is the correct answer.

## Question 6

An ML engineer needs to choose the most appropriate data format for various data uses. Different teams will access the data for analytics, ML, and reporting purposes.
Select the correct data format from the following list to meet the requirements for each use case. Select each data format one time. (Select FOUR.)
Suggested Solution
Expert Solution
Answer
Answer:
Show Explanation
Explanation
The best answers are Parquet, JSON, CSV, and ORC in that order.
Parquet is the strongest choice for complex analytical queries over large structured datasets because it is a columnar format. Columnar storage allows query engines such as Amazon Athena, AWS Glue, and Spark to read only the columns required by the query instead of scanning full rows. AWS documentation states that Apache Parquet and ORC are columnar storage formats optimized for fast retrieval in analytical applications, and that column-level compression can reduce storage space and I/O during query processing. This directly matches the need to filter, aggregate, reduce query response time, and lower storage/query cost.
JSON is correct for semi-structured real-time logs because JSON supports flexible and nested data structures. AWS Glue documentation describes JSON as a format for data structures with consistent shape but flexible contents and notes that it is not row-based or column-based. That makes it appropriate for application logs, event records, and evolving schemas used later for analytics or ML ingestion.
CSV is correct for small spreadsheet exports and occasional human-readable analysis. AWS Glue describes CSV as a minimal, row-based data format. CSV is widely supported by spreadsheet tools and is easy for humans to inspect, but it is not ideal for large-scale analytical performance because it lacks efficient column pruning and rich schema support.
ORC is correct for the Apache Hive read-heavy big data pipeline. ORC is a performance-oriented, column-based format, and it is strongly associated with Hive-based analytics workloads. It provides high compression and efficient reads, making it well suited for structured data in read-heavy big data pipelines.


**Answer:** 

**Explanation:**
The best answers are Parquet, JSON, CSV, and ORC in that order.
Parquet is the strongest choice for complex analytical queries over large structured datasets because it is a columnar format. Columnar storage allows query engines such as Amazon Athena, AWS Glue, and Spark to read only the columns required by the query instead of scanning full rows. AWS documentation states that Apache Parquet and ORC are columnar storage formats optimized for fast retrieval in analytical applications, and that column-level compression can reduce storage space and I/O during query processing. This directly matches the need to filter, aggregate, reduce query response time, and lower storage/query cost.
JSON is correct for semi-structured real-time logs because JSON supports flexible and nested data structures. AWS Glue documentation describes JSON as a format for data structures with consistent shape but flexible contents and notes that it is not row-based or column-based. That makes it appropriate for application logs, event records, and evolving schemas used later for analytics or ML ingestion.
CSV is correct for small spreadsheet exports and occasional human-readable analysis. AWS Glue describes CSV as a minimal, row-based data format. CSV is widely supported by spreadsheet tools and is easy for humans to inspect, but it is not ideal for large-scale analytical performance because it lacks efficient column pruning and rich schema support.
ORC is correct for the Apache Hive read-heavy big data pipeline. ORC is a performance-oriented, column-based format, and it is strongly associated with Hive-based analytics workloads. It provides high compression and efficient reads, making it well suited for structured data in read-heavy big data pipelines.

## Question 7

A company uses a hybrid cloud environment. A model that is deployed on premises uses data in Amazon 53 to provide customers with a live conversational engine.
The model is using sensitive data. An ML engineer needs to implement a solution to identify and remove the sensitive data.
Which solution will meet these requirements with the LEAST operational overhead?

- A. Deploy the model on Amazon SageMaker. Create a set of AWS Lambda functions to identify and remove the sensitive data.
- B. Deploy the model on an Amazon Elastic Container Service (Amazon ECS) cluster that uses AWS Fargate. Create an AWS Batch job to identify and remove the sensitive data.
- C. Use Amazon Macie to identify the sensitive data. Create a set of AWS Lambda functions to remove the sensitive data.
- D. Use Amazon Comprehend to identify the sensitive data. Launch Amazon EC2 instances to remove the sensitive data.

**Answer:** C

**Explanation:**
Amazon Macie is a fully managed data security and privacy service that uses machine learning to discover and classify sensitive data in Amazon S3. It is purpose-built to identify sensitive data with minimal operational overhead. After identifying the sensitive data, you can use AWS Lambda functions to automate the process of removing or redacting the sensitive data, ensuring efficiency and integration with the hybrid cloud environment. This solution requires the least development effort and aligns with the requirement to handle sensitive data effectively.

## Question 8

An ML engineer is designing an AI-powered traffic management system. The system must use near real-time inference to predict congestion and prevent collisions.
The system must also use batch processing to perform historical analysis of predictions over several hours to improve the model. The inference endpoints must scale automatically to meet demand.
Which combination of solutions will meet these requirements? (Select TWO.)

- A. Use Amazon SageMaker real-time inference endpoints with automatic scaling based on ConcurrentInvocationsPerInstance.
- B. Use AWS Lambda with reserved concurrency and SnapStart to connect to SageMaker endpoints.
- C. Use an Amazon SageMaker Processing job for batch historical analysis. Schedule the job with Amazon EventBridge.
- D. Use Amazon EC2 Auto Scaling to host containers for batch analysis.
- E. Use AWS Lambda for historical analysis.

**Answer:** 

**Explanation:**
For near real-time predictions, AWS documentation recommends Amazon SageMaker real-time inference endpoints. These endpoints support automatic scaling based on metrics such as ConcurrentInvocationsPerInstance, ensuring low latency and high availability during traffic spikes.
For long-running historical analysis, SageMaker Processing jobs are the appropriate solution. Processing jobs are designed for batch workloads, can run for hours, and integrate cleanly with SageMaker pipelines. Scheduling them with Amazon EventBridge provides a fully managed, scalable, and serverless solution.
AWS Lambda is unsuitable for multi-hour workloads. EC2 Auto Scaling adds unnecessary infrastructure management overhead.
Therefore, Options A and C together meet all requirements and align with AWS best practices.

## Question 9

An ML engineer is setting up a continuous integration and continuous delivery (CI/CD) pipeline for an ML workflow in Amazon SageMaker AI. The pipeline needs to automate model re-training, testing, and deployment whenever new data is uploaded to an Amazon S3 bucket. New data files are approximately 10 GB in size. The ML engineer wants to track model versions for auditing.
Which solution will meet these requirements?

- A. Use AWS CodePipeline, Amazon S3, and AWS CodeBuild to retrain and deploy the model automatically and to track model versions.
- B. Use SageMaker Pipelines with the SageMaker Model Registry to orchestrate model training and version tracking.
- C. Create an AWS Lambda function to re-train and deploy the model. Use Amazon EventBridge to invoke the Lambda function. Reference the Lambda logs to track model versions.
- D. Use SageMaker AI notebook instances to manually re-train and deploy the model when needed. Reference AWS CloudTrail logs to track model versions.

**Answer:** B

**Explanation:**
AWS provides Amazon SageMaker Pipelines as the native CI/CD solution for ML. Pipelines can automatically trigger retraining when new data arrives in Amazon S3, handle large datasets (such as 10 GB files), and orchestrate preprocessing, training, evaluation, and deployment steps.
For governance and auditing, Amazon SageMaker Model Registry integrates directly with Pipelines to manage model versions, approval status, and metadata such as training metrics. This combination eliminates the need for custom tracking logic and ensures traceability across the ML lifecycle.
Option A lacks native ML lineage and version governance. Option C is unsuitable for large data and complex workflows. Option D is manual and not scalable.
Therefore, using SageMaker Pipelines with the Model Registry is the correct and AWS-recommended solution.

## Question 10

A company uses a batching solution to process data analytics each day. The company wants to build an analytics platform to provide near real-time updates. The company wants to use open source technology and does not want to manage or scale the infrastructure.
Which solution will meet these requirements?

- A. Create Amazon Managed Streaming for Apache Kafka (Amazon MSK) Serverless clusters to process the data.
- B. Create Amazon Managed Streaming for Apache Kafka (Amazon MSK) Provisioned clusters. Configure the clusters based on data volume.
- C. Create data streams in Amazon Kinesis Data Streams. Use AWS Application Auto Scaling to scale the infrastructure.
- D. Create self-hosted Apache Flink applications on Amazon EC2. Run the applications as containers.

**Answer:** A

**Explanation:**
The correct answer is A. Create Amazon Managed Streaming for Apache Kafka (Amazon MSK) Serverless clusters to process the data.
Amazon MSK Serverless allows organizations to run Apache Kafka workloads without managing or provisioning the underlying infrastructure. Unlike MSK Provisioned clusters (Option B), serverless clusters automatically scale to accommodate variable workloads and handle operational tasks such as patching, scaling, and monitoring. This meets the company’s requirement to avoid managing or scaling infrastructure while providing near real-time streaming analytics.
Option C, using Amazon Kinesis Data Streams with Application Auto Scaling, provides a managed streaming solution but is not based on open-source technology. While Kinesis can handle near real-time data, the question specifically emphasizes the company’s desire for open-source technology, which points toward Kafka.
Option D, self-hosting Apache Flink on EC2, requires manual management of servers, scaling, patching, and container orchestration. This approach contradicts the requirement of not managing or scaling infrastructure and introduces operational complexity.
MSK Serverless provides seamless integration with open-source Kafka APIs, enabling applications to produce and consume streaming data in real time while AWS handles scaling and availability. It is ideal for analytics pipelines where data ingestion is continuous and variable, and infrastructure overhead must be minimized.
By choosing MSK Serverless, the company can implement near real-time updates for its analytics platform using open-source Kafka without the operational burden of cluster management, fully aligning with AWS best practices for deployment and orchestration of ML workflows in a managed, scalable, and serverless manner.

## Question 11

An ML engineer is developing a neural network to run on new user data. The dataset has dozens of floating-point features. The dataset is stored as CSV objects in an Amazon S3 bucket. Most objects and columns are missing at least one value. All features are relatively uniform except for a small number of extreme outliers. The ML engineer wants to use Amazon SageMaker Data Wrangler to handle missing values before passing the dataset to the neural network.
Which solution will provide the MOST complete data?

- A. Drop samples that are missing values.
- B. Impute missing values with the mean value.
- C. Impute missing values with the median value.
- D. Drop columns that are missing values.

**Answer:** C

**Explanation:**
The primary goal is to produce the most complete dataset while handling missing values and extreme outliers appropriately. Dropping samples (Option A) or columns (Option D) would reduce data completeness and potentially remove valuable information, which contradicts the requirement.
Imputation is therefore the correct approach. Between mean and median imputation, AWS ML best practices recommend using the median when features contain outliers. The mean is sensitive to extreme values and can be skewed significantly, leading to imputed values that are not representative of the typical data distribution. In contrast, the median is robust to outliers, making it a better statistical estimator for central tendency in such datasets.
Amazon SageMaker Data Wrangler supports median imputation as a built-in transformation, enabling ML engineers to handle missing values consistently across large tabular datasets without custom code. This approach preserves all rows and columns while minimizing distortion caused by extreme values, which is particularly important for neural networks that are sensitive to input distributions.
Therefore, imputing missing values with the median value provides the most complete and statistically appropriate dataset for training.

## Question 12

An ML company wants to monitor and analyze the API calls that its AWS resources make. The company has created an AWS CloudTrail log file that logs to an Amazon S3 bucket. The company has also created an organization in AWS Organizations to manage permissions across accounts.
The company needs to enable log file validation to ensure the integrity of its log files.
Which solution will meet these requirements?

- A. Enable CloudTrail log file integrity validation.
- B. Create a multi-Region trail in CloudTrail.
- C. Create a trail in CloudTrail for the organization.
- D. Enable Amazon CloudWatch Logs delivery.

**Answer:** A

**Explanation:**
The correct answer is A. Enable CloudTrail log file integrity validation.
AWS CloudTrail provides the ability to record API calls made to AWS services and delivers log files to an Amazon S3 bucket. For organizations that need to ensure the authenticity and integrity of these log files, AWS recommends enabling log file integrity validation. This feature applies a hash function to each log file and stores the hash separately, allowing you to verify that the logs have not been altered, deleted, or tampered with after delivery.
Enabling log file integrity validation is critical in ML operations when auditing model training pipelines, production inference calls, or system access patterns across accounts. It ensures that security-sensitive API activity is accurately recorded and verifiable. In multi-account environments managed by AWS Organizations, this validation provides an extra layer of trust when logs are consolidated from multiple accounts.
Option B, creating a multi-Region trail, ensures that API activity across regions is logged but does not inherently guarantee the integrity of logs. Option C, creating an organization trail, centralizes logging for all accounts, which is valuable for governance, but again does not automatically provide verification of log integrity. Option D, enabling CloudWatch Logs delivery, allows real-time monitoring and alerting but does not address the verification of historical log files.
By enabling log file integrity validation, organizations can cryptographically verify each log file, detect unauthorized changes, and meet compliance requirements for secure ML monitoring and auditing. This aligns with AWS best practices for ML solution monitoring, maintenance, and security, ensuring reliable tracking of model operations and API usage across distributed ML systems.

## Question 13

A company is developing an internal cost-estimation tool that uses an ML model in Amazon SageMaker AI. Users upload high-resolution images to the tool.
The model must process each image and predict the cost of the object in the image. The model also must notify the user when processing is complete.
Which solution will meet these requirements?

- A. Store the images in an Amazon S3 bucket. Deploy the model on SageMaker AI. Use batch transform jobs for model inference. Use an Amazon Simple Queue Service (Amazon SQS) queue to notify users.
- B. Store the images in an Amazon S3 bucket. Deploy the model on SageMaker AI. Use an asynchronous inference strategy for model inference. Use an Amazon Simple Notification Service (Amazon SNS) topic to notify users.
- C. Store the images in an Amazon Elastic File System (Amazon EFS) file system. Deploy the model on SageMaker AI. Use batch transform jobs for model inference. Use an Amazon Simple Queue Service (Amazon SQS) queue to notify users.
- D. Store the images in an Amazon Elastic File System (Amazon EFS) file system. Deploy the model on SageMaker AI. Use an asynchronous inference strategy for model inference. Use an Amazon Simple Notification Service (Amazon SNS) topic to notify users.

**Answer:** B

**Explanation:**
This use case involves large payloads (high-resolution images), variable processing time, and a requirement to notify users upon completion. AWS documentation recommends Amazon SageMaker asynchronous inference for exactly this scenario.
Asynchronous inference allows clients to submit inference requests without waiting for a response. The request payload is stored in Amazon S3, and the inference result is written back to S3 when processing is complete. This approach is designed for workloads that cannot meet real-time latency requirements and where payload sizes exceed real-time limits.
SageMaker asynchronous inference integrates natively with Amazon SNS to send notifications when inference completes or fails, which directly satisfies the user notification requirement.
Batch transform jobs are intended for offline, scheduled processing of large datasets and do not provide built-in user notification mechanisms. Amazon EFS is not required because SageMaker natively integrates with S3 for inference input and output.
AWS explicitly documents asynchronous inference as the preferred solution for large image inference with completion notifications.
Therefore, Option B is the correct and AWS-verified answer.

## Question 14

A company has trained an ML model in Amazon SageMaker. The company needs to host the model to provide inferences in a production environment.
The model must be highly available and must respond with minimum latency. The size of each request will be between 1 KB and 3 MB. The model will receive unpredictable bursts of requests during the day. The inferences must adapt proportionally to the changes in demand.
How should the company deploy the model into production to meet these requirements?

- A. Create a SageMaker real-time inference endpoint. Configure auto scaling. Configure the endpoint to present the existing model.
- B. Deploy the model on an Amazon Elastic Container Service (Amazon ECS) cluster. Use ECS scheduled scaling that is based on the CPU of the ECS cluster.
- C. Install SageMaker Operator on an Amazon Elastic Kubernetes Service (Amazon EKS) cluster. Deploy the model in Amazon EKS. Set horizontal pod auto scaling to scale replicas based on the memory metric.
- D. Use Spot Instances with a Spot Fleet behind an Application Load Balancer (ALB) for inferences. Use the ALBRequestCountPerTarget metric as the metric for auto scaling.

**Answer:** A

**Explanation:**
Amazon SageMaker real-time inference endpoints are designed to provide low-latency predictions in production environments. They offer built-in auto scaling to handle unpredictable bursts of requests, ensuring high availability and responsiveness. This approach is fully managed, reduces operational complexity, and is optimized for the range of request sizes (1 KB to 3 MB) specified in the requirements.

## Question 15

A company is using Amazon SageMaker and millions of files to train an ML model. Each file is several megabytes in size. The files are stored in an Amazon S3 bucket. The company needs to improve training performance.
Which solution will meet these requirements in the LEAST amount of time?

- A. Transfer the data to a new S3 bucket that provides S3 Express One Zone storage. Adjust the training job to use the new S3 bucket.
- B. Create an Amazon FSx for Lustre file system. Link the file system to the existing S3 bucket. Adjust the training job to read from the file system.
- C. Create an Amazon Elastic File System (Amazon EFS) file system. Transfer the existing data to the file system. Adjust the training job to read from the file system.
- D. Create an Amazon ElastiCache (Redis OSS) cluster. Link the Redis OSS cluster to the existing S3 bucket. Stream the data from the Redis OSS cluster directly to the training job.

**Answer:** B

**Explanation:**
Amazon FSx for Lustre is designed for high-performance workloads like ML training. It provides fast, low-latency access to data by linking directly to the existing S3 bucket and caching frequently accessed files locally. This significantly improves training performance compared to directly accessing millions of files from S3. It requires minimal changes to the training job and avoids the overhead of transferring or restructuring data, making it the fastest and most efficient solution.

## Question 16

Case Study
A company is building a web-based AI application by using Amazon SageMaker. The application will provide the following capabilities and features: ML experimentation, training, a
central model registry, model deployment, and model monitoring.
The application must ensure secure and isolated use of training data during the ML lifecycle. The training data is stored in Amazon S3.
The company needs to use the central model registry to manage different versions of models in the application.
Which action will meet this requirement with the LEAST operational overhead?

- A. Create a separate Amazon Elastic Container Registry (Amazon ECR) repository for each model.
- B. Use Amazon Elastic Container Registry (Amazon ECR) and unique tags for each model version.
- C. Use the SageMaker Model Registry and model groups to catalog the models.
- D. Use the SageMaker Model Registry and unique tags for each model version.

**Answer:** C

**Explanation:**
Amazon SageMaker Model Registry is a feature designed to manage machine learning (ML) models throughout their lifecycle. It allows users to catalog, version, and deploy models systematically, ensuring efficient model governance and management.
Key Features of SageMaker Model Registry:
Centralized Cataloging: Organizes models into Model Groups, each containing multiple versions.
Version Control: Maintains a history of model iterations, making it easier to track changes.
Metadata Association: Attach metadata such as training metrics and performance evaluations to models.
Approval Status Management: Allows setting statuses like PendingManualApproval or Approved to ensure only vetted models are deployed.
Seamless Deployment: Direct integration with SageMaker deployment capabilities for real-time inference or batch processing.
Implementation Steps:
Create a Model Group: Organize related models into groups to simplify management and versioning.
Register Model Versions: Each model iteration is registered as a version within a specific Model Group.
Set Approval Status: Assign approval statuses to models before deploying them to ensure quality control.
Deploy the Model: Use SageMaker endpoints for deployment once the model is approved.
Benefits:
Centralized Management: Provides a unified platform to manage models efficiently.
Streamlined Deployment: Facilitates smooth transitions from development to production.
Governance and Compliance: Supports metadata association and approval processes.
By leveraging the SageMaker Model Registry, the company can ensure organized management of models, version control, and efficient deployment workflows with minimal operational overhead.
AWS Documentation: SageMaker Model Registry
AWS Blog: Model Registry Features and Usage

## Question 17

An ML engineer wants to use Amazon SageMaker Data Wrangler to perform preprocessing on a dataset. The ML engineer wants to use the processed dataset to train a classification model. During preprocessing, the ML engineer notices that a text feature has a range of thousands of values that differ only by spelling errors. The ML engineer needs to apply an encoding method so that after preprocessing is complete, the text feature can be used to train the model.
Which solution will meet these requirements?

- A. Perform ordinal encoding to represent categories of the feature.
- B. Perform similarity encoding to represent categories of the feature.
- C. Perform one-hot encoding to represent categories of the feature.
- D. Perform target encoding to represent categories of the feature.

**Answer:** B

**Explanation:**
The text feature contains high-cardinality categorical values with minor spelling variations, which is a common real-world data quality issue. Traditional encoding techniques such as ordinal encoding (Option A) or one-hot encoding (Option C) would treat each misspelled value as a completely separate category, resulting in poor generalization and very high dimensionality.
Similarity encoding addresses this problem by grouping or encoding categories based on string similarity. Categories that differ only slightly—such as spelling errors—are encoded with similar numerical representations. Amazon SageMaker Data Wrangler supports similarity encoding specifically for such noisy categorical features.
Target encoding (Option D) depends on the target label distribution and risks target leakage if not carefully applied.
Therefore, similarity encoding is the most appropriate and AWS-recommended solution.

## Question 18

A company is planning to create several ML prediction models. The training data is stored in Amazon S3. The entire dataset is more than 5 ТВ in size and consists of CSV, JSON, Apache Parquet, and simple text files.
The data must be processed in several consecutive steps. The steps include complex manipulations that can take hours to finish running. Some of the processing involves natural language processing (NLP) transformations. The entire process must be automated.
Which solution will meet these requirements?

- A. Process data at each step by using Amazon SageMaker Data Wrangler. Automate the process by using Data Wrangler jobs.
- B. Use Amazon SageMaker notebooks for each data processing step. Automate the process by using Amazon EventBridge.
- C. Process data at each step by using AWS Lambda functions. Automate the process by using AWS Step Functions and Amazon EventBridge.
- D. Use Amazon SageMaker Pipelines to create a pipeline of data processing steps. Automate the pipeline by using Amazon EventBridge.

**Answer:** D

**Explanation:**
Amazon SageMaker Pipelines is designed for creating, automating, and managing end-to-end ML workflows, including complex data preprocessing tasks. It supports handling large datasets and can integrate with custom steps, such as NLP transformations. By combining SageMaker Pipelines with Amazon EventBridge, the entire workflow can be triggered and automated efficiently, meeting the requirements for scalability, automation, and processing complexity.

## Question 19

An ML engineer is collecting data to train a classification ML model by using Amazon SageMaker AI. The target column can have two possible values: Class A or Class B. The ML engineer wants to ensure that the number of samples for both Class A and Class B are balanced, without losing any existing training data. The ML engineer must test the balance of the training data.
Which solution will meet this requirement?

- A. Use SageMaker Clarify to check for class imbalance (CI). If the value is equal to 0, then use random undersampling in SageMaker Data Wrangler to balance the classes.
- B. Use SageMaker Clarify to check for class imbalance (CI). If the value is greater than 0, then use synthetic minority oversampling technique (SMOTE) in SageMaker Data Wrangler to balance the classes.
- C. Use SageMaker JumpStart to generate a class imbalance (CI) report. If the value is greater than 0, then use random undersampling in SageMaker Studio to balance the classes.
- D. Use SageMaker JumpStart to generate a class imbalance (CI) report. If the value is equal to 0, then use synthetic minority oversampling technique (SMOTE) in SageMaker Studio to balance the classes.

**Answer:** B

**Explanation:**
The requirement has two key constraints: detect class imbalance and balance classes without losing any existing data. AWS provides Amazon SageMaker Clarify as the native tool to detect pre-training bias, including class imbalance (CI). CI measures differences in label distributions between classes, and a CI value greater than 0 indicates imbalance.
Once imbalance is detected, the engineer must rebalance the dataset without discarding data. Random undersampling would remove samples from the majority class, violating the requirement. Instead, oversampling is required. SMOTE (Synthetic Minority Oversampling Technique) creates synthetic samples for the minority class, preserving all original data while improving class balance.
Amazon SageMaker Data Wrangler natively supports SMOTE, making it the correct AWS-managed tool for this preprocessing task.
Options C and D are incorrect because SageMaker JumpStart is used for pretrained models and solutions, not for bias detection reporting. Option A is incorrect because it uses undersampling and misinterprets CI = 0 (which actually indicates no imbalance).
Therefore, detecting imbalance with SageMaker Clarify and correcting it using SMOTE in Data Wrangler is the correct solution.

## Question 20

A company wants to share data with a vendor in real time to improve the performance of the vendor ' s ML models. The vendor needs to ingest the data in a stream. The vendor will use only some of the columns from the streamed data.
Which solution will meet these requirements?

- A. Use AWS Data Exchange to stream the data to an Amazon S3 bucket. Use an Amazon Athena CREATE TABLE AS SELECT (CTAS) query to define relevant columns.
- B. Use Amazon Kinesis Data Streams to ingest the data. Use Amazon Managed Service for Apache Flink as a consumer to extract relevant columns.
- C. Create an Amazon S3 bucket. Configure the S3 bucket policy to allow the vendor to upload data to the S3 bucket. Configure the S3 bucket policy to control which columns are shared.
- D. Use AWS Lake Formation to ingest the data. Use the column-level filtering feature in Lake Formation to extract relevant columns.

**Answer:** B

**Explanation:**
The requirement specifies real-time streaming ingestion and column-level transformation before sharing data with a vendor. Amazon Kinesis Data Streams is designed for low-latency, real-time data ingestion and delivery.
To extract only required columns from the stream, AWS recommends using Amazon Managed Service for Apache Flink as a stream consumer. Flink enables real-time transformations such as filtering, projection, and enrichment on streaming data before delivering it downstream.
Option A and D are batch-oriented and not suitable for real-time streaming. Option C is incorrect because S3 bucket policies cannot enforce column-level access controls.
Therefore, Kinesis Data Streams combined with Apache Flink meets all requirements.

## Question 21

A company is building an Amazon SageMaker AI pipeline for an ML model. The pipeline uses distributed processing and distributed training.
An ML engineer needs to encrypt network communication between instances that run distributed jobs. The ML engineer configures the distributed jobs to run in a private VPC.
What should the ML engineer do to meet the encryption requirement?

- A. Enable network isolation.
- B. Configure traffic encryption by using security groups.
- C. Enable inter-container traffic encryption.
- D. Enable VPC flow logs.

**Answer:** C

**Explanation:**
In Amazon SageMaker, distributed training and distributed processing jobs often involve multiple instances exchanging data over the network. By default, when these jobs run inside a VPC, network traffic remains private but is not automatically encrypted between instances. When compliance or security requirements mandate encryption of in-transit data, additional configuration is required.
The correct solution is to enable inter-container traffic encryption, which ensures that all network communication between containers running on different instances is encrypted using TLS. Amazon SageMaker provides a built-in feature for this purpose. When inter-container traffic encryption is enabled, SageMaker automatically configures secure communication channels between all nodes participating in a distributed job, including training clusters and processing jobs.
Option A (Network isolation) is incorrect because network isolation prevents containers from making outbound network calls and accessing the internet. It does not encrypt traffic between instances.
Option B (Security groups) is incorrect because security groups control network access and traffic flow, not encryption. They can restrict which instances can communicate, but they do not provide data-in-transit encryption.
Option D (VPC flow logs) is incorrect because VPC flow logs are used for monitoring and auditing network traffic, not for encrypting it.
AWS documentation explicitly states that enabling inter-container traffic encryption is the recommended and supported approach for encrypting data exchanged between instances during distributed SageMaker jobs. This feature aligns with enterprise security best practices and regulatory requirements for protecting sensitive ML training data in transit.
Therefore, Option C is the only solution that directly fulfills the encryption requirement for distributed SageMaker workloads.

## Question 22

A company is gathering audio, video, and text data in various languages. The company needs to use a large language model (LLM) to summarize the gathered data that is in Spanish.
Which solution will meet these requirements in the LEAST amount of time?

- A. Train and deploy a model in Amazon SageMaker to convert the data into English text. Train and deploy an LLM in SageMaker to summarize the text.
- B. Use Amazon Transcribe and Amazon Translate to convert the data into English text. Use Amazon Bedrock with the Jurassic model to summarize the text.
- C. Use Amazon Rekognition and Amazon Translate to convert the data into English text. Use Amazon Bedrock with the Anthropic Claude model to summarize the text.
- D. Use Amazon Comprehend and Amazon Translate to convert the data into English text. Use Amazon Bedrock with the Stable Diffusion model to summarize the text.

**Answer:** B

**Explanation:**
Amazon Transcribe is well-suited for converting audio data into text, including Spanish.
Amazon Translate can efficiently translate Spanish text into English if needed.
Amazon Bedrock, with the Jurassic model, is designed for tasks like text summarization and can handle large language models (LLMs) seamlessly. This combination provides a low-code, managed solution to process audio, video, and text data with minimal time and effort.

## Question 23

Case study
An ML engineer is developing a fraud detection model on AWS. The training dataset includes transaction logs, customer profiles, and tables from an on-premises MySQL database. The transaction logs and customer profiles are stored in Amazon S3.
The dataset has a class imbalance that affects the learning of the model ' s algorithm. Additionally, many of the features have interdependencies. The algorithm is not capturing all the desired underlying patterns in the data.
Which AWS service or feature can aggregate the data from the various data sources?

- A. Amazon EMR Spark jobs
- B. Amazon Kinesis Data Streams
- C. Amazon DynamoDB
- D. AWS Lake Formation

**Answer:** A

**Explanation:**
Problem Description:
The dataset includes multiple data sources:
Transaction logs and customer profiles in Amazon S3.
Tables in an on-premises MySQL database.
There is a class imbalance in the dataset and interdependencies among features that need to be addressed.
The solution requires data aggregation from diverse sources for centralized processing.
Why AWS Lake Formation?
AWS Lake Formation is designed to simplify the process of aggregating, cataloging, and securing data from various sources, including S3, relational databases, and other on-premises systems.
It integrates with AWS Glue for data ingestion and ETL (Extract, Transform, Load) workflows, making it a robust choice for aggregating data from Amazon S3 and on-premises MySQL databases.
How It Solves the Problem:
Data Aggregation: Lake Formation collects data from diverse sources, such as S3 and MySQL, and consolidates it into a centralized data lake.
Cataloging and Discovery: Automatically crawls and catalogs the data into a searchable catalog, which the ML engineer can query for analysis or modeling.
Data Transformation: Prepares data using Glue jobs to handle preprocessing tasks such as addressing class imbalance (e.g., oversampling, undersampling) and handling interdependencies among features.
Security and Governance: Offers fine-grained access control, ensuring secure and compliant data management.
Steps to Implement Using AWS Lake Formation:
Step 1: Set up Lake Formation and register data sources, including the S3 bucket and on-premises MySQL database.
Step 2: Use AWS Glue to create ETL jobs to transform and prepare data for the ML pipeline.
Step 3: Query and access the consolidated data lake using services such as Athena or SageMaker for further ML processing.
Why Not Other Options?
Amazon EMR Spark jobs: While EMR can process large-scale data, it is better suited for complex big data analytics tasks and does not inherently support data aggregation across sources like Lake Formation.
Amazon Kinesis Data Streams: Kinesis is designed for real-time streaming data, not batch data aggregation across diverse sources.
Amazon DynamoDB: DynamoDB is a NoSQL database and is not suitable for aggregating data from multiple sources like S3 and MySQL.
Conclusion: AWS Lake Formation is the most suitable service for aggregating data from S3 and on-premises MySQL databases, preparing the data for downstream ML tasks, and addressing challenges like class imbalance and feature interdependencies.
AWS Lake Formation Documentation
AWS Glue for Data Preparation

## Question 24

An airline company deploys ML models to one dozen Amazon SageMaker Al inference endpoints. The inference endpoints must be able to handle different types of
workloads in a cost-effective way.
Select the correct inference option from the following list to handle each type of workload. Select each inference option one time. (Select FOUR.)
Asynchronous inference
Batch inference
Real-time inference
Serverless inference
Suggested Solution
Expert Solution
Answer
Answer:
Show Explanation
Explanation
Provide flight departure, arrival, and delay information, and provide updates for low-latency workloads→ Real-time inference
Advertise holiday travel promotional deals to millions of users in multiple markets before holiday seasons for spiky workloads→ Serverless inference
Generate quarterly and annual flight reports and insights for trend analysis of large datasets→ Batch inference
Generate online image and audio stories for passengers to watch or listen to while waiting at an airport→ Asynchronous inference
The correct mapping depends on latency requirement, traffic pattern, payload size, processing duration, and whether the workload needs a persistent endpoint.
Real-time inference is the right choice for flight departure, arrival, and delay updates because this is an online user-facing workload that requires low latency. AWS states that SageMaker real-time inference is ideal for online inference workloads with low-latency or high-throughput requirements and uses a persistent fully managed endpoint. That fits flight status information because passengers and airline systems expect immediate responses.
Serverless inference is the best choice for holiday promotional deals because this traffic is spiky, seasonal, and unpredictable. AWS describes SageMaker Serverless Inference as suitable for intermittent or unpredictable traffic patterns. It is cost-effective because SageMaker manages the infrastructure and scales down when there are no requests, so the company does not pay for idle endpoint capacity.
Batch inference is correct for quarterly and annual flight reports because this workload analyzes large datasets offline and does not need an always-running endpoint. AWS says SageMaker batch transform is used to get inferences from large datasets and when a persistent endpoint is not required. Reports and trend analysis are scheduled, non-real-time analytics workloads, so batch inference is the most cost-effective option.
Asynchronous inference is the right choice for generating online image and audio stories. These requests can have larger payloads and longer processing times than normal low-latency API calls. AWS states that SageMaker Asynchronous Inference queues incoming requests and is ideal for large payloads, long processing times, and near-real-time latency requirements. Image and audio generation can take seconds or minutes, so asynchronous inference is more appropriate than real-time inference.


**Answer:** 

**Explanation:**
Provide flight departure, arrival, and delay information, and provide updates for low-latency workloads→ Real-time inference
Advertise holiday travel promotional deals to millions of users in multiple markets before holiday seasons for spiky workloads→ Serverless inference
Generate quarterly and annual flight reports and insights for trend analysis of large datasets→ Batch inference
Generate online image and audio stories for passengers to watch or listen to while waiting at an airport→ Asynchronous inference
The correct mapping depends on latency requirement, traffic pattern, payload size, processing duration, and whether the workload needs a persistent endpoint.
Real-time inference is the right choice for flight departure, arrival, and delay updates because this is an online user-facing workload that requires low latency. AWS states that SageMaker real-time inference is ideal for online inference workloads with low-latency or high-throughput requirements and uses a persistent fully managed endpoint. That fits flight status information because passengers and airline systems expect immediate responses.
Serverless inference is the best choice for holiday promotional deals because this traffic is spiky, seasonal, and unpredictable. AWS describes SageMaker Serverless Inference as suitable for intermittent or unpredictable traffic patterns. It is cost-effective because SageMaker manages the infrastructure and scales down when there are no requests, so the company does not pay for idle endpoint capacity.
Batch inference is correct for quarterly and annual flight reports because this workload analyzes large datasets offline and does not need an always-running endpoint. AWS says SageMaker batch transform is used to get inferences from large datasets and when a persistent endpoint is not required. Reports and trend analysis are scheduled, non-real-time analytics workloads, so batch inference is the most cost-effective option.
Asynchronous inference is the right choice for generating online image and audio stories. These requests can have larger payloads and longer processing times than normal low-latency API calls. AWS states that SageMaker Asynchronous Inference queues incoming requests and is ideal for large payloads, long processing times, and near-real-time latency requirements. Image and audio generation can take seconds or minutes, so asynchronous inference is more appropriate than real-time inference.

## Question 25

An ML engineer is building an ML model in Amazon SageMaker AI. The ML engineer needs to load historical data directly from Amazon S3, Amazon Athena, and Snowflake into SageMaker AI.
Which solution will meet this requirement?

- A. Use AWS Glue DataBrew to import the data into SageMaker AI.
- B. Build a pipeline in SageMaker Pipelines to process the data. Use AWS DataSync to load the processed data into SageMaker AI.
- C. Create a feature store in SageMaker Feature Store. Use an Apache Spark connector to Feature Store to access the data.
- D. Use SageMaker Data Wrangler to query and import the data.

**Answer:** D

**Explanation:**
AWS provides Amazon SageMaker Data Wrangler as a native tool for importing, transforming, and analyzing data from multiple sources directly into SageMaker Studio. Data Wrangler supports Amazon S3, Amazon Athena, and Snowflake as built-in data sources through managed connectors.
Using Data Wrangler, ML engineers can query data from Athena using SQL, load structured files from S3, and securely connect to Snowflake without writing custom ingestion code. This approach significantly reduces development effort and aligns with AWS best practices for rapid ML experimentation.
Option A is incorrect because AWS Glue DataBrew is designed for data preparation but does not natively integrate with SageMaker training workflows. Option B introduces unnecessary complexity and is not intended for direct ML data loading. Option C focuses on feature storage, not raw historical data ingestion.
Therefore, SageMaker Data Wrangler is the correct solution.

## Question 26

An ML engineer at a credit card company built and deployed an ML model by using Amazon SageMaker AI. The model was trained on transaction data that contained very few fraudulent transactions. After deployment, the model is underperforming.
What should the ML engineer do to improve the model’s performance?

- A. Retrain the model with a different SageMaker built-in algorithm.
- B. Use random undersampling to reduce the majority class and retrain the model.
- C. Use Synthetic Minority Oversampling Technique (SMOTE) to generate synthetic minority samples and retrain the model.
- D. Use random oversampling to duplicate minority samples and retrain the model.

**Answer:** C

**Explanation:**
This is a classic class imbalance problem, where fraudulent transactions (minority class) are severely underrepresented. AWS documentation for SageMaker Data Wrangler recommends SMOTE (Synthetic Minority Oversampling Technique) as an effective approach for improving model performance in such scenarios.
SMOTE generates synthetic minority samples by interpolating between existing minority class examples. This improves the model’s ability to learn decision boundaries without simply duplicating data, which can cause overfitting.
Random undersampling removes valuable majority class data, reducing overall model robustness. Random oversampling duplicates data and increases overfitting risk. Changing algorithms does not address the root cause.
AWS best practices highlight SMOTE as the preferred technique for fraud detection and other highly imbalanced datasets.
Therefore, Option C is the correct and AWS-verified answer.

## Question 27

An ML engineer needs to use Amazon SageMaker to fine-tune a large language model (LLM) for text summarization. The ML engineer must follow a low-code no-code (LCNC) approach.
Which solution will meet these requirements?

- A. Use SageMaker Studio to fine-tune an LLM that is deployed on Amazon EC2 instances.
- B. Use SageMaker Autopilot to fine-tune an LLM that is deployed by a custom API endpoint.
- C. Use SageMaker Autopilot to fine-tune an LLM that is deployed on Amazon EC2 instances.
- D. Use SageMaker Autopilot to fine-tune an LLM that is deployed by SageMaker JumpStart.

**Answer:** D

**Explanation:**
SageMaker JumpStart provides access to pre-trained models, including large language models (LLMs), which can be easily deployed and fine-tuned with a low-code/no-code (LCNC) approach. Using SageMaker Autopilot with JumpStart simplifies the fine-tuning process by automating model optimization and reducing the need for extensive coding, making it the ideal solution for this requirement.

## Question 28

An ML engineer is using a training job to fine-tune a deep learning model in Amazon SageMaker Studio. The ML engineer previously used the same pre-trained model with a similar
dataset. The ML engineer expects vanishing gradient, underutilized GPU, and overfitting problems.
The ML engineer needs to implement a solution to detect these issues and to react in predefined ways when the issues occur. The solution also must provide comprehensive real-time metrics during the training.
Which solution will meet these requirements with the LEAST operational overhead?

- A. Use TensorBoard to monitor the training job. Publish the findings to an Amazon Simple Notification Service (Amazon SNS) topic. Create an AWS Lambda function to consume the findings and to initiate the predefined actions.
- B. Use Amazon CloudWatch default metrics to gain insights about the training job. Use the metrics to invoke an AWS Lambda function to initiate the predefined actions.
- C. Expand the metrics in Amazon CloudWatch to include the gradients in each training step. Use the metrics to invoke an AWS Lambda function to initiate the predefined actions.
- D. Use SageMaker Debugger built-in rules to monitor the training job. Configure the rules to initiate the predefined actions.

**Answer:** D

**Explanation:**
SageMaker Debugger provides built-in rules to automatically detect issues like vanishing gradients, underutilized GPU, and overfitting during training jobs. It generates real-time metrics and allows users to define predefined actions that are triggered when specific issues occur. This solution minimizes operational overhead by leveraging the managed monitoring capabilities of SageMaker Debugger without requiring custom setups or extensive manual intervention.

## Question 29

A company has AWS Glue data processing jobs that are orchestrated by an AWS Glue workflow. The AWS Glue jobs can run on a schedule or can be launched manually.
The company is developing pipelines in Amazon SageMaker Pipelines for ML model development. The pipelines will use the output of the AWS Glue jobs during the data processing phase of model development. An ML engineer needs to implement a solution that integrates the AWS Glue jobs with the pipelines.
Which solution will meet these requirements with the LEAST operational overhead?

- A. Use AWS Step Functions for orchestration of the pipelines and the AWS Glue jobs.
- B. Use processing steps in SageMaker Pipelines. Configure inputs that point to the Amazon Resource Names (ARNs) of the AWS Glue jobs.
- C. Use Callback steps in SageMaker Pipelines to start the AWS Glue workflow and to stop the pipelines until the AWS Glue jobs finish running.
- D. Use Amazon EventBridge to invoke the pipelines and the AWS Glue jobs in the desired order.

**Answer:** C

**Explanation:**
Callback steps in Amazon SageMaker Pipelines allow you to integrate external processes, such as AWS Glue jobs, into the pipeline workflow. By using a Callback step, the SageMaker pipeline can trigger the AWS Glue workflow and pause execution until the Glue jobs complete. This approach provides seamless integration with minimal operational overhead, as it directly ties the pipeline ' s execution flow to the completion of the AWS Glue jobs without requiring additional orchestration tools or complex setups.

## Question 30

A company is training a deep learning model to detect abnormalities in images. The company has limited GPU resources and a large hyperparameter space to explore. The company needs to test different configurations and avoid wasting computation time on poorly performing models that show weak validation accuracy in early epochs.
Which hyperparameter optimization strategy should the company use?

- A. Grid search across all possible combinations
- B. Bayesian optimization with early stopping
- C. Manual tuning of each parameter individually
- D. Exhaustive search without early stopping

**Answer:** B

**Explanation:**
When GPU resources are limited and the hyperparameter search space is large, AWS documentation strongly recommends Bayesian optimization combined with early stopping. Bayesian optimization uses past evaluation results to intelligently select the next set of hyperparameters to test, focusing exploration on promising regions of the search space rather than testing all combinations.
In Amazon SageMaker, Bayesian optimization is the default and recommended strategy for hyperparameter tuning jobs. It significantly reduces the number of training runs required compared to grid or random search, making it highly cost-efficient for deep learning workloads.
Early stopping further improves efficiency by terminating training jobs that show poor validation performance in early epochs. This prevents wasted GPU time on configurations that are unlikely to perform well. AWS explicitly documents early stopping as a key feature for controlling training cost and duration.
Grid search and exhaustive search are computationally expensive and impractical for large hyperparameter spaces. Manual tuning is slow, error-prone, and does not scale.
By combining Bayesian optimization with early stopping, the company can rapidly converge on high-performing hyperparameter configurations while minimizing resource usage.
Therefore, Option B is the correct and AWS-aligned solution.

## Question 31

A company uses ML models to predict whether transactions are fraudulent. The company needs to identify as many fraudulent transactions as possible. Which evaluation metric should the company use to evaluate the models to meet this requirement?

- A. F1 score
- B. Area Under the ROC Curve (AUC)
- C. Precision
- D. Recall

**Answer:** D

**Explanation:**
Option D is correct because the company’s primary goal is to identify as many fraudulent transactions as possible. In AWS documentation, recall is defined as TP / (TP + FN), where TP is true positives and FN is false negatives. Recall measures how well a model finds all actual positive cases. In a fraud-detection setting, the “positive” class is the fraudulent transaction, so maximizing recall means minimizing missed fraud cases.
AWS documentation also explains the business tradeoff clearly: a use case that needs to correctly predict as many positive examples as possible should prioritize high recall, even if that means accepting some additional false positives and therefore only moderate precision. That matches this question exactly, because the requirement is not to be most selective or most balanced overall; it is specifically to catch the largest possible number of fraudulent transactions.
The other metrics are less aligned to the stated goal. Precision focuses on how many predicted fraud cases are actually fraud, which is most important when false positives are very costly. F1 score balances precision and recall, but the question does not ask for balance; it asks for finding as many fraudulent transactions as possible. AUC is useful for overall ranking and threshold-independent model discrimination, but it is not the most direct metric for optimizing missed-fraud detection in this scenario. Based on AWS metric definitions, when the cost of missing true positives is the main concern, recall is the best evaluation metric. Therefore, the best verified answer is D.

## Question 32

An ML engineer is using an Amazon SageMaker AI shadow test to evaluate a new model that is hosted on a SageMaker AI endpoint. The shadow test requires significant GPU resources for high performance. The production variant currently runs on a less powerful instance type.
The ML engineer needs to configure the shadow test to use a higher performance instance type for a shadow variant. The solution must not affect the instance type of the production variant.
Which solution will meet these requirements?

- A. Modify the existing ProductionVariant configuration in the endpoint to include a ShadowProductionVariants list. Specify the larger instance type for the shadow variant.
- B. Create a new endpoint configuration with two ProductionVariant definitions. Configure one definition for the existing production variant and one definition for the shadow variant with the larger instance type. Use the UpdateEndpoint action to apply the new configuration.
- C. Create a separate SageMaker AI endpoint for the shadow variant that uses the larger instance type. Create an AWS Lambda function that routes a portion of the traffic to the shadow endpoint. Assign the Lambda function to the original endpoint.
- D. Use the CreateEndpointConfig action to define a new configuration. Specify the existing production variant in the configuration and add a separate ShadowProductionVariants list. Specify the larger instance type for the shadow variant. Use the CreateEndpoint action and pass the new configuration to the endpoint.

**Answer:** D

**Explanation:**
Amazon SageMaker AI shadow testing enables ML engineers to evaluate new model versions by sending a copy of live production traffic to a shadow variant without affecting production inference responses. AWS documentation specifies that shadow variants are configured separately from production variants and can use different instance types, including higher-performance GPU instances.
The correct approach is to create a new endpoint configuration using the CreateEndpointConfig API. This configuration includes the existing production variant and a separate ShadowProductionVariants list. The shadow variant can be assigned a larger instance type to meet GPU performance requirements while leaving the production variant unchanged. After creating the configuration, the engineer deploys it using the CreateEndpoint action.
Option A is incorrect because production variant configurations cannot be directly modified to include shadow variants. Option B is incorrect because shadow variants are not defined as standard production variants; defining two production variants would route traffic differently and could affect production behavior. Option C introduces unnecessary complexity and deviates from SageMaker’s built-in shadow testing functionality.
AWS explicitly documents that shadow variants are designed to isolate testing resources, support different instance types, and ensure zero impact on production inference. Therefore, Option D is the correct and AWS-recommended solution.

## Question 33

An ML engineer is training an ML model to identify medical patients for disease screening. The tabular dataset for training contains 50,000 patient records: 1,000 with the disease and 49,000 without the disease.
The ML engineer splits the dataset into a training dataset, a validation dataset, and a test dataset.
What should the ML engineer do to transform the data and make the data suitable for training?

- A. Apply principal component analysis (PCA) to oversample the minority class in the training dataset.
- B. Apply Synthetic Minority Oversampling Technique (SMOTE) to generate new synthetic samples of the minority class in the training dataset.
- C. Randomly oversample the majority class in the validation dataset.
- D. Apply k-means clustering to undersample the minority class in the test dataset.

**Answer:** B

**Explanation:**
This dataset shows severe class imbalance, with only 2% of records representing patients with the disease. AWS ML best practices recommend correcting imbalance only in the training dataset, while keeping validation and test sets representative of real-world distributions.
Synthetic Minority Oversampling Technique (SMOTE) generates synthetic samples of the minority class by interpolating between existing minority examples. This improves the model’s ability to learn disease-related patterns without discarding data.
PCA is a dimensionality reduction method, not an oversampling technique. Oversampling the majority class worsens imbalance. Altering the test dataset would invalidate evaluation results.
Therefore, applying SMOTE to the training dataset is the correct approach.

## Question 34

A company uses AWS CodePipeline to orchestrate a continuous integration and continuous delivery (CI/CD) pipeline for ML models and applications.
Select and order the steps from the following list to describe a CI/CD process for a successful deployment. Select each step one time. (Select and order FIVE.)
. CodePipeline deploys ML models and applications to production.
· CodePipeline detects code changes and starts to build automatically.
. Human approval is provided after testing is successful.
. The company builds and deploys ML models and applications to staging servers for testing.
. The company commits code changes or new training datasets to a Git repository.
Suggested Solution
Expert Solution
Answer
Answer:
Show Explanation
Explanation
Step 1:
The company commits code changes or new training datasets to a Git repository.
This is the trigger point. A source code or data change initiates the CI/CD pipeline.
Step 2:
CodePipeline detects code changes and starts to build automatically.
CodePipeline monitors the Git repository (for example, AWS CodeCommit, GitHub, or Bitbucket) and automatically triggers the pipeline when changes are detected.
Step 3:
The company builds and deploys ML models and applications to staging servers for testing.
The pipeline runs build, training, and test stages (often using AWS CodeBuild and SageMaker) and deploys artifacts to a staging or test environment for validation.
Step 4:
Human approval is provided after testing is successful.
A manual approval action is a best practice for ML workflows to ensure governance, compliance, and quality checks before production deployment.
Step 5:
CodePipeline deploys ML models and applications to production.
After approval, the pipeline automatically deploys the validated model or application to the production environment.


**Answer:** 

**Explanation:**
Step 1:
The company commits code changes or new training datasets to a Git repository.
This is the trigger point. A source code or data change initiates the CI/CD pipeline.
Step 2:
CodePipeline detects code changes and starts to build automatically.
CodePipeline monitors the Git repository (for example, AWS CodeCommit, GitHub, or Bitbucket) and automatically triggers the pipeline when changes are detected.
Step 3:
The company builds and deploys ML models and applications to staging servers for testing.
The pipeline runs build, training, and test stages (often using AWS CodeBuild and SageMaker) and deploys artifacts to a staging or test environment for validation.
Step 4:
Human approval is provided after testing is successful.
A manual approval action is a best practice for ML workflows to ensure governance, compliance, and quality checks before production deployment.
Step 5:
CodePipeline deploys ML models and applications to production.
After approval, the pipeline automatically deploys the validated model or application to the production environment.

## Question 35

A company uses a training job on Amazon SageMaker Al to train a neural network. The job first trains a model and then evaluates the model ' s performance ag
test dataset. The company uses the results from the evaluation phase to decide if the trained model will go to production.
The training phase takes too long. The company needs solutions that can shorten training time without decreasing the model ' s final performance.
Select the correct solutions from the following list to meet the requirements for each description. Select each solution one time or not at all. (Select THREE.)
. Change the epoch count.
. Choose an Amazon EC2 Spot Fleet.
· Change the batch size.
. Use early stopping on the training job.
· Use the SageMaker Al distributed data parallelism (SMDDP) library.
. Stop the training job.
Suggested Solution
Expert Solution
Answer
Answer:
Show Explanation
Explanation
Change the number of samples used in each iteration of training
Correct selection:
Change the batch size
Why:
Increasing the batch size reduces the number of iterations per epoch, which can significantly shorten training time while maintaining model quality when tuned appropriately. AWS explicitly recommends batch size tuning as a primary performance optimization.
Increase the number of instances used during training
Correct selection:
Use the SageMaker AI distributed data parallelism (SMDDP) library
Why:
SMDDP is designed to efficiently distribute training data across multiple GPU instances with optimized gradient synchronization. This accelerates training without affecting model convergence or accuracy, unlike naive scaling approaches.
Stop training before the maximum number of epochs are reached if performance is sufficient and not improving
Correct selection:
Use early stopping on the training job
Why:
Early stopping automatically terminates training when validation metrics stop improving. AWS recommends this to reduce wasted compute time while preserving optimal model performance.


**Answer:** 

**Explanation:**
Change the number of samples used in each iteration of training
Correct selection:
Change the batch size
Why:
Increasing the batch size reduces the number of iterations per epoch, which can significantly shorten training time while maintaining model quality when tuned appropriately. AWS explicitly recommends batch size tuning as a primary performance optimization.
Increase the number of instances used during training
Correct selection:
Use the SageMaker AI distributed data parallelism (SMDDP) library
Why:
SMDDP is designed to efficiently distribute training data across multiple GPU instances with optimized gradient synchronization. This accelerates training without affecting model convergence or accuracy, unlike naive scaling approaches.
Stop training before the maximum number of epochs are reached if performance is sufficient and not improving
Correct selection:
Use early stopping on the training job
Why:
Early stopping automatically terminates training when validation metrics stop improving. AWS recommends this to reduce wasted compute time while preserving optimal model performance.

## Question 36

An ML engineer is building a model to predict house and apartment prices. The model uses three features: Square Meters, Price, and Age of Building. The dataset has 10,000 data rows. The data includes data points for one large mansion and one extremely small apartment.
The ML engineer must perform preprocessing on the dataset to ensure that the model produces accurate predictions for the typical house or apartment.
Which solution will meet these requirements?

- A. Remove the outliers and perform a log transformation on the Square Meters variable.
- B. Keep the outliers and perform normalization on the Square Meters variable.
- C. Remove the outliers and perform one-hot encoding on the Square Meters variable.
- D. Keep the outliers and perform one-hot encoding on the Square Meters variable.

**Answer:** A

**Explanation:**
In regression problems such as house price prediction, extreme values can significantly distort model learning. In this dataset, the presence of a large mansion and an extremely small apartment represents clear outliers in the Square Meters feature. According to AWS Machine Learning best practices, outliers can disproportionately influence loss functions (such as mean squared error), leading to poor predictions for the majority of typical data points.
Removing these outliers helps the model focus on learning patterns that apply to the majority of houses and apartments, which aligns with the requirement to produce accurate predictions for typical properties. After removing outliers, applying a log transformation to the Square Meters feature further improves model performance by reducing skewness and stabilizing variance. Log transformations are commonly recommended in AWS and general ML documentation when numerical features span multiple orders of magnitude.
Option B is incorrect because normalization alone does not address the undue influence of extreme outliers. Option C and D are incorrect because one-hot encoding is intended for categorical variables, not continuous numerical features such as square meters.
Therefore, removing outliers and applying a log transformation is the most statistically sound preprocessing approach.

## Question 37

A company has trained an ML model that is packaged in a container. The company will integrate the model with an existing Python web application. The company needs to host the model on AWS by using Kubernetes.
The company does not want to manage the control plane and must provision the resources in a repeatable manner. The infrastructure must be provisioned by using Python.
Which solution will meet these requirements?

- A. Use AWS CloudFormation to provision Amazon EC2 instances in multiple Availability Zones. Set up a Kubernetes cluster. Host the model container on the Kubernetes cluster.
- B. Use the AWS CLI to provision an Amazon Elastic Kubernetes Service (Amazon EKS) cluster. Store the image in an Amazon Elastic Container Registry (Amazon ECR) repository. Host the model container on the EKS cluster.
- C. Use the AWS Cloud Development Kit (AWS CDK) to provision an Amazon Elastic Kubernetes Service (Amazon EKS) cluster. Store the image in an Amazon Elastic Container Registry (Amazon ECR) repository. Host the model container on the EKS cluster.
- D. Use AWS CloudFormation to provision an Amazon Elastic Kubernetes Service (Amazon EKS) cluster. Store the image in an Amazon Elastic Container Registry (Amazon ECR) repository. Host the model container on the EKS cluster.

**Answer:** C

**Explanation:**
Option C is correct because the company needs Kubernetes hosting, does not want to manage the control plane, wants repeatable infrastructure provisioning, and requires that provisioning be done by using Python. Amazon EKS is AWS’s managed Kubernetes service, so it satisfies the requirement to avoid managing the Kubernetes control plane directly. The AWS CDK documentation also confirms that Python is a fully supported client language for defining infrastructure as code.
AWS CDK is the best fit because it lets engineers define cloud infrastructure programmatically in Python and deploy it in a repeatable way. The AWS CDK EKS construct library specifically supports defining Amazon EKS clusters and related Kubernetes resources. This makes it a strong match for infrastructure that must be reproducible and expressed in code rather than provisioned manually. Since the model is already packaged in a container, storing the image in Amazon ECR and then deploying it to Amazon EKS follows the normal AWS container workflow.
The other options are less suitable. Option A requires setting up and managing a Kubernetes cluster on EC2, which violates the requirement to avoid control-plane management. Option B uses the AWS CLI, but the question specifically requires infrastructure provisioning by using Python, not command-line provisioning. Option D uses CloudFormation, which is repeatable infrastructure as code, but the question explicitly says the infrastructure must be provisioned by using Python. AWS CDK uniquely satisfies both the IaC and Python requirements while using managed Kubernetes with EKS.
Therefore, the best verified AWS-docs answer is C.

## Question 38

A music streaming company constantly streams song ratings from an application to an Amazon S3 bucket. The company wants to use the ratings as an input for training and inference of an Amazon SageMaker AI model.
The company has an AWS Glue Data Catalog that is configured with the S3 bucket as the source. An ML engineer needs to implement a solution to create a repository for this data. The solution must ensure that the data stays synchronized during batch training and real-time inference.
Which solution will meet these requirements?

- A. Ingest data into SageMaker Feature Store from the S3 bucket. Apply tags and indexes.
- B. Use Amazon Athena. Create tables by using CREATE TABLE AS SELECT (CTAS) queries to group data.
- C. Use AWS Lake Formation. Apply tag-based control on the data.
- D. Use the Generate Data Insights function in SageMaker Data Wrangler.

**Answer:** A

**Explanation:**
Option A is correct because Amazon SageMaker Feature Store is the AWS service designed to act as a centralized repository for ML features that are used consistently across training and inference. AWS documentation states that SageMaker Feature Store simplifies how you create, store, share, and manage features for data exploration, model training, and model inference. This directly matches the requirement to create a repository for streamed song ratings that will be used in both batch training and real-time inference.
The most important requirement in the question is that the data must stay synchronized between batch training and real-time inference. AWS documents explain that Feature Store provides both an offline store and an online store. The offline store is used for historical data, model training, and batch inference, while the online store is a low-latency, high-availability store intended for real-time lookup during inference. This dual-store design is exactly why Feature Store is used to maintain feature consistency across training and serving workflows. AWS Well-Architected guidance also explicitly says Feature Store provides online storage for real-time inference and offline storage for model training and batch inference.
The other options do not solve the full problem. Athena CTAS can organize query results but does not provide a synchronized feature repository for online and offline ML use. Lake Formation governs access to data lakes but is not a feature repository for training and inference consistency. Data Wrangler Generate Data Insights is for analysis and preparation, not synchronized feature serving. Therefore, the best AWS-documented answer is A.

## Question 39

A company has an application that uses different APIs to generate embeddings for input text. The company needs to implement a solution to automatically rotate the API tokens every 3 months.
Which solution will meet this requirement?

- A. Store the tokens in AWS Secrets Manager. Create an AWS Lambda function to perform the rotation.
- B. Store the tokens in AWS Systems Manager Parameter Store. Create an AWS Lambda function to perform the rotation.
- C. Store the tokens in AWS Key Management Service (AWS KMS). Use an AWS managed key to perform the rotation.
- D. Store the tokens in AWS Key Management Service (AWS KMS). Use an AWS owned key to perform the rotation.

**Answer:** A

**Explanation:**
AWS Secrets Manager is designed for securely storing, managing, and automatically rotating secrets, including API tokens. By configuring a Lambda function for custom rotation logic, the solution can automatically rotate the API tokens every 3 months as required. Secrets Manager simplifies secret management and integrates seamlessly with other AWS services, making it the ideal choice for this use case.

## Question 40

An ML engineer is training a simple neural network model. The model’s performance improves initially and then degrades after a certain number of epochs.
Which solutions will mitigate this problem? (Select TWO.)

- A. Enable early stopping on the model.
- B. Increase dropout in the layers.
- C. Increase the number of layers.
- D. Increase the number of neurons.
- E. Investigate and reduce the sources of model bias.

**Answer:** 

**Explanation:**
The described behavior indicates overfitting, where the model starts to memorize training data instead of generalizing.
Early stopping halts training when validation performance stops improving, preventing the model from overfitting further. AWS documentation recommends early stopping as a primary regularization technique.
Dropout randomly disables neurons during training, forcing the model to learn robust representations and reducing reliance on specific neurons. Increasing dropout is a well-established method for improving generalization.
Increasing layers or neurons increases model capacity and worsens overfitting. Model bias is unrelated to epoch-based degradation.
Therefore, Options A and B are correct.

## Question 41

A company is running ML models on premises by using custom Python scripts and proprietary datasets. The company is using PyTorch. The model building requires unique domain knowledge. The company needs to move the models to AWS.
Which solution will meet these requirements with the LEAST development effort?

- A. Use SageMaker AI built-in algorithms to train the proprietary datasets.
- B. Use SageMaker AI script mode and premade images for ML frameworks.
- C. Build a container on AWS that includes custom packages and a choice of ML frameworks.
- D. Purchase similar production models through AWS Marketplace.

**Answer:** B

**Explanation:**
The company already has custom Python training scripts, proprietary datasets, and uses PyTorch, with significant domain-specific logic embedded in the model. The goal is to migrate these workloads to AWS with the least development effort.
According to AWS documentation, Amazon SageMaker AI script mode is explicitly designed for this scenario. Script mode allows customers to bring their existing training scripts with minimal or no code changes and run them using prebuilt SageMaker framework containers, including PyTorch. This approach eliminates the need to redesign models or rewrite training logic while still benefiting from SageMaker’s managed infrastructure, scalability, monitoring, and security.
Option A is incorrect because SageMaker built-in algorithms require adapting data formats and training logic to AWS-provided implementations, which would increase development effort and may not support proprietary domain logic.
Option C is also incorrect because building and maintaining a custom container requires additional effort for containerization, dependency management, security updates, and lifecycle maintenance—making it more complex than necessary.
Option D is not viable because purchasing models from AWS Marketplace would not support the company’s proprietary datasets or unique domain knowledge embedded in existing models.
Therefore, using SageMaker AI script mode with prebuilt PyTorch containers is the fastest, most efficient, and AWS-recommended migration path that minimizes development effort while preserving existing workflows.

## Question 42

A company wants to use large language models (LLMs) that are supported by Amazon Bedrock to develop a chat interface for the company ' s internal technical documentation. The company stores the documentation as dozens of text files that are several megabytes in total size. The company updates the text files often.
Which solution will meet these requirements MOST cost-effectively?

- A. Create a new LLM on Amazon Bedrock. Train the new LLM on the original dataset and the company documentation. Make the new model available in Bedrock for calls from the chat interface.
- B. Integrate the company documentation with Amazon Bedrock guardrails. Invoke the guardrails for all Amazon Bedrock calls from the chat interface.
- C. Use all the text files to fine tune a model in Amazon Bedrock. Use the fine-tuned model to process user prompts.
- D. Upload all the text files to an Amazon Bedrock knowledge base. Use the knowledge base to provide context when the chat interface makes calls to Amazon Bedrock.

**Answer:** D

**Explanation:**
Option D is correct because Amazon Bedrock Knowledge Bases are designed for applications that need to answer questions using private documents without retraining or repeatedly fine-tuning a foundation model. AWS documentation states that with Amazon Bedrock Knowledge Bases, you can build applications enriched by context retrieved from a knowledge base, and that this provides an out-of-the-box RAG solution. AWS also explicitly says that adding a knowledge base increases cost-effectiveness by removing the need to continually train your model to use your private data. That matches this use case very closely.
The question also says the documentation consists of text files that are only several megabytes total and are updated often. A retrieval-based approach is more economical and operationally simpler than creating a new model or repeatedly fine-tuning one whenever the documents change. AWS documentation for Bedrock knowledge bases describes adding data sources and running ingestion jobs to process and index the content, which is exactly the pattern needed for frequently updated internal documentation used by a chat interface.
The other options are not as cost-effective. Creating a new LLM is far beyond the need here. Guardrails help control model behavior and policy enforcement, but they do not serve as a document retrieval layer for internal documentation. Fine-tuning a model on frequently changing text files is usually more expensive and less flexible than using retrieval augmentation. For a modest-sized, frequently updated documentation corpus, the AWS-native and most cost-effective solution is to load the files into an Amazon Bedrock knowledge base and use it to provide context at inference time. Therefore, the best verified answer is D.

## Question 43

A company has significantly increased the amount of data that is stored as .csv files in an Amazon S3 bucket. Data transformation scripts and queries are now taking much longer than they used to take.
An ML engineer must implement a solution to optimize the data for query performance.
Which solution will meet this requirement with the LEAST operational overhead?

- A. Configure an AWS Lambda function to split the .csv files into smaller objects in the S3 bucket.
- B. Configure an AWS Glue job to drop columns that have string type values and to save the results to the S3 bucket.
- C. Configure an AWS Glue extract, transform, and load (ETL) job to convert the .csv files to Apache Parquet format.
- D. Configure an Amazon EMR cluster to process the data that is in the S3 bucket.

**Answer:** C

**Explanation:**
AWS documentation strongly recommends using columnar storage formats to optimize analytical query performance on large datasets stored in Amazon S3. Apache Parquet is a columnar, compressed, and splittable file format that significantly improves query speed and reduces I/O compared to row-based formats such as CSV.
By using AWS Glue to convert CSV files into Parquet format, the company can achieve faster query execution with minimal operational overhead. Glue is fully managed, serverless, and integrates natively with S3, Amazon Athena, and Amazon Redshift Spectrum.
Option A does not improve query efficiency; splitting files still leaves the data in an inefficient row-based format. Option B may reduce data size but does not address the fundamental inefficiency of CSV for analytics. Option D introduces significant operational overhead because Amazon EMR requires cluster provisioning, scaling, and maintenance.
Therefore, converting CSV files to Apache Parquet using AWS Glue ETL is the most efficient and low-maintenance solution.

## Question 44

A company needs to host a custom ML model to perform forecast analysis. The forecast analysis will occur with predictable and sustained load during the same 2-hour period every day.
Multiple invocations during the analysis period will require quick responses. The company needs AWS to manage the underlying infrastructure and any auto scaling activities.
Which solution will meet these requirements?

- A. Schedule an Amazon SageMaker batch transform job by using AWS Lambda.
- B. Configure an Auto Scaling group of Amazon EC2 instances to use scheduled scaling.
- C. Use Amazon SageMaker Serverless Inference with provisioned concurrency.
- D. Run the model on an Amazon Elastic Kubernetes Service (Amazon EKS) cluster on Amazon EC2 with pod auto scaling.

**Answer:** C

**Explanation:**
SageMaker Serverless Inference is ideal for workloads with predictable, intermittent demand. By enabling provisioned concurrency, the model can handle multiple invocations quickly during the high-demand 2-hour period. AWS manages the underlying infrastructure and scaling, ensuring the solution meets performance requirements with minimal operational overhead. This approach is cost-effective since it scales down when not in use.

## Question 45

A company is exploring generative AI and wants to add a new product feature. An ML engineer is making API calls from existing Amazon EC2 instances to Amazon Bedrock.
The EC2 instances are in a private subnet and must remain private during the implementation. The EC2 instances have a security group that allows access to all IP addresses in the private subnet.
What should the ML engineer do to establish a connection between the EC2 instances and Amazon Bedrock?

- A. Modify the security group to allow inbound and outbound traffic to and from Amazon Bedrock.
- B. Use AWS PrivateLink to access Amazon Bedrock through an interface VPC endpoint.
- C. Configure Amazon Bedrock to use the private subnet where the EC2 instances are deployed.
- D. Use AWS Direct Connect to link the VPC to Amazon Bedrock.

**Answer:** B

**Explanation:**
Amazon Bedrock is a fully managed AWS service and does not reside inside customer VPCs. To allow private connectivity from EC2 instances in a private subnet without using the public internet, AWS documentation recommends AWS PrivateLink.
By creating an interface VPC endpoint for Amazon Bedrock, traffic between the EC2 instances and Bedrock remains entirely within the AWS network. This approach preserves the private subnet configuration and meets strict security requirements.
Security groups alone cannot establish connectivity to a managed service endpoint. Amazon Bedrock cannot be deployed into a customer subnet, making Option C invalid. AWS Direct Connect is designed for on-premises connectivity, not for accessing AWS managed services from within a VPC.
AWS explicitly documents that PrivateLink is the correct mechanism for private access to Amazon Bedrock.
Therefore, Option B is the correct and AWS-verified answer.

## Question 46

A company has developed a new ML model. The company requires online model validation on 10% of the traffic before the company fully releases the model in production. The company uses an Amazon SageMaker endpoint behind an Application Load Balancer (ALB) to serve the model.
Which solution will set up the required online validation with the LEAST operational overhead?

- A. Use production variants to add the new model to the existing SageMaker endpoint. Set the variant weight to 0.1 for the new model. Monitor the number of invocations by using Amazon CloudWatch.
- B. Use production variants to add the new model to the existing SageMaker endpoint. Set the variant weight to 1 for the new model. Monitor the number of invocations by using Amazon CloudWatch.
- C. Create a new SageMaker endpoint. Use production variants to add the new model to the new endpoint. Monitor the number of invocations by using Amazon CloudWatch.
- D. Configure the ALB to route 10% of the traffic to the new model at the existing SageMaker endpoint. Monitor the number of invocations by using AWS CloudTrail.

**Answer:** A

**Explanation:**
Scenario: The company wants to perform online validation of a new ML model on 10% of the traffic before fully deploying the model in production. The setup must have minimal operational overhead.
Why Use SageMaker Production Variants?
Built-In Traffic Splitting: Amazon SageMaker endpoints support production variants, allowing multiple models to run on a single endpoint. You can direct a percentage of incoming traffic to each variant by adjusting the variant weights.
Ease of Management: Using production variants eliminates the need for additional infrastructure like separate endpoints or custom ALB configurations.
Monitoring with CloudWatch: SageMaker automatically integrates with CloudWatch, enabling real-time monitoring of model performance and invocation metrics.
Steps to Implement:
Deploy the New Model as a Production Variant:
Update the existing SageMaker endpoint to include the new model as a production variant. This can be done via the SageMaker console, CLI, or SDK.
Example SDK Code:
import boto3
sm_client = boto3.client( ' sagemaker ' )
response = sm_client.update_endpoint_weights_and_capacities(
EndpointName= ' existing-endpoint-name ' ,
DesiredWeightsAndCapacities=[
{ ' VariantName ' :  ' current-model ' ,  ' DesiredWeight ' : 0.9},
{ ' VariantName ' :  ' new-model ' ,  ' DesiredWeight ' : 0.1}
]
)
Set the Variant Weight:
Assign a weight of 0.1 to the new model and 0.9 to the existing model. This ensures 10% of traffic goes to the new model while the remaining 90% continues to use the current model.
Monitor the Performance:
Use Amazon CloudWatch metrics, such as InvocationCount and ModelLatency, to monitor the traffic and performance of each variant.
Validate the Results:
Analyze the performance of the new model based on metrics like accuracy, latency, and failure rates.
Why Not the Other Options?
Option B: Setting the weight to 1 directs all traffic to the new model, which does not meet the requirement of splitting traffic for validation.
Option C: Creating a new endpoint introduces additional operational overhead for traffic routing and monitoring, which is unnecessary given SageMaker ' s built-in production variant capability.
Option D: Configuring the ALB to route traffic requires manual setup and lacks SageMaker ' s seamless variant monitoring and traffic splitting features.
Conclusion:
Using production variants with a weight of 0.1 for the new model on the existing SageMaker endpoint provides the required traffic split for online validation with minimal operational overhead.
[References:, Amazon SageMaker Endpoints, SageMaker Production Variants, Monitoring SageMaker Endpoints with CloudWatch, , , , , ]

## Question 47

An ML engineer needs to implement a solution to host a trained ML model. The rate of requests to the model will be inconsistent throughout the day.
The ML engineer needs a scalable solution that minimizes costs when the model is not in use. The solution also must maintain the model ' s capacity to respond to requests during times of peak usage.
Which solution will meet these requirements?

- A. Create AWS Lambda functions that have fixed concurrency to host the model. Configure the Lambda functions to automatically scale based on the number of requests to the model.
- B. Deploy the model on an Amazon Elastic Container Service (Amazon ECS) cluster that uses AWS Fargate. Set a static number of tasks to handle requests during times of peak usage.
- C. Deploy the model to an Amazon SageMaker endpoint. Deploy multiple copies of the model to the endpoint. Create an Application Load Balancer to route traffic between the different copies of the model at the endpoint.
- D. Deploy the model to an Amazon SageMaker endpoint. Create SageMaker endpoint auto scaling policies that are based on Amazon CloudWatch metrics to adjust the number of instances dynamically.

**Answer:** 

**Explanation:**


## Question 48

A bank needs to use Amazon SageMaker AI to create an ML model to determine which customers qualify for a new product. The bank must use algorithms that SageMaker AI directly supports. The model must be explainable to the bank ' s regulators.
Which modeling approach will meet these requirements?

- A. Train the model by using the Object2Vec algorithm.
- B. Train the model by using the linear learner algorithm.
- C. Train a neural network.
- D. Train the model by using the k-means algorithm.

**Answer:** B

**Explanation:**
Option B is correct because the bank needs a SageMaker-supported algorithm and the resulting model must be explainable for regulators. AWS documentation states that the Amazon SageMaker AI linear learner algorithm is a built-in algorithm that provides a solution for classification and regression problems. Since determining whether customers qualify for a new product is a classification-style decision problem, linear learner fits the business requirement directly.
The explainability requirement strongly favors a simpler and more interpretable model family. AWS Prescriptive Guidance on model interpretability states that simple models are considered more interpretable because the effects of input features on the output are easier to understand. It specifically gives linear regression as an example where the magnitudes of coefficients provide a clear global feature-importance signal. While the question does not explicitly mention SageMaker Clarify, AWS documentation also says SageMaker offers explainability features to help interpret predictions, which complements inherently interpretable model types such as linear models.
The other options are weaker fits. Object2Vec is mainly for learning embeddings and similarity relationships rather than a straightforward explainable eligibility model. A neural network may work technically, but it is generally less interpretable for regulatory review. k-means is an unsupervised clustering algorithm and is not suitable for predicting whether a customer qualifies for a product. Because the bank wants a directly supported SageMaker algorithm and a model that is easier to explain to regulators, the best AWS-aligned answer is B, linear learner.

## Question 49

An ML engineer normalized training data by using min-max normalization in AWS Glue DataBrew. The ML engineer must normalize the production inference data in the same way as the training data before passing the production inference data to the model for predictions.
Which solution will meet this requirement?

- A. Apply statistics from a well-known dataset to normalize the production samples.
- B. Keep the min-max normalization statistics from the training set. Use these values to normalize the production samples.
- C. Calculate a new set of min-max normalization statistics from a batch of production samples. Use these values to normalize all the production samples.
- D. Calculate a new set of min-max normalization statistics from each production sample. Use these values to normalize all the production samples.

**Answer:** B

**Explanation:**
To ensure consistency between training and inference, the min-max normalization statistics (min and max values) calculated during training must be retained and applied to normalize production inference data. Using the same statistics ensures that the model receives data in the same scale and distribution as it did during training, avoiding discrepancies that could degrade model performance. Calculating new statistics from production data would lead to inconsistent normalization and affect predictions.

## Question 50

A company is developing ML models by using PyTorch and TensorFlow estimators with Amazon SageMaker AI. An ML engineer configures the SageMaker AI estimator and now needs to initiate a training job that uses a training dataset.
Which SageMaker AI SDK method can initiate the training job?

- A. fit method
- B. create_model method
- C. deploy method
- D. predict method

**Answer:** A

**Explanation:**
In the Amazon SageMaker Python SDK, the fit() method is used to start a training job after an estimator has been configured. AWS documentation explicitly states that once an estimator (such as PyTorch or TensorFlow) is defined with parameters like instance type, framework version, and hyperparameters, the fit() method is responsible for launching the training process.
The fit() method accepts the training data location (commonly an Amazon S3 URI) and initiates the managed training job on SageMaker infrastructure. SageMaker then provisions the required compute resources, stages the data, executes the training script, and stores model artifacts in Amazon S3.
The create_model() method is used after training to create a SageMaker model object from trained artifacts. The deploy() method deploys a trained model to an endpoint for inference. The predict() method is used only after deployment to request predictions from an endpoint.
AWS documentation clearly separates these lifecycle steps and identifies fit() as the correct method to initiate training.
Therefore, Option A is the correct and AWS-verified answer.

## Question 51

Case study
An ML engineer is developing a fraud detection model on AWS. The training dataset includes transaction logs, customer profiles, and tables from an on-premises MySQL database. The transaction logs and customer profiles are stored in Amazon S3.
The dataset has a class imbalance that affects the learning of the model ' s algorithm. Additionally, many of the features have interdependencies. The algorithm is not capturing all the desired underlying patterns in the data.
The training dataset includes categorical data and numerical data. The ML engineer must prepare the training dataset to maximize the accuracy of the model.
Which action will meet this requirement with the LEAST operational overhead?

- A. Use AWS Glue to transform the categorical data into numerical data.
- B. Use AWS Glue to transform the numerical data into categorical data.
- C. Use Amazon SageMaker Data Wrangler to transform the categorical data into numerical data.
- D. Use Amazon SageMaker Data Wrangler to transform the numerical data into categorical data.

**Answer:** C

**Explanation:**
Preparing a training dataset that includes both categorical and numerical data is essential for maximizing the accuracy of a machine learning model. Transforming categorical data into numerical format is a critical step, as most ML algorithms require numerical input.
Why Transform Categorical Data into Numerical Data?
Model Compatibility: Many ML algorithms cannot process categorical data directly and require numerical representations.
Improved Performance: Proper encoding of categorical variables can enhance model accuracy and convergence speed.
Why Use Amazon SageMaker Data Wrangler?
Amazon SageMaker Data Wrangler offers a visual interface with over 300 built-in data transformations, including tools for encoding categorical variables.
Implementation Steps:
Import Data:
Load the dataset into SageMaker Data Wrangler from sources like Amazon S3 or on-premises databases.
Identify Categorical Features:
Use Data Wrangler ' s data type inference to detect categorical columns.
Apply Categorical Encoding:
Choose appropriate encoding techniques (e.g., one-hot encoding or ordinal encoding) from Data Wrangler ' s transformation options.
Apply the selected transformation to convert categorical features into numerical format.
Validate Transformations:
Review the transformed dataset to ensure accuracy and completeness.
Advantages of Using SageMaker Data Wrangler:
Ease of Use: Provides a user-friendly interface for data transformation without extensive coding.
Operational Efficiency: Integrates data preparation steps, reducing the need for multiple tools and minimizing operational overhead.
Flexibility: Supports various data sources and transformation techniques, accommodating diverse datasets.
By utilizing SageMaker Data Wrangler to transform categorical data into numerical format, the ML engineer can efficiently prepare the dataset, thereby enhancing the model ' s accuracy with minimal operational overhead.
Transform Data - Amazon SageMaker
Prepare ML Data with Amazon SageMaker Data Wrangler

## Question 52

A company is using an Amazon Redshift database as its single data source. Some of the data is sensitive.
A data scientist needs to use some of the sensitive data from the database. An ML engineer must give the data scientist access to the data without transforming the source data and without storing anonymized data in the database.
Which solution will meet these requirements with the LEAST implementation effort?

- A. Configure dynamic data masking policies to control how sensitive data is shared with the data scientist at query time.
- B. Create a materialized view with masking logic on top of the database. Grant the necessary read permissions to the data scientist.
- C. Unload the Amazon Redshift data to Amazon S3. Use Amazon Athena to create schema-on-read with masking logic. Share the view with the data scientist.
- D. Unload the Amazon Redshift data to Amazon S3. Create an AWS Glue job to anonymize the data. Share the dataset with the data scientist.

**Answer:** A

**Explanation:**
Dynamic data masking allows you to control how sensitive data is presented to users at query time, without modifying or storing transformed versions of the source data. Amazon Redshift supports dynamic data masking, which can be implemented with minimal effort. This solution ensures that the data scientist can access the required information while sensitive data remains protected, meeting the requirements efficiently and with the least implementation effort.

## Question 53

An ML engineer is using Amazon SageMaker Canvas to build a custom ML model from an imported dataset. The model must make continuous numeric predictions based on 10 years of data.
Which metric should the ML engineer use to evaluate the model’s performance?

- A. Accuracy
- B. InferenceLatency
- C. Area Under the ROC Curve (AUC)
- D. Root Mean Square Error (RMSE)

**Answer:** D

**Explanation:**
This is a regression problem, where the target variable is continuous and numeric. AWS documentation clearly states that classification metrics such as accuracy and AUC are not appropriate for regression models.
Root Mean Square Error (RMSE) measures the square root of the average squared differences between predicted and actual values. RMSE penalizes larger errors more heavily, making it especially useful when large prediction errors are costly or undesirable.
SageMaker Canvas automatically selects regression metrics such as RMSE and MAE when building regression models. RMSE is widely used for time-based and numeric prediction problems, especially when evaluating long historical datasets.
Inference latency measures system performance, not model accuracy.
Therefore, Option D is the correct and AWS-verified answer.

## Question 54

A company has a Retrieval Augmented Generation (RAG) application that uses a vector database to store embeddings of documents. The company must migrate the application to AWS and must implement a solution that provides semantic search of text files. The company has already migrated the text repository to an Amazon S3 bucket.
Which solution will meet these requirements?

- A. Use an AWS Batch job to process the files and generate embeddings. Use AWS Glue to store the embeddings. Use SQL queries to perform the semantic searches.
- B. Use a custom Amazon SageMaker notebook to run a custom script to generate embeddings. Use SageMaker Feature Store to store the embeddings. Use SQL queries to perform the semantic searches.
- C. Use the Amazon Kendra S3 connector to ingest the documents from the S3 bucket into Amazon Kendra. Query Amazon Kendra to perform the semantic searches.
- D. Use an Amazon Textract asynchronous job to ingest the documents from the S3 bucket. Query Amazon Textract to perform the semantic searches.

**Answer:** C

**Explanation:**
Amazon Kendra is an AI-powered search service designed for semantic search use cases. It allows ingestion of documents from an Amazon S3 bucket using the Amazon Kendra S3 connector. Once the documents are ingested, Kendra enables semantic searches with its built-in capabilities, removing the need to manually generate embeddings or manage a vector database. This approach is efficient, requires minimal operational effort, and meets the requirements for a Retrieval Augmented Generation (RAG) application.

## Question 55

A company is using Amazon SageMaker AI to build an ML model to predict customer behavior. The company needs to explain the bias in the model to an auditor. The explanation must focus on demographic data of the customers.
Which solution will meet these requirements?

- A. Use SageMaker Clarify to generate a bias report. Send the report to the auditor.
- B. Use AWS Glue DataBrew to create a job to detect drift in the model ' s data quality. Send the job output to the auditor.
- C. Use Amazon QuickSight integration with SageMaker AI to generate a bias report. Send the report to the auditor.
- D. Use Amazon CloudWatch metrics from the SageMaker AI namespace to create a bias dashboard. Share the dashboard with the auditor.

**Answer:** A

**Explanation:**
AWS documentation identifies Amazon SageMaker Clarify as the primary service for detecting, measuring, and explaining bias in ML models, particularly across demographic and sensitive attributes such as age, gender, and location. Clarify can analyze bias before training, after training, and during inference, making it suitable for audit and compliance requirements.
SageMaker Clarify generates bias reports using established fairness metrics such as difference in positive proportions, disparate impact, and conditional demographic disparity. These reports are exportable and auditor-friendly, directly meeting the requirement to explain bias to an external party.
AWS Glue DataBrew focuses on data preparation and quality, not bias detection. Amazon QuickSight does not provide ML fairness metrics. Amazon CloudWatch captures operational metrics, not demographic bias indicators.
AWS best practices explicitly recommend SageMaker Clarify for model transparency, fairness evaluation, and regulatory reporting.
Therefore, Option A is the correct and AWS-verified solution.

## Question 56

A gaming company needs to deploy a natural language processing (NLP) model to moderate a chat forum in a game. The workload experiences heavy usage during evenings and weekends but minimal activity during other hours.
Which solution will meet these requirements MOST cost-effectively?

- A. Use an Amazon SageMaker AI batch transform job with fixed capacity.
- B. Use Amazon SageMaker Serverless Inference.
- C. Use a single Amazon EC2 GPU instance with reserved capacity.
- D. Use Amazon SageMaker Asynchronous Inference.

**Answer:** B

**Explanation:**
The key requirements in this scenario are variable traffic patterns and cost efficiency. The workload has unpredictable spikes during evenings and weekends, followed by long periods of low or no usage. According to AWS Machine Learning documentation, Amazon SageMaker Serverless Inference is specifically designed for such use cases.
SageMaker Serverless Inference automatically provisions, scales, and shuts down compute resources based on incoming inference requests. Customers are billed only for the compute time used during inference, not for idle resources. This makes it highly cost-effective for workloads with intermittent or spiky traffic, such as real-time chat moderation in gaming environments.
Option A is incorrect because batch transform jobs are intended for offline, large-scale inference and require fixed capacity during job execution. They are not suitable for real-time NLP moderation.
Option C is also incorrect because reserving an EC2 GPU instance incurs continuous costs regardless of utilization. This would be inefficient given the long idle periods described in the scenario.
Option D, SageMaker Asynchronous Inference, is designed for workloads with long processing times or large payloads and still requires endpoint provisioning. While it can handle traffic spikes, it does not scale down to zero in the same cost-efficient manner as Serverless Inference.
Therefore, Amazon SageMaker Serverless Inference is the most cost-effective and operationally efficient solution for deploying an NLP moderation model with highly variable usage patterns.

## Question 57

An ML engineer needs to organize a large set of text documents into topics. The ML engineer will not know what the topics are in advance. The ML engineer wants to use built-in algorithms or pre-trained models available through Amazon SageMaker AI to process the documents.
Which solution will meet these requirements?

- A. Use the BlazingText algorithm to identify the relevant text and to create a set of topics based on the documents.
- B. Use the Sequence-to-Sequence algorithm to summarize the text and to create a set of topics based on the documents.
- C. Use the Object2Vec algorithm to create embeddings and to create a set of topics based on the embeddings.
- D. Use the Latent Dirichlet Allocation (LDA) algorithm to process the documents and to create a set of topics based on the documents.

**Answer:** D

**Explanation:**
The task described is unsupervised topic modeling, where topics are unknown in advance. Latent Dirichlet Allocation (LDA) is a probabilistic generative model specifically designed to discover latent topics from a corpus of documents without labeled data. AWS provides LDA as a built-in algorithm in Amazon SageMaker, making it well suited for this requirement.
LDA models documents as mixtures of topics and topics as mixtures of words, enabling interpretable topic discovery at scale. This aligns precisely with the need to organize documents into topics when the topics are not predefined.
BlazingText is optimized for word embeddings and supervised text classification, not topic modeling. Sequence-to-sequence models are used for translation or summarization. Object2Vec creates embeddings but does not itself perform topic discovery without additional clustering steps.
Therefore, LDA is the correct and purpose-built solution.

## Question 58

A company stores training data as a .csv file in an Amazon S3 bucket. The company must encrypt the data and must control which applications have access to the encryption key.
Which solution will meet these requirements?

- A. Create a new SSH access key and use the AWS Encryption CLI to encrypt the file.
- B. Create a new API key by using Amazon API Gateway and use it to encrypt the file.
- C. Create a new IAM role with permissions for kms:GenerateDataKey and use the role to encrypt the file.
- D. Create a new AWS Key Management Service (AWS KMS) key and use the AWS Encryption CLI with the KMS key to encrypt the file.

**Answer:** D

**Explanation:**
AWS Key Management Service (AWS KMS) is the recommended service for encryption and key access control. By creating a customer-managed KMS key, the company can define granular IAM policies that control which applications and roles can use the key.
The AWS Encryption CLI integrates directly with KMS and enables client-side encryption of files before storing them in Amazon S3. This approach ensures data is encrypted at rest and that only authorized principals can decrypt it.
SSH keys and API keys are not designed for data encryption. IAM roles alone do not create or manage encryption keys—they only grant permissions.
AWS documentation explicitly states that KMS customer-managed keys provide centralized key management, auditing, and access control.
Therefore, Option D is the correct and AWS-aligned solution.

## Question 59

An ML engineer wants to re-train an XGBoost model at the end of each month. A data team prepares the training data. The training dataset is a few hundred megabytes in size. When the data is ready, the data team stores the data as a new file in an Amazon S3 bucket.
The ML engineer needs a solution to automate this pipeline. The solution must register the new model version in Amazon SageMaker Model Registry within 24 hours.
Which solution will meet these requirements?

- A. Create an AWS Lambda function that runs one time each week to poll the S3 bucket for new files. Invoke the Lambda function asynchronously. Configure the Lambda function to start the pipeline if the function detects new data.
- B. Create an Amazon CloudWatch rule that runs on a schedule to start the pipeline every 30 days.
- C. Create an S3 Lifecycle rule to start the pipeline every time a new object is uploaded to the S3 bucket.
- D. Create an Amazon EventBridge rule to start an AWS Step Functions TrainingStep every time a new object is uploaded to the S3 bucket.

**Answer:** D

**Explanation:**
The requirement is event-driven automation when new data arrives in Amazon S3, followed by training and model registration. Amazon EventBridge natively supports S3 object creation events and can trigger downstream workflows immediately.
By using EventBridge to start an AWS Step Functions workflow that includes a training step and a SageMaker Model Registry registration step, the pipeline runs automatically as soon as new data is uploaded—well within the 24-hour requirement.
Option A introduces unnecessary polling and delay. Option B is time-based and does not ensure alignment with data readiness. Option C is invalid because S3 Lifecycle rules manage object transitions, not workflow execution.
Therefore, EventBridge-triggered Step Functions is the correct solution.

## Question 60

An ML engineer is using Amazon Quick Suite (previously known as Amazon QuickSight) anomaly detection to detect very high or very low machine operating temperatures compared to normal. The ML engineer sets the Severity parameter to Low and above. The ML engineer sets the Direction parameter to All.
What effect will the ML engineer observe in the anomaly detection results if the ML engineer changes the Direction parameter to Lower than expected?

- A. Increased anomaly identification frequency and increased recall
- B. Decreased anomaly identification frequency and decreased recall
- C. Increased anomaly identification frequency and decreased recall
- D. Decreased anomaly identification frequency and increased recall

**Answer:** B

**Explanation:**
In Amazon QuickSight anomaly detection, the Direction parameter controls which deviations from the expected baseline are flagged as anomalies. When Direction is set to All, QuickSight detects both higher-than-expected and lower-than-expected values. This results in a larger set of detected anomalies, increasing recall.
When the Direction parameter is changed to Lower than expected, the anomaly detection algorithm filters out all higher-than-expected anomalies and only reports unusually low values. Because the scope of detection is narrowed, the total number of detected anomalies decreases. As a result, fewer true anomalies are identified overall, which reduces recall.
Since fewer data points are evaluated as potential anomalies, the frequency of anomaly identification also decreases. This behavior is consistent with AWS documentation, which explains that directional filtering limits detection to a subset of deviations and therefore reduces overall anomaly counts.
Therefore, changing the Direction from All to Lower than expected leads to decreased anomaly identification frequency and decreased recall.

## Question 61

A company has a conversational AI assistant that sends requests through Amazon Bedrock to an Anthropic Claude large language model (LLM). Users report that when they ask similar questions multiple times, they sometimes receive different answers. An ML engineer needs to improve the responses to be more consistent and less random.
Which solution will meet these requirements?

- A. Increase the temperature parameter and the top_k parameter.
- B. Increase the temperature parameter. Decrease the top_k parameter.
- C. Decrease the temperature parameter. Increase the top_k parameter.
- D. Decrease the temperature parameter and the top_k parameter.

**Answer:** D

**Explanation:**
The temperature parameter controls the randomness in the model ' s responses. Lowering the temperature makes the model produce more deterministic and consistent answers.
The top_k parameter limits the number of tokens considered for generating the next word. Reducing top_k further constrains the model ' s options, ensuring more predictable responses.
By decreasing both parameters, the responses become more focused and consistent, reducing variability in similar queries.

## Question 62

A company uses Amazon SageMaker AI to create ML models. The data scientists need fine-grained control of ML workflows, DAG visualization, experiment history, and model governance for auditing and compliance.
Which solution will meet these requirements?

- A. Use AWS CodePipeline with SageMaker Studio and SageMaker ML Lineage Tracking.
- B. Use AWS CodePipeline with SageMaker Experiments.
- C. Use SageMaker Pipelines with SageMaker Studio and SageMaker ML Lineage Tracking.
- D. Use SageMaker Pipelines with SageMaker Experiments.

**Answer:** C

**Explanation:**
Amazon SageMaker Pipelines provides native orchestration of ML workflows with fine-grained control, DAG-based visualization, and seamless integration with SageMaker Studio. AWS documentation explicitly states that Pipelines is designed for end-to-end ML workflow automation and visualization.
SageMaker ML Lineage Tracking records relationships between datasets, models, training jobs, and endpoints, enabling full auditability and governance, which is essential for compliance.
SageMaker Experiments tracks experiment metrics but does not provide lineage-level governance. CodePipeline is a general CI/CD service and lacks ML-specific DAG visualization and lineage tracking.
AWS best practices recommend combining SageMaker Pipelines + SageMaker Studio + ML Lineage Tracking for enterprise-grade ML workflow management.
Therefore, Option C is the correct and AWS-verified solution.

## Question 63

A company needs to update the model definition of an existing Amazon SageMaker Al endpoint.
Select and order the correct steps from the following list to update the model definition settings with the LEAST interruption of inferences. Select each step one time or not
at all. (Select and order THREE.)
Create a new endpoint configuration that uses the new model definition.
Create a new model definition with updated settings by using the CreateModel action in the SageMaker AI API.
Delete the endpoint that needs to be updated and recreate the endpoint with the new endpoint configuration.
Delete the IAM role and permissions for the ExecutionRoleArn parameter.
Update the endpoint with the new endpoint configuration.
Suggested Solution
Expert Solution
Answer
Answer:
Show Explanation
Explanation
Step 1: Create a new model definition with updated settings by using the CreateModel action in the SageMaker AI API.
Step 2: Create a new endpoint configuration that uses the new model definition.
Step 3: Update the endpoint with the new endpoint configuration.
Do not delete and recreate the endpoint. That causes unnecessary inference interruption. SageMaker endpoint updates are designed to use a new EndpointConfig; AWS explicitly states that to update an endpoint, you must create a new endpoint configuration, then call UpdateEndpoint on the existing endpoint. During the update, SageMaker changes the endpoint to Updating and then back to InService.


**Answer:** 

**Explanation:**
Step 1: Create a new model definition with updated settings by using the CreateModel action in the SageMaker AI API.
Step 2: Create a new endpoint configuration that uses the new model definition.
Step 3: Update the endpoint with the new endpoint configuration.
Do not delete and recreate the endpoint. That causes unnecessary inference interruption. SageMaker endpoint updates are designed to use a new EndpointConfig; AWS explicitly states that to update an endpoint, you must create a new endpoint configuration, then call UpdateEndpoint on the existing endpoint. During the update, SageMaker changes the endpoint to Updating and then back to InService.

## Question 64

An ML engineer needs to deploy ML models to get inferences from large datasets in an asynchronous manner. The ML engineer also needs to implement scheduled monitoring of data quality for the models and must receive alerts when changes in data quality occur.
Which solution will meet these requirements?

- A. Deploy the models by using scheduled AWS Glue jobs. Use Amazon CloudWatch alarms to monitor the data quality and send alerts.
- B. Deploy the models by using scheduled AWS Batch jobs. Use AWS CloudTrail to monitor the data quality and send alerts.
- C. Deploy the models by using Amazon ECS on AWS Fargate. Use Amazon EventBridge to monitor the data quality and send alerts.
- D. Deploy the models by using Amazon SageMaker AI batch transform. Use SageMaker Model Monitor to monitor the data quality and send alerts.

**Answer:** D

**Explanation:**
This requirement combines asynchronous inference on large datasets with automated data quality monitoring and alerting. AWS documentation explicitly recommends Amazon SageMaker batch transform for large-scale, asynchronous inference workloads. Batch transform jobs process large datasets stored in Amazon S3 without requiring a persistent endpoint, making them cost-effective and scalable.
For data quality monitoring, Amazon SageMaker Model Monitor is the AWS-native solution. Model Monitor can be scheduled to analyze inference data, compare it against a baseline, and detect data quality issues such as missing values, schema changes, or statistical drift. When violations occur, Model Monitor emits metrics to Amazon CloudWatch, where alarms can trigger alerts.
Options A, B, and C lack ML-aware data quality monitoring capabilities. AWS Glue and Batch are not designed for model data quality analysis, and CloudTrail tracks API activity—not data quality.
AWS best practices clearly position Batch Transform + Model Monitor as the correct architecture for asynchronous inference with automated monitoring and alerting.
Therefore, Option D is the correct and AWS-verified solution.

## Question 65

A company is using an AWS Lambda function to monitor the metrics from an ML model. An ML engineer needs to implement a solution to send an email message when the metrics breach a threshold.
Which solution will meet this requirement?

- A. Log the metrics from the Lambda function to AWS CloudTrail. Configure a CloudTrail trail to send the email message.
- B. Log the metrics from the Lambda function to Amazon CloudFront. Configure an Amazon CloudWatch alarm to send the email message.
- C. Log the metrics from the Lambda function to Amazon CloudWatch. Configure a CloudWatch alarm to send the email message.
- D. Log the metrics from the Lambda function to Amazon CloudWatch. Configure an Amazon CloudFront rule to send the email message.

**Answer:** D

**Explanation:**
Logging the metrics to Amazon CloudWatch allows the metrics to be tracked and monitored effectively.
CloudWatch Alarms can be configured to trigger when metrics breach a predefined threshold.
The alarm can be set to notify through Amazon Simple Notification Service (SNS), which can send email messages to the configured recipients.
This is the standard and most efficient way to achieve the desired functionality.

## Question 66

An ML engineer trained an ML model on Amazon SageMaker to detect automobile accidents from dosed-circuit TV footage. The ML engineer used SageMaker Data Wrangler to create a training dataset of images of accidents and non-accidents.
The model performed well during training and validation. However, the model is underperforming in production because of variations in the quality of the images from various cameras.
Which solution will improve the model ' s accuracy in the LEAST amount of time?

- A. Collect more images from all the cameras. Use Data Wrangler to prepare a new training dataset.
- B. Recreate the training dataset by using the Data Wrangler corrupt image transform. Specify the impulse noise option.
- C. Recreate the training dataset by using the Data Wrangler enhance image contrast transform. Specify the Gamma contrast option.
- D. Recreate the training dataset by using the Data Wrangler resize image transform. Crop all images to the same size.

**Answer:** B

**Explanation:**
The model is underperforming in production due to variations in image quality from different cameras. Using the corrupt image transform with the impulse noise option in SageMaker Data Wrangler simulates real-world noise and variations in the training dataset. This approach helps the model become more robust to inconsistencies in image quality, improving its accuracy in production without the need to collect and process new data, thereby saving time.

## Question 67

An ML engineer is analyzing a classification dataset before training a model in Amazon SageMaker AI. The ML engineer suspects that the dataset has a significant imbalance between class labels that could lead to biased model predictions. To confirm class imbalance, the ML engineer needs to select an appropriate pre-training bias metric.
Which metric will meet this requirement?

- A. Mean squared error (MSE)
- B. Difference in proportions of labels (DPL)
- C. Silhouette score
- D. Structural similarity index measure (SSIM)

**Answer:** B

**Explanation:**
In Amazon SageMaker AI, identifying bias in machine learning datasets before model training is a critical step to ensure fairness and reliability of predictions. This process is referred to as pre-training bias analysis, and it focuses on understanding whether the training data itself introduces bias—particularly through imbalanced class labels or sensitive attributes.
The Difference in Proportions of Labels (DPL) is a pre-training bias metric specifically designed to measure class imbalance. DPL compares the proportion of a specific label (such as a positive outcome) across different groups or classes within a dataset. If one class or group is overrepresented relative to another, the DPL value will deviate significantly from zero, clearly indicating imbalance. AWS documentation highlights DPL as a key metric used by SageMaker Clarify to detect label imbalance prior to model training.
By contrast, Mean Squared Error (MSE) is a regression evaluation metric used after model training to measure prediction error, not dataset bias. Silhouette score is an unsupervised learning metric used to evaluate clustering quality, making it irrelevant for supervised classification bias detection. Structural Similarity Index Measure (SSIM) is an image-quality metric used in computer vision tasks and has no application in dataset bias analysis.
Using DPL allows ML engineers to proactively detect and address skewed label distributions—such as by re-sampling, re-weighting, or collecting additional data—before training begins. This aligns with AWS best practices for responsible AI and helps reduce the risk of biased predictions that could negatively impact real-world decision-making.
Therefore, Difference in Proportions of Labels (DPL) is the correct and AWS-recommended metric for confirming class imbalance during pre-training bias analysis in Amazon SageMaker AI.

## Question 68

An ML engineer is developing a fraud detection model by using the Amazon SageMaker XGBoost algorithm. The model classifies transactions as either fraudulent or legitimate.
During testing, the model excels at identifying fraud in the training dataset. However, the model is inefficient at identifying fraud in new and unseen transactions.
What should the ML engineer do to improve the fraud detection for new transactions?

- A. Increase the learning rate.
- B. Remove some irrelevant features from the training dataset.
- C. Increase the value of the max_depth hyperparameter.
- D. Decrease the value of the max_depth hyperparameter.

**Answer:** D

**Explanation:**
A high max_depth value in XGBoost can lead to overfitting, where the model learns the training dataset too well but fails to generalize to new and unseen data. By decreasing the max_depth, the model becomes less complex, reducing overfitting and improving its ability to detect fraud in new transactions. This adjustment helps the model focus on general patterns rather than memorizing specific details in the training data.

## Question 69

A construction company is using Amazon SageMaker AI to train specialized custom object detection models to identify road damage. The company uses images from multiple cameras. The images are stored as JPEG objects in an Amazon S3 bucket.
The images need to be pre-processed by using computationally intensive computer vision techniques before the images can be used in the training job. The company needs to optimize data loading and pre-processing in the training job. The solution cannot affect model performance or increase compute or storage resources.
Which solution will meet these requirements?

- A. Use SageMaker AI file mode to load and process the images in batches.
- B. Reduce the batch size of the model and increase the number of pre-processing threads.
- C. Reduce the quality of the training images in the S3 bucket.
- D. Convert the images into RecordIO format and use the lazy loading pattern.

**Answer:** D

**Explanation:**
AWS documentation recommends using RecordIO format with lazy loading to optimize data input pipelines for image-based training workloads. RecordIO is a binary data format that enables sequential reads, reducing I/O overhead and improving throughput during training.
By converting JPEG images into RecordIO format, the training job can read data more efficiently from Amazon S3. Lazy loading ensures that only the required data is loaded into memory when needed, which optimizes CPU utilization during computationally intensive preprocessing steps.
Option A (file mode) results in many small S3 GET requests, which can become a bottleneck for large image datasets. Option B changes training behavior and can negatively affect convergence and performance. Option C reduces image quality, which directly impacts model accuracy and violates the requirement.
AWS SageMaker documentation explicitly highlights RecordIO and lazy loading as best practices for high-performance image training pipelines, especially when preprocessing is CPU-intensive.
Therefore, Option D is the correct and AWS-aligned solution.

## Question 70

An ML engineer needs to create data ingestion pipelines and ML model deployment pipelines on AWS. All the raw data is stored in Amazon S3 buckets.
Which solution will meet these requirements?

- A. Use Amazon Data Firehose to create the data ingestion pipelines. Use Amazon SageMaker Studio Classic to create the model deployment pipelines.
- B. Use AWS Glue to create the data ingestion pipelines. Use Amazon SageMaker Studio Classic to create the model deployment pipelines.
- C. Use Amazon Redshift ML to create the data ingestion pipelines. Use Amazon SageMaker Studio Classic to create the model deployment pipelines.
- D. Use Amazon Athena to create the data ingestion pipelines. Use an Amazon SageMaker notebook to create the model deployment pipelines.

**Answer:** B

**Explanation:**
AWS Glue is a serverless data integration service that is well-suited for creating data ingestion pipelines, especially when raw data is stored in Amazon S3. It can clean, transform, and catalog data, making it accessible for downstream ML tasks.
Amazon SageMaker Studio Classic provides a comprehensive environment for building, training, and deploying ML models. It includes built-in tools and capabilities to create efficient model deployment pipelines with minimal setup.
This combination ensures seamless integration of data ingestion and ML model deployment with minimal operational overhead.

## Question 71

A company uses Amazon Athena to query a dataset in Amazon S3. The dataset has a target variable that the company wants to predict.
The company needs to use the dataset in a solution to determine if a model can predict the target variable.
Which solution will provide this information with the LEAST development effort?

- A. Create a new model by using Amazon SageMaker Autopilot. Report the model ' s achieved performance.
- B. Implement custom scripts to perform data pre-processing, multiple linear regression, and performance evaluation. Run the scripts on Amazon EC2 instances.
- C. Configure Amazon Macie to analyze the dataset and to create a model. Report the model ' s achieved performance.
- D. Select a model from Amazon Bedrock. Tune the model with the data. Report the model ' s achieved performance.

**Answer:** A

**Explanation:**
The requirement is to quickly determine whether the target variable is predictable, with minimal development effort. Amazon SageMaker Autopilot is specifically designed for this purpose.
SageMaker Autopilot automatically handles data preprocessing, feature engineering, algorithm selection, model training, and evaluation. It generates multiple candidate models and provides detailed performance metrics, allowing teams to quickly assess predictability without writing custom code.
Option B requires significant manual development. Option C is incorrect because Amazon Macie is a data security and classification service, not an ML modeling service. Option D is unsuitable because Amazon Bedrock models are not intended for structured tabular prediction tasks.
Therefore, SageMaker Autopilot provides the fastest and least effort solution.

## Question 72

An ML engineer wants to deploy an Amazon SageMaker AI model for inference. The payload sizes are less than 3 MB. Processing time does not exceed 45 seconds. The traffic patterns will be irregular or unpredictable.
Which inference option will meet these requirements MOST cost-effectively?

- A. Asynchronous inference
- B. Real-time inference
- C. Serverless inference
- D. Batch transform

**Answer:** C

**Explanation:**
Amazon SageMaker Serverless Inference is designed for irregular or unpredictable traffic patterns. It automatically provisions and scales compute resources based on request volume and scales down to zero when idle, making it the most cost-effective option.
Serverless inference supports payloads up to 6 MB and request durations up to 60 seconds, which comfortably meets the stated constraints. Customers are billed only for actual compute usage during inference execution, not for idle capacity.
Asynchronous inference is intended for long-running jobs (up to 1 hour) and large payloads (up to 1 GB). Real-time inference requires always-on instances, increasing cost during idle periods. Batch transform is designed for offline processing.
Therefore, serverless inference is the optimal choice.
