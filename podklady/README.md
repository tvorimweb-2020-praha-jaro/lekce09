# [Tvořím Web od A do Z](https://github.com/czechitas/tvorim-web-a-z): Podklady pro 9. lekci

Krátký odkaz na stažení: [https://is.gd/webaz10](https://is.gd/webaz10)

## Příklady

- [Centrování prvků](priklady/01-centrovani)
- [Způsoby připojení CSS](#způsoby-připojení-css)
- [CSS Specificita](#selektory-ajejich-specificita)  
- [Pseudotřídy](priklady/03-pseudotridy)
  - [CSS Diner](https://flukeout.github.io/)
- [Pseudoelementy](priklady/04-pseudoelementy)
- [CSS custom properties](priklady/05-custom-properties)

---

## Způsoby připojení CSS

Zatím jsme se seznámili s připojením CSS jako externího souboru pomocí značky `link`. Je to asi nejběžnější způsob a v mnoha ohledech nejvýhodnější, ale je dobré znát i ostatní možnosti, protože i ty mají své použití. A přinejmenším se s nimi můžete setkat na jiných projektech. 

1. externí soubor nebo více souborů CSS pomocí tagu `link`
    Známe již celkem důvěrně, pouze poznamenáme, že lze takto připojit i více souborů CSS (jeden pro obecné styly písma  a barev, další pro konkrétní stránku), aby se zbytečně nestahovaly styly, které se na stránce nepoužijí.
    
    ```html
    <link rel="stylesheet" href="/styles/typography.css">
    <link rel="stylesheet" href="/styles/contact.css">
    ```

2. přímo v HTML prostřednictvím značky `style`
    Párová značka `style` vlastně vyznačí místo v HTML dokumentu, kam lze psát CSS. Takový to blok CSS kódu lze umístit kamkoli do dokumentu, včetně prvku `head`, což je nejběžnější užití toho zápisu. Zapisují se do něho například styly, které chceme mít na stránce vykresleny okamžitě (bez nutnosti čekat na načtení velkého souboru CSS), ale to již spíše zastaralý způsob.

    ```html
    <style>
    /* CSS komentář */
    body {
        max-width: 1600px;
    }

    p {
        margin-bottom: 1rem;
    }
    </style>
    ```

3. tzv. inline styly, přímo do otvírací značky prostřednictvím atributu `style`
    Někdy nemáme možnost zasáhnout do kódu jinak než tímto způsobem. Nejčastěji jsme k tomu nuceni při úpravě obsahu stránky přes nějaký redakční systém. Dále se tento způsob využívá při tvorbě HTML e-mailů.
    
    `<p style="max-width: 20rem; color: silver;">Text odstavce s maximální šířkou 20 rem a šedou barvou písma.</p> `

4. pomocí jazyka JavaScript
    Uvádíme pouze pro úplnost, protože s JavaScriptem jsme se dosud nesetkali.

Pozor, poslední dva způsoby nemohou využít plnou škálu možnosti CSS (například nezapíšete stavové styly pro `:hover` či `:focus`).

---

## Selektory a&nbsp;jejich specificita

Selektory používáme k&nbsp;zacílení konkrétní sady CSS pravidel na zvolený prvek (skupinu prvků) v&nbsp;HTML dokumentu. Musíme tedy mít jasno v&nbsp;tom, na který prvek/prvky se pravidla uplatní.

### Základní typy selektorů

#### Pomocí názvů tagů

- prostý název HTML tagu (v CSS bez špičatých závorek!)

```css
/* odstavce vypiš tučně */

p {
    font-weight: bold;
}
```

- vícenásobný selektor: tatáž pravidla se přiřadí více prvkům naráz
- zvolené prvky oddělujeme čárkou
- píšeme jeden selektor na řádek (kvůli přehlednosti)

```css
/* nadpisy 1.‒4 úrovně vypiš karmínovou */

h1,
h2,
h3,
h4 {
    color: crimson;
}
```

#### Pomocí atributů

- prvek s&nbsp;konkrétním atributem

```css
/* prvkům (tedy obrázkům) s atributem `alt` přidej spodní ohraničení */
[alt] {
    border: 1px solid silver;
}

```

- prvek s&nbsp;konkrétní hodnotou atributu

```css
/* obrázky s prázdným atributem `alt` orámuj karmínově */
[alt=""] {
    border-color: crimson;
}

```

#### Pomocí tříd

- název třídy (začíná tečkou)

```css
/* prvky s třídou .perex vypiš kurzívou */

.perex {
    font-style: italic;
}
```

### Specificita

Specificita je hodnota, která vyjadřuje přesnost zacílení daného selektoru (má číselnou hodnotu, více o tom v&nbsp;[článku Smashing Magazine](https://www.smashingmagazine.com/2007/07/css-specificity-things-you-should-know/#specificity-examples-test-yourself)).

**Pamatuj si, že pravidlo se vyšší specificitou se uplatní bez ohledu na pořadí v&nbsp;kódu.** Teprve střetnou-li se stejně „silná“ pravidla, vítězí to, které je ve stylopise později.

Porovnání se skutečným světem:

| **obleč si** | cokoli | svetr | její svetr | její velký svetr | její velký svetr, co máš v&nbsp;ruce* |
| **selektor** | `*` | `li` | `ul li` | `ul .nav-item` | `.nav > .nav-item` |

<small>* Jedná se o příměr, ber s&nbsp;rezervou</small>

#### Zanoření prvku

- naznačíme mezerou

```css

/* seznamu v navigaci (neplatí pro ostatní seznamy) odeber odrážky */

nav ul {
    list-style: none;
}

/* odkazy uvnitř prvků s&nbsp;třídou .perex (a jenom tam) vypiš chrpovou */

.perex a {
    color: cornflowerblue;
}

```

#### Přímý potomek

- předchozí pravdla lze zpřesnit (dát jim vyšší specificitu)

```css

/* odrážky se odeberou jen na první úrovni navigace odrážek, na zanořený seznam se pravidlo nevztahuje */

nav > ul {
    list-style-type: square;
}
```
```html
    <!-- Novinky a Starosti bez odrážek, ročníky s odrážkou -->
    <nav>
        <ul>
            <li><a href="novinky.html">Novinky</a></li>
            <li>
                <a href="starosti.html">Starosti</a>
                <ul>
                    <li><a href="2017">2017</a></li>
                    <li><a href="2016">2016</a></li>
                    <li><a href="2015">2015</a></li>
                </ul>
            </li>
        </ul>
    </nav>
```

Jiný příklad

```css
/* odkazy uvnitř prvků s třídou .perex, které jsou jeho přímým potomkem vypiš kaštanovou */

.perex > a {
    color: maroon;
}

```
```html
<div class="perex">
    <p>Text odstavce <a href="#">odkaz na zajímavý zdroj</a>, další text</p>
    <a href="#">odkaz na celý článek</a><!-- tento odkaz bude kaštanovou barvou, předchozí odkaz ne -->
</div>
```

#### Vícenásobné třídy nám umožní přesnější zacílení

- používat s&nbsp;rozumem, pokud nás prosazení pravidla nutí řadit více než tři třídy, bude vhodnější přidat třídu novou

```css
/* .perex, který má současně třídu .main vykresli se slonovinovým pozadím */

.perex.main {
    background-color: ivory;
}

```

- předchozí zápis podbarví tento prvek:

```html
<p class="perex main">Úvodní odstavec nějakého článku…</p>
```

- [Specificity calculator](https://specificity.keegan.st)
- [Specificity with Fish](https://specifishity.com)
- [CSS specificity Wars](https://stuffandnonsense.co.uk/archives/css_specificity_wars.html )

---

## Pseudotřídy

Názvy tříd jsou záležitostí kódéra. Nicméně CSS nabízí tzv. pseudotřídy, které mají jednak předem definovaný název a stejně tak definovaný selektor, na který zacílí. Pseudotříd je celá řada, a některé už dokonce znáš. Jsou to pseudotřídy, které se vážou ke stavu odkazu (elementu `<a>`):

- `:link` odkaz
- `:visited` navštívený odkaz
- `:hover` odkaz pod kurzorem myši
- `:active` právě klikaný odkaz (pouze dokud se drží levé myšítko)
- `:focus` vybraný element (nejen odkaz, ale třeba formulářové políčko, ve které je právě kurzor)

### Použití

```css
a:visited {
    color: currentColor; /* navstivenemu odkazu nastav barvu okolniho textu */
}

a:hover {
    text-decoration: none; /* po najeti mysi skryj podtrzeni */
}
```

**Poznámka**: nevěnuj příliš pozornosti nastavování pseudotřídy `:hover`. Je sice poměrně efektní, ale s rozmachem dotykových zařízení pozbyla významu, na dotykové obrazovce `:hover` není (zjednodušeně řečeno).

Další užitečné pseudotřídy jsou `:first-child` `:last-child`. Jak název napovídá, vážou se k pořadí prvku ve struktuře HTML, přesněji k jejich pozici uvnitř rodičovského prvku.

```css

p:first-child {
    font-weight: bold;
}

p:last-child {
    margin-bottom: 0;
}

```

Předchozí pravidla změní odstavce v následující HTML struktuře tak, jak je popsáno v jednotlivých odstavcích.

```html

<section>
    <p>Tento je první, takže se vypíše tučně.</p>
    <p>Tento odstavec je obyčejným odstavcem.</p>
    <p>Tento rovněž</p>
    <p>Tento odstavec je poslední, a tudíž má vynulovaný spodní okraj.</p>
</section>

```

Pseudotřídy jsou velmi užitečnou pomůckou, která nás zbavuje nutnosti zakládat kvůli každé dílčí úpravě novou třídu.

Z dalších přseudotříd věnuj pozornost těmto (subjektivní výběr):

- univerzální ‒ `:not`, `:nth-child(n)`, `:nth-of-type(n)`
- formulářové ‒ `:checked`, `:enabled`,`:disabled`
- vícejazyčné weby ‒ `:lang`

Samostudium na [Je čas od Bohumila Jahody](http://jecas.cz/css-selektory)

---

## Pseudoelementy `::before` a `::after`

Jedná se o virtuální prvek, který se nenachází ve struktuře HTML, ale na stránce to vypadá, jako by tam byl. Zní to dost abstraktně a možná se ptáš, proč něco takového chtít, proč vlastně takový prvek nevytvořit. Je to proto, že základním kóderským principem je oddělovat obsah od formy.* Tj. mít někde nějaký HTML element, který má posloužit jako oddělovač, nebo dekorativní prvek, a přitom nenese žádný obsahový význam, je lépe ho nahradit právě pseudoelementem a přenést ho tak, kam patří, tj. do CSS.

A teď prakticky.

```css

blockquote::before {
    content: '„'; /* bez teto vlastnosti se pseudoelement nezobrazi */
}

```

Před prvkem `blockquote` se nyní zobrazí spodní uvozovka. Tu můžeme libovolně ostylovat pomocí běžných CSS vlastností. **Podstatné je uvést vlastnost `content`.** Dokude se neuvede, pseudoelement jako by nebyl. Stačí i s prázdnou hodnotou:

```css

blockquote::after {
    content: '';
}

```

Pak je ale nezbytné doplnit pseudoelementu rozměry (`width/height`), aby se na stránce něco objevilo. A protože to je možné jen u elementů blokových (a pseudoelementy jsou řádkové), musíš ještě změnit typ elementu pomocí vlastnosti `display`. Celé po kupě to může vypadat nějak takto:

```css

blockquote::after {
    content: '';
    display: block;
    width: 100%;
    height: 30px
    background-image: linear-gradient(to right, #009245 33.3333%, #fff 33.3333%, #fff 66.66666%, #ce2b37 66.66666%); /* skutecene barvy italske vlajky */
}

```

Předchozí pravidla nám za prvkem `blockquote` vytvoří pruh přes celou šířku prvku, vysoký 30 px, který bude mít na pozadí pruhy italské vlajky (za inspiraci díky Lucii).

Často se pseudoelementy používají k doplňování ikon k tlačítku a podobným účelům. Vyšší dívčí jsou pak různé triky, z nichž nejpopulárnější je tzv. [clearfix, který ošetřuje trable s plovoucími elementy](https://css-tricks.com/snippets/css/clear-fix/). S nástupem [flexboxu](http://jecas.cz/flexbox) ovšem jeho význam a všudypřítomnost poklesly, takže není potřeba se s ním zabývat, dokud to nebude potřeba.

*<small>V praxi se to často porušuje a to i proto, že současně toho nelze 100% dosáhnout. Ale to přenechme vášnivým debatám na toto téma na diskuzních fórech.</small>
