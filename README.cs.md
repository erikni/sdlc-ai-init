# Začněte nový projekt se SDLC.AI

Návod pro neprogramátory. AI agent běží na vašem počítači, například v Claude Code nebo OpenAI Codex, a pracuje s GitHubem za vás. Nastavení trvá zhruba 20 minut a děláte ho jen jednou. Pak zadáváte práci běžným jazykem a jen schvalujete výsledky.

Tento repozitář (`erikni/sdlc-ai-init`) je **veřejný bootstrap**: stačí k zahájení nastavení. Úplná metodika SDLC.AI je v samostatném privátním repozitáři `erikni/sdlc-ai` a do vašeho projektu se nalinkuje jen tehdy, pokud vám byl k němu udělen přístup (viz [Co bude dál](#co-bude-dál)).

- **Pro vás (člověka):** toto `README.md`.
- **Pro vašeho agenta:** [AGENTS.md](AGENTS.cs.md) — závazný bootstrap workflow.

## Než začnete: schůzka a vaše varianta

Onboarding začíná krátkou schůzkou s Erikem — objednejte se na [calendly.com/erik-brozek](https://calendly.com/erik-brozek). Tam si vyberete variantu SDLC.AI:

- **Vlastní vývoj** — projekt stavíte společně s agentem na svém počítači. Požádáte Erika o **READ přístup** k repozitáři metodiky `erikni/sdlc-ai`; bez něj nemůže agent metodiku nainstalovat. Tento návod je pro vás.
- **Outsourcing** — vývoj zajistí Erik nebo jeho tým. Přístup k metodice nepotřebujete a toto nastavení sami nespouštíte.

**Co budete potřebovat (vlastní vývoj)**

- účet na [github.com](https://github.com) (zdarma);
- nástroj s AI agentem nainstalovaný na počítači;
- READ přístup k `erikni/sdlc-ai`, vyžádaný na schůzce.

## Nejrychlejší cesta: nechte se provést agentem

Otevřete svůj nástroj s agentem a zkopírujte mu tento pokyn:

```text
Proveď mě úvodním nastavením nového projektu podle skills/ONBOARDING.cs.md
z repozitáře erikni/sdlc-ai-init. Nejsem programátor, veď mě krok za krokem.
```

Agent vás provede vším, co je popsané níže: zkontroluje, co na počítači chybí, zeptá se vás na GitHub adresu projektu a pár nastavení (nic nedomýšlí — z adresy jen odvodí vlastníka a název repozitáře a nechá vás je potvrdit), řekne, kam kliknout, a každý krok si sám ověří. Hesla a přístupové klíče nikdy nepište do konverzace; zadáváte je jen vy sami, v prohlížeči nebo ve vlastním okně terminálu.

Zbytek návodu popisuje tytéž kroky pro případ, že si jimi chcete projít sami. Kroky 1–4 jsou na webu GitHubu, krok 5 na vašem počítači a od kroku 6 pracuje agent.

**Slovníček**

| Pojem | Co to je |
|---|---|
| Repozitář | složka vašeho projektu na GitHubu; drží všechny soubory a jejich historii |
| Hlavní větev (`main`) | platná verze projektu |
| Pull request (PR) | navržená změna, která čeká na vaše schválení, než se dostane do hlavní větve |
| Issue | zadání nebo úkol |
| Token | přístupový klíč, kterým agent pracuje ve vašem repozitáři |

Názvy tlačítek jsou anglicky, jak je GitHub zobrazuje. GitHub rozhraní občas upraví; pokud se název mírně liší, hledejte nejbližší podobný.

## Krok 1 – Založte repozitář

Pokud repozitář už existuje, přeskočte na krok 2. Agent se vás zeptá ANO/NE, zda existuje, protože u soukromého repozitáře to nelze spolehlivě ověřit.

1. Otevřete <https://github.com/new>.
2. **Owner:** vyberte svůj účet.
3. **Repository name:** napište název projektu bez mezer a diakritiky, například `my-project`.
4. Zvolte viditelnost: **Public** (vidí kdokoli) nebo **Private** (jen vy a pozvaní).
5. Zapněte přidání souboru README (**Add a README**). To je důležité: vznikne tím větev `main`, bez níž agent nemůže začít.
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
   - **Require a pull request before merging** – každá změna jde přes návrh. Nechte **Required approvals** na `0` a zaškrtněte **Dismiss stale pull request approvals when new commits are pushed**;
   - **Block force pushes** – historii nelze přepsat.
9. Dole klikněte na **Create**.

✅ Hotovo, když má seznam Rulesets položku `main` ve stavu **Active**.

## Krok 3 – Nastavte slučování

1. V **Settings** klikněte v levém menu na **General**.
2. V sekci **Features** ověřte, že je zaškrtnuté **Issues**.
3. Sjeďte dolů do sekce **Pull Requests** a ověřte, že je zaškrtnuté **Allow merge commits**.
4. Tamtéž zaškrtněte **Automatically delete head branches**. Pracovní větve se pak po schválení uklidí.

> Kroky 1–3 jsou jednorázové nastavení repozitáře. Agent je za vás může proklikat v **Chrome** se svým rozšířením (Claude / Codex); na Macu, kde je výchozí prohlížeč Safari, je automaticky proklikat nedokáže, takže nainstalujte Chrome, nebo je udělejte ručně.

## Krok 4 – Zvolte, jak agent přistupuje k repozitáři

Agent se potřebuje přihlásit, aby mohl pracovat ve vašem repozitáři. Vyberte možnost, která vám vyhovuje; agent nabídne obě a doporučí první.

| Možnost | Co znamená |
|---|---|
| **Omezený přístupový klíč (doporučeno)** | klíč, který platí jen pro jeden repozitář a jen pro činnosti, které povolíte. Agent nemůže měnit nastavení, smazat repozitář ani sáhnout na vaše ostatní projekty. Vytvoříte ho podle kroků níže. |
| **GitHub Desktop (nejjednodušší)** | jednou se přihlásíte v aplikaci [GitHub Desktop](https://desktop.github.com/download) a agent přes ni pracuje bez klíče. Jednodušší, ale agent pak má plná práva vašeho účtu ve všech vašich repozitářích — vědomý kompromis za pohodlí. |

Pokud zvolíte GitHub Desktop, nainstalujte ji, přihlaste se (**Settings → Accounts → Sign in**) a pokračujte krokem 5 (vytvoření klíče můžete přeskočit). Jinak vytvořte omezený klíč:

1. Klikněte vpravo nahoře na svůj profilový obrázek a zvolte **Settings**.
2. V levém menu úplně dole klikněte na **Developer settings**.
3. Klikněte na **Personal access tokens** a pak **Fine-grained tokens**.
4. Klikněte na **Generate new token**.
5. **Token name:** napište například `sdlc-ai-my-project`.
6. **Expiration:** zvolte dobu platnosti, například 90 dní. Po vypršení vytvořte stejným způsobem nový.
7. **Resource owner:** vyberte svůj účet.
8. **Repository access:** zvolte **Only select repositories** a vyberte repozitář z kroku 1.
9. **Permissions → Repository permissions:** nastavte přesně tyto tři položky, ostatní nechte být:

   | Položka | Nastavení | Proč to agent potřebuje |
   |---|---|---|
   | Contents | Read and write | ukládání změn souborů a publikování verzí |
   | Issues | Read and write | zakládání a uzavírání úkolů |
   | Pull requests | Read and write | navrhování změn a jejich sloučení po vašem schválení |

10. Klikněte na **Generate token** a klíč hned zkopírujte. GitHub ho ukáže jen jednou.

**Bezpečnost klíče**

- Uložte si ho do správce hesel. Neposílejte ho e-mailem ani zprávou a nevkládejte ho do repozitáře, úkolu ani do konverzace s agentem.
- Kdykoli ho můžete zneplatnit na téže stránce (**Fine-grained tokens**); agent okamžitě ztratí přístup.
- Protože klíč patří vašemu účtu, všechno, co agent udělá, bude na GitHubu vedené pod vaším jménem.

## Krok 5 – Připravte agenta na počítači

Agent pracuje s GitHubem přes program GitHub CLI (`gh`), který běží na vašem počítači.

1. Nainstalujte [Git](https://git-scm.com/downloads), [GitHub CLI](https://cli.github.com/) a [Python](https://www.python.org/downloads/). U každého stáhněte instalátor pro svůj systém a proklikejte ho s výchozími volbami.
2. Otevřete okno terminálu: na Macu aplikaci **Terminal**, na Windows **PowerShell**.
3. Přihlaste agenta. S **omezeným klíčem** z kroku 4 napište příkaz, stiskněte Enter, vložte klíč, znovu Enter a pak Ctrl+D (na Windows Ctrl+Z a Enter):

   ```bash
   gh auth login --with-token
   ```

   S **GitHub Desktop** jste už přihlášeni; pro příkazy `gh` jednou spusťte `gh auth login --web`.
4. Vyberte na počítači složku pro projekt a otevřete v ní svůj nástroj s agentem.

✅ Hotovo, když příkaz `gh auth status` vypíše váš účet (s omezeným klíčem token začínající `github_pat_`).

Klíč zadávejte jen v okně terminálu, nikdy do konverzace s agentem.

## Krok 6 – Dejte agentovi pokyn

Zkopírujte do konverzace s agentem tento text a doplňte hodnoty v lomených závorkách:

```text
Inicializuj nový projekt SDLC.AI podle AGENTS.md z repozitáře erikni/sdlc-ai-init.
Repozitář: https://github.com/<vas-ucet>/<nazev-projektu>
Verze metodiky: nejnovější vydaná
Jazyk komunikace: čeština
Web projektu: <adresa, nebo „zatím žádný">
Nasazení: <„Cloudflare", „lokální Docker", nebo „žádné">
Změny schvaluje: <vaše jméno>
```

Agent z adresy odvodí vlastníka a název repozitáře a nechá vás je potvrdit, pak se doptá na vše ostatní, co potřebuje (u Cloudflare, zda jde o Pages nebo Workers, aby odvodil náhledovou adresu).

## Co bude dál

1. Agent ověří, že má přístup k vašemu repozitáři. K instalaci metodiky navíc potřebuje přístup k privátnímu `erikni/sdlc-ai`. Pokud ten přístup nemáte, agent se zastaví a nahlásí `BLOCKED` s vysvětlením — privátní repozitář nikdy nečte ani neobchází. Požádejte vlastníka metodiky o přístup.
2. S přístupem **nalinkuje** metodiku do vašeho projektu: přidá jen `AGENTS.md` (vyplněný pro váš projekt) a poznámku v `README.md`; každý další dokument metodiky zůstává v `erikni/sdlc-ai` a odkazuje se na něj odkazem, takže pozdější aktualizace se k vám dostanou bez přepisování souborů. Přidá také sdílená workflow CI / preview / production jako tenké caller soubory.
3. Ověří, že je vše připravené: přístup, ochrana hlavní větve, nastavení a odkazy v dokumentech.
4. Pošle vám žádost začínající nadpisem **„Co schvalujete"**. Obsahuje odkaz na navrženou změnu, odkaz **Diff** (ukazuje přesně, co se mění) a seznam změněných souborů.
5. Pokud souhlasíte, odpovězte `SCHVÁLIT PR`. Pokud chcete něco jinak, napište `DOPLNIT ZADÁNÍ` a co se má změnit.

Po schválení je projekt připravený. Když agent narazí na překážku, napíše, co ho blokuje a co máte udělat.

## Jak zadávat práci

Napište agentovi, co chcete, a začněte velikostí úkolu:

```text
[TASK] Oprav překlep na úvodní stránce.
[STORY] Přidej přihlášení e-mailem a heslem.
[EPIC] Zákaznický portál – první verze.
```

| Typ | Kdy ho použít | Co budete schvalovat |
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

Pokud má projekt běžet na internetu, potřebujete navíc účet u poskytovatele (metodika má hotový postup pro Cloudflare) a uložení jeho přístupového klíče mezi secrets repozitáře: **Settings → Secrets and variables → Actions → New repository secret**. Sdílené production workflow ho pak načte podle jména. Tento krok je techničtější; doporučujeme ho udělat s někým zkušeným. Bez něj funguje vše ostatní, jen se výsledek sám nezveřejní.

Pokud chcete jen běžet na vlastním počítači bez účtu u poskytovatele, nastavte nasazení na lokální Docker a projekt poběží v lokálním Docker kontejneru.

Po schválení a sloučení změny proběhne nasazení automaticky. Agent počká na jeho dokončení — nejvýš 5 minut — a pak zkontroluje náhled nebo adresu vašeho webu. Pokud se nestihne dokončit nebo selže, agent to nahlásí, místo aby hádal.

Metodika nevyžaduje žádné webhooky ani další integrace.

## Když něco nefunguje

| Co vidíte | Co dělat |
|---|---|
| V repozitáři chybí záložka **Settings** | nejste přihlášeni účtem, který repozitář vlastní |
| Ruleset nejde zapnout nebo se neuplatní | repozitář je **Private** na free tarifu; změňte ho na **Public** (Settings → General → úplně dole **Change visibility**), nebo přejděte na placený tarif |
| Agent nedokáže nastavení proklikat v prohlížeči | automatika funguje jen v Chrome s rozšířením agenta; nainstalujte Chrome, nebo udělejte kroky 1–3 ručně |
| Agent hlásí, že nemá přístup | zkontrolujte repozitář vybraný v kroku 4 a tři oprávnění; klíč mohl také vypršet |
| Agent hlásí, že nemá přístup k metodice | repozitář `erikni/sdlc-ai` je privátní; požádejte jeho vlastníka o READ přístup. Veřejný bootstrap sám umožní začít, ale metodika se nainstaluje jen s přístupem |
| Agent hlásí `BLOCKED` | napíše, co ho blokuje a co to odblokuje; obvykle chybí nastavení z kroků 1–5, přístup k metodice, nebo nasazení neskončilo |
| Omylem jste klíč někam vložili | ihned ho zneplatněte na stránce **Fine-grained tokens** a vytvořte nový |

## Pro pokročilé

Technické detaily pro ty, kdo chtějí totéž udělat příkazy, nebo kdo pracují v týmu.

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

V týmu je vhodné, aby agent pracoval pod vlastním účtem: je vidět, co udělal agent a co člověk, a lze vyžadovat schválení od jiného účtu, než který změnu navrhl (**Required approvals** `1` v kroku 2).

- Omezený token (fine-grained) funguje jen pro repozitáře vlastněné samotným účtem nebo organizací, jíž je členem. Repozitář tedy musí patřit organizaci a účet agenta musí být jejím členem s rolí Write, ne Admin.
- Token se pak vytvoří pod účtem agenta stejně jako v kroku 4, s organizací jako **Resource owner**.
- Pokud místo rulesetu používáte klasickou ochranu větve, přidejte tokenu oprávnění **Administration: Read-only**, jinak ji agent nenačte a přestane slučovat.
- Má-li projekt automatické kontroly (CI), přidejte **Commit statuses** a **Actions** jako read-only.

</details>

Přesný, agentem vykonatelný postup inicializace je v [AGENTS.md](AGENTS.cs.md).
