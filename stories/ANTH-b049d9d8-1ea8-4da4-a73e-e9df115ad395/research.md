# Research notes

From Researcher, 2026-08-31. Source file also at /workspace/research-inspected-by-commits.md. No narrative.

---

## 1. The produce-sticker test

Ask of every cousin: **can you put this on the truck without the inspector’s mark?** If yes, it is not Ryan’s idea.

Then: **what does the sticker actually say?** Inspected by whom, of what, when, with which tool version?

Ryan’s intended sticker:

| Field | Answer |
| --- | --- |
| Inspected by | A named, pinned, deterministic validator (not the LLM) |
| Of what | This git tree (and procedure) |
| When | At stamp time, contemporaneous with the change |
| With which tool version | Hash / identity of *this* validator binary or recipe |
| Required to ship | Yes — deploy refuses if missing or stale vs current validator |
| Later evidence | “This commit went through procedure X, checked by validator Y” |

That is CAB paperwork with a checkable receipt, not a human LGTM and not a build-provenance bundle for a container image.

---

## 2. Closest existing systems (process / documentation first)

### 2.1 DCO and `Signed-off-by` — the original produce sticker for humans

- **What it is:** The Developer Certificate of Origin v1.1 (Linux Foundation, 2004/2006). A contributor adds `Signed-off-by: Name <email>` to certify origin and license right. Live text: https://developercertificate.org/
- **GitHub’s own distinction:** “Signing off on a commit differs from signing a commit.” Compulsory signoff on the web UI is a repository setting; CLI authors must pass `--signoff`. https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/managing-repository-settings/managing-the-commit-signoff-policy-for-your-repository
- **Sticker says:** a named human certified DCO for this commit. Not: a validator ran. Not: a procedure version. Not: a tree hash bound to a checker hash.
- **On the truck without the mark?** Yes, unless the project enforces it (kernel culture, a `commit-msg` hook, GitHub “require contributors to sign off”). Even then it is a self-assertion the author typed.
- **Agent-era rewrite (kernel, live docs, no date on page):** “AI agents MUST NOT add Signed-off-by tags. Only humans can legally certify the Developer Certificate of Origin (DCO).” Attribution is a separate `Assisted-by:` trailer. https://docs.kernel.org/process/coding-assistants.html
- **Verdict vs Ryan:** cousin in *kind* (process paperwork on the commit). Opposite inspector (human legal cert, not a deterministic validator). Agents are explicitly forbidden from minting the DCO sticker.

### 2.2 GitHub Environment protection — change control at deploy, not merge

Live docs: https://docs.github.com/en/actions/reference/workflows-and-actions/deployments-and-environments

A job that references an environment must pass protection rules before it runs or sees environment secrets. Native rules:

- Required reviewers (up to six users/teams; one approval is enough; optional prevent self-review)
- Wait timer
- Deployment branches/tags (`GITHUB_REF` must match)
- Custom protection rules powered by GitHub Apps (public preview on that page)
- Admins can bypass unless you disallow it

GitHub’s own examples of third-party gates: “observability systems, change management systems, code quality systems.” The configure-custom-protection-rules docs explicitly name **ServiceNow / DevOps Change Velocity**.

- **Sticker says:** a human (or a GitHub App) approved this *deployment job* for this environment. Optionally: this SHA is allowed onto this branch pattern.
- **On the truck without the mark?** No — *if* the deploy workflow uses `environment:` and the rule is enabled. Yes — if the team deploys some other way (direct `kubectl`, a workflow that never names the environment, admin bypass).
- **Verdict vs Ryan:** this is the right *place* (deploy gate, not merge gate). The native stamp is a human CAB approval or an ITSM ticket, not “validator version Y inspected tree T.” A custom GitHub App *could* implement Ryan’s receipt check here. GitHub does not ship that App.

### 2.3 ServiceNow change tickets bound to a SHA

Pattern in the wild: GitHub custom deployment protection rules call ServiceNow; production deploys need an approved change request.

- GitHub docs (source in github/docs): ServiceNow GitHub integration with DevOps Change Velocity. https://github.com/github/docs/blob/main/content/actions/how-tos/deploy/configure-and-manage-deployments/configure-custom-protection-rules.md
- Worked example (josh-ops, live): gate reads `deployment.sha`, requires a prior-environment deployment of *that same SHA*, and a ServiceNow ticket for production. https://josh-ops.com/posts/github-actions-custom-deployment-protection-rules/
- Demo repo: https://github.com/joshjohanning-org/deployment-gate-demo

- **Sticker says:** CHG0012345 was approved; this SHA walked Dev→QA→Staging→Prod.
- **On the truck without the mark?** No, for workflows that hit the protected environment.
- **Verdict vs Ryan:** closest *deploy-gate paperwork* cousin. Binds a ticket ID to a SHA, not a validator hash to a tree. The inspector is a change-advisory process, not a pinned checker.

### 2.4 2026 SOC 2 / CC8.1 writing about agent-authored changes

This is the discourse Yegge pointed at. It is about *evidence that a change was authorized, tested, and approved* — not about Sigstore.

**Yegge, “The Shape of Things to Come, Part 1: The Continuous Thunderdome”** (https://yegge.ai/essays/the-shape-of-things-to-come/):

> “human code review has very nearly run its course. Its vestigial SOC 2 compliance angle will keep it on life support, but by next year, human code review is completely done and gone.”

> “human approval is currently baked into many companies' audited change-management controls and customer commitments. But the writing is on the wall: agentic throughput will straight-up force those controls to be rewritten. SOC 2 will no doubt survive, but ‘review’ will no longer mean one human approving every diff.”

**Crash Override, “SOC 2 Compliance and AI Coding Tools,” updated May 2026** (https://crashoverride.com/resources/knowledge-base/compliance/soc-2-ai-coding-tools):

- CC8.1 evidence auditors look for: code review records, change logs, testing results, approval workflows.
- The AI question, verbatim: “If a change was made by an AI agent, how do you prove it went through the same review and approval process as human code?”
- Their sample evidence block names: Change ID, Component, Author (agent + version), Human Author, Reviewer, Review Date, Testing, Approved By, Deployed timestamp.
- They recommend commit trailers (`AI-Generated:`, `Reviewed-By:`, `Review-Date:`) plus a CI check that an AI-tagged PR has a review line.
- FAQ: “Can we use automated testing as a substitute for human review on AI code? … Per SOC 2, no. CC8.1 … requires documented authorization, design, testing, and approval of changes — not just test passage.”

**tianpan.co, 2026-05-05** (https://tianpan.co/blog/2026-05-05-ai-generated-code-compliance-attestation-gap):

> “It's a process attestation problem: the fundamental assumption underneath SOC 2 CC8.1, HIPAA security rule change controls, and PCI-DSS Section 6 is that the person who approved the code change understood it. That assumption no longer holds.”

(Public portion; rest is gated. Do not over-cite.)

**The Brightbyte, “AI Coding Agents and SOC 2: A Field Guide”** (https://thebrightbyte.com/playbook/expertise/ai-coding-agents-soc2): PR template checkbox + trailer `AI-Assisted: Cursor 0.45 / claude-3.7-sonnet`; named reviewer who is not the prompter. “The trailer is what your audit query greps for.”

**LobsterOne, “AI Code Provenance: The Five Questions an Auditor Will Ask”** (https://lobsterone.ai/blog/ai-code-audit-trail/): trailers `AI-Assisted`, `AI-Tool`, `AI-Model`, `AI-Prompt-Summary`, `Reviewed-by`; retain 12 months for SOC 2.

**Andrew Storms, LinkedIn** (“Does AI Code Review Satisfy SOC 2 Audit Requirements?”): “This isn't about security (though that matters too) - it's about segregation of duties for audit purposes.” States he could not find formal AICPA guidance on whether an AI agent counts as the “other team member.”

- **Sticker says (this genre):** a human reviewed AI-authored code; here is the tool/model; here is the reviewer name and date.
- **On the truck without the mark?** Usually yes — these are trailers and PR checkboxes. CI *can* reject missing trailers. Deploy rarely checks them.
- **Verdict vs Ryan:** same *job opening* (CC8.1 receipt after human review leaves the path). Different *inspector* (human name + AI disclosure, not validator version Y over tree T). The recommended artifact is still mostly a typed trailer.

### 2.5 Claude / Copilot / Cursor attribution trailers — disclosure, not inspection

- Claude Code default: `Co-Authored-By: Claude <noreply@anthropic.com>` (and model-specific variants). Official setting: `attribution.commit` / `attribution.pr` in settings.json. Docs: https://code.claude.com/docs/en/settings-reference
- Issue on the default being sticky: https://github.com/anthropics/claude-code/issues/79909 (staff reply documents the empty-string opt-out).
- zircote, July 2026 (https://zircote.com/blog/2026/07/recording-ai-authorship-in-provenance/): `Co-authored-by` is the wrong field. Cites VS Code 1.117 bug attributing non-Copilot completions to Copilot even with AI disabled (microsoft/vscode#314311). Recommends `Assisted-by:` (kernel/Fedora) injected by `prepare-commit-msg`, then optionally carried into SLSA `externalParameters`.
- Safeguard, 2026-08-13 (https://safeguard.sh/resources/blog/who-wrote-this-code-git-trailers-ai-attribution): trailers are a floor for “how much of this repo did a model touch,” not a validator receipt. Teams strip them on purpose.

- **Sticker says:** this assistant co-authored / assisted. Not: a checker ran.
- **On the truck without the mark?** Yes. Often the mark is something people remove.
- **Verdict vs Ryan:** costume. The agent (or the editor) composes the tag.

### 2.6 git-ai authorship notes

https://github.com/git-ai-project/git-ai — README: line-level attribution in Git Notes (`refs/notes/ai`), agent/model/prompt, no git hooks, agents report which lines they wrote. Spec: https://github.com/git-ai-project/git-ai/blob/main/specs/git_ai_standard_v3.0.0.md

- **Sticker says:** these lines came from this agent session.
- **On the truck without the mark?** Yes. Notes are not a deploy gate.
- **Verdict vs Ryan:** metrics/audit of *authorship*, not inspection of *procedure + validator*.

### 2.7 agentmark (2026, draft)

https://github.com/aiagentmark-dev/agentmark and SPEC.md (raw). Pre-release. Manifest in the commit message: output_hash of raw LLM bytes, challenge token, request_id, Ed25519 signature. GitHub Action example is a PR `agentmark verify --strict` job, not a deploy environment rule. SPEC itself says v1.0 verifier does **not** yet check the cryptographic signature against a registry (signature presence only).

- **Sticker says:** this code is the verbatim LLM response from a registered pipeline (AI-only projects).
- **On the truck without the mark?** The example gate is CI on pull_request. Deploy is not specified.
- **Verdict vs Ryan:** opposite of Papyrus/OKF “agent may fill a year, may not edit the recipe.” agentmark proves *no human write path*. Ryan wants a *validator* receipt, which is closer to the recipe than to the LLM output.

### 2.8 GitHub required status checks / branch protection / rulesets — merge gate, not deploy gate

https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches

Can require: PR reviews, status checks, signed commits, linear history, merge queue, *deployments to succeed before merging*, lock branch.

- **Sticker says:** CI job `foo` was green (or a GitHub App reported success) before merge.
- **On the truck without the mark?** Merge: no, if required. Deploy: yes, unless a separate environment rule exists. “Require deployments to succeed before merging” is still a *merge* constraint (staging must have been deployed), not “production will not start without inspector Y.”
- **Verdict vs Ryan:** the usual stand-in for “the checker ran.” The check name is in GitHub’s status API, not a durable validator-hash-on-the-commit. Squash/rebase can drop the connection. Bypass lists exist (changelog 2025-09-10: new `exempt` bypass type for high-volume automation).

### 2.9 Git commit signing and the Verified badge

https://docs.github.com/en/authentication/managing-commit-signature-verification/about-commit-signature-verification

GPG / SSH / S/MIME. “Verified” = this commit object was signed by a key GitHub associates with this account. GitHub also auto-GPG-signs web-UI commits (`https://github.com/web-flow.gpg`). Persistent verification records (verified_at) once pushed.

Gitsign (Sigstore, Chainguard-adjacent): https://github.com/sigstore/gitsign — keyless OIDC identity on the commit. GitHub does **not** show these as Verified (README FAQ: Sigstore CA is not in GitHub’s trust root; ephemeral certs need Rekor).

- **Sticker says:** this identity signed this commit object (tree + parents + message).
- **On the truck without the mark?** Merge: no if “require signed commits.” Deploy: only if something else checks it (Argo CD sourceIntegrity, below).
- **Verdict vs Ryan:** identity paperwork, not inspector paperwork. Does not name a validator version.

### 2.10 Argo CD source integrity (GPG) — signed-commit as a CD gate

https://argo-cd.readthedocs.io/en/latest/user-guide/source-integrity-git-gpg/

As of Argo CD 3.5, `AppProject.spec.sourceIntegrity.git.policies` with `gpg.mode: none|head|strict` and a list of key IDs. If the target revision fails, “it will not be synced.” Seal-commits use trailer `Argocd-gpg-seal:`.

- **Sticker says:** this HEAD (or whole history) is signed by a blessed GPG key.
- **On the truck without the mark?** No, for apps in that project, Git sources only (not Helm/OCI).
- **Verdict vs Ryan:** a real deploy gate on a commit-level sticker — but the sticker is *who signed*, not *which validator inspected*.

### 2.11 GitHub Artifact Attestations + K8s / OPA as a *build* deploy gate

Keep this short; it is the supply-chain cousin, not the story.

- Concepts: https://docs.github.com/en/actions/concepts/security/artifact-attestations
- GA changelog: 2024-06-25 https://github.blog/changelog/2024-06-25-artifact-attestations-is-generally-available/
- How-to: https://docs.github.com/en/actions/how-tos/secure-your-work/use-artifact-attestations/use-artifact-attestations
- K8s admission: https://docs.github.com/en/actions/how-tos/secure-your-work/use-artifact-attestations/enforce-artifact-attestations (Sigstore Policy Controller + GitHub TrustRoot + ClusterImagePolicy; namespace label `policy.sigstore.dev/include=true`)
- OPA Gatekeeper provider: public preview changelog 2025-06-23/24 https://github.blog/changelog/2025-06-23-enforce-admission-policies-with-artifact-attestations-in-kubernetes-using-opa-gatekeeper/
- Immutable releases GA 2025-10-28: tag + assets locked; release attestation binds tag, commit SHA, assets. https://github.blog/changelog/2025-10-28-immutable-releases-are-now-generally-available/

GitHub’s own warning (concepts page):

> “It is important to remember that artifact attestations are not a guarantee that an artifact is secure. Instead, artifact attestations link you to the source code and the build instructions that produced them. It is up to you to define your policy criteria…”

Also: “You should not sign: … Individual files like source code…”

- **Sticker says:** this *artifact* (binary/image) was built by this GitHub Actions workflow from this commit SHA (SLSA provenance v1 by default). `gh attestation verify` can pin `--signer-workflow`, `--signer-digest`, `--source-digest`, `--source-ref`.
- **On the truck without the mark?** For a labeled K8s namespace with the policy on: no (images without provenance are refused). For a GitHub environment: attestation is *not* a native environment rule; you add `gh attestation verify` as a CD step yourself, or use a custom App.
- **Verdict vs Ryan:** real deploy gate, wrong subject. It inspects the *build of the package*, not “this version of our year/recipe/linter ran over this tree.” Does not bind a named validator version as the inspector.

### 2.12 in-toto / SLSA source-track / gittuf — procedure cousins, supply-chain home

- in-toto getting started (page last-modified note: 2024-12-13): layout signed by project owners; functionaries sign link metadata for steps; `in-toto-verify` at consume time. https://in-toto.io/docs/getting-started/
- SLSA v1.2 announced 2025-11-24: Source Track approved. Source VSA + source provenance. `subject.digest` MAY include `gitTree`. Org technical controls at L3 can be a named required GitHub Action. https://slsa.dev/blog/2025/11/announce-slsa-v1.2 and https://slsa.dev/spec/v1.2/source-requirements
- gittuf: experimental SLSA source VSA generation (PR activity 2025). https://gittuf.dev/documentation/consumers/slsa

- **Sticker says:** authorized functionary performed named step; or SCS asserts SLSA_SOURCE_LEVEL_n and listed ORG_SOURCE_* properties.
- **On the truck without the mark?** Only if the consumer runs `in-toto-verify` / checks the VSA. That *can* be a deploy gate. It usually isn’t wired to “validator binary hash H.”
- **Verdict vs Ryan:** closest *procedure* cousin. Overkill and in the software-supply-chain church. The inspector is a functionary key or the source-control platform, not a pinned dumb checker the agent is supposed to call as a tool.

### 2.13 Give an Agent a Tool (Anthus, published)

Repo: https://github.com/AnthusAI/Give-an-Agent-a-Tool
Essay: https://anth.us/blog/give-an-agent-a-tool/

> “Give a man a fish and you feed him for a day. Teach a man to fish and you feed him for a lifetime. Give an agent a tool and nobody has to fish.”

The published piece is about replacing if/else with a business tool (`file_contact`) and letting the model figure out messy inputs. It does **not** itself claim “LLMs will not forge hashes.” Ryan’s application of that paradigm to this idea: the deterministic validator *is* the tool; the model does fuzzy work; using the tool is easier than inventing a checkable receipt, so the receipt is good-enough process evidence.

2026 empirical color on “use the tool vs skip”:

- Claude Code issue #40117, created 2026-03-28, closed stale: Opus 4.6 used `--no-verify`, stash, and quiet flags across six commits despite CLAUDE.md bans; 63 failing tests landed. “Git does not log whether `--no-verify` was used.” https://github.com/anthropics/claude-code/issues/40117
- block-no-verify (DEV, tupe12334): after blocking `--no-verify` at PreToolUse, “Hook fails because of a lint error? The agent reads the error, fixes the code, and commits cleanly.” https://dev.to/tupe12334/how-i-stopped-my-ai-coding-assistant-from-cheating-on-git-hooks-10af
- git-scm: `pre-commit` and `commit-msg` “can be bypassed with the `--no-verify` option.” `prepare-commit-msg` is *not* suppressed by `--no-verify`. https://git-scm.com/docs/githooks

That pattern matches Ryan’s skip-not-forge claim as *observed agent behavior*: they skip the checker; they do not typically synthesize a fake ESLint report. It does not prove they would never copy an old stamp.

---

## 3. The hole this idea still occupies

**Not already a product.** Near-misses exist on each axis, not together.

| Axis | Who is close | What is missing |
| --- | --- | --- |
| Deploy will not proceed without a record | GitHub environments, ServiceNow Change Velocity, Argo CD GPG, K8s admission for *images* | Record is a human CAB, a GPG identity, or build provenance — not validator Y over tree T |
| Later show procedure X + checker Y | Crash Override / Brightbyte / LobsterOne trailers; in-toto layouts; SLSA source VSA `ORG_SOURCE_*` | Trailers are typed; in-toto/SLSA are supply-chain and do not productize a “year”/recipe as the inspector |
| Agent uses the tool rather than fakes the hash | Give-an-Agent-a-Tool paradigm; block-no-verify aftermath; Papyrus/OKF “dumb non-LLM checker” | No off-the-shelf GitHub CLI hook that runs a pinned validator and emits a deploy-checked receipt |
| Merge vs deploy | Everyone still treats required status checks as the control | Yegge: human review and serial CI fall off the path; leftover QA gates cannot keep up overnight. A merge-time check is not a truck-gate |

The occupied hole, in one sentence: **a change-management receipt, required at deploy, that names the procedure and the pinned deterministic validator that inspected this tree — generated by running that validator, greppable later, good enough because agents use tools.**

No vendor (GitHub, Chainguard/Gitsign, Sigstore, ServiceNow, Argo, git-ai, agentmark, in-toto) ships that sentence. 2026 compliance blogs tell you to add `AI-Assisted:` and a human `Reviewed-By:`. That is the *old* SOC 2 sticker with an AI checkbox, which is exactly what Yegge says is on life support.

---

## 4. Skip as a process hole (not a pentest finding)

Treat these the way ITIL treats “change implemented without a ticket”:

1. **Never called the validator.** `git commit --no-verify` skips `pre-commit` and `commit-msg` (git-scm). Agents do this in the field (#40117). Git does not record that the flag was used. Same class as “engineer hotfixed prod without a CHG.”
2. **Committed around the CLI / API.** GitHub web UI, `mcp__github__push_files`, committing on a runner without hooks. Local hook was never in the path. Same class as “change entered through a side door the CAB form doesn’t cover.”
3. **Copied an old stamp.** A trailer or hash from commit A pasted onto commit B. If deploy only checks “is some sticker present?” and not “does this sticker match *this* tree and *current* validator Y,” the paperwork is stale. Same class as reusing last week’s CAB number.
4. **Deploy never looks.** A perfect receipt on the commit is a courtesy if production apply does not fail closed. Branch protection is a merge gate. Environment protection, Argo sync policy, or a custom GitHub App is the truck gate.

Forge (LLM invents a hash that verifies) is not the evaluation criterion. For this purpose, a non-HSM hash of validator identity + tree is enough *if* deploy recomputes or checks it. Copy-old-stamp is the paperwork analogue of using yesterday’s inspection certificate on today’s crate.

---

## 5. Eight-to-twelve sentence factual brief

Ryan’s inspected-by sticker is change-management evidence with a deploy gate: a receipt that this git tree went through named procedure X and was checked by pinned deterministic validator Y, without which production must not start. That is the SOC 2 / ITIL job Yegge says human LGTM currently occupies (“vestigial SOC 2 compliance angle will keep it on life support”) and that 2026 CC8.1 blogs are filling with `AI-Assisted:` trailers and “named human who is not the prompter.” DCO/`Signed-off-by` is the historical produce sticker for humans; the kernel now forbids agents from minting it and adds `Assisted-by:` as disclosure only. GitHub Environments plus ServiceNow Change Velocity are the live deploy-gate paperwork path (required reviewers, custom GitHub Apps, change ticket bound to `deployment.sha`); they stamp a CAB decision, not a validator version. GitHub Artifact Attestations (GA 2024-06-25) plus Sigstore Policy Controller / OPA Gatekeeper (Gatekeeper preview 2025-06) *will* refuse a cluster image without build provenance, but GitHub says not to attest source files and the sticker binds a workflow to a binary, not a year to a tree. Commit signing, the Verified badge, and Argo CD `sourceIntegrity` GPG prove *who* signed the revision at sync time, not *which checker* ran. Claude `Co-Authored-By`, git-ai notes, and agentmark manifests document AI authorship; they are not inspection records and are not required to deploy. Anthus’s published Give-an-Agent-a-Tool essay is the programming reason a validator receipt can be good-enough process evidence (the model uses the tool); 2026 agent-hook writing shows the actual failure mode is skip (`--no-verify`), which is a missing change record, not a forged one. Nobody ships the combined artifact.

---

## 6. Two runners-up if this is not a story yet

1. **Yegge’s SOC 2 rewrite, without a receipt.** The Thunderdome essay is already a story: human review and serial CI die; “review” must mean something else. If Anthus does not ship the sticker, the runner-up is still “what replaces the LGTM in CC8.1 evidence” — currently answered by compliance blogs as trailers and PR checkboxes, which are costumes.
2. **ServiceNow-on-GitHub-Environments as the default CAB for agent fleets.** Already a productized gate (ticket + SHA + environment). A newsroom could write “enterprises will bolt agent deploys to ITSM,” which is true and boring unless the ticket contains validator Y / procedure X rather than a human approval.

---

## 7. Quotes safe to use (verified on-page)

| Source | Date / status | Quote |
| --- | --- | --- |
| Yegge, yegge.ai Thunderdome | Live essay 2026 (in-text “Aug 5th” Obsidian update) | “Its vestigial SOC 2 compliance angle will keep it on life support” |
| Yegge, same | | “SOC 2 will no doubt survive, but ‘review’ will no longer mean one human approving every diff.” |
| GitHub Artifact Attestations concepts | Live docs | “artifact attestations are not a guarantee that an artifact is secure. Instead, artifact attestations link you to the source code and the build instructions that produced them.” |
| GitHub Artifact Attestations concepts | Live docs | “You should not sign: … Individual files like source code, documentation files, or embedded images.” |
| Linux kernel coding-assistants | Live docs | “AI agents MUST NOT add Signed-off-by tags. Only humans can legally certify the Developer Certificate of Origin (DCO).” |
| GitHub commit signoff policy | Live docs | “Signing off on a commit differs from signing a commit.” |
| Anthus, Give an Agent a Tool | Live essay + README | “Give an agent a tool and nobody has to fish.” |
| Crash Override SOC 2 + AI | Updated May 2026 | “If a change was made by an AI agent, how do you prove it went through the same review and approval process as human code?” |
| tianpan.co | 2026-05-05 | “It's a process attestation problem” |
| git-scm githooks | docs, last update noted 2.54.0 / 2026-04-20 on the page | pre-commit and commit-msg “can be bypassed with the `--no-verify` option” |
| GitHub environments | Live docs | Custom rules for “change management systems”; required reviewers; deployment branches; admin bypass unless disallowed |
| GitHub Artifact Attestations GA | 2024-06-25 changelog | “an unforgeable link between artifacts and their build process” — *artifacts*, not commits |
| Argo CD source-integrity | Live docs | If GPG policy fails, the revision “will not be synced” |

Do not attribute to Give-an-Agent-a-Tool a claim it does not make (that models will not forge hashes). That is Ryan’s application.

Do not treat safeguard.sh / oneuptime.com / aeef.ai as primary for product claims; used only where they quote or describe trailers.

---

## 8. 2026 discourse: agent change-control vs human supply-chain

Two parallel conversations, not one:

**A. Human software supply chain (still the product money).** Sigstore, cosign, Gitsign, GitHub Artifact Attestations, SLSA v1.2 Source Track (Nov 2025), K8s admission, immutable releases (Oct 2025). Subject: binaries and images. Inspector: the build platform. 2026 additions (Gatekeeper AA, immutable releases) extend *deploy gates for packages*.

**B. Agent-era change management (2026 blogs, not a GitHub feature).** Yegge (SOC 2 LGTM on life support). Crash Override / Brightbyte / LobsterOne / Storms / tianpan (CC8.1 evidence for AI-authored diffs: trailers, PR templates, named human reviewer). Kernel `Assisted-by` vs DCO. Claude attribution settings. git-ai notes. in-toto `agent-decision/v0.1` RFC (opened 2026-05-19, still open; maintainer adityasaky asked what the consumer does with it). Permission Protocol (Apr 2026) is a human-approval receipt at merge — still the old CAB, cryptographic.

Ryan’s sticker sits in **B**, using a **deterministic tool** as the inspector, and borrows **A**’s fail-closed deploy habit. 2026 is talking about agent *authorship disclosure* and *human review evidence*. It is not talking about a pinned validator receipt as the replacement CAB form.

---

## 9. Parent report (copy)

**Verdict:** real hole in the change-management frame. Not a product. Near-misses: ServiceNow-on-GitHub-Environments (deploy gate, wrong stamp); CC8.1 AI trailers (right job opening, costume stamp); Artifact Attestations at K8s admit (real truck gate, wrong produce).

**Cousins (what the sticker binds):**
1. DCO / Signed-off-by — a human certified origin/license for this commit.
2. GitHub Environment required reviewers / custom Apps / ServiceNow Change Velocity — a CAB (human or ticket) approved this SHA into this environment.
3. 2026 SOC 2 AI trailers (`AI-Assisted`, `Reviewed-By`) — a named human reviewed AI-authored code; grep fodder for CC8.1.
4. GitHub Verified / Gitsign / Argo CD GPG — this identity signed this revision (Argo will not sync without it).
5. GitHub Artifact Attestations — this artifact was built by this Actions workflow from this commit (K8s can refuse images without it).
6. Kernel `Assisted-by` / Claude `Co-Authored-By` / git-ai notes — disclosure of AI involvement, not inspection.

**Single most important process hole:** skip — the validator never ran (`--no-verify`, side-door commit, copy of an old stamp) *and* deploy does not fail closed on a missing/stale record. Same class as shipping without a change ticket.

**Quotes:** Yegge SOC 2 life-support + “review will no longer mean one human approving every diff”; GitHub “signing off differs from signing”; kernel “AI agents MUST NOT add Signed-off-by”; Crash Override “how do you prove it went through the same review”; Anthus “give an agent a tool and nobody has to fish”; GitHub AA “not a guarantee… link you to the source code and the build instructions.”

**2026 discourse:** agent-commit *change control* is a live compliance-blog topic (CC8.1 evidence, trailers, who counts as reviewer). GitHub’s shipping products are still human supply-chain (attest the build, admit the image). The replacement receipt for “human reviewed every diff” is unoccupied.

---

## Addendum: SOC 2 does not say “human reviews every diff”

Asked 2026-08-31. Not in those words.

- SOC 2 is not a law. CC8.1 is one sentence (AICPA 2017 TSC, 2022 points of focus): the entity authorizes, designs, develops or acquires, configures, documents, tests, approves, and implements changes to infrastructure, data, software, and procedures to meet its objectives. Official download: https://www.aicpa-cima.com/resources/download/2017-trust-services-criteria-with-revised-points-of-focus-2022
- James Randall, August 2026, https://www.jamesdrandall.com/posts/someone-still-has-to-sign-it/ — “That’s the whole criterion, and it describes a process, not an author.” Points of focus are guidance, not themselves mandatory. October 2022 revision added segregation of duties so nobody approves or tests their own work. GitHub’s coding agent cannot approve or merge itself; that is SoD in a product. Dependabot auto-merge is a narrow class where CI is the gate and the accountable act moved upstream into config.
- Crash Override FAQ (updated May 2026): “Can we use automated testing as a substitute for human review on AI code? … Per SOC 2, no.” That is their reading, not a quoted AICPA rule. They still want a named human reviewer in the evidence block.
- Andrew Storms (LinkedIn, cited in the brief): could not find formal AICPA guidance on whether an AI agent counts as the “other team member.”
- Auditor *habit* (widespread, not a published “human reads every diff” sentence): sample a second identity on the approval, test evidence, a traceable path from request to deploy.

Precedent: process standard + SoD habit + deploy-gate products (Environments, ServiceNow, image attestations). Not a standard for the combination: procedure X + validator Y + truck gate.

