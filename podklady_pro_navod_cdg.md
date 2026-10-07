# Komplexní referenční příručka a podklady pro návod k systému Český discgolf (ČDG)
## Oficiální specifikace, pracovní postupy, systémová logika, pravidla a řešení praktických situací

Tento dokument představuje vyčerpávající a detailní referenční příručku pro systém **Český discgolf (ČDG)** na adrese `www.ceskydiscgolf.cz`. Slouží jako ucelený podklad pro tvorbu oficiálních uživatelských manuálů pro ředitele turnajů (TD), správce ligových soutěží, administrátory klubů i běžné hráče. Vychází z veškeré e-mailové komunikace zástupce asociace Jiřího Vaška, provozních deníků zpětné vazby (feedback OLC), technických specifikací Redmine a oficiálních pravidel soutěží České asociace discgolfu (ČADG).

---

## 1. Architektura systému, terminologie a dva základní provozní režimy turnajů

### 1.1 Oficiální terminologie a přístup k systému
* **Oficiální název platformy**: **ČDG (Český discgolf)**. Tento název nahrazuje dřívější označení iDG. V textech pro veřejnost i pro pořadatele se používá označení Český discgolf nebo ČDG.
* **Hlavní adresa**: `www.ceskydiscgolf.cz` (administrační rozhraní se nachází pod cestou `/idg/`).
* **Základní poslání systému**: Centralizovaná správa členské základny, klubů a hřišť v ČR, registrační portál na turnaje, nástroj pro řízení turnajů a webový Live Scoring, integrační můstek s mezinárodní asociací PDGA, výpočet Českého ratingu a automatizované vyhodnocování celostátních i regionálních ligových soutěží.

### 1.2 Dva odlišné provozní režimy turnajů
Systém striktně rozlišuje, zda má turnaj mezinárodní sankci od Professional Disc Golf Association (PDGA C / B / A / Major), nebo zda se jedná o tuzemský nesankcionovaný turnaj (ČDG turnaj). Každý režim má zcela odlišný pracovní postup (workflow):

```
+---------------------------------------------------------------------------------------+
|                                  REGISTRACE HRÁČŮ                                     |
|                      Vždy probíhá na webu www.ceskydiscgolf.cz                        |
+-------------------------------------------+-------------------------------------------+
                                            |
                        Má turnaj oficiální PDGA sankci (C / B / A)?
                       /                                           \
                     ANO                                           NE
                      |                                             |
                      v                                             v
+---------------------------------------+   +-------------------------------------------+
|          REŽIM PDGA TURNAJE           |   |          REŽIM ČDG TURNAJE                |
+---------------------------------------+   +-------------------------------------------+
| 1. V nastavení zadat PDGA ID turnaje  |   | 1. Před každým kolem vytvořit a nasadit   |
|    a heslo administrátora PDGA.       |   |    skupiny (flighty) v administraci ČDG.  |
| 2. Po uzavření registrací odeslat     |   | 2. Nastavit heslo pro Live Scoring.       |
|    hráče tlačítkem do PDGA Tournament |   | 3. Hráči zapisují výsledky přes webový    |
|    Manageru.                          |   |    Live Scoring na ceskydiscgolf.cz.      |
| 3. Skupiny (flighty) vytvářet         |   | 4. Ředitel sleduje a řídí kolo přes       |
|    VÝHRADNĚ v PDGA rozhraní.          |   |    Livescoring Dashboard (řeší DNF,       |
| 4. Do ČDG stáhnout skupiny pouze      |   |    penalizace +2 hody, maže karty).       |
|    pro náhled hráčům (needitovat!).   |   | 5. Kontrola stavu plateb a výsledků.      |
| 5. Skórování probíhá VÝHRADNĚ         |   | 6. Validační kontrola a administrativní   |
|    v aplikaci PDGA Live.              |   |    uzavření turnaje v administraci ČDG.   |
|    (Na ČDG se v tomto režimu          |   +-------------------------------------------+
|    vůbec neskóruje!).                 |
| 6. Po skončení turnaje stáhnout       |
|    výsledky a statistiky tlačítkem    |
|    z PDGA zpět do ČDG.                |
|    (PODMÍNKA: Na ČDG musí být předem  |
|    založen odpovídající layout!).     |
| 7. Administrativně uzavřít turnaj     |
|    v ČDG -> automatický přepočet      |
|    a zobrazení PDGA i CZ ratingů.     |
+---------------------------------------+
```

---

## 2. Založení a nastavení turnaje, editor a prezentační část

### 2.1 Struktura modulární administrace
Administrace turnaje je rozdělena do logických sekcí v levém menu:
1. **Základní údaje**: Název, termín, lokalita, přiřazené hřiště, organizující klub, status záštity ČADG, typ turnaje (jednotlivci vs. doubles).
2. **Struktura**: Vypsané kategorie (divize), kapacita jednotlivých kategorií a minimální počty hráčů.
3. **Registrace a fáze**: Časové milníky spuštění a konce jednotlivých registračních vln, podmínky přístupu (rating, členství).
4. **Finance a platby**: Výše základního startovného, slevy, volitelné položky (obědy, disky, Ace pool), bankovní spojení, variabilní symboly, platební QR kódy.
5. **Obsah a prezentace (Identita)**: WYSIWYG editor pro tvorbu propozic, nahrávání banneru, loga turnaje, správa modulárních bloků a sekce partnerů.
6. **Hráči a skupiny**: Správa přihlášek, schvalování divokých karet, rozřazení do flightů, Livescoring Dashboard, exporty.

### 2.2 WYSIWYG editor a modulární bloky
* **Modulární bloky**: Informace o turnaji se skládají z bloků, které lze přidávat, přetahovat a mazat.
* **Předdefinované systémové moduly**:
  * *Základní informace*: Souhrn termínů, lokality a ředitele.
  * *Harmonogram*: Časový rozvrh meetingu, výhozů jednotlivých kol a vyhlášení výsledků.
  * *Mapa hřiště a Caddy book*: Moduly určené pro nahrání plánku jamek a herního manuálu.
  * *Vlastní textový blok*: Libovolný obsah formátovaný textovým editorem.
* **Práce s dlouhými texty**: Pro přehlednost v administraci disponují bloky funkcí sbalení a rozbalení (Collapse / Expand), což šetří nutnost nekonečného scrollování.
* **Vkládání odkazů**: Při vkládání hypertextových odkazů systém automaticky validuje tvar URL. Není nutné ručně zdvojovat protokol `https://`.
* **Jazykové mutace**: Výchozí systémové nadpisy se v anglické verzi webu automaticky překládají do angličtiny (např. Schedule, Payment, Registration). Názvy vlastních bloků zadává ředitel podle zvoleného jazyka.

### 2.3 Bannery, loga a partneři turnaje
* **Bannery a loga**: V sekci Identita lze nahrát úvodní grafický banner a logo turnaje. V administraci je k dispozici tlačítko pro smazání nebo nahrazení obrázku.
* **Sekce Partnerů (sponzoři)**:
  * Systém podporuje rozdělení log partnerů do 3 velikostních úrovní: **S (Small)**, **M (Medium)** a **L (Large)**.
  * U každého loga lze zadat proklik na web partnera (systém vynucuje bezpečný protokol `https://`).
  * Bloky sponzorů lze v administraci libovolně řadit.

### 2.4 Zobrazení externích turnajů v kalendáři
* Pro turnaje pořádané v zahraničí nebo akce, jejichž registrace běží na externích platformách (Discgolfscene, PDGA), slouží režim externího turnaje.
* Turnaj se zobrazuje v celostátním kalendáři ČDG, ale namísto interní registrace ČDG obsahuje přímé přesměrování na externí registrační web.

---

## 3. Kategorie, ratingové stropy a pravidla rozřazování

### 3.1 Standardní amatérské kategorie (MA1 až MA4, FA1 až FA4)
Systém ČDG plně přebírá oficiální mezinárodní řád PDGA a pro standardní amatérské kategorie striktně vynucuje ratingové stropy. Kategorie MA (Mixed Amateur) a FA (Women's Amateur) mají rozdílné ratingové hranice zohledňující odlišnou distribuci výkonnosti:

* **Smíšené amatérské kategorie (Mixed Amateur – MA)**:
  * **MA1**: rating do 969 (strop 970 bodů pro amatérský status, doporučeno od 935)
  * **MA2**: rating do 934 (strop < 935)
  * **MA3**: rating do 899 (strop < 900)
  * **MA4**: rating do 849 (strop < 850, mezinárodní označení Novice)

* **Ženské amatérské kategorie (Women's Amateur – FA)**:
  * **FA1**: rating do 874 (strop 875 bodů pro amatérský status žen, doporučeno od 825)
  * **FA2**: rating do 824 (strop < 825)
  * **FA3**: rating do 774 (strop < 775)
  * **FA4**: rating do 724 (strop < 725, mezinárodní označení Novice)

Tyto ratingové hranice jsou v systému zavedeny natvrdo a ředitel turnaje je nemůže u kategorií nesoucích oficiální označení MA1–MA4 a FA1–FA4 individuálně měnit. Tím je zajištěno, že kategorie mají jednotnou váhu a význam napříč celou republikou i mezinárodně.

### 3.2 Barevné kategorie podle ratingu (Ratings-Based Divisions)
PDGA i systém ČDG umožňují vypsat turnaj v tzv. **Ratings-Based Divisions (barevné kategorie)**. Tyto kategorie rozdělují startovní pole výhradně podle aktuálního ratingu bez ohledu na věk či pohlaví:

| Barva (ČJ) | Barva (EN) | PDGA kód | Ratingový limit | Charakteristika |
|---|---|---|---|---|
| **Zlatá** | Gold | `RPA` | **970+** (bez horního limitu) | Profesionální úroveň (Pro) |
| **Modrá** | Blue | `RAH` | **< 970** (rating do 969) | Pokročilí hráči (ekvivalent MA1) |
| **Bílá** | White | `RAD` | **< 935** (rating do 934) | Středně pokročilí (ekvivalent MA2) |
| **Červená** | Red | `RAE` | **< 900** (rating do 899) | Rekreační hráči (ekvivalent MA3) |
| **Zelená** | Green | `RAF` | **< 850** (rating do 849) | Začátečníci (ekvivalent MA4) |
| **Fialová** | Purple | `RAG` | **< 800** (rating do 799) | Mírně pokročilí nováčci |
| **Žlutá** | Yellow | `RAY` | **< 750** / **< 700** | Úplní nováčci a děti |

* Pokud ředitel vypíše turnaj s PDGA sankcí v těchto oficiálních divizích (s kódy RPA až RAG), jedná se o standardní PDGA formát, který PDGA Tournament Manager plně podporuje.
* Všechny tyto názvy a kódy jsou v systému ČDG chráněny jako rezervované.

### 3.3 Vlastní kategorie (Custom divisions)
Pro regionální turnaje, neligové akce, klubové přebory nebo specifické začátečnické série umožňuje systém vytvoření vlastních kategorií:
* **Nastavitelné parametry**:
  * *Vlastní název*: Slovní pojmenování kategorie.
  * *Omezení pohlaví*: Otevřená kategorie pro všechny (Open/Mixed), nebo pouze pro ženy (Female-only).
  * *Maximální rating (Rating max)*: Libovolně stanovený ratingový strop (např. 820, 750 apod.).
* **Chování ratingové kontroly při registraci**:
  * Pokud je u vlastní kategorie zadán Rating max, systém tento strop **kontroluje automaticky při každé jednotlivé registraci**.
  * Systém neumí provést kontrolu ratingu pouze na prvním turnaji a následně hráči umožnit setrvat v kategorii celou sezónu bez ohledu na růst ratingu.
  * Pokud chce pořadatel umožnit hráčům setrvat v kategorii po celou sezónu i při zlepšení výkonnosti, **musí nechat pole pro Rating max prázdné** a rozřazování si řídit a hlídat sám.
* **Ochrana rezervovaných názvů a kódů**:
  * Systém aktivně blokuje pokusy o založení vlastní kategorie se jménem nebo kódem, který koliduje s oficiálními stálými kategoriemi ČADG / PDGA.
  * *Seznam rezervovaných hodnot*:
    * Mezinárodní divize: `MPO`, `FPO`, `MP40`–`MP80`, `FP40`–`FP80`, `MA1`–`MA4`, `FA1`–`FA4`, `MA40`–`MA80`, `FA40`–`FA80`, `MJ18`–`MJ06`, `FJ18`–`FJ06`.
    * České barevné názvy: *Zlatá*, *Modrá*, *Bílá*, *Červená*, *Zelená*, *Fialová*, *Žlutá*.
    * Anglické barevné názvy: *Gold*, *Blue*, *White*, *Red*, *Green*, *Purple*, *Yellow*.
    * PDGA kódy barevných kategorií: `RPA`, `RAH`, `RAD`, `RAE`, `RAF`, `RAG`, `RAY`.
  * Validace je nezávislá na velikosti písmen (case-insensitive) a ořezává mezery. Zabraňuje vzniku duplicitních kategorií v databázi.

### 3.4 Striktní zákaz vlastních kategorií na PDGA turnajích
* **Povinnost shody kategorií 1:1**: Každý turnaj pořádaný pod mezinárodní sankcí PDGA **musí mít vypsány výhradně oficiální PDGA kategorie** (oficiální věkové divize, výkonnostní amatérské divize MA/FA, případně oficiální barevné divize RPA až RAG).
* **Zákaz odlišných a lokálních kategorií**: Je přísně zakázáno vypisovat na PDGA turnaji v systému ČDG jakékoliv vlastní lokální kategorie, které neodpovídají PDGA řádu, nebo provozovat paralelní skórování v odlišných divizích.
* **Disciplinární odpovědnost**: Použití neoficiálních či neshodných kategorií představuje hrubé porušení soutěžního řádu PDGA i ČADG. Takové jednání vystavuje ředitele turnaje bezodkladnému disciplinárnímu řízení před Disciplinární komisí ČADG a hrozbě mezinárodních sankcí ze strany PDGA.
* **Jednotnost startovní listiny**: Kategorie v systému ČDG musí přesně a bez výjimky odpovídat kategoriím nastaveným v PDGA Tournament Manageru, aby byl zajištěn bezchybný přenos startovní listiny i import výsledků.

---

## 4. Registrační fáze, kapacity, divoké karty a odhlašování

### 4.1 Registrační fáze a časování (Pravidlo 20:00)
* **Časové spouštění fází**:
  * Všechny registrační fáze v systému ČDG standardně začínají a končí přesně ve **20:00 hodin** místního času.
  * Fáze jsou v systému navázány jako relativní počet dní před začátkem turnaje.
* **Odpočet do startu registrace**:
  * Na veřejném detailu turnaje běží grafický odpočet času (countdown).
  * V okamžiku dosažení nuly (20:00:00) se registrace otevře. Hráči na stránce doporučujeme provést obnovení stránky (F5), pokud se formulář nezpřístupní automaticky.
* **Pravidlo uzamčení konfigurace fází**:
  * **Jakmile se na turnaj zaregistruje první hráč, nastavení registračních fází se v administraci nevratně uzamkne.**
  * Jedná se o bezpečnostní pojistku zamezující nekalému posouvání fází uprostřed registrace.
  * *Postup nápravy při chybě v nastavení*: Pokud ředitel zjistí chybu v datech fází těsně po spuštění, musí prvního přihlášeného hráče z turnaje smazat, provést opravu termínů fází a hráče obratem zaregistrovat zpět.
* **Dědění ligových fází (Override)**:
  * Turnaje zapojené do ligy standardně přebírají jednotné ligové fáze.
  * Výkonná rada ČADG má pravomoc v odůvodněných případech povolit turnaji lokální výjimku (override) ligových fází.

### 4.2 Celková kapacita turnaje a divoké karty (Wild Cards)
* **Kapacitní blokace divokými kartami**:
  * Počet vyhrazených divokých karet v nastavení turnaje přímo blokuje celkovou kapacitu pro volnou registraci. Pokud má turnaj kapacitu 90 hráčů a 5 divokých karet, volná registrace se uzavře po přihlášení 85 hráčů.
* **Pracovní postup udělení divoké karty**:
  1. Hráč, který usiluje o divokou kartu, se přihlásí na turnaj běžnou cestou. Jelikož je volná kapacita naplněna, systém ho zařadí na Waiting List.
  2. Ředitel turnaje otevře v administraci seznam hráčů, vyhledá daného hráče na Waiting Listu a stiskne tlačítko pro **udělení divoké karty (Wild Card)**.
  3. Hráč je okamžitě přesunut z Waiting Listu do hlavní startovní listiny.
  4. V seznamech a při řazení se hráč s udělenou divokou kartou chová jako běžný platně registrovaný účastník.

### 4.3 Waiting List (Čekačka)
* Každý hráč přihlášený nad rámec volné kapacity je zařazen na Waiting List.
* Pořadí na Waiting Listu je striktně určeno časem odeslání přihlášky (s přesností na sekundy).
* Pokud se dříve přihlášený hráč z turnaje odhlásí, první náhradník z Waiting Listu se **automaticky neposouvá do turnaje**, pokud ředitel nepoužije funkci automatického doplňování nebo hráče nepřesune ručně.
* Ředitel má před začátkem turnaje možnost v administraci Waiting List vymazat/vyčistit, aby náhradníci nekomplikovali přípravu flightů.
* Při administrativním uzavření turnaje systém všechny zbývající čekající hráče z Waiting Listu automaticky vymaže.

### 4.4 Pravidla odhlašování hráčů z turnaje
* **Samostatné odhlášení hráčem**:
  * Ředitel v nastavení turnaje definuje časové okno (počet dní před turnajem), do kdy má hráč právo se z turnaje odhlásit sám přímo jedním kliknutím ve svém profilu.
* **Žádost o odhlášení po termínu**:
  * Po uplynutí nastaveného termínu tlačítko pro přímé odhlášení zmizí a nahradí ho formulář „Požádat o odhlášení“.
  * Hráč musí povinně vyplnit textový důvod odhlášení. Žádost je doručena řediteli turnaje, který hráče odhlásí ručně.
* **Neomluvená neúčast a disciplinární řád**:
  * Ředitelé turnajů nemají v systému k dispozici funkci vlastního banlistu a nesmí svévolně blokovat hráče.
  * Hráč, který se z turnaje neodhlásí a bez omluvy nedorazí, porušuje soutěžní řád. Ředitel turnaje řeší situaci podáním oficiálního podnětu Disciplinární komisi ČADG dle disciplinárního řádu.

---

## 5. Finance, záštita ČADG, startovné, platby a fakturace

### 5.1 Záštita ČADG a poplatky za nečleny
Systém rozlišuje status záštity turnaje a automaticky řídí cenotvorbu, tabulku startovného i odvody asociaci:

* **Turnaje se záštitou ČADG (ČADG 1, ČADG 2, ČADG 3, ČADG N)**:
  * *Poplatky za nečlena asociaci podle kategorie turnaje*:
    * **ČADG 1**: **100 Kč**
    * **ČADG 2**: **200 Kč**
    * **ČADG 3**: **300 Kč**
    * **ČADG N**: **100 Kč** (oficiální neligové akce, např. MČR dvojic)
  * *Zadávání startovného ředitelem*: Ředitel turnaje v administraci zadává **pouze normální základní cenu startovného** (určenou pro členy ČADG). Nemusí počítat ani zadávat žádné částky pro nečleny ručně.
  * *Automatická veřejná tabulka startovného*: Systém k základní částce automaticky připočte příslušný poplatek za nečlena podle typu turnaje a na veřejné stránce propozic vygeneruje přehlednou tabulku se **dvěma sloupci: Člen ČADG a Nečlen ČADG**. Tuto tabulku vidí všichni návštěvníci.
  * *Automatický platební QR kód*: Při registraci systém sám ověří, zda přihlášený hráč má platné členství v ČADG. Platební předpis i platební QR kód se každému uživateli vygeneruje na míru podle jeho reálného statusu (člen platí základní cenu, nečlen platí cenu navýšenou o poplatek).
  * *Automatický odvod asociaci při uzavření*: Při administrativním uzavření turnaje systém projde startovní listinu, sečte všechny zúčastněné nečleny ČADG a řediteli vygeneruje souhrnné vyúčtování poplatků za nečleny k odvodu asociaci ČADG.
* **Turnaje bez záštity ČADG**:
  * U turnajů bez záštity se generuje **poplatek 20 Kč za každého hráče za použití systému ČDG**.
  * *Osvobození od poplatku pro ČADG kluby*: Pokud je u turnaje jako organizátor uveden **oficiální klub ČADG**, je turnaj od tohoto poplatku 20 Kč za hráče **zcela osvobozen** (poplatek činí 0 Kč).

### 5.2 Dotace na startovné juniorů
* ČADG poskytuje u vybraných turnajů podporu juniorů formou dotace na startovné podle oficiálně vyhlášených podmínek podpory pro danou sezónu.
* **Parametry dotační podpory**: Výše dotace se řídí aktuálním usnesením ČADG pro daný rok (například pro aktuální sezónu činí sleva **50 %, maximálně však 400 Kč**).
* **Správa nastavení**: Volbu „Juniorská dotace“ smí v systému zapínat výhradně Výkonná rada ČADG.
* **Chování mechanismu a tabulka startovného**:
  * Pokud je tato volba u turnaje zapnuta, veřejná tabulka startovného na stránce propozic automaticky obsahuje **další sloupec uvádějící částku, kterou v dané kategorii platí junior**.
  * Dotace je vázána na **fyzický věk hráče (do 18 let podle data narození / rodného čísla)**, nikoliv na název vypsané kategorie.
  * Systém automaticky poníží předepsané startovné a vygeneruje odpovídající platební QR kód každému účastníkovi mladšímu 18 let, a to i v případě, že startuje v divizi dospělých (např. MA1 nebo MPO).

### 5.3 Stavy plateb a variabilní symboly
* **Dostupné stavy plateb v administraci**:
  * `Nezaplaceno` (výchozí stav po registraci)
  * `Zaplaceno (účet)` (platba připsána na bankovní účet pořadatele)
  * `Zaplaceno (hotovost)` (platba uhrazena na místě v hotovosti)
  * `Zaplatí na místě` (hráč má schválenou úhradu při ranní prezenci)
* **Logika generování Variabilního symbolu (VS)**:
  * Ředitel v nastavení platebních údajů turnaje zadává **prefix variabilního symbolu** (např. `202601`). Systém pak generuje variabilní symboly přihlášených hráčů s tímto prefixem (např. 1. hráč `202601001`, 2. hráč `202601002` atd.).
  * Zadání prefixu je povinné pro správné fungování párování plateb i generování platebních QR kódů.
  * Variabilní symbol je stabilní a neměnný – případné odhlášení jiných hráčů nesmí změnit platební symboly již přihlášených účastníků.
* **Podmínky pro zobrazení platebních QR kódů**:
  * Aby systém hráčům generoval a zobrazoval platební QR kód (standard SPAYD), musí ředitel v sekci *Finance a platby* splnit následující podmínky:
    1. **Aktivovat volbu „Zobrazovat QR kód“** (zaškrtnout zapnutí QR plateb).
    2. **Mít kompletně vyplněné bankovní údaje** (číslo bankovního účtu a kód banky).
    3. **Mít nastavený prefix variabilního symbolu**.
  * Bez splnění těchto tří náležitostí systém QR kód nevygeneruje.
  * Po splnění podmínek se QR kód s přesnou částkou a variabilním symbolem hráči zobrazuje na stránce turnaje ihned po přihlášení a zároveň se odesílá v potvrzovacím e-mailu.
* **Zahraniční platby**:
  * V administraci lze vyplnit pole „Majitel účtu“ (Account holder name), IBAN a SWIFT/BIC, což zahraničním hráčům usnadňuje zadání mezinárodní SEPA platby.

### 5.4 Deaktivace funkce „Vynucovat termín platby“
* Funkce, která měla automaticky vyřazovat neplatiče na Waiting List po uplynutí zadaného počtu dní od registrace (např. po 7 dnech), byla po testování v ostrém provozu **deaktivována a skryta z administrace**.
* *Důvody vyřazení*: Funkce působila komplikace těsně před startem turnaje, nevhodně zasahovala do registrací při částečně naplněné kapacitě a v některých prohlížečích narážela na síťové chyby.
* *Aktuální postup*: Kontrolu včasného zaplacení startovného provádí ředitel turnaje průběžně sám a neplatiče odhlašuje v administraci ručně.

---

## 6. Volitelné položky, formuláře a práce s datovými exporty

### 6.1 Volitelné položky (Finance) vs. Formuláře (po registraci)
Mezi těmito dvěma nástroji existuje zásadní koncepční rozdíl:

| Vlastnost | Volitelné položky (sekce Finance) | Doplňkové formuláře (po registraci) |
|---|---|---|
| **Hlavní účel** | Finanční evidence plateb navíc k základnímu startovnému | Zjišťování preferencí a organizačních dat od hráčů |
| **Typické využití** | Poplatek za Ace pool, koupě turnajového disku, fixní oběd | Výběr druhu jídla (vegetarián/maso), velikost trička |
| **Generování platby** | Vytváří fixní předpis částky a šablonu QR kódu | Nemá přímou vazbu na bankovní QR kód |
| **Kombinování položek** | Systém neumí dynamický košík; pro balíčky nutno vytvořit sloučenou položku (např. „Oběd + Pivo“) | Umožňuje libovolné zaškrtávací a výběrové možnosti |
| **Zpracování dat** | Ředitel si odškrtává úhradu v administraci plateb | Ředitel má k dispozici souhrnné odpovědi a statistiky |

### 6.2 Doplňkové formuláře u turnaje
* **Podmínka „Po registraci“**:
  * Zajišťuje, že se formulář hráči nabídne ihned po úspěšném odeslání přihlášky na turnaj.
  * Hráč nemusí provádět ruční refresh stránky, rozhraní ho přímo navede k vyplnění dotazníku.
* **Volba „Povinné“**:
  * Zamezí hráči dokončit přihlášku bez zodpovězení povinných dotazů (např. volba velikosti hráčského balíčku).

### 6.3 Exporty seznamu hráčů a správné otevření v MS Excel
* **Formát exportu**: Systém generuje exporty ve formátu CSV s mezinárodním kódováním znaků **UTF-8**.
* **Problém s MS Excel**: Pokud uživatel otevře CSV soubor ve Windows běžným dvojklikem, MS Excel jej může otevřít ve starém kódování Windows-1250 a české znaky se zobrazí jako nečitelné symboly („rozsypaný čaj“).
* **Správný postup importu do MS Excel**:
  1. Otevřete program MS Excel a založte nový prázdný sešit.
  2. V horní nabídce přejděte na kartu **Data**.
  3. Zvolte ikonu **Z textu/CSV** (Get Data -> From Text/CSV).
  4. Vyberte stažený soubor.
  5. V okně náhledu nastavte *Původ souboru (File Origin)* na **65001: Unicode (UTF-8)** a oddělovač na *Čárka* nebo *Středník*.
  6. Klikněte na **Načíst**. Tabulka se otevře s bezchybnou českou diakritikou.
* **Alternativní rychlý postup**: V systému stáhněte HTML export hráčů, otevřete jej v běžném internetovém prohlížeči, označte tabulku (Ctrl+A), zkopírujte (Ctrl+C) a vložte přímo do Excelu (Ctrl+V).

---

## 7. Skupiny (flighty), příprava kol a rozřazení

### 7.1 Příprava skupin pro kola
* Pro každé vypsané kolo turnaje musí mít ředitel v administraci vytvořené a nasazené skupiny. Bez existence skupin nelze Live Scoring spustit.
* **Typy startu**:
  * *Shotgun start*: Všechny skupiny startují ve stejný čas na různých jamkách (jamka 1A, 1B, 2 atd.).
  * *Postupný start (Tee times)*: Skupiny startují postupně z jamky č. 1 v časových intervalech (např. po 10 minutách).
* **Úprava času výhozu**: Čas výhozu zadaný u skupiny lze v administraci upravovat i po publikaci (např. při zpoždění z důvodu počasí).

### 7.2 Práce s rozhraním pro tvorbu flightů
* **Technologie drag-and-drop**: Hráči se do skupin a mezi jamkami přesouvají přetažením myší.
* **Draft skupin**: Rozpracované skupiny lze uložit jako neveřejný koncept (Draft). Veřejnosti na webu se flighty zobrazí až po stisknutí tlačítka publikovat.
* **Nasazování vedoucí skupiny (Leaders flight)**:
  * Při generování flightů do finálových nebo dalších kol podle průběžného pořadí musí být vedoucí skupina nejlepší kategorie nasazena **přednostně na jamku číslo 1**.

### 7.3 Tiskové sestavy flightů (PDF)
* **PDF export skupin**: Systém generuje formátovaný PDF přehled skupin vhodný pro tisk a vyvěšení na turnajovém meetingu.

---

## 8. Webový Live Scoring – Zapisování výsledků a statistiky

### 8.1 Přístup ke skórování
* Hráči ke skórování přistupují přímo přes internetový prohlížeč na adrese `ceskydiscgolf.cz`.
* Vstup do skórovací karty je chráněn **heslem turnaje**, které ředitel nastavil v administraci a sdělil hráčům na ranním meetingu.

### 8.2 Současný (paralelní) zápis více hráči
* Systém ČDG podporuje paralelní zápis: v rámci jedné skupiny může skóre zapisovat libovolný počet hráčů současně (ideálně zapisují všichni hráči ve flightu).
* Zadané hody se okamžitě synchronizují mezi zařízeními. Pokud dojde k neshodě v zadaném skóre, systém hráče upozorní.

### 8.3 Zápis hodů a pokročilé statistiky
Zapisovací rozhraní podporuje jak rychlý zápis celkového počtu hodů, tak detailní statistiky:
* **Penalizace na jedno kliknutí**:
  * `OB` (Out of Bounds)
  * `HAZ.` / `Hazard` (Hazard zóna)
  * `Mando` (Chybné minutí povinného směru)
  * `Jiná penalizace` (např. time violation, varování)
* **Statistika puttování v kruzích**:
  * *Kruh 1 (C1)*: Vzdálenost do 10 metrů od koše.
  * *Kruh 2 (C2)*: Vzdálenost 10 až 20 metrů od koše.
  * Zápis úspěšného C2 puttu funguje i v případě, že byl hod zahrán z drop zóny (dropky).
* **Zápis esa (Hole-in-one)**:
  * Při zápisu esa (hodnota 1) systém automaticky předvyplní vzdálenost hodu podle délky dané jamky uvedené v layoutu hřiště.
  * Pole je otevřené pro libovolnou číselnou hodnotu bez umělého omezení.

### 8.4 Sledování Live Leaderboardu
* Průběžné výsledky na webu jsou optimalizovány pro mobilní telefony i stolní počítače.
* Záhlaví výsledkové tabulky je fixované (sticky header), takže zůstává viditelné i při scrollování dolů mezi hráče na nižších pozicích.

---

## 9. Livescoring Dashboard pro ředitele turnaje

Livescoring Dashboard představuje centrální dispečink ředitele turnaje během probíhajícího kola. Ředitel zde vidí postup jednotlivých flightů a provádí nezbytné administrativní úkony.

### 9.1 Odstoupení hráče z kola (DNF)
Pokud hráč v průběhu kola nemůže pokračovat (zranění, nevolnost, časové důvody):
1. **Krok na hřišti**: Hráči na skórovací kartě odeberou odstupujícího hráče ze zápisu pro danou jamku a situaci neprodleně nahlásí řediteli turnaje.
2. **Krok ředitele v administraci**:
   * Ředitel otevře **Livescoring Dashboard**.
   * Vyhledá jméno odstupujícího hráče.
   * U daného kola mu nastaví oficiální status **DNF (Did Not Finish)**.
   * Systém hráče ve výsledcích korektně označí příznakem DNF a odsunuje ho na konec pořadí dané divize.

### 9.2 Udělení trestných hodů (Penalizace)
* Pokud dojde k porušení soutěžních pravidel (např. přeskočení jamky, pozdní příchod ke startu o méně než 5 minut apod.):
  * Ředitel v Livescoring Dashboardu vyhledá kartu daného hráče.
  * Vloží oficiální penalizaci (nejčastěji **+2 hody**). Trestné hody se přičtou k celkovému výsledku hráče za dané kolo.

### 9.3 Oprava chyb, mazání karet a odemykání skórování
Při vzniku nesrovnalostí v zápisu disponují ředitel a hráči odlišnými pravomocemi:
* **Přímá úprava skóre ředitelem**:
  * Pokud dojde k drobné chybě v počtu hodů nebo penalizaci na konkrétní jamce, ředitel může v Livescoring Dashboardu otevřít danou kartu a **přímo sám hodnoty přepsat, opravit a uložit**. Opravené skóre se ihned promítne do výsledků.
* **Smazání karty ředitelem**:
  * Ředitel v Livescoring Dashboardu karty neodemyká. Pokud je karta zapsána zcela chybně nebo došlo k zásadnímu zmatku, má ředitel možnost celou skórovací kartu v administraci **smazat**.
* **Odemčení odeslané karty hráčem**:
  * Pokud hráči kolo již potvrdili a odeslali a následně zjistí, že potřebují v zápisu provést změnu, **odemknout kartu pro další úpravy může sám hráč** přímo ve svém mobilním rozhraní Live Scoringu.

---

## 10. Kompletní pracovní postup pro PDGA sankcionované turnaje

Pořádání turnaje pod sankcí PDGA vyžaduje dodržení přesné posloupnosti kroků, aby nedošlo k poškození dat na PDGA ani v ČDG.

### 10.1 Fáze 1: Nastavení před turnajem
1. Ředitel získá od PDGA sankci turnaje, číselné **PDGA Tournament ID** a heslo správce (Event Password).
2. V administraci turnaje na ČDG vyplní tyto údaje do polí pro propojení s PDGA.
3. **Založení layoutu na ČDG**: Ředitel **musí** v administraci ČDG vytvořit stejný layout hřiště (stejný počet jamek a přesné pary), jaký bude hrán v PDGA Tournament Manageru. Bez vytvořeného layoutu na ČDG systém nedokáže po turnaji spárovat výsledky!

### 10.2 Fáze 2: Export hráčů po uzavření registrací
1. Registrace hráčů probíhá běžným způsobem na webu ČDG.
2. Po uzavření přihlášek ředitel stiskne tlačítko **„Odeslat hráče do PDGA“**.
3. Systém přenese startovní listinu do PDGA Tournament Manageru.
4. *Ochranné mechanismy ČDG při exportu*:
   * Hráči se shodným jménem v kategorii bez PDGA čísla jsou do PDGA automaticky doplněni rokem narození (např. Novák 99 a Novák 05).
   * **Varování**: Tlačítko exportu hráčů do PDGA již nikdy nemačkejte po zahájení turnaje ani po jeho skončení. PDGA API totiž při importu přepisuje kompletní startovku, což by smazalo rozpracované výsledky na PDGA.

### 10.3 Fáze 3: Skupiny a skórování
1. **Tvorba skupin**: Probíhá výhradně v rozhraní PDGA Tournament Manageru.
2. **Náhled na ČDG**: V administraci ČDG lze stisknout tlačítko pro stažení skupin z PDGA. Toto slouží čistě jako náhled pro české hráče, skupiny se na ČDG neupravují.
3. **Průběh hry**: Veškerý zápis skóre probíhá výhradně v aplikaci **PDGA Live**. Na webu ČDG se neskóruje.

### 10.4 Fáze 4: Import výsledků a ukončení turnaje
1. Po dohrání všech kol a kontrole výsledků na PDGA otevře ředitel administraci turnaje na ČDG.
2. Namísto tlačítka Livescoring Dashboard se zobrazuje tlačítko **„Stáhnout výsledky z PDGA“**.
3. Ředitel jedním kliknutím stáhne kompletní výsledky i statistiky. Proces trvá 10–30 sekund v závislosti na odezvě PDGA serverů.
4. Ředitel zkontroluje stažené výsledky na ČDG a provede administrativní uzavření turnaje.
5. Po uzavření systém automaticky stáhne a zobrazí oficiální PDGA ratingy jednotlivých kol.

---

## 11. Administrativní uzavření turnaje a validační blokace

Uzavření turnaje představuje finální krok turnajového cyklu, který spouští nevratné přepočty. Systém uplatňuje přísné validační mechanismy na ochranu integrity databáze.

### 11.1 Validační kontrola před uzavřením
Při pokusu o stisknutí tlačítka **„Uzavřít turnaj“** provede systém automatickou validační kontrolu:

1. **Kontrola výsledků hráčů (stávající podmínka)**:
   * Každý hráč na startovní listině musí mít zapsané platné výsledky pro **všechna vypsaná kola** turnaje, **NEBO** musí mít nastaven status **DNF**.
   * Pokud má některý hráč prázdná kola, systém uzavření nepovolí.
2. **Úhrada poplatků asociaci před uzavřením (připravovaná podmínka)**:
   * V systému se připravuje nová povinná validační podmínka: **ředitel turnaje musí před finálním uzavřením uhradit vygenerované poplatky asociaci** (poplatky za nečleny ČADG u turnajů se záštitou, resp. poplatek 20 Kč za hráče u turnajů bez záštity, pokud není pořadatelem ČADG klub).
   * Bez zaevidované úhrady poplatků systém nedovolí turnaj definitivně uzavřít a nespustí finální přepočty. Tím se zamezí vzniku neuhrazených závazků po skončení akce.

```
                           Pokus o uzavření turnaje
                                      |
                                      v
                  +---------------------------------------+
                  |  1. Kontrola hráčů na startovce:      |
                  |  Má každý hráč všechna kola nebo DNF? |
                  +-------------------+-------------------+
                                      |
                         +------------+------------+
                         |                         |
                        ANO                        NE
                         |                         |
                         v                         v
       +-----------------------------------+     +-----------------------------------+
       |  2. Kontrola úhrady poplatků:     |     |       BLOKACE UZAVŘENÍ            |
       |  (připravovaná podmínka)          |     | Chybí výsledky nebo DNF:          |
       |  Jsou uhrazeny poplatky asociaci? |     | 1. Nedohraným nastavit DNF.       |
       +-----------------+-----------------+     | 2. Nenastoupivší hráče SMAZAT!    |
                         |                       +-----------------------------------+
            +------------+------------+
            |                         |
           ANO                        NE
            |                         |
            v                         v
  +-------------------+     +-----------------------------------+
  |  TURNAJ UZAVŘEN   |     |       BLOKACE UZAVŘENÍ            |
  +-------------------+     | Chybí úhrada poplatků ČADG.       |
  | 1. Vyčištění WL.  |     | Ředitel musí nejprve vyrovnat     |
  | 2. Výpočet ratingů|     | poplatky vůči asociaci.           |
  |    a ligy.        |     +-----------------------------------+
  +-------------------+
```

### 11.2 Hráči, kteří k turnaji vůbec nenastoupili (Neúčast / DNS)
* Systém ČDG nepracuje se statusem DNS (Did Not Start).
* Pokud se hráč nedostaví na turnaj a neodehraje ani jedno kolo, **ředitel turnaje jej musí kompletně smazat / odebrat ze startovní listiny**.
* Pokud by takový hráč zůstal na startovní listině bez vyplněných kol, systém uzavření turnaje zablokuje.

### 11.3 Automatické akce vyvolané uzavřením turnaje
Jakmile turnaj projde validační kontrolou a je úspěšně uzavřen:
1. Systém automaticky a bezpečně vymaže všechny zbývající náhradníky z Waiting Listu.
2. U turnajů se záštitou ČADG se vygeneruje vyúčtování poplatků za nečleny.
3. Spustí se výpočet a aktualizace Českého ratingu pro všechny účastníky.
4. Výsledky se propíší do všech přiřazených ligových soutěží a proběhne automatický přepočet ligových tabulek.
5. Nastavení a parametry turnaje se uzamknou proti nechtěným zpětným zásahům.

---

## 12. Turnaje dvojic (Doubles) – Specifická pravidla

1. **Tým jako jedna nedělitelná entita**:
   * Dvojice vystupuje v celém systému jako jeden tým.
   * V rozhraní Live Scoringu na mobilu se jména obou hráčů zalamují pod sebe a zobrazují se siluety/fotografie obou účastníků se samostatnými odkazy na profily.
2. **Platba startovného**:
   * Startovné se v systému předepisuje a eviduje za celý tým dohromady. Systém nepodporuje dělení platby na jednotlivé hráče v páru.
   * *Automatické navýšení za nečleny*: Má-li turnaj dvojic záštitu ČADG (např. ČADG N), systém automaticky navýší startovné týmu v platebním QR kódu o 100 Kč za každého hráče v páru, který není členem ČADG (pokud jsou nečlenové oba, předpis startovného se automaticky zvýší o 200 Kč). Ředitel nic ručně nenastavuje.
3. **Prezence (Check-in)**:
   * Check-in se provádí za celý tým najednou. Ředitel provádí check-in teprve ve chvíli, kdy jsou na místě fyzicky přítomni oba hráči.
4. **Vypnutí ratingových limitů**:
   * U turnajů dvojic systém nekontroluje ratingové limity při registraci.
   * Sloupce osobních ratingů jsou ve výsledcích turnaje dvojic automaticky skryty, jelikož turnaje dvojic se do individuálního ratingu nezapočítávají.

---

## 13. Ligový modul a bodování soutěží

### 13.1 Bodování ČADG ligy
ČADG liga je dlouhodobá virtuální soutěž jednotlivců běžící automaticky na pozadí všech zapojených a uzavřených turnajů ČADG.
* **Virtuální kategorie podle Českého ratingu**:
  * Hráči startují na turnaji v reálných divizích (Open, Masters, Junioři apod.).
  * Do ligy se jejich body zapisují do virtuálních kategorií (**MA1 až MA4** pro muže, **FA1 až FA4** pro ženy) podle jejich **Českého ratingu platného v den daného turnaje**.
* **Výpočet bodů za turnaj**:
  * Body vycházejí z celkového absolutního pořadí turnaje podle vzorce:
    $$\mathbf{Body} = \mathbf{Koeficient\ turnaje} \times (\mathbf{počet\ poražených\ hráčů} + \mathbf{počet\ hráčů\ se\ stejným\ výsledkem})$$
  * Koeficienty: 1denní turnaj = 1×, 2denní turnaj = 2×, 3denní celostátní turnaj = 3×.
* **Pravidla pro PDGA PRO hráče**:
  * Amatérští hráči za poražení PRO hráče body vždy dostávají (startovní pole se nekrátí).
  * Do ligové tabulky sami nezískávají body hráči s aktivním statusem **PDGA PRO**, jejichž **PDGA rating** v den turnaje činí **970 a více** (muži), resp. **875 a více** (ženy). Kontroluje se výhradně PDGA rating, nikoliv Český rating.
* **Pravidlo pro rozhozy o trofeje (Dělená místa)**:
  * Rozhoz (play-off / sudden-death) slouží výhradně k určení držitele fyzického poháru či věcné ceny.
  * Pro ligové bodování mají oba hráči shodné herní skóre a v lize získávají **shodný plný počet bodů za dělenou pozici** (např. oba plný počet za 1. místo).
* **Vyhodnocení sezóny a Tie-break**:
  * Započítává se maximálně **13 nejlepších bodových zisků** v sezóně.
  * Hráč je vyhodnocen v té kategorii, ve které odehrál nejvíce turnajů z oněch 13 započítaných.
  * *Tie-break pravidlo*: Při shodě počtu turnajů ve dvou kategoriích (např. 2x FA2 a 2x FA1) je hráčka zařazena do **té vyšší kategorie** (FA1).

### 13.2 Specifická pravidla Amatérské ligy (ADGL)
* Srovnání probíhá ve 3 skupinách podle layoutu (Skupina 1: MA1–MA3 těžší layout, Skupina 2: MA4+MJ15 lehčí layout, Skupina 3: FA1–FA4+FJ15 ženský layout).
* Základní body se udělují sestupně od 100 bodů dolů v rámci skupiny.
* K základním bodům se přičítají bonusové body za pódium v kategorii (MA1–MA3: +6, +4, +2 body; ostatní: +3, +2, +1 bod).
* Mezi MA4 a vyššími kategoriemi (MA1–MA3) jsou body nepřenosné z důvodu odlišných layoutů. Hráč je při přestupu vyhodnocen ve dvou kategoriích.

### 13.3 Typy bodování v regionálních ligách
> **Poznámka (probíhající vývoj)**: Na možnostech a pravidlech bodování v regionálních ligách se v současné době aktivně pracuje. Níže uvedený přehled vychází ze současného návrhu a systémová logika se bude v nejbližší době dále upravovat a měnit.

Nový ligový modul ČDG aktuálně navrhuje či podporuje:
1. *Lineární sestupné bodování* (100, 99, 98...).
2. *Relativní bodování podle poražených soupeřů*.
3. *Bodovací stropy (Cutoff)* – body pouze pro top X hráčů (např. v Hraj Discgolf Tour body pouze pro top 15).
4. *Minimální počet hráčů (`minimum_players`)* – podmínka min. 3 hráčů v divizi pro udělení bodů.
5. *Ochrana neregistrovaných hráčů*: Systém předchází mezerovým chybám (Gap Errors) a počítá body i pro hráče bez plného uživatelského profilu v databázi.

---

## 14. Výpočet Českého ratingu, uživatelské účty, kluby a hřiště

### 14.1 Logika výpočtu Českého ratingu (ČDG rating)
Český rating reprezentuje objektivní herní úroveň hráče a počítá se podle těchto zásad:
1. **Stabilita ratingu při neaktivitě**: Pokud hráč nehraje, jeho rating se v čase **nikdy nesnižuje ani nenuluje** (ani po několika letech neaktivity). Hráč si trvale drží rating ze svého posledního odehraného turnaje.
2. **Klouzavé okno 12 měsíců**: Počítá se vždy zpětně od **data posledního odehraného turnaje daného hráče** (nikoliv od dnešního kalendářního dne).
3. **Překryv turnajů**: Pokud turnaj odehraný před 12 měsíci zasahuje do ročního okna, započítává se celý turnaj (všechna jeho kola).
4. **Dvojnásobná váha nejnovějších kol**: Nejnovějších 25 % odehraných kol v okně má ve výpočtu dvojnásobnou váhu (2×), stejně jako v metodice PDGA.
5. **Návrat po dlouhé pauze**: Při odehrání turnaje po více než 12 měsících se okno posune k novému turnaji a nový rating se spočítá z kol tohoto turnaje.

### 14.2 Uživatelské účty a rodné číslo
* Rodné číslo je vyžadováno u občanů ČR při registraci ze dvou důvodů:
  1. Automatické a přesné vyčtení pohlaví a data narození pro správné rozřazení do věkových kategorií.
  2. Zákonná povinnost evidence sportovců pro dotační tituly Národní sportovní agentury (NSA) u členů ČADG.
* Údaj není veřejně zobrazován. Každý uživatel si může po přihlášení změnit e-mail a heslo v editaci profilu.

### 14.3 Správa klubů a primární členství (`is_primary`)
* Statistiky klubu (počet členů, aktivních členů ČADG a průměrné ratingy) se počítají z členů, kteří mají klub označen jako primární (`is_primary = true`).
* Pokud má uživatel v systému pouze jeden klub, systém mu tento klub automaticky nastavuje jako primární.
* Správci klubu mají právo spravovat soupisku a vyřazovat neaktivní členy.

### 14.4 Zakládání nových hřišť
* Nové hřiště může v systému založit každý přihlášený člen s platným členstvím ČADG na adrese `https://ceskydiscgolf.cz/idg/hriste`.
* V databázi je hřiště dostupné ihned. Na veřejné mapě se zobrazí po denním přepočtu. Podmínkou je správné vyplnění zeměpisných souřadnic GPS.

---

## 15. Velký přehled reálných otázek, incidentů z provozu a odpovědí (FAQ)

### Dotazy ředitelů turnajů (TD FAQ)

**Otázka: Turnaj pod PDGA je řádně odehrán a uzavřen, ale na webu ČDG se u hráčů nezobrazují PDGA ratingy kol. Kde se stala chyba?**
* **Vysvětlení**: Turnaj buď nebyl v administraci propojen s PDGA tlačítkem „Propojit s PDGA“, nebo ještě nebyl administrativně uzavřen na ČDG. Pro zobrazení ratingů musí turnaj projít celým cyklem: odeslání hráčů do PDGA -> skórování na PDGA Live -> stažení výsledků do ČDG -> uzavření turnaje na ČDG. Teprve po uzavření se oficiální ratingy stáhnou a zobrazí.

**Otázka: U PDGA turnaje ve Smečně se po importu výsledků objevilo nesmyslné celkové skóre +110. Proč k tomu došlo?**
* **Vysvětlení**: Ředitel turnaje na webu ČDG před importem nevytvořil layout hřiště (počet jamek a pary). Importní algoritmus neměl data z PDGA na co napárovat a stáhl pouze hrubá čísla. Layout hřiště na ČDG musí být před stažením výsledků z PDGA vždy kompletně vyplněn.

**Otázka: Mohu si u vlastní kategorie nastavit maximální rating tak, aby ho systém zkontroloval jen při první registraci na začátku roku a nechal hráče ligu dohrát i po zlepšení ratingu?**
* **Vysvětlení**: Ne. Systém podmínku maximálního ratingu hlídá buď při každé jednotlivé registraci, nebo vůbec. Pokud chcete umožnit hráčům setrvat v kategorii po celou sezónu bez ohledu na růst ratingu, musíte nechat pole pro Rating max prázdné a rozřazení si hlídat sami.

**Otázka: Hráč si chce při registraci objednat startovné, oběd i turnajový disk. Jak mu vygeneruji jeden společný QR kód na míru?**
* **Vysvětlení**: Systém ČDG není e-shop s dynamickým nákupním košíkem. Vygenerované QR kódy představují fixní šablony pro jednotlivé položky. Pokud nabízíte zvýhodněný balíček služeb, vytvořte v sekci Finance samostatnou položku s pevnou souhrnnou cenou (např. „Startovné + Oběd + Disk“ za 700 Kč). Pro zjišťování konkrétních jídel či velikostí triček použijte modul Formuláře po registraci.

**Otázka: Co se stane, když se na PDGA turnaj přihlásí otec a syn se stejným jménem a ani jeden z nich nemá PDGA číslo?**
* **Vysvětlení**: Systém ČDG tyto hráče při exportu do PDGA Tournament Manageru automaticky odliší připojením roku narození za příjmení (např. Karel Zdeněk 75 a Karel Zdeněk 04). V systému ČDG zůstávají plně spárováni podle svých profilů.

**Otázka: Hráč v průběhu turnaje odstoupil ze hry kvůli zranění. Kde a jak mu nastavím DNF?**
* **Vysvětlení**: V administraci turnaje v sekci Livescoring Dashboard. Hráči ve flightu si odstupujícího hráče odeberou ze své skórovací karty na dané jamce a ředitel turnaje v Livescoring Dashboardu vyhledá jeho profil a nastaví mu oficiální status DNF.

**Otázka: Hráč se na turnaj přihlásil, ale vůbec nedorazil a neodehrál ani jedno kolo. Mám mu nastavit DNF?**
* **Vysvětlení**: Ne. Status DNF je určen výhradně pro hráče, kteří do turnaje nastoupili, ale nedokončili jej. Hráče, kteří k turnaji vůbec nenastoupili (neodehráli ani jamku), musí ředitel turnaje **zcela smazat ze startovní listiny**. Pokud by na startovce zůstali bez vyplněných kol, systém zablokuje uzavření turnaje.

**Otázka: Proč systém při uzavírání turnaje dvojic vygeneroval řediteli poplatek za nečleny ČADG?**
* **Vysvětlení**: Turnaj měl v nastavení status záštity ČADG N (poplatek 100 Kč za každého nečlena). Ředitel v propozicích nemusí nic ručně upravovat – systém sám automaticky zobrazuje navýšené startovné o 100 Kč za každého nečlena v tabulce i v platebním QR kódu při registraci. Vygenerovaný poplatek při uzavření turnaje pak představuje pouze souhrnný předpis k odvodu těchto částek asociaci, které ředitel od nečlenů vybral.

**Otázka: Stáhl jsem si export hráčů v CSV, ale v MS Excel mám místo českých písmen rozsypané znaky. Jak to napravit?**
* **Vysvětlení**: Soubor CSV neotevírejte dvojklikem. V MS Excel otevřete nový sešit, zvolte Data -> Získat data z textu/CSV, vyberte soubor a v okně náhledu nastavte kódování na 65001: Unicode (UTF-8). Alternativně stáhněte HTML verzi exportu a tabulku překopírujte.

**Otázka: Na turnaji se hrál rozhoz (play-off) o 1. místo. Vítěz dostal v lize 100 bodů, ale poražený pouze 86 bodů za 2. místo. Je to správně?**
* **Vysvětlení**: Není, jedná se o chybnou interpretaci v systému. Rozhoz slouží výhradně k určení držitele fyzického poháru. Pro ligové bodování dosáhli oba hráči shodného výsledku a v ligové tabulce mají mít oba zapsán plný počet bodů za dělené 1. místo (100 bodů).

**Otázka: Proč se doplňkový formulář nastavený „Po registraci“ hráči po přihlášení nezobrazil?**
* **Vysvětlení**: V dřívějších verzích frontendu byla chyba vyžadující ruční obnovení stránky (F5). Formulář nastavený „Po registraci“ se má hráči zobrazit v modálním okně okamžitě po potvrzení přihlášky.

### Dotazy hráčů a veřejnosti (Player FAQ)

**Otázka: Proč systém při zakládání účtu na ceskydiscgolf.cz vyžaduje rodné číslo?**
* **Vysvětlení**: Z rodného čísla systém automaticky a bezchybně vyčítá datum narození a pohlaví pro správné zařazení do věkových kategorií. U členů ČADG je rodné číslo navíc zákonnou podmínkou pro žádosti o sportovní dotace u Národní sportovní agentury (NSA). Údaj je neveřejný a zabezpečený.

**Otázka: Změnil jsem e-mailovou adresu a klub, jak provedu změnu v systému?**
* **Vysvětlení**: E-mail a heslo si může každý hráč změnit sám ve svém profilu po přihlášení (ikona profilu -> Upravit profil). Správu členství v klubu provádí určený správce daného klubu.

**Otázka: Založil jsem si účet, ale nepřišel mi aktivační e-mail. Co mám dělat?**
* **Vysvětlení**: Zkontrolujte složku Nevyžádaná pošta (Spam). Pokud e-mail nedorazil, použijte formulář pro obnovu zapomenutého hesla na přihlašovací stránce, který vám zašle nový odkaz pro nastavení hesla.

**Otázka: Po roční pauze jsem se přihlásil na web a můj Český rating ukazuje 0. Proč?**
* **Vysvětlení**: Šlo o systémovou chybu výpočtu, která počítala 12měsíční okno od dnešního dne místo od data posledního odehraného turnaje. Správně si hráč drží svůj rating trvale i po dobu neaktivity. Po nahlášení podpoře ČADG vám bude historický rating v databázi obnoven.
