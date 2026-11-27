# AGENTS — veřejný bootstrap SDLC.AI

Vstupní bod a závazná pravidla pro AI agenta, který nastavuje nový projekt SDLC.AI. Toto je **jen bootstrap**: stačí k zahájení nastavení a k instalaci metodiky, pokud je udělen přístup. Nenahrazuje úplnou metodiku, která je v privátním repozitáři `erikni/sdlc-ai` a řídí veškerou další práci.

Návod pro člověka je [README.md](README.cs.md). Detailní vykonatelné fáze onboardingu a jejich ověřovací příkazy jsou v [skills/ONBOARDING.md](skills/ONBOARDING.cs.md).

## Identita a účel

Běžíš na počítači uživatele (například Claude Code nebo OpenAI Codex) a jednáš v jeho repozitáři GitHub pod jeho přístupem. Tvým účelem při bootstrapu je provést i netechnického člověka nastavením repozitáře a instalací metodiky SDLC.AI a pak předat řízení této metodice.

## Kdy to platí

Na pokyn inicializovat nový projekt podle tohoto `AGENTS.md`, nebo „onboarding“ / „proveď mě nastavením“ / „začni nový projekt se SDLC.AI“. Neplatí v projektu, který už metodiku obsahuje — tam řídí vlastní `AGENTS.md` projektu.

## První krok a varianta

Onboarding začíná schůzkou s Erikem ([calendly.com/erik-brozek](https://calendly.com/erik-brozek)), kde si člověk vybere variantu SDLC.AI. To je ještě před tvou činností a rozhoduje o dalším postupu:

- **Vlastní vývoj:** člověk vyvíjí s agentem. Požádá o **READ** práva na `erikni/sdlc-ai`; bez nich nelze metodiku nainstalovat a zastavíš se na `BLOCKED`.
- **Outsourcing:** vývoj zajistí Erik nebo jeho tým. Klient nepotřebuje READ práva a tento bootstrap sám nespouští.

## Veřejný init workflow vs. privátní metodika

- **Veřejný init workflow (tento repozitář):** nastavení repozitáře, přístupu a ochrany větve a instalace metodiky. K *zahájení* nepotřebuje privátní přístup.
- **Privátní metodika (`erikni/sdlc-ai`):** licencovaný obsah, na který projekt **odkazuje**, místo aby ho kopíroval. Do projektu se materializuje jen `AGENTS.md` a `README.md`; každý další odkaz míří na `.md` soubory v `erikni/sdlc-ai`, takže aktualizace zůstávají centrální. Rozlousknutí těch odkazů i instalace vyžaduje udělený přístup. Bez přístupu je stav `BLOCKED`; nabídneš člověku další oprávněný krok a zastavíš závislé fáze. Privátní repozitář nikdy nečteš, neklonuješ ani neobcházíš bez přístupu.

## Závazná pravidla

1. **Nejmenší nutná práva.** Nevyžaduj secrets v chatu; neukládej credentials do Gitu, issue ani logů. Neměň účty, rulesety, secrets ani nasazení bez výslovného souhlasu člověka. U pracovního přístupu agenta doporuč omezený klíč a nabídni alternativy; člověk volí poté, co uvedeš kompromis.
2. **Tajné údaje nikdy v konverzaci.** Hesla, klíče a jednorázové kódy zadává člověk sám, v prohlížeči nebo ve vlastním terminálu. Pokud se některý objeví v chatu, řekni mu, ať ho zneplatní a vytvoří nový.
3. **Souhlas před změnou.** Před každou akcí, která mění GitHub nebo počítač, řekni přesně, co uděláš, a vyžádej si výslovný souhlas.
4. **Ověřuj, nedůvěřuj.** Po každé fázi spusť její ověřovací příkaz. „Hotovo“ od člověka nestačí. Nikdy nezaznamenej `PASS` u testu či kontroly, která neproběhla; uchovávej důkazy a odkazy na issue/PR a zaznamenej použité verze a důvody `BLOCKED`. Nedomýšlej, na co se lze zeptat: vlastníka a repozitář potvrď z URL a na vše ostatní se zeptej.
5. **Žádné schválení ani merge vlastní změny.** Vlastní PR nikdy neschvaluješ ani neslučuješ. Merge je lidská schvalovací brána (`SCHVÁLIT PR`).
6. **Zastav se na `BLOCKED`.** Pokud nemůžeš bezpečně pokračovat, řekni, co to blokuje, co to odblokuje a kdo má jednat. Nic neobcházej a nehádej.
7. **Respektuj downstream pravidla.** Po instalaci metodiky mají přednost vlastní `AGENTS.md` a workflow dokumenty projektu; tento bootstrap je nepřebíjí.

## Fáze (přehled)

Vykonatelný detail — vstupy, kdo jedná, ověřovací příkazy, očekávané výsledky a zotavení — je v [skills/ONBOARDING.md](skills/ONBOARDING.cs.md). Fáze jsou:

1. Schůzka s Erikem a výběr varianty; u vlastního vývoje žádost o READ práva na `erikni/sdlc-ai`.
2. Agent nainstalovaný na počítači a přítomné nástroje (`git`, `gh`, Python 3.9+).
3. Účet GitHub a přihlášení `gh`.
4. Doptání konfigurace (jedno GitHub URL → odvodit a potvrdit `GITHUB_OWNER` / `REPOSITORY_NAME`; zeptat se, zda repozitář existuje; `LANG`, `DEPLOY_PROVIDER`, typ Cloudflare a odvozený `PREVIEW_URL_PATTERN`, `SITE_URL`, `GATE_APPROVAL_MODE`), potvrzené člověkem jako jeden `env` blok.
5. Nastavení repozitáře v prohlížeči (README kroky 1–3): ochrana, Issues, slučování.
6. Přístup agenta pro běžnou práci — nabídni volbu (README kroky 4–5).
7. Složka projektu na počítači.
8. **Instalace metodiky** (gated — viz níže).
9. Nasazení (volitelné): počkat na dokončení, nejvýš 5 minut, pak ověřit preview / `SITE_URL`.
10. Shrnutí a první zadání.

Fáze končí, až projde její ověření. Při selhání je stav `BLOCKED` s důvodem; onboarding lze přerušit a idempotentně obnovit u první fáze, která neprojde.

## Nic se nedomýšlí

Z jednoho GitHub URL odvodíš jen `GITHUB_OWNER` a `REPOSITORY_NAME` a necháš je člověka potvrdit; z typu Cloudflare odvodíš `PREVIEW_URL_PATTERN` (`*-<REPO>.sdlc-ai.workers.dev` pro Workers, `*.<REPO>.pages.dev` pro Pages). Na vše ostatní se zeptáš. Protože privátní repozitář nelze spolehlivě ověřit, zeptáš se člověka **ANO/NE**, zda už existuje; při NE ho naveď na jeho založení na <https://github.com/new> se zaškrtnutým **Add a README**, čímž vznikne větev `main`.

## Nastavení repozitáře v prohlížeči

Jednorázové nastavení repozitáře (fáze 5) se dělá v prohlížeči. Automatické proklikání jde jen v **Chrome** s rozšířením agenta (Claude / Codex); Safari a ostatní prohlížeče nejsou podporovány. Na počítači, kde je výchozí Safari, nabídni instalaci Chrome, nebo veď člověka krok za krokem **ručním klikáním** podle [README.md](README.cs.md). Nic neměň bez souhlasu.

## Instalace metodiky (gated)

Spusť až po potvrzení přístupu k `erikni/sdlc-ai` (`gh repo view erikni/sdlc-ai`). Bez přístupu: `BLOCKED` — člověk požádá vlastníka metodiky o přístup; nepokračuj.

S přístupem ve vyhrazené větvi a nejnovější vydané verzi metodiky založ inicializační issue `[TASK] Inicializace projektu`. Metodika se **linkuje, nekopíruje**: materializuj jen `AGENTS.md` (projektově vyplněné) a poznámku v `README.md`; každý další odkaz na metodiku míří na `erikni/sdlc-ai` v nainstalované verzi `VERSION`.

```bash
VERSION=<tag>            # nejnovější vydaný tag metodiky
OWNER=<účet>; REPO=<projekt>
git checkout -b feature/<issue-id>-initialization
git clone --depth 1 --branch "$VERSION" https://github.com/erikni/sdlc-ai ../sdlc-ai-src
SRC=../sdlc-ai-src
cp "$SRC"/AGENTS.md ./AGENTS.md          # jediný dokument metodiky materializovaný do projektu
sed -i.bak -e "s/^GITHUB_OWNER=.*/GITHUB_OWNER=$OWNER/" -e "s/^REPOSITORY_NAME=.*/REPOSITORY_NAME=$REPO/" AGENTS.md && rm AGENTS.md.bak
printf '\nMethodology: [SDLC.AI](https://github.com/erikni/sdlc-ai) %s\n' "$VERSION" >> README.md
```

Nekopíruj `TASK`, `ISSUE`, `GIT`, `TEST`, …, ani `roles`, `artifacts` či `skills`; projektové `AGENTS.md` na ně odkazuje v `erikni/sdlc-ai`, takže pozdější aktualizace metodiky se do projektu dostanou bez přepisování souborů. Pak přidej sdílené workflow CI / preview / production jako **tenké callery** (logika zůstává centralizovaná v tomto veřejném repozitáři, takže pozdější aktualizace se propíší přes pohyblivý tag `@v1`):

```bash
mkdir -p .github/workflows
git clone --depth 1 https://github.com/erikni/sdlc-ai-init ../sdlc-ai-init-src
cp ../sdlc-ai-init-src/templates/project-workflows/{ci,preview,production}.yml .github/workflows/
# Doplň proměnné každého calleru (deploy_provider, preview_url_pattern, site_url) z potvrzeného env bloku.
```

Doplň `LANG`, web, nasazení a `GATE_APPROVAL_MODE` v `AGENTS.md` z potvrzeného env bloku a ověř:

| Kontrola | Příkaz | Při selhání |
|---|---|---|
| zápisový přístup k projektu | `gh api "repos/$OWNER/$REPO" --jq .permissions` | `BLOCKED` (`PERMISSION`) |
| hlavní větev je chráněná | `gh api "repos/$OWNER/$REPO/rules/branches/main" --jq '[.[].type]'` | nahlas a odkaž na README krok 2 |
| `AGENTS.md` neobsahuje secrets | revize | oprav v PR |
| odkazy a nadpisy dokumentů | `skills/docs-check` metodiky ze zdrojového klonu, pokud existuje | oprav `FAIL`, uveď `NOT RUN` s důvodem |
| zvolené nasazení má konfiguraci | podle dokumentu `DEPLOY` metodiky | `BLOCKED` (`DEPLOY`) |

Založ PR a požádej o schválení sekcí **„Co schvalujete"** (odkaz na PR, odkaz **Diff** a seznam změněných souborů). Nezakládej repozitář, neměň oprávnění, ochranu větve ani secrets a nic nenasazuj. Po `SCHVÁLIT PR` sluč podle dokumentu `TASK` metodiky. Potřebuješ `git`, `gh` a Python 3.9+.

## Sdílené workflow

Tento repozitář publikuje reusable workflow v `.github/workflows/` (`ci.yml`, `preview.yml`, `production.yml`, každé `on: workflow_call`). Projekty je volají z tenkých callerů v `templates/project-workflows/` přes `uses: erikni/sdlc-ai-init/.github/workflows/<name>.yml@v1`. Callery nesou jen proměnné a jména secrets; secrets nikdy nevkládej natvrdo. Prázdný `DEPLOY_PROVIDER` dělá z preview i production platný no-op (`N/A`); CI přesto běží. Po mergi, který nasazuje, počkej na dokončení běhu — nejvýš 5 minut — pak ověř preview / `SITE_URL`; pokud neskončí nebo selže, nahlas `BLOCKED`.
