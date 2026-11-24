# Jak začít nový projekt se SDLC.AI

Návod pro neprogramátory. AI agent běží na vašem počítači, například v Claude Code nebo OpenAI Codex, a pracuje za vás s GitHubem. Nastavení zabere asi 20 minut a děláte ho jen jednou. Pak zadáváte práci běžným jazykem a jen schvalujete výsledky.

Tento repozitář (`erikni/sdlc-ai-init`) je **veřejný bootstrap**: stačí k zahájení nastavení. Úplná metodika SDLC.AI je v samostatném privátním repozitáři `erikni/sdlc-ai` a do vašeho projektu se nainstaluje, jen pokud k ní máte udělený přístup (viz [Co bude následovat](#co-bude-následovat)).

- **Pro vás (člověka):** toto `README.md`.
- **Pro vašeho agenta:** [AGENTS.md](AGENTS.cs.md) — závazný bootstrap workflow.

**Co budete potřebovat**

- účet na [github.com](https://github.com) (zdarma);
- nástroj s AI agentem nainstalovaný na počítači.

## Nejrychlejší cesta: ať vás provede agent

Otevřete nástroj s agentem a zkopírujte mu tento pokyn:

```text
Proveď mě úvodním nastavením nového projektu podle skills/ONBOARDING.cs.md
z repozitáře erikni/sdlc-ai-init. Nejsem programátor, veď mě krok za krokem.
```

Agent vás provede vším popsaným níže: zkontroluje, co na počítači chybí, řekne, kam kliknout, každý krok si sám ověří a co jde udělat příkazem, nabídne udělat sám. Hesla ani přístupové klíče nikdy nepište do konverzace; zadáváte je jen vy sami, v prohlížeči nebo ve vlastním okně terminálu.

Zbytek návodu popisuje stejné kroky pro případ, že si jimi chcete projít sami. Kroky 1–4 jsou na webu GitHubu, krok 5 na počítači a od kroku 6 pracuje agent.

**Slovníček**

| Pojem | Co to je |
|---|---|
| Repozitář | složka vašeho projektu na GitHubu; drží všechny soubory a jejich historii |
| Hlavní větev (`main`) | platná verze projektu |
| Pull request (PR) | navržená změna, která čeká na vaše schválení, než se dostane do hlavní větve |
| Issue | zadání nebo úkol |
| Token | přístupový klíč, kterým agent pracuje ve vašem repozitáři |

Názvy tlačítek jsou anglicky tak, jak je ukazuje GitHub. GitHub občas upraví rozhraní; pokud se název mírně liší, hledejte nejbližší podobný.

## Krok 1 – Vytvořte repozitář

1. Otevřete <https://github.com/new>.
2. **Owner:** zvolte svůj účet.
3. **Repository name:** napište název projektu bez mezer a diakritiky, například `muj-projekt`.
4. Zvolte viditelnost: **Public** (vidí ho kdokoli) nebo **Private** (jen vy a pozvaní).
5. Zapněte přidání README souboru (**Add README**). Je to důležité: vytvoří větev `main`, bez níž agent nemůže začít.
6. Klikněte na **Create repository**.

✅ Hotovo, když vidíte stránku repozitáře se souborem `README.md` a větví `main` vlevo nahoře.

> U soukromého repozitáře (**Private**) vyžaduje ochrana z kroku 2 placený tarif GitHubu (Pro nebo Team). U veřejného je zdarma.

## Krok 2 – Zapněte ochranu hlavní větve

Ochrana zajistí, že se do hlavní větve nic nedostane jinak než navrženou změnou (pull requestem), kterou schválíte. Platí i pro agenta.

1. Na stránce repozitáře klikněte nahoře na **Settings**.
2. V levém menu klikněte na **Rules** a pak **Rulesets**.
3. Klikněte na **New ruleset** a zvolte **New branch ruleset**.
4. **Ruleset name:** napište `main`.
5. **Enforcement status:** nastavte na **Active**.
6. **Bypass list:** nechte prázdný. Nikdo, ani agent, nemá výjimku.
7. **Target branches:** klikněte na **Add target** a zvolte **Include default branch**.
8. V sekci pravidel zaškrtněte:
   - **Restrict deletions** – větev nelze smazat;
   - **Require a pull request before merging** – každá změna jde přes návrh. **Required approvals** nechte na `0` a zaškrtněte **Dismiss stale pull request approvals when new commits are pushed**;
   - **Block force pushes** – historii nelze přepsat.
9. Dole klikněte na **Create**.

✅ Hotovo, když má seznam Rulesets položku `main` ve stavu **Active**.

## Krok 3 – Nastavte slučování

1. V **Settings** klikněte v levém menu na **General**.
2. V sekci **Features** potvrďte, že je zaškrtnuté **Issues**.
3. Sjeďte k sekci **Pull Requests** a potvrďte, že je zaškrtnuté **Allow merge commits**.
4. Tamtéž zaškrtněte **Automatically delete head branches**. Pracovní větve se pak po schválení uklidí.

## Krok 4 – Vytvořte přístupový klíč pro agenta

Token je klíč, kterým agent pracuje ve vašem repozitáři za vás. Tento typ klíče platí jen pro jeden repozitář a jen pro činnosti, které povolíte. S ním agent nemůže měnit nastavení, smazat repozitář ani sáhnout na vaše ostatní projekty.

1. Vpravo nahoře klikněte na profilový obrázek a zvolte **Settings**.
2. V levém menu úplně dole klikněte na **Developer settings**.
3. Klikněte na **Personal access tokens** a pak **Fine-grained tokens**.
4. Klikněte na **Generate new token**.
5. **Token name:** napište například `sdlc-ai-muj-projekt`.
6. **Expiration:** zvolte platnost, například 90 dní. Po vypršení vytvořte nový stejně.
7. **Resource owner:** zvolte svůj účet.
8. **Repository access:** zvolte **Only select repositories** a vyberte repozitář z kroku 1.
9. **Permissions → Repository permissions:** nastavte přesně tyto tři položky, ostatní nechte být:

   | Položka | Nastavení | Proč to agent potřebuje |
   |---|---|---|
   | Contents | Read and write | ukládání změn souborů a publikace verzí |
   | Issues | Read and write | zakládání a uzavírání úkolů |
   | Pull requests | Read and write | navrhování změn a jejich sloučení po vašem schválení |

10. Klikněte na **Generate token** a klíč hned zkopírujte. GitHub ho ukáže jen jednou.

**Bezpečnost klíče**

- Uložte ho do správce hesel. Neposílejte ho e-mailem ani zprávou a nevkládejte ho do repozitáře, úkolu ani konverzace s agentem.
- Kdykoli ho můžete zneplatnit na stejné stránce (**Fine-grained tokens**); agent okamžitě ztratí přístup.
- Protože klíč patří vašemu účtu, vše, co agent udělá, bude na GitHubu uvedeno pod vaším jménem.

## Krok 5 – Připravte agenta na počítači

Agent pracuje s GitHubem přes program GitHub CLI (`gh`), který běží na vašem počítači.

1. Nainstalujte [Git](https://git-scm.com/downloads), [GitHub CLI](https://cli.github.com/) a [Python](https://www.python.org/downloads/). U každého stáhněte instalátor pro svůj systém a proklikejte ho s výchozími volbami.
2. Otevřete okno terminálu: na Macu aplikaci **Terminal**, na Windows **PowerShell**.
3. Přihlaste agenta klíčem z kroku 4. Napište příkaz, stiskněte Enter, vložte klíč, znovu Enter a pak Ctrl+D (na Windows Ctrl+Z a Enter):

   ```bash
   gh auth login --with-token
   ```

4. Vyberte na počítači složku pro projekt a otevřete v ní nástroj s agentem.

✅ Hotovo, když příkaz `gh auth status` vypíše váš účet a token začínající `github_pat_`.

Klíč zadávejte jen v okně terminálu, nikdy v konverzaci s agentem. Přihlášení přes prohlížeč příkazem `gh auth login` je jednodušší, ale agent pak má stejná práva jako vy ve všech vašich repozitářích; omezený klíč je bezpečnější.

## Krok 6 – Dejte agentovi pokyn

Zkopírujte tento text do konverzace s agentem a doplňte hodnoty v lomených závorkách:

```text
Inicializuj nový projekt SDLC.AI podle AGENTS.md z repozitáře erikni/sdlc-ai-init.
Repozitář: <váš-účet>/<název-projektu>
Verze metodiky: nejnovější vydaná
Jazyk komunikace: čeština
Web projektu: <adresa, nebo „zatím žádný">
Nasazení: <„Cloudflare", „lokální Docker", nebo „žádné">
Změny schvaluje: <vaše jméno>
```

## Co bude následovat

1. Agent ověří, že má přístup k vašemu repozitáři. K instalaci metodiky navíc potřebuje přístup k privátnímu `erikni/sdlc-ai`. Pokud ten přístup nemáte, agent se zastaví a nahlásí `BLOCKED` s vysvětlením — privátní repozitář nikdy nečte ani neobchází. O přístup požádejte vlastníka metodiky.
2. S přístupem zkopíruje pravidla SDLC.AI do vašeho projektu a přidá sdílené workflow CI / preview / production jako tenké caller soubory (logika zůstává centralizovaná zde, takže pozdější aktualizace se do projektu dostanou bez přepisování souborů).
3. Zkontroluje, že je vše připravené: přístup, ochrana hlavní větve, nastavení a odkazy v dokumentech.
4. Pošle vám žádost začínající nadpisem **„Co schvalujete"**. Obsahuje odkaz na navrženou změnu, odkaz **Diff** (ukáže přesně, co se mění) a seznam změněných souborů.
5. Pokud souhlasíte, odpovězte `SCHVÁLIT PR`. Chcete-li něco jinak, napište `DOPLNIT ZADÁNÍ` a co se má změnit.

Po schválení je projekt připraven. Když agent narazí na překážku, napíše, co ho blokuje a co máte udělat.

## Jak zadávat práci

Napište agentovi, co chcete, a začněte velikostí úkolu:

```text
[TASK] Oprav překlep na úvodní stránce.
[STORY] Přidej přihlášení e-mailem a heslem.
[EPIC] Zákaznický portál – první verze.
```

| Typ | Kdy použít | Co budete schvalovat |
|---|---|---|
| `TASK` | malá, jasná změna | jednu žádost „Co schvalujete" |
| `HOTFIX` | oprava, kterou chcete k uživatelům co nejdřív | jednu žádost o zařazení i vydání a pak přijetí vydané verze |
| `STORY` | nová funkce | jednu žádost; agent navíc zdokumentuje testy a bezpečnost |
| `EPIC` | větší celek z více částí | krok za krokem: záměr, návrh, jednotlivé části a nakonec vydání |

| Agent dělá sám | Vy |
|---|---|
| zakládá úkoly, připravuje změny a návrhy | zadáváte práci a volíte její velikost |
| spouští kontroly a pravdivě hlásí výsledky | schvalujete každou změnu, než se zařadí do hlavní větve |
| po vašem schválení změnu zařadí a úkol uzavře | u `EPIC` a `HOTFIX` navíc schvalujete vydání a jeho přijetí |
| hlásí překážky a navrhuje řešení | spravujete účet, přístupový klíč a nastavení repozitáře |

Agent nikdy neschvaluje vlastní práci a bez vašeho souhlasu nic nezařadí do hlavní větve.

## Nasazení na web (volitelné)

Má-li projekt běžet na internetu, potřebujete navíc účet u poskytovatele (metodika má hotový postup pro Cloudflare) a uložení jeho přístupového klíče mezi secrets repozitáře: **Settings → Secrets and variables → Actions → New repository secret**. Sdílené produkční workflow ho pak čte podle jména. Tento krok je techničtější; doporučujeme ho udělat s někým zkušeným. Bez něj vše ostatní funguje, jen se výsledek sám nezveřejní.

Chcete-li jen běžet na vlastním počítači bez účtu u poskytovatele, nastavte `DEPLOY_PROVIDER=DOCKER` a projekt poběží v lokálním Docker kontejneru.

Žádné webhooky ani další integrace metodika nevyžaduje.

## Když něco nefunguje

| Co vidíte | Co udělat |
|---|---|
| V repozitáři chybí záložka **Settings** | nejste přihlášeni účtem, který repozitář vlastní |
| Ruleset nejde zapnout nebo neplatí | repozitář je **Private** na free tarifu; změňte ho na **Public** (Settings → General → úplně dole **Change visibility**), nebo přejděte na placený tarif |
| Agent hlásí, že nemá přístup | zkontrolujte repozitář zvolený v kroku 4 a tři oprávnění; klíč také mohl vypršet |
| Agent hlásí, že nemá přístup k metodice | repozitář `erikni/sdlc-ai` je privátní; požádejte jeho vlastníka o přístup. Samotný veřejný bootstrap umožní začít, metodika se ale nainstaluje jen s přístupem |
| Agent hlásí `BLOCKED` | napíše, co ho blokuje a co to odblokuje; obvykle chybí nastavení z kroků 2–5 |
| Omylem jste klíč někam vložili | hned ho zneplatněte na stránce **Fine-grained tokens** a vytvořte nový |

## Pro pokročilé

Technické detaily pro ty, kdo chtějí totéž příkazy, nebo pracují v týmu.

<details>
<summary>Kroky 1–3 příkazy GitHub CLI</summary>

```bash
OWNER=vas-ucet
REPO=nazev-projektu

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

Poslední příkaz má vrátit `["deletion","non_fast_forward","pull_request"]`.

</details>

<details>
<summary>Samostatný účet pro agenta (týmy a organizace)</summary>

V týmu je vhodné, aby agent pracoval pod vlastním účtem: je vidět, co udělal agent a co člověk, a lze vyžádat schválení od jiného účtu, než který změnu navrhl (**Required approvals** `1` v kroku 2).

- Omezený token (fine-grained) funguje jen pro repozitáře vlastněné samotným účtem nebo organizací, jejímž je členem. Repozitář proto musí patřit organizaci a účet agenta musí být jejím členem s rolí Write, ne Admin.
- Token se pak vytvoří pod účtem agenta stejně jako v kroku 4, s organizací jako **Resource owner**.
- Pokud místo rulesetu používáte klasickou ochranu větve, přidejte tokenu oprávnění **Administration: Read-only**, jinak ho agent nenačte a přestane slučovat.
- Má-li projekt automatické kontroly (CI), přidejte **Commit statuses** a **Actions** jako read-only.

</details>

Přesný, agentem vykonatelný postup inicializace je v [AGENTS.md](AGENTS.cs.md).
