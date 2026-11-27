# Start a new project with SDLC.AI

A guide for non-programmers. The AI agent runs on your computer, for example in Claude Code or OpenAI Codex, and works with GitHub on your behalf. Setup takes about 20 minutes and you do it only once. After that you assign work in plain language and just approve the results.

This repository (`erikni/sdlc-ai-init`) is the **public bootstrap**: it is enough to start the setup. The full SDLC.AI methodology lives in the separate private repository `erikni/sdlc-ai` and is linked into your project only if you have been granted access to it (see [What happens next](#what-happens-next)).

- **For you (a human):** this `README.md`.
- **For your agent:** [AGENTS.md](AGENTS.md) — the binding bootstrap workflow.

## Before you start: a meeting and your variant

Onboarding starts with a short meeting with Erik — book it at [calendly.com/erik-brozek](https://calendly.com/erik-brozek). There you choose the SDLC.AI variant:

- **Own development** — you build the project together with the agent on your computer. You ask Erik for **READ access** to the methodology repository `erikni/sdlc-ai`; without it the agent cannot install the methodology. This guide is for you.
- **Outsourcing** — Erik or his team handle the development. You do not need access to the methodology and do not run this setup yourself.

**What you will need (own development)**

- an account on [github.com](https://github.com) (free);
- an AI agent tool installed on your computer;
- READ access to `erikni/sdlc-ai`, requested at the meeting.

## Fastest path: let the agent guide you

Open your agent tool and copy this instruction to it:

```text
Guide me through the initial setup of a new project per skills/ONBOARDING.en.md
from the erikni/sdlc-ai-init repository. I am not a programmer, lead me step by step.
```

The agent will guide you through everything described below: it checks what is missing on the computer, asks you for the project's GitHub address and a few settings (it assumes nothing — it only derives the owner and repository name from the address and has you confirm them), tells you where to click, and verifies each step itself. Never write passwords or access keys into the conversation; you enter them only yourself, in the browser or your own terminal window.

The rest of this guide describes the same steps in case you want to go through them yourself. Steps 1–4 are on the GitHub website, step 5 on your computer, and from step 6 the agent works.

**Glossary**

| Term | What it is |
|---|---|
| Repository | your project's folder on GitHub; keeps all files and their history |
| Main branch (`main`) | the valid version of the project |
| Pull request (PR) | a proposed change that waits for your approval before it reaches the main branch |
| Issue | a specification or task |
| Token | an access key the agent uses to work in your repository |

Button names are given in English as GitHub shows them. GitHub occasionally adjusts the interface; if a name differs slightly, look for the nearest similar one.

## Step 1 – Create the repository

If the repository already exists, skip to step 2. The agent will ask you YES/NO whether it exists, because for a private repository it cannot check reliably.

1. Open <https://github.com/new>.
2. **Owner:** select your account.
3. **Repository name:** write a project name without spaces or diacritics, for example `my-project`.
4. Choose visibility: **Public** (anyone can see it) or **Private** (only you and invitees).
5. Enable adding a README file (**Add a README**). This matters: it creates the `main` branch, without which the agent cannot start.
6. Click **Create repository**.

✅ Done when you see the repository page with a `README.md` file and the `main` branch at the top left.

> For a private repository (**Private**), the protection from step 2 requires a paid GitHub plan (Pro or Team). For a public one it is free.

## Step 2 – Enable main branch protection

Protection ensures nothing reaches the main branch except through a proposed change (pull request) that you approve. It applies to the agent too.

1. On the repository page, click **Settings** at the top.
2. In the left menu, click **Rules** and then **Rulesets**.
3. Click **New ruleset** and choose **New branch ruleset**.
4. **Ruleset name:** write `main`.
5. **Enforcement status:** set to **Active**.
6. **Bypass list:** leave empty. No one, not even the agent, has an exception.
7. **Target branches:** click **Add target** and choose **Include default branch**.
8. In the rules section, check:
   - **Restrict deletions** – the branch cannot be deleted;
   - **Require a pull request before merging** – every change goes through a proposal. Leave **Required approvals** at `0` and check **Dismiss stale pull request approvals when new commits are pushed**;
   - **Block force pushes** – history cannot be rewritten.
9. Click **Create** at the bottom.

✅ Done when the Rulesets list has a `main` entry with the **Active** status.

## Step 3 – Set up merging

1. In **Settings**, click **General** in the left menu.
2. In the **Features** section, confirm **Issues** is checked.
3. Scroll down to the **Pull Requests** section and confirm **Allow merge commits** is checked.
4. In the same place, check **Automatically delete head branches**. Working branches will then clean up after approval.

> Steps 1–3 are the one-time repository setup. The agent can click them for you in **Chrome** with its extension (Claude / Codex); on a Mac whose default browser is Safari, it cannot automate them, so install Chrome or do these steps by hand.

## Step 4 – Choose how the agent accesses your repository

The agent needs to sign in to work in your repository. Pick the option that suits you; the agent offers both and recommends the first.

| Option | What it means |
|---|---|
| **Restricted access key (recommended)** | a key that works for only one repository and only for the activities you permit. The agent cannot change settings, delete the repository, or touch your other projects. Create it as in the steps below. |
| **GitHub Desktop (easiest)** | you sign in once in the [GitHub Desktop](https://desktop.github.com/download) app and the agent works through it without a key. Simpler, but the agent then has your full account rights across all your repositories — a conscious trade-off for convenience. |

If you choose GitHub Desktop, install it, sign in (**Settings → Accounts → Sign in**), and continue at step 5 (you can skip creating a key). Otherwise create the restricted key:

1. Click your profile picture at the top right and choose **Settings**.
2. In the left menu, at the very bottom, click **Developer settings**.
3. Click **Personal access tokens** and then **Fine-grained tokens**.
4. Click **Generate new token**.
5. **Token name:** write for example `sdlc-ai-my-project`.
6. **Expiration:** choose a validity period, for example 90 days. After it expires, create a new one the same way.
7. **Resource owner:** select your account.
8. **Repository access:** choose **Only select repositories** and select the repository from step 1.
9. **Permissions → Repository permissions:** set exactly these three items, leave the rest alone:

   | Item | Setting | Why the agent needs it |
   |---|---|---|
   | Contents | Read and write | saving file changes and publishing versions |
   | Issues | Read and write | creating and closing tasks |
   | Pull requests | Read and write | proposing changes and merging them after your approval |

10. Click **Generate token** and copy the key immediately. GitHub shows it only once.

**Key security**

- Save it to a password manager. Do not send it by email or message and do not put it into the repository, a task, or the conversation with the agent.
- You can revoke it at any time on the same page (**Fine-grained tokens**); the agent immediately loses access.
- Because the key belongs to your account, everything the agent does will be listed on GitHub under your name.

## Step 5 – Prepare the agent on your computer

The agent works with GitHub through the GitHub CLI (`gh`) program, which runs on your computer.

1. Install [Git](https://git-scm.com/downloads), [GitHub CLI](https://cli.github.com/), and [Python](https://www.python.org/downloads/). For each, download the installer for your system and click through it with the default options.
2. Open a terminal window: on Mac the **Terminal** app, on Windows **PowerShell**.
3. Sign the agent in. With the **restricted key** from step 4, type the command, press Enter, paste the key, Enter again, and then Ctrl+D (on Windows Ctrl+Z and Enter):

   ```bash
   gh auth login --with-token
   ```

   With **GitHub Desktop**, you are already signed in; for `gh` commands run `gh auth login --web` once.
4. Pick a folder for the project on your computer and open your agent tool in it.

✅ Done when the command `gh auth status` prints your account (with the restricted key, a token starting with `github_pat_`).

Enter the key only in the terminal window, never in the conversation with the agent.

## Step 6 – Give the agent the instruction

Copy this text into the conversation with the agent and fill in the values in angle brackets:

```text
Initialize a new SDLC.AI project per AGENTS.md from the erikni/sdlc-ai-init repository.
Repository: https://github.com/<your-account>/<project-name>
Methodology version: latest released
Communication language: English
Project website: <address, or "none yet">
Deployment: <"Cloudflare", "local Docker", or "none">
Changes are approved by: <your name>
```

The agent derives the owner and repository name from the address and has you confirm them, then asks for anything else it needs (for Cloudflare, whether it is Pages or Workers, so it can derive the preview address).

## What happens next

1. The agent verifies it has access to your repository. To install the methodology it also needs access to the private `erikni/sdlc-ai`. If you do not have that access, the agent stops and reports `BLOCKED` with an explanation — it never reads or bypasses the private repository. Ask the methodology owner for access.
2. With access, it **links** the methodology into your project: only `AGENTS.md` (filled in for your project) and a note in `README.md` are added; every other methodology document stays in `erikni/sdlc-ai` and is referenced by link, so later updates reach you without rewriting files. It also adds the shared CI / preview / production workflows as thin caller files.
3. It checks that everything is ready: access, main branch protection, settings, and the links in the documents.
4. It sends you a request starting with the heading **"What you are approving"**. It contains a link to the proposed change, a **Diff** link (shows exactly what changes), and a list of changed files.
5. If you agree, reply `SCHVÁLIT PR`. If you want something different, write `DOPLNIT ZADÁNÍ` and what should change.

After approval the project is ready. When the agent hits an obstacle, it writes what blocks it and what you should do.

## How to assign work

Write to the agent what you want and start with the task size:

```text
[TASK] Fix the typo on the home page.
[STORY] Add email-and-password sign-in.
[EPIC] Customer portal – first version.
```

| Type | When to use it | What you will approve |
|---|---|---|
| `TASK` | a small, clear change | one "What you are approving" request |
| `HOTFIX` | a fix you want to users as soon as possible | one request for both inclusion and release, and then acceptance of the released version |
| `STORY` | a new feature | one request; the agent additionally documents tests and security |
| `EPIC` | a larger whole of several parts | step by step: intent, design, individual parts, and finally the release |

| The agent does itself | You |
|---|---|
| creates tasks, prepares changes and proposals | assign the work and choose its size |
| runs checks and reports results truthfully | approve every change before it is included in the main branch |
| after your approval, includes the change and closes the task | for `EPIC` and `HOTFIX` you also approve the release and its acceptance |
| reports obstacles and proposes solutions | manage the account, access key, and repository settings |

The agent never approves its own work and includes nothing in the main branch without your consent.

## Web deployment (optional)

If the project is to run on the internet, you additionally need an account with a provider (the methodology has a ready procedure for Cloudflare) and storing its access key among the repository secrets: **Settings → Secrets and variables → Actions → New repository secret**. The shared production workflow then reads it by name. This step is more technical; we recommend doing it with someone experienced. Without it, everything else works, only the result does not publish itself.

If you just want to run on your own computer without a provider account, set deployment to local Docker and the project will run in a local Docker container.

After a change is approved and merged, a deployment runs automatically. The agent waits for it to finish — at most 5 minutes — and then checks the preview or your website address. If it does not finish in time or fails, the agent reports it rather than guessing.

No webhooks or other integrations are required by the methodology.

## When something doesn't work

| What you see | What to do |
|---|---|
| The **Settings** tab is missing in the repository | you are not signed in with the account that owns the repository |
| The ruleset cannot be enabled or does not apply | the repository is **Private** on a free plan; change it to **Public** (Settings → General → at the very bottom **Change visibility**), or switch to a paid plan |
| The agent cannot click the setup in the browser | automation works only in Chrome with the agent's extension; install Chrome, or do steps 1–3 by hand |
| The agent reports it has no access | check the repository selected in step 4 and the three permissions; the key may also have expired |
| The agent reports it has no access to the methodology | the `erikni/sdlc-ai` repository is private; ask its owner for READ access. The public bootstrap alone lets you start, but the methodology installs only with access |
| The agent reports `BLOCKED` | it writes what blocks it and what unblocks it; usually a setting from steps 1–5, access to the methodology, or a deployment that did not finish |
| You accidentally pasted the key somewhere | revoke it immediately on the **Fine-grained tokens** page and create a new one |

## For advanced users

Technical details for those who want to do the same by commands or who work in a team.

<details>
<summary>Steps 1–3 by GitHub CLI commands</summary>

```bash
OWNER=your-account
REPO=project-name

gh repo create "$OWNER/$REPO" --public --add-readme
gh repo edit "$OWNER/$REPO" --enable-issues --enable-merge-commit --delete-branch-on-merge

gh api -X POST "repos/$OWNER/$REPO/rulesets" --input - <<'JSON'
{
  "name": "main",
  "target": "branch",
  "enforcement": "active",
  "bypass_actors": [],
  "conditions": { "ref_name": { "include": ["~DEFAULT_BRANCH"], "exclude": [] } },
  "rules": [
    { "type": "deletion" },
    { "type": "non_fast_forward" },
    { "type": "pull_request",
      "parameters": {
        "required_approving_review_count": 0,
        "dismiss_stale_reviews_on_push": true,
        "require_code_owner_review": false,
        "require_last_push_approval": false,
        "required_review_thread_resolution": false
      } }
  ]
}
JSON

gh api "repos/$OWNER/$REPO/rules/branches/main" --jq '[.[].type]'
```

The last command should return `["deletion","non_fast_forward","pull_request"]`.

</details>

<details>
<summary>A separate account for the agent (teams and organizations)</summary>

In a team it is appropriate for the agent to work under its own account: it is visible what the agent did and what a human did, and you can require approval from an account other than the one that proposed the change (**Required approvals** `1` in step 2).

- A restricted token (fine-grained) works only for repositories owned by the account itself or an organization it is a member of. The repository must therefore belong to an organization and the agent's account must be its member with the Write, not Admin, role.
- The token is then created under the agent's account the same way as in step 4, with the organization as the **Resource owner**.
- If you use classic branch protection instead of a ruleset, add the **Administration: Read-only** permission to the token, otherwise the agent will not load it and will stop merging.
- If the project has automated checks (CI), add **Commit statuses** and **Actions** as read-only.

</details>

The exact, agent-executable initialization procedure is in [AGENTS.md](AGENTS.md).
