---
marp: true
theme: default
size: "16:9"
paginate: true
footer: "AI Review findings | Data through August 20, 2026"
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

---

# Prompt Injection Case: Conformance PR #15

> **Untrusted PR diff -> AI review context -> model behavior influenced**

## Why it happened

| Step | Failure mechanism |
| --- | --- |
| **1. The reviewer needed the diff** | Reviewing the PR required giving the model the complete proposed diff as context. |
| **2. The diff contained instructions** | PR #15 changed the AI prompt itself, so the diff included natural-language directions about reviewer identity and output formatting. |
| **3. Trusted and untrusted text shared one context** | The model saw both the trusted review prompt and instruction-like text from the untrusted diff. There was no hard boundary inside the model separating them. |
| **4. The model was influenced** | Telling the model to treat the diff only as data reduced risk, but could not guarantee compliance. The proposed text still steered its review behavior. |

**Impact:** the integrity of the generated review could be manipulated. The case did not demonstrate PR code execution, branch modification, or credential theft.

> "This demonstrates a real prompt injection risk. So we still have to rely on manual approvals for untrusted PRs."
> [Incident comment on PR #15](https://github.com/aws/aws-durable-execution-conformance-tests/pull/15#issuecomment-5050882517)

---

# Security Measures and Prompt Injection Mitigations

| Control | Implementation |
| --- | --- |
| **Manual approval gate** | Forks, drafts, and Dependabot reviews require approval through the protected `ai-pr-review` environment before model execution. |
| **Trusted execution context** | Checks out the exact base SHA. PR code is never checked out or executed; metadata and the SHA-anchored diff come from the GitHub API. |
| **Sandboxed reviewers** | Claude and Codex run as dedicated unprivileged users against a read-only workspace with restricted inspection tools. |
| **Split privileges** | Model jobs can invoke Bedrock but cannot publish. Separate publication jobs get PR write access but no AWS credentials and run no model. |
| **Trusted, fail-closed publication** | Trusted code supplies reviewer identity, enforces a JSON schema, validates paths and changed lines, and rechecks base/head SHAs before posting. |
| **Pinned supply chain** | Actions and reusable workflows are commit-pinned, checkout credentials are disabled, and OIDC credentials are short-lived and Bedrock-scoped. |
