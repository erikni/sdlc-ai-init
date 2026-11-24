# Skilly

Skilly pro veřejný bootstrap SDLC.AI. Skill se načítá na výslovný pokyn uživatele nebo když na něj dojde workflow; do té doby se nečte. Závazná pravidla jsou v kořenovém [AGENTS.md](../AGENTS.md).

| Skill | Spouštěče | Nespouští |
|---|---|---|
| [ONBOARDING.md](ONBOARDING.cs.md) | pokyn „onboarding“, „proveď mě nastavením“ nebo „začni nový projekt se SDLC.AI“ | zmínka v jiném kontextu; projekt, který metodiku už obsahuje |

Úplná metodika (a její další skilly) je v privátním repozitáři `erikni/sdlc-ai` a do projektu se instaluje jen s uděleným přístupem podle [AGENTS.md](../AGENTS.md). Bez přístupu agent hlásí `BLOCKED` a pravidla si nevymýšlí.
