# ExamLab - AWS MLA-C01 Mock Exam (65 Questions)

Source: https://examlab.net/en/certs/aws/mla-c01/exam

---

## Question 1

**Domain:** data-preparation-for-ml
**Difficulty:** medium

An ML Engineer needs to one-hot encode a high-cardinality categorical column (5,000 distinct values) inside a Data Wrangler flow. Which transform behaviour is the canonical recommendation?

- B. Use Data Wrangler's categorical-encoding transforms with techniques like Ordinal, Similarity (hashing), or Target encoding instead of plain One-Hot to avoid feature-space explosion.
- A. Apply One-Hot Encoding directly to all 5,000 categories.
- C. Drop the column entirely.
- D. Convert the column to a Base64 string.

**Correct Answer:** B

**Explanation:**
<p>Data Wrangler's encoding transforms include Ordinal Encode, One-Hot Encode, Similarity (feature hashing), and Target Encode. For high-cardinality columns, plain One-Hot creates an unmanageably wide and sparse matrix. Hashing trades collisions for fixed dimensionality. Target encoding replaces categories with the mean target value (with smoothing) - effective for tree models. The MLA-C01 lesson is to recognise the cardinality trap and the encoding alternatives that Data Wrangler exposes.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>One-Hot Encode for 5,000 categories is giving each customer a unique phone with one number - too many phones. Hashing groups similar customers onto shared phones; target encoding writes the average tip on each phone instead.</p>
<h3 id="option-analysis751">Option Analysis:751</h3>
<ul>
<li><strong>A</strong>：Wrong - 5,000 One-Hot columns explodes feature space.</li>
<li><strong>B</strong>：Correct - ordinal/hashing/target encoding handle high-cardinality safely.</li>
<li><strong>C</strong>：Wrong - dropping loses signal.</li>
<li><strong>D</strong>：Wrong - Base64 is not categorical encoding.</li>
</ul>

## Question 2

**Domain:** ml-model-development
**Difficulty:** medium

An ML Engineer is summarising for the team when distributed training is worth the extra complexity over single-instance training. Which framing best captures the canonical MLA-C01 trade-off?

- B. Distribute when the model exceeds single-GPU memory or when single-instance training is too slow for the project's deadline; otherwise, single-instance training is simpler and often sufficient.
- A. Always distribute training to maximise efficiency.
- C. Never distribute training because cluster overhead always wins.
- D. Distribute only when training data exceeds 10 TB.

**Correct Answer:** B

**Explanation:**
<p>Distributed training adds operational complexity: networking, debugging, checkpoint management, scaling efficiency. The canonical trigger conditions are model size (does not fit on one GPU) or wall-clock pressure (single-instance training is too slow for the deadline). Below those thresholds, single-instance training is simpler, cheaper, and often sufficient. Universal claims (always distribute, never distribute) miss the trade-off. The MLA-C01 lesson is to articulate the conditions under which distributed training is worth the cost.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Hiring more workers makes sense when one cannot fit the kitchen or one cannot finish on time. If neither is true, hiring more workers just adds management overhead.</p>
<h3 id="option-analysis739">Option Analysis:739</h3>
<ul>
<li><strong>A</strong>：Wrong - always distribute ignores complexity costs.</li>
<li><strong>B</strong>：Correct - distribute when memory or wall-clock requires it; otherwise single-instance.</li>
<li><strong>C</strong>：Wrong - never distribute is too restrictive for large workloads.</li>
<li><strong>D</strong>：Wrong - data size is not the only or binary trigger.</li>
</ul>

## Question 3

**Domain:** ml-solution-monitoring-maintenance-and-security
**Difficulty:** medium

A SageMaker domain is multi-tenant: 10 data-science teams share one domain. Each team must only see and use S3 buckets belonging to its own team. Which architectural mechanism BEST achieves per-team data isolation?

- A. Provision one Studio user-profile per team (or per user) and back each profile with a distinct execution role granting access only to that team's S3 buckets and CMK; rely on Studio's per-profile execution role to enforce data-access isolation.
- B. Give every team the AdministratorAccess policy
- C. Make all S3 buckets public
- D. Use a single shared role for all teams

**Correct Answer:** A

**Explanation:**
<p>Studio user profiles each have an execution role attached. By assigning each team (or each user) a profile whose execution role is scoped to that team's resources (S3 prefixes, CMKs, and so on), Studio's apps automatically run with the right scope and cannot read other teams' data. This is the canonical multi-tenancy pattern documented for shared Studio domains. SageMaker Role Manager (covered in another topic) can simplify creating these scoped roles. AdministratorAccess, public buckets, and a shared role all break isolation. The MLA-C01 exam expects candidates to recognise per-profile scoped roles as the multi-tenancy isolation mechanism.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine a co-working space where every tenant has their own keycard that only opens their own office. Studio user-profile execution roles are those keycards. A single master key (shared role) would let everyone wander into everyone else's office.</p>
<h3 id="option-analysis930">Option Analysis:930</h3>
<ul>
<li><strong>A</strong>：Correct because per-profile execution roles are the documented multi-tenancy isolation mechanism.</li>
<li><strong>B</strong>：Wrong because AdministratorAccess violates least privilege.</li>
<li><strong>C</strong>：Wrong because public buckets are a security catastrophe.</li>
<li><strong>D</strong>：Wrong because a single shared role removes isolation.</li>
</ul>

## Question 4

**Domain:** data-preparation-for-ml
**Difficulty:** medium

A pharmaceutical research lab has 180 TB of historical genome-sequencing data sitting on an on-prem NetApp filer, with no AWS Direct Connect circuit and a shared 200 Mbps internet uplink. The compliance committee has approved a six-week migration window before the SageMaker training pipeline must begin. The ML engineer estimates that even at theoretical peak the public internet would saturate at well under 100 Mbps sustained because the office shares the link with daily business traffic. The team needs all 180 TB landed in Amazon S3 within the six-week window with strong integrity verification and encryption in transit. Which ingestion mechanism is the correct choice?

- A. Order multiple AWS Snowball Edge devices, copy the data on-prem at 10 Gbps to the appliances, and ship them back to AWS for import to S3.
- B. Use Amazon S3 Transfer Acceleration over the existing 200 Mbps internet link to upload the entire dataset directly.
- C. Configure AWS DataSync over the existing 200 Mbps public internet link with parallel agents.
- D. Provision an AWS Direct Connect circuit and run an AWS Database Migration Service task in CDC mode.

**Correct Answer:** A

**Explanation:**
<p>At 100 Mbps sustained the migration math is decisive: 180 TB requires roughly 4,000 hours, or about 167 days, of pure transfer time over the existing public internet — well past the six-week window. AWS Snowball Edge is the canonical answer for tens-to-hundreds of TB migrations under bandwidth or deadline constraints. Each appliance accepts 80 TB at 10 Gbps locally, ships back to AWS, and is imported into S3 within days. Three or four devices in parallel cover 180 TB inside the six-week window with built-in 256-bit encryption in transit and integrity verification on import.</p>
<p>S3 Transfer Acceleration optimizes the wide-area portion of an S3 upload by routing through CloudFront edges, but the local uplink is still the bottleneck — it cannot exceed the 200 Mbps that the office actually has. AWS DataSync runs over the same network path; parallel agents share the same uplink and do not multiply bandwidth. Direct Connect provisioning lead time is typically four to eight weeks before the circuit is even live, defeating the deadline, and DMS CDC mode targets ongoing relational change-data-capture, not one-shot bulk file migration. The MLA-C01 stem signal 'tens-to-hundreds of TB, limited bandwidth, deadline in days-to-weeks' is the Snowball signature.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine you need to move a houseful of furniture across the country in two weeks, but the only road out of your driveway is single-lane gravel. No matter how fancy your delivery service is, the gravel road throttles everything to walking pace. The right answer is a moving truck (Snowball) parked in the driveway: load it locally at warehouse speed and let it drive on the highway you cannot reach.</p>
<h3 id="option-analysis1696">Option Analysis:1696</h3>
<ul>
<li><strong>A</strong>：Correct because Snowball Edge bypasses the public-internet bottleneck with physical shipping and lands the data within the deadline.</li>
<li><strong>B</strong>：Wrong because Transfer Acceleration cannot exceed the local uplink ceiling, which is the binding bottleneck.</li>
<li><strong>C</strong>：Wrong because DataSync still traverses the 200 Mbps link; parallel agents share the same pipe.</li>
<li><strong>D</strong>：Wrong because Direct Connect provisioning alone burns most of the deadline, and DMS CDC is for relational replication not file migration.</li>
</ul>

## Question 5

**Domain:** data-preparation-for-ml
**Difficulty:** medium

An ML platform engineer is designing the S3 bucket structure for a multi-tenant ML data lake serving 12 ML teams. Each team will produce raw data, curated training-ready datasets, and trained model artifacts. The platform team wants strong logical separation, easy IAM scoping per team, clear lifecycle policies per dataset class, and the ability to delegate Lake Formation grants per team. They are debating whether to use one bucket per team or a single bucket with team-prefixes. Which layout is the AWS-recommended starting pattern for this multi-tenant ML data lake, and why?

- A. One bucket per team with structured prefixes inside each (raw/, curated/, models/), so IAM, lifecycle, and Lake Formation grants scope cleanly per team.
- B. A single shared bucket with everything mixed at the top level, relying on object tags for separation.
- C. One bucket per S3 storage class (Standard, IA, Glacier) regardless of team ownership.
- D. One bucket per AWS account, with all 12 teams sharing the same set of buckets via cross-account access.

**Correct Answer:** A

**Explanation:**
<p>For a multi-tenant ML data lake at the scale described, the AWS-recommended starting pattern is <strong>one bucket per team</strong> with internally structured prefixes (<code>raw/</code>, <code>curated/</code>, <code>models/</code>, etc.). This layout gives each team a clean IAM boundary (one bucket policy per team), clean S3 lifecycle policies per dataset class within the team's bucket, clean Lake Formation grants per team, and a clean cost-allocation tag boundary. Each team's data scientists and ML engineers operate in a well-bounded namespace with the platform team owning the cross-team Lake Formation governance.</p>
<p>A single shared bucket with object tags forces every IAM and lifecycle decision to be expressed via tag-based conditions, which scales poorly and is error-prone at 12 teams. Sharding by storage class breaks per-team ownership entirely. Sharing one bucket set across multiple accounts adds cross-account complexity without buying any logical separation. Within the per-team bucket, a structured prefix layout (<code>raw/&#x3C;dataset>/&#x3C;version>/&#x3C;partition>/</code>, <code>curated/&#x3C;dataset>/&#x3C;version>/</code>, <code>models/&#x3C;model_name>/&#x3C;version>/</code>) gives the predictable structure SageMaker training, Athena, and Lake Formation expect.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Per-team buckets are like giving each research lab its own room in the building, with a labelled door, a labelled key, and the lab's own cleaning schedule. A single shared bucket with object tags is twelve labs sharing one giant room, hoping that little plastic flags on every petri dish keep things straight. The first scales; the second does not.</p>
<h3 id="option-analysis1565">Option Analysis:1565</h3>
<ul>
<li><strong>A</strong>：Correct because per-team buckets give clean IAM, lifecycle, and Lake Formation boundaries and are the AWS-recommended pattern.</li>
<li><strong>B</strong>：Wrong because object tags do not provide the per-team scoping clarity needed at 12 teams.</li>
<li><strong>C</strong>：Wrong because storage class is a lifecycle property, not a sharding key, and breaks per-team ownership.</li>
<li><strong>D</strong>：Wrong because sharing buckets across 12 teams in one account defeats logical separation.</li>
</ul>

## Question 6

**Domain:** ml-solution-monitoring-maintenance-and-security
**Difficulty:** hard

A SageMaker training job needs to read encrypted training data from S3 in another AWS account. Which configurations are required?

- A. The S3 bucket policy in the source account must allow the SageMaker execution role from the training-job account; the SageMaker execution role's IAM policy must allow s3:GetObject on the source bucket; the source bucket's CMK key policy must allow kms:Decrypt for the same role.
- B. Just IAM policy on the role
- C. Make the bucket public
- D. Email the CMK ARN

**Correct Answer:** A

**Explanation:**
<p>Cross-account S3 reads with SSE-KMS require three separate authorisation surfaces to align: (1) the source bucket policy must allow s3:GetObject for the consuming role, (2) the consuming role's IAM policy must allow s3:GetObject on the source bucket, and (3) the source CMK's key policy must allow kms:Decrypt for the consuming role. KMS uses an AND model — both the IAM policy AND the key policy must allow the call. Missing any of these three produces an Access Denied error. Public access is forbidden; ARN sharing alone does not grant access. The MLA-C01 exam tests this cross-account triple-policy pattern.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Cross-account access is like a child borrowing a friend's locker at another school. The friend's parents (bucket policy) must consent. The child's own school (role policy) must allow them to walk over. And the locker's lock-maker (key policy) must accept the child's key. All three or no entry.</p>
<h3 id="option-analysis941">Option Analysis:941</h3>
<ul>
<li><strong>A</strong>：Correct because cross-account SSE-KMS reads require all three policy surfaces to align.</li>
<li><strong>B</strong>：Wrong because IAM policy alone is insufficient.</li>
<li><strong>C</strong>：Wrong because public access is a security anti-pattern.</li>
<li><strong>D</strong>：Wrong because ARN sharing alone does not grant access.</li>
</ul>

## Question 7

**Domain:** data-preparation-for-ml
**Difficulty:** hard

An ML Engineer uses feature groups across two AWS regions. They want a feature group's online store to fail over to a secondary region during regional outage. Which approach is correct?

- D. Replicate features by writing to a feature group in each region (active-active or active-passive ingestion) and route reads to the available region; Feature Store online store is regional and not natively cross-region replicated.
- A. Feature Store auto-replicates online store to all regions.
- B. S3 cross-region replication automatically syncs the online store.
- C. Online store is global by default.

**Correct Answer:** D

**Explanation:**
<p>Feature Store online store is a regional resource. Cross-region resilience requires application-level replication: ingest pipelines write to feature groups in multiple regions (active-active) or replicate from primary to secondary (active-passive), and the inference layer fails over by region. The offline-store S3 bucket can use S3 Cross-Region Replication, but that does not give a queryable online store in the secondary region. Universal claims (auto-replicated, global) are wrong. The MLA-C01 lesson is the regional scope of Feature Store and the application-level cross-region pattern.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Each region is its own independent cash register. If you want a backup register in another city, you have to set it up and feed it the same updates - the regions do not auto-mirror.</p>
<h3 id="option-analysis809">Option Analysis:809</h3>
<ul>
<li><strong>A</strong>：Wrong - no native cross-region online replication.</li>
<li><strong>B</strong>：Wrong - S3 CRR covers offline only.</li>
<li><strong>C</strong>：Wrong - online is regional.</li>
<li><strong>D</strong>：Correct - application-level replication with regional routing.</li>
</ul>

## Question 8

**Domain:** ml-model-development
**Difficulty:** hard

A team uses SageMaker Debugger built-in rules but wants a custom rule that fires when an internal metric (e.g., a layer activation magnitude) exceeds 100. Which approach supports this?

- C. Write a custom rule using the smdebug Python library, package it as a container image, and reference it in the Debugger configuration alongside built-in rules.
- A. Custom rules are not supported.
- B. Built-in rules already include every conceivable check.
- D. Use AWS Lambda to inspect tensors directly during training.

**Correct Answer:** C

**Explanation:**
<p>SageMaker Debugger supports both built-in rules (LossNotDecreasing, VanishingGradient, etc.) and custom rules. A custom rule is implemented using the smdebug Python library, packaged into a container image (often extending an AWS-provided debugger base image), pushed to ECR, and referenced in the Debugger rule configuration. The custom rule receives a SMDebug Trial object exposing captured tensors and metrics; it returns True to fire. Distractors deny support or invoke wrong-context services. The MLA-C01 lesson is to know that Debugger is extensible via custom rules.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Built-in rules are off-the-shelf medical screenings. Custom rules are the specialty test you write for a condition not in the standard panel.</p>
<h3 id="option-analysis750">Option Analysis:750</h3>
<ul>
<li><strong>A</strong>：Wrong - custom rules are supported.</li>
<li><strong>B</strong>：Wrong - built-in rules cover common, not all, pathologies.</li>
<li><strong>C</strong>：Correct - smdebug-based custom rules packaged as container images.</li>
<li><strong>D</strong>：Wrong - Lambda cannot inspect training tensors.</li>
</ul>

## Question 9

**Domain:** data-preparation-for-ml
**Difficulty:** medium

An ML team has been asked to reproduce, exactly, the predictions of a model that was trained six months ago to support a regulatory audit. The training script and container hashes are pinned in Git and recoverable. The dataset, however, was read from `s3://team-ml-data/clickstream/raw/` at training time without any version pinning, and the bucket has S3 Object Versioning enabled but no other dataset-version markers. New events have been written to the same prefix every day since, so the live prefix today contains six additional months of data. The lead engineer asks how to reliably reproduce the exact training data slice as of six months ago. Which approach offers the strongest, most maintainable answer for going forward?

- A. Rely on S3 Object Versioning to roll back the bucket state to six months ago; this is exactly what versioning is designed for.
- B. Compare the size of training-input S3 objects and use the smallest matching combination as the training set.
- C. Run a Glue Crawler over the live prefix; the crawler produces a snapshot of how the prefix looked six months ago.
- D. Adopt prefix-based dataset versioning (for example s3://team-ml-data/clickstream/v17/) and lock SageMaker training input URIs to the versioned prefix.

**Correct Answer:** D

**Explanation:**
<p>Training reproducibility is a lineage problem, not a delete-protection problem. S3 Object Versioning is operationally critical for protecting against accidental delete and overwrite per object, but it is not a logical training-set versioning system. Six months of new objects have been added to the same prefix; rolling back individual object versions does not give you 'the dataset as it was on the training date.' The AWS-recommended pattern for ML reproducibility is <strong>prefix-based dataset versioning</strong> — write each dataset version to its own immutable prefix (<code>s3://bucket/datasets/clickstream/v17/</code>), record the exact prefix in the SageMaker Pipelines run, and never mutate that prefix. Six months later, retraining or audit replay simply points at the same prefix.</p>
<p>DVC is an alternative tool for finer-grained dataset versioning with Git-style semantics, but the prefix-based pattern works without extra tooling and is the simpler answer the exam expects. Comparing object sizes is not a real lineage strategy. Glue Crawlers reflect current state, not historical state. The MLA-C01 stem signal 'reproduce a model trained months ago' is the prefix-or-DVC versioning signature.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>S3 Object Versioning is like keeping every receipt you ever crumpled and threw away — useful to dig out an individual receipt, useless for reconstructing 'what did my whole filing cabinet look like in March?' Prefix versioning is keeping a labelled folder for every monthly statement: open the March 2026 folder and you have exactly what you had then.</p>
<h3 id="option-analysis1569">Option Analysis:1569</h3>
<ul>
<li><strong>A</strong>：Wrong because S3 Object Versioning protects against per-object delete or overwrite, not logical training-set lineage across many added objects.</li>
<li><strong>B</strong>：Wrong because object sizes are not a lineage signal and cannot reconstruct a historical dataset state.</li>
<li><strong>C</strong>：Wrong because Glue Crawlers reflect current state, not historical snapshots of a prefix.</li>
<li><strong>D</strong>：Correct because immutable per-version prefixes provide explicit, auditable, reproducible training-set lineage and integrate cleanly with SageMaker training input URIs.</li>
</ul>

## Question 10

**Domain:** ml-model-development
**Difficulty:** medium

A team uses Amazon SageMaker Feature Store. They want to ensure that the same feature definitions are used at training time (offline) and at inference time (online) to avoid training/serving skew. Which Feature Store capability supports this?

- A. Feature groups with both online and offline stores backed by a single feature definition; ingestion writes to both consistently.
- B. Feature Store does not support online inference.
- C. Each environment must define its own feature schema.
- D. Feature Store stores only image data.

**Correct Answer:** A

**Explanation:**
<p>SageMaker Feature Store organises features into feature groups, each with a single schema definition and two storage tiers: an offline store (S3-backed, used for training) and an online store (low-latency key-value access, used for inference). Ingestion writes to both, ensuring training and serving see the same features. This eliminates a major source of training/serving skew. The MLA-C01 lesson is that Feature Store is the AWS-managed solution for the skew problem and that feature groups with both stores are the relevant pattern.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Feature Store is one cookbook with two kitchens - one for batch prep (offline) and one for live service (online). Same recipes everywhere; no surprises at lunch service.</p>
<h3 id="option-analysis741">Option Analysis:741</h3>
<ul>
<li><strong>A</strong>：Correct - feature groups with online and offline stores share definitions and prevent skew.</li>
<li><strong>B</strong>：Wrong - Feature Store does support online inference.</li>
<li><strong>C</strong>：Wrong - separate schemas cause skew; Feature Store deliberately shares definitions.</li>
<li><strong>D</strong>：Wrong - Feature Store handles tabular features, not images.</li>
</ul>

## Question 11

**Domain:** ml-model-development
**Difficulty:** medium

A team wants to compare the cost of distributed training across 4 ml.p4d.24xlarge instances on On-Demand vs Spot. Which trade-off statement most accurately summarises Spot for multi-node training?

- B. Spot can save up to 90% on cost but introduces interruption risk; for multi-node training, checkpointing and Managed Spot Training are essential to recover from reclaims.
- A. Spot is always 50% cheaper with no risk.
- C. Spot cannot be used for multi-node training.
- D. On-Demand is always more expensive than the same workload on a different cloud.

**Correct Answer:** B

**Explanation:**
<p>EC2 Spot pricing offers up to 90% savings but instances can be reclaimed at any time. For multi-node training, a single reclaim can disrupt the whole cluster. SageMaker Managed Spot Training mitigates this by checkpointing to S3 (checkpoint_s3_uri) and resuming on new capacity when reclaims happen. The MLA-C01 lesson is to know the cost-vs-interruption trade-off and the AWS-managed mitigation. Distractors overstate or deny capabilities.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Spot is taking the cheap standby ticket - you save money but might get bumped. SageMaker checkpointing is the boarding pass that says you can rebook on the next flight without losing your seat.</p>
<h3 id="option-analysis669">Option Analysis:669</h3>
<ul>
<li><strong>A</strong>：Wrong - Spot has reclaim risk; no-risk claim is wrong.</li>
<li><strong>B</strong>：Correct - Spot saves up to 90% with reclaim risk; checkpointing + Managed Spot mitigate.</li>
<li><strong>C</strong>：Wrong - Spot is usable for multi-node with mitigation.</li>
<li><strong>D</strong>：Wrong - cross-cloud comparisons are off-topic.</li>
</ul>

## Question 12

**Domain:** ml-solution-monitoring-maintenance-and-security
**Difficulty:** easy

A retail analytics team has just deployed a demand-forecast XGBoost model behind an Amazon SageMaker real-time endpoint. The MLOps lead wants the platform to automatically detect when the statistical distribution of incoming features (such as price, promotion_flag, store_region) drifts away from the distribution seen during training. The team explicitly does not want to wait for ground-truth sales numbers to come back from the data warehouse before raising an alarm — they want a signal based purely on the request payloads. Which Amazon SageMaker Model Monitor monitoring type should they enable as the primary signal for this requirement?

- A. Model Quality monitoring, because it inspects each prediction against the live ground truth.
- B. Data Quality monitoring, because it baselines feature statistics from the training data and flags drift on captured request payloads without requiring labels.
- C. Bias Drift monitoring, because feature drift is conceptually a form of bias.
- D. Feature Attribution Drift monitoring, because it tracks how features behave at inference time.

**Correct Answer:** B

**Explanation:**
<p>SageMaker Model Monitor exposes four monitoring types: Data Quality, Model Quality, Bias Drift, and Feature Attribution Drift. Data Quality is the only one that watches the raw statistical distribution of input features captured by Data Capture against a baseline computed from the training set using Deequ. It fires drift alarms based on inputs alone and does not require any ground-truth labels — which matches the stem exactly. Model Quality requires labels merged in via the ground-truth ingestion job. Bias Drift uses Clarify pre/post-training bias metrics over protected attributes. Feature Attribution Drift uses SHAP attributions, not raw distributions. The MLA-C01 exam frequently tests the ability to map a single sentence about the signal source (raw inputs vs labels vs fairness vs SHAP) to the correct monitor type.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine four security cameras pointed at a shop. Camera 1 watches what walks in the door (Data Quality). Camera 2 compares the receipt totals from a separate auditor at the end of the day (Model Quality — needs ground truth). Camera 3 watches whether different customer groups are treated fairly (Bias Drift). Camera 4 watches which products the staff highlights to each customer (Feature Attribution Drift). The team only wants to watch the door right now — that is camera 1.</p>
<h3 id="option-analysis1340">Option Analysis:1340</h3>
<ul>
<li><strong>A</strong>：Wrong because Model Quality requires merging predictions with ground-truth labels.</li>
<li><strong>B</strong>：Correct because Data Quality is the input-distribution drift detector and works with captured payloads alone.</li>
<li><strong>C</strong>：Wrong because Bias Drift is about Clarify fairness metrics, not raw feature distributions.</li>
<li><strong>D</strong>：Wrong because Feature Attribution Drift uses SHAP attributions, not raw feature statistics.</li>
</ul>

## Question 13

**Domain:** data-preparation-for-ml
**Difficulty:** medium

An ML Engineer needs to detect and quantify duplicate or near-duplicate records in a 100-million-row training dataset before training. Which AWS service is best suited for scalable deduplication transforms?

- C. AWS Glue with FindMatches ML transform, or SageMaker Processing/Spark, both of which can compute fuzzy matches and dedupe records at scale.
- A. Amazon DynamoDB.
- B. Amazon Lex.
- D. Amazon CloudFront.

**Correct Answer:** C

**Explanation:**
<p>AWS Glue FindMatches is an ML transform that learns from labeled match/no-match examples and applies fuzzy matching to find duplicate or near-duplicate records (useful when records have typos, format variations). For large-scale custom logic, SageMaker Processing with PySpark is also common. Both run distributed and handle 100M-row scale. Distractors invoke unrelated services. The MLA-C01 lesson is to know Glue FindMatches and Spark Processing as the AWS dedup options.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Glue FindMatches is the librarian who reads through your file cabinet and notices that 'John Smith, 123 Main' and 'J. Smith, 123 Main St' are probably the same person. DynamoDB just stores items.</p>
<h3 id="option-analysis704">Option Analysis:704</h3>
<ul>
<li><strong>A</strong>：Wrong - DynamoDB is a database.</li>
<li><strong>B</strong>：Wrong - Lex is a chatbot service.</li>
<li><strong>C</strong>：Correct - Glue FindMatches and SageMaker Processing handle dedup at scale.</li>
<li><strong>D</strong>：Wrong - CloudFront is a CDN.</li>
</ul>

## Question 14

**Domain:** ml-model-development
**Difficulty:** hard

A team's distributed training job suddenly slows in the second epoch. CloudWatch shows that one node's GPU memory usage is far higher than others. What likely root cause should the team investigate first?

- B. Imbalanced data sharding causing one rank to receive disproportionately large batches; verify the sampler/distributor configuration.
- A. GPU hardware error fixed by rebooting the team's laptop.
- C. Region outage; switch regions immediately.
- D. S3 throughput limit; increase bucket size.

**Correct Answer:** B

**Explanation:**
<p>Imbalanced data sharding is a classic distributed-training pitfall: if the sampler does not distribute samples evenly, some ranks process larger batches and consume more memory. Fixes include using PyTorch DistributedSampler, Hugging Face's distributed-aware datasets, or padding/truncation strategies for variable-length sequences. The MLA-C01 lesson is to read per-rank metrics and recognise imbalance signatures. Distractors invoke unrelated infrastructure or fictional limits.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>If one chef gets twice as many orders, their counter overflows. The fix is the order distributor (sampler), not the kitchen layout.</p>
<h3 id="option-analysis647">Option Analysis:647</h3>
<ul>
<li><strong>A</strong>：Wrong - laptop is unrelated to training-instance GPU memory.</li>
<li><strong>B</strong>：Correct - imbalanced data sharding produces uneven memory per rank.</li>
<li><strong>C</strong>：Wrong - region outage affects all nodes uniformly.</li>
<li><strong>D</strong>：Wrong - S3 bucket size is not a fixed limit.</li>
</ul>

## Question 15

**Domain:** deployment-and-orchestration-of-ml-workflows
**Difficulty:** medium

A platform team running a Shadow Test wants to capture both the production variant's responses (returned to the client) and the shadow variant's responses (not returned to the client) to S3 for offline comparison. Which configuration captures both streams?

- A. Enable DataCaptureConfig on the EndpointConfig with capture mode Both (Input and Output) — Data Capture writes per-variant captured records covering both production and shadow variants.
- B. Configure two separate Data Capture configurations, one per variant.
- C. Run SageMaker Clarify on each variant.
- D. Use SageMaker Pipelines to schedule Lambda calls that record requests.

**Correct Answer:** A

**Explanation:**
<p>Endpoint Data Capture is configured once on the EndpointConfig (DataCaptureConfig block) and applies across all ProductionVariants and ShadowProductionVariants on that endpoint. With CaptureMode set to Input/Output/Both and a destination S3 prefix, SageMaker writes JSONL records that include the inference ID, variant name, captured request, captured response, and timestamps. Records from production and shadow variants are written under the same S3 prefix but distinguishable by variant name, perfect for offline comparison.</p>
<p>Separate Data Capture configurations per variant are not the API. Clarify is for bias analysis. Custom Lambda recording reinvents the native feature. Single DataCaptureConfig with CaptureMode = Both is the right answer.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine a single recorder microphone hung in the middle of two rooms — production and shadow. Whatever happens in either room is captured to one tape, with each segment labelled by which room it came from. That is Data Capture across multiple variants.</p>
<h3 id="option-analysis1036">Option Analysis:1036</h3>
<ul>
<li><strong>A</strong>：Correct — DataCaptureConfig captures across all variants on the EndpointConfig with per-variant records.</li>
<li><strong>B</strong>：Wrong — Data Capture is configured once per EndpointConfig; separate configs per variant are not the API.</li>
<li><strong>C</strong>：Wrong — Clarify is for bias analysis, not request capture.</li>
<li><strong>D</strong>：Wrong — custom Lambda reinvents the native Data Capture feature.</li>
</ul>

## Question 16

**Domain:** ml-model-development
**Difficulty:** medium

A team's Model Monitor schedule fails because the captured data does not match the baseline schema (a new feature was added to inputs). Which action is most appropriate?

- B. Regenerate the baseline using a representative dataset that includes the new feature, update the schedule, and retrain the monitor as needed.
- A. Ignore the schema mismatch.
- C. Disable data capture entirely.
- D. Switch to a different cloud provider.

**Correct Answer:** B

**Explanation:**
<p>Model Monitor's baseline encodes the schema and statistics of the training data. When production data adds or removes features, the schema diverges and the monitor cannot compare meaningfully. The fix is to regenerate the baseline on a representative dataset that includes the new feature, then update the monitoring schedule to reference the new baseline. Ignoring, disabling, or migrating clouds are not real fixes. The MLA-C01 lesson is that schema changes require baseline regeneration.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>If the recipe gains an extra ingredient, the baseline cookbook needs to be reprinted with the new ingredient listed. The old book is no longer the right reference.</p>
<h3 id="option-analysis689">Option Analysis:689</h3>
<ul>
<li><strong>A</strong>：Wrong - ignoring the mismatch leaves monitoring broken.</li>
<li><strong>B</strong>：Correct - regenerate baseline on data with the new feature and update the schedule.</li>
<li><strong>C</strong>：Wrong - disabling capture removes inputs.</li>
<li><strong>D</strong>：Wrong - cross-cloud is unrelated.</li>
</ul>

## Question 17

**Domain:** ml-model-development
**Difficulty:** hard

A team writes a one-line summary of when to use SageMaker SMP vs PyTorch FSDP for sharding a 30B-parameter model. Which framing is most accurate?

- B. FSDP is the native PyTorch sharding solution and is broadly supported; SMP adds AWS-specific optimisations and integrates tightly with SageMaker for very large transformer training - choose based on ecosystem fit and performance benchmarks.
- A. FSDP and SMP are identical with no differences.
- C. SMP is always strictly better than FSDP.
- D. FSDP cannot be used on SageMaker.

**Correct Answer:** B

**Explanation:**
<p>PyTorch FSDP is the native PyTorch sharding library widely used across hardware. SageMaker Model Parallel Library (SMP) is AWS's library with tight SageMaker integration and AWS-specific optimisations. Both shard model state across workers and serve similar purposes. The choice depends on ecosystem familiarity, integration with chosen training stacks (HF Trainer integrates with both), and benchmark results on the target hardware/workload. The MLA-C01 lesson is to articulate the trade-off without absolutist claims.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>FSDP is the off-the-shelf saw widely available; SMP is the local-shop saw tuned for the AWS workshop. Both cut, but the right choice depends on what tools you already use and what benchmarks show.</p>
<h3 id="option-analysis751">Option Analysis:751</h3>
<ul>
<li><strong>A</strong>：Wrong - they have implementation differences.</li>
<li><strong>B</strong>：Correct - FSDP native PyTorch; SMP AWS-optimised; pick on ecosystem fit and benchmarks.</li>
<li><strong>C</strong>：Wrong - SMP is not strictly better universally.</li>
<li><strong>D</strong>：Wrong - FSDP is supported on SageMaker.</li>
</ul>

## Question 18

**Domain:** ml-model-development
**Difficulty:** medium

A SaaS startup needs a forecasting model that predicts daily orders for each of its 9,000 customers. Each customer has between 30 and 800 days of history. Forecasts must be re-generated nightly. The team is small and prefers a managed AWS service that handles model training and per-item forecasting without bespoke ML pipelines. Which modeling approach is most appropriate?

- A. Use the SageMaker DeepAR built-in algorithm which is designed for many related time series and produces probabilistic forecasts for each item.
- B. Call Amazon Bedrock and prompt a foundation model to forecast next day orders from a CSV blob of history.
- C. Train a separate JumpStart vision model per customer using time-series-as-image conversion.
- D. Train a single XGBoost model on customer-level aggregates.

**Correct Answer:** A

**Explanation:**
<p>Forecasting many related time series is exactly the workload SageMaker DeepAR was built for. It learns a single global model across all 9,000 customer series, which is more robust than fitting 9,000 separate ARIMA models or rolling per-customer XGBoost. DeepAR ships as a SageMaker built-in algorithm so the small team does not need bespoke training code. FMs through Bedrock are a frequent MLA-C01 distractor in forecasting questions because they look like a simple solution but do not produce calibrated numeric forecasts, especially at thousands of independent series. Vision-of-time-series is a contrived option, and a single XGBoost on aggregates throws away the per-item history that matters. Recognising 'many related time series' as the DeepAR trigger phrase is a key MLA-C01 pattern.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine 9,000 different store managers each wanting tomorrow's order forecast. A single forecaster who has read all 9,000 logbooks and learned shared seasonal patterns (DeepAR) is faster and more accurate than fitting one tiny model per store, and far more reliable than asking a chatbot to guess.</p>
<h3 id="option-analysis1125">Option Analysis:1125</h3>
<ul>
<li><strong>A</strong>：Correct - DeepAR is the canonical AWS many-related-time-series tool and ships as a built-in algorithm.</li>
<li><strong>B</strong>：Wrong - FMs are not forecasting models and produce unreliable numeric predictions.</li>
<li><strong>C</strong>：Wrong - converting time series to images is an exotic non-solution.</li>
<li><strong>D</strong>：Wrong - aggregating loses per-customer temporal structure that DeepAR exploits.</li>
</ul>

## Question 19

**Domain:** ml-model-development
**Difficulty:** medium

A team's SageMaker training job repeatedly fails with insufficient disk space when downloading the input dataset to local storage. The job uses File mode and the dataset is 800 GB. The team prefers to keep File mode for compatibility reasons but solve the disk-space issue. Which SageMaker training-job parameter is the most direct fix?

- B. Increase the volume_size_in_gb on the training instance to provision a larger EBS volume.
- A. Switch the input mode to Pipe mode.
- C. Use a smaller training instance type to reduce overhead.
- D. Compress the dataset with gzip before training.

**Correct Answer:** B

**Explanation:**
<p>SageMaker training-job configuration includes a volume_size_in_gb parameter that provisions an EBS volume on the training instance. The default is 30 GB, far too small for an 800 GB dataset in File mode. Increasing this parameter is the direct fix. Switching to Pipe mode would also help but is excluded by the compatibility requirement. Smaller instances generally come with smaller volumes. Compressing the dataset is a workaround that does not fix the underlying provisioning issue. Recognising the volume_size_in_gb parameter as the lever for storage-bound File-mode training is a high-yield MLA-C01 detail.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>If your suitcase is too small for the trip, you buy a bigger suitcase, not lighter shoes. volume_size_in_gb is the suitcase size for SageMaker training instances.</p>
<h3 id="option-analysis809">Option Analysis:809</h3>
<ul>
<li><strong>A</strong>：Wrong - the stem excludes mode change for compatibility.</li>
<li><strong>B</strong>：Correct - volume_size_in_gb directly provisions a larger EBS volume.</li>
<li><strong>C</strong>：Wrong - smaller instances ship with smaller default volumes, worsening the issue.</li>
<li><strong>D</strong>：Wrong - gzip is a workaround that does not fix the volume size.</li>
</ul>

## Question 20

**Domain:** ml-solution-monitoring-maintenance-and-security
**Difficulty:** medium

A team uses IAM Access Analyzer for SageMaker. They want it to surface which actions a role HAS used in the last 90 days vs which actions are merely Allowed but never invoked. Which Access Analyzer feature is documented for this last-mile permission tightening?

- A. IAM Access Analyzer policy generation (also known as Access Analyzer policy generation from CloudTrail / 'Generate policy based on access activity'), which produces a least-privilege candidate policy from observed activity over a recent window.
- B. AWS Config rules
- C. AWS WAF
- D. Amazon QuickSight

**Correct Answer:** A

**Explanation:**
<p>IAM Access Analyzer can generate least-privilege policies from CloudTrail-observed activity over a configurable window (up to 90 days). For a given role, it produces a candidate IAM policy listing the actions that were actually invoked during the window — anything else in the existing policy is a candidate for removal. This is the documented last-mile tightening pattern after Role Manager produces an initial persona-based scope. Config rules check compliance state, WAF is for web traffic, QuickSight is BI. The MLA-C01 exam tests Access Analyzer's role in least-privilege workflows.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine reviewing a year's worth of credit-card statements and discovering you have not used a particular merchant authorisation. Cancel that merchant. Access Analyzer produces this 'unused authorisations' report for IAM permissions.</p>
<h3 id="option-analysis856">Option Analysis:856</h3>
<ul>
<li><strong>A</strong>：Correct because Access Analyzer policy generation produces a least-privilege candidate from observed activity.</li>
<li><strong>B</strong>：Wrong because Config rules check compliance, not produce policy candidates.</li>
<li><strong>C</strong>：Wrong because WAF is a web-application firewall.</li>
<li><strong>D</strong>：Wrong because QuickSight is BI.</li>
</ul>

## Question 21

**Domain:** ml-model-development
**Difficulty:** easy

An ML Engineer is required to track lineage and approval status for every model produced by SageMaker training jobs as part of a regulated MLOps process. Which AWS service is purpose-built for this?

- B. SageMaker Model Registry, which catalogs model versions with approval status, deployment metadata, and lineage tracking.
- A. SageMaker Feature Store.
- C. Amazon CodeCommit.
- D. Amazon Comprehend.

**Correct Answer:** B

**Explanation:**
<p>SageMaker Model Registry is the AWS-native model-version catalogue. Each registered model has versions, approval statuses (PendingManualApproval, Approved, Rejected), lineage links to training-job inputs, and deployment metadata. CI/CD pipelines (SageMaker Pipelines, Step Functions) gate deployment on approval status. For regulated MLOps, Model Registry is the only correct answer here. Feature Store catalogues features (different artifact). CodeCommit catalogues code. Comprehend is NLP. The MLA-C01 lesson is to know the four MLOps catalogues: Model Registry (models), Feature Store (features), CodeCommit (code), and Pipelines (workflows).</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Model Registry is the filing cabinet of model versions with approval stamps. Feature Store is the filing cabinet of feature definitions. They sit next to each other but hold different paperwork.</p>
<h3 id="option-analysis875">Option Analysis:875</h3>
<ul>
<li><strong>A</strong>：Wrong - Feature Store is for features, not models.</li>
<li><strong>B</strong>：Correct - Model Registry catalogs model versions with approval and lineage.</li>
<li><strong>C</strong>：Wrong - CodeCommit is for source code.</li>
<li><strong>D</strong>：Wrong - Comprehend is unrelated NLP service.</li>
</ul>

## Question 22

**Domain:** data-preparation-for-ml
**Difficulty:** hard

An ML Engineer's Data Wrangler Processing job hits OOM on ml.m5.4xlarge during a join. Which two-axis lever fixes this most directly?

- C. Choose a memory-optimised Processing instance (e.g., ml.r5.4xlarge or ml.r5.8xlarge) and/or increase instance_count to distribute the join across more workers (Spark).
- A. Use a smaller instance to force Spark to spill to disk.
- B. Switch to Amazon Comprehend.
- D. Disable encryption to free memory.

**Correct Answer:** C

**Explanation:**
<p>Data Wrangler Processing jobs run Spark for large joins. OOM during a join means executor memory is insufficient. The two levers are: (1) memory-optimised instance type (r5/r6 family) which gives more RAM per dollar, and (2) instance_count to distribute work across more executors. Spark partition tuning is also relevant. Smaller instances worsen OOM; Comprehend is irrelevant; disabling encryption is a violation. The MLA-C01 lesson is the type+count tuning pattern for Spark-backed Processing.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>OOM is the kitchen running out of counter space mid-recipe. Get a bigger counter (memory-optimised instance) or hire more cooks each with their own counter (more instances).</p>
<h3 id="option-analysis705">Option Analysis:705</h3>
<ul>
<li><strong>A</strong>：Wrong - smaller instance worsens OOM.</li>
<li><strong>B</strong>：Wrong - Comprehend is unrelated.</li>
<li><strong>C</strong>：Correct - memory-optimised type plus more workers.</li>
<li><strong>D</strong>：Wrong - encryption is unrelated and disabling is a violation.</li>
</ul>

## Question 23

**Domain:** deployment-and-orchestration-of-ml-workflows
**Difficulty:** medium

A platform team wants to deploy ML models to staging in account A and to production in account B as separate stages of one CodePipeline. Which AWS feature lets a single CodePipeline deploy to multiple accounts with isolation?

- D. Cross-account roles configured on the CodePipeline pipeline, where each deploy stage assumes a role in the target account that has only the permissions it needs in that account.
- A. Run two separate pipelines and synchronise them via S3 file flags.
- B. Use VPC peering to connect the two accounts.
- C. Share IAM users between the two accounts.

**Correct Answer:** D

**Explanation:**
<p>Cross-account CodePipeline deployment is the documented pattern for multi-account ML CI/CD. The pipeline is hosted in a 'tooling' or central account; each deploy stage is configured with a roleArn that points at a cross-account role in the target account (staging or production). The target-account role has a trust policy that permits the central account's CodePipeline service-role to assume it, and a permissions policy that grants only the minimum required actions (e.g., sagemaker:CreateEndpointConfig, sagemaker:UpdateEndpoint, iam:PassRole) on the target-account resources.</p>
<p>This pattern delivers strong isolation (each account has its own security boundary) plus a single auditable pipeline. Two separate pipelines double management overhead. VPC peering is for network, not IAM. Sharing IAM users violates security best practices. Cross-account roles is the right answer.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine a head office (CodePipeline) with key-cards that let the office manager unlock specific rooms in remote branches. Each branch issues its own key-card with limited access. The head office uses different cards for staging branch and production branch.</p>
<h3 id="option-analysis1173">Option Analysis:1173</h3>
<ul>
<li><strong>D</strong>：Correct — cross-account roles per stage in CodePipeline support multi-account deployment with strong isolation.</li>
<li><strong>A</strong>：Wrong — two pipelines double management overhead.</li>
<li><strong>B</strong>：Wrong — VPC peering is for network, not IAM.</li>
<li><strong>C</strong>：Wrong — sharing IAM users violates security best practices.</li>
</ul>

## Question 24

**Domain:** ml-model-development
**Difficulty:** medium

A team uses SageMaker Pipelines and wants the pipeline to register a model in Model Registry only when the evaluation report's accuracy exceeds 0.85. Which pipeline construct enables this?

- A. ConditionStep that reads the evaluation report's accuracy and gates RegisterModel based on the comparison.
- B. Always register every model regardless of evaluation.
- C. Manually approve every run before registration.
- D. Use AWS Glue to compare metrics.

**Correct Answer:** A

**Explanation:**
<p>SageMaker Pipelines includes ConditionStep, which evaluates a condition (e.g., ConditionGreaterThanOrEqualTo on a JSON path in the evaluation report) and routes execution to one set of steps if true and another if false. The team uses this to gate RegisterModel: register only when evaluation passes. Always registering, manual approval, and Glue are wrong-fit. The MLA-C01 lesson is to know ConditionStep as the gating construct in Pipelines.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>ConditionStep is the bouncer at the model registry door. Only models that pass the evaluation grade get in.</p>
<h3 id="option-analysis586">Option Analysis:586</h3>
<ul>
<li><strong>A</strong>：Correct - ConditionStep gates downstream steps based on metrics.</li>
<li><strong>B</strong>：Wrong - always registering ignores the evaluation gate.</li>
<li><strong>C</strong>：Wrong - manual approval is fragile.</li>
<li><strong>D</strong>：Wrong - Glue is ETL.</li>
</ul>

## Question 25

**Domain:** deployment-and-orchestration-of-ml-workflows
**Difficulty:** medium

A team wants to automatically retrain their fraud-detection model every Monday at 03:00 UTC AND any time SageMaker Model Monitor detects feature drift above the configured threshold. The retraining must trigger CodePipeline (which then runs SageMaker Pipelines). Which combination of AWS features wires both triggers?

- A. EventBridge Scheduler for the cron, plus EventBridge rules on the Model Monitor MonitoringJobStatus / Schedule events with Constraint Violations, both targeting CodePipeline StartPipelineExecution.
- B. Lambda function that runs every minute polling Model Monitor and the calendar.
- C. An always-on EC2 instance running cron and a custom drift checker.
- D. Manual trigger by a data scientist who watches drift dashboards.

**Correct Answer:** A

**Explanation:**
<p>Automated retraining triggers fall into two categories that are both event-driven on AWS:</p>
<ol>
<li>
<p>Schedule-based: EventBridge Scheduler (the modern replacement for CloudWatch Events scheduled rules) supports cron and rate expressions and can target CodePipeline StartPipelineExecution natively.</p>
</li>
<li>
<p>Drift-based: SageMaker Model Monitor emits events about monitoring schedule executions and constraint violations to the EventBridge default bus. An EventBridge rule that matches on the 'SageMaker Model Monitoring Schedule Status' detail-type with detail.MonitoringStatus = CompletedWithViolations (or similar) targets CodePipeline StartPipelineExecution, kicking off retraining as soon as drift is detected.</p>
</li>
</ol>
<p>Both triggers feed the same retraining pipeline, ensuring the production model is refreshed both on a fixed cadence and reactively on drift. Lambda polling is wasteful; self-managed EC2 reinvents the feature; manual triggers fail the automatic requirement. EventBridge Scheduler + EventBridge rules is the right answer.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine a smoke alarm and a calendar both wired to call the fire department: the calendar triggers an annual fire drill, the smoke alarm triggers an emergency call. EventBridge is the wiring; CodePipeline is the fire department; either trigger gets the same response.</p>
<h3 id="option-analysis1326">Option Analysis:1326</h3>
<ul>
<li><strong>A</strong>：Correct — EventBridge Scheduler for cron + EventBridge rules on Model Monitor drift events both target CodePipeline StartPipelineExecution.</li>
<li><strong>B</strong>：Wrong — Lambda polling is wasteful; EventBridge is event-driven.</li>
<li><strong>C</strong>：Wrong — self-managed EC2 reinvents the feature.</li>
<li><strong>D</strong>：Wrong — manual trigger fails the automatic requirement.</li>
</ul>

## Question 26

**Domain:** ml-model-development
**Difficulty:** medium

A media company wants to summarise 10,000 internal meeting transcripts per day. The transcripts contain product code names that no public foundation model knows, but the summary style is generic ('action items', 'decisions', 'open questions'). The CIO wants minimum operational complexity, accepts a small per-call cost, and wants the system live within four weeks. Which approach should the ML Engineer recommend?

- A. Use Amazon Bedrock with a foundation model and supply the product code-name glossary in the system prompt for each request.
- B. Continued pretraining of a SageMaker JumpStart Llama checkpoint on three years of transcripts.
- C. Train a custom encoder-decoder transformer from scratch on the company's transcripts.
- D. Use Amazon Comprehend's built-in entity extraction and write the summary by concatenating extracted entities.

**Correct Answer:** A

**Explanation:**
<p>The interesting tension here is that the team has domain vocabulary the FM does not know (product code names) but the task is otherwise generic. The MLA-C01 decision tree puts prompt engineering ahead of fine-tuning whenever the customisation can fit in a prompt or system message - and a glossary of code names easily does. Bedrock therefore wins on operational complexity, time-to-launch, and cost. Continued pretraining and from-scratch training are heavyweight options reserved for cases where the FM's foundational knowledge is wrong or where regulatory ownership demands a custom artifact. Comprehend entity extraction is a different task type entirely. Recognising 'small glossary that fits in a prompt' as a prompt-engineering signal is a high-value MLA-C01 skill.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine asking a meeting note-taker to use the company's nicknames. You hand them a one-page glossary at the start of the meeting (system prompt) - you do not send them to night school for six months (continued pretraining). The cheap fix solves the whole problem.</p>
<h3 id="option-analysis1072">Option Analysis:1072</h3>
<ul>
<li><strong>A</strong>：Correct - Bedrock + prompt-injected glossary is the lowest-overhead path that handles both generic summary and code names.</li>
<li><strong>B</strong>：Wrong - continued pretraining is overkill for a glossary that fits in a prompt.</li>
<li><strong>C</strong>：Wrong - from-scratch training cannot meet a four-week deadline.</li>
<li><strong>D</strong>：Wrong - Comprehend extracts entities but does not produce coherent narrative summaries.</li>
</ul>

## Question 27

**Domain:** deployment-and-orchestration-of-ml-workflows
**Difficulty:** hard

A team wants to host 50 candidate model variants behind one SageMaker real-time endpoint and let the caller select which variant to invoke per request via the TargetVariant header. They are running into an EndpointConfig limit on the number of ProductionVariants. Which alternative pattern is the AWS-canonical fit when the count of independent models exceeds the practical ProductionVariant limit?

- A. Use a SageMaker Multi-Model Endpoint where each model artifact lives in S3 and the caller selects via the TargetModel header.
- B. Create 50 ProductionVariants weighted equally.
- C. Create 50 separate endpoints behind ALB.
- D. Use Shadow Testing with 50 shadow variants.

**Correct Answer:** A

**Explanation:**
<p>ProductionVariants are designed for low N (typically 2-5 — for A/B tests, canary deployments, and a small set of competing models). EndpointConfig has practical limits on the number of variants it can hold. When the count of independent caller-selectable models is large (tens or thousands), the right pattern is the Multi-Model Endpoint (MME): all model artifacts live in a shared S3 prefix, the InvokeEndpoint call carries a TargetModel header naming the artifact, and the endpoint lazy-loads each model into memory on first request, evicting LRU models on memory pressure.</p>
<p>For 50 caller-selectable models sharing a container image, MME is the right tool. 50 ProductionVariants strains EndpointConfig limits. 50 endpoints multiply idle cost. Shadow does not return responses. MME is the right answer.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine the difference between hosting four chefs in a kitchen (ProductionVariants — small N, shared concurrent service) and a library with 50 cookbooks (Multi-Model Endpoint — large N, fetch on demand). Trying to fit 50 chefs in the same kitchen does not scale; switching to a cookbook library does.</p>
<h3 id="option-analysis1139">Option Analysis:1139</h3>
<ul>
<li><strong>A</strong>：Correct — Multi-Model Endpoints scale to thousands of caller-selectable models behind one endpoint.</li>
<li><strong>B</strong>：Wrong — 50 ProductionVariants strains EndpointConfig limits and is the wrong tool for caller-selectable models at this count.</li>
<li><strong>C</strong>：Wrong — 50 endpoints multiply idle instance cost.</li>
<li><strong>D</strong>：Wrong — Shadow variants do not return responses to clients.</li>
</ul>

## Question 28

**Domain:** ml-solution-monitoring-maintenance-and-security
**Difficulty:** easy

A team realises a SageMaker execution role's policy grants s3:* on all buckets in the account. They want to scope it to only the team's bucket. Which least-privilege rewrite is BEST?

- A. Replace the broad Allow with a specific Allow on s3:GetObject, s3:PutObject, s3:ListBucket, scoped to arn:aws:s3:::team-bucket and arn:aws:s3:::team-bucket/* via Resource constraints.
- B. Keep s3:* but add a tag-based Condition
- C. Add iam:* to the role
- D. Disable the role entirely

**Correct Answer:** A

**Explanation:**
<p>Least-privilege rewrites tighten Action and Resource together. Replace s3:* on Resource '<em>' with the minimum actions actually used (s3:GetObject, s3:PutObject, s3:ListBucket, s3:DeleteObject if needed) and the minimum resources (arn:aws:s3:::team-bucket and arn:aws:s3:::team-bucket/</em>). For object actions you also typically include the bucket itself for s3:ListBucket. Tag conditions can complement but should not be a substitute for action/resource scoping; iam:* is unrelated and dangerous; disabling the role breaks legitimate flows. The MLA-C01 exam tests this rewrite pattern.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>A grocery-store master key opens every aisle. The least-privilege fix is a specific aisle key for each employee. Replace s3:* with action-scoped, resource-scoped permissions and the role goes from master key to single-aisle key.</p>
<h3 id="option-analysis846">Option Analysis:846</h3>
<ul>
<li><strong>A</strong>：Correct because action-and-resource-scoped Allow is the canonical least-privilege rewrite.</li>
<li><strong>B</strong>：Wrong because s3:* with tag conditions is still too broad.</li>
<li><strong>C</strong>：Wrong because iam:* is dangerous and unrelated.</li>
<li><strong>D</strong>：Wrong because disabling the role breaks legitimate access.</li>
</ul>

## Question 29

**Domain:** data-preparation-for-ml
**Difficulty:** easy

An ML Engineer's real-time fraud-detection endpoint must look up customer features in single-digit milliseconds during inference. Which SageMaker Feature Store mode satisfies this latency requirement?

- C. Online store, which provides low-latency (millisecond) GetRecord reads optimised for real-time inference.
- A. Offline store only.
- B. Amazon S3 Glacier Deep Archive.
- D. Amazon S3 Standard with no Feature Store at all.

**Correct Answer:** C

**Explanation:**
<p>SageMaker Feature Store has two coordinated storage tiers. The online store is a low-latency key-value store providing millisecond GetRecord reads keyed by record identifier - the right tier for real-time inference. The offline store is S3-backed and Athena-queryable - the right tier for training and batch jobs but with seconds-to-minutes latency. Glacier and plain S3 are wrong tools. The MLA-C01 lesson is to recognise the online tier as the millisecond-latency tier.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Online store is the front-counter cash register: instant access. Offline store is the warehouse: bulk and cheap but slower. Real-time inference needs the cash register.</p>
<h3 id="option-analysis675">Option Analysis:675</h3>
<ul>
<li><strong>A</strong>：Wrong - offline store latency is too high for real-time.</li>
<li><strong>B</strong>：Wrong - Glacier latency is hours.</li>
<li><strong>C</strong>：Correct - online store provides millisecond reads.</li>
<li><strong>D</strong>：Wrong - plain S3 is not key-value optimised.</li>
</ul>

## Question 30

**Domain:** ml-solution-monitoring-maintenance-and-security
**Difficulty:** medium

An MLOps team needs to differentiate between two CloudWatch metric namespaces emitted by Model Monitor. They observe metrics under aws/sagemaker/Endpoints/data-metrics and aws/sagemaker/Endpoints/model-metrics. What do these two namespaces correspond to?

- A. data-metrics is for Bias Drift; model-metrics is for Feature Attribution Drift.
- B. data-metrics is for Data Capture volume statistics; model-metrics is for endpoint latency.
- C. data-metrics is for Data Quality monitor outputs; model-metrics is for Model Quality monitor outputs.
- D. Both namespaces hold identical content but are duplicated for redundancy.

**Correct Answer:** C

**Explanation:**
<p>The aws/sagemaker/Endpoints/data-metrics namespace receives Data Quality monitor outputs — per-feature distribution-distance and constraint-violation counts. The aws/sagemaker/Endpoints/model-metrics namespace receives Model Quality monitor outputs — accuracy, precision, recall, F1, AUC for classification or MAE/MSE/RMSE/R^2 for regression. Bias Drift and Feature Attribution Drift have their own dedicated namespaces (bias-metrics and feature-metrics, respectively). Endpoint latency and Data Capture volume metrics are emitted under separate AWS/SageMaker namespaces and dimensions. Knowing the right namespace is essential for building CloudWatch alarms; the MLA-C01 exam tests this directly.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Picture two filing cabinets in the office: one labelled 'data drift' and one labelled 'model quality'. Each scheduled monitor writes its readings into the right cabinet. CloudWatch alarms are like sticky notes you tape to the cabinet drawers — but you have to know which cabinet first.</p>
<h3 id="option-analysis1018">Option Analysis:1018</h3>
<ul>
<li><strong>A</strong>：Wrong because Bias and Attribution Drift have their own dedicated namespaces.</li>
<li><strong>B</strong>：Wrong because capture volume and latency metrics live in different namespaces.</li>
<li><strong>C</strong>：Correct because data-metrics maps to Data Quality outputs and model-metrics maps to Model Quality outputs.</li>
<li><strong>D</strong>：Wrong because the two namespaces hold different content; they are not duplicates.</li>
</ul>

## Question 31

**Domain:** ml-solution-monitoring-maintenance-and-security
**Difficulty:** easy

An ML team wants to encrypt the EBS volume attached to each training-job instance, so any temporary checkpoints, intermediate data, or scratch files written there are encrypted with a customer-managed CMK. Which CreateTrainingJob field controls this?

- A. ResourceConfig.VolumeKmsKeyId
- B. OutputDataConfig.KmsKeyId
- C. InputDataConfig.S3Uri
- D. EnableInterContainerTrafficEncryption

**Correct Answer:** A

**Explanation:**
<p>The CreateTrainingJob API exposes ResourceConfig.VolumeKmsKeyId for explicitly encrypting the per-instance EBS volume with a customer-managed KMS CMK. This covers temporary scratch space, downloaded training data on the volume, intermediate checkpoints written locally, and any other on-disk state. OutputDataConfig.KmsKeyId encrypts the final model artifact uploaded to S3. EnableInterContainerTrafficEncryption encrypts traffic between distributed-training containers (using TLS). InputDataConfig.S3Uri is just the data source location. The MLA-C01 exam tests these specific knobs.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Different locks on different surfaces: VolumeKmsKeyId locks the kitchen counter (EBS volume), OutputDataConfig.KmsKeyId locks the takeaway box (S3 model artifact), and EnableInterContainerTrafficEncryption secures the conveyor belts between cooks. Pick the right lock for the right surface.</p>
<h3 id="option-analysis909">Option Analysis:909</h3>
<ul>
<li><strong>A</strong>：Correct because VolumeKmsKeyId encrypts the EBS volume of training instances.</li>
<li><strong>B</strong>：Wrong because OutputDataConfig.KmsKeyId encrypts S3 outputs, not the EBS volume.</li>
<li><strong>C</strong>：Wrong because S3Uri has no relation to volume encryption.</li>
<li><strong>D</strong>：Wrong because that field encrypts inter-container traffic, not the volume.</li>
</ul>

## Question 32

**Domain:** deployment-and-orchestration-of-ml-workflows
**Difficulty:** hard

A SageMaker Pipeline execution failed at the TrainingStep with status Failed. The team wants to fix the bug in the training script and retry, but they want to avoid re-running the upstream ProcessingStep that succeeded (cost and time). Which SageMaker Pipelines feature lets them retry from the failed step using the cached output of upstream successful steps?

- A. Re-execute the pipeline with step caching enabled (CacheConfig); SageMaker reuses the upstream ProcessingStep's cached output and re-runs only the TrainingStep.
- B. Use the SageMaker Pipeline 'partial retry' button that retries only the failed step.
- C. Manually copy the ProcessingStep output to a new S3 location and rewrite the pipeline.
- D. Delete the SageMaker Pipeline and recreate it with a different name.

**Correct Answer:** A

**Explanation:**
<p>SageMaker Pipelines does not have a per-step retry button. The documented mechanism for 'fix a downstream step and re-run while skipping unchanged upstream steps' is step caching (CacheConfig). With caching enabled, SageMaker hashes each step's inputs (image URI, hyperparameters, data, code) on each execution; if a previous successful execution with the same hash exists within the ExpireAfter window, SageMaker reuses the cached result and skips re-execution. The team fixes the bug in the training script (which changes the TrainingStep's hash, invalidating its cache), then re-executes the pipeline. The unchanged ProcessingStep hits the cache and is skipped; only the TrainingStep re-runs.</p>
<p>There is no partial-retry primitive. Manual S3 copy reinvents caching. Deleting the pipeline forces a full rerun. Step caching is the right answer.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine a software build that recompiles only the source files you edited and re-uses the rest from the build cache. SageMaker Pipelines step caching is that build cache: re-run only what changed.</p>
<h3 id="option-analysis1076">Option Analysis:1076</h3>
<ul>
<li><strong>A</strong>：Correct — step caching reuses upstream cached outputs when inputs are unchanged, re-running only the changed step.</li>
<li><strong>B</strong>：Wrong — there is no partial-retry button on SageMaker Pipelines.</li>
<li><strong>C</strong>：Wrong — manual copy reinvents caching and is fragile.</li>
<li><strong>D</strong>：Wrong — deletion forces a full rerun.</li>
</ul>

## Question 33

**Domain:** data-preparation-for-ml
**Difficulty:** hard

A Glue Spark engineer is optimizing a join between a 2 TB transactions table and a 200 MB lookup table of country codes. Currently the job uses a default sort-merge join and is slow due to shuffle. The engineer asks which Spark optimization most cleanly addresses small-large joins where the smaller side fits comfortably in memory on every executor. Which optimization is the right answer?

- A. Configure a broadcast join hint so Spark replicates the small table to every executor and avoids shuffling the large side.
- B. Disable Spark and switch to Glue Python shell.
- C. Repartition the small table into 1000 partitions to match the large table.
- D. Run the join twice in parallel to halve the time.

**Correct Answer:** A

**Explanation:**
<p>A <strong>broadcast join</strong> is the canonical Spark optimization for joining a large table with a small one when the small side fits comfortably in memory on every executor (typically under a few hundred MB by default, configurable via <code>spark.sql.autoBroadcastJoinThreshold</code>). Spark replicates the small side to every executor and performs the join locally without shuffling the large side. The 200 MB lookup table in the stem is well within the broadcast threshold; broadcasting eliminates the shuffle on the 2 TB transactions table and dramatically speeds up the join.</p>
<p>Switching to Python shell loses Spark entirely. Heavy repartitioning of the small table adds shuffle without benefit. Running the join twice doubles cost without reducing time. The MLA-C01 stem signal 'small-large join with the small side fitting in memory' is the broadcast join signature.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Broadcast join is handing every chef in the kitchen a personal copy of the day's special menu so they don't have to walk to the office every time they need it. Sort-merge join is making every chef walk to the office for every dish. For a small menu and a busy kitchen, broadcast wins.</p>
<h3 id="option-analysis1174">Option Analysis:1174</h3>
<ul>
<li><strong>A</strong>：Correct because broadcast joins are the canonical Spark optimization for small-large joins where the small side fits in executor memory.</li>
<li><strong>B</strong>：Wrong because Python shell cannot run Spark joins of this scale.</li>
<li><strong>C</strong>：Wrong because heavy repartitioning of the small table adds shuffle without benefit.</li>
<li><strong>D</strong>：Wrong because running the join twice doubles compute cost without reducing time.</li>
</ul>

## Question 34

**Domain:** data-preparation-for-ml
**Difficulty:** medium

A finance team has a model that needs to read features from three sources: a curated S3 Parquet prefix, an Amazon Aurora PostgreSQL operational database, and an Amazon RDS for SQL Server reference table. The data scientist wants to write a single SQL query that joins all three sources for an exploratory analysis and prefers not to ETL all three into S3 first because the analysis is one-off and small in result size. Which AWS service supports federated SQL queries across S3, Aurora, and RDS without an upfront ETL stage?

- A. Amazon Redshift Spectrum, which queries S3 only.
- B. AWS Glue Crawlers running across all three sources to produce a unified Catalog table.
- C. Amazon DynamoDB streams replicating all three sources into one DynamoDB table for unified queries.
- D. Amazon Athena with Federated Query (using the appropriate Lambda data source connectors for Aurora and RDS).

**Correct Answer:** D

**Explanation:**
<p>Amazon Athena Federated Query extends Athena's SQL execution across multiple data sources by using AWS Lambda-based <strong>data source connectors</strong>. For each non-S3 source — Aurora, RDS, DynamoDB, Snowflake, Redshift, and many others — a connector Lambda implements the Athena federation protocol. The data scientist writes a single SQL query referencing logical tables from all three sources; Athena's planner pushes predicates to each connector, fetches result sets, and joins them at the query layer. There is no upfront ETL into S3, which is exactly what the data scientist wants for a one-off exploratory analysis with a small result set.</p>
<p>Redshift Spectrum extends Redshift's SQL to query S3 — it does not federate to Aurora or RDS. Glue Crawlers infer schema and update the Catalog; they do not execute federated queries. The DynamoDB-based architecture described does not exist as an AWS pattern. For sustained, large-scale joins between sources, ETL into S3 plus Athena/Redshift is more cost-efficient than federation; for exploratory one-offs with small result sizes, Athena Federated Query is the right tool.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Federated query is the universal-translator passport: walk into three different countries with one document and they all let you ask questions in your own language. The translator (Lambda connector) handles the local language. Bringing all three countries' citizens to a single embassy first (ETL into S3) is overkill if you just have a quick question.</p>
<h3 id="option-analysis1502">Option Analysis:1502</h3>
<ul>
<li><strong>A</strong>：Wrong because Redshift Spectrum queries S3 only and does not federate to Aurora or RDS.</li>
<li><strong>B</strong>：Wrong because Crawlers infer schema; they do not execute federated queries.</li>
<li><strong>C</strong>：Wrong because DynamoDB is not a federated SQL query engine and the architecture does not exist.</li>
<li><strong>D</strong>：Correct because Athena Federated Query supports SQL across S3 and relational sources via Lambda connectors.</li>
</ul>

## Question 35

**Domain:** data-preparation-for-ml
**Difficulty:** medium

An ML Engineer is summarising for the team how Feature Store helps prevent train-serve skew. Which framing is most accurate?

- C. By writing each feature once into Feature Store and reading from the same feature group at training (offline) and inference (online), engineers avoid maintaining two parallel feature pipelines that drift apart.
- A. Feature Store automatically rewrites inference inputs to match training; no engineering effort needed.
- B. Train-serve skew is impossible by definition.
- D. Feature Store is unrelated to skew.

**Correct Answer:** C

**Explanation:**
<p>Train-serve skew - features computed differently at training and inference - is one of production ML's most common failure modes. Feature Store's design directly addresses it: the offline store is the training data source and the online store is the inference data source, and both are populated from the same ingestion path. Engineers commit to one pipeline writing to the feature group, and both consumers read from it. Universal claims (auto-fix, impossible, unrelated) are wrong. The MLA-C01 lesson is to articulate Feature Store's role in skew prevention.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Feature Store is the single recipe book read by both the prep kitchen (training) and the line kitchen (inference). When everyone reads the same book, the dish tastes the same in both kitchens.</p>
<h3 id="option-analysis788">Option Analysis:788</h3>
<ul>
<li><strong>A</strong>：Wrong - Feature Store does not auto-rewrite.</li>
<li><strong>B</strong>：Wrong - skew is real and common.</li>
<li><strong>C</strong>：Correct - single source of truth via dual-tier model prevents skew.</li>
<li><strong>D</strong>：Wrong - Feature Store directly addresses skew.</li>
</ul>

## Question 36

**Domain:** deployment-and-orchestration-of-ml-workflows
**Difficulty:** medium

A team wants to subscribe to a third-party fraud-detection model published on AWS Marketplace and use it inside their SageMaker Pipeline like any other model. Which Registry concept represents a Marketplace-purchased model that the team can deploy into their account?

- A. An AWS Marketplace ML model subscription appears as a Model Package in the team's account, deployable like any internally-trained Model Package via CreateModel and EndpointConfig.
- B. Marketplace models cannot be used inside SageMaker Pipelines.
- C. Marketplace models are stored in the seller's account and referenced via cross-account roles.
- D. Marketplace models are downloadable as ZIP files.

**Correct Answer:** A

**Explanation:**
<p>AWS Marketplace ML models are delivered through the SageMaker Model Package mechanism. When a customer subscribes to a Marketplace ML model, AWS provisions a Model Package in the customer's account that references the seller's container image and artifact (the seller controls these and may charge per-hour or per-inference fees). The customer deploys the Marketplace Model Package the same way as an internally-trained Model Package: CreateModel referencing the Model Package ARN, EndpointConfig, UpdateEndpoint.</p>
<p>Marketplace Model Packages can also be referenced by SageMaker Pipelines (as the source for a CreateModelStep), letting customers integrate purchased models into their workflows alongside their own.</p>
<p>Claiming Marketplace models cannot be used in Pipelines is wrong. They are not cross-account references. They are not ZIP downloads. The Marketplace-as-Model-Package answer is correct.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine subscribing to a magazine — your subscription generates a copy delivered to your mailbox each month. AWS Marketplace ML models are like that subscription: the subscription generates a Model Package in your account, and you treat it like any other model.</p>
<h3 id="option-analysis1196">Option Analysis:1196</h3>
<ul>
<li><strong>A</strong>：Correct — Marketplace ML models appear as Model Packages in the buyer's account, deployable like any internal Model Package.</li>
<li><strong>B</strong>：Wrong — Marketplace models can be used in Pipelines.</li>
<li><strong>C</strong>：Wrong — they are delivered to the buyer's account, not via cross-account references.</li>
<li><strong>D</strong>：Wrong — they are not ZIP downloads.</li>
</ul>

## Question 37

**Domain:** deployment-and-orchestration-of-ml-workflows
**Difficulty:** medium

A real-time SageMaker endpoint hosts a fraud-scoring model behind two ml.c5.2xlarge instances. During morning peak the endpoint sees CPU climb to 78% and per-instance request latency rises from 25 ms to 110 ms. Off-peak it drops to 12% CPU. The team wants to add Application Auto Scaling so the fleet expands during peak and contracts off-peak. Which scaling policy and metric should they configure as the primary auto-scaling signal for an inference endpoint?

- A. Step scaling on CPUUtilization with thresholds at 60% and 80%.
- B. Target tracking on the SageMakerVariantInvocationsPerInstance CloudWatch metric.
- C. Scheduled scaling that doubles instance count at 8 AM and halves it at 8 PM.
- D. Manual scaling adjusted by an on-call engineer based on latency dashboards.

**Correct Answer:** B

**Explanation:**
<p>SageMaker Application Auto Scaling supports target tracking, step scaling, and scheduled scaling for real-time endpoints. AWS documentation explicitly recommends target tracking on the CloudWatch metric SageMakerVariantInvocationsPerInstance as the primary scaling signal, because it directly measures per-instance request load (RPS divided by instance count). You set a target value (e.g., 50 invocations per instance per minute) and the service adds or removes instances to keep the metric near the target.</p>
<p>CPU and memory utilisation are usable as fallback metrics but are proxies — they can stay flat while latency tail rises (e.g., when a model is I/O bound). Scheduled scaling complements target tracking but is not a substitute. Manual scaling is the antipattern. Target tracking on InvocationsPerInstance is the textbook answer on MLA-C01.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine a coffee shop where the manager watches 'cups served per barista' and adds another barista when the number gets too high. That is target tracking on InvocationsPerInstance. Watching CPU is like watching how fast the espresso machine spins — informative but indirect. Watching the actual queue per worker is the most direct signal.</p>
<h3 id="option-analysis1221">Option Analysis:1221</h3>
<ul>
<li><strong>A</strong>：Wrong — CPU is a fallback proxy; AWS recommends InvocationsPerInstance as the primary signal.</li>
<li><strong>B</strong>：Correct — target tracking on SageMakerVariantInvocationsPerInstance is the documented best-practice metric for real-time endpoint auto-scaling.</li>
<li><strong>C</strong>：Wrong — scheduled scaling complements but does not replace dynamic target tracking.</li>
<li><strong>D</strong>：Wrong — manual scaling is fragile and unnecessary when target tracking is available.</li>
</ul>

## Question 38

**Domain:** ml-solution-monitoring-maintenance-and-security
**Difficulty:** medium

An ML team uses SageMaker Pipelines and stores pipeline artefacts in S3 with a customer-managed CMK. Which IAM principal must be granted kms:Decrypt access on the CMK?

- A. The pipeline's execution role (used by SageMaker Pipelines steps to access the CMK-encrypted artefacts) — and any other downstream services or roles that read the artefacts.
- B. The CloudFront distribution role
- C. The Glacier vault role
- D. Anonymous public users

**Correct Answer:** A

**Explanation:**
<p>SageMaker Pipelines uses an execution role (specified via RoleArn on the pipeline) that the pipeline's individual steps assume to perform their work. To read CMK-encrypted artefacts written by previous steps, this execution role needs kms:Decrypt on the CMK (and DescribeKey for metadata inspection). Both the role's IAM policy and the CMK's key policy must allow the call. Any downstream consumer (Lambda functions, additional services) likewise needs kms:Decrypt. CloudFront, Glacier, and public users are not relevant. The MLA-C01 exam tests this Pipelines-CMK alignment.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>If your pipeline is a relay race and each runner hands the next a sealed envelope, every runner needs the key (kms:Decrypt) to open the envelope. Granting it to only the first runner breaks the race.</p>
<h3 id="option-analysis809">Option Analysis:809</h3>
<ul>
<li><strong>A</strong>：Correct because pipeline execution roles need kms:Decrypt for SSE-KMS artefact reads.</li>
<li><strong>B</strong>：Wrong because CloudFront is unrelated.</li>
<li><strong>C</strong>：Wrong because Glacier is unrelated.</li>
<li><strong>D</strong>：Wrong because public access is a security anti-pattern.</li>
</ul>

## Question 39

**Domain:** ml-solution-monitoring-maintenance-and-security
**Difficulty:** hard

A SageMaker pipeline deploys a model to an endpoint. A data scientist accidentally uploads a malicious model artifact to S3, replacing the trusted artifact. The endpoint then loads the malicious artifact at scale. Which AWS feature is documented to PREVENT this artifact-substitution risk?

- A. S3 Bucket Versioning combined with S3 Object Lock retention and an IAM policy requiring versioned reads (s3:GetObjectVersion) on a specific version-id, so the SageMaker model registers a specific immutable version of the artifact rather than the bucket's current pointer.
- B. Disable encryption to detect tampering
- C. Make the bucket public
- D. Switch to AWS Glue

**Correct Answer:** A

**Explanation:**
<p>Artifact substitution is mitigated by storing model artifacts in an S3 bucket with Versioning enabled, Object Lock with retention to prevent deletion or overwrite, and SageMaker model registration that pins a specific version-id rather than the latest pointer. Optionally combine with explicit IAM policy denials for s3:DeleteObject, s3:PutObject on the model-artifact prefix and CloudTrail data-event logging on the prefix to detect any attempt. Disabling encryption, making buckets public, and switching to Glue are security anti-patterns or unrelated. The MLA-C01 exam tests immutable-artifact patterns.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine signing a courier-delivered package with both a serial number (version-id) and a tamper-proof seal (Object Lock). Even if someone slips a different package into the warehouse, the delivery contract refuses anything but the originally signed serial. Same idea for model artifacts pinned by version-id.</p>
<h3 id="option-analysis950">Option Analysis:950</h3>
<ul>
<li><strong>A</strong>：Correct because Versioning + Object Lock + version-pinned reads produce tamper-resistant artifacts.</li>
<li><strong>B</strong>：Wrong because disabling encryption is a security catastrophe.</li>
<li><strong>C</strong>：Wrong because public access is the opposite of required.</li>
<li><strong>D</strong>：Wrong because Glue is for ETL, unrelated to artifact substitution.</li>
</ul>

## Question 40

**Domain:** deployment-and-orchestration-of-ml-workflows
**Difficulty:** medium

An ML team has built an inference flow that requires three sequential containers: a preprocessing container that normalises images, a feature-extractor based on ResNet50, and a downstream classifier that returns the final label. Each container is independently versioned by a different team. The architects want all three behind a single SageMaker endpoint, deployed together, callable as one invocation that internally chains the containers in order, and reusing one fleet of instances. Which SageMaker endpoint capability matches this requirement?

- A. Three separate real-time endpoints connected by Step Functions Express Workflows.
- B. A SageMaker Multi-Model Endpoint hosting three artifacts.
- C. A SageMaker Inference Pipeline that hosts the three containers in sequence behind one real-time endpoint.
- D. Three Lambda functions wired together via SQS.

**Correct Answer:** C

**Explanation:**
<p>SageMaker Inference Pipelines let you deploy 2-15 containers in a linear pipeline behind a single real-time endpoint. Each container's response is fed as the request to the next container, all within one InvokeEndpoint call. The pipeline runs on one fleet, sharing instances, and exposes a single auto-scaling target. This is the canonical answer for the pre-process / extract / classify pattern, and is widely used to combine SageMaker built-in feature processors (Scikit-Learn, SparkML) with downstream framework containers.</p>
<p>Multi-Container Endpoints in 'direct invocation mode' are a different concept — they let the caller name which container to invoke, used for hosting independent models on shared infra. MMEs lazy-load many independent artifacts but do not chain them. Step Functions or Lambda glue would work but adds latency and complexity that the native Inference Pipeline avoids.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine a sushi conveyor where the rice presser, the topping placer, and the wrapper sit in a line, each doing one step. The customer clicks one button and the finished roll comes out. Inference Pipelines are that conveyor: one endpoint, three containers chained, one click yields one final answer.</p>
<h3 id="option-analysis1227">Option Analysis:1227</h3>
<ul>
<li><strong>A</strong>：Wrong — three endpoints plus Step Functions multiplies cost and adds orchestration latency.</li>
<li><strong>B</strong>：Wrong — MMEs are independent artifacts loaded on demand, not a sequential pipeline.</li>
<li><strong>C</strong>：Correct — Inference Pipelines chain 2-15 containers behind one endpoint with output-as-input wiring.</li>
<li><strong>D</strong>：Wrong — Lambda+SQS is not a SageMaker endpoint and lacks the hosting features the team needs.</li>
</ul>

## Question 41

**Domain:** deployment-and-orchestration-of-ml-workflows
**Difficulty:** hard

A SageMaker Pipeline executes successfully on first run but fails on subsequent runs at the TrainingStep with the error 'Step caching enabled but cache hit returned a TrainingStep output that does not match the current pipeline variables'. The team did not change the training code. What is the most likely cause and the AWS-canonical fix?

- D. Pipeline parameters or step inputs that produce non-deterministic hashes (e.g., timestamp injected into hyperparameters) cause cache misses or stale matches; remove non-deterministic inputs from the cache hash or disable caching for that step.
- A. SageMaker Pipelines step caching is unreliable and should never be enabled.
- B. Disable IAM roles on the pipeline.
- C. Switch to Step Functions; SageMaker Pipelines does not support caching.

**Correct Answer:** D

**Explanation:**
<p>SageMaker Pipelines step caching hashes a step's inputs — image URI, instance type, hyperparameters, code, data sources, role, and other configuration — to determine cache reuse. If any of those inputs vary between runs in a way that is not meaningful (e.g., a timestamp injected into hyperparameters, a random seed passed as a parameter, a non-deterministic temp S3 path), the hash drifts every run, producing cache misses or stale matches that confuse downstream steps. The AWS-canonical fix is to remove non-deterministic inputs from the cache-hash surface — replace timestamp-based parameters with stable values, or disable caching for the step where determinism cannot be guaranteed.</p>
<p>Claiming caching is unreliable misses the determinism root cause. Disabling IAM roles is unrelated and insecure. Step Functions does not solve this; the same determinism issue exists there. Removing non-determinism is the right answer.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine a photocopy machine with a 'reuse last copy' optimisation that hashes the page content. If you stamp 'today's date' on every page, every page hashes differently and the optimisation never triggers. Removing the date stamp restores caching.</p>
<h3 id="option-analysis1208">Option Analysis:1208</h3>
<ul>
<li><strong>D</strong>：Correct — non-deterministic inputs in the cache hash cause drift; remove non-determinism or disable caching for the affected step.</li>
<li><strong>A</strong>：Wrong — caching is reliable when inputs are deterministic.</li>
<li><strong>B</strong>：Wrong — disabling IAM roles is unrelated and insecure.</li>
<li><strong>C</strong>：Wrong — Step Functions has the same determinism requirement; switching does not solve the problem.</li>
</ul>

## Question 42

**Domain:** ml-solution-monitoring-maintenance-and-security
**Difficulty:** medium

An auditor asks the team to prove that no SageMaker training job has run with a non-encrypted EBS volume in the last 90 days. Which AWS service is the documented surface to produce this evidence?

- A. AWS Config rules (e.g. sagemaker-training-job-volume-encryption-enabled) plus a Config Aggregator that retains 90 days of resource-configuration history.
- B. Amazon QuickSight
- C. AWS Trusted Advisor
- D. AWS WAF

**Correct Answer:** A

**Explanation:**
<p>AWS Config records the configuration state of supported resources over time and evaluates them against rules. SageMaker-specific managed Config rules (such as sagemaker-training-job-volume-encryption-enabled) check whether each training job had VolumeKmsKeyId configured. A Config Aggregator centralises results across regions/accounts. With Config history (default retention can be extended via S3 export), auditors can review compliance over arbitrary windows including the last 90 days. QuickSight, Trusted Advisor, and WAF are unrelated. The MLA-C01 exam expects candidates to know AWS Config as the compliance-evidence surface.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>AWS Config is the security camera that records every resource's configuration over time, plus a checklist (rules) that flags violations. The auditor wants to scroll back 90 days — the camera footage is exactly that.</p>
<h3 id="option-analysis883">Option Analysis:883</h3>
<ul>
<li><strong>A</strong>：Correct because AWS Config rules + Aggregator produce historical compliance evidence.</li>
<li><strong>B</strong>：Wrong because QuickSight is a BI tool.</li>
<li><strong>C</strong>：Wrong because Trusted Advisor is best-practice signals, not historical evidence.</li>
<li><strong>D</strong>：Wrong because WAF is for web-application firewall.</li>
</ul>

## Question 43

**Domain:** deployment-and-orchestration-of-ml-workflows
**Difficulty:** medium

An MLOps engineer is creating an IAM role for a SageMaker real-time endpoint. The endpoint needs to (a) pull the inference container image from Amazon ECR, (b) read the model artifact from a specific S3 prefix, and (c) write CloudWatch logs and metrics. The principle of least privilege requires that the role grant exactly these capabilities and nothing more. Which IAM principal should the role's trust policy allow to assume it?

- A. ec2.amazonaws.com because endpoint instances are EC2 instances under the hood.
- B. lambda.amazonaws.com because Lambda invokes the endpoint.
- C. sagemaker.amazonaws.com — the service principal that creates and runs endpoint instances on the customer's behalf.
- D. ecr.amazonaws.com because the role pulls images from ECR.

**Correct Answer:** C

**Explanation:**
<p>SageMaker endpoint execution roles must trust the SageMaker service principal sagemaker.amazonaws.com so that the SageMaker control plane can assume the role when provisioning endpoint instances and running the inference container. The role's permission policies grant the actual access: ECR pull, S3 GetObject on the artifact prefix, and the standard logs:CreateLogStream / logs:PutLogEvents / cloudwatch:PutMetricData actions for CloudWatch.</p>
<p>ec2.amazonaws.com is the wrong principal because endpoint instances are managed by SageMaker, not by the customer's EC2 console. lambda.amazonaws.com is the principal for Lambda execution roles, not SageMaker endpoint roles. ECR is a resource, not a principal. The correct answer is sagemaker.amazonaws.com.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine handing your house key to a property manager. The key is the IAM role; the manager is SageMaker. The trust policy is the note saying 'only this property manager may use this key' — naming the SageMaker service principal. Naming EC2 or Lambda would be giving the key to the wrong person.</p>
<h3 id="option-analysis1082">Option Analysis:1082</h3>
<ul>
<li><strong>A</strong>：Wrong — endpoint instances are not user-managed EC2; SageMaker is the principal that needs to assume the role.</li>
<li><strong>B</strong>：Wrong — Lambda is the caller, not the endpoint role's principal.</li>
<li><strong>C</strong>：Correct — sagemaker.amazonaws.com is the service principal that assumes the endpoint execution role.</li>
<li><strong>D</strong>：Wrong — ECR is a resource, not a service principal capable of assuming roles.</li>
</ul>

## Question 44

**Domain:** ml-solution-monitoring-maintenance-and-security
**Difficulty:** hard

A SageMaker administrator wants the option to revoke a model artifact's accessibility instantly even if all object-level deletes are denied. Which KMS feature delivers this?

- A. KMS key disabling (or scheduling key deletion): once the CMK is disabled, all SSE-KMS-encrypted objects under that key become unreadable until the key is re-enabled.
- B. S3 ACL deny
- C. Shutting down the SageMaker endpoint
- D. Lowering the bucket name's TTL

**Correct Answer:** A

**Explanation:**
<p>Customer-managed KMS CMKs offer an instant revocation lever: disabling the key (or scheduling its deletion) makes every SSE-KMS-encrypted object that uses that key undecipherable, even if the objects themselves remain in S3 and Object Lock prevents deletion. This is a powerful incident-response and contractual-revocation mechanism. Re-enabling the key restores access. ACL denies are per-object; endpoint shutdown only stops serving from one entry point; bucket name TTLs are not a thing. The MLA-C01 exam tests this CMK-revocation pattern.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine every locked box in the warehouse uses the same master lock. To revoke access to all of them at once, you do not break each box — you change the master lock. Disabling the CMK is changing the master lock for SSE-KMS-encrypted objects.</p>
<h3 id="option-analysis820">Option Analysis:820</h3>
<ul>
<li><strong>A</strong>：Correct because disabling the CMK instantly revokes decryption for all objects under it.</li>
<li><strong>B</strong>：Wrong because ACL deny is per-object, not a global revoke.</li>
<li><strong>C</strong>：Wrong because endpoint shutdown only stops one consumer.</li>
<li><strong>D</strong>：Wrong because bucket-name TTLs are meaningless.</li>
</ul>

## Question 45

**Domain:** ml-model-development
**Difficulty:** medium

A team wants to compare two trained SageMaker models on the same test set using A/B traffic splits in production. Which deployment pattern supports this?

- B. Multi-variant SageMaker real-time endpoint with weighted traffic distribution between Variant A and Variant B; CloudWatch metrics compare per-variant performance.
- A. Two separate endpoints with manual traffic splitting in client code.
- C. Use AWS Glue to split data between models.
- D. Run both models offline in batch transform.

**Correct Answer:** B

**Explanation:**
<p>SageMaker real-time endpoints support multi-variant deployment: a single endpoint can host multiple model variants with weighted traffic distribution (e.g., 90/10 or 50/50). CloudWatch publishes per-variant metrics (invocation count, latency, errors) and the team can call the endpoint with a TargetVariant override for shadow testing. Clarify and Model Monitor integrate with multi-variant endpoints. Distractors propose client-side workarounds or wrong-context services. The MLA-C01 lesson is to know multi-variant endpoints as the AWS-native A/B deployment pattern.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Multi-variant endpoint is one storefront with two checkout lines - 90% of customers go to A, 10% to B. The store records which line moved faster.</p>
<h3 id="option-analysis749">Option Analysis:749</h3>
<ul>
<li><strong>A</strong>：Wrong - client-side splitting is fragile and lacks AWS-native A/B.</li>
<li><strong>B</strong>：Correct - multi-variant endpoint splits traffic with per-variant CloudWatch metrics.</li>
<li><strong>C</strong>：Wrong - Glue is ETL.</li>
<li><strong>D</strong>：Wrong - batch transform is offline.</li>
</ul>

## Question 46

**Domain:** ml-solution-monitoring-maintenance-and-security
**Difficulty:** medium

A team is investigating cost. Their Data Quality monitoring schedule runs every hour on an ml.m5.4xlarge for a small endpoint that only handles a few thousand calls per day. Most hourly runs report no drift. The MLOps lead wants to lower cost without sacrificing compliance with their SLA, which requires drift detection within four hours of an incident. What is the BEST adjustment?

- A. Move the monitoring container to AWS Lambda to avoid SageMaker Processing costs.
- B. Increase the ScheduleExpression cadence to every four hours and right-size the instance to a smaller class such as ml.m5.xlarge.
- C. Stop the monitoring schedule entirely and review violations at quarter-end.
- D. Switch to ml.p4d.24xlarge to reduce per-second cost via faster execution.

**Correct Answer:** B

**Explanation:**
<p>Model Monitor jobs run on SageMaker Processing, so the cost levers are (1) instance size, (2) instance count, and (3) schedule cadence. When the SLA tolerates four-hour detection latency and traffic is light, the ideal is to lower cadence to every four hours and right-size the instance to a smaller m5.xlarge or m5.2xlarge that still finishes within the run window. This reduces total compute hours by roughly 75% versus hourly runs on a 4xlarge. Lambda cannot host scheduled Model Monitor jobs (they run on Processing). Quarterly reviews violate the SLA. GPU p4d instances are gross overkill for Deequ statistics and would multiply cost rather than reduce it. The MLA-C01 exam tests cost-sizing tradeoffs against SLA constraints.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine a security guard checking the building every hour with a giant spotlight. The neighbourhood is quiet and the contract only requires patrols every four hours. Switching to a smaller flashlight on a four-hourly cadence still meets the contract and slashes the lighting bill. That is exactly the trade-off Model Monitor lets you tune.</p>
<h3 id="option-analysis1106">Option Analysis:1106</h3>
<ul>
<li><strong>A</strong>：Wrong because Model Monitor is not Lambda-hosted; it uses SageMaker Processing.</li>
<li><strong>B</strong>：Correct because lower cadence + smaller instance still meets the SLA and saves cost.</li>
<li><strong>C</strong>：Wrong because quarterly reviews violate the four-hour SLA.</li>
<li><strong>D</strong>：Wrong because GPU instances are unnecessary for Deequ Data Quality and would dramatically raise cost.</li>
</ul>

## Question 47

**Domain:** data-preparation-for-ml
**Difficulty:** easy

A data engineering team enabled a Glue Crawler against an S3 prefix where a producer team writes nightly Parquet partitions in a Hive-style key=value layout. The team expects the Crawler will register new partitions in the Data Catalog and automatically rewrite or compact the underlying Parquet files into a more efficient layout. After two weeks of running, the catalog correctly shows the new partitions, but the underlying Parquet files in S3 remain exactly as the producer wrote them — many small files per partition, no compaction. The lead asks the team to explain what Glue Crawlers do and do NOT do. Which response correctly characterizes Glue Crawlers?

- A. Crawlers compact small files into larger ones as part of their default behavior; the team's configuration is incorrect.
- B. Crawlers infer schema, register tables and partitions in the Data Catalog, and never modify the underlying data files.
- C. Crawlers transform CSV to Parquet on the fly during their scan.
- D. Crawlers move files between S3 storage classes based on access frequency.

**Correct Answer:** B

**Explanation:**
<p>A Glue Crawler is a metadata-only tool. It scans the configured S3 location (or JDBC source), reads file headers and a sample of rows to infer schema, and creates or updates Glue Data Catalog tables and partition entries. The Crawler does not move, copy, transform, compact, or rewrite the underlying files in any way. Compacting many small Parquet files into fewer larger ones requires an explicit Glue ETL job (or another Spark job) that reads the small files and writes a compacted output. Format conversion (CSV to Parquet) is also the job of a Glue ETL job.</p>
<p>This Crawler-versus-ETL distinction is one of the most heavily tested points in MLA-C01 Domain 1. A stem that says 'we ran a Crawler to convert CSV to Parquet' is wrong by construction; a stem that says 'the Crawler should have compacted small files' is wrong by construction. The MLA-C01 mental model: Crawler = metadata only. ETL job = data movement and transformation.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>A Glue Crawler is a librarian walking the stacks with a clipboard, writing down which books are on which shelves. The librarian does not rebind the books, retype them, or move them to a different building. Reorganizing the shelves is a different job altogether — that is the moving company (Glue ETL).</p>
<h3 id="option-analysis1272">Option Analysis:1272</h3>
<ul>
<li><strong>A</strong>：Wrong because Crawlers do not compact, rewrite, or copy data.</li>
<li><strong>B</strong>：Correct because Crawlers infer schema and register Data Catalog entries without modifying underlying files.</li>
<li><strong>C</strong>：Wrong because Crawlers do not transform file formats; that is the job of a Glue ETL job.</li>
<li><strong>D</strong>：Wrong because storage class transitions are handled by S3 lifecycle policies, not by Crawlers.</li>
</ul>

## Question 48

**Domain:** ml-model-development
**Difficulty:** easy

A team wants AMT to evaluate a hyperparameter that is a discrete unordered choice among 'relu', 'tanh', and 'gelu' activation functions. Which AMT parameter type is appropriate?

- A. CategoricalParameter(['relu', 'tanh', 'gelu']) for unordered discrete choices.
- B. IntegerParameter(0, 2) and map indices in the script.
- C. ContinuousParameter(0.0, 1.0) and threshold in the script.
- D. Hard-code activation='relu'.

**Correct Answer:** A

**Explanation:**
<p>CategoricalParameter is AMT's type for unordered discrete choices, expressed as a Python list of values. AMT samples from this list directly and the surrogate model treats each choice as an unordered category. Mapping integers or continuous values to categories in the training script breaks AMT's modelling because the surrogate assumes ordering on numeric types. Hard-coding excludes tuning. The MLA-C01 lesson is to know the three parameter types and pick the right one based on hyperparameter semantics.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Activation functions are different colours, not different temperatures - there is no order. CategoricalParameter respects that; mapping to numbers fakes an order that does not exist.</p>
<h3 id="option-analysis725">Option Analysis:725</h3>
<ul>
<li><strong>A</strong>：Correct - CategoricalParameter handles unordered discrete choices.</li>
<li><strong>B</strong>：Wrong - integer indexing fakes ordering and breaks surrogate modelling.</li>
<li><strong>C</strong>：Wrong - continuous-to-categorical thresholding breaks the surrogate.</li>
<li><strong>D</strong>：Wrong - hard-coding excludes tuning.</li>
</ul>

## Question 49

**Domain:** ml-solution-monitoring-maintenance-and-security
**Difficulty:** medium

An MLOps team has a custom data-quality Python library with proprietary rules that go beyond what Deequ supports. They still want to participate in the Model Monitor scheduling, output, and CloudWatch metrics ecosystem. Which Model Monitor feature lets them plug in their own logic?

- A. BYOC (Bring Your Own Container) Model Monitor: package the custom logic into a container that conforms to the Model Monitor input/output contract, then point the monitoring schedule at it.
- B. Submit a feature request and wait for AWS to add the rule.
- C. Override the constraints.json schema by adding non-standard keys; Model Monitor will execute them.
- D. Move all monitoring to Amazon Forecast.

**Correct Answer:** A

**Explanation:**
<p>Model Monitor supports a Bring Your Own Container (BYOC) workflow for both Data Quality and Model Quality monitoring. You package your custom logic into a Docker container that follows the documented input/output contract: read captured data from the configured S3 input prefix, evaluate your rules, and write a violations.json + (optionally) statistics.json to the configured S3 output prefix. The scheduled job invocation, CloudWatch metric emission (if you publish them), and Studio integration all continue to work. constraints.json schema is fixed and does not execute arbitrary code; AWS Forecast is a different service entirely. The MLA-C01 exam expects candidates to recognise BYOC as the extensibility hook.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine a power outlet that accepts any plug as long as it has the right shape. You can build any appliance you want, as long as it fits the plug. Model Monitor's BYOC is that outlet — your container can do anything internally, as long as it reads from and writes to the agreed S3 layout.</p>
<h3 id="option-analysis1040">Option Analysis:1040</h3>
<ul>
<li><strong>A</strong>：Correct because BYOC is the documented extensibility model for Model Monitor.</li>
<li><strong>B</strong>：Wrong because the BYOC mechanism already exists.</li>
<li><strong>C</strong>：Wrong because constraints.json is a fixed schema, not a code-execution surface.</li>
<li><strong>D</strong>：Wrong because Amazon Forecast is unrelated to Model Monitor.</li>
</ul>

## Question 50

**Domain:** ml-solution-monitoring-maintenance-and-security
**Difficulty:** medium

A regulated insurance company wants to ensure that Clarify-powered monitors output their reports inside a private VPC, never traverse the public internet, and write to a CMK-encrypted S3 bucket. Which combination of fields on the Bias Drift monitoring schedule satisfies the requirements?

- A. Configure NetworkConfig with EnableNetworkIsolation=true and a VpcConfig pointing at private subnets, and set MonitoringOutputConfig.KmsKeyId to a customer-managed CMK; Studio integration continues to work via the same VPC.
- B. Use only the AWS-managed S3 alias for encryption and rely on default networking.
- C. Run the monitor outside SageMaker by exporting captures to an EC2 instance in a public subnet.
- D. Set ScheduleExpression to nightly so daytime traffic does not need encryption.

**Correct Answer:** A

**Explanation:**
<p>MonitoringJobDefinition exposes the same NetworkConfig and KMS fields for Bias Drift / Feature Attribution Drift schedules as for Data Quality / Model Quality schedules. Setting NetworkConfig.EnableNetworkIsolation to true blocks the container's internet egress; an embedded VpcConfig with the team's private Subnets and SecurityGroupIds routes any AWS-API traffic through VPC endpoints. Setting MonitoringOutputConfig.KmsKeyId to a customer-managed CMK encrypts the output reports at rest. AWS-managed encryption keys are usually insufficient for regulated workloads, EC2 in a public subnet breaks the private-network requirement, and cadence has no relation to encryption. The MLA-C01 exam tests that candidates can name the security knobs on Clarify-backed monitor schedules.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Three locks on the Clarify cabinet: a wall around it (network isolation), an internal-route map so deliveries stay on private roads (VpcConfig), and a CMK lock on the storage drawer (CMK-encrypted output). All three switches sit on the same MonitoringJobDefinition.</p>
<h3 id="option-analysis1079">Option Analysis:1079</h3>
<ul>
<li><strong>A</strong>：Correct because EnableNetworkIsolation + VpcConfig + CMK output is the documented secure configuration.</li>
<li><strong>B</strong>：Wrong because AWS-managed aliases are insufficient for regulated workloads and default networking allows public egress.</li>
<li><strong>C</strong>：Wrong because EC2 in a public subnet breaks both the private-network and managed-monitor requirements.</li>
<li><strong>D</strong>：Wrong because cadence is unrelated to encryption.</li>
</ul>

## Question 51

**Domain:** ml-model-development
**Difficulty:** hard

An ML Engineer is choosing between PyTorch FSDP and DeepSpeed ZeRO Stage 3 for sharding optimiser states, gradients, and parameters. Which statement most accurately summarises the trade-off on SageMaker?

- C. Both shard states/gradients/parameters and serve similar use cases; choice often comes down to ecosystem familiarity, integration with chosen libraries (HF Trainer, custom PyTorch), and benchmark performance on the target hardware.
- A. FSDP and ZeRO Stage 3 are entirely different and incomparable.
- B. FSDP cannot be used on AWS.
- D. DeepSpeed ZeRO is the only option AWS supports.

**Correct Answer:** C

**Explanation:**
<p>PyTorch FSDP and DeepSpeed ZeRO Stage 3 both shard parameters, gradients, and optimiser states across workers. FSDP is the native PyTorch implementation; DeepSpeed is Microsoft's framework with similar capabilities and additional features. Both run on SageMaker. The MLA-C01 lesson is to know they serve similar purposes and that choice typically follows ecosystem familiarity, library integration (HF Trainer integrates well with both), and benchmark results. Distractors deny the comparison or AWS support.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>FSDP and ZeRO are two brands of the same kind of saw - they cut the model into shareable chunks. Pick the one your toolkit already knows how to swing.</p>
<h3 id="option-analysis694">Option Analysis:694</h3>
<ul>
<li><strong>A</strong>：Wrong - they are directly comparable.</li>
<li><strong>B</strong>：Wrong - FSDP is supported on SageMaker.</li>
<li><strong>C</strong>：Correct - similar purpose; choice depends on ecosystem and benchmarks.</li>
<li><strong>D</strong>：Wrong - both FSDP and DeepSpeed are supported on AWS.</li>
</ul>

## Question 52

**Domain:** data-preparation-for-ml
**Difficulty:** medium

An ML Engineer's Data Wrangler flow reads from S3 and writes the prepared dataset back to S3. They want the destination encrypted with a customer-managed KMS key. How is this configured?

- C. Specify the customer-managed KMS key ARN in the Data Wrangler export step's S3 destination configuration; the SageMaker execution role must have kms:Encrypt/Decrypt permissions on that key.
- A. Disable encryption entirely.
- B. Encrypt files locally with OpenSSL before uploading.
- D. KMS encryption is unsupported in Data Wrangler.

**Correct Answer:** C

**Explanation:**
<p>Data Wrangler S3 export supports SSE-S3 and SSE-KMS. For CMK encryption, specify the KMS key ARN in the export configuration. The Studio/Processing execution role needs kms:Encrypt, kms:Decrypt, kms:GenerateDataKey on the CMK; the CMK key policy must trust the role. This is the standard SageMaker + KMS pattern. Disabling encryption is a violation; OpenSSL is wrong-tool; KMS is supported. The MLA-C01 lesson is to know the CMK ARN + IAM/key-policy pairing.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Customer-managed KMS is your own padlock on the storage box. You give the SageMaker role the spare key (IAM permissions) and put the lock's serial number (CMK ARN) on the box's manifest.</p>
<h3 id="option-analysis680">Option Analysis:680</h3>
<ul>
<li><strong>A</strong>：Wrong - disabling encryption is a violation.</li>
<li><strong>B</strong>：Wrong - local OpenSSL defeats S3 SSE.</li>
<li><strong>C</strong>：Correct - CMK ARN in export config + IAM/key-policy permissions.</li>
<li><strong>D</strong>：Wrong - CMK is supported.</li>
</ul>

## Question 53

**Domain:** ml-solution-monitoring-maintenance-and-security
**Difficulty:** hard

A central data-platform account owns an S3 bucket containing labeled training images for a downstream ML team in a separate AWS account. The objects are encrypted with a customer-managed AWS KMS key in the data-platform account. The data-platform team has updated the bucket policy to grant the ML team's SageMaker execution role list and read access on the prefix; the ML team has confirmed their role allows `s3:GetObject` on the prefix. Despite this, every SageMaker training run started from the consumer account fails with `AccessDenied` errors when reading the encrypted objects. Which additional configuration is most likely missing?

- A. Enable S3 Cross-Region Replication of the bucket into the consumer account's region.
- B. Update the KMS key policy in the data-platform account to grant kms:Decrypt to the consumer account's SageMaker execution role.
- C. Disable encryption on the source bucket so cross-account reads do not require KMS permissions.
- D. Move the bucket and key into the consumer account so they share a trust boundary.

**Correct Answer:** B

**Explanation:**
<p>Cross-account access to objects encrypted with SSE-KMS requires three permission layers, not two. The S3 bucket policy must allow the consumer principal to call <code>s3:GetObject</code> on the relevant prefix. The consumer's IAM role must allow the same actions. And the producer's KMS key policy must also grant <code>kms:Decrypt</code> (and typically <code>kms:DescribeKey</code>) to the consumer's principal — without that, the consumer can list and authorize the read at the S3 layer but cannot decrypt the ciphertext, so the read fails with AccessDenied. Defense-in-depth conditions such as <code>aws:SourceArn</code> and <code>aws:SourceAccount</code> on the KMS grant prevent confused-deputy escalation.</p>
<p>Cross-Region Replication does not solve a permission problem; it copies objects to another region, and the same KMS-decrypt issue would persist. Disabling encryption on labeled medical training data is not an acceptable remediation in any regulated environment, and moving ownership of the bucket and key defeats the producer-consumer separation that was deliberately designed. The MLA-C01 stem signal 'cross-account S3 access to KMS-encrypted training data fails despite a bucket policy' is the missing-KMS-grant signature.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Picture a self-storage facility where you have keys to the front gate (S3 bucket policy) and to the unit door (IAM role) — but the items inside are locked in a private safe (KMS key) that the previous renter still owns. You can walk in and stare at the safe, but without the safe combination (kms:Decrypt) you cannot open it. The fix is asking the safe owner to add you to the combination list.</p>
<h3 id="option-analysis1612">Option Analysis:1612</h3>
<ul>
<li><strong>A</strong>：Wrong because CRR copies objects to another region and does not address the missing KMS decrypt grant.</li>
<li><strong>B</strong>：Correct because cross-account access to SSE-KMS objects requires explicit kms:Decrypt on the producer's key for the consumer's principal.</li>
<li><strong>C</strong>：Wrong because disabling encryption on regulated training data is unacceptable and unnecessary.</li>
<li><strong>D</strong>：Wrong because moving ownership defeats the producer-consumer separation; the right fix is the missing KMS grant.</li>
</ul>

## Question 54

**Domain:** deployment-and-orchestration-of-ml-workflows
**Difficulty:** easy

A team's MLOps process requires a manual approval step before a registered model is deployed to production. Which built-in Model Registry feature implements this gate?

- C. Model package approval status (PendingManualApproval -> Approved or Rejected), set via UpdateModelPackage; deployment automation reads this status before deploying.
- A. Slack messages reviewed by senior engineers.
- B. Manual JIRA tickets only.
- D. Approval is impossible in Model Registry.

**Correct Answer:** C

**Explanation:**
<p>Model Registry assigns each model package an approval status: PendingManualApproval, Approved, or Rejected. CI/CD pipelines (CodePipeline, EventBridge -> Lambda, SageMaker Pipelines conditional steps) check the status and deploy only Approved packages. Reviewers update status via UpdateModelPackage with justification metadata. Slack/JIRA can supplement but are not the authoritative registry-level gate. The MLA-C01 lesson is to know approval status as the in-registry deployment gate.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Approval status is the official 'green-light/red-light' stamp on the model. Deployment robots only act on green-light - chats and tickets are nice but the stamp is what counts.</p>
<h3 id="option-analysis699">Option Analysis:699</h3>
<ul>
<li><strong>A</strong>：Wrong - Slack is not authoritative.</li>
<li><strong>B</strong>：Wrong - JIRA is supplemental.</li>
<li><strong>C</strong>：Correct - approval status is the canonical gate.</li>
<li><strong>D</strong>：Wrong - approval is supported.</li>
</ul>

## Question 55

**Domain:** ml-solution-monitoring-maintenance-and-security
**Difficulty:** medium

A team needs to ensure that no SageMaker user-profile execution role can launch training on instance types other than ml.m5.* (development bound). Which IAM construct is the documented enforcement?

- A. Add a Deny statement on sagemaker:CreateTrainingJob with Condition StringNotLike on sagemaker:InstanceTypes matching 'ml.m5.*' to all user-profile execution roles or as an SCP.
- B. Email reminders to engineers
- C. Use Amazon CloudFront for instance whitelisting
- D. Disable IAM

**Correct Answer:** A

**Explanation:**
<p>SageMaker exposes condition keys for resource attributes including sagemaker:InstanceTypes (the requested instance types in a CreateTrainingJob). A Deny statement with Condition StringNotLike on this key, matching 'ml.m5.*', rejects any training job on a non-m5 instance. Apply at the role or via SCP at the OU level for cross-account enforcement. Email is not enforcement, CloudFront is unrelated, and disabling IAM is catastrophic. The MLA-C01 exam tests this exact condition-key pattern for cost guardrails.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine a corporate credit card that refuses any merchant outside the 'office supplies' category. StringNotLike on sagemaker:InstanceTypes is the same idea — refuse any instance type not on the allow-pattern.</p>
<h3 id="option-analysis754">Option Analysis:754</h3>
<ul>
<li><strong>A</strong>：Correct because StringNotLike on sagemaker:InstanceTypes is the documented enforcement.</li>
<li><strong>B</strong>：Wrong because email is not enforcement.</li>
<li><strong>C</strong>：Wrong because CloudFront is a CDN.</li>
<li><strong>D</strong>：Wrong because disabling IAM is a catastrophe.</li>
</ul>

## Question 56

**Domain:** deployment-and-orchestration-of-ml-workflows
**Difficulty:** easy

A small internal tools team runs a low-traffic SageMaker endpoint (4 RPS, two engineers as the only users). They want to deploy a new model with the simplest, fastest update mechanism. Risk of brief errors during the cutover is acceptable. Which deployment-guardrails traffic-shifting mode best fits this low-stakes scenario?

- A. Linear traffic shifting at 10% / 5 minutes intervals.
- B. Canary traffic shifting at 25% for 30 minutes then 100%.
- C. Shadow Testing for 24 hours before any production cutover.
- D. All-at-once traffic shifting (full cutover).

**Correct Answer:** D

**Explanation:**
<p>SageMaker deployment guardrails support three traffic-shift modes: all-at-once (instant 100% cutover), canary (fixed canary percentage for a baking period, then 100%), and linear (equal increments over time). The right mode depends on the blast radius of a bad deployment.</p>
<p>All-at-once is appropriate when stakes are low and the operator can tolerate brief errors during cutover — internal tools with a handful of users, dev/staging environments, low-RPS internal jobs. The benefits are simplicity and speed; the downside is no progressive exposure. For high-stakes production endpoints, canary or linear with auto-rollback is mandatory. For very high-stakes model swaps with material decision-boundary changes, Shadow Testing first is the AWS-recommended de-risking step. The MLA-C01 exam tests the ability to map blast-radius to mode; for this scenario, all-at-once is right.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine swapping the lightbulb in your home office: you flip the switch off, change the bulb, flip the switch on. No need for a 30-minute baking period — the blast radius is one room and one person. All-at-once is the home-office bulb swap.</p>
<h3 id="option-analysis1153">Option Analysis:1153</h3>
<ul>
<li><strong>A</strong>：Wrong — linear is a careful pattern for high-stakes production; overkill for a 4 RPS internal tool.</li>
<li><strong>B</strong>：Wrong — canary's baking period is for higher-stakes traffic; not needed for two internal users.</li>
<li><strong>C</strong>：Wrong — Shadow Testing has parallel-fleet cost and is overkill for a low-stakes tool.</li>
<li><strong>D</strong>：Correct — all-at-once is the simplest, fastest mode and is appropriate when the blast radius is small.</li>
</ul>

## Question 57

**Domain:** ml-model-development
**Difficulty:** hard

An ML Engineer wants to compare offline holdout-set evaluation metrics against early production metrics from the same model. Which combination provides the cleanest workflow?

- C. Use a SageMaker Pipelines ProcessingStep to compute holdout metrics into Experiments; once deployed, use Model Monitor (model-quality with ground-truth labels) to compute production metrics, and compare both in Studio dashboards.
- A. Compute production metrics in the holdout-evaluation script.
- B. Use only Debugger for both offline and production metrics.
- D. Migrate to a different cloud provider.

**Correct Answer:** C

**Explanation:**
<p>Bridging offline and online evaluation is a recurring MLOps task. The AWS-native combination is: offline metrics from Pipelines ProcessingStep tracked in Experiments; online metrics from Model Monitor (model-quality monitoring with ground-truth labels) in CloudWatch and Studio; cross-comparison in Studio. This produces a continuous picture of model behaviour from training through production. Distractors confuse offline/online or invoke unrelated services. The MLA-C01 lesson is to know how the SageMaker observability stack stitches together for the full ML lifecycle.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Holdout metrics are the practice exam scores; production metrics are the real-world test. SageMaker tracks both - Experiments holds the practice scores, Model Monitor reports the real ones, Studio shows them side by side.</p>
<h3 id="option-analysis829">Option Analysis:829</h3>
<ul>
<li><strong>A</strong>：Wrong - holdout scripts run pre-deployment, cannot see production metrics.</li>
<li><strong>B</strong>：Wrong - Debugger is training-time only.</li>
<li><strong>C</strong>：Correct - Pipelines + Experiments + Model Monitor + Studio for offline/online comparison.</li>
<li><strong>D</strong>：Wrong - cross-cloud is unrelated.</li>
</ul>

## Question 58

**Domain:** deployment-and-orchestration-of-ml-workflows
**Difficulty:** easy

An ML team has built a video-segment classifier and needs to choose between SageMaker real-time and async endpoints. Each request is a single 35 MB video clip, and inference takes 28 seconds on average. The clients are willing to poll an S3 location for the result. Which SageMaker endpoint type's documented limits make it the only viable choice for this combination of payload size and inference duration?

- A. Real-time, because real-time accepts up to 1 GB request payloads.
- B. Asynchronous, because async endpoints support up to 1 GB payloads and up to one hour processing time, with results written to S3.
- C. Serverless, because it can be scaled to handle large videos.
- D. Batch Transform, because video clips must always be processed offline.

**Correct Answer:** B

**Explanation:**
<p>SageMaker endpoint types have explicitly documented payload and timeout limits that often determine the answer outright. Real-time: 6 MB request, 60 s invocation timeout. Serverless: 4 MB request, 60 s timeout. Asynchronous: 1 GB request, up to 1 hour processing. Batch Transform: bulk job lifecycle against S3 manifests with per-record limits depending on framework.</p>
<p>For a 35 MB clip with 28 s inference and S3-based result polling, only async satisfies all three constraints. Real-time and serverless are killed by the payload limit alone. Batch Transform changes the operational model from per-request submission to bulk job, which the stem rules out (clients submit individual clips and poll). Async is the correct match.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Imagine a courier service: regular mail accepts envelopes up to 6 ounces, express letters up to 4 ounces, freight up to 1 gigaton. A 35 oz package goes to freight. Real-time and serverless reject it at the door; async accepts it. The size and processing time are non-negotiable physical limits.</p>
<h3 id="option-analysis1056">Option Analysis:1056</h3>
<ul>
<li><strong>A</strong>：Wrong — real-time payload cap is 6 MB and timeout is 60 s; both are exceeded.</li>
<li><strong>B</strong>：Correct — async supports 1 GB payloads and 1 hour processing with S3 result delivery, satisfying every constraint.</li>
<li><strong>C</strong>：Wrong — serverless has tighter limits (4 MB / 60 s); it is the most constrained.</li>
<li><strong>D</strong>：Wrong — Batch Transform is for bulk jobs against S3 manifests, not per-request video submission.</li>
</ul>

## Question 59

**Domain:** data-preparation-for-ml
**Difficulty:** hard

An ML Engineer's training pipeline retrieves features from the offline store using point-in-time joins. After the join, the resulting training set has duplicate rows for some labels. Which root cause is most plausible?

- D. Multiple feature records exist with the same record_id at or before the label's prediction time; the join did not de-duplicate to the latest event_time per record_id.
- A. Feature Store has a bug; report to AWS.
- B. Athena always duplicates rows by default.
- C. The offline store does not support joins.

**Correct Answer:** D

**Explanation:**
<p>A correct point-in-time join selects, for each label at time T, the single feature record per record_id whose event_time is the maximum value &#x3C;= T. Naive joins that just filter event_time &#x3C;= T return all qualifying records, producing duplicates. Solutions: window functions (ROW_NUMBER OVER (PARTITION BY record_id ORDER BY event_time DESC) with rank=1) or correlated subqueries. AWS bug claims, Athena duplication-by-default, and join-unsupported are wrong. The MLA-C01 lesson is to know the deduplication-to-latest pattern in point-in-time joins.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Point-in-time join needs to pick exactly one customer photo per label - the most recent photo before the label was taken. Forgetting that step gives you every photo ever taken before the label, multiplying rows.</p>
<h3 id="option-analysis795">Option Analysis:795</h3>
<ul>
<li><strong>A</strong>：Wrong - duplicates are query design issues.</li>
<li><strong>B</strong>：Wrong - Athena does not duplicate by default.</li>
<li><strong>C</strong>：Wrong - the offline store supports joins.</li>
<li><strong>D</strong>：Correct - join must select latest event_time per record_id.</li>
</ul>

## Question 60

**Domain:** deployment-and-orchestration-of-ml-workflows
**Difficulty:** hard

An MLA candidate is presented with the following matrix during a design review. The team must pick exactly one SageMaker endpoint type for each scenario and the architect wants the cost-optimal choice given each constraint. Scenario W: 5 RPS at 50 ms p95, 24x7. Scenario X: 0.05 RPS daytime only, sub-second OK with occasional 1 s cold start. Scenario Y: 200 MB payload per call, 8 minutes processing, asynchronous OK. Scenario Z: nightly score of 50M rows from S3, no live traffic. Which mapping uses the cost-optimal endpoint type for every scenario?

- A. W: real-time, X: serverless, Y: async, Z: Batch Transform.
- B. W: serverless, X: real-time, Y: real-time, Z: real-time.
- C. W: Batch Transform, X: async, Y: serverless, Z: real-time.
- D. W: async, X: Batch Transform, Y: serverless, Z: serverless.

**Correct Answer:** A

**Explanation:**
<p>MLA-C01 commonly tests endpoint-type mapping across mixed scenarios. The decision rules are stable:</p>
<ul>
<li>Real-time: sustained synchronous traffic with strict p95 latency. Scenario W fits exactly.</li>
<li>Serverless: intermittent low-volume sync traffic where small cold-starts are tolerable. Scenario X fits exactly.</li>
<li>Async: long-running and/or large-payload workloads where the client tolerates async results in S3. Scenario Y (200 MB, 8 min) fits — both real-time (6 MB / 60 s) and serverless (4 MB / 60 s) are blocked by their hard limits.</li>
<li>Batch Transform: offline bulk scoring against S3 input. Scenario Z (50M rows nightly) fits exactly.</li>
</ul>
<p>The correct mapping is W: real-time, X: serverless, Y: async, Z: Batch Transform. The other options invert at least one mapping in a way that breaks a hard constraint.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Each scenario is a vehicle choice. W is a daily commute (real-time sedan). X is a once-in-a-while errand (rideshare/serverless). Y is moving a sofa across town (rental truck/async). Z is shipping a thousand boxes overnight (cargo flight/Batch Transform). Mixing them up makes every trip more expensive or impossible.</p>
<h3 id="option-analysis1157">Option Analysis:1157</h3>
<ul>
<li><strong>A</strong>：Correct — each scenario matches its textbook cost-optimal SageMaker endpoint type.</li>
<li><strong>B</strong>：Wrong — serverless is too expensive at 5 RPS 24x7 versus real-time, and real-time is wrong for nightly 50M-row scoring.</li>
<li><strong>C</strong>：Wrong — Batch Transform cannot handle live 5 RPS sync traffic, and serverless cannot do 8-minute / 200 MB jobs.</li>
<li><strong>D</strong>：Wrong — async fails the 50 ms sync requirement; serverless fails 8-minute and 50M-row workloads.</li>
</ul>

## Question 61

**Domain:** data-preparation-for-ml
**Difficulty:** medium

An engineer writes a Glue ETL job that converts CSV to Parquet and wants the output partitions registered in the Glue Data Catalog automatically as part of the job rather than running a separate Crawler afterward. Which Glue option supports automatic Catalog registration during the ETL write?

- A. Set `enableUpdateCatalog=True` and `partitionKeys=[...]` on the Glue ETL write.
- B. Switch to Athena CREATE TABLE AS SELECT and skip Glue ETL entirely.
- C. Add a Glue Crawler step inside the ETL Spark code via a Lambda invocation.
- D. Manually run `MSCK REPAIR TABLE` after every job.

**Correct Answer:** A

**Explanation:**
<p>Glue ETL supports <strong>automatic Catalog updates during the write</strong> via <code>enableUpdateCatalog=True</code> plus <code>partitionKeys=[...]</code> on the sink configuration. When set, the Glue ETL job updates the Catalog table schema and partition entries as part of the write — no separate Crawler or manual MSCK REPAIR is needed. This eliminates a class of orchestration bugs where the data is written but the Catalog is not yet aware of new partitions.</p>
<p>CTAS is one valid alternative but does not address the in-job Catalog registration question. Wrapping a Crawler inside Lambda is a workaround; the native pattern is enableUpdateCatalog. MSCK REPAIR is a manual Athena/Hive command, not automatic in-job registration. The MLA-C01 stem signal 'register partitions in Catalog during the ETL write without a separate Crawler' is the enableUpdateCatalog signature.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>enableUpdateCatalog is the contractor who builds the room AND updates the building registry in one trip. Calling a separate inspector (Crawler) afterwards is fine but redundant when the contractor offers both services.</p>
<h3 id="option-analysis1095">Option Analysis:1095</h3>
<ul>
<li><strong>A</strong>：Correct because enableUpdateCatalog is the Glue ETL feature that registers Catalog tables and partitions during the write.</li>
<li><strong>B</strong>：Wrong because CTAS is an alternative path but does not address in-job Catalog registration during a Glue ETL write.</li>
<li><strong>C</strong>：Wrong because Crawler-via-Lambda is a workaround; enableUpdateCatalog is the AWS-native pattern.</li>
<li><strong>D</strong>：Wrong because MSCK REPAIR is manual and not the automatic in-job mechanism.</li>
</ul>

## Question 62

**Domain:** data-preparation-for-ml
**Difficulty:** medium

An ML team currently retrains a customer-churn model nightly. A scheduled AWS Glue job at 2 a.m. queries the production Amazon RDS for PostgreSQL database, scans the full customer-events table (now 4 TB), and writes a new Parquet partition to S3. The DBA has filed multiple complaints because the nightly scan runs for 90 minutes, drives CPU on the primary to 95 percent, and slows business-hours queries the next morning while the buffer cache is repopulating. The ML team needs near-real-time data freshness in S3 for model retraining without imposing query load on the operational database. Which architecture best resolves these pain points?

- A. Add an Amazon RDS read replica and point the nightly Glue job at the replica instead of the primary.
- B. Replace Glue with an Amazon Athena Federated Query that reads from RDS on demand at training time.
- C. Increase the RDS instance size to absorb the nightly scan and continue using the existing Glue job.
- D. Configure AWS Database Migration Service in change-data-capture mode to replicate inserts, updates, and deletes from RDS to S3 as Parquet.

**Correct Answer:** D

**Explanation:**
<p>AWS Database Migration Service (DMS) in change-data-capture (CDC) mode reads the source database transaction log rather than scanning tables. Inserts, updates, and deletes flow incrementally to S3 as Parquet with seconds-of-latency, the source RDS sees only the lightweight log reader thread, and the buffer cache on the primary is undisturbed. ML retraining then consumes the curated S3 partitions on whatever cadence the team chooses — nightly, hourly, or continuously — without any further hit on the operational database. This is the canonical AWS pattern when an ML stem mentions 'minimize impact on the production database' or 'fresher data for retraining'.</p>
<p>A read replica reduces primary CPU but still performs the same full-table scan and provides only nightly freshness. Athena Federated Query issues live JDBC against RDS at query time, reproducing the same load and latency problem. Scaling up the instance hides the symptom and increases cost without changing the architecture. The MLA-C01 stem signal 'minimize OLTP load' or 'near-real-time freshness from a relational source' is the DMS CDC signature.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>The nightly Glue job is like asking the bank manager to count every coin in every safety deposit box at 2 a.m. so you can update your spreadsheet. DMS CDC is like asking the bank to send you a copy of the transaction log — every deposit and withdrawal as it happens — without ever opening a box. Same data, dramatically less disruption.</p>
<h3 id="option-analysis1488">Option Analysis:1488</h3>
<ul>
<li><strong>A</strong>：Wrong because the read replica performs the same scan and still leaves freshness at nightly cadence.</li>
<li><strong>B</strong>：Wrong because Federated Query reproduces the same OLTP load at query time.</li>
<li><strong>C</strong>：Wrong because scaling up hides the symptom and does not change the architectural mismatch.</li>
<li><strong>D</strong>：Correct because DMS CDC reads the transaction log, imposes near-zero load on the source DB, and produces near-real-time S3 output.</li>
</ul>

## Question 63

**Domain:** data-preparation-for-ml
**Difficulty:** easy

A team's ML pipeline reads CSV files from a producer S3 prefix and needs ML-ready Parquet files in a curated prefix for SageMaker training. A junior engineer suggests configuring a Glue Crawler with 'Convert to Parquet' enabled to do the format conversion as part of the crawl. The lead engineer disagrees and explains that Crawlers do not transform data. Which AWS service combination correctly produces Parquet output from CSV input and registers it in the Glue Data Catalog?

- A. A Glue Crawler with the 'Convert to Parquet' option, which automatically rewrites the source files.
- B. A Glue ETL job that reads CSV and writes Parquet to a curated prefix, then a Crawler (or enableUpdateCatalog) to register the Parquet output.
- C. A Glue Workflow that runs only Crawlers in sequence — first against CSV, then against Parquet.
- D. An S3 lifecycle rule that automatically transitions CSV to Parquet after seven days.

**Correct Answer:** B

**Explanation:**
<p>Format conversion (CSV → Parquet) is a transformation, not a metadata operation. The correct AWS pattern is a <strong>Glue ETL job</strong> that reads CSV from the producer prefix, applies any needed schema casting or filtering, and writes Parquet to a curated prefix. Once the Parquet is written, either a separate <strong>Glue Crawler</strong> scans the curated prefix and updates the Data Catalog, or the ETL job uses <strong>enableUpdateCatalog</strong> to register the Parquet schema directly during the write.</p>
<p>Crawlers do not transform data — they only infer schema and register Catalog entries. There is no 'Convert to Parquet' option on a Crawler. Chaining multiple Crawlers does not perform format conversion. S3 lifecycle rules transition between storage classes or expire objects; they do not transform file formats. The MLA-C01 trap is offering 'Crawler with format conversion' as a candidate answer; the correct mental model is Crawler = metadata only, ETL = data transformation.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Crawler-vs-ETL is a librarian-vs-translator distinction. The librarian writes down what shelf each book is on (Crawler). The translator actually rewrites the book in another language (ETL). Asking the librarian to translate is just a category mistake.</p>
<h3 id="option-analysis1241">Option Analysis:1241</h3>
<ul>
<li><strong>A</strong>：Wrong because Crawlers do not transform files; there is no 'Convert to Parquet' option on a Crawler.</li>
<li><strong>B</strong>：Correct because format conversion is the job of an ETL job; the Crawler only registers the resulting schema.</li>
<li><strong>C</strong>：Wrong because chaining Crawlers does not produce Parquet from CSV; an ETL job must do the conversion.</li>
<li><strong>D</strong>：Wrong because lifecycle rules do not change file format.</li>
</ul>

## Question 64

**Domain:** data-preparation-for-ml
**Difficulty:** medium

A team's Data Wrangler export to a SageMaker Processing job ran successfully but produced an output schema slightly different from the training script's expected schema. Which mitigation is canonical?

- B. Add a final 'Manage columns' step in the Data Wrangler flow (rename, reorder, drop) to match the training script's expected schema, then re-export.
- A. Modify the Processing job container to silently drop unexpected columns at runtime.
- C. Ignore the schema mismatch and hope the training script handles it.
- D. Switch from Data Wrangler to AWS Glue.

**Correct Answer:** B

**Explanation:**
<p>Data Wrangler's 'Manage columns' transform group covers rename, drop, reorder, and move-to-position. The clean pattern is to make the Data Wrangler flow's last step enforce the schema contract that downstream training expects. This makes schema changes a deliberate, reviewable flow change rather than a hidden runtime fix. Silently dropping in containers hides drift; ignoring breaks training; switching tools is overreaction. The MLA-C01 lesson is to keep schema enforcement explicit in the flow.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Manage columns is the final-checkout step where you arrange the dish on the plate exactly how the menu photo shows. The downstream tasting panel expects what the menu promised.</p>
<h3 id="option-analysis710">Option Analysis:710</h3>
<ul>
<li><strong>A</strong>：Wrong - silent runtime drops hide drift.</li>
<li><strong>B</strong>：Correct - Manage columns at end of flow enforces schema contract.</li>
<li><strong>C</strong>：Wrong - ignoring breaks training.</li>
<li><strong>D</strong>：Wrong - switching tools is overreaction.</li>
</ul>

## Question 65

**Domain:** ml-model-development
**Difficulty:** medium

An ML Engineer must tune a model and is asked when grid search is the appropriate choice over Bayesian or random search in SageMaker AMT. Which scenario favours grid search?

- A. A small categorical search space (e.g., 2 hyperparameters with 3-4 values each) where exhaustive coverage is desirable.
- B. A continuous search space spanning many orders of magnitude.
- C. A search space with 8+ hyperparameters.
- D. Whenever you want results in fewer trials than Bayesian.

**Correct Answer:** A

**Explanation:**
<p>Grid search exhaustively evaluates every combination of pre-specified values. It scales O(product of value counts), which becomes infeasible quickly. The legitimate use case for grid search is small categorical spaces where exhaustive coverage is the goal - for example, comparing two activation functions and three optimisers (6 trials). For continuous spaces or larger search dimensions, Bayesian or random search dominate. The MLA-C01 lesson is to know the strengths and weaknesses of each strategy and to avoid the common trap of recommending grid as a default.</p>
<h3 id="plain-language-explanation">Plain-Language Explanation:</h3>
<p>Grid search is checking every house on a small street - feasible for 6 houses, impossible for a city. Bayesian and random are smart sampling strategies for big spaces.</p>
<h3 id="option-analysis768">Option Analysis:768</h3>
<ul>
<li><strong>A</strong>：Correct - small categorical spaces where exhaustive coverage is desirable.</li>
<li><strong>B</strong>：Wrong - grid is impractical for continuous spaces.</li>
<li><strong>C</strong>：Wrong - grid blows up combinatorially with more dimensions.</li>
<li><strong>D</strong>：Wrong - Bayesian generally finds good results in fewer trials than grid.</li>
</ul>
