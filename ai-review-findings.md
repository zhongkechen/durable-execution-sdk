# AI Review Findings

Data collected from public GitHub pull request, review, comment, and workflow
metadata through August 20, 2026.

## Scope

This report evaluates the shared AI pull request review workflow from
[`aws/aws-durable-execution-ci`](https://github.com/aws/aws-durable-execution-ci)
across:

- `aws/aws-durable-execution-sdk-java`
- `aws/aws-durable-execution-sdk-js`
- `aws/aws-durable-execution-sdk-python`
- `aws/aws-durable-execution-conformance-tests`

The docs repository is excluded because its AI review runtime is not configured
correctly yet. Workflow coverage is not treated as an efficiency or quality
metric because some reviews were interrupted by GitHub availability incidents
or superseded workflow runs.

Dependabot and other bot-authored pull requests are excluded. Results are
reported only in aggregate because the repository-level samples are too small
for reliable comparisons.

## Summary

| Category | Headline result |
| --- | --- |
| AI review effectiveness | 74 of 109 adjudicated findings were applicable, or **67.9%**. Among finding-bearing PRs that later received human approval, 23 of 34, or **67.6%**, received a clean approval with no human corrective feedback. |
| Help for PR authors | At least 17 PRs were revised in response to 63 explicitly applied AI findings. Median time to first review fell from 1.5 hours to **23.1 minutes**. |
| Help for human reviewers | Human inline comments per PR fell **53%**, and non-approval feedback rounds per PR fell **39%**. Median approval time did not improve, although P90 approval time fell **49%**. |

These are early observational results, not proof that AI review caused every
measured change.

## Method

### Datasets

The quality analysis includes AI review activity after migration to the shared
workflow through August 20:

- 115 human-authored pull requests with an AI review result.
- 489 raw inline AI comments.
- 394 distinct repository/PR/file/line findings after deduplicating reruns.

The before/after efficiency comparison uses:

- A 14-day baseline before AI review was introduced.
- A 14-day period after migration to the shared workflow.
- Human-authored pull requests created during each period.
- Pull requests that were subsequently merged.

The matched sample contains 103 merged pull requests in the baseline and 87 in
the shared-workflow period.

### Applicability

Maintainer replies to AI findings were classified as:

- **Applied or accepted:** The reply confirms a fix, code change, test,
  documentation change, or acceptance of the finding.
- **Valid but deferred:** The concern is acknowledged but assigned to later
  work or an intentional rollout boundary.
- **Rejected:** The reply identifies a false positive, stale finding,
  intentional behavior, unsupported compatibility expectation, or issue
  outside the pull request's scope.
- **Unclear:** The reply does not establish a final verdict.

Workflow reruns can repeat a finding. Deduplicated findings are grouped by
repository, pull request, file, and original line. The latest maintainer
verdict is used.

### Author-confirmed revisions

A finding counts as author-confirmed applied only when the pull request author
explicitly says it was fixed, addressed, changed, tested, documented, or
otherwise incorporated.

Later commits, outdated threads, and resolved conversations are not treated as
proof that AI caused a revision unless the author explicitly connects the
change to the finding. Valid but deferred findings are reported separately.

### Review timing

A review event is:

- A human GitHub review submitted as `APPROVED`, `COMMENTED`, or
  `CHANGES_REQUESTED` by someone other than the pull request author.
- An AI inline finding or AI review summary, including a no-findings summary.

Time to first review is measured from pull request creation to the earliest
qualifying human or AI event.

GitHub does not expose a reliable historical push timestamp for every sampled
commit. `pushedDate` was null in the GraphQL data, and Actions did not retain
pull request/head associations for the sampled merged pull requests.
Follow-up timing is therefore an estimate:

- Committer time is used as a proxy for push time.
- Commits within five minutes are treated as one code-update batch.
- A review counts only when it arrives before the next update or merge.
- Percentiles include only update intervals with an observable review.

The P50 through P90 results were unchanged when the batch threshold varied
from zero to five minutes and changed only slightly at ten minutes.

### Human reviewer workload

Human reviewer metrics exclude:

- Reviews submitted by the pull request author.
- Bot reviews.
- Dismissed reviews.
- Reviews submitted after merge.

A human feedback round is a `COMMENTED` or `CHANGES_REQUESTED` review
submission. Human inline-comment counts are code review comments attached to
human review submissions.

For the PR-level AI-versus-human finding comparison, a human finding means a
human `COMMENTED` or `CHANGES_REQUESTED` review, or an inline comment attached
to any human review. A PR appearing in both groups is counted once in the
combined denominator.

## 1. AI Review Effectiveness

### Headline measures

| Metric | Result |
| --- | ---: |
| Pull requests with an AI review result | 115 |
| Pull requests with at least one distinct finding | 68 (59.1%) |
| Pull requests with no inline finding | 47 (40.9%) |
| Raw inline AI comments | 489 |
| Distinct findings after deduplicating reruns | 394 |
| Directly applicable GitHub suggestion blocks | 63 |
| Applicable findings among decisive verdicts | 74 of 109 (67.9%) |
| Rejected or non-actionable findings among decisive verdicts | 35 of 109 (32.1%) |
| PRs with AI findings among PRs with any AI or human finding | 68 of 73 (93.2%) |
| Confirmed applicable findings per reviewed PR | 0.64 |
| Confirmed applicable findings per finding-bearing PR | 1.09 |
| Clean human approvals after AI findings | 23 of 34 eligible PRs (67.6%) |

Raw comment counts include repeated findings from workflow reruns. Distinct
finding counts are the better measure of review output.

The confirmed applicable rates per PR are lower bounds because 283 of 394
distinct findings did not receive an explicit verdict.

### PR-level AI and human finding coverage

| Finding source | Pull requests |
| --- | ---: |
| AI finding | 68 |
| Human finding | 21 |
| Both AI and human findings | 16 |
| AI finding only | 52 |
| Human finding only | 5 |
| Any AI or human finding | 73 |

AI reported at least one finding on:

**68 / 73 = 93.2%**

of pull requests that received any AI or human review finding. This is a
PR-level coverage ratio, not evidence that AI found 93.2% of individual issues.

### Applicability of raw comments

| Verdict | Comments | Percent of all comments | Percent of decisive verdicts |
| --- | ---: | ---: | ---: |
| Applied or accepted | 79 | 16.2% | 60.8% |
| Valid but deferred | 9 | 1.8% | 6.9% |
| Rejected / non-actionable | 42 | 8.6% | 32.3% |
| Unclear | 2 | 0.4% | - |
| No explicit maintainer verdict | 357 | 73.0% | - |
| **Total** | **489** | **100%** | **100% (n=130)** |

Among the 130 raw comments with a decisive verdict, 88 were applicable:

**88 / 130 = 67.7%**

### Applicability of distinct findings

| Verdict | Distinct findings | Percent of all findings | Percent of decisive verdicts |
| --- | ---: | ---: | ---: |
| Applied or accepted | 67 | 17.0% | 61.5% |
| Valid but deferred | 7 | 1.8% | 6.4% |
| Rejected / non-actionable | 35 | 8.9% | 32.1% |
| Unclear | 2 | 0.5% | - |
| No explicit maintainer verdict | 283 | 71.8% | - |
| **Total** | **394** | **100%** | **100% (n=109)** |

Among the 109 distinct findings with a decisive verdict:

- Applicable: **74 / 109 = 67.9%**
- Rejected or non-actionable: **35 / 109 = 32.1%**
- Strictly applied or accepted: **67 / (67 + 35) = 65.7%**

The confirmed applicability lower bound across all findings is:

**74 / 394 = 18.8%**

This lower bound is not the overall precision. Most findings received no
explicit verdict. Of the 283 unadjudicated findings, 98 later became outdated
because the relevant code changed, but that alone does not prove correctness
or causation.

### Findings per pull request

Across all 115 human-authored reviewed pull requests:

| Metric | Distinct findings per PR |
| --- | ---: |
| Mean | 3.4 |
| P50 | 1 |
| P75 | 4 |
| P90 | 8 |

Among the 68 pull requests with at least one finding:

| Metric | Distinct findings per PR |
| --- | ---: |
| Mean | 5.8 |
| P50 | 3 |
| P75 | 7 |
| P90 | 12 |
| Maximum | 57 |

| Distinct findings on a PR | Pull requests | Percent of reviewed PRs |
| --- | ---: | ---: |
| 0 | 47 | 40.9% |
| 1 | 17 | 14.8% |
| 2-4 | 24 | 20.9% |
| 5-9 | 18 | 15.7% |
| 10 or more | 9 | 7.8% |
| **Total** | **115** | **100%** |

### Clean human approval after AI findings

As a strict proxy for "AI found all issues observed during review," a pull
request must:

- Be human-authored and receive at least one AI inline finding.
- Receive a later human approval.
- Receive no human `COMMENTED` or `CHANGES_REQUESTED` review.
- Receive no human inline comments, including on the approval review.

| Denominator | Clean approvals | Ratio |
| --- | ---: | ---: |
| Finding-bearing PRs with a later human approval | 23 of 34 | **67.6%** |
| All finding-bearing PRs | 23 of 68 | **33.8%** |
| All human-authored AI-reviewed PRs | 23 of 115 | **20.0%** |

Of the 23 cleanly approved PRs, 22 had merged by the data cutoff and one
remained open after approval.

This demonstrates that human reviewers found no additional issues in those
GitHub review records. It does not prove that AI found every possible defect;
reviewers can also miss issues, and ordinary issue comments are not included
in this proxy.

## 2. Help for PR Authors

### PRs revised from AI feedback

Authors explicitly confirmed that they applied or fixed 63 distinct findings
across at least 17 pull requests:

- 17 of 115 reviewed pull requests, or **14.8%**, were explicitly revised
  based on AI feedback.
- Among the 68 pull requests that received findings, 17, or **25.0%**, had an
  explicitly confirmed revision.
- This equals **0.55 author-confirmed applied findings per reviewed PR**, or
  0.93 per finding-bearing PR.

Another five findings were accepted as valid but deferred. Including them
brings the author-acknowledged applicable total to 68 findings across 19 pull
requests:

- 19 of 115 reviewed pull requests, or **16.5%**.
- 19 of 68 finding-bearing pull requests, or **27.9%**.
- Affected pull requests had a median of 3 applicable findings, a mean of 3.6,
  and a maximum of 14.

Authors acknowledged findings after a median of 24 minutes; P75 was 1 hour.
These ratios are lower bounds because most findings did not receive an
explicit author verdict.

### Time to initial feedback

The first three result columns compare the period before AI adoption with two
views of the same period after adoption: the earliest AI or human review, and
the first human review when AI events are ignored.

| Percentile | Before AI adoption: first human review | After AI adoption: first AI or human review | Change from before to earliest after | After AI adoption: first human review |
| --- | ---: | ---: | ---: | ---: |
| P50 | 1.5 hours | 23.1 minutes | **-74%** | 1.3 hours |
| P75 | 21.5 hours | 2.9 hours | **-86%** | 13.5 hours |
| P90 | 67.1 hours | 21.8 hours | **-67%** | 42.5 hours |

All 103 baseline and 87 shared-period pull requests received a qualifying
review before merge.

In the shared period:

- AI was first on 43 of 87 pull requests, or **49.4%**.
- A human was first on 44 of 87 pull requests, or **50.6%**.
- AI-first reviews arrived after a median of **10.0 minutes**.
- Human-first reviews arrived after a median of **43.6 minutes**.
- Across pull requests reviewed from their initial revision, median AI
  feedback was **19.4 minutes**.

### Time to follow-up feedback

Because historical push timestamps are unavailable, these are estimated
commit-to-review times using the method described above.

| Percentile | Before AI adoption: next human review | After AI adoption: next AI or human review | Change from before to earliest after | After AI adoption: next human review |
| --- | ---: | ---: | ---: | ---: |
| P50 | 30.4 minutes | 14.0 minutes | **-54%** | 29.1 minutes |
| P75 | 1.3 hours | 35.3 minutes | **-54%** | 2.0 hours |
| P90 | 13.5 hours | 2.6 hours | **-81%** | 15.6 hours |

The baseline contained 83 observable update-to-review intervals. The shared
period contained 104 intervals when the first AI or human review was counted.

In the shared period:

- AI was first on 62 of 104 intervals, or **59.6%**.
- A human was first on 42 of 104 intervals, or **40.4%**.
- AI-first follow-ups arrived after a median of **12.0 minutes**.
- Human-first follow-ups arrived after a median of **25.3 minutes**.

The shared-period human-only median was 29.1 minutes, close to the 30.4-minute
baseline. The 14.0-minute combined median therefore reflects AI providing
earlier feedback rather than humans responding faster.

### End-to-end author outcomes

| Metric | Before AI adoption | After AI adoption | Change |
| --- | ---: | ---: | ---: |
| Median merge time | 4.4 hours | 3.2 hours | **-27%** |
| Median commits per PR | 2 | 2 | No change |

The merge-time result is encouraging but may reflect changes in pull request
mix and release activity. The unchanged commit count does not show faster
initial authoring.

## 3. Help for Human Reviewers

### Human review workload

| Metric | Before AI adoption | After AI adoption | Change |
| --- | ---: | ---: | ---: |
| Human review submissions | 185 total; 1.80 per PR | 130 total; 1.49 per PR | **-17% per PR** |
| Human inline review comments | 125 total; 1.21 per PR | 50 total; 0.57 per PR | **-53% per PR** |
| Non-approval human feedback rounds | 77 total; 0.75 per PR | 40 total; 0.46 per PR | **-39% per PR** |
| PRs receiving human non-approval feedback | 32 of 103 (31.1%) | 19 of 87 (21.8%) | **-9.3 points** |
| PRs receiving two or more feedback rounds | 14 of 103 (13.6%) | 11 of 87 (12.6%) | -1.0 point |
| PRs receiving a formal change request | 6 of 103 (5.8%) | 1 of 87 (1.1%) | **-4.7 points** |

The strongest reviewer-workload signal is lower comment volume and fewer
non-approval review submissions. This is consistent with AI reducing
issue-finding work while leaving the final human approval step intact.

The proportion of PRs with two or more human feedback rounds changed little.
The reduction appears to come mainly from more PRs requiring no human
corrective feedback, rather than eliminating repeated loops on every complex
PR.

### Time to human approval

| Percentile | Before AI adoption: first human approval | After AI adoption: first human approval | Change |
| --- | ---: | ---: | ---: |
| P50 | 2.2 hours | 2.6 hours | **+22%** |
| P75 | 25.4 hours | 19.5 hours | **-23%** |
| P90 | 116.7 hours | 59.4 hours | **-49%** |

Median approval time did not improve. The upper tail did: P75 and P90 approval
times were lower in the shared-workflow period.

For time between first human review and first approval, the median was zero in
both samples because most first human reviews were approvals. P90 fell from
23.8 hours to 2.6 hours, suggesting that the longest human review loops closed
faster, although the sample is too short to attribute that change solely to
AI.

## Interpretation

The most defensible effectiveness statement is:

> Approximately two-thirds of AI review findings explicitly adjudicated by
> maintainers were applicable.

For authors, the strongest signals are the explicitly confirmed revisions and
the shorter wait for initial and follow-up feedback. AI delivered about half
of initial reviews first and about three-fifths of observable follow-up reviews
first.

For reviewers, the strongest signals are 53% fewer inline comments per PR and
39% fewer non-approval feedback rounds per PR. Median approval time did not
improve, so the data supports reduced review work more strongly than faster
approval.

The results should not be presented as causal proof because:

- The observation window is short.
- Pull request complexity and release activity changed between periods.
- There is no randomized or contemporaneous control group.
- Some reviews were cancelled or delayed by GitHub availability incidents.
- Historical push timing is estimated from commit timestamps.
- Repeated model runs can produce semantically duplicate findings.
- Most findings do not receive an explicit maintainer verdict.

## Recommended Measurement Improvements

Future AI review comments should include a stable finding identifier so the
same issue can be tracked across reruns and revisions.

Maintainers could resolve comments with a structured verdict:

- `accepted`
- `deferred`
- `false-positive`
- `out-of-scope`
- `already-fixed`

The shared workflow could export:

- Time to first AI feedback.
- Time to first review from either AI or a human.
- Time from each `synchronize` event to the next AI or human review.
- Time from finding to corrective commit.
- Applied suggestion count.
- Applicable-finding precision.
- Findings repeated across model reruns.
- Human inline comments and feedback rounds per pull request.
- Time from first human review to approval.

For exact follow-up timing, the workflow should record the pull request event
type, event timestamp, pull request number, and head SHA on every `opened` and
`synchronize` run. Review output should carry the same head SHA so each review
can be joined to the code revision it evaluated.

This would replace proxy measurements with a durable, auditable quality and
efficiency dataset.
