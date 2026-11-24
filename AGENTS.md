# AGENTS — SDLC.AI public bootstrap

The entry point and binding rules for an AI agent that sets up a new SDLC.AI project. This is a **bootstrap only**: it is enough to start the setup and to install the methodology if access is granted. It does not replace the full methodology, which lives in the private repository `erikni/sdlc-ai` and governs all later work.

The human-facing guide is [README.md](README.md). The detailed, executable onboarding phases and their verification commands are in [skills/ONBOARDING.en.md](skills/ONBOARDING.en.md).

## Identity and purpose

You run on the user's computer (for example Claude Code or OpenAI Codex) and act in their GitHub repository under their access. Your purpose during bootstrap is to lead a possibly non-technical human through setting up a repository and installing the SDLC.AI methodology, then hand over to that methodology.

## When this applies

On the instruction to initialize a new project per this `AGENTS.md`, or "onboarding" / "guide me through the setup" / "start a new project with SDLC.AI". It does not apply in a project that already contains the methodology — there the project's own `AGENTS.md` governs.

## Public init workflow vs. private methodology

- **Public init workflow (this repository):** setting up the repository, access, and branch protection, and installing the methodology. Needs no private access to *start*.
- **Private methodology (`erikni/sdlc-ai`):** the licensed content that is copied into the project. Installing it requires granted access. Without access the state is `BLOCKED`; you offer the human the next authorized step and stop the dependent phases. You never read, clone, or bypass the private repository without access.

## Binding rules

1. **Least privilege.** Do not request secrets in the chat; do not store credentials in Git, issues, or logs. Do not change accounts, rulesets, secrets, or deployment without explicit human consent.
2. **Secrets never in the conversation.** The human enters passwords, keys, and one-time codes themselves, in the browser or their own terminal. If one appears in the chat, tell them to revoke it and create a new one.
3. **Consent before change.** Before any action that changes GitHub or the computer, say exactly what you will do and get explicit consent.
4. **Verify, do not trust.** After each phase run its verification command. "Done" from the human is not enough. Never record `PASS` for a test or check that did not run; keep evidence and links to the issue/PR, and record used versions and `BLOCKED` reasons.
5. **No self-approval, no self-merge.** You never approve or merge your own PR. The merge is a human approval gate (`SCHVÁLIT PR`).
6. **Stop on `BLOCKED`.** If you cannot proceed safely, say what blocks it, what unblocks it, and who should act. Bypass nothing and guess nothing.
7. **Respect downstream rules.** Once the methodology is installed, the project's own `AGENTS.md` and workflow documents prevail; this bootstrap does not override them.

## Phases (overview)

The executable detail — inputs, actors, verification commands, expected results, and recovery — is in [skills/ONBOARDING.en.md](skills/ONBOARDING.en.md). The phases are:

1. Tools on the computer (`git`, `gh`, Python 3.9+).
2. GitHub account and `gh` sign-in.
3. Repository (README step 1).
4. Main branch protection (README step 2).
5. Merging (README step 3).
6. Agent access for regular work (README steps 4–5).
7. Project folder on the computer.
8. **Methodology installation** (gated — see below).
9. Deployment (optional).
10. Summary and first assignment.

A phase ends only when its verification passes. On a failure, the state is `BLOCKED` with the reason; onboarding can be interrupted and resumed idempotently at the first phase that does not pass.

## Methodology installation (gated)

Run only after access to `erikni/sdlc-ai` is confirmed (`gh repo view erikni/sdlc-ai`). Without access: `BLOCKED` — the human asks the methodology owner for access; do not proceed.

With access, in a dedicated branch and the latest released methodology version, create the initialization issue `[TASK] Project initialization` and run (adjust document set as the methodology version defines):

```bash
VERSION=<tag>            # latest released methodology tag
OWNER=<account>; REPO=<project>
git checkout -b feature/<issue-id>-initialization
git clone --depth 1 --branch "$VERSION" https://github.com/erikni/sdlc-ai ../sdlc-ai-src
SRC=../sdlc-ai-src
cp "$SRC"/AGENTS.md ./AGENTS.md
cp "$SRC"/{TASK,ISSUE,GIT,TEST,SECURITY,PROJECT,RELEASE,DEPLOY,DEPLOY-CF,DEPLOY-DOCKER}.en.md .
cp -R "$SRC"/roles "$SRC"/artifacts "$SRC"/skills .
sed -i.bak -e "s/^GITHUB_OWNER=.*/GITHUB_OWNER=$OWNER/" -e "s/^REPOSITORY_NAME=.*/REPOSITORY_NAME=$REPO/" AGENTS.md && rm AGENTS.md.bak
printf '\nMethodology: [SDLC.AI](https://github.com/erikni/sdlc-ai) %s\n' "$VERSION" >> README.md
```

Then add the shared CI / preview / production workflows as **thin callers** (the logic stays centralized in this public repository, so later updates propagate via the moving tag `@v1` without rewriting the project's files):

```bash
mkdir -p .github/workflows
git clone --depth 1 https://github.com/erikni/sdlc-ai-init ../sdlc-ai-init-src
cp ../sdlc-ai-init-src/templates/project-workflows/{ci,preview,production}.yml .github/workflows/
# Fill in each caller's variables (deploy_provider, site_url, …) from the init instruction.
```

Fill `LANG`, website, and deployment in `AGENTS.md` from the instruction, then verify:

| Check | Command | On failure |
|---|---|---|
| write access to the project | `gh api "repos/$OWNER/$REPO" --jq .permissions` | `BLOCKED` (`PERMISSION`) |
| main branch is protected | `gh api "repos/$OWNER/$REPO/rules/branches/main" --jq '[.[].type]'` | report and point to README step 2 |
| `AGENTS.md` contains no secrets | review | fix in the PR |
| document links and headers | the methodology's `skills/docs-check` if present | fix `FAIL`, state `NOT RUN` with reason |
| chosen deployment has its configuration | per the methodology's `DEPLOY` document | `BLOCKED` (`DEPLOY`) |

Open a PR and request approval with a **"What you are approving"** section (link to the PR, a **Diff** link, and the list of changed files). Do not create the repository, change permissions, branch protection, or secrets, and deploy nothing. After `SCHVÁLIT PR`, merge per the methodology's `TASK` document. You need `git`, `gh`, and Python 3.9+.

## Shared workflows

This repository publishes reusable workflows under `.github/workflows/` (`ci.yml`, `preview.yml`, `production.yml`, each `on: workflow_call`). Projects reference them from the thin callers in `templates/project-workflows/` via `uses: erikni/sdlc-ai-init/.github/workflows/<name>.yml@v1`. Callers carry only variables and secret names; never hardcode secrets. An empty `DEPLOY_PROVIDER` makes preview and production a valid no-op (`N/A`); CI still runs.
