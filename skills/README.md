# Skills

Skills for the public SDLC.AI bootstrap. A skill is loaded on the user's explicit instruction or when the workflow reaches the stated step; until then it is not read. The binding rules are in the root [AGENTS.md](../AGENTS.md).

| Skill | Triggers | Does not trigger |
|---|---|---|
| [ONBOARDING.en.md](ONBOARDING.en.md) | the instruction "onboarding", "guide me through the setup", or "start a new project with SDLC.AI" | a mention in another context; a project that already contains the methodology |

The full methodology (and its other skills) lives in the private `erikni/sdlc-ai` repository and is installed into the project only with granted access, per [AGENTS.md](../AGENTS.md). Without access the agent reports `BLOCKED` and does not invent the rules.
