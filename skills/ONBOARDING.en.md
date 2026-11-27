# ONBOARDING – initial setup guide

**Version:** 3.0.0 · **Type:** skill triggered on instruction

Executable procedure for an AI agent that runs on the user's computer (for example Claude Code or OpenAI Codex) and leads a human step by step through the setup. The human need not be a programmer. The binding rules and the public/private boundary are in the public [AGENTS.md](../AGENTS.md); the human-facing click steps are in [README.md](../README.md). This skill holds the detailed phases and verification commands and does not duplicate those normative instructions.

The authoritative diagram of the whole flow, with every branch, is [docs/onboarding-flow.md](../docs/onboarding-flow.md) (source [onboarding-flow.mmd](../docs/onboarding-flow.mmd)).

## When it runs

On the instruction "onboarding", "guide me through the setup", or "start a new project with SDLC.AI". It is not triggered by a mention of onboarding in another context or by work in a project that already contains the methodology.

## Guidance principles

Follow the binding rules in [AGENTS.md](../AGENTS.md) (least privilege, secrets never in the conversation, explicit consent before any change, verify don't trust, stop on `BLOCKED`, never a false `PASS`, no self-approval or self-merge). In addition:

- **Assume nothing.** Derive only `GITHUB_OWNER` / `REPOSITORY_NAME` from the GitHub URL (and the preview pattern from the Cloudflare type) and have the human confirm; ask about everything else. Because a private repository cannot be checked reliably, ask the human YES/NO whether it exists.
- **One step at a time.** Say what will be done now and why, in one or two sentences without jargon; explain a new term at first use. Start the next step only once the previous one is verified.
- **Offer who will perform the step.** Offer to do by a command whatever can be done by a command; otherwise guide the human by clicking per [README.md](../README.md).
- **The human decides.** The project name, visibility, the agent's access method, and deployment are chosen by them; you explain the options and recommend one.
- **Show the status continuously:** a short list of phases marked done, in progress, remaining.

Speak the human's language; after the methodology is linked, `LANG` from the project's `AGENTS.md` applies.

## Phases

The meeting with Erik and the variant choice happen before you act. At the start, determine the operating system, the tool you run in, the chosen variant, and whether the human already has a GitHub account. The variables `OWNER` and `REPO` are derived and confirmed in phase 4. The click steps referenced below are in [README.md](../README.md).

| # | Phase | Who | Verification |
|---|---|---|---|
| 1 | Meeting with Erik + variant (own development / outsourcing); for own development, READ access to `erikni/sdlc-ai` requested | the human | the human confirms the variant; for own development `gh repo view erikni/sdlc-ai` later in phase 8 |
| 2 | Agent on the computer and tools: `git`, GitHub CLI `gh`, Python 3.9+ | the agent determines; installation by the human, or the agent with consent | `git --version`, `gh --version`, `python3 --version` |
| 3 | GitHub account and `gh` sign-in | the human | `gh auth status`, `gh api user --jq .login` |
| 4 | Configuration interview → one `env` block confirmed by the human | the agent asks; the human confirms | the human confirms the `env` block; `OWNER`/`REPO` match the URL |
| 5 | Repository setup in the browser (README steps 1–3): protection, Issues, merging | the agent in Chrome with consent, or the human by clicking | `gh api "repos/$OWNER/$REPO/rules/branches/main" --jq '[.[].type]'`; `gh repo view "$OWNER/$REPO" --json hasIssuesEnabled,mergeCommitAllowed,deleteBranchOnMerge` |
| 6 | Agent access for regular work — offer the choice (README steps 4–5) | the human | `gh auth status` |
| 7 | Project folder on the computer | the agent with consent | `git -C <folder> remote get-url origin` |
| 8 | Methodology installation (gated, see AGENTS.md) | the agent; the merge is approved by the human | the merged PR; the methodology's own `docs-check` if present |
| 9 | Deployment (optional) | the human stores credentials; the agent waits and verifies | per the methodology's `DEPLOY` document; the preview / `SITE_URL` after the run |
| 10 | Summary and first assignment | the agent | – |

### Phase details

1. **Meeting and variant.** Onboarding starts with a meeting with Erik (<https://calendly.com/erik-brozek>), where the human picks the variant. Confirm which variant applies. For **own development**, the human requests READ access to `erikni/sdlc-ai`; note that phase 8 is `BLOCKED` until it is granted. For **outsourcing**, Erik or his team develop and the client does not run the rest of this bootstrap.
2. **Agent and tools.** The agent runs on the computer (Claude Code or Codex). If any tool is missing, give a link to the official installation (<https://git-scm.com/downloads>, <https://cli.github.com/>, <https://www.python.org/downloads/>) and offer installation by the system package manager. Install nothing without consent. On Windows, Python may be available as `python` or `py`. Optionally recommend GitHub Desktop and GitHub Mobile (approving a PR from the phone).
3. **Sign-in.** If the human has no account, send them to <https://github.com/signup>; do not create the account for them. Then ask them to run `gh auth login` in their own terminal window and choose **GitHub.com**, **HTTPS**, and **Login with a web browser**; they copy the one-time code into the browser themselves.
4. **Configuration interview.** Ask for the project's GitHub URL (`https://github.com/<owner>/<repo>`) and from it derive `GITHUB_OWNER` and `REPOSITORY_NAME`; have the human confirm both. Ask **YES/NO** whether the repository already exists; on NO, guide them to create it at <https://github.com/new> with **Add a README** checked (this creates the `main` branch) per [README.md](../README.md). Then ask `LANG`, `DEPLOY_PROVIDER` (`CLOUDFLARE` / `DOCKER` / none), and for Cloudflare the type (**PAGES** / **WORKERS**), from which you derive `PREVIEW_URL_PATTERN` (`*-<REPO>.sdlc-ai.workers.dev` for Workers, `*.<REPO>.pages.dev` for Pages); for Docker ask `DOCKERFILE_PATH` and `PORT`. Ask `SITE_URL` (empty if none) and `GATE_APPROVAL_MODE` (how the merge is approved; the default is a click in the chat with `SCHVÁLIT PR`). Show the whole `env` block and let the human confirm it before any change.
5. **Repository setup in the browser.** The one-time settings — main branch protection, Issues, and merging (README steps 1–3) — are done in the browser. Automated clicking works only in **Chrome** with the agent's extension; on a Safari-default machine, offer to install Chrome or guide the human by **manual clicking**. Explain that without protection a change could reach the main branch without approval. Expected results: `["deletion","non_fast_forward","pull_request"]`, and `hasIssuesEnabled` and `mergeCommitAllowed` are `true`. If protection fails due to the plan, offer a public repository, a paid plan, or continuing without protection with an explicit warning that the rule is then enforced only by the methodology.
6. **Access for regular work.** Offer the human the choice and explain the trade-off; recommend the restricted key:

   | Option | What it means |
   |---|---|
   | Restricted access key (recommended) | the agent may work only in this repository and cannot change its settings. The human creates the key by clicking per [README.md](../README.md) step 4 and in their own terminal window runs `gh auth login --with-token`, pastes the key, and confirms. `gh auth status` then shows a token starting with `github_pat_`. |
   | GitHub Desktop (easiest) | the human signs in once in GitHub Desktop; `git push`/`pull` then work without a key. Simpler for a non-programmer, but the agent then has the human's full account rights across all their repositories — a conscious trade-off against least privilege. |

   Record the choice in the summary. Never take the key into the conversation.
7. **Project folder.** Ask where to store the project and, with consent, run `gh repo clone "$OWNER/$REPO" <folder>`. Tell the human to open the tool in this folder next time; the `AGENTS.md` in its root will then load itself.
8. **Installation (gated).** This installs the **private** methodology and is governed by [AGENTS.md](../AGENTS.md) ("Methodology installation"). First verify access with `gh repo view erikni/sdlc-ai`; **without access the state is `BLOCKED`** and the human must ask the methodology owner for it — do not read or bypass the private repository. With access, perform the installation steps from `AGENTS.md` with the latest released version: the methodology is **linked, not copied** — materialize only `AGENTS.md` (project-filled) and the note in `README.md`, add the shared CI/preview/production caller workflows, open a PR, and request approval with the "What you are approving" section. Explain that the **Diff** link shows exactly what changes and that the reply `SCHVÁLIT PR` includes the change in the main branch. After approval, merge per the installed methodology's `TASK` document.
9. **Deployment.** Ask whether the project is to run on the web. If yes, explain what is needed per [README.md](../README.md) and let the human store the provider's access credentials. After a merge that deploys, wait for the run to finish — at most 5 minutes — then verify the preview / `SITE_URL`; if it does not finish or fails, report `BLOCKED`. If the project is not deployed, leave `DEPLOY_PROVIDER` empty.
10. **Summary.** State the repository address, the chosen variant, the folder on the computer, the chosen access method, the branch protection status, the methodology version, the preview / `SITE_URL` if deployed, and what remained unresolved. Offer a first assignment and explain the difference between `TASK`, `STORY`, and `EPIC` in one sentence each.

## Interruption and continuation

Onboarding can be interrupted at any time. On continuation, do not start over: run the verification commands from phase 2 and resume at the first phase that does not pass. Once the repository and an initialization issue exist, record the status into that issue in one sentence.
