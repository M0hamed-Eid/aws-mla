# Original Practice Question Bank for MLA-C01 Review


> **Note:** The following questions are original and written for training purposes according to the skills outlined in the MLA-C01 exam guide. They are not actual exam questions nor copied from the AWS exam or the Official Practice Question Set. Use the official set provided by the instructor for official AWS phrasing, and use this set to expand training and analyze decision-making.[1] [2]

## Classroom Usage Method

Display the question without the answer, and give students 60–120 seconds depending on its length. After voting, do not start by announcing the answer; first ask them to identify the **keyword** in the scenario, then the reason for eliminating each distractor. Each student records their mistake in a category: concept, service confusion, missed constraint, or time management.

# Questions Section

## Question 1 — Domain 1 — Multiple Choice

A company receives 20 TB of transaction logs daily and stores them in Amazon S3. Training jobs typically use only 8 columns out of 120, and the cost and time of reading must be reduced. What is the most appropriate change?

A. Store the data in CSV format with GZIP  
B. Store the data in JSON format without compression  
C. Convert the data to Parquet with appropriate partitioning  
D. Copy the data to Amazon EBS for each training job

## Question 2 — Domain 1 — Multiple Response: Select Two

A company wants to process a continuous clickstream, calculate aggregations in time windows, and then save the results in S3 for later training. The processing must operate in near real time with minimal management. Which two services are the most appropriate?

A. Amazon Kinesis Data Streams  
B. AWS Managed Service for Apache Flink  
C. AWS DataSync  
D. Amazon S3 Glacier  
E. AWS Batch

## Question 3 — Domain 1 — Multiple Choice

A data scientist wants to create transformations and features visually within the SageMaker environment, with minimal coding, and then reuse the flow in a processing job. What is the most appropriate tool?

A. AWS Glue Data Catalog  
B. SageMaker Data Wrangler  
C. Amazon CloudWatch Logs Insights  
D. SageMaker Model Monitor

## Question 4 — Domain 1 — Multiple Choice

An engineer split a dataset into training and validation after performing standardization using the mean and standard deviation calculated from the entire dataset. The validation results appeared unrealistically excellent. What is the most likely cause and the correction?

A. Underfitting; increase regularization  
B. Data leakage; split first then fit the transformations on the training data only  
C. Concept drift; run Model Monitor  
D. Class imbalance; increase batch size

## Question 5 — Domain 1 — Multiple Choice

An organization wants to measure pre-training bias in labels and compare the proportion of positive outcomes between two demographic groups. What is the most appropriate option?

A. SageMaker Clarify with Difference in Proportions of Labels  
B. SageMaker Debugger with tensor collections  
C. AWS CloudTrail Lake  
D. SageMaker Inference Recommender

## Question 6 — Domain 2 — Multiple Choice

A bank is building a fraud detection model. The cost of passing a true fraudulent transaction is much higher than the cost of mistakenly reviewing a legitimate transaction. Which metric should be given the highest priority?

A. Recall for the fraudulent class  
B. Precision for the fraudulent class  
C. Overall accuracy only  
D. RMSE

## Question 7 — Domain 2 — Multiple Response: Select Two

A neural network model achieves very high training accuracy, but the validation accuracy is much lower. What are the two most direct actions to mitigate the issue?

A. Add dropout or increase regularization  
B. Use early stopping  
C. Increase model complexity indefinitely  
D. Evaluate the model on the training set only  
E. Delete the validation set

## Question 8 — Domain 2 — Multiple Choice

A company wants to analyze sentiment and detect PII in natural language customer reviews, and does not want to train a custom model or manage infrastructure. What is the most appropriate service?

A. Amazon Comprehend  
B. Amazon Rekognition  
C. Amazon Transcribe  
D. SageMaker Ground Truth

## Question 9 — Domain 2 — Multiple Choice

Every training job is expensive, and you want a tuning process that uses the results of previous trials to select promising hyperparameters for subsequent trials. What is the most appropriate strategy?

A. Exhaustive grid search  
B. Bayesian optimization  
C. Randomly select fixed values once  
D. Increase the number of epochs only

## Question 10 — Domain 2 — Matching

Match each need with the most appropriate tool:

| Need | Options |
|---|---|
| 1. Explain feature impact and detect bias | A. SageMaker Model Registry |
| 2. Diagnose non-converging training | B. SageMaker Clarify |
| 3. Manage model versions and approval status | C. SageMaker Debugger |
| 4. Track runs, parameters, and artifacts | D. SageMaker Experiments |

## Question 11 — Domain 3 — Multiple Choice

An internal application sends infrequent and sporadic inference requests, and hours may pass without any requests. It can tolerate a slight cold start, and the company wants to pay only for usage with minimal management. What is the most appropriate option?

A. Always-on SageMaker real-time endpoint  
B. SageMaker Serverless Inference  
C. SageMaker Batch Transform every second  
D. GPU cluster on Amazon EKS

## Question 12 — Domain 3 — Multiple Choice

Each request sends a 300 MB file, and inference may take ten minutes. The client does not need an open connection or an immediate response, but requests must be queued and processed automatically. What is the most appropriate option?

A. SageMaker Asynchronous Inference  
B. SageMaker Serverless Inference  
C. API Gateway with synchronous Lambda for ten minutes  
D. Multi-model real-time endpoint

## Question 13 — Domain 3 — Ordering

Order the following stages of a logical MLOps pipeline:

A. Deploy approved model  
B. Register model version  
C. Build and test code  
D. Monitor production  
E. Train and evaluate model  
F. Approve model for production

## Question 14 — Domain 4 — Multiple Choice

The security team wants to know the identity of the principal who invoked DeleteEndpoint, the time, and the source address. What is the most appropriate service?

A. Amazon CloudWatch Metrics  
B. AWS CloudTrail  
C. SageMaker Model Monitor  
D. AWS X-Ray

## Question 15 — Domain 4 — Multiple Choice

The distribution of an input feature in production has changed compared to the baseline, while there are no immediate labels to measure accuracy. What is the most appropriate solution to detect the change?

A. SageMaker Model Monitor to monitor data quality/drift  
B. SageMaker Debugger on the production endpoint  
C. AWS CodeArtifact  
D. Amazon Inspector

## Question 16 — Domain 4 — Multiple Response: Select Three

A SageMaker training job must read sensitive data from S3 from within a private network, with least privilege and encryption using an organization-managed key. What are the appropriate actions?

A. Create an IAM execution role with specific permissions for the required bucket and prefix  
B. Use an AWS KMS key and appropriate key policies for encryption  
C. Use VPC configuration and an S3 VPC endpoint when appropriate for the architecture  
D. Place static access keys inside the training script  
E. Make the bucket public to avoid connectivity issues

## Question 17 — Domain 4 — Multiple Choice

A training job can tolerate interruption and resume work from a checkpoint, and the goal is to reduce compute cost. What is the best option?

A. SageMaker Managed Spot Training with checkpointing to S3  
B. Dedicated real-time endpoint  
C. Reserved DB instances  
D. Always run a larger On-Demand instance

## Question 18 — Integrated Case — Multiple Response: Select Three

A company wants a pipeline that retrains a model weekly, prevents deploying a model worse than the current one, and deploys the approved version gradually with automatic rollback upon increased errors. What are the most direct components?

A. SageMaker Pipelines to orchestrate processing/training/evaluation  
B. Condition step and Model Registry approval gate  
C. Canary or linear deployment with CloudWatch alarms and rollback  
D. Manually copy the model artifact to the developer's machine  
E. Delete the current model before evaluating the new one

# Answer Key and Explanations

## Answer 1 — C

**Keyword:** Reading only 8 columns out of 120 on a large volume. Parquet is a columnar format that allows reading the required columns and supports compression and partition pruning. Compressed CSV might reduce storage but does not provide the same columnar reading efficiency. EBS does not solve the format issue and adds copying and management.[1]

## Answer 2 — A and B

Kinesis Data Streams ingests the stream, and Managed Service for Apache Flink performs stateful windowed aggregations in a managed manner. DataSync is for batch transfer between storage systems, Glacier is for archiving, and Batch is not a near real time stream processing engine.[1]

## Answer 3 — B

Data Wrangler is dedicated to preparing data and feature engineering visually within SageMaker and reusing the flow. Data Catalog stores metadata, Logs Insights analyzes logs, and Model Monitor monitors data and predictions in production.[1]

## Answer 4 — B

Calculating statistics from the entire dataset allows validation information to enter preprocessing, which is **data leakage**. Splitting must precede fitting the transformations; parameters are calculated on the training data and then applied to validation/test.[1]

## Answer 5 — A

Clarify provides pre-training bias metrics, including Difference in Proportions of Labels mentioned in the exam guide. Debugger is for training and convergence, CloudTrail is for audit, and Inference Recommender is for selecting inference configurations.[1]

## Answer 6 — A

Passing a fraudulent transaction is a False Negative, so increasing recall for the fraudulent class reduces these cases. Precision should also be monitored because increasing recall might increase false alarms, but the constraint in the question gives recall priority. Accuracy can be misleading with class imbalance, and RMSE is for regression.[1]

## Answer 7 — A and B

The gap between training and validation indicates overfitting. Regularization/dropout reduce over-reliance on training data, and early stopping halts training before continued degradation on validation. The other options exacerbate the problem or eliminate proper measurement.[1]

## Answer 8 — A

Comprehend is a managed NLP service that supports sentiment and entity/PII-related capabilities without training custom infrastructure. Rekognition is for images and video, Transcribe converts speech to text, and Ground Truth is for labeling.[1]

## Answer 9 — B

Bayesian optimization builds an understanding from previous jobs' results to guide subsequent trials, which is suitable when the trial is expensive. Grid search might waste many jobs, selecting once is not tuning, and increasing epochs does not search the hyperparameters space.[1]

## Answer 10 — 1-B, 2-C, 3-A, 4-D

Clarify is for explainability and bias, Debugger is for diagnosing training/convergence, Model Registry is for versioning and approval, and Experiments is for tracking runs, parameters, and artifacts. This comparison is highly valuable because the tool names are similar but their questions are different.[1]

## Answer 11 — B

Serverless Inference suits sporadic traffic when a cold start is acceptable, and reduces the management and cost of idle capacity. A real-time endpoint is better when consistent and low latency is required, Batch Transform is for datasets not interactive requests, and EKS increases management unnecessarily.[1]

## Answer 12 — A

Asynchronous Inference is designed for large payloads and long processing times with a queue and background processing. Serverless has limits and a different use case, synchronous Lambda is not suitable for ten minutes behind an API in the proposed manner, and multi-model does not solve the request length or payload.[1]

## Answer 13 — C → E → B → F → A → D

The code is built and tested, then the model is trained and evaluated, then the version is registered, and after passing the gate it gets approval, then it is deployed and production is monitored. The exact services may vary, but separating evaluation, approval, and monitoring is the core of an auditable pipeline.[1]

## Answer 14 — B

CloudTrail records API activity, identity, time, and request data suitable for auditing. CloudWatch is for metrics and operational logs, Model Monitor is for ML monitoring, and X-Ray is for tracing latency across distributed requests.[1]

## Answer 15 — A

Model Monitor can compare production data to the baseline and detect data quality violations or drift even before labels are available. Debugger is a training tool, and the other options do not monitor the distribution of features.[1]

## Answer 16 — A, B, and C

A scoped IAM role achieves least privilege, KMS achieves encryption with a managed key, and VPC configuration with an S3 endpoint helps with private access depending on the architecture. Static keys are a security risk, and making the bucket public violates the requirements.[1]

## Answer 17 — A

Managed Spot Training reduces the cost of interruptible training, and checkpointing to S3 allows resumption. The rest of the options do not apply the scenario's characteristics or might increase the cost.[1]

## Answer 18 — A, B, and C

Pipelines orchestrates the training cycle, a Condition step prevents promotion when a metric threshold fails while Model Registry manages approval, then canary/linear deployment with CloudWatch alarms allows gradual rollout and rollback. Manual work and deleting production before evaluation conflict with reliability and MLOps.[1]

# Coverage Matrix

| Domain | Questions | Count |
|---|---|---:|
| Domain 1 | 1–5 | 5 |
| Domain 2 | 6–10 | 5 |
| Domain 3 | 11–13 | 3 |
| Domain 4 | 14–17 | 4 |
| Integrated | 18 | 1 |

## References

[1]: https://d1.awsstatic.com/training-and-certification/docs-machine-learning-engineer-associate/AWS-Certified-Machine-Learning-Engineer-Associate_Exam-Guide.pdf "AWS Certified Machine Learning Engineer – Associate (MLA-C01) Exam Guide"
[2]: https://skillbuilder.aws/learn/H9QT54A6FP/official-practice-question-set-aws-certified-machine-learning-engineer--associate-mlac01--english/S32HWF3JVF "Official Practice Question Set: AWS Certified Machine Learning Engineer – Associate"
