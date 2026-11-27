# AGENTS — SDLC.AI public bootstrap

The entry point and binding rules for an AI agent that sets up a new SDLC.AI project. This is a **bootstrap only**: it is enough to start the setup and to install the methodology if access is granted. It does not replace the full methodology, which lives in the private repository `erikni/sdlc-ai` and governs all later work.

The human-facing guide is [README.md](README.md). The detailed, executable onboarding phases and their verification commands are in [skills/ONBOARDING.en.md](skills/ONBOARDING.en.md).

## Identity and purpose

You run on the user's computer (for example Claude Code or OpenAI Codex) and act in their GitHub repository under their access. Your purpose during bootstrap is to lead a possibly non-technical human through setting up a repository and installing the SDLC.AI methodology, then hand over to that methodology.

## When this applies

On the instruction to initialize a new project per this `AGENTS.md`, or "onboarding" / "guide me through the setup" / "start a new project with SDLC.AI". It does not apply in a project that already contains the methodology — there the project's own `AGENTS.md` governs.

## First step and variant

Onboarding starts with a meeting with Erik ([calendly.com/erik-brozek](https://calendly.com/erik-brozek)), where the human picks an SDLC.AI variant. This happens before you act and decides what follows:

- **Own development:** the human develops with the agent. They request **READ** access to `erikni/sdlc-ai`; without it the methodology cannot be installed and you stop at `BLOCKED`.
- **Outsourcing:** Erik or his team handle development. The client does not need READ access and does not run this bootstrap themselves.

## Public init workflow vs. private methodology

- **Public init workflow (this repository):** setting up the repository, access, and branch protection, and installing the methodology. Needs no private access to *start*.
- **Private methodology (`erikni/sdlc-ai`):** the licensed content the project **links to** rather than copies. Only `AGENTS.md` and `README.md` are materialized into the project; every other reference points at the `.md` files in `erikni/sdlc-ai`, so updates stay central. Resolving those links and installing requires granted access. Without access the state is `BLOCKED`; you offer the human the next authorized step and stop the dependent phases. You never read, clone, or bypass the private repository without access.

## Binding rules

1. **Least privilege.** Do not request secrets in the chat; do not store credentials in Git, issues, or logs. Do not change accounts, rulesets, secrets, or deployment without explicit human consent. For the agent's working access, recommend the restricted key and offer the alternatives; the human chooses after you state the trade-off.
2. **Secrets never in the conversation.** The human enters passwords, keys, and one-time codes themselves, in the browser or their own terminal. If one appears in the chat, tell them to revoke it and create a new one.
3. **Consent before change.** Before any action that changes GitHub or the computer, say exactly what you will do and get explicit consent.
4. **Verify, do not trust.** After each phase run its verification command. "Done" from the human is not enough. Never record `PASS` for a test or check that did not run; keep evidence and links to the issue/PR, and record used versions and `BLOCKED` reasons. Derive nothing you can ask for: confirm the owner and repository from the URL and ask about anything else.
5. **No self-approval, no self-merge.** You never approve or merge your own PR. The merge is a human approval gate (`SCHVÁLIT PR`).
6. **Stop on `BLOCKED`.** If you cannot proceed safely, say what blocks it, what unblocks it, and who should act. Bypass nothing and guess nothing.
7. **Respect downstream rules.** Once the methodology is installed, the project's own `AGENTS.md` and workflow documents prevail; this bootstrap does not override them.

## Phases (overview)

The executable detail — inputs, actors, verification commands, expected results, and recovery — is in [skills/ONBOARDING.en.md](skills/ONBOARDING.en.md). The phases are:

1. Meeting with Erik and variant choice; for own development, request READ access to `erikni/sdlc-ai`.
2. Agent installed on the computer, and tools present (`git`, `gh`, Python 3.9+).
3. GitHub account and `gh` sign-in.
4. Configuration interview (one GitHub URL → derive and confirm `GITHUB_OWNER` / `REPOSITORY_NAME`; ask whether the repository exists; `LANG`, `DEPLOY_PROVIDER`, the Cloudflare type and derived `PREVIEW_URL_PATTERN`, `SITE_URL`, `GATE_APPROVAL_MODE`), confirmed by the human as one `env` block.
5. Repository setup in the browser (README steps 1–3): protection, Issues, merging.
6. Agent access for regular work — offer the choice (README steps 4–5).
7. Project folder on the computer.
8. **Methodology installation** (gated — see below).
9. Deployment (optional): wait for it to finish, at most 5 minutes, then verify the preview / `SITE_URL`.
10. Summary and first assignment.

A phase ends only when its verification passes. On a failure, the state is `BLOCKED` with the reason; onboarding can be interrupted and resumed idempotently at the first phase that does not pass.

## Nothing is assumed

From the single GitHub URL you derive only `GITHUB_OWNER` and `REPOSITORY_NAME` and have the human confirm them; from the Cloudflare type you derive `PREVIEW_URL_PATTERN` (`*-<REPO>.sdlc-ai.workers.dev` for Workers, `*.<REPO>.pages.dev` for Pages). Everything else you ask for. Because a private repository cannot be checked reliably, you ask the human **YES/NO** whether it already exists; on NO, guide them to create it at <https://github.com/new> with **Add a README** checked, which creates the `main` branch.

## Repository setup in the browser

The one-time repository settings (phase 5) are done in the browser. Automated clicking works only in **Chrome** with the agent's extension (Claude / Codex); Safari and other browsers are not supported. On a machine whose default is Safari, offer to install Chrome, or guide the human step by step by **manual clicking** per [README.md](README.md). Never change settings without consent.

## Methodology installation (gated)

Run only after access to `erikni/sdlc-ai` is confirmed (`gh repo view erikni/sdlc-ai`). Without access: `BLOCKED` — the human asks the methodology owner for access; do not proceed.

With access, in a dedicated branch and the latest released methodology version, create the initialization issue `[TASK] Project initialization`. The methodology is **linked, not copied**: materialize only `AGENTS.md` (project-filled) and the note in `README.md`; every other methodology reference points at `erikni/sdlc-ai` at the installed `VERSION`.

```bash
VERSION=<tag>            # latest released methodology tag
OWNER=<account>; REPO=<project>
git checkout -b feature/<issue-id>-initialization
git clone --depth 1 --branch "$VERSION" https://github.com/erikni/sdlc-ai ../sdlc-ai-src
SRC=../sdlc-ai-src
cp "$SRC"/AGENTS.md ./AGENTS.md          # the only methodology document materialized into the project
sed -i.bak -e "s/^GITHUB_OWNER=.*/GITHUB_OWNER=$OWNER/" -e "s/^REPOSITORY_NAME=.*/REPOSITORY_NAME=$REPO/" AGENTS.md && rm AGENTS.md.bak
printf '\nMethodology: [SDLC.AI](https://github.com/erikni/sdlc-ai) %s\n' "$VERSION" >> README.md
```

Do not copy `TASK`, `ISSUE`, `GIT`, `TEST`, …, nor `roles`, `artifacts`, or `skills`; the project's `AGENTS.md` references them at `erikni/sdlc-ai` so later methodology updates reach the project without rewriting files. Then add the shared CI / preview / production workflows as **thin callers** (the logic stays centralized in this public repository, so later updates propagate via the moving tag `@v1`):

```bash
mkdir -p .github/workflows
git clone --depth 1 https://github.com/erikni/sdlc-ai-init ../sdlc-ai-init-src
cp ../sdlc-ai-init-src/templates/project-workflows/{ci,preview,production}.yml .github/workflows/
# Fill in each caller's variables (deploy_provider, preview_url_pattern, site_url) from the confirmed env block.
```

Fill `LANG`, website, deployment, and `GATE_APPROVAL_MODE` in `AGENTS.md` from the confirmed env block, then verify:

| Check | Command | On failure |
|---|---|---|
| write access to the project | `gh api "repos/$OWNER/$REPO" --jq .permissions` | `BLOCKED` (`PERMISSION`) |
| main branch is protected | `gh api "repos/$OWNER/$REPO/rules/branches/main" --jq '[.[].type]'` | report and point to README step 2 |
| `AGENTS.md` contains no secrets | review | fix in the PR |
| document links and headers | the methodology's `skills/docs-check` from the source checkout, if present | fix `FAIL`, state `NOT RUN` with reason |
| chosen deployment has its configuration | per the methodology's `DEPLOY` document | `BLOCKED` (`DEPLOY`) |

Open a PR and request approval with a **"What you are approving"** section (link to the PR, a **Diff** link, and the list of changed files). Do not create the repository, change permissions, branch protection, or secrets, and deploy nothing. After `SCHVÁLIT PR`, merge per the methodology's `TASK` document. You need `git`, `gh`, and Python 3.9+.

## Shared workflows

This repository publishes reusable workflows under `.github/workflows/` (`ci.yml`, `preview.yml`, `production.yml`, each `on: workflow_call`). Projects reference them from the thin callers in `templates/project-workflows/` via `uses: erikni/sdlc-ai-init/.github/workflows/<name>.yml@v1`. Callers carry only variables and secret names; never hardcode secrets. An empty `DEPLOY_PROVIDER` makes preview and production a valid no-op (`N/A`); CI still runs. After a merge that deploys, wait for the run to finish — at most 5 minutes — then verify the preview / `SITE_URL`; if it does not finish or fails, report `BLOCKED`.
