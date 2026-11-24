# ONBOARDING – initial setup guide

**Version:** 2.0.0 · **Type:** skill triggered on instruction

Executable procedure for an AI agent that runs on the user's computer (for example Claude Code or OpenAI Codex) and leads a human step by step through the setup. The human need not be a programmer. The binding rules and the public/private boundary are in the public [AGENTS.md](../AGENTS.md); the human-facing click steps are in [README.md](../README.md). This skill holds the detailed phases and verification commands and does not duplicate those normative instructions.

## When it runs

On the instruction "onboarding", "guide me through the setup", or "start a new project with SDLC.AI". It is not triggered by a mention of onboarding in another context or by work in a project that already contains the methodology.

## Guidance principles

Follow the binding rules in [AGENTS.md](../AGENTS.md) (least privilege, secrets never in the conversation, explicit consent before any change, verify don't trust, stop on `BLOCKED`, never a false `PASS`, no self-approval or self-merge). In addition:

- **One step at a time.** Say what will be done now and why, in one or two sentences without jargon; explain a new term at first use. Start the next step only once the previous one is verified.
- **Offer who will perform the step.** Offer to do by a command whatever can be done by a command; otherwise guide the human by clicking per [README.md](../README.md).
- **The human decides.** The project name, visibility, the agent's access method, and deployment are chosen by them; you explain the options and recommend one.
- **Show the status continuously:** a short list of phases marked done, in progress, remaining.

Speak the human's language; after the methodology is copied, `LANG` from the project's `AGENTS.md` applies.

## Phases

At the start, determine the operating system, the tool you run in, and whether the human already has a GitHub account. The variables `OWNER` and `REPO` mean the account and project name the human chooses in phase 3. The click steps referenced below are in [README.md](../README.md).

| # | Phase | Who | Verification |
|---|---|---|---|
| 1 | Tools on the computer: `git`, GitHub CLI `gh`, Python 3.9+ | the agent determines; installation by the human, or the agent with consent | `git --version`, `gh --version`, `python3 --version` |
| 2 | GitHub account and `gh` sign-in | the human | `gh auth status`, `gh api user --jq .login` |
| 3 | Repository (README step 1) | the agent with consent, or the human by clicking | `gh repo view "$OWNER/$REPO" --json defaultBranchRef,visibility` |
| 4 | Main branch protection (README step 2) | the agent with consent, or the human by clicking | `gh api "repos/$OWNER/$REPO/rules/branches/main" --jq '[.[].type]'` |
| 5 | Merging (README step 3) | the agent with consent, or the human by clicking | `gh repo view "$OWNER/$REPO" --json hasIssuesEnabled,mergeCommitAllowed,deleteBranchOnMerge` |
| 6 | Agent access for regular work (README steps 4 and 5) | the human | `gh auth status` |
| 7 | Project folder on the computer | the agent with consent | `git -C <folder> remote get-url origin` |
| 8 | Methodology initialization (gated, see AGENTS.md) | the agent; the merge is approved by the human | the merged PR; the methodology's own `docs-check` if present |
| 9 | Deployment (optional) | the human; the agent only explains | per the methodology's `DEPLOY` document |
| 10 | Summary and first assignment | the agent | – |

### Phase details

1. **Tools.** If any is missing, give a link to the official installation (<https://git-scm.com/downloads>, <https://cli.github.com/>, <https://www.python.org/downloads/>) and offer installation by the system package manager. Install nothing without consent. On Windows, Python may be available as `python` or `py`.
2. **Sign-in.** If the human has no account, send them to <https://github.com/signup>; do not create the account for them. Then ask them to run `gh auth login` in their own terminal window and choose **GitHub.com**, **HTTPS**, and **Login with a web browser**; they copy the one-time code into the browser themselves.
3. **Repository.** Ask for the name (without spaces or diacritics) and visibility. Point out that for a private repository the protection from phase 4 requires a paid GitHub plan. With consent, run the commands from the "For advanced users" section in [README.md](../README.md), or guide by clicking. The repository must have a `main` branch with an initial README.
4. **Protection.** Explain that without it a change could reach the main branch without approval. The expected verification result is `["deletion","non_fast_forward","pull_request"]`. If creation fails due to the plan, offer a public repository, a paid plan, or continuing without protection with an explicit warning that the rule is then enforced only by the methodology.
5. **Merging.** Expected result: `hasIssuesEnabled` and `mergeCommitAllowed` are `true`.
6. **Access for regular work.** Explain both options and recommend the first:

   | Option | What it means |
   |---|---|
   | Restricted access key (recommended) | the agent may work only in this repository and cannot change its settings. The human creates the key by clicking per [README.md](../README.md) step 4 and in their own terminal window runs `gh auth login --with-token`, pastes the key, and confirms. `gh auth status` then shows a token starting with `github_pat_`. |
   | Keep the sign-in from phase 2 | simpler, but the agent has the same rights as the human across all their repositories including changing settings. The methodology rules forbid this, but nothing technically prevents it. |

   Record the choice in the summary. Never take the key into the conversation.
7. **Project folder.** Ask where to store the project and, with consent, run `gh repo clone "$OWNER/$REPO" <folder>`. Tell the human to open the tool in this folder next time; the `AGENTS.md` in its root will then load itself.
8. **Initialization (gated).** This installs the **private** methodology and is governed by [AGENTS.md](../AGENTS.md) ("Methodology installation"). First verify access with `gh repo view erikni/sdlc-ai`; **without access the state is `BLOCKED`** and the human must ask the methodology owner for it — do not read or bypass the private repository. With access, perform the installation steps from `AGENTS.md` with the latest released version, add the shared CI/preview/production caller workflows, open a PR, and request approval with the "What you are approving" section. Explain that the **Diff** link shows exactly what changes and that the reply `SCHVÁLIT PR` includes the change in the main branch. After approval, merge per the installed methodology's `TASK` document.
9. **Deployment.** Ask whether the project is to run on the web. If yes, explain what is needed per [README.md](../README.md) and let the human store the provider's access credentials. If not, leave `DEPLOY_PROVIDER` empty.
10. **Summary.** State the repository address, the folder on the computer, the chosen access method, the branch protection status, the methodology version, and what remained unresolved. Offer a first assignment and explain the difference between `TASK`, `STORY`, and `EPIC` in one sentence each.

## Interruption and continuation

Onboarding can be interrupted at any time. On continuation, do not start over: run the verification commands from phase 1 and resume at the first phase that does not pass. Once the repository and an initialization issue exist, record the status into that issue in one sentence.
