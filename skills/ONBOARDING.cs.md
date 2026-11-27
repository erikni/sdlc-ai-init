# ONBOARDING – průvodce úvodním nastavením

**Verze:** 3.0.0 · **Typ:** skill spouštěný na pokyn

Vykonatelný postup pro AI agenta, který běží na počítači uživatele (například Claude Code nebo OpenAI Codex) a provede člověka krok za krokem nastavením. Člověk nemusí být programátor. Závazná pravidla a hranice public/private jsou ve veřejném [AGENTS.md](../AGENTS.md); lidské kroky klikáním jsou v [README.md](../README.md). Tento skill obsahuje detailní fáze a ověřovací příkazy a tyto normativní instrukce nedubluje.

Autoritativní diagram celého flow se všemi větvemi je [docs/onboarding-flow.md](../docs/onboarding-flow.md) (zdroj [onboarding-flow.mmd](../docs/onboarding-flow.mmd)).

## Kdy se spouští

Na pokyn „onboarding“, „proveď mě nastavením“ nebo „začni nový projekt se SDLC.AI“. Nespouští ho zmínka o onboardingu v jiném kontextu ani práce v projektu, který už metodiku obsahuje.

## Zásady vedení

Dodržuj závazná pravidla z [AGENTS.md](../AGENTS.md) (nejmenší nutná práva, tajné údaje nikdy v konverzaci, výslovný souhlas před každou změnou, ověřuj místo důvěry, zastav se na `BLOCKED`, nikdy falešné `PASS`, žádné schválení ani merge vlastní změny). Navíc:

- **Nic nedomýšlej.** Z GitHub URL odvoď jen `GITHUB_OWNER` / `REPOSITORY_NAME` (a náhledový vzor z typu Cloudflare) a nech potvrdit; na vše ostatní se zeptej. Protože privátní repozitář nelze spolehlivě ověřit, zeptej se člověka ANO/NE, zda existuje.
- **Jeden krok po druhém.** Řekni, co se teď udělá a proč, v jedné dvou větách bez žargonu; nový pojem vysvětli při prvním použití. Další krok začni až po ověření předchozího.
- **Nabídni, kdo krok provede.** Co jde udělat příkazem, nabídni udělat sám; jinak veď člověka klikáním podle [README.md](../README.md).
- **Člověk rozhoduje.** Název projektu, viditelnost, způsob přístupu agenta a nasazení volí on; ty vysvětli možnosti a jednu doporuč.
- **Průběžně ukazuj stav:** krátký seznam fází hotovo / probíhá / zbývá.

Mluv jazykem člověka; po nalinkování metodiky platí `LANG` z `AGENTS.md` projektu.

## Fáze

Schůzka s Erikem a výběr varianty proběhnou ještě před tvou činností. Na začátku zjisti operační systém, nástroj, ve kterém běžíš, zvolenou variantu a zda už má člověk účet na GitHubu. Proměnné `OWNER` a `REPO` se odvodí a potvrdí ve fázi 4. Kroky klikáním níže jsou v [README.md](../README.md).

| # | Fáze | Kdo | Ověření |
|---|---|---|---|
| 1 | Schůzka s Erikem + varianta (vlastní vývoj / outsourcing); u vlastního vývoje požádáno o READ práva na `erikni/sdlc-ai` | člověk | člověk potvrdí variantu; u vlastního vývoje později ve fázi 8 `gh repo view erikni/sdlc-ai` |
| 2 | Agent na počítači a nástroje: `git`, GitHub CLI `gh`, Python 3.9+ | agent zjistí; instalace člověkem, nebo agentem se souhlasem | `git --version`, `gh --version`, `python3 --version` |
| 3 | Účet GitHub a přihlášení `gh` | člověk | `gh auth status`, `gh api user --jq .login` |
| 4 | Doptání konfigurace → jeden `env` blok potvrzený člověkem | agent se ptá; člověk potvrdí | člověk potvrdí `env` blok; `OWNER`/`REPO` odpovídají URL |
| 5 | Nastavení repozitáře v prohlížeči (README kroky 1–3): ochrana, Issues, slučování | agent v Chrome se souhlasem, nebo člověk klikáním | `gh api "repos/$OWNER/$REPO/rules/branches/main" --jq '[.[].type]'`; `gh repo view "$OWNER/$REPO" --json hasIssuesEnabled,mergeCommitAllowed,deleteBranchOnMerge` |
| 6 | Přístup agenta pro běžnou práci — nabídni volbu (README kroky 4–5) | člověk | `gh auth status` |
| 7 | Složka projektu na počítači | agent se souhlasem | `git -C <složka> remote get-url origin` |
| 8 | Instalace metodiky (gated, viz AGENTS.md) | agent; merge schvaluje člověk | sloučený PR; `docs-check` metodiky, pokud existuje |
| 9 | Nasazení (volitelné) | člověk uloží údaje; agent počká a ověří | podle dokumentu `DEPLOY` metodiky; preview / `SITE_URL` po běhu |
| 10 | Shrnutí a první zadání | agent | – |

### Detaily fází

1. **Schůzka a varianta.** Onboarding začíná schůzkou s Erikem (<https://calendly.com/erik-brozek>), kde si člověk vybere variantu. Potvrď, která platí. U **vlastního vývoje** člověk požádá o READ práva na `erikni/sdlc-ai`; upozorni, že fáze 8 je `BLOCKED`, dokud nejsou udělena. U **outsourcingu** vyvíjí Erik nebo jeho tým a klient zbytek tohoto bootstrapu nespouští.
2. **Agent a nástroje.** Agent běží na počítači (Claude Code nebo Codex). Pokud některý nástroj chybí, dej odkaz na oficiální instalaci (<https://git-scm.com/downloads>, <https://cli.github.com/>, <https://www.python.org/downloads/>) a nabídni instalaci správcem balíčků. Nic neinstaluj bez souhlasu. Na Windows může být Python jako `python` nebo `py`. Volitelně doporuč GitHub Desktop a GitHub Mobile (schválení PR z telefonu).
3. **Přihlášení.** Pokud člověk nemá účet, pošli ho na <https://github.com/signup>; účet mu nezakládej. Pak ho požádej, ať ve vlastním okně terminálu spustí `gh auth login` a zvolí **GitHub.com**, **HTTPS** a **Login with a web browser**; jednorázový kód si do prohlížeče zkopíruje sám.
4. **Doptání konfigurace.** Zeptej se na GitHub URL projektu (`https://github.com/<owner>/<repo>`) a z něj odvoď `GITHUB_OWNER` a `REPOSITORY_NAME`; nech člověka obojí potvrdit. Zeptej se **ANO/NE**, zda repozitář už existuje; při NE ho naveď na jeho založení na <https://github.com/new> se zaškrtnutým **Add a README** (čímž vznikne větev `main`) podle [README.md](../README.md). Pak se zeptej na `LANG`, `DEPLOY_PROVIDER` (`CLOUDFLARE` / `DOCKER` / žádný) a u Cloudflare na typ (**PAGES** / **WORKERS**), ze kterého odvodíš `PREVIEW_URL_PATTERN` (`*-<REPO>.sdlc-ai.workers.dev` pro Workers, `*.<REPO>.pages.dev` pro Pages); u Dockeru se zeptej na `DOCKERFILE_PATH` a `PORT`. Zeptej se na `SITE_URL` (prázdné, pokud žádné) a `GATE_APPROVAL_MODE` (jak se schvaluje merge; výchozí je klik v chatu přes `SCHVÁLIT PR`). Ukaž celý `env` blok a nech ho člověka potvrdit ještě před jakoukoli změnou.
5. **Nastavení repozitáře v prohlížeči.** Jednorázové nastavení — ochrana větve main, Issues a slučování (README kroky 1–3) — se dělá v prohlížeči. Automatické proklikání jde jen v **Chrome** s rozšířením agenta; na počítači s výchozím Safari nabídni instalaci Chrome, nebo veď člověka **ručním klikáním**. Vysvětli, že bez ochrany by změna mohla do hlavní větve bez schválení. Očekávané výsledky: `["deletion","non_fast_forward","pull_request"]` a `hasIssuesEnabled` i `mergeCommitAllowed` jsou `true`. Pokud ochrana selže kvůli tarifu, nabídni veřejný repozitář, placený tarif, nebo pokračování bez ochrany s výslovným upozorněním, že pravidlo pak vynucuje jen metodika.
6. **Přístup pro běžnou práci.** Nabídni člověku volbu a vysvětli kompromis; doporuč omezený klíč:

   | Možnost | Co znamená |
   |---|---|
   | Omezený přístupový klíč (doporučeno) | agent smí pracovat jen v tomto repozitáři a nemůže měnit jeho nastavení. Člověk klíč vytvoří klikáním podle [README.md](../README.md) kroku 4 a ve vlastním okně terminálu spustí `gh auth login --with-token`, klíč vloží a potvrdí. `gh auth status` pak ukáže token začínající `github_pat_`. |
   | GitHub Desktop (nejjednodušší) | člověk se jednou přihlásí v GitHub Desktop; `git push`/`pull` pak funguje bez klíče. Pro netechnického člověka jednodušší, ale agent má pak plná práva účtu člověka ve všech jeho repozitářích — vědomý kompromis proti nejmenším nutným právům. |

   Volbu zaznamenej do shrnutí. Klíč nikdy neber do konverzace.
7. **Složka projektu.** Zeptej se, kam projekt uložit, a se souhlasem spusť `gh repo clone "$OWNER/$REPO" <složka>`. Řekni člověku, ať příště otevře nástroj v této složce; `AGENTS.md` v jejím kořeni se pak načte sám.
8. **Instalace (gated).** Instaluje **privátní** metodiku a řídí se [AGENTS.md](../AGENTS.md) („Instalace metodiky“). Nejdřív ověř přístup příkazem `gh repo view erikni/sdlc-ai`; **bez přístupu je stav `BLOCKED`** a člověk o něj musí požádat vlastníka metodiky — privátní repozitář nečti ani neobcházej. S přístupem proveď instalační kroky z `AGENTS.md` s nejnovější vydanou verzí: metodika se **linkuje, nekopíruje** — materializuj jen `AGENTS.md` (projektově vyplněné) a poznámku v `README.md`, přidej sdílené caller workflow (CI/preview/production), založ PR a požádej o schválení sekcí „Co schvalujete“. Vysvětli, že odkaz **Diff** ukazuje přesně, co se mění, a že odpověď `SCHVÁLIT PR` změnu zařadí do hlavní větve. Po schválení sluč podle dokumentu `TASK` nainstalované metodiky.
9. **Nasazení.** Zeptej se, zda má projekt běžet na webu. Pokud ano, vysvětli, co je potřeba podle [README.md](../README.md), a přístupové údaje poskytovatele nech ukládat člověka. Po mergi, který nasazuje, počkej na dokončení běhu — nejvýš 5 minut — pak ověř preview / `SITE_URL`; pokud neskončí nebo selže, nahlas `BLOCKED`. Pokud se projekt nenasazuje, nech `DEPLOY_PROVIDER` prázdný.
10. **Shrnutí.** Uveď adresu repozitáře, zvolenou variantu, složku na počítači, zvolený způsob přístupu, stav ochrany větve, verzi metodiky, preview / `SITE_URL` při nasazení a co zůstalo nevyřešené. Nabídni první zadání a vysvětli rozdíl mezi `TASK`, `STORY` a `EPIC` jednou větou u každého.

## Přerušení a pokračování

Onboarding lze kdykoli přerušit. Při pokračování nezačínej znovu: spusť ověřovací příkazy od fáze 2 a navaž první fází, která neprojde. Jakmile existuje repozitář a issue inicializace, zapisuj stav do tohoto issue jednou větou.
