---
marp: true
theme: default
size: "16:9"
paginate: true
footer: "AI PR Review | Data through August 20, 2026"
style: |
  section {
    font-family: Arial, Helvetica, sans-serif;
    padding: 38px 52px;
  }
  h1 {
    color: #172554;
    font-size: 34px;
    margin-bottom: 18px;
  }
  h2 {
    color: #334155;
    font-size: 21px;
    margin: 18px 0 8px;
  }
  table {
    font-size: 17px;
    margin: 0;
  }
  th {
    background: #e2e8f0;
  }
  strong {
    color: #0369a1;
  }
  blockquote {
    border-left: 6px solid #0e7490;
    color: #334155;
    font-size: 22px;
    margin-top: 26px;
  }
  footer {
    color: #64748b;
    font-size: 13px;
  }
  pre {
    font-size: 14px;
    line-height: 1.25;
  }
  section.setup {
    font-size: 24px;
  }
  section.setup h1 {
    margin-bottom: 12px;
  }
  section.setup p {
    margin: 10px 0;
  }
  section.setup pre {
    margin: 10px 0;
    font-size: 17px;
  }
  section.cover {
    text-align: center;
    justify-content: center;
  }
  section.cover h1 {
    font-size: 56px;
    margin-bottom: 18px;
  }
  section.cover h2 {
    color: #334155;
    font-size: 28px;
    font-weight: normal;
    margin: 0 0 30px;
  }
  section.cover p {
    color: #64748b;
    font-size: 18px;
  }
  section.end p {
    color: #0369a1;
    font-size: 24px;
    font-weight: bold;
  }
  section.security p {
    color: #64748b;
    font-size: 16px;
    margin: 12px 0 0;
  }
  section.workload {
    text-align: center;
  }
  section.workload h1 {
    text-align: left;
  }
  .workload-metrics {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 0;
    margin: 84px 0 64px;
  }
  .workload-metric {
    border-right: 1px solid #cbd5e1;
  }
  .workload-metric:last-child {
    border-right: 0;
  }
  .workload-metric strong {
    display: block;
    font-size: 72px;
    line-height: 1;
  }
  .workload-metric span {
    color: #334155;
    display: block;
    font-size: 24px;
    margin-top: 14px;
  }
  .workload-repos {
    color: #64748b;
    font-size: 18px;
  }
  section.injection blockquote {
    font-size: 18px;
    margin-top: 18px;
  }
  .causal-flow {
    align-items: center;
    display: grid;
    grid-template-columns: 1fr 34px 1fr 34px 1fr 34px 1fr;
    margin: 34px 0 28px;
  }
  .flow-step {
    min-height: 210px;
    padding: 0 12px;
    text-align: center;
  }
  .flow-step-number {
    border: 3px solid #0e7490;
    border-radius: 50%;
    color: #0e7490;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    font-size: 25px;
    font-weight: bold;
    height: 48px;
    width: 48px;
  }
  .flow-step strong {
    color: #172554;
    display: block;
    font-size: 20px;
    margin: 16px 0 9px;
  }
  .flow-step p {
    color: #475569;
    font-size: 17px;
    line-height: 1.35;
    margin: 0;
  }
  .flow-arrow {
    color: #0e7490;
    font-size: 34px;
    text-align: center;
  }
  .injection-impact {
    color: #334155;
    font-size: 18px;
    margin: 0;
    text-align: center;
  }
---

<!-- _class: cover -->
<!-- _paginate: false -->
<!-- _footer: "" -->

# AI Pull Request Review

## Reusable workflow, early findings, and prompt-injection safeguards

Frank Chen
AWS Lambda Durable Functions SDK
August 21, 2026

---

<!-- _class: workload -->

# Code Review Workload

<div class="workload-metrics">
  <div class="workload-metric"><strong>196</strong><span>PRs</span></div>
  <div class="workload-metric"><strong>6</strong><span>repositories</span></div>
  <div class="workload-metric"><strong>13</strong><span>work days</span></div>
</div>

<p class="workload-repos">
Java &middot; JavaScript &middot; Python &middot; Conformance &middot; Docs &middot; CI<br>
August 4-20, 2026
</p>

<!--
[Sources]
- GitHub Search API counts for pull requests opened August 4-20, 2026,
  retrieved August 20, 2026.
- `ai-review-findings.md` for the 115-PR completed-results sample.
-->

---

<!-- _class: setup -->

# Reusable AI Review Workflow

**Created:** a reusable, commit-pinned workflow that runs independent Claude and
Codex reviews through Amazon Bedrock.

**Add `.github/workflows/ai-pr-review.yml` in the consuming repository:**

```yaml
jobs:
  ai-pr-review:
    uses: aws/aws-durable-execution-ci/.github/workflows/ai-pr-review.yml@<full-commit-sha>
    secrets: inherit
```

**Consumer setup**

- Trigger with `pull_request_target`; grant `contents: read`, `id-token: write`,
  and `pull-requests: write`.
- Create `ai-pr-review` with required reviewers and `ai-pr-review-runtime` with
  `BEDROCK_ROLE_ARN` and no wait timer.

---

# How Did AI Review Help Authors and Reviewers?

## PR authors

| **93.2% finding coverage** | **67.9% applicable** | **74% faster initial feedback** | **67.6% clean approvals** |
| --- | --- | --- | --- |
| AI findings on 68 of 73 PRs with any findings | 74 of 109 adjudicated findings | P50: 1.5 hours to **23.1 minutes** | 23 of 34 eligible PRs |

## Human reviewers

| **53% fewer inline comments** | **39% fewer feedback rounds** | **17% fewer review submissions** | **49% lower P90 approval time** |
| --- | --- | --- | --- |
| 1.21 to **0.57** per PR | 0.75 to **0.46** per PR | 1.80 to **1.49** per PR | 116.7 to **59.4 hours** |

> **Preliminary and directional:** The observation window is short, the number
> of repositories and PRs is limited, and GitHub availability incidents
> interrupted some review runs.

<!--
[Sources]
- `ai-review-findings.md`, Scope and Limitations.
-->

---

<!-- _class: injection -->

# Prompt Injection Case: Conformance PR #15

<div class="causal-flow">
  <div class="flow-step">
    <span class="flow-step-number">1</span>
    <strong>Review needs the diff</strong>
    <p>The model receives the complete proposed change as context.</p>
  </div>
  <div class="flow-arrow">&rarr;</div>
  <div class="flow-step">
    <span class="flow-step-number">2</span>
    <strong>The diff contains instructions</strong>
    <p>PR #15 changed the AI prompt, so untrusted text resembled trusted directions.</p>
  </div>
  <div class="flow-arrow">&rarr;</div>
  <div class="flow-step">
    <span class="flow-step-number">3</span>
    <strong>The context boundary blurs</strong>
    <p>The trusted review prompt and untrusted diff share the model context.</p>
  </div>
  <div class="flow-arrow">&rarr;</div>
  <div class="flow-step">
    <span class="flow-step-number">4</span>
    <strong>The review is influenced</strong>
    <p>The proposed text can steer behavior despite instructions to treat it only as data.</p>
  </div>
</div>

<p class="injection-impact"><strong>Impact:</strong> The generated review could be manipulated. The case did not demonstrate code execution, branch modification, or credential theft.</p>

> "This demonstrates a real prompt injection risk. So we still have to rely on manual approvals for untrusted PRs."
> [Incident comment on PR #15](https://github.com/aws/aws-durable-execution-conformance-tests/pull/15#issuecomment-5050882517)

---

<!-- _class: security -->

# Security Measures and Prompt Injection Mitigations

| Control | Implementation |
| --- | --- |
| **Manual approval gate** | Forks, drafts, and Dependabot reviews require approval through the protected `ai-pr-review` environment before model execution. |
| **Trusted execution context** | Checks out the exact base SHA. PR code is never checked out or executed; metadata and the SHA-anchored diff come from the GitHub API. |
| **Sandboxed reviewers** | Claude and Codex run as dedicated unprivileged users against a read-only workspace with restricted inspection tools. |
| **Split privileges** | Model jobs can invoke Bedrock but cannot publish. Separate publication jobs get PR write access but no AWS credentials and run no model. |
| **Trusted, fail-closed publication** | Trusted code supplies reviewer identity, enforces a JSON schema, validates paths and changed lines, and rechecks base/head SHAs before posting. |
| **Pinned supply chain** | Actions and reusable workflows are commit-pinned, checkout credentials are disabled, and OIDC credentials are short-lived and Bedrock-scoped. |

*Security assurance: The workflow was audited and approved by the GenAI
Security team.*

<!--
[Sources]
- User-provided: the GenAI Security team audited and approved the workflow.
-->

---

<!-- _class: cover end -->
<!-- _paginate: false -->
<!-- _footer: "" -->

# AI Review Helps, Human Review Decides

## Supporting authors and reviewers, not replacing them

Questions and discussion
