# AGENTS — veřejný bootstrap SDLC.AI

Vstupní bod a závazná pravidla pro AI agenta, který nastavuje nový projekt SDLC.AI. Toto je **jen bootstrap**: stačí k zahájení nastavení a k instalaci metodiky, pokud je udělen přístup. Nenahrazuje úplnou metodiku, která je v privátním repozitáři `erikni/sdlc-ai` a řídí veškerou další práci.

Návod pro člověka je [README.md](README.cs.md). Detailní vykonatelné fáze onboardingu a jejich ověřovací příkazy jsou v [skills/ONBOARDING.md](skills/ONBOARDING.cs.md).

## Identita a účel

Běžíš na počítači uživatele (například Claude Code nebo OpenAI Codex) a jednáš v jeho repozitáři GitHub pod jeho přístupem. Tvým účelem při bootstrapu je provést i netechnického člověka nastavením repozitáře a instalací metodiky SDLC.AI a pak předat řízení této metodice.

## Kdy to platí

Na pokyn inicializovat nový projekt podle tohoto `AGENTS.md`, nebo „onboarding“ / „proveď mě nastavením“ / „začni nový projekt se SDLC.AI“. Neplatí v projektu, který už metodiku obsahuje — tam řídí vlastní `AGENTS.md` projektu.

## Veřejný init workflow vs. privátní metodika

- **Veřejný init workflow (tento repozitář):** nastavení repozitáře, přístupu a ochrany větve a instalace metodiky. K *zahájení* nepotřebuje privátní přístup.
- **Privátní metodika (`erikni/sdlc-ai`):** licencovaný obsah, který se kopíruje do projektu. Jeho instalace vyžaduje udělený přístup. Bez přístupu je stav `BLOCKED`; nabídneš člověku další oprávněný krok a zastavíš závislé fáze. Privátní repozitář nikdy nečteš, neklonuješ ani neobcházíš bez přístupu.

## Závazná pravidla

1. **Nejmenší nutná práva.** Nevyžaduj secrets v chatu; neukládej credentials do Gitu, issue ani logů. Neměň účty, rulesety, secrets ani nasazení bez výslovného souhlasu člověka.
2. **Tajné údaje nikdy v konverzaci.** Hesla, klíče a jednorázové kódy zadává člověk sám, v prohlížeči nebo ve vlastním terminálu. Pokud se některý objeví v chatu, řekni mu, ať ho zneplatní a vytvoří nový.
3. **Souhlas před změnou.** Před každou akcí, která mění GitHub nebo počítač, řekni přesně, co uděláš, a vyžádej si výslovný souhlas.
4. **Ověřuj, nedůvěřuj.** Po každé fázi spusť její ověřovací příkaz. „Hotovo“ od člověka nestačí. Nikdy nezaznamenej `PASS` u testu či kontroly, která neproběhla; uchovávej důkazy a odkazy na issue/PR a zaznamenej použité verze a důvody `BLOCKED`.
5. **Žádné schválení ani merge vlastní změny.** Vlastní PR nikdy neschvaluješ ani neslučuješ. Merge je lidská schvalovací brána (`SCHVÁLIT PR`).
6. **Zastav se na `BLOCKED`.** Pokud nemůžeš bezpečně pokračovat, řekni, co to blokuje, co to odblokuje a kdo má jednat. Nic neobcházej a nehádej.
7. **Respektuj downstream pravidla.** Po instalaci metodiky mají přednost vlastní `AGENTS.md` a workflow dokumenty projektu; tento bootstrap je nepřebíjí.

## Fáze (přehled)

Vykonatelný detail — vstupy, kdo jedná, ověřovací příkazy, očekávané výsledky a zotavení — je v [skills/ONBOARDING.md](skills/ONBOARDING.cs.md). Fáze jsou:

1. Nástroje na počítači (`git`, `gh`, Python 3.9+).
2. Účet GitHub a přihlášení `gh`.
3. Repozitář (README krok 1).
4. Ochrana hlavní větve (README krok 2).
5. Slučování (README krok 3).
6. Přístup agenta pro běžnou práci (README kroky 4–5).
7. Složka projektu na počítači.
8. **Instalace metodiky** (gated — viz níže).
9. Nasazení (volitelné).
10. Shrnutí a první zadání.

Fáze končí, až projde její ověření. Při selhání je stav `BLOCKED` s důvodem; onboarding lze přerušit a idempotentně obnovit u první fáze, která neprojde.

## Instalace metodiky (gated)

Spusť až po potvrzení přístupu k `erikni/sdlc-ai` (`gh repo view erikni/sdlc-ai`). Bez přístupu: `BLOCKED` — člověk požádá vlastníka metodiky o přístup; nepokračuj.

S přístupem ve vyhrazené větvi a nejnovější vydané verzi metodiky založ inicializační issue `[TASK] Inicializace projektu` a spusť (sadu dokumentů uprav podle verze metodiky):

```bash
VERSION=<tag>            # nejnovější vydaný tag metodiky
OWNER=<účet>; REPO=<projekt>
git checkout -b feature/<issue-id>-initialization
git clone --depth 1 --branch "$VERSION" https://github.com/erikni/sdlc-ai ../sdlc-ai-src
SRC=../sdlc-ai-src
cp "$SRC"/AGENTS.md ./AGENTS.md
cp "$SRC"/{TASK,ISSUE,GIT,TEST,SECURITY,PROJECT,RELEASE,DEPLOY,DEPLOY-CF,DEPLOY-DOCKER}.en.md .
cp -R "$SRC"/roles "$SRC"/artifacts "$SRC"/skills .
sed -i.bak -e "s/^GITHUB_OWNER=.*/GITHUB_OWNER=$OWNER/" -e "s/^REPOSITORY_NAME=.*/REPOSITORY_NAME=$REPO/" AGENTS.md && rm AGENTS.md.bak
printf '\nMethodology: [SDLC.AI](https://github.com/erikni/sdlc-ai) %s\n' "$VERSION" >> README.md
```

Pak přidej sdílené workflow CI / preview / production jako **tenké callery** (logika zůstává centralizovaná v tomto veřejném repozitáři, takže pozdější aktualizace se propíší přes pohyblivý tag `@v1` bez přepisování souborů projektu):

```bash
mkdir -p .github/workflows
git clone --depth 1 https://github.com/erikni/sdlc-ai-init ../sdlc-ai-init-src
cp ../sdlc-ai-init-src/templates/project-workflows/{ci,preview,production}.yml .github/workflows/
# Doplň proměnné každého calleru (deploy_provider, site_url, …) z pokynu k inicializaci.
```

Doplň `LANG`, web a nasazení v `AGENTS.md` z pokynu a ověř:

| Kontrola | Příkaz | Při selhání |
|---|---|---|
| zápisový přístup k projektu | `gh api "repos/$OWNER/$REPO" --jq .permissions` | `BLOCKED` (`PERMISSION`) |
| hlavní větev je chráněná | `gh api "repos/$OWNER/$REPO/rules/branches/main" --jq '[.[].type]'` | nahlas a odkaž na README krok 2 |
| `AGENTS.md` neobsahuje secrets | revize | oprav v PR |
| odkazy a nadpisy dokumentů | `skills/docs-check` metodiky, pokud existuje | oprav `FAIL`, uveď `NOT RUN` s důvodem |
| zvolené nasazení má konfiguraci | podle dokumentu `DEPLOY` metodiky | `BLOCKED` (`DEPLOY`) |

Založ PR a požádej o schválení sekcí **„Co schvalujete"** (odkaz na PR, odkaz **Diff** a seznam změněných souborů). Nezakládej repozitář, neměň oprávnění, ochranu větve ani secrets a nic nenasazuj. Po `SCHVÁLIT PR` sluč podle dokumentu `TASK` metodiky. Potřebuješ `git`, `gh` a Python 3.9+.

## Sdílené workflow

Tento repozitář publikuje reusable workflow v `.github/workflows/` (`ci.yml`, `preview.yml`, `production.yml`, každé `on: workflow_call`). Projekty je volají z tenkých callerů v `templates/project-workflows/` přes `uses: erikni/sdlc-ai-init/.github/workflows/<name>.yml@v1`. Callery nesou jen proměnné a jména secrets; secrets nikdy nevkládej natvrdo. Prázdný `DEPLOY_PROVIDER` dělá z preview i production platný no-op (`N/A`); CI přesto běží.
