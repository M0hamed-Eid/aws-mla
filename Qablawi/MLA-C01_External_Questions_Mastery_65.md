# Mastery Exam Prep - AWS MLA-C01 Free Practice Exam (65 Questions)

Source: https://masteryexamprep.com/exams/aws/mla-c01/free-practice-exam/

---

## Question 1

**Topic:** Data Preparation for Machine Learning (ML)

In an Amazon SageMaker training workflow, you apply feature transformations such as normalization, log transforms, and binning. Which statement is
INCORRECT
(unsafe) about applying these transformations to improve model performance and stability?

- A. Use a log transform to reduce extreme right-skew in a feature.
- B. Fit the scaler on training data and reuse it for inference.
- C. Bin a continuous feature to reduce outlier sensitivity.
- D. Compute scaling statistics using all splits to ensure consistency.

**Best answer: D**

**Explanation:**
Scaling parameters (for example, mean and standard deviation) must be learned only from the training set and then applied unchanged to validation/test and inference. Computing them using all splits introduces data leakage, which can make offline evaluation look better than it will in production and can destabilize retraining comparisons. The other statements describe standard, generally safe transformations and their typical benefits.
The core issue is data leakage: any transformation that learns parameters from data (normalization, standardization, target encoding, learned embeddings, etc.) must be fit on the training split only. If you compute scaling statistics using validation/test data, the model indirectly “sees” distribution information it would not have at training time, which can inflate evaluation results and make performance look less stable across retrains.
A safe SageMaker pattern is:
Split data (or define splits)
Fit transformers on the training split
Persist transformer artifacts (for example, in S3/Model package)
Apply the same transformer to validation/test and to the inference input path
Log transforms often stabilize skewed features, and binning can reduce sensitivity to outliers (with a potential loss of granularity).
Train-only fitting
is the correct practice to avoid leakage and keep training/inference consistent.
Log transform skew
is commonly used to reduce skew and improve stability for some models.
Binning for robustness
can reduce outlier impact and help simple models capture nonlinear effects.
All-splits statistics
is unsafe because it contaminates evaluation with test/validation information.


## Question 2

**Topic:** Deployment and Orchestration of ML Workflows

A company trained a PyTorch transformer for real-time text classification in Amazon SageMaker. The inference code requires a proprietary, compiled tokenizer library and OS packages that cannot be installed at startup.
Deployment requirements:
p95 latency -150 ms at ~100 requests/second with autoscaling
Endpoint must run in a private VPC with
no outbound internet
Container image must come from a private registry and be auditable
Which deployment approach BEST meets these requirements?

- A. Run the model on Amazon EKS using a Kubernetes Deployment
- B. Build a custom container, push to ECR, deploy to SageMaker endpoint
- C. Use SageMaker Batch Transform with the built-in PyTorch container
- D. Use the SageMaker Hugging Face container with pip installs at startup

**Best answer: B**

**Explanation:**
Because the endpoint has no outbound internet and needs proprietary compiled dependencies, the inference environment must be fully self-contained. Packaging the required OS libraries and tokenizer into a custom Docker image in Amazon ECR satisfies the private, auditable image requirement. That image can then be deployed to an autoscaling SageMaker real-time endpoint inside the VPC to meet the latency SLA.
The core decision is whether a SageMaker provided container can satisfy the runtime requirements without modification. When inference needs proprietary binaries, custom system packages, or nonstandard runtime components that cannot be safely installed at startup (especially with no outbound internet), you should use a bring-your-own container (BYOC).
In this scenario, a custom image built with the compiled tokenizer and required OS packages can be stored in private Amazon ECR for auditability and deployed as a SageMaker real-time endpoint with VPC configuration and autoscaling to meet the p95 latency and throughput requirements. SageMaker provided deep learning containers are best when your dependencies can be expressed through supported mechanisms (and network access/policy allows installation).
Startup pip installs
conflicts with the no-outbound-internet requirement and proprietary binary dependency.
Batch Transform
is not intended for low-latency, always-on real-time SLAs.
EKS deployment
can work technically but adds operational overhead and is not the best fit for the stated preference for an AWS-native managed endpoint.


## Question 3

**Topic:** ML Model Development

A team runs a PyTorch training job on Amazon SageMaker using instance type
ml.g4dn.xlarge
(the instance type cannot change due to budget controls). The job fails with different errors across retries.
Exhibit: CloudWatch log excerpts
RuntimeError: CUDA out of memory. Tried to allocate 512.00 MiB
pandas.errors.ParserError: Error tokenizing data. C error: Expected 12 fields
WARNING: loss is NaN at step 430
Which THREE actions are the most appropriate mitigations? (Select THREE.)

- A. Switch to an instance type with more GPU memory
- B. Enable SageMaker Managed Spot Training
- C. Add a preprocessing validation/cleaning step before training
- D. Increase the training job EBS volume size
- E. Reduce batch size or use gradient accumulation
- F. Lower the learning rate and add gradient clipping

**Correct answers: C, E and F**

**Explanation:**
The errors indicate three distinct failure modes: GPU out-of-memory, input parsing errors, and unstable optimization (NaN loss). The best mitigations are to reduce GPU memory pressure in the training loop, validate/clean input data before it reaches the training container, and adjust optimization settings to improve numerical stability and convergence.
Troubleshooting training failures in SageMaker starts by mapping the error to the resource or pipeline stage that causes it. CUDA OOM is almost always driven by model size and per-step memory (batch size/sequence length/activations), so you mitigate by reducing memory use in the training code rather than changing unrelated storage settings. Parser errors indicate malformed or inconsistent input (schema/field count/quoting), which is best handled by adding an explicit validation/cleaning step (for example, a SageMaker Processing job or AWS Glue job) that produces a known-good dataset for training. NaN loss typically indicates divergence or numeric instability, so mitigation is to adjust optimizer hyperparameters (lower learning rate) and add stabilizers such as gradient clipping.
Key takeaway: pick mitigations that directly address the failure signal (GPU memory, data validity, optimization stability) and fit stated constraints.
OK (GPU OOM)
Reducing batch size or using gradient accumulation lowers GPU memory per step.
OK (Input errors)
A preprocessing validation/cleaning step prevents malformed rows from crashing the loader.
OK (Convergence)
Lower learning rate and gradient clipping help avoid divergence that yields NaNs.
NO (Wrong resource)
Increasing EBS volume affects disk capacity, not GPU memory or CSV parsing.
NO (Violates constraint)
Switching to a larger GPU instance conflicts with the fixed instance-type budget control.
NO (Different goal)
Managed Spot Training reduces cost but does not resolve OOM, parsing, or NaN loss.


## Question 4

**Topic:** Data Preparation for Machine Learning (ML)

A company must ensure that training datasets containing PII are stored and processed only in
eu-west-1
. Amazon S3 is used for raw storage, and Amazon SageMaker processing jobs are used for feature engineering. Which action best reflects a core principle to keep data confined to the approved Region and approved storage locations?

- A. Create CloudTrail alarms for S3 and SageMaker in other Regions.
- B. Remove PII columns before storing data for feature engineering.
- C. Encrypt S3 data with KMS keys created in
- eu-west-1
- .
- D. Use SCP region-deny and allow-list approved S3 buckets.

**Best answer: D**

**Explanation:**
The best fit is the principle of least privilege: explicitly allow only the approved Region and the specific S3 locations that may be used. An AWS Organizations SCP can prevent using services in disallowed Regions, and an allow-list policy limits where identities can read/write data, reducing the chance of accidental noncompliant storage or processing.
The core principle is least privilege: identities should be permitted to operate only within the minimum set of Regions and storage locations required for the workflow. To enforce data residency, use preventive guardrails that block actions rather than only detecting them after the fact.
Practically, this means:
Use an AWS Organizations SCP to deny API calls when
aws:RequestedRegion
is not
eu-west-1
(for Regional services such as SageMaker).
Use IAM and/or S3 bucket policies to allow access only to the approved S3 buckets/prefixes used for the ML pipeline.
This combination reduces the permitted “blast radius” so data cannot be easily processed or written outside approved boundaries, which is stronger than monitoring-only or encryption-only approaches.
Monitoring-only
alarms can detect noncompliant activity but do not prevent it.
Encryption-only
with KMS does not stop copying or processing data in other Regions.
Data minimization
(dropping PII) reduces sensitivity but does not enforce residency controls.


## Question 5

**Topic:** Deployment and Orchestration of ML Workflows

A team deploys the same Amazon SageMaker inference container image to dev, test, and prod. They avoid baking environment-specific values into the image.
At deployment time, they pass non-sensitive settings (for example, log level and downstream endpoint URL) as environment variables and retrieve sensitive values (for example, an API key) from AWS Secrets Manager using the endpoint’s IAM role.
Which core principle does this action most directly reflect?

- A. Least privilege
- B. Reproducibility
- C. Separation of duties
- D. Defense in depth

**Best answer: B**

**Explanation:**
This approach externalizes configuration from the built artifact so the same container image can be promoted across environments without modification. Environment-specific values are supplied at deploy time through environment variables and managed secrets, which reduces configuration drift between dev, test, and prod. That directly supports reproducible, consistent deployments.
The core concept is reproducibility: build once, deploy many. When you keep the container image identical across dev/test/prod and inject environment-specific settings at deployment time, you can reliably reproduce the same workload behavior in each environment and promote releases without rebuilding.
In AWS ML deployments, this is typically achieved by:
Passing non-secret configuration as environment variables (or parameter references).
Storing secrets in Secrets Manager (or SSM Parameter Store SecureString) and retrieving them at runtime with the endpoint’s IAM role.
This pattern also improves security, but the primary principle demonstrated is that deployments remain consistent and repeatable because configuration is separated from the artifact.
Least privilege
is about scoping IAM permissions, not primarily about externalizing configuration for consistent promotion.
Defense in depth
focuses on layered controls (network, encryption, detection) rather than repeatable build/promote mechanics.
Separation of duties
is about splitting responsibilities (for example, deploy vs approve) rather than how configuration values are supplied.


## Question 6

**Topic:** Data Preparation for Machine Learning (ML)

Which statement is
INCORRECT
about using Amazon SageMaker Data Wrangler to create, validate, and export a repeatable feature engineering workflow?

- A. After exporting to a Processing job, you can delete the
- .flow
- file because the exported job contains the full transformation graph.
- B. Export the flow to a SageMaker Processing job or SageMaker Pipelines step and parameterize the input and output S3 URIs for repeatable runs.
- C. Store the Data Wrangler
- .flow
- file in Amazon S3 with versioning to track transformation changes over time.
- D. Use Data Wrangler’s data quality/insights to validate the dataset and save the report artifacts to Amazon S3 for auditability.

**Best answer: A**

**Explanation:**
A repeatable Data Wrangler workflow depends on persisting the flow definition (the
.flow
file) as the source of truth for the transformation graph. Exports (Processing job/Pipelines) use that flow to apply the same steps on new data, and validation artifacts can be saved for traceability. Deleting the flow undermines reruns and auditability.
In SageMaker Data Wrangler, the
.flow
file is the durable, declarative definition of your feature engineering steps (imports, transforms, joins, encodings, etc.). When you export a Data Wrangler workflow (for example, to a SageMaker Processing job or a SageMaker Pipelines step), the runtime uses the flow definition to reproduce the same transformations on future datasets.
To make the workflow repeatable and auditable:
Version and retain the
.flow
file (for example, in S3 with versioning).
Use Data Wrangler’s data quality/insights to validate assumptions and store the generated artifacts.
Parameterize inputs/outputs (such as S3 URIs) so the same flow runs across different partitions or dates.
Keeping the flow artifact is a core requirement for reproducibility; export alone is not a substitute for the flow definition.
Keep flow as source of truth
is valid because versioned
.flow
captures the exact transform graph.
Validate and persist reports
is valid because saved artifacts support repeatable checks and audits.
Export with parameterized S3 URIs
is valid because it enables rerunning the same transforms on new data.
Delete the
.flow
after export
is unsafe because the export relies on the flow definition to reproduce transforms.


## Question 7

**Topic:** ML Solution Monitoring, Maintenance, and Security

A team uses AWS CodePipeline and CodeBuild to train a model and then promote it in Amazon SageMaker Model Registry before deploying to production. A security review captured this IAM policy fragment attached to the CodeBuild project role.
Exhibit: CodeBuild role policy (partial)
1 Effect: Allow
2 Action: secretsmanager:GetSecretValue
3 Resource: arn:aws:secretsmanager:us-east-1:111122223333:secret:prod/db*
4 Action: sagemaker:UpdateModelPackage
5 Resource: *
6 Action: sts:AssumeRole
7 Resource: arn:aws:iam::111122223333:role/ProdDeployRole
Which action is the best next step to secure the ML CI/CD pipeline?

- A. Move the production secret into S3 with SSE-KMS
- B. Split build and promotion roles; least-privilege both
- C. Encrypt the Secrets Manager secret with a new KMS key
- D. Enable CloudTrail data events for Secrets Manager

**Best answer: B**

**Explanation:**
The exhibit shows the CodeBuild role can fetch a production database secret and can directly promote a model package and assume a production deploy role. The best security improvement is to separate duties: keep the build role restricted to building artifacts and use a tightly scoped, gated promotion/deploy role for production changes.
Securing ML CI/CD pipelines relies on least privilege, separation of duties, and controlled promotion of artifacts. In the exhibit, the same CodeBuild role can retrieve a production secret (line 2 with a prod secret ARN on line 3) and can also promote artifacts and reach production permissions (it can call
sagemaker:UpdateModelPackage
on
*
in lines 4–5 and can
sts:AssumeRole
into
ProdDeployRole
in lines 6–7).
A safer approach is to:
Limit the build role to training/build steps and non-prod resources.
Remove access to production secrets from the build role.
Perform model approval/promotion and production deployment using a separate, tightly scoped role (often behind a manual approval or controlled pipeline stage).
Key takeaway: the risky capability is explicitly visible in lines 2–7, so the fix is to restrict roles and promotion permissions, not just add encryption or logging.
KMS key rotation only
doesn’t remove the build role’s ability to read the prod secret shown in lines 2–3.
Putting secrets in S3
is not an improvement over Secrets Manager and still wouldn’t address the overbroad promotion/deploy permissions in lines 4–7.
More logging
can help detect misuse but does not prevent the build role from using the permissions shown in the exhibit.


## Question 8

**Topic:** ML Solution Monitoring, Maintenance, and Security

A company in a regulated industry must run Amazon SageMaker training jobs and a real-time endpoint in a VPC with
no internet access
(no direct or indirect egress). Training data is in Amazon S3, and custom containers are stored in Amazon ECR. Logs must go to Amazon CloudWatch.
Which approach is
NOT
appropriate to meet the network isolation requirement?

- A. Run training and the endpoint in private subnets with no route to an internet gateway
- B. Use VPC endpoints for S3, ECR, and CloudWatch so jobs can access required services privately
- C. Place jobs in a public subnet and use a NAT gateway so the container can download packages from the internet at startup
- D. Enable SageMaker network isolation and ensure all dependencies are packaged in the container or retrieved from S3 via VPC endpoints

**Best answer: C**

**Explanation:**
When policies require no internet access, SageMaker training and inference must run in private subnets without internet routes and rely on private connectivity (VPC endpoints) for AWS service access. Allowing internet egress (even via NAT) breaks network isolation because the container can reach external networks.
The core principle is enforcing network isolation by preventing any internet egress path for training and inference while still allowing access to required AWS services over private networking. In SageMaker, this typically means placing jobs/endpoints in private subnets, removing routes to an internet gateway/NAT gateway, and using VPC endpoints to reach services such as S3 (Gateway endpoint), ECR (Interface endpoints), and CloudWatch Logs (Interface endpoint). If additional packages or artifacts are needed, they must be pre-packaged into the container image or stored in S3 and accessed through those endpoints.
Allowing a NAT gateway so containers can fetch dependencies at runtime is an anti-pattern because it reintroduces outbound internet connectivity.
Private subnets
supports isolation when there is no IGW/NAT route.
VPC endpoints
provide private access to S3/ECR/CloudWatch without internet egress.
Packaged dependencies
avoids needing public package repositories during startup.


## Question 9

**Topic:** ML Model Development

A data scientist trains two binary classifiers in Amazon SageMaker and evaluates them using ROC AUC on the same held-out test set. Model A has an AUC of 0.91, and Model B has an AUC of 0.87. Which statement is the most accurate interpretation of this result?

- A. Model A has better overall class-separation across thresholds
- B. Model A will have higher precision for the positive class
- C. Model A will have higher accuracy at any chosen threshold
- D. Model A has a lower false positive rate at every recall level

**Best answer: A**

**Explanation:**
ROC AUC summarizes a model’s ability to discriminate between classes across all possible classification thresholds. Because Model A’s AUC is higher than Model B’s on the same test set, Model A is the better overall discriminator in a threshold-independent sense. This does not guarantee it will be best at a specific operating threshold or metric like precision.
ROC curves plot true positive rate against false positive rate as the decision threshold varies, and AUC compresses that curve into a single, threshold-independent number. Interpreting AUC at a high level: it reflects how well the model separates (ranks) positive examples above negative examples across all thresholds. Therefore, when two models are evaluated on the same dataset, the model with the higher AUC (0.91 vs. 0.87) has better overall discriminative performance.
AUC alone does not guarantee better performance at any particular threshold, nor does it directly determine threshold-dependent metrics such as accuracy, precision, or recall; those depend on the chosen operating point and class prevalence. The key takeaway is that higher AUC means better overall ranking/separation, not universal superiority for every threshold or metric.
Any-threshold accuracy
is incorrect because accuracy depends on a specific threshold and class balance.
Precision guarantee
is incorrect because precision is threshold- and prevalence-dependent and is better assessed with a PR curve.
Dominates ROC everywhere
is incorrect because a higher AUC does not require one ROC curve to be better at every point.


## Question 10

**Topic:** ML Model Development

A team needs to extract printed text from scanned loan applications (OCR). The team does not need custom fields beyond standard form and table extraction. Which action best applies the core principle of
data minimization
while meeting the requirement on AWS?

- A. Use Amazon Textract and store only extracted text outputs
- B. Use Amazon Rekognition Custom Labels for text extraction
- C. Run open-source OCR on EC2 and archive all raw images
- D. Train a custom OCR model in SageMaker and keep all scans

**Best answer: A**

**Explanation:**
Amazon Textract is the AWS-managed service purpose-built for OCR on scanned documents, forms, and tables, so custom modeling is unnecessary here. Data minimization means collecting and retaining only what is needed for the stated purpose. Storing only the extracted text (and not the source scans) best matches that principle.
The key decision is matching the computer vision need to the appropriate managed service and then applying the stated principle. For OCR of scanned documents (including forms and tables) where specialized customization is not required, Amazon Textract is the high-level, fit-for-purpose choice.
Data minimization means limiting both the data you collect/process and the data you retain. In this scenario, the business output is the extracted text, so minimizing retained data favors persisting the Textract results while avoiding unnecessary long-term storage of the original scanned images.
By contrast, custom modeling (for example, training an OCR model in SageMaker) is warranted when managed OCR does not meet accuracy, language/layout, or domain-specific extraction requirements.
Custom training
is unnecessary overhead and retains extra sensitive raw data.
Wrong service
: Rekognition Custom Labels targets image classification/object detection, not document OCR.
Self-managed OCR
can work, but archiving all raw images violates minimization under the stated need.


## Question 11

**Topic:** Data Preparation for Machine Learning (ML)

A team has existing producers and consumers that use the Apache Kafka protocol and client libraries. The team wants a fully managed AWS service to ingest a high-throughput stream with minimal application changes while retaining Kafka concepts such as topics and partitions. Which AWS service should the team use?

- A. Amazon Managed Streaming for Apache Kafka (Amazon MSK)
- B. Amazon Kinesis Data Streams
- C. Amazon Kinesis Data Firehose
- D. Amazon SageMaker Feature Store

**Best answer: A**

**Explanation:**
Amazon MSK is AWS’s fully managed service for running Apache Kafka, allowing teams to keep Kafka APIs and client libraries while ingesting streaming data at scale. This directly matches the requirement to preserve Kafka-specific constructs like topics and partitions with minimal application changes.
Use Amazon Managed Streaming for Apache Kafka (Amazon MSK) when you need a managed Kafka control plane and data plane but want to keep the Kafka protocol, client libraries, and operational model (topics, partitions, consumer groups). This is a common choice for streaming ingestion when teams already have Kafka-based applications and want to reduce operational burden (broker management, patching, scaling) without rewriting producers/consumers.
A close alternative is Amazon Kinesis Data Streams, which is also for streaming ingestion at scale, but it uses Kinesis APIs and shard-based scaling rather than Kafka APIs and partitions, so it typically requires application changes.
Kinesis vs Kafka compatibility
Kinesis Data Streams ingests streams but is not Kafka-protocol compatible.
Delivery service confusion
Kinesis Data Firehose is primarily for loading streams into destinations (like S3/Redshift) with limited custom consumer patterns.
Feature management, not ingestion
SageMaker Feature Store stores and serves features; it is not a streaming ingest service.


## Question 12

**Topic:** ML Model Development

A team runs multiple Amazon SageMaker training jobs while tuning hyperparameters and wants each run to be reproducible and comparable later. They need a managed way to record randomness-related settings, track parameters and metrics, and version the run’s inputs and produced artifacts (for example, S3 URIs for datasets, training code, and model artifacts).
Which SageMaker capability best meets this requirement?

- A. Amazon CloudWatch Logs
- B. Amazon SageMaker Model Registry
- C. Amazon SageMaker Experiments
- D. AWS CloudTrail

**Best answer: C**

**Explanation:**
Amazon SageMaker Experiments is designed to organize and track ML runs so they can be reproduced and compared. It captures run metadata such as parameters (including seeds), metrics, and references to inputs and outputs (like dataset and model artifact locations). This makes it the most direct managed feature for reproducible experimentation in SageMaker.
Reproducible ML experiments require consistent capture of what was run and what data/artifacts were used: training parameters (including any random seeds), evaluation metrics, and the specific input and output artifacts for each run. Amazon SageMaker Experiments provides this experiment tracking by grouping runs into trials and recording each run’s parameters, metrics, and artifact references (for example, S3 locations for datasets, code bundles, and model artifacts). This lets you reliably compare results across runs and re-run the same configuration later using the recorded metadata.
Operational logs and API audit trails can support debugging and governance, but they are not purpose-built to structure experiment metadata for repeatable model development.
Operational logging
via CloudWatch can store stdout/stderr, but it doesn’t structure experiment parameters and artifacts for comparison.
Model lifecycle control
with Model Registry focuses on approved model versions and deployments, not tracking every training run.
API auditing
with CloudTrail records SageMaker API calls, but not the curated experiment metadata needed for reproducibility.


## Question 13

**Topic:** Data Preparation for Machine Learning (ML)

A team is preparing a 5 TB image dataset in Amazon S3 for distributed training on Amazon SageMaker. The team wants to maximize training throughput and minimize data loading bottlenecks.
Which statement is
INCORRECT
about preparing the training data so it can be loaded efficiently by the training infrastructure?

- A. Compress files when the training job is not CPU-bound on decompression
- B. Upload a single large file per epoch to reduce S3 GET overhead
- C. Store many small objects to maximize S3 request parallelism
- D. Shard the dataset into multiple medium-size files per channel

**Best answer: B**

**Explanation:**
Efficient SageMaker training input typically comes from balanced sharding: enough files to allow parallel reads without creating pathological tiny-object overhead. A single very large file forces serial or limited parallel reads and makes retries and partial re-reads expensive, which can reduce overall training throughput.
The core concept is to shape S3 training data for high-throughput, parallel ingestion by the training cluster. For distributed training, you generally want multiple reasonably sized shards (often tens to hundreds of MB up to a few GB depending on the pipeline) so each worker can read different files concurrently; one monolithic file tends to cap parallelism and can become a hotspot.
Compression can improve effective I/O and reduce network transfer, but only helps when decompression CPU cost does not become the bottleneck. Conversely, extremely small files can overwhelm the input pipeline with per-object overhead (listing/opens/metadata and request management), even if S3 can scale requests.
Key takeaway: optimize for parallel reads with sensible shard sizes, not single giant objects.
Many small objects
can increase parallelism but risks tiny-file overhead; it is not inherently unsafe.
Sharding into medium files
is a standard way to improve parallel reads during distributed training.
Compression when CPU allows
can reduce I/O and speed up input when decompression isn’t the limiting factor.
Single huge file
is the problematic pattern because it limits parallel ingestion and makes retries costly.


## Question 14

**Topic:** ML Solution Monitoring, Maintenance, and Security

An ML team stores training datasets in one S3 bucket using prefixes
raw/
(contains PII) and
curated/
(PII removed). Data scientists must be able to list and read objects only under
curated/
and must not access
raw/
. Which action best reflects the core security principle of least privilege?

- A. Use separate IAM roles for ETL and model training jobs
- B. Scope S3 permissions to only the
- curated/
- prefix
- C. Automatically delete datasets after every training run
- D. Enable S3 Versioning and log access to CloudTrail

**Best answer: B**

**Explanation:**
Least privilege means granting only the minimum permissions needed for a specific job. In S3, that commonly requires restricting access by prefix so principals can only list and read the specific dataset paths they are authorized to use. This directly prevents access to the
raw/
PII data while still enabling work on
curated/
.
The core principle being tested is least privilege: each identity should receive only the permissions (actions) and scope (resources) required to perform its tasks. For S3-hosted ML datasets, the practical way to enforce least privilege is to limit
s3:GetObject
(and
s3:ListBucket
with an
s3:prefix
condition) to the exact prefixes that the persona needs, such as
curated/*
, and avoid granting broad bucket-wide access. This reduces the blast radius of accidental misuse or credential compromise and helps ensure PII-containing
raw/*
objects remain inaccessible to data scientists.
A common pitfall is using controls that improve auditing or role separation without actually constraining which objects can be accessed; those are valuable, but they do not satisfy the “only this prefix” requirement by themselves.
Auditing vs restriction
Versioning and CloudTrail help trace changes and access but don’t prevent reads of
raw/
.
Separation of duties
Using separate roles is good practice, but it doesn’t inherently enforce prefix-level access for data scientists.
Over-aggressive minimization
Deleting datasets after every run is not required to meet the access constraint and can harm traceability and repeatability.


## Question 15

**Topic:** ML Model Development

A company uses separate AWS accounts for each ML team. A team in account
210987654321
wants to reuse a standard training container that is built and versioned by a central platform team in account
123456789012
.
Exhibit: SageMaker training job log excerpt
[INFO] TrainingImage: 123456789012.dkr.ecr.us-east-1.amazonaws.com/ml-train:1.4.0
[INFO] SageMakerExecutionRole: arn:aws:iam::210987654321:role/sm-training-role
[ERROR] ClientError: AccessDeniedException
[ERROR] not authorized to perform: ecr:BatchGetImage
[ERROR] on resource: arn:aws:ecr:us-east-1:123456789012:repository/ml-train
Which action best enables sharing this reusable training container across accounts while keeping a single centrally managed image?

- A. Add a cross-account ECR repository policy to allow image pull, and grant the training role ECR read permissions
- B. Create VPC interface endpoints for Amazon ECR in the team account to resolve the image pull failure
- C. Switch the training job to use a SageMaker built-in algorithm container instead of a custom container
- D. Export the image to Amazon S3 and have each account load it into its own ECR repository before training

**Best answer: A**

**Explanation:**
The log indicates an authorization failure when the team account tries to pull a Docker image from an ECR repository in a different account. The right sharing strategy is to keep the image centralized in one ECR repository and explicitly grant cross-account pull access through an ECR repository policy, along with the necessary ECR read actions on the SageMaker execution role.
The core issue is cross-account authorization to a centrally managed ECR image. The exhibit explicitly shows an
AccessDeniedException
and the denied action
ecr:BatchGetImage
against the repository ARN in account
123456789012
(see the lines stating
not authorized to perform: ecr:BatchGetImage
and the
arn:aws:ecr:...:123456789012:repository/ml-train
).
To share reusable training containers across accounts while keeping one source of truth:
Keep the image versioned in a central ECR repository.
Add an ECR repository resource policy that allows the consuming account (or its IAM roles) to perform pull actions.
Ensure the SageMaker execution role in the consuming account has the corresponding ECR permissions (for example, authorization token plus read/pull actions).
Network endpoints can help connectivity, but they do not fix an IAM/ECR authorization error.
S3 image shuttling
adds operational overhead and breaks the “single centrally managed image” goal.
Built-in algorithm switch
doesn’t address the reuse requirement for a standard custom training container.
VPC endpoints
affect network routing; the exhibit shows an IAM authorization denial (
AccessDeniedException
), not a connectivity failure.


## Question 16

**Topic:** ML Solution Monitoring, Maintenance, and Security

A company uses Amazon SageMaker to train models on sensitive customer PII stored in Amazon S3 and serves real-time inference through SageMaker endpoints. The security team must prevent unauthorized access to both training data and inference payloads while keeping deployments and operations scalable.
Which approach should the company
AVOID
?

- A. Encrypt S3 training data with SSE-KMS and restrict the bucket policy to approved SageMaker roles and specific S3 access paths
- B. Enable auditing with AWS CloudTrail for data access and SageMaker API activity and send logs to a protected central account
- C. Use one shared SageMaker execution role with broad
- s3:*
- and
- kms:*
- permissions on
- *
- for all teams and environments
- D. Place training jobs and endpoints in a VPC and use VPC endpoints to keep S3 and inference traffic on the AWS network

**Best answer: C**

**Explanation:**
To protect sensitive training data and inference payloads, access should be limited to the minimum set of identities, resources, and network paths required. Overbroad IAM permissions create unnecessary blast radius and make it easier for unauthorized principals to read training data or misuse KMS keys. Using encryption, private connectivity, and auditing supports secure operations without blocking legitimate workflows.
The core control for preventing unauthorized access at scale is least-privilege authorization combined with encrypted storage/transport and auditable access. In SageMaker workflows, that typically means scoping IAM roles to specific S3 prefixes and required KMS keys, limiting network paths (for example, VPC endpoints) so traffic stays on the AWS network, and logging access for investigation and compliance.
A single shared execution role with
s3:*
and
kms:*
on
*
is an anti-pattern because it:
Makes it difficult to enforce separation between teams/environments
Expands the impact of credential leakage or misconfiguration
Increases the chance of unintended access to unrelated datasets or keys
The key takeaway is to minimize permissions and scope access, then layer encryption, private connectivity, and auditing around that foundation.
SSE-KMS + restrictive bucket policy
is a standard control to protect sensitive S3 training data and limit who can read it.
VPC + VPC endpoints
helps keep training and inference traffic private and reduces exposure to the public internet.
CloudTrail auditing
supports detection and investigation of unauthorized access attempts and changes.
One shared broad role
breaks separation of duties and least privilege, increasing blast radius.


## Question 17

**Topic:** ML Model Development

A bank trained an Amazon SageMaker XGBoost model to predict loan approval. The team must validate that performance is consistent across subpopulations (for example,
gender
and
age_band
) and identify potential fairness concerns before registering the model.
Which THREE analyses should the ML engineer perform to meet this requirement?

- A. Tune the global classification threshold for best overall F1
- B. Run a SageMaker Clarify post-training bias report
- C. Compare FPR/TPR and AUC across gender and age bands
- D. Report only overall ROC-AUC on the full test set
- E. Check calibration curves separately for each subgroup
- F. Remove gender from the dataset and assume fairness

**Correct answers: B, C and E**

**Explanation:**
To validate consistency across subpopulations, evaluate model quality and errors separately by group, not only in aggregate. A high-level fairness check also requires computing group-based bias metrics on predictions and verifying that scores are similarly calibrated across groups. These analyses can surface disparate error rates, selection rates, or miscalibration that may create unfair outcomes.
Fairness and consistency checks start by slicing evaluation results by protected or sensitive subpopulations (or their proxies) and comparing metrics that drive decisions. Aggregate metrics can hide large gaps when one group dominates the test set.
Useful high-level checks include:
Per-group performance and error rates (for example AUC, FPR, TPR) to find disparity.
Standard bias metrics (for example disparate impact, equal opportunity difference) computed on predictions; SageMaker Clarify can generate these post-training.
Per-group score calibration/distribution analysis to ensure a score means the same risk level across groups and that a single threshold doesn’t create unintended disparities.
The key takeaway is to evaluate by subgroup using both predictive performance and bias metrics, rather than relying on overall scores or “fairness by omission.”
OK
Compare FPR/TPR and AUC across groups: directly tests consistency and disparity.
OK
SageMaker Clarify post-training bias report: computes common group fairness metrics.
OK
Per-subgroup calibration curves: finds unequal score meaning across groups.
NO
Overall ROC-AUC only: can mask large subgroup performance gaps.
NO
Remove gender and assume fairness: proxies can still encode bias.
NO
Optimize one global threshold for overall F1: ignores subgroup impacts.


## Question 18

**Topic:** Data Preparation for Machine Learning (ML)

A retail company is building a binary classifier in Amazon SageMaker to predict whether a customer will churn in the next 30 days. The dataset contains 1 year of daily customer snapshots with features such as
days_since_last_purchase
,
support_tickets_last_30d
, and a unique
customer_id
. Churn occurs in ~3% of snapshots.
The team needs train/validation/test splits that avoid leakage and keep class balance comparable across splits.
Which approach should the team
AVOID
?

- A. Randomly split snapshots across all dates, then stratify by churn label
- B. Split by
- customer_id
- so each customer appears in only one split, and stratify by churn at the customer level where feasible
- C. Split by time: oldest 80% train, next 10% validation, newest 10% test
- D. Split by time, and within each time window use stratified sampling to maintain the churn rate in train and validation

**Best answer: A**

**Explanation:**
For time-evolving customer snapshots, a random stratified split across all dates is a leakage anti-pattern because it can place later snapshots for the same customer in training while evaluating earlier snapshots in test (or vice versa). That breaks the intended “predict the future from the past” setup and inflates metrics. A leakage-safe split should respect time and/or keep entities isolated across splits while maintaining label distribution as appropriate.
The core principle is to design splits that match the real inference setting and prevent information from the evaluation set from “bleeding” into training. With customer snapshots over time, randomly splitting rows across the full year can put the same
customer_id
into multiple splits and mix future and past observations, which introduces temporal/entity leakage (the model indirectly learns customer-specific patterns that won’t generalize).
Leakage-safe approaches typically:
Use a chronological split (train on older data, test on newer data).
Or split by entity (e.g.,
customer_id
) so each entity appears in only one split.
Apply stratification only when it doesn’t violate the time/entity boundaries.
The key takeaway is that stratification is useful for class imbalance, but it must not override boundaries that prevent leakage.
Chronological holdout
is acceptable because it preserves the “past predicts future” distribution shift.
Group/entity split
is acceptable because it prevents the same customer from appearing in multiple splits.
Stratify within time windows
can be acceptable when done without mixing time boundaries, helping keep churn prevalence comparable.


## Question 19

**Topic:** ML Model Development

A team uses Amazon SageMaker Pipelines to train and evaluate a binary classifier. After a recent pipeline change, the evaluation AUC jumped from 0.76 to 0.99, but the model performs poorly in production. The team suspects evaluation issues such as data leakage, metric misconfiguration, or inconsistent train/test splits across runs.
Which action should the team take
EXCEPT
?

- A. Fit preprocessing on train and apply artifacts to test
- B. Generate features on the full dataset before splitting
- C. Validate the evaluation script’s metric mapping and thresholds
- D. Use a deterministic split and persist a split manifest

**Best answer: B**

**Explanation:**
To troubleshoot an evaluation pipeline, the team should eliminate leakage and ensure the evaluation procedure is repeatable and correctly measured. Actions that fit transformations only on training data, lock down split determinism, and verify metric configuration improve evaluation fidelity. Computing features across the full dataset before splitting is a classic leakage anti-pattern that can produce unrealistically high offline metrics.
The core principle is to keep the test set isolated so it represents unseen data. Any operation that learns from data (feature scaling, encoding, aggregations that depend on label/time, imputation statistics) must be fit using only the training split, then applied to validation/test using the saved artifacts. In SageMaker Pipelines, this typically means producing and passing preprocessing artifacts between steps and making the split deterministic (seeded random split or time-based split) so evaluation is consistent across runs. Separately, the evaluation step must compute the intended metric (and any thresholds) correctly; a miswired metric can also create misleading “great” results. The key takeaway: improve reproducibility and metric correctness without introducing train-test contamination.
Train-only preprocessing
helps prevent leakage by reusing fitted artifacts on test.
Deterministic split
makes evaluations comparable across pipeline runs and reduces accidental overlap.
Metric verification
addresses misconfiguration (wrong metric, label mapping, or threshold) in the evaluation step.
Full-dataset feature generation
is risky because it can encode test-set information into training features.


## Question 20

**Topic:** ML Solution Monitoring, Maintenance, and Security

A team uses a standard workflow (data in S3 train/tune in SageMaker deploy to a SageMaker real-time endpoint monitor). The endpoint occasionally has elevated latency and intermittent request throttling, but the team currently checks the console manually after incidents.
The team must
detect and alert on endpoint health issues (errors, latency, throttling) within 5 minutes
using AWS-native monitoring, with minimal ongoing cost and no changes to the model container or client integration.
Exhibit: Available CloudWatch metrics for the endpoint
ModelLatency
Invocation4XXErrors
Invocation5XXErrors
InvocationThrottles
Which change best improves operability and reliability while meeting the constraints?

- A. Convert the deployment to an asynchronous endpoint to reduce throttling
- B. Use CloudTrail to alert on
- InvokeEndpoint
- API calls that return errors
- C. Create CloudWatch alarms on latency, errors, and throttles; enable logs to CloudWatch Logs with alerts
- D. Enable SageMaker Model Monitor with hourly drift and data quality reports to S3

**Best answer: C**

**Explanation:**
CloudWatch metrics and logs are the primary tools to monitor inference endpoint health at runtime. Alarming on latency percentiles, 4XX/5XX errors, and throttling provides fast detection, and shipping container logs to CloudWatch Logs provides the context needed to troubleshoot. This meets the 5-minute alerting goal with low overhead and no model or client changes.
For SageMaker endpoints, health monitoring is best implemented with CloudWatch metrics for fast, quantitative signals (latency, error rates, throttling) and CloudWatch Logs for qualitative diagnostics (stack traces, timeouts, upstream dependency errors). In this scenario, creating CloudWatch alarms on
ModelLatency
,
Invocation4XXErrors
,
Invocation5XXErrors
, and
InvocationThrottles
(and routing notifications through SNS/on-call tooling) reduces time-to-detect while keeping costs low.
Enabling the endpoint/container logs in CloudWatch Logs lets the team correlate alarm spikes with specific error messages and request patterns. The key tradeoff is some additional log ingestion/storage cost, but it is typically modest compared with the operational benefit of rapid detection and troubleshooting.
Options focused on drift detection, audit logging, or changing the endpoint type do not directly meet the stated 5-minute endpoint health alerting requirement under the constraints.
Model Monitor for drift
helps with data/model drift over hours/days, not rapid detection of endpoint errors/latency/throttling.
CloudTrail for invocations
is for API audit events and does not provide the needed runtime latency/throttling health signals.
Switching to async inference
can change client integration patterns and is unnecessary when the requirement is improved monitoring and alerting.


## Question 21

**Topic:** ML Solution Monitoring, Maintenance, and Security

A team trains a fraud model in Amazon SageMaker, registers it, and deploys it to a real-time endpoint. CloudWatch alarms show p95 latency breaches only during a daily 10-minute traffic spike. The application requires real-time responses (<250 ms p95) and the model cannot be changed.
Exhibit: Endpoint metrics during spike
Current capacity: min 1, max 2 instances (ml.m5.large)
SageMakerVariantInvocationsPerInstance
: ~260
CPU utilization: 92–97%; memory utilization: ~35%
No 4XX errors; occasional 5XX timeouts
Which change is the best way to reduce latency during spikes while minimizing cost outside the spike window?

- A. Increase max capacity to 10 and lower the target invocations per instance (with faster scale-out)
- B. Change the instance type to ml.m5.4xlarge and keep max capacity at 2
- C. Replace the real-time endpoint with a Batch Transform job that runs every minute
- D. Disable autoscaling and set a fixed desired capacity of 8 instances

**Best answer: A**

**Explanation:**
The metrics indicate the endpoint is CPU-bound and constrained by a very low autoscaling max capacity, so it cannot add enough replicas during the spike. Increasing the maximum instance count and scaling out sooner reduces per-instance CPU pressure and request queueing, which lowers p95 latency and timeouts. Because capacity scales back down after the spike, cost outside the spike window stays low.
For SageMaker real-time endpoints, spike latency commonly comes from insufficient concurrency (too few instances) and autoscaling that reacts too late or is capped. Here, CPU is near saturation while memory is low, and the current max capacity of 2 prevents the endpoint from adding enough replicas to handle the spike.
A high-level fix is to tune endpoint autoscaling so the variant scales out earlier and is allowed to scale higher:
Increase
maxCapacity
to cover the spike.
Lower the target for
SageMakerVariantInvocationsPerInstance
(or similar target-tracking metric) so scale-out starts before CPU saturates.
Reduce scale-out cooldown to react faster, while keeping scale-in conservative to avoid thrashing.
This typically improves p95 latency and reliability during spikes while keeping costs low the rest of the day because you only pay for extra instances when they are needed.
Oversize instances only
increases hourly cost and still leaves the endpoint capped at 2 instances during spikes.
Fixed high capacity
can meet the SLA but wastes money during the long off-peak period.
Batch inference swap
breaks the stated real-time response requirement.


## Question 22

**Topic:** ML Model Development

A team trains an XGBoost fraud model in Amazon SageMaker. Data is prepared in S3, then a SageMaker hyperparameter tuning job runs, the best model is deployed to a real-time endpoint, and CloudWatch alarms monitor latency and errors.
Each training job takes ~2 hours on
ml.m5.4xlarge
. The team has a fixed budget that allows at most 40 total training jobs for tuning, and only 4 can run in parallel. The search space is 6 hyperparameters that are mostly continuous ranges (for example,
eta
,
min_child_weight
,
subsample
) and model quality changes smoothly with small hyperparameter changes.
Which change to the tuning step is the best way to improve expected model performance without exceeding the budget?

- A. Switch to exhaustive grid search
- B. Keep random search and double max jobs
- C. Tune only one hyperparameter at a time
- D. Switch tuning to Bayesian optimization

**Best answer: D**

**Explanation:**
With only 40 total trials and 2-hour trainings, the priority is finding a strong configuration with as few evaluations as possible. For a low-dimensional, mostly continuous, smooth search space, Bayesian optimization can use results from prior trials to choose more promising next trials than random sampling. The main tradeoff is less effective scaling to massive parallelism and potential sensitivity to noisy objectives.
This is a “limited budget, expensive trial” tuning problem: each evaluation costs hours and the team can only afford 40 total trials. When the hyperparameter space is relatively small in dimensionality and largely continuous, and the objective behaves reasonably smoothly, Bayesian optimization is usually preferred because it is sample-efficient (it learns from earlier trials to choose better next candidates).
Random search is often better when you can afford many trials, want easy parallelism at large scale, or have very high-dimensional spaces with many irrelevant parameters or lots of discrete/categorical choices. Here, parallelism is already limited (4 at a time), so Bayesian’s more sequential, guided exploration is not a major drawback. Key takeaway: choose Bayesian optimization to maximize performance under a tight trial budget; choose random search when you need broad, parallel exploration or the space is awkward for surrogate modeling.
More random trials
violates the stated cap of 40 total training jobs.
Grid search
is inefficient as dimensions grow and wastes trials on unpromising combinations.
One-at-a-time tuning
misses interactions between hyperparameters and usually underperforms.


## Question 23

**Topic:** ML Model Development

A retail company uses Amazon SageMaker to forecast daily demand per store for the next 7 days (forecast horizon = 7). A nightly SageMaker Pipeline builds lag features and trains an XGBoost regressor. Offline evaluation shows very low RMSE, but after deployment, Amazon SageMaker Model Monitor raises a constraint violation because prediction error is much higher than the training baseline.
Current training split (in the preprocessing step):
train_test_split(data, test_size=0.2, shuffle=True)
Which change will most likely fix the root cause with the least disruption?

- A. Enable more aggressive hyperparameter tuning for XGBoost
- B. Randomly shuffle data within each store to reduce seasonality bias
- C. Use a chronological split and leave a 7-day gap before validation
- D. Increase the training instance size to avoid underfitting

**Best answer: C**

**Explanation:**
The symptom (great offline RMSE but poor post-deployment accuracy) is typical of time-series leakage caused by random shuffling. For forecasting, validation must simulate the real use case: predicting future values from past data with the required horizon. Using a chronological holdout (often with a horizon-sized gap) produces a realistic estimate and trains without peeking into the future.
Time series forecasting requires splits that preserve time order and match the prediction horizon. With
shuffle=True
, examples from the future can end up in the training set while earlier timestamps land in validation, especially when labels are shifted to predict 7 days ahead. This “future peek” inflates offline RMSE while the deployed model performs poorly on truly future periods, triggering Model Monitor accuracy violations.
A minimal, high-impact fix is to change to a temporal split that mirrors production:
Sort by timestamp (per series/store if applicable).
Use the latest period as the validation set.
Add a gap of 7 days between train and validation so no training row can use information that overlaps the 7-day-ahead target window.
This preserves the existing model approach while aligning evaluation and training with the 7-day horizon.
Bigger instances
address resource limits, not leakage-driven accuracy collapse.
More tuning
optimizes within a flawed evaluation setup and won’t correct the optimistic validation signal.
More shuffling
makes temporal leakage worse by further mixing future and past observations.


## Question 24

**Topic:** Data Preparation for Machine Learning (ML)

A financial services company trains a binary loan-approval model in Amazon SageMaker each night (S3 data -> train/tune -> register -> deploy). Auditors now require an automated, repeatable check for potential bias related to a sensitive attribute (for example,
gender
) and the model label, and the team must keep costs low by avoiding always-on infrastructure. The check must run as part of the workflow before a model can be approved for deployment.
Which change best meets these requirements?

- A. Run fairness SQL in Athena instead of Clarify
- B. Add a Clarify processing step in SageMaker Pipelines
- C. Enable endpoint auto scaling to reduce bias risk
- D. Use Data Wrangler to balance classes automatically

**Best answer: B**

**Explanation:**
Use Amazon SageMaker Clarify as an automated processing step in the pipeline to detect bias in the training data and to compute high-level bias metrics before model approval. This improves operability and auditability because the report is produced consistently every run and stored centrally. It also reduces cost compared with running manual analyses on always-on notebook instances.
Amazon SageMaker Clarify is designed to identify potential bias and compute bias-related metrics at a high level, including pre-training (dataset) bias and post-training (model) bias. Embedding Clarify as a SageMaker Processing job inside SageMaker Pipelines makes the check repeatable and gateable (for example, fail or stop the pipeline if metrics exceed agreed thresholds) and produces artifacts (reports/metrics) that can be stored in S3 and attached to model approval workflows.
This is typically implemented as:
Clarify processing step that reads the training dataset from S3
Computes bias metrics for the specified sensitive feature and label
Writes a bias report/metrics to S3 (and optionally records them with the model package)
The main tradeoff is added pipeline runtime and per-run processing cost, but it removes manual steps and avoids always-on cost while improving governance.
Auto scaling
improves availability/cost for inference capacity, not training-data bias measurement.
Data Wrangler
can transform/prepare data, but it is not the standard service for bias metrics and reporting.
Athena SQL
could produce custom aggregates, but it lacks Clarify’s bias metrics/reporting and is harder to standardize and audit in ML workflows.


## Question 25

**Topic:** ML Model Development

A team runs a nightly Amazon SageMaker training job (data in S3, training in a private VPC) to refresh a PyTorch model before 7:00 AM. The current job uses On-Demand
ml.p3.2xlarge
, costs are high, and the job typically finishes in 4.5 hours. The training script already writes periodic checkpoints.
Which change will reduce training cost while preserving reliability if capacity is interrupted?

- A. Switch the training instance to a smaller CPU instance
- B. Deploy the model to a multi-model endpoint to cut costs
- C. Enable SageMaker managed Spot training with S3 checkpointing
- D. Use EC2 Spot Instances for training without checkpointing

**Best answer: C**

**Explanation:**
Use SageMaker managed Spot training and configure checkpointing to S3. This captures progress so the job can continue after Spot interruptions, preserving reliability while benefiting from Spot discounts. The main tradeoff is potential longer wall-clock time due to interruptions and checkpoint overhead.
The core concept is using discounted Spot capacity for training while maintaining reliability through checkpointing. With SageMaker managed Spot training, you enable Spot for the training job and configure an S3 checkpoint location so SageMaker can restore the latest checkpoint if the instance is reclaimed.
To keep the workflow reliable within the nightly window:
Enable managed Spot training for the training job.
Configure
CheckpointConfig
to an S3 path and ensure the script saves checkpoints regularly.
Set runtime/wait settings so occasional interruptions still allow completion before 7:00 AM.
This improves cost without changing the model or pipeline stages; the main tradeoff is that interruptions can increase end-to-end training time compared to stable On-Demand capacity.
Right-size blindly
can break the 7:00 AM deadline if training slows significantly.
Spot without checkpoints
risks losing progress and missing the nightly delivery if interrupted.
Inference optimization
(multi-model endpoints) doesn’t reduce training cost for the nightly job.


## Question 26

**Topic:** Data Preparation for Machine Learning (ML)

A company trains a fraud classifier in Amazon SageMaker using a pipeline (S3 data AWS Glue SageMaker training/AMT real-time endpoint Model Monitor). The target label is highly imbalanced: 0.4% fraud.
Constraints:
Training+hyperparameter tuning cost must not increase by more than 10%.
The model must remain auditable; the company cannot create synthetic customer records.
The team wants higher fraud recall without redesigning the inference architecture.
Which change is the best optimization to address the class imbalance while meeting these constraints?

- A. Generate synthetic fraud with SMOTE in Data Wrangler
- B. Downsample non-fraud to a small subset
- C. Oversample fraud examples during preprocessing
- D. Use class weights (for example, XGBoost
- scale_pos_weight
- )

**Best answer: D**

**Explanation:**
Applying class weights during training addresses imbalance by penalizing fraud misclassification more heavily, typically improving fraud recall with little to no increase in training cost. It also preserves auditability because it does not fabricate new customer records. The main tradeoff is a higher false-positive rate and the need to re-tune thresholds and calibration.
When the positive class is rare, many algorithms optimize overall loss by prioritizing the majority class. A cost-effective, auditable way to counter this is to use class weights (or per-example weights) so fraud errors contribute more to the training objective. In SageMaker (for example, the built-in XGBoost), setting
scale_pos_weight
(often near ,\(\frac{\#\text{negative}}{\#\text{positive}}\),) can improve fraud recall without increasing the number of training rows, so it typically stays within tight cost constraints and avoids synthetic-record governance issues.
The tradeoff is that emphasizing the minority class can increase false positives and affect probability calibration, so you usually need to re-evaluate precision/recall tradeoffs and choose an operating threshold that meets business requirements.
Oversampling cost/overfit
increases effective dataset size and can overfit duplicates, often raising training/tuning cost.
Downsampling loses signal
can drop important majority-class patterns, hurting stability and overall generalization.
Synthetic data prohibited
violates the stated auditability constraint against creating synthetic customer records.


## Question 27

**Topic:** ML Model Development

When evaluating whether a modeling approach will fit a strict budget for training and time-to-train in Amazon SageMaker, which term describes the process of running many separate training jobs to search for the best hyperparameter values?

- A. Hyperparameter tuning
- B. Model drift
- C. Regularization
- D. Data leakage

**Best answer: A**

**Explanation:**
Hyperparameter tuning is a search procedure that evaluates many hyperparameter configurations by running multiple training jobs. Because it multiplies the number of training runs, it can significantly increase training cost and extend time-to-train compared with training a single model once. In SageMaker, this is commonly performed with Automatic Model Tuning.
Hyperparameter tuning is the process of selecting the best hyperparameter values (for example, learning rate, tree depth, or regularization strength) by training and evaluating multiple candidate models. In Amazon SageMaker, Automatic Model Tuning orchestrates this by creating a tuning job that runs many underlying training jobs, each with a different hyperparameter configuration. This is why tuning is a key cost-and-schedule consideration: more trials typically mean more compute time and higher training spend.
Regularization is often confused with tuning, but it is a modeling technique applied within a single training run to reduce overfitting by adding a penalty to the objective; it does not inherently require multiple training jobs. The key takeaway is that tuning scales training cost roughly with the number of trials you run.
Regularization vs. tuning
regularization changes the loss/constraints inside one training job, while tuning runs many jobs to search values.
Model drift
is a post-deployment change in data/model performance, not a training-time search procedure.
Data leakage
is unintended use of future/target information during training, affecting validity rather than cost planning.


## Question 28

**Topic:** Data Preparation for Machine Learning (ML)

A healthcare company is preparing an Amazon SageMaker training dataset in Amazon S3 that includes structured claims (patient name, SSN, date of birth, ZIP code, diagnosis codes) and free-text clinician notes containing PHI. Data scientists must be able to join records across tables, but they do not need to see direct identifiers. Which TWO actions best protect sensitive attributes in the ML dataset before training? (Select TWO.)

- A. Detect and redact PHI in notes using Amazon Comprehend Medical
- B. Enable Amazon Macie on the S3 buckets and review findings
- C. Tokenize identifiers with deterministic keyed hashing; drop raw fields
- D. Run training jobs in private subnets and use VPC endpoints for S3
- E. Encrypt the S3 buckets and SageMaker volumes with SSE-KMS
- F. Use Lake Formation to deny access to columns containing PII

**Correct answers: A and C**

**Explanation:**
Use anonymization and masking that change the data content before it reaches training. Deterministic tokenization (with a secret key) removes direct identifiers while keeping a consistent surrogate key for joins. PHI redaction on clinician notes masks sensitive entities in unstructured text so the model does not learn or expose them.
To protect PII/PHI in ML datasets, apply data classification to locate sensitive fields and then apply anonymization/masking so sensitive attributes are not present in the training data. For structured identifiers, replace values like name/SSN/patient ID with a deterministic, keyed token (for example, HMAC) so the same person maps to the same surrogate key across tables, while the original identifier is removed. For unstructured notes, run an NLP de-identification step (such as Amazon Comprehend Medical PHI detection) and redact or replace detected entities before writing the curated training dataset to S3.
Controls like encryption, network isolation, and access permissions reduce exposure but do not remove sensitive attributes from the dataset itself.
OK
Tokenize identifiers with deterministic keyed hashing; drop raw fields — anonymizes direct identifiers while preserving joinability.
OK
Detect and redact PHI in notes using Amazon Comprehend Medical — masks PHI in unstructured text before training.
NO
Encrypt the S3 buckets and SageMaker volumes with SSE-KMS — protects at rest but leaves PII/PHI unchanged in the dataset.
NO
Enable Amazon Macie on the S3 buckets and review findings — classifies/discovers sensitive data but does not anonymize or mask it.
NO
Use Lake Formation to deny access to columns containing PII — access control helps governance, but PII still exists in the stored dataset.
NO
Run training jobs in private subnets and use VPC endpoints for S3 — reduces network exposure but does not transform sensitive attributes.


## Question 29

**Topic:** Deployment and Orchestration of ML Workflows

A company deploys an NLP model for real-time inference on Amazon SageMaker. The model is retrained weekly and must be promoted from dev to staging to prod across separate AWS accounts.
Requirements:
Infrastructure must be repeatable and parameterized (no manual console steps).
Rollouts must be safe with automatic rollback if p95 latency exceeds 200 ms or 5xx rate increases.
Inference must be real time (not batch) and support autoscaling.
Deployments must be auditable and artifacts encrypted at rest.
Which solution best meets these requirements?

- A. CDK + CodePipeline/CodeDeploy canary to SageMaker endpoint variants
- B. Deploy the model container to ECS with a rolling update
- C. Manually update a single SageMaker endpoint in the console
- D. Run SageMaker Batch Transform weekly and store outputs in S3

**Best answer: A**

**Explanation:**
Define the deployment as infrastructure as code and use a managed progressive delivery mechanism for SageMaker endpoints. Pair canary/linear traffic shifting with CloudWatch alarms to automatically roll back when latency or error rates regress. Use SageMaker model/version artifacts with encryption and AWS audit logs to satisfy governance requirements.
The core need is maintainable, reliable ML infrastructure: repeatable configuration, safe rollouts, and observability hooks that can trigger rollback. An AWS-native pattern is to use infrastructure as code (AWS CDK or CloudFormation) to define SageMaker models/endpoints and supporting IAM/KMS/VPC settings, then run CI/CD with CodePipeline. For safe rollout, use CodeDeploy’s blue/green (canary or linear) deployment for SageMaker by shifting traffic between endpoint production variants while watching CloudWatch alarms (for p95 latency and 5xx), and automatically rolling back on alarm. Governance is met by using encrypted artifacts (S3/KMS, ECR/KMS as applicable) and auditable change history (CodePipeline/CodeDeploy events and AWS CloudTrail).
The key takeaway is to combine IaC + progressive delivery + monitoring-driven rollback for SageMaker endpoints.
Manual console updates
violates the repeatable, parameterized infrastructure requirement.
ECS rolling update
does not provide SageMaker-native variant traffic shifting with CloudWatch-alarm rollback for this endpoint.
Batch Transform to S3
violates the real-time inference latency/SLA requirement.


## Question 30

**Topic:** ML Model Development

A team is fine-tuning a pretrained model in Amazon SageMaker for a new classification task. They must reduce the risk of catastrophic forgetting of the model’s original capabilities.
Which statement is
NOT
correct (unsafe) as a high-level fine-tuning approach?

- A. Freeze most layers or use LoRA/adapters
- B. Validate on old-task and new-task evaluation sets
- C. Use a large learning rate with only new-task data
- D. Mix a replay set of base-domain data during fine-tuning

**Best answer: C**

**Explanation:**
To prevent catastrophic forgetting, fine-tuning should preserve performance on the original distribution while adapting to the new task. Strategies such as including representative replay data, constraining weight updates (freezing layers or PEFT), and monitoring performance on both old and new evaluation sets help maintain prior capabilities. Fine-tuning only on new-task data with a large learning rate is unsafe because it accelerates overwriting previously learned representations.
Catastrophic forgetting happens when fine-tuning shifts the model parameters too far toward the new task, reducing performance on previously learned behaviors. High-level mitigation is to (1) keep the fine-tuning data representative of both old and new distributions and (2) limit how much the pretrained weights can change.
Common, practical approaches include:
Rehearsal/replay: mix in a subset of original-domain examples (or synthetic proxies) stored in S3.
Constrained updates: freeze most layers, or use parameter-efficient fine-tuning (for example, LoRA/adapters) so only a small set of parameters changes.
Continuous evaluation: track metrics on an old-task holdout set and the new-task set to detect regressions early.
In contrast, training only on new-task data with an aggressive learning rate tends to overwrite prior representations and increases forgetting risk.
Replay data
helps retain prior behavior by keeping gradients anchored to the original distribution.
Freeze/PEFT
limits parameter drift, reducing overwriting of pretrained knowledge.
High learning rate + new-only data
accelerates drift toward the new task and is unsafe for retention.
Dual evaluation sets
are needed to detect regressions on the original capabilities while improving the new task.


## Question 31

**Topic:** Deployment and Orchestration of ML Workflows

A team has a fraud model in Amazon SageMaker. They must score 40 million transactions once per day from Amazon S3 and write results back to S3 within 3 hours. There is no per-transaction real-time requirement, and the team wants to minimize cost and operational overhead.
Which deployment approach should the team
AVOID
for this workload?

- A. Run a SageMaker Batch Transform job on a daily schedule
- B. Run an Amazon ECS task on AWS Fargate to perform batch scoring
- C. Run a SageMaker Processing job that performs batch inference
- D. Keep a SageMaker real-time endpoint running 24/7 for daily scoring

**Best answer: D**

**Explanation:**
Because scoring happens only once per day and can run for hours, the team should use batch-oriented compute that is provisioned only for the job duration. Keeping a real-time endpoint running continuously adds unnecessary always-on instance cost without improving required latency. The violated principle is selecting always-on infrastructure when the workload is offline and periodic.
The core tradeoff is paying for reserved/continuous inference capacity versus provisioning compute only when you need it. For periodic, offline scoring from S3 to S3 with a multi-hour SLA, batch patterns (Batch Transform, Processing, or a containerized batch job on ECS/Fargate) are cost-efficient because they spin up resources for the run and then terminate.
A real-time SageMaker endpoint is designed for low-latency, on-demand requests and charges for provisioned instances while the endpoint is up. Using it for a once-per-day batch job typically wastes money on idle capacity between runs and adds endpoint operations you do not need. The key takeaway is to match the deployment pattern to the access pattern: batch for scheduled bulk inference, endpoints for interactive latency-sensitive inference.
Batch Transform
fits S3-to-S3 offline scoring and avoids always-on capacity.
Processing job
is acceptable for custom batch inference logic with job-scoped compute.
ECS/Fargate batch task
is acceptable when you want containerized batch compute that runs only during the scoring window.
Always-on endpoint
is the exception because it incurs ongoing cost for unused capacity in this scenario.


## Question 32

**Topic:** Data Preparation for Machine Learning (ML)

A data engineer is building an Amazon SageMaker training dataset in an AWS Glue Spark job by joining a fact table (
transactions
) with a dimension table (
customer_attributes
) on
customer_id
. The output row count is unexpectedly higher than the input fact table.
Exhibit: Glue job log (row counts)
transactions: rows=10,000 distinct(customer_id)=9,500
customer_attributes: rows=12,300 distinct(customer_id)=9,500
joined_output (left join on customer_id): rows=12,940
Based on the exhibit, what is the best next step to preserve join correctness and avoid unintended duplication?

- A. Change the left join to an inner join
- B. Deduplicate customer_attributes to one row per customer_id before joining
- C. Normalize customer_id strings (trim/lowercase) before joining
- D. Increase Spark shuffle partitions for the join stage

**Best answer: B**

**Explanation:**
The join is multiplying fact rows because the dimension side is not unique on the join key. The exhibit shows
customer_attributes
has more rows than distinct
customer_id
, and the joined output row count is greater than the fact table row count. Ensuring a single record per
customer_id
in the dimension (or otherwise enforcing the intended join cardinality) prevents unintended duplication.
Join correctness depends on the expected key cardinality. Here,
transactions
should not grow after a left join unless the right-side table has multiple matches per key.
The exhibit indicates this exact problem:
customer_attributes: rows=12,300 distinct(customer_id)=9,500
shows multiple rows per
customer_id
.
joined_output ... rows=12,940
being greater than
transactions: rows=10,000
confirms row multiplication.
To preserve correctness, make the dimension deterministic at one row per
customer_id
before the join (for example, pick the latest effective record with a window function, or aggregate to a single row). This keeps the join one-to-one (or the intended one-to-many) instead of unintentionally many-to-many.
Inner vs left join
changes which facts are retained; it doesn’t address duplicated matches on the right.
Tuning shuffle partitions
can improve performance but won’t change the number of matched rows.
String normalization
helps prevent missed matches; it typically reduces matches rather than increasing row counts.


## Question 33

**Topic:** Data Preparation for Machine Learning (ML)

You are troubleshooting scalability issues in an ML data ingestion and storage pipeline on AWS. Which statement is
NOT
correct about diagnosing throughput bottlenecks and choosing mitigations?

- A. AWS Glue jobs can scale out by increasing workers and parallelism.
- B. S3 request throughput can improve by spreading objects across prefixes.
- C. Kinesis Data Streams shard hot spots can occur from skewed partition keys.
- D. S3 throughput is unaffected by prefix design, so prefix spread never helps.

**Best answer: D**

**Explanation:**
A common scalability bottleneck in S3-backed pipelines is uneven request distribution (a hot prefix), which can limit effective parallel throughput for reads/writes. Spreading objects and requests across multiple prefixes increases parallelism and reduces hot-spotting risk. The other statements correctly describe common throughput ceilings and mitigations for Kinesis and AWS Glue.
Scalability bottlenecks in ML ingestion/storage pipelines often come from constrained parallelism (too few partitions/shards) or skew (hot partitions). For S3-backed datasets, request patterns that concentrate activity on a small keyspace (for example, a single heavily used prefix) can reduce effective throughput; distributing objects across multiple prefixes and enabling readers to parallelize across those prefixes is a standard mitigation.
Similarly, Kinesis Data Streams ingest/read throughput scales with shard count, but a poor partition-key strategy can cause one shard to become a hot spot even when overall shard capacity looks sufficient. For transformation/ETL, AWS Glue can reduce runtime by scaling out workers/DPUs and increasing parallelism, assuming the job is not bottlenecked by a single hot partition or external system.
The key takeaway is to look for throughput ceilings and hot partitions, then increase parallelism and improve key/partition distribution.
S3 prefix distribution
is a valid mitigation for request hot spots and improves parallelism.
Kinesis hot shards
are commonly caused by partition-key skew and require key/reshard mitigation.
Glue scale-out
is a normal approach when the job is limited by available parallel workers.


## Question 34

**Topic:** Data Preparation for Machine Learning (ML)

A team trains an XGBoost model in Amazon SageMaker using tabular data in Amazon S3. The team uses multiple transformations (imputation, one-hot encoding, and numeric scaling) and must ensure feature parity between training and inference across future model versions.
Which approach should the team
NOT
use?

- A. Use the same preprocessing script in both a SageMaker Processing step and the inference container
- B. Store transformation artifacts (for example, encoders/scalers) from training and load them at inference
- C. Recreate the transformations in the inference code by hand to match training
- D. Register the feature definitions and transformations and reuse them via SageMaker Feature Store

**Best answer: C**

**Explanation:**
To ensure feature parity, transformation logic and feature definitions should be reusable and applied consistently in both training and inference. Manually re-implementing preprocessing separately for inference creates two sources of truth, making it easy for the online path to diverge from the training pipeline as code and schemas evolve.
The core principle is to keep training and inference on a single, versioned source of truth for feature definitions and transformation logic. If the team hand-codes preprocessing separately for inference, even small differences (category ordering, missing-value handling, scaling parameters, new columns) can change the feature vector and degrade predictions or break the endpoint.
Acceptable patterns include:
Reusing the same preprocessing code artifact in both pipelines and deployment.
Persisting fit-time artifacts (encoders/scalers) and loading them for inference.
Centralizing feature definitions/transformations in a feature store so online/offline retrieval stays consistent.
The key takeaway is to avoid duplicated, independently maintained transformation implementations across training and inference.
Reusable preprocessing
is aligned with feature parity because the same logic runs in both paths.
Persisted encoders/scalers
helps ensure inference uses identical fitted parameters from training.
Feature Store definitions
supports consistent offline/online feature computation and reuse.


## Question 35

**Topic:** Deployment and Orchestration of ML Workflows

A team uses an AWS ML workflow: data in Amazon S3 - train/tune in Amazon SageMaker - deploy - monitor. After training, they deployed a SageMaker real-time endpoint and run a nightly scoring job by invoking the endpoint ~80,000 times in parallel from AWS Lambda, then writing predictions to S3.
The endpoint is idle 23 hours/day, costs are high, and the nightly run often throttles and misses its 2-hour completion SLA. There are no interactive users; results only need to be available in S3 by morning.
Which change is the best way to improve cost and reliability without breaking requirements?

- A. Use a SageMaker multi-model endpoint for the trained model
- B. Enable autoscaling and raise the endpoint max capacity
- C. Switch the deployment to a SageMaker serverless endpoint
- D. Run SageMaker Batch Transform nightly and remove the endpoint

**Best answer: D**

**Explanation:**
This workload is pure batch scoring with an S3 output requirement and no need for low-latency online inference. SageMaker Batch Transform provisions compute only for the job duration and can use multiple instances for throughput, eliminating always-on endpoint cost while reducing throttling risk during the nightly peak.
The core issue is a deployment selection mismatch: a real-time endpoint is designed for interactive, low-latency requests and is billed continuously, but the team’s pattern is a scheduled, high-throughput batch job. Moving inference to SageMaker Batch Transform improves both cost and reliability because it uses ephemeral compute sized for the batch window and avoids per-request endpoint throttling.
A high-level corrective approach is:
Trigger a Batch Transform job on a schedule (for example, EventBridge).
Read input from S3 and write predictions back to S3.
Size instance count/type for the 2-hour SLA and monitor job status/metrics.
The main tradeoff is that inference becomes job-based (not interactive), but that aligns with the stated requirements.
Autoscaling an endpoint
still pays for baseline capacity and can still bottleneck under bursty, highly parallel invocations.
Serverless endpoint
keeps the per-request invocation pattern and does not address batch orchestration or peak-concurrency bottlenecks as directly.
Multi-model endpoint
is for hosting many models behind one endpoint, not for scheduled high-throughput batch scoring.


## Question 36

**Topic:** ML Model Development

A team is comparing two Amazon SageMaker training jobs and wants the results to be reproducible.
Exhibit: Training summary
Job: churn-xgb-2026-02-20-01
Input: s3://ml-data/churn/train.csv
HyperParams: {eta=0.1, max_depth=5}
Metric: validation:auc=0.912
Job: churn-xgb-2026-02-20-02
Input: s3://ml-data/churn/train.csv
HyperParams: {eta=0.1, max_depth=5}
Metric: validation:auc=0.935
Which action best enables reproducible model development for these runs?

- A. Deploy both models to a multi-model endpoint to compare inference performance
- B. Enable SageMaker Debugger to capture tensors and gradients during training
- C. Version the dataset and log the exact dataset version, parameters, and metrics in SageMaker Experiments
- D. Run SageMaker Automatic Model Tuning to explore additional hyperparameters

**Best answer: C**

**Explanation:**
The exhibit shows identical input location and hyperparameters across runs but different validation AUC, which is a reproducibility red flag. To reproduce and audit results, each experiment run should record immutable dataset identifiers (for example, versioned S3 objects or timestamped prefixes) together with the run’s parameters and resulting metrics using an experiment tracker such as SageMaker Experiments.
Reproducible training requires being able to re-run the same code against the same dataset version with the same configuration and then compare resulting metrics. In the exhibit, the two jobs have the same hyperparameters and point to the same S3 object key (
Input: s3://ml-data/churn/train.csv
) but yield different AUC values, which commonly happens when the data at that key changes over time (or when run configuration isn’t fully captured).
A practical AWS approach is to:
Make datasets immutable or versioned (for example, S3 Versioning or unique, timestamped prefixes/manifests).
Track each run as an experiment/trial component and record dataset identifiers, hyperparameters, and metrics in SageMaker Experiments.
This creates an auditable lineage for each metric value back to the exact dataset and settings that produced it.
Debugger focus
helps diagnose training behavior, but it doesn’t solve missing dataset lineage implied by reusing
s3://.../train.csv
.
HPO focus
improves search, but it doesn’t explain differing metrics when
HyperParams
are already the same.
Deployment comparison
is an inference workflow and doesn’t capture training dataset/metadata needed for reproducibility.


## Question 37

**Topic:** ML Solution Monitoring, Maintenance, and Security

A company runs Amazon SageMaker training pipelines and real-time endpoints across multiple AWS accounts in an AWS Organization. Compliance requires audit evidence for
7 years
, centrally stored in a dedicated logging account, encrypted with KMS, and protected against tampering/deletion. Security operations also needs near real-time visibility into API activity and endpoint application logs for investigations.
Which TWO actions should the ML engineer
AVOID
because they do not meet these requirements? (Select TWO.)

- A. Configure SageMaker endpoint container logs to stream to CloudWatch Logs and set the log group retention to 7 years
- B. Create an organization trail for all Regions that delivers to an S3 bucket in the logging account with SSE-KMS and log file validation enabled
- C. Rely on CloudTrail Event history and set CloudWatch Logs retention to 30 days
- D. Create separate account-level trails that deliver to S3 buckets in each application account managed by engineers
- E. Enable S3 Object Lock (compliance or governance mode) on the centralized CloudTrail S3 bucket and restrict delete actions to a break-glass role
- F. Send CloudTrail events to CloudWatch Logs for alerting and investigation, in addition to storing the long-term copy in S3

**Correct answers: C and D**

**Explanation:**
Meeting compliance and investigation needs requires durable, centralized, tamper-resistant audit logs and searchable operational logs. CloudTrail should provide organization-wide API auditing with long-term retention in a protected S3 bucket, while CloudWatch Logs supports near real-time visibility and retained endpoint logs. Any approach that relies on short-lived history or decentralized, engineer-managed storage fails the stated evidence requirements.
For security investigations and compliance evidence, treat audit and operational logs as immutable records with controlled access. Use CloudTrail (preferably an organization trail across Regions) to capture API activity and deliver logs to a centralized S3 bucket in a dedicated logging account with KMS encryption, integrity controls (log file validation), and strong anti-tamper protections (for example, S3 Object Lock and restricted delete permissions).
CloudWatch Logs complements this by providing near real-time log search, metric filters/alarms, and centralized application logging (such as SageMaker endpoint container logs) with retention set to match policy. The key failure patterns are depending on short-retention sources (like CloudTrail Event history) or storing logs in non-central, weakly protected locations where administrators can alter or delete evidence.
Short-lived sources
like CloudTrail Event history and short CloudWatch retention cannot satisfy 7-year evidence needs.
Decentralized storage
in application accounts increases the risk of log deletion/tampering and breaks centralized logging-account requirements.
Org trail to protected S3
supports durable audit evidence with encryption and integrity controls.
CloudWatch Logs + S3
is a common pairing: CloudWatch for near real-time ops/security, S3 for long-term compliance retention.


## Question 38

**Topic:** Deployment and Orchestration of ML Workflows

A machine learning engineer is reviewing an Amazon SageMaker batch inference configuration.
Exhibit: Transform job request (partial)
TransformInput:
S3Uri: s3://ml-prod/inference-input/2026-02-25/
TransformOutput:
S3OutputPath: s3://ml-prod/inference-output/2026-02-25/
TransformResources:
InstanceType: ml.m5.xlarge
InstanceCount: 4
Based on the exhibit, which interpretation best describes the batch workflow (input location, output location, and parallelism)?

- A. Creates a real-time endpoint with 4 instances for inference
- B. Reads from S3OutputPath and writes results to the input prefix
- C. Runs on one instance; parallelism comes only from S3 prefix sharding
- D. Reads input S3Uri, writes to S3OutputPath, uses 4 instances

**Best answer: D**

**Explanation:**
This is a SageMaker batch transform-style configuration where S3 is used for both the input data location and the output results location. The job’s compute parallelism is determined by how many instances are provisioned for the transform resources.
In a batch inference workflow on SageMaker, you typically provide an S3 prefix for where the input objects are read from, an S3 prefix for where inference outputs are written, and a resource configuration that determines how much compute is allocated.
Here, the exhibit explicitly identifies:
Input location as
S3Uri: s3://ml-prod/inference-input/2026-02-25/
Output location as
S3OutputPath: s3://ml-prod/inference-output/2026-02-25/
Parallelism capacity via
InstanceCount: 4
under
TransformResources
A real-time endpoint configuration would not be expressed with
TransformInput
/
TransformOutput
fields like these.
Swapped S3 paths
contradicts the separate
S3Uri
(input) and
S3OutputPath
(output) lines in the exhibit.
Real-time endpoint
is inconsistent with the presence of
TransformInput
and
TransformOutput
in the request.
No instance parallelism
conflicts with
TransformResources
showing
InstanceCount: 4
.


## Question 39

**Topic:** Deployment and Orchestration of ML Workflows

A company is building a regulated ML CI/CD pipeline using AWS CodePipeline and Amazon SageMaker Pipelines. Auditors require that for any deployed model the team can prove, for at least 2 years, exactly which training data, code commit, and container image produced it.
Which TWO actions should the team
AVOID
because they make the pipeline non-auditable or non-reproducible?

- A. Register models in SageMaker Model Registry with metadata
- B. Overwrite S3 dataset objects with versioning disabled
- C. Enable S3 versioning for data and model artifact buckets
- D. Enable CloudTrail for CodePipeline, SageMaker, and S3 events
- E. Pull ECR
- latest
- image tag without recording digest
- F. Tag training jobs with git SHA and dataset version ID

**Correct answers: B and E**

**Explanation:**
Auditability requires immutable, linkable evidence for data, code, and runtime artifacts. Using mutable references (like an ECR tag) or losing historical versions of datasets prevents the team from reconstructing what produced a specific model. The safer pattern is to keep versioned/immutable artifacts and record their identifiers as metadata through the pipeline.
To meet compliance traceability, an ML CI/CD pipeline should produce an end-to-end lineage record that ties each model to immutable identifiers for (1) the training data snapshot, (2) the exact code revision, and (3) the exact container image/build. Mutable pointers (for example, an ECR tag that can be moved) and non-versioned data locations break reproducibility because the same identifier can refer to different content over time.
A robust approach is:
Store datasets and model artifacts in versioned S3 locations and capture object version IDs.
Capture code provenance (commit SHA) and container provenance (ECR image digest) and attach them as tags/metadata to training runs and registered model packages.
Use CloudTrail (and, where appropriate, S3 data events) to provide an independent audit log of changes and access.
The key takeaway is to record immutable artifact IDs, not mutable names.
Mutable container reference
using
latest
without a digest prevents proving which image actually ran.
Lost data lineage
overwriting non-versioned S3 objects removes the historical training snapshot.
Versioned artifacts
S3 versioning supports reconstructing exact inputs/outputs used.
Metadata + audit logs
tags/Model Registry entries and CloudTrail events strengthen end-to-end traceability.


## Question 40

**Topic:** ML Solution Monitoring, Maintenance, and Security

A team hosts a fraud model on a SageMaker real-time endpoint with Data Capture enabled. A SageMaker Model Monitor schedule runs hourly and publishes a CloudWatch alarm (
Fraud-DataDrift-Alarm
) when a constraint violation is detected. The team expects an AWS Step Functions state machine to start a SageMaker Pipeline retraining run whenever the alarm goes to ALARM, but no executions are starting.
Exhibit: Current EventBridge rule event pattern
{
"source"
:
[
"aws.sagemaker"
],
"detail-type"
:
[
"SageMaker Model Monitor Schedule Execution"
],
"detail"
:
{
"monitoringScheduleName"
:
[
"fraud-mm"
]
}
}
Which change will fix the issue with the least operational change while preserving the existing drift trigger?

- A. Disable Data Capture and read inference logs from CloudWatch Logs to detect drift
- B. Recreate the Model Monitor baseline with a larger reference dataset so the alarm stops flapping
- C. Increase the Model Monitor schedule frequency from hourly to every 5 minutes
- D. Update the EventBridge rule to match
- CloudWatch Alarm State Change
- for
- Fraud-DataDrift-Alarm
- and target the Step Functions
- StartExecution
- API

**Best answer: D**

**Explanation:**
The monitoring signal already exists as a CloudWatch alarm, but the EventBridge rule is listening for a SageMaker event type that won’t fire for alarm transitions. Use the alarm’s state change event as the retraining trigger. Then EventBridge can invoke Step Functions to orchestrate the SageMaker Pipeline run.
Symptom: the drift alarm is entering ALARM, but no Step Functions executions start.
Root cause: the EventBridge rule is filtering on a SageMaker Model Monitor “schedule execution” event, while the team’s chosen retraining trigger is a CloudWatch alarm transition. EventBridge will never match the alarm event with the current pattern.
Fix: connect the monitoring output to orchestration by triggering on the CloudWatch alarm state change event and using it to start the workflow:
Create/update an EventBridge rule with
source
=
aws.cloudwatch
Match
detail-type
=
CloudWatch Alarm State Change
and the specific alarm name
Set the rule target to Step Functions
StartExecution
(optionally passing alarm details)
This keeps the existing drift threshold as the retraining trigger and only corrects the event wiring.
More frequent monitoring
increases cost/noise and doesn’t address the event pattern mismatch.
Turning off Data Capture
removes the data needed for Model Monitor and breaks the existing drift trigger.
Rebuilding the baseline
changes model-monitoring behavior but still won’t start Step Functions without the correct EventBridge trigger.


## Question 41

**Topic:** Deployment and Orchestration of ML Workflows

In Amazon SageMaker, which deployment option is intended for inference requests with
large payload sizes and long processing times
, where the client should not keep a synchronous connection open while the result is computed?

- A. Serverless inference endpoint
- B. Asynchronous inference endpoint
- C. Batch transform job
- D. Real-time inference endpoint

**Best answer: B**

**Explanation:**
SageMaker asynchronous inference is built for workloads where requests can take longer to process and/or carry larger payloads than a synchronous request-response pattern is meant to handle. It decouples the client from the model’s processing time by queueing requests and delivering outputs asynchronously. This makes it a better fit than low-latency real-time patterns.
The key requirement is that inference processing time and payload size may exceed what is practical for a synchronous API call. A SageMaker asynchronous inference endpoint addresses this by accepting requests, placing them in a queue, and writing outputs to a destination (commonly Amazon S3) when processing completes. This design helps handle variable processing times and supports higher concurrency without forcing the caller to keep a connection open.
Batch transform is the closest alternative but is intended for offline, job-based processing of an entire dataset rather than request-driven endpoint inference. If you need interactive, request/response behavior with low latency, a real-time endpoint (including serverless real-time for spiky traffic) is typically more appropriate.
Real-time latency focus
fits low-latency synchronous calls, not long-running requests.
Serverless inference
targets intermittent traffic and scales-to-zero, but still follows a synchronous real-time pattern.
Batch transform
is offline/batch processing and does not provide an endpoint for per-request inference.


## Question 42

**Topic:** ML Model Development

A data science team trained a regression model in Amazon SageMaker to predict home sale price (USD). The team captured the following evaluation summary on the validation set.
Exhibit: Validation metrics and residual summary
Train RMSE (USD): 18,900
Validation RMSE (USD): 19,300
Residual mean (val): +$120
Residual std by true price bucket (val):
<$200k: $8,100
$200k–$500k: $17,600
>$500k: $42,300
Based on the exhibit, which next step is the BEST way to address the issue revealed by the residual pattern?

- A. Model
- log(price)
- (or similar) and re-evaluate on original scale
- B. Increase model complexity to fix underfitting
- C. Switch to classification metrics such as AUC to assess performance
- D. Add early stopping to reduce overfitting

**Best answer: A**

**Explanation:**
The model’s overall RMSE is similar for train and validation, but the residual standard deviation grows substantially for higher-priced homes. This indicates heteroscedastic errors (non-constant variance) rather than classic overfitting/underfitting. A common next step is to transform the target (for example, predict log(price)) or otherwise weight the loss to better fit relative error across price ranges.
For regression, RMSE summarizes average error magnitude, but residual patterns can reveal systematic issues that a single metric can hide. In the exhibit, train and validation RMSE are close (18,900 vs 19,300), suggesting generalization is not the primary problem.
The key signal is non-constant residual variance across the target range: residual standard deviation rises from $8,100 for homes under $200k to $42,300 for homes over $500k (see the bucketed residual std lines). This is heteroscedasticity, which often improves by modeling the target on a transformed scale (for example, log) so errors become more proportional across ranges, then converting predictions back to USD.
This is a better fit than simply tuning training duration or model size when the dominant issue is variance that grows with the target magnitude.
Early stopping
doesn’t target the issue because train and validation RMSE are already similar (18,900 vs 19,300).
More complexity
is unlikely to help when the main problem is error variance increasing with price (std $8,100 to $42,300).
Classification metrics
are not applicable because the task is price regression and the exhibit reports regression residuals.


## Question 43

**Topic:** Data Preparation for Machine Learning (ML)

A healthcare company needs to create human-labeled data for an NLP model. Use the exhibit to choose the most appropriate AWS labeling approach.
Exhibit: Labeling request (excerpt)
1) Dataset: 250,000 call transcripts
2) Contains PHI
3) Data must not leave the AWS account
4) Annotators must be internal employees (HIPAA BAA)
5) Quality: 2 annotators per item + adjudication
Which labeling approach best satisfies these requirements?

- A. Use SageMaker Ground Truth with the public workforce and rely on NDAs
- B. Use Amazon Mechanical Turk to run public HITs with worker qualifications
- C. Export transcripts to an external labeling vendor and import labels back to S3
- D. Use SageMaker Ground Truth with a private workforce and annotation consolidation

**Best answer: D**

**Explanation:**
The exhibit’s governance constraints require that PHI stays within the AWS account and that only internal employees label the data. SageMaker Ground Truth with a private workforce satisfies these constraints while also supporting the required two-annotator workflow with adjudication through annotation consolidation.
The core decision is governance and quality control for labeling. The exhibit states the data contains PHI (line 2), must not leave the AWS account (line 3), and annotators must be internal employees under a HIPAA BAA (line 4). That rules out public crowdsourcing and external vendor workflows where data access expands beyond the company’s controlled workforce.
SageMaker Ground Truth with a private workforce lets you:
Restrict labeling access to internal users while keeping data in your account (lines 3–4)
Configure multiple labelers per item and use annotation consolidation for adjudication (line 5)
Key takeaway: when sensitive data and strict workforce governance are required, choose a private workforce labeling solution rather than public or external options.
Public crowd with qualifications
still uses non-internal workers, violating the internal-employee requirement (line 4).
Public workforce with NDAs
does not meet the “internal employees” constraint and increases PHI exposure (lines 2 and 4).
External vendor labeling
contradicts the requirement that data must not leave the AWS account (line 3).


## Question 44

**Topic:** Data Preparation for Machine Learning (ML)

A financial services company is preparing a training dataset in Amazon S3 for a loan approval model in Amazon SageMaker. The dataset includes a label column
approved
(1 = approved, 0 = denied), a sensitive attribute column
gender
, and PII columns (
ssn
,
email
).
The ML engineer must use SageMaker Clarify to assess potential bias in the training data and produce an auditable bias report. Company policy requires that PII is not exported to publicly accessible locations and that all Clarify outputs in S3 are encrypted with SSE-KMS.
Which TWO actions should the engineer
AVOID
? (Select TWO.)

- A. Point Clarify at the raw S3 prefix containing
- ssn
- and write the report to an S3 bucket with public read access
- B. Run Clarify without specifying a
- facet_name
- (sensitive attribute) and infer bias from overall label imbalance
- C. Use Clarify post-training bias with a SageMaker model (or model package) and store outputs in an SSE-KMS encrypted S3 location
- D. Add a Clarify processing step in SageMaker Pipelines to generate and store bias reports
- E. Run separate Clarify bias jobs for
- gender
- (and other protected attributes) and compare the resulting metrics
- F. Run a Clarify pre-training bias processing job on a curated dataset that excludes PII

**Correct answers: A and B**

**Explanation:**
SageMaker Clarify bias metrics require defining the sensitive attribute (facet) so metrics can be computed across groups. The workflow must also meet data handling requirements by preventing public exposure of PII and ensuring the generated reports are stored securely with SSE-KMS encryption.
SageMaker Clarify evaluates bias by comparing outcomes across groups defined by a sensitive attribute (for example,
gender
) for either pre-training (dataset) or post-training (model predictions) analysis. To generate meaningful bias metrics, you must configure Clarify with the label and the facet so it can compute group-based statistics (such as disparate impact–style measures) rather than only global class distributions.
Because Clarify produces artifacts (reports, constraints, and intermediate outputs) in S3, the storage location must follow your organization’s security requirements. For regulated data, this typically means restricting bucket access (no public policies/ACLs) and enabling SSE-KMS so outputs are encrypted at rest. The key takeaway is that Clarify needs an explicitly defined sensitive facet for correct bias evaluation, and its output handling must meet security controls.
Curated, non-PII input
is acceptable because Clarify can assess bias using non-PII features while keeping PII out of the analysis artifacts.
Pipeline-integrated reports
is acceptable because Clarify processing steps can produce repeatable, auditable outputs in S3.
Multiple facets via multiple runs
is acceptable because running Clarify per protected attribute is a common way to assess bias across different groups.
Post-training bias with encrypted outputs
is acceptable because Clarify supports post-training bias analysis while storing artifacts in SSE-KMS protected S3 locations.


## Question 45

**Topic:** Data Preparation for Machine Learning (ML)

A team stores real-time fraud features in an Amazon SageMaker Feature Store feature group that was created with only the online store enabled. A new SageMaker training pipeline step tries to build a training dataset by running an Athena query against the feature group’s offline store.
Exhibit: Pipeline error
ValidationException: FeatureGroup FraudFeatures has no OfflineStoreConfig
The team must keep low-latency feature lookups for the existing real-time endpoint and make the smallest change that fixes the training failure. What should the ML engineer do?

- A. Add IAM permissions for
- athena:StartQueryExecution
- to the role
- B. Read training data from the online store using
- GetRecord
- calls
- C. Create a new feature group with an offline store and backfill
- D. Increase training instance size to avoid the Athena query failure

**Best answer: C**

**Explanation:**
The training step fails because the feature group was created without an offline store configuration, so there is no S3-backed offline table for Athena to query. SageMaker Feature Store does not support enabling the offline store on an existing feature group after creation. Creating a new feature group with offline storage (and backfilling features) fixes training while preserving online lookups for low-latency inference.
SageMaker Feature Store uses two distinct storage paths: the online store for low-latency key-based inference lookups, and the offline store (S3 + Glue Data Catalog) for analytics and training-time batch access (for example, via Athena). The error indicates the feature group has no
OfflineStoreConfig
, which happens when it was created with only the online store enabled.
To fix this with minimal disruption:
Create a new feature group with offline store enabled (optionally enable both online and offline).
Ingest/backfill the same feature records into the new feature group.
Point the training pipeline’s Athena/offline reads at the new offline store while leaving the existing endpoint’s online lookups unchanged until you cut over.
The key takeaway is that offline storage must be planned at feature group creation time for training use cases.
Bigger instances
does not create an offline store or resolve missing
OfflineStoreConfig
.
IAM permissions
can still be required, but the failure is that the offline store does not exist to query.
Online store for training
is inefficient and not intended for large batch dataset creation compared with the offline store.


## Question 46

**Topic:** ML Model Development

A company runs a SageMaker pipeline for a fraud model. The ML engineer wants to add an automated check that flags unusual shifts in incoming feature distributions (no labels available) so the team can investigate potential data drift before model performance degrades. Which Amazon SageMaker built-in algorithm best matches this problem type?

- A. XGBoost
- B. k-means
- C. Linear Learner
- D. Random Cut Forest

**Best answer: D**

**Explanation:**
This is an unlabeled, unsupervised need to identify unusual incoming data patterns that may indicate drift. Random Cut Forest is a SageMaker built-in algorithm designed for anomaly detection on numeric features, making it appropriate for automated drift signals. This aligns with the core principle of monitoring for drift to maintain ML system performance over time.
The core principle here is
monitoring for drift
: you want an automated signal when new data looks meaningfully different from what the model was trained on. Because the stem specifies
no labels
and the goal is to
flag unusual patterns
in feature distributions, the matching built-in approach is unsupervised anomaly detection. Amazon SageMaker Random Cut Forest (RCF) is designed to detect outliers by assigning anomaly scores to observations, which can be used to trigger investigation workflows (for example, alarms and retraining evaluation) when feature behavior changes.
The key takeaway is to choose an algorithm that matches the learning setup (unlabeled anomaly detection) rather than a supervised predictor.
Supervised boosting
would require labeled targets to train a predictive model, which the stem does not provide.
Supervised linear models
are for classification/regression with labels, not unlabeled drift signaling.
Clustering
can segment data, but it is not purpose-built to score point anomalies as a drift trigger in the way RCF is.


## Question 47

**Topic:** ML Model Development

A company uses an Amazon SageMaker pipeline to forecast daily demand for 10,000 SKUs. Data is stored in Amazon S3, a processing step builds lag features, SageMaker Automatic Model Tuning trains an XGBoost model, the model is deployed, and Amazon CloudWatch monitors MAPE.
The business requires a 30-day forecast horizon. Offline validation MAPE is 8%, but after deployment, MAPE is 18%. The current pipeline creates the training and validation sets by randomly splitting rows across the full 2-year history.
Which change is the best way to improve forecast performance and reliability without increasing cost significantly?

- A. Enable SageMaker managed Spot Training for the tuning jobs
- B. Use a time-ordered split that holds out the most recent 30 days per SKU for validation
- C. Replace the split with k-fold cross-validation using shuffled folds
- D. Increase real-time endpoint capacity and configure automatic scaling

**Best answer: B**

**Explanation:**
Time series data must be split by time to avoid training on information from the future relative to the validation period. Holding out the most recent 30 days (the forecast horizon) creates an evaluation that matches how the model will be used in production. This usually improves model selection and reduces the offline-to-online performance gap with minimal added cost.
The core issue is temporal leakage and a mismatch between how the model is validated and how it is used. Random row splits let the model see patterns from dates that occur after (or interleaved with) the validation dates, which can make offline metrics unrealistically optimistic for forecasting.
A better approach is to validate using a chronological holdout window that matches the forecast horizon:
Train on earlier history only.
Validate on the most recent 30 days for each SKU (or use rolling-origin backtesting with 30-day windows).
Use the validation results to tune and select the model.
This change improves reliability and forecast performance assessment with little to no additional infrastructure cost; tuning and deployment choices become consistent with the 30-day-ahead requirement.
Spot Training
can lower cost but does not fix time series leakage or horizon-misaligned validation.
Shuffled k-fold CV
is generally inappropriate for time series because it mixes future and past observations across folds.
More endpoint capacity
improves inference throughput/latency, not forecasting accuracy or evaluation correctness.


## Question 48

**Topic:** ML Solution Monitoring, Maintenance, and Security

A SageMaker training job writes model artifacts to an S3 bucket that enforces SSE-KMS with a customer managed key. The job fails with:
AccessDeniedException: User: arn:aws:sts::111122223333:assumed-role/SageMakerTrainRole/... is not authorized to perform: kms:GenerateDataKey on key arn:aws:kms:us-east-1:111122223333:key/...
The role already has
s3:PutObject
to the bucket. Which action best applies the core principle of
least privilege
while fixing the failure?

- A. Attach AmazonS3FullAccess to SageMakerTrainRole
- B. Disable SSE-KMS on the S3 bucket for the model artifacts prefix
- C. Update the KMS key policy to allow SageMakerTrainRole and grant only required KMS actions scoped to that key
- D. Allow
- kms:*
- on
- *
- for SageMakerTrainRole

**Best answer: C**

**Explanation:**
The failure is in the KMS authorization path (not S3), because the role cannot call
kms:GenerateDataKey
on the specific CMK required by the bucket’s encryption setting. The least-privilege fix is to allow that role to use only the necessary KMS actions on only that key, and ensure the key policy also permits the role.
This error indicates the failing control plane is AWS KMS authorization, not S3: when an S3 bucket enforces SSE-KMS, the caller must be allowed to use the specified CMK for data key generation and encryption. With least privilege, you grant only the KMS permissions required for the workload (commonly
kms:GenerateDataKey
, plus encryption/decryption as needed) and scope them to the specific CMK ARN. Because KMS evaluates both IAM policy and the key policy, the CMK’s key policy must also allow the SageMaker execution role (directly or via an allowed principal such as the account with appropriate conditions).
The key takeaway is to fix the specific missing permission on the specific resource, rather than broadening access.
Over-broad S3 permissions
doesn’t address the missing KMS permission and expands access unnecessarily.
Wildcard KMS admin access
would likely work but violates least privilege and increases blast radius.
Disabling encryption
avoids the symptom but weakens security instead of correcting KMS authorization.


## Question 49

**Topic:** Deployment and Orchestration of ML Workflows

A team deploys a real-time fraud model to an Amazon SageMaker endpoint using a CI/CD pipeline (AWS CodePipeline) that promotes an approved model package from SageMaker Model Registry.
Release requirements:
Endpoint must remain private (in a VPC); no public access.
All stored payloads/artifacts must be encrypted with AWS KMS.
Roll back to the previously approved model within 5 minutes if business KPI or latency degrades.
Extra capacity during rollout must not exceed ~25% above steady-state for more than 30 minutes.
Which TWO rollout approaches should the team
AVOID
for this release?
(Select TWO.)

- A. Shift 5% of traffic to a new production variant with CloudWatch alarms and automatic rollback to the previous variant
- B. Keep the previous approved model package and endpoint configuration in Model Registry to enable one-click rollback in the pipeline
- C. Run a short-lived blue/green cutover using identical VPC/KMS settings, then swap the endpoint configuration and keep the prior config for rollback
- D. Use a linear rollout that increases traffic in small steps with alarms, then revert traffic weights if thresholds are breached
- E. Temporarily deploy the candidate model to a public endpoint and store request/response payloads unencrypted for faster debugging
- F. Replace the production model and route 100% of traffic immediately, planning to retrain and redeploy if issues are found

**Correct answers: E and F**

**Explanation:**
Approaches to avoid are the ones that break explicit rollout constraints. Exposing inference publicly or writing unencrypted payloads violates security requirements, and cutting over 100% at once without an immediate rollback mechanism violates the 5-minute rollback requirement. Canary, linear, and controlled blue/green patterns can meet the stated rollback and cost-cap constraints when paired with alarms and preserved prior configurations.
High-level model release strategies (canary, linear, blue/green) are acceptable when they (1) shift traffic gradually or with a controlled cutover, (2) include health/KPI alarms, and (3) keep a ready rollback target (previous model package + endpoint config) that can be restored quickly.
In this scenario, the unsafe approaches are the ones that directly violate stated requirements:
Public exposure or unencrypted payload storage breaks the VPC-only and KMS encryption constraints.
An immediate 100% cutover with “retrain/redeploy later” is not a rollback plan and cannot reliably restore service within 5 minutes.
By contrast, canary and linear deployments use traffic weighting and monitoring to limit blast radius, and a time-boxed blue/green swap can satisfy the temporary capacity constraint if the overlap window is kept short and rollback is an endpoint config revert.
Security/compliance breach
The approach that makes the endpoint public and stores payloads unencrypted violates the explicit VPC-only and KMS requirements.
No rapid rollback
The approach that shifts 100% traffic immediately and relies on retraining/redeploying cannot meet the 5-minute rollback requirement.
Controlled traffic shifting
Canary and linear rollouts are acceptable because weights can be reverted quickly based on alarms.
Safe blue/green
A time-boxed blue/green swap is acceptable when it preserves the previous configuration and stays within the temporary capacity limit.


## Question 50

**Topic:** ML Solution Monitoring, Maintenance, and Security

A team runs a nightly Amazon SageMaker Processing job that reads the full raw clickstream dataset from Amazon S3 (including many columns that are never used by the model) and writes a large intermediate dataset back to S3 before training. To reduce costs without changing model behavior, the team updates the preprocessing to select only the required feature columns, aggregates records to the granularity needed for training, and deletes the intermediate artifacts after the training job finishes.
Which core principle does this action most directly reflect?

- A. Data minimization
- B. Least privilege
- C. Defense in depth
- D. Reproducibility

**Best answer: A**

**Explanation:**
This change applies the data minimization principle: only collect, retain, and process what is necessary for the ML objective. By reducing unused columns and unnecessary intermediate outputs, the workflow lowers S3 storage and processing compute costs while preserving training inputs that matter to the model.
The core principle is data minimization: design ML pipelines to use (and keep) only the minimum data needed to meet the purpose. In the scenario, dropping unused columns and aggregating to the required granularity reduces I/O, storage footprint, and processing time, which directly lowers infrastructure costs without changing the model’s intended inputs. Deleting intermediate artifacts after use further reduces ongoing storage cost and reduces the amount of data that could be exposed in the event of an incident.
Key takeaway: cost optimization can be achieved by minimizing unnecessary data movement, storage, and processing—not just by changing instance sizes.
Reproducibility
is about repeatable runs (versioning code/data), not reducing retained/processed data.
Least privilege
focuses on IAM permissions; it doesn’t primarily address data volume and storage/compute costs.
Defense in depth
is layering security controls; it’s not the main principle behind dropping unused fields and artifacts.


## Question 51

**Topic:** ML Solution Monitoring, Maintenance, and Security

A company has deployed a fraud detection model to a SageMaker real-time endpoint. The team wants to detect feature distribution drift in production by using SageMaker Model Monitor.
They have a representative training dataset in S3 and have enabled endpoint data capture to an S3 prefix. After a scheduled monitoring run, the team sees Model Monitor artifacts written to an S3 output prefix.
Which TWO statements are correct about configuring this monitoring and interpreting the high-level outputs of the monitoring job? (Select TWO.)

- A. Model Monitor automatically derives the baseline from the first schedule run.
- B. A non-empty
- constraint_violations.json
- means baseline constraints were breached.
- C. Review
- statistics.json
- to identify which constraints failed.
- D. Use SageMaker Debugger rules to detect inference data drift.
- E. Create a baseline that outputs
- constraints.json
- and
- statistics.json
- to S3.
- F. Model Monitor reads CloudWatch Logs as the monitoring input by default.

**Correct answers: B and E**

**Explanation:**
To monitor data drift, SageMaker Model Monitor compares captured inference data to a baseline made from representative data and saved as baseline statistics and constraints in S3. The monitoring run then writes a violations artifact when the captured data no longer conforms to those baseline constraints.
SageMaker Model Monitor data quality monitoring works by establishing a baseline and then comparing production inference data to it on a schedule.
Typical setup and outputs:
Run a baseline job (often from training data) to generate
statistics.json
and
constraints.json
in S3.
Enable endpoint data capture (or otherwise provide a dataset) so the monitoring job has inference records to analyze.
Each monitoring execution writes results to S3, including a
constraint_violations.json
file that lists any violated constraints (for example, missing values, type mismatches, or distribution constraints you baselined).
Key takeaway: baseline artifacts define “normal,” and
constraint_violations.json
is the high-level signal that drift/quality constraints were violated.
OK
Create a baseline that outputs
constraints.json
and
statistics.json
to S3 — these baseline artifacts are required for comparison.
OK
A non-empty
constraint_violations.json
means baseline constraints were breached — this is the primary drift/quality indicator from a run.
NO
Model Monitor automatically derives the baseline from the first schedule run — you must provide or create baseline statistics/constraints.
NO
Use SageMaker Debugger rules to detect inference data drift — Debugger targets training diagnostics, not Model Monitor drift reports.
NO
Review
statistics.json
to identify which constraints failed — the violations are surfaced in
constraint_violations.json
.
NO
Model Monitor reads CloudWatch Logs as the monitoring input by default — it analyzes captured data (for example, from S3 data capture), not logs.


## Question 52

**Topic:** ML Solution Monitoring, Maintenance, and Security

A company runs a SageMaker real-time endpoint for credit decisions. The model’s features include PII. The security team requires that inference data remains in AWS, encrypted with a KMS key, with
no internet egress
. The application also has a strict p99 latency SLO of 200 ms.
The ML engineer must detect changes in inference data distributions over time and choose high-level analysis approaches (including SageMaker Clarify) to support drift investigation.
Which TWO actions should the engineer
AVOID
? (Select TWO.)

- A. Enable endpoint data capture to KMS-encrypted S3 for analysis
- B. Schedule Clarify bias drift runs on captured inference samples
- C. Export full inference payloads to an external SaaS for drift reporting
- D. Run Clarify SHAP explainability synchronously on every inference request
- E. Compute PSI/KS drift metrics in a scheduled SageMaker processing job
- F. Create a Model Monitor data quality baseline from training data

**Correct answers: C and D**

**Explanation:**
Detecting distribution shifts typically requires capturing inference inputs, establishing a baseline from training (or a known-good period), and running offline or scheduled comparisons. SageMaker Model Monitor and SageMaker Clarify support these analyses using batch processing and stored reports. Approaches that move PII outside AWS or that add expensive analysis to the real-time inference path violate the stated security and latency requirements.
Data distribution drift detection is usually done by comparing summary statistics or distributions of recent inference data to a baseline (often from training data). In SageMaker, a common pattern is to enable endpoint data capture to encrypted S3, then run scheduled monitoring jobs: Model Monitor for data quality drift (feature statistics/constraints) and Clarify for bias drift analysis on captured samples. You can also compute custom drift metrics (for example PSI or KS tests) in a scheduled SageMaker processing job and publish results to CloudWatch for alarming.
Actions to avoid are those that violate explicit constraints: exporting PII outside AWS breaks the no-egress requirement, and running explainability synchronously per request risks latency and unnecessary cost. Keep heavy analysis asynchronous/offline and keep captured data protected with encryption and controlled access.
PII exfiltration risk
sending full payloads to an external SaaS violates the stated no-internet-egress and data residency constraints.
Online-path overhead
computing SHAP for every request is not a drift-monitoring necessity and commonly breaks tight p99 latency targets.
Baseline + scheduled comparison
using Model Monitor and/or scheduled processing jobs fits distribution-shift detection without impacting online latency.
Clarify for bias drift
running Clarify on captured samples supports high-level drift investigation while keeping analysis asynchronous.


## Question 53

**Topic:** ML Solution Monitoring, Maintenance, and Security

A company runs an Amazon SageMaker real-time endpoint in multiple AWS accounts. They capture 1% of requests/responses for troubleshooting and need to retain
all monitoring artifacts (logs, metrics, captured payloads, and Model Monitor reports) for 7 years
for audits. Requirements:
Adding latency to inference must be minimal.
Artifacts must be immutable (WORM) and encrypted with a customer managed AWS KMS key.
The last 30 days must be quickly searchable; older data should be low cost.
CloudWatch retains metrics for only 15 months, but audits require 7 years.
Which approach BEST meets these requirements?

- A. Keep payloads, logs, and metrics only in CloudWatch with 7-year retention
- B. Centralize artifacts in SSE-KMS S3 Object Lock with lifecycle; export CloudWatch logs/metrics and write Data Capture/Model Monitor there
- C. Store payloads and metrics in DynamoDB with a 7-year TTL
- D. Write captured payloads to EFS on endpoint instances; snapshot weekly to S3

**Best answer: B**

**Explanation:**
Use an immutable, low-cost archival store for audit retention and keep recent data quickly accessible. An SSE-KMS encrypted S3 bucket with Object Lock satisfies WORM and 7-year retention, while S3 lifecycle policies control cost over time. Exporting CloudWatch logs/metrics to S3 and writing SageMaker Data Capture and Model Monitor outputs to the same bucket preserves all required monitoring artifacts.
The core requirement is audit-ready retention of monitoring artifacts (including captured inference payloads and time-series telemetry) with immutability and encryption, without adding meaningful inference latency. Amazon S3 is the audit-friendly system of record here: S3 Object Lock (WORM) plus SSE-KMS meets governance requirements, and S3 lifecycle transitions meet the “hot for 30 days, cheap afterward” requirement.
To cover all artifact types end-to-end:
Configure SageMaker endpoint Data Capture and Model Monitor to write to the centralized S3 bucket.
Export CloudWatch Logs (subscription) and CloudWatch metrics (metric streams) to S3 to satisfy the 7-year metric retention requirement.
Keeping operational views in CloudWatch for recent troubleshooting is compatible, but S3 is the durable audit store.
CloudWatch-only retention
doesn’t satisfy the stated 7-year metrics requirement (CloudWatch metrics retention is shorter).
EFS on instances
is not an appropriate immutable, centralized audit store and adds operational risk for long retention.
DynamoDB TTL for artifacts
is a poor fit and cost-inefficient for large log/payload archives and WORM retention.


## Question 54

**Topic:** ML Solution Monitoring, Maintenance, and Security

Which AWS service provides an immutable-style audit trail of AWS API activity (for example, SageMaker
CreateTrainingJob
,
CreateEndpoint
, and IAM
PassRole
) that can be stored in Amazon S3 for compliance evidence and security investigations?

- A. AWS CloudTrail
- B. Amazon SageMaker Model Monitor
- C. Amazon CloudWatch Logs
- D. AWS Config

**Best answer: A**

**Explanation:**
AWS CloudTrail is the AWS service used for auditing because it records management and data events for API calls across AWS services, including Amazon SageMaker. Those event records can be centrally stored (commonly in Amazon S3) and used as compliance evidence and for security investigations. This is distinct from operational logging and ML-specific monitoring features.
For security investigations and compliance, you typically need a reliable record of “who did what, when, and from where” in an AWS account. AWS CloudTrail provides this by capturing API activity (management events and, when enabled, selected data events) across services such as IAM, Amazon S3, and Amazon SageMaker. CloudTrail logs can be delivered to an Amazon S3 bucket (often with encryption and retention controls) and optionally sent to CloudWatch Logs for near-real-time searching and alerting. In contrast, CloudWatch Logs is primarily for ingesting and querying application/service log streams, and SageMaker monitoring features focus on ML behavior (like drift) rather than account-level API auditing.
Operational logs vs audit trail
CloudWatch Logs is for log ingestion/analysis, not the authoritative record of AWS API calls.
ML drift monitoring
SageMaker Model Monitor detects data/quality/drift issues, not IAM/SageMaker API actions.
Config state tracking
AWS Config tracks resource configuration and changes, but it is not the primary API call audit log like CloudTrail.


## Question 55

**Topic:** Deployment and Orchestration of ML Workflows

A team is implementing CI/CD for an Amazon SageMaker-based ML workflow using CodePipeline and SageMaker Pipelines. The pipeline includes stages for training, evaluation, model registration, deployment, and monitoring.
Which statement is
NOT
correct (false/unsafe) about applying CI/CD principles to this MLOps workflow on AWS?

- A. Use a model registry to version artifacts and track lineage from data and code to deployed model.
- B. Register only models that pass evaluation gates, and promote them through environments using approvals.
- C. Monitor deployed models for drift and performance, and trigger retraining or rollback workflows when thresholds are breached.
- D. Deploy the model automatically to production first, then evaluate and roll back if needed.

**Best answer: D**

**Explanation:**
In MLOps CI/CD, automation promotes only validated model artifacts. Training outputs must be evaluated against defined metrics before the model is registered and deployed, so production receives a controlled, approved version. Monitoring then closes the loop by detecting drift or degradation and triggering an automated response.
CI/CD principles for ML add explicit quality controls around model artifacts, not just application code. A typical automated flow is: train a candidate model, evaluate it against agreed metrics/baselines, register/version the model and its metadata (including lineage), deploy a specific approved version to the target environment, and continuously monitor the deployed model to detect drift or performance regression.
Deploying to production before evaluation is unsafe because it bypasses the key CI/CD gate that prevents unvalidated models from being promoted. The correct pattern is “train → evaluate → register → (approve) → deploy → monitor,” with automation and approvals appropriate to risk.
Evaluation gates before promotion
is a standard CI/CD control to prevent pushing failing models.
Model registry and lineage
supports versioning, auditability, and reproducible promotion across environments.
Monitoring-driven automation
is an expected post-deploy stage to detect drift/regressions and trigger retrain/rollback.


## Question 56

**Topic:** ML Solution Monitoring, Maintenance, and Security

A team is optimizing costs for an Amazon SageMaker workload after reviewing CloudWatch metrics.
A real-time endpoint serves a 350 MB PyTorch transformer on
ml.m5.2xlarge
; p95 latency is 450 ms and CPU is ~85%.
A nightly SageMaker Model Monitor processing job on
ml.m5.2xlarge
reads ~120 GB from Amazon S3 to compute drift statistics; it fails with out-of-memory errors while CPU stays ~20%.
Which TWO instance family changes best fit these workloads? (Select TWO.)

- A. Move the Model Monitor processing job to a memory-optimized instance
- B. Move the real-time endpoint to an inference-optimized instance (Inferentia-based)
- C. Move the real-time endpoint to a memory-optimized instance
- D. Move the real-time endpoint to a GPU training-optimized instance
- E. Move the Model Monitor processing job to a compute-optimized instance
- F. Move both workloads to newer general-purpose instances

**Correct answers: A and B**

**Explanation:**
The endpoint is CPU-bound and serving a deep learning transformer, so an inference-optimized family is the best match to improve latency per dollar for supported frameworks. The Model Monitor processing job is failing due to memory exhaustion with low CPU use, so a memory-optimized family is the best fit to prevent OOM and stabilize the nightly run.
Instance family selection should follow the dominant resource bottleneck you observe in monitoring data. For real-time inference of deep learning models, inference-optimized instances (such as Inferentia-based choices) are purpose-built to deliver better throughput/latency per cost than general-purpose CPU instances when the model/framework is supported. For batch-style monitoring jobs that crash with out-of-memory while CPU remains low, the limiting factor is RAM, so memory-optimized instances are the appropriate fit.
A practical approach is:
Use CloudWatch/endpoint metrics to identify whether inference is compute/accelerator-bound.
Use CloudWatch memory/OS logs for processing to confirm RAM pressure.
Change only the bottlenecked dimension first, then re-measure.
Compute-optimized helps when CPU is the constraint; it does not fix RAM-driven failures.
OK
Move the endpoint to inference-optimized: targets deep learning serving efficiency and lowers latency/cost.
OK
Move the monitoring processing job to memory-optimized: increases available RAM to eliminate OOM.
NO
Move the endpoint to memory-optimized: adds RAM but doesn’t address inference acceleration/throughput.
NO
Move the processing job to compute-optimized: increases CPU but leaves the memory bottleneck.
NO
Move both to general-purpose: may be incremental, but doesn’t directly solve the specific bottlenecks.
NO
Move the endpoint to a GPU training-optimized instance: typically overprovisioned and cost-inefficient for steady inference needs.


## Question 57

**Topic:** Data Preparation for Machine Learning (ML)

A team is preparing a customer churn dataset for training in Amazon SageMaker. The data is stored in a private S3 bucket encrypted with SSE-KMS and contains PII. Company policy requires that PII must not leave the AWS account, and the team must preserve the integrity of an existing train/validation/test split for unbiased evaluation.
Which TWO data cleaning steps should the team
AVOID
?

- A. Deduplicate by customer_id keeping the latest event_time
- B. Add missing-value indicator features for sparse columns
- C. Fit imputers on the training split and apply to others
- D. Cap outliers using IQR thresholds computed on training data
- E. Export raw PII to a third-party tool for deduplication
- F. Impute using statistics computed from all dataset splits

**Correct answers: E and F**

**Explanation:**
The steps to avoid are the ones that break explicit constraints: protecting PII and preventing data leakage across the existing train/validation/test split. Computing imputation statistics across all splits contaminates the holdout sets and biases evaluation. Sending raw PII outside the AWS account violates the security requirement.
Good data cleaning improves training quality only if it preserves security boundaries and the validity of model evaluation. In this scenario, two explicit rules apply: PII must stay inside the AWS account, and the existing train/validation/test split must remain a true holdout.
Practical guardrails:
Compute any data-driven transforms (imputation values, outlier caps) using the training split only, then apply the fitted transform to validation/test.
Keep deduplication and other cleaning inside AWS services (for example, SageMaker Processing/Data Wrangler, AWS Glue) against data stored in private, encrypted S3.
A common failure mode is “peeking” at validation/test when choosing cleaning parameters, which silently turns the evaluation into a training artifact.
Leakage via preprocessing
is introduced when imputation is fit using all splits, making metrics unreliable.
PII exfiltration
occurs if raw data is sent to a third-party dedup tool, violating the stated policy.
Train-only fitting
for imputers and IQR thresholds is acceptable because it preserves holdout integrity.
In-account dedup and missingness indicators
are acceptable cleaning steps that can improve signal without breaking the constraints.


## Question 58

**Topic:** Deployment and Orchestration of ML Workflows

A team deploys a fraud model to a production Amazon SageMaker real-time endpoint. The team wants to reduce release risk by enforcing model versioning, artifact immutability, and controlled promotion. Requirements:
Each production deployment must reference an immutable model artifact and container image so it can be rolled back exactly.
A human approval step is required before any model is promoted to production.
Deployments must be performed by an automated pipeline (not ad hoc changes).
Which TWO deployment practices should the team
AVOID
?

- A. Allow training role to update production endpoint automatically
- B. Use Model Registry approvals; deploy specific model package version
- C. Overwrite the S3 object at a fixed model artifact key
- D. Create new EndpointConfig; shift traffic with blue/green policy
- E. Enable S3 versioning; deploy using specific object VersionId
- F. Record ECR image digest and immutable tag in Model Package

**Correct answers: A and C**

**Explanation:**
Avoid practices that mutate release artifacts in place or bypass gated promotion. Immutable, versioned artifacts (S3 version IDs and image digests) plus an approval-controlled pipeline enable repeatable deployments and exact rollback. Direct, automated promotion from training to production violates controlled promotion requirements.
Safe model releases on SageMaker rely on deploying
immutable, uniquely versioned
artifacts and promoting them through a controlled process. If you overwrite a model artifact at a fixed S3 key, the endpoint may still “point” to the same location while the bytes change, making audits and rollbacks ambiguous. Similarly, letting the training role update the production endpoint directly removes the required approval gate and mixes build and release responsibilities.
A typical low-risk pattern is:
Register a new model package version (including S3 artifact version ID and image digest).
Require approval in the Model Registry (or a manual approval action in CI/CD).
Deploy via a pipeline using a new
EndpointConfig
, then shift traffic (canary/blue-green) with fast rollback.
The key takeaway is that promotion should be gated, and references should be to immutable versions—not mutable “latest” locations.
Registry-based promotion
is acceptable because it provides versioning plus an approval gate before production.
Blue/green traffic shifting
is acceptable because it reduces blast radius and supports quick rollback.
Pinning image digests and S3 version IDs
is acceptable because it guarantees artifact immutability for repeatable releases.


## Question 59

**Topic:** Deployment and Orchestration of ML Workflows

A team uses Amazon SageMaker Model Registry to manage model versions for a production real-time endpoint. They want a manual approval gate so that a newly trained model is reviewed before any deployment occurs through their CI/CD pipeline.
Which statement is
INCORRECT
about implementing this workflow?

- A. Marking a model package as
- Approved
- automatically updates any endpoints using the model package group to the new version.
- B. Set new model packages to
- PendingManualApproval
- and require a reviewer to change them to
- Approved
- before promotion.
- C. Restrict
- sagemaker:UpdateModelPackage
- to an approver role and rely on CloudTrail for auditability of approval actions.
- D. Use an EventBridge rule for SageMaker model package state changes to trigger a deployment pipeline when a package becomes
- Approved
- .

**Best answer: A**

**Explanation:**
In SageMaker Model Registry, approval status is a governance control that gates promotion, but it does not perform deployments by itself. You must wire approval-state changes to an explicit deployment action (for example, a pipeline stage) to update staging or production endpoints.
SageMaker Model Registry provides a centralized place to version models (model packages) and apply an approval gate using
ModelApprovalStatus
(for example,
PendingManualApproval
to
Approved
). The approval state is used to control
when
automation is allowed to promote a model, but it does not automatically modify running infrastructure.
A typical high-level promotion flow is:
Training pipeline registers a new model package version in a model package group as
PendingManualApproval
.
A reviewer updates the model package to
Approved
.
Automation (for example, EventBridge
CodePipeline/Step Functions) detects the approval and runs deployment steps to create/update the endpoint.
Key takeaway: approval is a gate; promotion/deployment still requires an orchestrated action.
Manual approval gate
using
PendingManualApproval
/
Approved
is the standard way to block promotion until review.
Event-driven CI/CD
is valid because model package state-change events can start an automated deployment workflow.
Least-privilege approvals
are recommended by limiting who can update approval status and auditing via CloudTrail.
Auto-deploy misconception
fails because approval does not automatically update endpoints; a pipeline must perform the deployment.


## Question 60

**Topic:** Data Preparation for Machine Learning (ML)

A dataset has a nominal categorical feature
customer_id
with tens of thousands of unique values. The feature must be used by a linear model, and the encoding should avoid creating an extremely wide one-hot matrix.
Which encoding technique is most appropriate?

- A. Label encoding
- B. No encoding (pass the string values)
- C. Binary encoding
- D. One-hot encoding

**Best answer: C**

**Explanation:**
Binary encoding is designed for high-cardinality categorical features because it represents each category using multiple binary digits, producing far fewer columns than one-hot encoding. This avoids the extreme dimensionality and sparsity that one-hot encoding creates for tens of thousands of categories. It also avoids using a single integer code that can imply a misleading order.
For nominal categorical features, the main trade-off is between representational fidelity and feature dimensionality.
One-hot encoding is typically preferred for low-cardinality nominal features because it does not introduce any artificial ordering, but it scales poorly as cardinality grows (it creates one column per category). Label encoding maps categories to a single integer column, which can introduce an arbitrary numeric ordering that is not meaningful for nominal data and can mislead many model types.
Binary encoding is a practical compromise for high-cardinality features: it converts the category index into a binary representation spread across multiple columns, drastically reducing the number of generated features compared with one-hot while still representing distinct categories.
When cardinality is very large and the model is sensitive to input dimensionality, binary encoding is often the better fit than one-hot.
One-hot for high cardinality
creates an extremely wide sparse matrix with one column per unique
customer_id
.
Single integer codes
can inject an arbitrary ordering into a nominal feature.
Raw strings as features
are not directly usable by typical linear model training algorithms without transformation.


## Question 61

**Topic:** ML Solution Monitoring, Maintenance, and Security

A team runs Amazon SageMaker Pipelines that (1) reads features from an Amazon RDS database and (2) calls a third-party labeling API during a processing step. The team must manage these credentials securely and support periodic rotation.
Which statement is
INCORRECT
?

- A. Use IAM (and the secret’s KMS key policy) to restrict who can read secrets, and rely on CloudTrail to audit secret access.
- B. Configure Secrets Manager rotation (for example, with a Lambda rotation function) so applications retrieve the current secret value without code changes.
- C. Embed the third-party API token in the training container image or source repository to keep deployments simple.
- D. Store the RDS credentials in AWS Secrets Manager and allow the pipeline’s execution role to call
- secretsmanager:GetSecretValue
- at runtime.

**Best answer: C**

**Explanation:**
The unsafe statement is the one that recommends embedding an API token in a container image or source repository. Secrets should be stored in a managed secrets service (such as AWS Secrets Manager) and retrieved at runtime using least-privilege IAM permissions. This supports rotation, reduces accidental exposure, and improves auditability.
For ML workflows, credentials for data sources (for example, RDS) and third-party integrations (for example, labeling APIs) should be kept out of code, images, and configuration files that are broadly readable. AWS Secrets Manager (or an equivalent managed store) is designed for storing secrets encrypted with AWS KMS, controlling access with IAM, auditing access with AWS CloudTrail, and supporting automated rotation.
A secure pattern is:
Store each credential as a secret and encrypt it with KMS.
Grant only the SageMaker job/pipeline role permission to read the specific secret.
Retrieve the secret value at runtime (and cache only as needed).
Enable rotation where supported so the current value is fetched without redeploying artifacts.
The key takeaway is to avoid hardcoding credentials anywhere they could be copied, scanned, or baked into immutable artifacts.
Runtime secret retrieval
is an approved pattern when IAM permissions are scoped to the specific secret.
Secrets rotation
is a core reason to use Secrets Manager for credentials that must change periodically.
Least privilege and auditing
with IAM/KMS controls and CloudTrail supports compliance and incident response.


## Question 62

**Topic:** ML Model Development

A binary classifier deployed on Amazon SageMaker returns a fraud probability score from 0 to 1. The business will automatically block orders when the score exceeds an operating threshold.
Blocking a legitimate order (false positive) costs $200 in lost margin. Allowing a fraudulent order (false negative) costs $20. Which threshold choice best fits these constraints?

- A. Increase regularization strength to reduce overfitting
- B. Increase the threshold to favor higher precision
- C. Use SageMaker Model Monitor to detect drift and keep a 0.5 threshold
- D. Decrease the threshold to favor higher recall

**Best answer: B**

**Explanation:**
An operating (decision) threshold converts a model’s probability score into a class label. When false positives are much more expensive than false negatives, the business objective is to reduce false positives, which is achieved by raising the threshold. This increases precision while typically decreasing recall.
The operating (decision) threshold is the probability cutoff used to map a classifier’s score to a positive/negative prediction. Changing the threshold does not change the trained model; it changes the precision–recall tradeoff.
Given the stated costs, false positives are far more expensive than false negatives, so the deployment should be tuned to avoid predicting “fraud” unless the model is very confident.
Raise the threshold - fewer positives predicted
Fewer false positives - higher precision
More missed fraud cases - lower recall
Model-quality techniques (regularization, tuning) and operational tools (drift monitoring) are important, but they do not directly implement the business-driven precision vs. recall choice at inference time.
Lower threshold
increases recall but would increase costly false positives under these business costs.
Regularization
changes training behavior/generalization, not the inference-time cutoff decision.
Model Monitor
helps detect data/model drift, but it does not choose the precision/recall operating point by itself.


## Question 63

**Topic:** Data Preparation for Machine Learning (ML)

A data science team is preparing a binary classification dataset in Amazon SageMaker. They need an AWS-native way to identify potential bias in the training data and to produce high-level bias metrics (for example, comparing outcomes across sensitive groups) before training a model.
Which SageMaker capability is designed for this purpose?

- A. SageMaker Debugger
- B. SageMaker Data Wrangler
- C. SageMaker Clarify
- D. SageMaker Model Monitor

**Best answer: C**

**Explanation:**
Amazon SageMaker Clarify provides bias detection for ML datasets and models. It can compute bias-related metrics by comparing outcomes across sensitive attributes, helping teams assess potential bias in training data before model development.
SageMaker Clarify is the SageMaker feature specifically intended to help identify potential bias and explainability concerns. For bias, it analyzes a dataset (such as a training set) by using a chosen sensitive attribute and label/outcome to compute bias metrics that summarize how outcomes differ across groups. This makes it suitable for a pre-training data preparation step to evaluate bias at a high level and decide whether additional data collection, rebalancing, or feature changes are needed.
Other SageMaker features focus on data transformation, monitoring production drift/quality, or debugging training behavior rather than quantifying dataset bias metrics.
Data preparation tooling
transforms and profiles data but is not the SageMaker bias-metrics feature.
Production monitoring
targets data/model quality and drift after deployment, not pre-training dataset bias evaluation.
Training diagnostics
help find training issues (like overfitting or performance bottlenecks), not bias metrics.


## Question 64

**Topic:** Deployment and Orchestration of ML Workflows

A team must implement CI/CD for an Amazon SageMaker-based ML project. When code changes, the workflow must automatically build artifacts, run a repeatable training pipeline, register the resulting model, and deploy the approved model to an endpoint with minimal manual steps.
Which actions should the team take to meet these requirements? (Select THREE.)

- A. Use AWS Glue interactive sessions to deploy models directly to endpoints
- B. Use Amazon SageMaker Pipelines to orchestrate processing, training, and evaluation steps
- C. Use SageMaker Model Registry approval to trigger deployment to a SageMaker endpoint
- D. Send Amazon SNS notifications so engineers start each step manually
- E. Run training from SageMaker Studio notebooks whenever code changes
- F. Use AWS CodePipeline and CodeBuild to build/push images to Amazon ECR

**Correct answers: B, C and F**

**Explanation:**
Use CI/CD orchestration to automatically build artifacts, execute a managed ML workflow, and promote models into deployment. CodePipeline/CodeBuild automate build and packaging, SageMaker Pipelines automates the train/evaluate workflow, and SageMaker Model Registry enables approval-based promotion and deployment. Together, these minimize manual steps while keeping deployments repeatable and auditable.
The goal is an automated, repeatable path from code commit to deployed model. In AWS, a common pattern is to use CI/CD tooling for build automation, SageMaker-native workflow orchestration for training, and a registry/approval mechanism for controlled promotion to deployment.
A high-level automated flow is:
Code changes trigger CodePipeline.
CodeBuild builds containers/artifacts and publishes to ECR/S3.
SageMaker Pipelines runs processing training evaluation and produces a model artifact.
The pipeline registers a model package in SageMaker Model Registry.
An approval (automatic or manual) gates deployment of the approved model package to an endpoint.
This achieves end-to-end orchestration without relying on humans to kick off each stage.
OK
Use CodePipeline and CodeBuild to build/push images to ECR: provides automated build and release orchestration on commits.
OK
Use SageMaker Pipelines to orchestrate processing, training, and evaluation: managed workflow for repeatable ML pipelines.
OK
Use Model Registry approval to trigger deployment: supports controlled promotion and automated deployment actions.
NO
Running Studio notebooks is not a CI/CD orchestration mechanism and is inherently manual.
NO
SNS notifications that require engineers to start steps breaks the “minimal manual steps” requirement.
NO
Glue interactive sessions are for data processing/ETL, not direct SageMaker endpoint deployment orchestration.


## Question 65

**Topic:** Deployment and Orchestration of ML Workflows

A team uses Amazon SageMaker Pipelines to retrain a fraud model using new Parquet files (about 50 GB/day) in Amazon S3 and to register approved models in SageMaker Model Registry. Requirements:
Retraining must run every night at 1:00 AM UTC.
Retraining must also run automatically when a new labeled data file is uploaded to an S3 prefix.
Runs must be auditable with AWS-native logs and use least-privilege IAM roles (no long-lived servers).
The solution should minimize operational overhead and cost.
Which approach BEST meets these requirements?

- A. Run a continuously running EC2 cron job that starts the pipeline
- B. Use an S3 lifecycle rule to invoke the pipeline nightly
- C. Use a CodeCommit webhook as the only trigger for pipeline runs
- D. Use EventBridge schedule and S3 events to start the pipeline

**Best answer: D**

**Explanation:**
Use Amazon EventBridge rules to trigger SageMaker Pipelines for both scheduled and event-driven runs. An EventBridge schedule can start a nightly execution, and an EventBridge S3 object-created event can start execution when new labeled data arrives. This is serverless, auditable, and works with least-privilege IAM roles.
For high-level pipeline automation on AWS, Amazon EventBridge is the standard service to define both time-based schedules and event-driven triggers. In this scenario, you can create two EventBridge rules: a cron rule for 1:00 AM UTC and an event pattern rule that matches S3
Object Created
events for the labeled-data prefix. Each rule targets either SageMaker directly (where supported) or a small Lambda/Step Functions target that calls
sagemaker:StartPipelineExecution
using a least-privilege IAM role.
This approach is fully managed (no always-on compute), produces audit trails via CloudTrail/EventBridge/Lambda logs, and cleanly supports both trigger types for the same SageMaker Pipeline.
Always-on scheduler
running an EC2 cron job violates the cost/ops requirement for no long-lived servers.
S3 lifecycle rules
are for object management/expiration and are not an event trigger for SageMaker Pipeline executions.
Repo-only webhook
triggers on code changes, not nightly schedules or S3 data arrival, so it misses required triggers.
Continue in the web app
Use IT Mastery for interactive AWS MLA-C01 practice with mixed sets, timed mocks, topic drills, explanations, and progress tracking.
Try AWS MLA-C01 on Web
Focused topic pages
ML Data Prep
ML Model Development
ML Deployment
ML Monitoring
Share this guide:
Copy link
·
LinkedIn
·
Email
·
Reddit
ML Monitoring
Official Resources
Browse Certification Practice Tests by Exam Family
AIF-C01
Quick Review
Study Plan
Exam Blueprint
Scenario Guide
Quick Reference
Fundamentals of AI and ML
Fundamentals of GenAI
Applications of Foundation Models
Guidelines for Responsible AI
AI Security and Governance
Free Practice Exam
Official Resources
AIP-C01
Quick Review
Study Plan
Exam Blueprint
Scenario Guide
Quick Reference
FM Integration and Data
Implementation and Integration
AI Safety, Security, and Governance
GenAI Operations
Testing, Validation, and Troubleshooting
Free Practice Exam
Official Resources
CLF-C02
Quick Review
Study Plan
Exam Blueprint
Scenario Guide
Quick Reference
Cloud Concepts
Security and Compliance
Cloud Technology and Services
Billing, Pricing, and Support
Free Practice Exam
Official Resources
DEA-C01
Quick Review
Study Plan
Exam Blueprint
Scenario Guide
Quick Reference
Data Ingestion and Transformation
Data Store Management
Data Operations and Support
Data Security and Governance
Free Practice Exam
Official Resources
MLA-C01
Quick Review
Study Plan
Exam Blueprint
Scenario Guide
Quick Reference
ML Data Prep
ML Model Development
ML Deployment
ML Monitoring
Free Practice Exam
Official Resources
SAA-C03
Quick Review
Study Plan
Exam Blueprint
Scenario Guide
Quick Reference
Design Secure Architectures
Design Resilient Architectures
Design High-Performing Architectures
Design Cost-Optimized Architectures
Free Practice Exam
Official Resources
SCS-C03
Quick Review
Study Plan
Exam Blueprint
Scenario Guide
Quick Reference
Detection
Incident Response
Infrastructure Security
Identity and Access Management
Data Protection
Security Foundations and Governance
Free Practice Exam
Official Resources
SOA-C03
Quick Review
Study Plan
Exam Blueprint
Scenario Guide
Quick Reference
Monitoring and Optimization
Reliability and Business Continuity
Deployment, Provisioning, and Automation
Security and Compliance
Networking and Content Delivery
Free Practice Exam
Official Resources
Mastery Exam Prep
Mastery by Tokenizer provides text-first, practice-driven exam study tools with realistic exam-style questions and clear explanations for focused, self-paced learning.
Navigate
Support
FAQ
Methodology
Why Mastery
Author Page
About Us
For Organizations
Institutional Access
Training Providers
Referral Partners
Legal
Privacy Policy
Terms of Service
Trademarks & Disclaimer
Get the Apps
Finance Prep
— Finance, accounting, licensing & insurance
App Store (Canada)
·
App Store (U.S.)
·
Google Play (Canada)
·
Google Play (U.S.)
·
Web App
·
Login
·
Finance exam pages
IT Mastery
— Cloud & IT certifications
App Store
·
Google Play
·
Web App
·
Login
PM Mastery
— Project management & agile
App Store
·
Google Play
·
Web App
·
Login
Use these links for web access, subscriber login, and mobile installs. Each app family supports web and mobile access with the same app-family account.
© 2026
Tokenizer Inc.
(Canada) · Built by Tokenizer

