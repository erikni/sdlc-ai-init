# ONBOARDING – průvodce úvodním nastavením

**Verze:** 2.0.0 · **Typ:** skill spouštěný na pokyn

Vykonatelný postup pro AI agenta, který běží na počítači uživatele (například Claude Code nebo OpenAI Codex) a provede člověka krok za krokem nastavením. Člověk nemusí být programátor. Závazná pravidla a hranice public/private jsou ve veřejném [AGENTS.md](../AGENTS.md); lidské kroky klikáním jsou v [README.md](../README.md). Tento skill obsahuje detailní fáze a ověřovací příkazy a tyto normativní instrukce nedubluje.

## Kdy se spouští

Na pokyn „onboarding“, „proveď mě nastavením“ nebo „začni nový projekt se SDLC.AI“. Nespouští ho zmínka o onboardingu v jiném kontextu ani práce v projektu, který už metodiku obsahuje.

## Zásady vedení

Dodržuj závazná pravidla z [AGENTS.md](../AGENTS.md) (nejmenší nutná práva, tajné údaje nikdy v konverzaci, výslovný souhlas před každou změnou, ověřuj místo důvěry, zastav se na `BLOCKED`, nikdy falešné `PASS`, žádné schválení ani merge vlastní změny). Navíc:

- **Jeden krok po druhém.** Řekni, co se teď udělá a proč, v jedné dvou větách bez žargonu; nový pojem vysvětli při prvním použití. Další krok začni až po ověření předchozího.
- **Nabídni, kdo krok provede.** Co jde udělat příkazem, nabídni udělat sám; jinak veď člověka klikáním podle [README.md](../README.md).
- **Člověk rozhoduje.** Název projektu, viditelnost, způsob přístupu agenta a nasazení volí on; ty vysvětli možnosti a jednu doporuč.
- **Průběžně ukazuj stav:** krátký seznam fází hotovo / probíhá / zbývá.

Mluv jazykem člověka; po zkopírování metodiky platí `LANG` z `AGENTS.md` projektu.

## Fáze

Na začátku zjisti operační systém, nástroj, ve kterém běžíš, a zda už má člověk účet na GitHubu. Proměnné `OWNER` a `REPO` znamenají účet a název projektu, které člověk zvolí ve fázi 3. Kroky klikáním níže jsou v [README.md](../README.md).

| # | Fáze | Kdo | Ověření |
|---|---|---|---|
| 1 | Nástroje na počítači: `git`, GitHub CLI `gh`, Python 3.9+ | agent zjistí; instalace člověkem, nebo agentem se souhlasem | `git --version`, `gh --version`, `python3 --version` |
| 2 | Účet GitHub a přihlášení `gh` | člověk | `gh auth status`, `gh api user --jq .login` |
| 3 | Repozitář (README krok 1) | agent se souhlasem, nebo člověk klikáním | `gh repo view "$OWNER/$REPO" --json defaultBranchRef,visibility` |
| 4 | Ochrana větve main (README krok 2) | agent se souhlasem, nebo člověk klikáním | `gh api "repos/$OWNER/$REPO/rules/branches/main" --jq '[.[].type]'` |
| 5 | Slučování (README krok 3) | agent se souhlasem, nebo člověk klikáním | `gh repo view "$OWNER/$REPO" --json hasIssuesEnabled,mergeCommitAllowed,deleteBranchOnMerge` |
| 6 | Přístup agenta pro běžnou práci (README kroky 4 a 5) | člověk | `gh auth status` |
| 7 | Složka projektu na počítači | agent se souhlasem | `git -C <složka> remote get-url origin` |
| 8 | Inicializace metodiky (gated, viz AGENTS.md) | agent; merge schvaluje člověk | sloučený PR; `docs-check` metodiky, pokud existuje |
| 9 | Nasazení (volitelné) | člověk; agent jen vysvětlí | podle dokumentu `DEPLOY` metodiky |
| 10 | Shrnutí a první zadání | agent | – |

### Detaily fází

1. **Nástroje.** Pokud některý chybí, dej odkaz na oficiální instalaci (<https://git-scm.com/downloads>, <https://cli.github.com/>, <https://www.python.org/downloads/>) a nabídni instalaci správcem balíčků. Nic neinstaluj bez souhlasu. Na Windows může být Python jako `python` nebo `py`.
2. **Přihlášení.** Pokud člověk nemá účet, pošli ho na <https://github.com/signup>; účet mu nezakládej. Pak ho požádej, ať ve vlastním okně terminálu spustí `gh auth login` a zvolí **GitHub.com**, **HTTPS** a **Login with a web browser**; jednorázový kód si do prohlížeče zkopíruje sám.
3. **Repozitář.** Zeptej se na název (bez mezer a diakritiky) a viditelnost. Upozorni, že u soukromého repozitáře vyžaduje ochrana z fáze 4 placený tarif GitHubu. Se souhlasem spusť příkazy z části „Pro pokročilé“ v [README.md](../README.md), nebo veď klikáním. Repozitář musí mít větev `main` s úvodním README.
4. **Ochrana.** Vysvětli, že bez ní by změna mohla do hlavní větve bez schválení. Očekávaný výsledek ověření je `["deletion","non_fast_forward","pull_request"]`. Pokud vytvoření selže kvůli tarifu, nabídni veřejný repozitář, placený tarif, nebo pokračování bez ochrany s výslovným upozorněním, že pravidlo pak vynucuje jen metodika.
5. **Slučování.** Očekávaný výsledek: `hasIssuesEnabled` a `mergeCommitAllowed` jsou `true`.
6. **Přístup pro běžnou práci.** Vysvětli obě možnosti a doporuč první:

   | Možnost | Co znamená |
   |---|---|
   | Omezený přístupový klíč (doporučeno) | agent smí pracovat jen v tomto repozitáři a nemůže měnit jeho nastavení. Člověk klíč vytvoří klikáním podle [README.md](../README.md) kroku 4 a ve vlastním okně terminálu spustí `gh auth login --with-token`, klíč vloží a potvrdí. `gh auth status` pak ukáže token začínající `github_pat_`. |
   | Ponechat přihlášení z fáze 2 | jednodušší, ale agent má stejná práva jako člověk ve všech jeho repozitářích včetně změny nastavení. Pravidla metodiky to zakazují, technicky tomu ale nic nebrání. |

   Volbu zaznamenej do shrnutí. Klíč nikdy neber do konverzace.
7. **Složka projektu.** Zeptej se, kam projekt uložit, a se souhlasem spusť `gh repo clone "$OWNER/$REPO" <složka>`. Řekni člověku, ať příště otevře nástroj v této složce; `AGENTS.md` v jejím kořeni se pak načte sám.
8. **Inicializace (gated).** Instaluje **privátní** metodiku a řídí se [AGENTS.md](../AGENTS.md) („Instalace metodiky“). Nejdřív ověř přístup příkazem `gh repo view erikni/sdlc-ai`; **bez přístupu je stav `BLOCKED`** a člověk o něj musí požádat vlastníka metodiky — privátní repozitář nečti ani neobcházej. S přístupem proveď instalační kroky z `AGENTS.md` s nejnovější vydanou verzí, přidej sdílené caller workflow (CI/preview/production), založ PR a požádej o schválení sekcí „Co schvalujete“. Vysvětli, že odkaz **Diff** ukazuje přesně, co se mění, a že odpověď `SCHVÁLIT PR` změnu zařadí do hlavní větve. Po schválení sluč podle dokumentu `TASK` nainstalované metodiky.
9. **Nasazení.** Zeptej se, zda má projekt běžet na webu. Pokud ano, vysvětli, co je potřeba podle [README.md](../README.md), a přístupové údaje poskytovatele nech ukládat člověka. Pokud ne, nech `DEPLOY_PROVIDER` prázdný.
10. **Shrnutí.** Uveď adresu repozitáře, složku na počítači, zvolený způsob přístupu, stav ochrany větve, verzi metodiky a co zůstalo nevyřešené. Nabídni první zadání a vysvětli rozdíl mezi `TASK`, `STORY` a `EPIC` jednou větou u každého.

## Přerušení a pokračování

Onboarding lze kdykoli přerušit. Při pokračování nezačínej znovu: spusť ověřovací příkazy od fáze 1 a navaž první fází, která neprojde. Jakmile existuje repozitář a issue inicializace, zapisuj stav do tohoto issue jednou větou.
