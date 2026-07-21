# Tech Exam Lexicon - AWS MLA-C01 Sample Questions

Source: https://techexamlexicon.com/aws/mla-c01/sample-questions/

---

## Question 1

**Topic:** Reducing train/serve skew

A fraud model uses engineered customer-behavior features during training. In production, the application team recomputes similar features in application code and predictions are inconsistent with validation results. The ML team wants one governed feature definition for both training and inference. Which approach is strongest?

- A. Keep separate training and inference feature code, but increase the model retraining frequency.
- B. Move the model to a larger instance type so feature mismatch has less impact.
- C. Use SageMaker Feature Store or an equivalent governed feature-serving pattern so training and inference use consistent feature definitions.
- D. Disable feature engineering and train only on raw columns.

**Best answer:** C

**Explanation:**
The problem is train/serve skew. A governed feature store pattern lets teams define, reuse, and serve features consistently across training and inference workflows.

## Question 2

**Topic:** Choosing the inference pattern

A model scores millions of images every night for downstream reporting. No user waits for an immediate response, and the input set is known before the job starts. The team wants lower operational cost than keeping a low-latency endpoint idle all day. Which deployment choice best matches the workload?

- A. A real-time endpoint sized for peak traffic.
- B. A serverless endpoint because every ML workload should use serverless inference.
- C. Batch transform or a batch inference workflow.
- D. A multi-model endpoint, regardless of whether multiple models are needed.

**Best answer:** C

**Explanation:**
The workload is scheduled, high-volume, and not latency-sensitive. Batch inference avoids paying for always-on endpoint capacity when immediate per-request response is not required.

## Question 3

**Topic:** Accuracy drop after deployment

A real-time model performed well during validation, but after several weeks in production its prediction quality dropped. Infrastructure metrics look healthy, and request latency remains normal. The team suspects the input distribution changed. What should be the strongest first response?

- A. Increase endpoint instance size because latency is usually the cause of model quality issues.
- B. Delete the endpoint and redeploy the original model artifact without investigation.
- C. Disable logging to reduce endpoint overhead.
- D. Use model monitoring with a baseline, compare production inputs and predictions for drift, alert on threshold breaches, and trigger a controlled retraining or review workflow.

**Best answer:** D

**Explanation:**
The clue points to data or concept drift, not capacity. MLA-C01 operational questions reward monitoring baselines, drift evidence, alerting, and controlled retraining over blind redeployment.

## Question 4

**Topic:** Secure model deployment workflow

An ML platform team promotes models from experimentation to production. Security requires least-privilege access, encrypted artifacts, an approval trail, and the ability to roll back if a new model underperforms. Which design is strongest?

- A. Use a model registry with approval states, versioned artifacts, encrypted storage, scoped IAM roles, deployment automation, monitoring, and rollback steps.
- B. Allow data scientists to overwrite the production endpoint directly after local testing.
- C. Share one administrator role across all notebooks, pipelines, and endpoints for simplicity.
- D. Store model artifacts in an unversioned bucket and rely on endpoint logs to infer which model is active.

**Best answer:** A

**Explanation:**
The requirement combines governance, security, traceability, and operational rollback. A registry-backed deployment workflow with scoped permissions and versioned artifacts is stronger than manual endpoint overwrites.
