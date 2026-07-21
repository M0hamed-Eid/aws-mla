# ExamLab - AWS MLA-C01 Practice (20 Questions)

Source: https://examlab.net/en/certs/aws/mla-c01/practice

---

## Question 1

**Domain:** data-preparation-for-ml
**Topic:** ML Data Quality, Integrity, and Labeling

A regulated team needs to label medical images that cannot leave the company's compliance perimeter. Public crowdsourced workers are not acceptable. Which Ground Truth workforce option fits?

- A. Private workforce, where labeling is performed by employees you authorise via Cognito user pools, keeping data inside your environment.
- B. Mechanical Turk public workforce.
- C. Third-party vendor marketplace.
- D. Random S3 cross-account replication.

**Correct Answer:** A

**Explanation:**
<p>Ground Truth offers three workforce types: public (Mechanical Turk), vendor (curated third-party providers), and private (your own employees, authenticated via Cognito user pools). Private workforces are the only choice when data must remain inside the organisation - common for medical, legal, and financial use cases. The MLA-C01 lesson is to map workforce type to data-sensitivity and compliance constraints.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Public workforce is hiring random freelancers off the street; private workforce is your own trusted team in your office. For sensitive medical files, only the trusted team can see the data.</p>
<h3 id="option-analysis636">Option Analysis:636</h3>
<ul>
<li><strong>A</strong>：Correct - private workforces keep sensitive data inside the company.</li>
<li><strong>B</strong>：Wrong - Mechanical Turk is public crowdsourcing.</li>
<li><strong>C</strong>：Wrong - vendor workforces are external companies.</li>
<li><strong>D</strong>：Wrong - replication is not a labeling option.</li>
</ul>

## Question 2

**Domain:** deployment-and-orchestration-of-ml-workflows
**Topic:** ML Deployment Strategies — A/B Testing, Shadow, and Blue/Green

A team has been running a SageMaker real-time endpoint on ml.m5.2xlarge for six months. Cost-optimisation review shows the model would run within the latency budget on ml.c6i.xlarge at half the per-hour cost. The team must switch instance types with no end-user-visible downtime. Which SageMaker capability lets them roll the endpoint to a new instance type without dropping in-flight requests?

- A. Delete the endpoint and recreate it with the new EndpointConfig.
- B. Update the endpoint with a new EndpointConfig using UpdateEndpoint, which performs a blue/green-style rolling deployment.
- C. Modify the underlying EC2 instance type via the EC2 console.
- D. Use Application Auto Scaling to swap instance types automatically based on cost.

**Correct Answer:** B

**Explanation:**
<p>The SageMaker UpdateEndpoint API performs a blue/green-style rolling deployment when given a new EndpointConfig. AWS provisions the new fleet (with the new instance type, container, or model variant), waits for it to pass health checks, then shifts traffic from the old fleet to the new fleet, and finally tears down the old fleet. In-flight requests on the old fleet drain gracefully. This is the canonical zero-downtime deployment mechanism for SageMaker endpoints, and it is the right answer for instance-type changes, container image upgrades, and model artifact updates.</p>
<p>SageMaker further offers deployment guardrails — linear and canary traffic shifting, plus auto-rollback on CloudWatch alarms — for advanced rollout strategies. Deleting the endpoint and recreating it drops all in-flight requests. Endpoint instances are not user-managed EC2 — you cannot edit them in the EC2 console. Auto-scaling adjusts count, not type. UpdateEndpoint is the correct mechanism.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Picture changing a restaurant's kitchen team without closing the doors. You hire the new team, train them in a parallel kitchen, then start sending tickets to the new kitchen while the old team finishes their last orders. UpdateEndpoint is that handover. Deleting and recreating is closing the restaurant.</p>
<h3 id="option-analysis1313">Option Analysis:1313</h3>
<ul>
<li><strong>A</strong>：Wrong — delete plus recreate drops in-flight requests and creates a downtime window.</li>
<li><strong>B</strong>：Correct — UpdateEndpoint with a new EndpointConfig executes blue/green deployment with no in-flight loss.</li>
<li><strong>C</strong>：Wrong — endpoint instances are not user-managed EC2; the EC2 console cannot modify them.</li>
<li><strong>D</strong>：Wrong — Application Auto Scaling adjusts count, not instance type.</li>
</ul>

## Question 3

**Domain:** ml-solution-monitoring-maintenance-and-security
**Topic:** SageMaker Role Manager and Least-Privilege Patterns

A team's compliance auditor notices a SageMaker role with a permission boundary that is empty, rendering the boundary useless. Which IAM evaluation behaviour BEST explains this?

- A. An empty permissions boundary effectively allows nothing — the role's effective permissions become an empty set, and any API call by the role fails with AccessDenied even though the role's identity-based policies grant it.
- B. An empty permissions boundary grants AdministratorAccess
- C. Permissions boundaries cannot be empty
- D. Boundaries are ignored if the role has identity-based policies

**Correct Answer:** A

**Explanation:**
<p>Permissions boundaries cap the maximum effective permissions of a role. The effective set is the intersection of the identity-based Allow set and the boundary's Allow set. If the boundary's Allow set is empty (no statements or all statements have null effect), the intersection is empty, and the role can do nothing — regardless of how broad its identity-based policies are. This is a common misconfiguration where an admin attaches a boundary they assume is permissive but is actually empty. The MLA-C01 exam tests this nuance.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>A boundary is the lowest of two ceilings. If one ceiling is on the floor, the room is unusable no matter how tall the other ceiling is. An empty boundary is exactly that floor-level ceiling.</p>
<h3 id="option-analysis754">Option Analysis:754</h3>
<ul>
<li><strong>A</strong>：Correct because an empty boundary collapses effective permissions to nothing.</li>
<li><strong>B</strong>：Wrong because an empty boundary does not grant Admin.</li>
<li><strong>C</strong>：Wrong because boundaries can be (semantically) empty.</li>
<li><strong>D</strong>：Wrong because boundaries are always evaluated together with identity-based policies.</li>
</ul>

## Question 4

**Domain:** data-preparation-for-ml
**Topic:** SageMaker Data Wrangler and Feature Engineering

A team's Data Wrangler flow needs to scale a numerical feature to zero mean and unit variance for a downstream linear model. Which built-in transform is the right choice?

- A. Process Numeric -> Standard Scaler (StandardScaler), which centres at 0 with unit variance.
- B. One-Hot Encode the numeric column.
- C. Drop the numeric column.
- D. Convert numeric to string and hash.

**Correct Answer:** A

**Explanation:**
<p>Data Wrangler's Process Numeric transform group includes StandardScaler (zero-mean unit-variance), MinMaxScaler (0-1 range), RobustScaler (median/IQR), and MaxAbsScaler. Linear models, distance-based models (k-NN), and gradient-based optimisers benefit from feature scaling. StandardScaler matches the requirement exactly. Distractors mix up scaling with encoding or destruction. The MLA-C01 lesson is to know StandardScaler by name as the zero-mean unit-variance built-in.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>StandardScaler is the universal-units converter - it puts every numeric column on the same '0 mean, 1 spread' yardstick so the model treats them fairly.</p>
<h3 id="option-analysis661">Option Analysis:661</h3>
<ul>
<li><strong>A</strong>：Correct - StandardScaler centres at 0 with unit variance.</li>
<li><strong>B</strong>：Wrong - One-Hot Encode is for categorical.</li>
<li><strong>C</strong>：Wrong - dropping loses signal.</li>
<li><strong>D</strong>：Wrong - hashing numbers destroys magnitude.</li>
</ul>

## Question 5

**Domain:** data-preparation-for-ml
**Topic:** ML Data Quality, Integrity, and Labeling

An ML Engineer is preparing a customer-support dataset for a fine-tuning job and is told the data may contain PII (names, emails, phone numbers). Which AWS service is purpose-built to discover and classify PII in S3 datasets before training?

- C. Amazon Macie, which uses managed data identifiers and machine learning to automatically discover and classify sensitive data, including PII, in S3 buckets.
- A. AWS WAF.
- B. Amazon Inspector.
- D. AWS Shield.

**Correct Answer:** C

**Explanation:**
<p>Amazon Macie is AWS's managed sensitive-data discovery service. It scans S3 buckets, uses managed and custom data identifiers (regex + ML-based), and produces findings that classify objects as containing PII (names, emails, SSNs, credit cards) or other sensitive content. ML Engineers use Macie before fine-tuning to detect PII contamination, then redact or remove offending records. WAF, Inspector, and Shield are unrelated. The MLA-C01 lesson is to map PII discovery to Macie.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Macie is the security guard who walks through your S3 warehouse and reads every label looking for personal information. WAF is the bouncer at the door; Macie is the inventory inspector.</p>
<h3 id="option-analysis699">Option Analysis:699</h3>
<ul>
<li><strong>A</strong>：Wrong - WAF is for HTTP traffic, not S3 data scanning.</li>
<li><strong>B</strong>：Wrong - Inspector scans EC2/containers for CVEs.</li>
<li><strong>C</strong>：Correct - Macie is AWS's managed S3 PII discovery.</li>
<li><strong>D</strong>：Wrong - Shield is DDoS protection.</li>
</ul>

## Question 6

**Domain:** data-preparation-for-ml
**Topic:** ML Data Ingestion and Storage Patterns

An IoT analytics group is designing two parallel ML pipelines on top of the same telemetry stream from 100,000 connected devices. Pipeline A drives a real-time anomaly-detection model that must respond within one second when a device misbehaves, raising a Lambda-triggered alarm. Pipeline B is a nightly retraining pipeline that needs raw events archived to S3 in Parquet format, partitioned by hour, with no real-time consumer logic required. The team wants to use the most appropriate AWS streaming service for each pipeline and avoid forcing a single service to serve both patterns badly. Which combination correctly maps the two pipelines to AWS services?

- A. Pipeline A on Amazon Kinesis Data Streams with a Lambda consumer; Pipeline B on Amazon Data Firehose to S3 with Parquet conversion.
- B. Both pipelines should use Amazon Data Firehose to S3, with Pipeline A reading from S3 every second.
- C. Pipeline A on Amazon Data Firehose with custom Lambda transformations; Pipeline B on Amazon Kinesis Data Streams with a custom KCL consumer.
- D. Both pipelines should use Amazon Kinesis Data Streams; Pipeline B writes directly from KCL into S3 with no buffering.

**Correct Answer:** A

**Explanation:**
<p>The two pipelines have opposite latency profiles, and AWS provides two services intentionally tuned to each. Pipeline A's one-second response SLA places it in Kinesis Data Streams territory: producers write to KDS, a Lambda or KCL consumer reads with sub-second latency, scores or alerts, and the SLA is met. Pipeline B's nightly batch-archive use case maps cleanly onto Amazon Data Firehose, which buffers up to 60 seconds or 128 MB, automatically converts JSON to Parquet via the built-in format converter, and partitions by event time on the way into S3 — no consumer code, no replay logic, no operational burden.</p>
<p>The canonical 'tee' architecture is producers → KDS → fan out to (1) Lambda consumer for the real-time path, (2) Firehose subscribed to the same KDS stream for the archive path. Mapping both pipelines onto Firehose breaks Pipeline A's SLA because Firehose cannot deliver under 60 seconds. Inverting the mapping (Firehose for real-time, KDS for archive) is wrong on both counts. Building a custom KCL writer rebuilds Firehose's batching and Parquet conversion in code, which is operational waste. The MLA-C01 stem signal 'real-time inference plus archive to S3' is the KDS+Firehose tee signature.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Pipeline A is a fire alarm — when the smoke detector goes off, the bell must ring within seconds. Pipeline B is the maintenance log — yesterday's alarm history can be tabulated calmly the next morning. You would not bolt the bell onto the maintenance log printer (Firehose) or hand-build a maintenance log out of bell wires (custom KDS-to-S3). Use the right tool for each.</p>
<h3 id="option-analysis1621">Option Analysis:1621</h3>
<ul>
<li><strong>A</strong>：Correct because KDS handles Pipeline A's sub-second SLA and Firehose efficiently buffers and converts Pipeline B's archive writes.</li>
<li><strong>B</strong>：Wrong because Firehose's 60-second buffering makes a one-second SLA impossible.</li>
<li><strong>C</strong>：Wrong because this inverts the right mapping; Firehose cannot meet the SLA and KCL is unnecessary for archive-only.</li>
<li><strong>D</strong>：Wrong because building a custom KCL writer reinvents Firehose with no benefit for archive-only writes.</li>
</ul>

## Question 7

**Domain:** ml-model-development
**Topic:** Automatic Model Tuning — Bayesian and Hyperparameter Optimization

An ML Engineer wants to specify an XGBoost hyperparameter named max_depth as a search dimension in SageMaker AMT. The valid range is integers from 3 to 12 and the engineer expects most useful values to be near 6-10. Which parameter type and configuration is appropriate?

- D. Use IntegerParameter(min_value=3, max_value=12) so AMT samples integer values within the range.
- A. Use ContinuousParameter(3.0, 12.0) and round the value in the script.
- B. Use CategoricalParameter listing only [6, 8, 10].
- C. Hard-code max_depth=8 and skip tuning it.

**Correct Answer:** D

**Explanation:**
<p>AMT supports three hyperparameter types: ContinuousParameter (floats with optional log scaling), IntegerParameter (integers), and CategoricalParameter (discrete unordered choices). max_depth is an integer hyperparameter, so IntegerParameter with min=3 and max=12 is the correct choice. ContinuousParameter would sample floats and require manual rounding, which corrupts the surrogate. CategoricalParameter restricted to a few values defeats search. Hard-coding excludes the dimension. The MLA-C01 lesson is to know all three parameter types and to match them to hyperparameter semantics.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>IntegerParameter is the right tool because max_depth is a whole number (you cannot have a tree of depth 6.7). Continuous would require manual rounding that breaks AMT's mathematical model.</p>
<h3 id="option-analysis811">Option Analysis:811</h3>
<ul>
<li><strong>A</strong>：Wrong - ContinuousParameter samples floats unsuitable for integer-only hyperparameters.</li>
<li><strong>B</strong>：Wrong - hand-picked categorical values restrict search artificially.</li>
<li><strong>C</strong>：Wrong - hard-coding excludes the dimension entirely.</li>
<li><strong>D</strong>：Correct - IntegerParameter with min and max samples integer values within the range.</li>
</ul>

## Question 8

**Domain:** ml-model-development
**Topic:** Automatic Model Tuning — Bayesian and Hyperparameter Optimization

A team configures AMT and finds that the best trial achieves 0.84 AUC, while a colleague's manual exploration on the same dataset reached 0.86 AUC. Which most likely explanation should the ML Engineer investigate first?

- B. Hyperparameter ranges are too narrow or off-centre, missing the region the colleague found; widen ranges or warm-start from the colleague's settings.
- A. AMT is broken and should be replaced with grid search.
- C. Bayesian search cannot match manual exploration on any problem.
- D. Increase max_parallel_jobs to fix the result.

**Correct Answer:** B

**Explanation:**
<p>When AMT underperforms manual tuning, the most common cause is hyperparameter range mis-specification. The colleague's region of strong values may be outside or near the boundary of AMT's configured ranges. Fixes include widening ranges, re-centering them on the colleague's known-good values, or using warm-start to seed AMT with prior trial information. Replacing AMT with grid search or blaming Bayesian categorically misdiagnoses the problem. Increasing parallelism does not fix range mis-specification. The MLA-C01 lesson is to debug AMT under-performance by inspecting ranges first.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>If your fishing rod cannot reach the spot where fish are biting, the rod is not broken - the line is too short. Widening hyperparameter ranges is lengthening the line.</p>
<h3 id="option-analysis791">Option Analysis:791</h3>
<ul>
<li><strong>A</strong>：Wrong - AMT is reliable; the issue is range specification.</li>
<li><strong>B</strong>：Correct - widen or recenter ranges to cover the colleague's region.</li>
<li><strong>C</strong>：Wrong - Bayesian usually matches or exceeds manual tuning when ranges are correct.</li>
<li><strong>D</strong>：Wrong - parallelism does not fix range mis-specification.</li>
</ul>

## Question 9

**Domain:** ml-solution-monitoring-maintenance-and-security
**Topic:** SageMaker Role Manager and Least-Privilege Patterns

A team uses Role Manager but realises that when the persona evolves over time (new permissions added by AWS), they want to opt in or out per-role. Which manageability practice does the documentation recommend?

- A. Treat Role Manager-generated policies as a starting point baked into Infrastructure-as-Code; control persona refreshes via deliberate template updates so that auto-changes do not silently widen production roles.
- B. Auto-apply every persona update to every role
- C. Disable Role Manager
- D. Make all roles public

**Correct Answer:** A

**Explanation:**
<p>Personas in Role Manager evolve as AWS adds features. To keep production roles stable, treat each Role Manager-generated policy as a starting point and bake it into Infrastructure-as-Code (CloudFormation/CDK/Terraform). Deliberate template updates control when persona refreshes apply, so production permissions do not silently widen on AWS schedule. Auto-apply silently widens; disabling Role Manager is excessive; public roles are a catastrophe. The MLA-C01 exam tests this manageability principle.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine your operating system auto-installing every new feature. Sometimes a feature breaks workflows. Treat Role Manager updates the same way Sysadmins treat OS updates: stage them in IaC and deploy on your cadence.</p>
<h3 id="option-analysis752">Option Analysis:752</h3>
<ul>
<li><strong>A</strong>：Correct because treating Role Manager output as IaC starting point with deliberate refresh is the recommended practice.</li>
<li><strong>B</strong>：Wrong because auto-apply silently widens permissions.</li>
<li><strong>C</strong>：Wrong because disabling Role Manager is excessive.</li>
<li><strong>D</strong>：Wrong because public roles are a catastrophe.</li>
</ul>

## Question 10

**Domain:** data-preparation-for-ml
**Topic:** ML Data Quality, Integrity, and Labeling

An ML Engineer wants automated, repeatable data-quality checks (column types, cardinality, missing-value rates, distribution drift) integrated with SageMaker Pipelines so bad data can fail the pipeline before training. Which feature is purpose-built?

- A. SageMaker Model Monitor with a Data Quality job, and SageMaker Pipelines QualityCheck/ClarifyCheck steps to enforce baselines and gate execution.
- B. AWS WAF rules.
- C. Manually review CSVs in Athena.
- D. Amazon Rekognition Moderation.

**Correct Answer:** A

**Explanation:**
<p>SageMaker Model Monitor's Data Quality job runs Deequ-based statistics on a baseline dataset and produces constraints (column types, completeness, distribution). SageMaker Pipelines exposes QualityCheck and ClarifyCheck steps that run those checks on new data and fail the pipeline if thresholds are violated. This automates data-quality gates so a pipeline never trains on broken data. The MLA-C01 lesson is to map automated data-quality gates to the Pipelines + Model Monitor combo. Distractors invoke unrelated services.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Think of QualityCheck as the security scanner at airport baggage claim - if a suitcase fails inspection, the conveyor belt halts before it reaches the plane. Pipelines + Model Monitor are that scanner for your training data.</p>
<h3 id="option-analysis783">Option Analysis:783</h3>
<ul>
<li><strong>A</strong>：Correct - Model Monitor data-quality + Pipelines QualityCheck steps automate gates.</li>
<li><strong>B</strong>：Wrong - WAF is for HTTP, not data validation.</li>
<li><strong>C</strong>：Wrong - manual review is not automated.</li>
<li><strong>D</strong>：Wrong - Rekognition Moderation is for images, not data quality.</li>
</ul>

## Question 11

**Domain:** ml-solution-monitoring-maintenance-and-security
**Topic:** SageMaker CloudWatch Monitoring and Cost Optimization

A team observes that their SageMaker endpoint costs are dominated by GPU instance hours, but their model is small. They want to keep GPU acceleration only when input size warrants it. Which SageMaker hosting feature lets them attach GPU acceleration on-demand instead of running a full-time GPU instance?

- A. Amazon Elastic Inference (deprecated in favour of newer alternatives like Inferentia/g5/Serverless).
- B. Inference Components: deploy each model as a component on a shared endpoint with precise per-component CPU/GPU/memory allocation, scaling each component independently.
- C. Provisioned IOPS storage
- D. Compute Optimizer

**Correct Answer:** B

**Explanation:**
<p>SageMaker Inference Components let you deploy multiple models on a single shared endpoint with precise resource allocation per model — fractional GPUs, per-model memory, per-model min/max copies — and scale each component independently. This decouples model packaging from instance footprint and allows efficient sharing of the same GPU instance across multiple models that each need only a slice of GPU. Elastic Inference, which previously offered GPU-acceleration-as-a-side-attachment, is deprecated; the modern recommendation is Inferentia or Inference Components. Storage IOPS and Compute Optimizer are unrelated. The MLA-C01 exam expects candidates to recognise Inference Components as the modern fractional-GPU lever and to know Elastic Inference is no longer the answer.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine a co-working space where each tenant gets exactly the desk space and printer access they need, billed accordingly. Inference Components are that for shared GPU endpoints — each model rents a slice of GPU and memory and the rest is shared.</p>
<h3 id="option-analysis1059">Option Analysis:1059</h3>
<ul>
<li><strong>A</strong>：Wrong because Elastic Inference is deprecated.</li>
<li><strong>B</strong>：Correct because Inference Components allocate fractional GPU/CPU per model on a shared endpoint.</li>
<li><strong>C</strong>：Wrong because storage IOPS does not provide GPU acceleration.</li>
<li><strong>D</strong>：Wrong because Compute Optimizer suggests right-sizing, not on-demand GPU attach.</li>
</ul>

## Question 12

**Domain:** ml-model-development
**Topic:** Modeling Approach Selection — JumpStart, Bedrock, and Custom

A retail analytics team has fewer than 200 labelled tickets and needs a chatbot that answers questions about returns, store hours, and order status by referencing the company's policy PDFs. The team has no ML engineers, a six-week deadline, and no GPUs. Leadership wants the lowest possible operational overhead and is happy with a managed third-party foundation model as long as the data stays inside the AWS account. Which modeling approach should an ML Engineer recommend?

- A. Train a custom transformer from scratch on Amazon SageMaker using the 200 labelled tickets and a multi-GPU cluster.
- B. Fine-tune a SageMaker JumpStart text-classification model on the 200 tickets and host it on a real-time endpoint.
- C. Use Amazon Bedrock with a hosted foundation model plus a Knowledge Base that ingests the policy PDFs from Amazon S3 to provide retrieval-augmented answers.
- D. Deploy an open-source LLM on a self-managed Amazon EC2 GPU fleet behind an Application Load Balancer.

**Correct Answer:** C

**Explanation:**
<p>The signal in this stem is the combination of small labelled set, no ML staff, document-grounded answers, and tight timeline. Bedrock plus a Knowledge Base is the canonical AWS answer for that situation: the foundation model is hosted, the Knowledge Base does the chunking, embedding, and retrieval, and the data stays in the customer account. No fine-tuning is required because RAG injects the policy text into the prompt at query time. JumpStart fine-tuning would be the right call only if the team had thousands of labelled examples and a domain that the FM does not already cover. Training a transformer from scratch is almost never correct in MLA-C01 unless the question explicitly rules out FMs and pre-trained checkpoints. Self-managed EC2 hosting is the operational-overhead opposite of what is asked.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Picture a small store that wants a knowledgeable cashier in six weeks. Hiring a foundation-model-based assistant from Bedrock and giving it the policy binder via a Knowledge Base is like hiring an experienced contractor and handing them the staff handbook on day one. Training a model from scratch would be like sending someone to a four-year university first.</p>
<h3 id="option-analysis1205">Option Analysis:1205</h3>
<ul>
<li><strong>A</strong>：Wrong - from-scratch training needs orders of magnitude more data and time than this team has.</li>
<li><strong>B</strong>：Wrong - JumpStart classifier alone does not read PDFs and would need a custom retrieval layer the team cannot maintain.</li>
<li><strong>C</strong>：Correct - Bedrock + Knowledge Base provides managed RAG over the policy PDFs with zero training.</li>
<li><strong>D</strong>：Wrong - self-managed EC2 hosting maximises ops burden, opposite of the stated constraint.</li>
</ul>

## Question 13

**Domain:** ml-model-development
**Topic:** SageMaker Training Jobs and Built-in Algorithms

A team uses the SageMaker built-in Random Cut Forest algorithm. Which problem does this algorithm specifically address?

- D. Anomaly detection on numeric and tabular data using an unsupervised tree-based method.
- B. Image segmentation for autonomous driving.
- A. Word embeddings for NLP downstream tasks.
- C. Forecasting many related time series.

**Correct Answer:** D

**Explanation:**
<p>Random Cut Forest (RCF) is SageMaker's unsupervised anomaly-detection algorithm. It assigns anomaly scores to data points by measuring how easily they can be isolated in random tree partitions. RCF works on numeric and tabular data and is widely used for fraud detection, IT operations monitoring, and IoT outlier detection. The MLA-C01 lesson is to know algorithm-task mappings: BlazingText = word embeddings, Semantic Segmentation = image segmentation, DeepAR = many related time series, RCF = anomaly detection.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>RCF is the bouncer that notices the one customer who looks completely out of place at the party. It does not need a list of bad guests - it spots oddness directly.</p>
<h3 id="option-analysis713">Option Analysis:713</h3>
<ul>
<li><strong>A</strong>：Wrong - word embeddings are BlazingText.</li>
<li><strong>B</strong>：Wrong - image segmentation is Semantic Segmentation.</li>
<li><strong>C</strong>：Wrong - many related time series is DeepAR.</li>
<li><strong>D</strong>：Correct - RCF is unsupervised anomaly detection.</li>
</ul>

## Question 14

**Domain:** ml-solution-monitoring-maintenance-and-security
**Topic:** IAM, KMS, and VPC for SageMaker Workloads

A regulated insurance team requires that the violations.json output of every Model Monitor run be retained immutably for seven years for audit. Where should they configure this retention?

- A. On the SageMaker monitoring schedule's S3 output prefix using S3 Object Lock with a seven-year retention period and S3 Versioning enabled.
- B. Inside constraints.json by setting a retention_days field to 2555.
- C. By increasing the monitoring schedule cadence to once per day.
- D. By turning on AWS X-Ray on the endpoint.

**Correct Answer:** A

**Explanation:**
<p>Model Monitor writes violations.json (and statistics.json/constraints.json) to the S3 prefix configured on the monitoring schedule. Immutability and retention controls are S3-level features: enable Object Lock in compliance mode with a default retention period of seven years on the bucket (or apply per-object retention), and turn on Versioning so older versions are not silently overwritten. constraints.json does not carry retention metadata, monitoring cadence has no retention semantics, and X-Ray is unrelated. The MLA-C01 exam tests that candidates push retention requirements down to the storage layer rather than treating Model Monitor as the retention boundary.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Compliance retention is a property of the safe, not of what you put inside it. Model Monitor produces the documents; S3 Object Lock is the safe with the locked seven-year timer. Versioning is the safe's ledger that prevents documents from being silently replaced.</p>
<h3 id="option-analysis970">Option Analysis:970</h3>
<ul>
<li><strong>A</strong>：Correct because S3 Object Lock + Versioning is the documented immutability control.</li>
<li><strong>B</strong>：Wrong because constraints.json has no retention field.</li>
<li><strong>C</strong>：Wrong because cadence does not control retention.</li>
<li><strong>D</strong>：Wrong because X-Ray is for distributed tracing, not retention.</li>
</ul>

## Question 15

**Domain:** ml-model-development
**Topic:** Distributed Training on SageMaker

A team's SageMaker training job using Linear Learner takes 6 hours on a single ml.c5.4xlarge instance for a 100 GB tabular regression dataset. The team wants to reduce wall-clock time. Which approach is the SageMaker-native way?

- C. Configure the Linear Learner training job to run on multiple instances; SageMaker will distribute training across them automatically.
- A. Increase the number of epochs to converge faster.
- B. Disable hyperparameter tuning.
- D. Convert the dataset to images.

**Correct Answer:** C

**Explanation:**
<p>Many SageMaker built-in algorithms support distributed training across multiple instances. Linear Learner is one such algorithm - the team simply increases instance_count on the Estimator and SageMaker handles data sharding and gradient aggregation. XGBoost, BlazingText, K-Means, and others also support distributed training. The MLA-C01 lesson is that horizontal scaling is the standard wall-clock lever for built-in algorithms, and the SageMaker SDK exposes it as a single parameter.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>If one instance takes 6 hours, four instances take roughly 1.5 hours - assuming the algorithm scales horizontally, which Linear Learner does.</p>
<h3 id="option-analysis663">Option Analysis:663</h3>
<ul>
<li><strong>A</strong>：Wrong - more epochs increase wall-clock time, opposite of the goal.</li>
<li><strong>B</strong>：Wrong - disabling tuning is unrelated to the training-job wall-clock.</li>
<li><strong>C</strong>：Correct - Linear Learner supports distributed training; increase instance_count.</li>
<li><strong>D</strong>：Wrong - converting tabular to images is contrived and unrelated.</li>
</ul>

## Question 16

**Domain:** ml-solution-monitoring-maintenance-and-security
**Topic:** SageMaker Model Monitor — Data Quality and Model Quality

A team built a CloudWatch dashboard on top of Model Monitor metrics. They want to compare two endpoints (a champion and a challenger) side-by-side on the same panels, broken out by ProductionVariant. Which CloudWatch dimension is published by Model Monitor specifically for this purpose?

- A. The MonitoringSchedule dimension only; ProductionVariant is not published.
- B. Region and AccountId dimensions only; per-variant comparison is not supported.
- C. EndpointName + VariantName dimensions on each emitted metric, allowing side-by-side comparison across variants.
- D. An aggregated 'AllVariants' dimension that hides per-variant detail.

**Correct Answer:** C

**Explanation:**
<p>Model Monitor metrics are published with EndpointName and VariantName as CloudWatch dimensions, plus per-feature and per-metric labels. This means a CloudWatch dashboard can render champion-vs-challenger comparisons by selecting the same metric and filtering by VariantName, all on the same time series panel. MonitoringSchedule and Region are also dimensions but they are not the specific dimension that supports per-variant comparison; that is VariantName. There is no AllVariants aggregate that hides the per-variant detail. The MLA-C01 exam tests that candidates know the CloudWatch dimension surface for variant comparisons in shadow/blue-green/A-B testing scenarios.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>A scoreboard with two team-name columns lets fans compare them at a glance. Model Monitor's CloudWatch metrics carry the team-name column (VariantName) so the scoreboard can plot champion and challenger on the same chart.</p>
<h3 id="option-analysis929">Option Analysis:929</h3>
<ul>
<li><strong>A</strong>：Wrong because both Endpoint and Variant dimensions are published.</li>
<li><strong>B</strong>：Wrong because per-variant comparison is fully supported via VariantName.</li>
<li><strong>C</strong>：Correct because EndpointName + VariantName dimensions enable side-by-side variant comparisons.</li>
<li><strong>D</strong>：Wrong because per-variant detail is preserved; there is no hiding AllVariants aggregate.</li>
</ul>

## Question 17

**Domain:** deployment-and-orchestration-of-ml-workflows
**Topic:** ML Deployment Strategies — A/B Testing, Shadow, and Blue/Green

A risk team wants to evaluate a redesigned credit-scoring model against the current one for two weeks before promoting it. They want every production request to be sent to both models, but only the current model's response is returned to the client. The candidate model's predictions are recorded in S3 for offline comparison. Which SageMaker capability is purpose-built for this pattern?

- A. SageMaker Endpoint Shadow Testing (ShadowProductionVariants).
- B. Two production variants with weights 1 and 1.
- C. Two separate endpoints called by the client in parallel.
- D. Batch Transform of last week's traffic against the candidate model.

**Correct Answer:** A

**Explanation:**
<p>SageMaker Shadow Testing (introduced as ShadowProductionVariants on EndpointConfig and via the CreateShadowTest API in SageMaker Studio) lets you replay 100% of production traffic to a shadow variant in parallel with the production variant. The production variant's response is returned to the client; the shadow variant's response is captured (via Data Capture) for offline comparison and analysis. The shadow variant runs on its own instance fleet, so its load and latency do not affect the production response path.</p>
<p>This pattern is ideal for de-risking a model swap: you see exactly how the candidate would behave on real traffic without exposing customers to it. After the shadow window passes the team's bar (similar metrics or improved metrics on captured predictions), the team can promote the candidate via a normal blue/green deployment. Production variants with 50/50 weights would expose the candidate to clients. Client-side parallel calls reinvent the feature with more risk. Replaying last week is not live shadowing.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine a flight simulator running alongside the real flight. The real pilot's actions still control the plane; the simulator records what the candidate pilot would have done. After the trip you compare both and decide whether the candidate is ready to fly. Shadow testing is that simulator on the SageMaker endpoint.</p>
<h3 id="option-analysis1385">Option Analysis:1385</h3>
<ul>
<li><strong>A</strong>：Correct — Shadow Testing duplicates 100% of traffic to a shadow variant whose responses are captured but never returned to clients.</li>
<li><strong>B</strong>：Wrong — 50/50 production weights expose candidate responses to real clients, defeating the requirement.</li>
<li><strong>C</strong>：Wrong — client-side parallel calls reinvent the feature and risk leaking the candidate response.</li>
<li><strong>D</strong>：Wrong — historical batch replay is not live shadow testing on current traffic distribution.</li>
</ul>

## Question 18

**Domain:** deployment-and-orchestration-of-ml-workflows
**Topic:** SageMaker Pipelines and ML Workflow Orchestration

A team's SageMaker Pipeline must check, before model registration, whether the trained model's bias has drifted beyond acceptable thresholds compared to a baseline computed on a previous training run, and fail the pipeline if so. Which SageMaker Pipelines step type runs a Clarify-based check with a known baseline and integrates natively with model registration?

- D. ClarifyCheckStep — runs Clarify (bias or explainability) against a baseline and surfaces the drift indicator for downstream conditions or registry checks.
- A. ProcessingStep that calls Clarify manually.
- B. ConditionStep on Clarify output.
- C. QualityCheckStep that monitors model latency.

**Correct Answer:** D

**Explanation:**
<p>SageMaker Pipelines ships two purpose-built check steps:</p>
<ul>
<li>
<p>QualityCheckStep — runs Model Monitor data-quality or model-quality jobs against an upstream baseline; catches feature distribution drift or model performance drift.</p>
</li>
<li>
<p>ClarifyCheckStep — runs SageMaker Clarify bias or explainability jobs against an upstream baseline; catches bias drift or feature attribution drift.</p>
</li>
</ul>
<p>Both steps natively integrate with the Model Registry: their drift indicators can be attached to the model package as metadata, and they can be configured to fail the pipeline (or just surface the drift) when thresholds are breached. The right answer for bias drift checking is ClarifyCheckStep.</p>
<p>A manual ProcessingStep that calls Clarify reinvents the feature without the baseline integration. ConditionStep is the branching primitive that can act on the ClarifyCheckStep's output but is not the runner. QualityCheckStep is for data/model quality, not bias. ClarifyCheckStep is the right answer.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine a clinic that has a dedicated bloodwork machine (ClarifyCheckStep) and a separate dedicated vital-signs monitor (QualityCheckStep). Each runs its own diagnostic against a baseline. Using a generic lab tech (ProcessingStep) and asking them to run bloodwork manually misses the dedicated machine's calibration features.</p>
<h3 id="option-analysis1337">Option Analysis:1337</h3>
<ul>
<li><strong>D</strong>：Correct — ClarifyCheckStep runs Clarify bias/explainability against a baseline and integrates with the Model Registry.</li>
<li><strong>A</strong>：Wrong — manual ProcessingStep call reinvents the feature without the baseline integration.</li>
<li><strong>B</strong>：Wrong — ConditionStep is the branching primitive, not the Clarify runner.</li>
<li><strong>C</strong>：Wrong — QualityCheckStep is for data/model quality drift, not Clarify bias.</li>
</ul>

## Question 19

**Domain:** data-preparation-for-ml
**Topic:** SageMaker Data Wrangler and Feature Engineering

A team uses Data Wrangler with sensitive PII columns. They want the flow to redact or mask PII before any downstream consumer sees the data. Which approach fits?

- B. Apply Data Wrangler transforms (Drop column, Replace, Hash, Tokenise) on PII columns and validate using the Bias Report or PII detection prior to export.
- A. Trust the downstream consumers to ignore PII columns.
- C. Encrypt the entire S3 bucket and do nothing else.
- D. Email PII to data scientists instead of storing in S3.

**Correct Answer:** B

**Explanation:**
<p>Data Wrangler can redact or transform PII columns before downstream consumption: Drop column, Replace with constant, Hash values (one-way), or Tokenise. Combined with Macie (S3 PII discovery) and IAM access controls, this lets the flow produce a non-PII derived dataset for ML training. Encryption at rest does not solve the 'authorised user should not see PII' concern - column-level redaction does. The MLA-C01 lesson is that Data Wrangler has explicit transforms for PII handling, not just generic encryption.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Locking the kitchen door (encryption) does not stop the cooks from seeing the secret ingredient. Removing the secret ingredient from the prep tray (redaction) does.</p>
<h3 id="option-analysis712">Option Analysis:712</h3>
<ul>
<li><strong>A</strong>：Wrong - trust is not a control.</li>
<li><strong>B</strong>：Correct - drop/hash/tokenise transforms redact PII at flow time.</li>
<li><strong>C</strong>：Wrong - encryption alone does not redact data for authorised users.</li>
<li><strong>D</strong>：Wrong - email is a worse risk.</li>
</ul>

## Question 20

**Domain:** deployment-and-orchestration-of-ml-workflows
**Topic:** SageMaker Model Registry and Versioning

An ML Engineer notices a model package group has 200 versions and they want to query 'show me all versions Approved in 2025'. Which API/feature supports this?

- D. ListModelPackages with filters on ModelPackageGroupName, ModelApprovalStatus=Approved, and CreationTime range; results are paginated.
- A. Manually open each version in the console one at a time.
- B. Run an Athena query against the registry.
- C. Use the SageMaker JumpStart catalogue to browse versions.

**Correct Answer:** D

**Explanation:**
<p>ListModelPackages accepts filters: ModelPackageGroupName, ModelApprovalStatus, ModelPackageType, CreationTimeAfter/Before. Combined with pagination, callers can query the registry programmatically for Approved versions in a date range. The Studio UI also exposes filter controls. JumpStart is unrelated; Athena does not query the registry. The MLA-C01 lesson is to know ListModelPackages and its filters as the audit/query mechanism.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>ListModelPackages is the registry's filtered search bar - 'show me approved 2025 versions' returns the matching list, paginated, in seconds. Manual scrolling is the slow alternative.</p>
<h3 id="option-analysis651">Option Analysis:651</h3>
<ul>
<li><strong>A</strong>：Wrong - 200 manual reviews is infeasible.</li>
<li><strong>B</strong>：Wrong - registry is not a Glue table.</li>
<li><strong>C</strong>：Wrong - JumpStart is for pre-trained models.</li>
<li><strong>D</strong>：Correct - ListModelPackages with filters is the audit API.</li>
</ul>
