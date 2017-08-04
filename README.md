# XML feed operativních leasingů Driveto.cz

XML feed umožňuje partnerům Driveto.cz zpřístupnit svá data pro automatickou aktualizaci nabídek operativního leasingu. 

Obchodní dotazy prosím směřujte na operaky@driveto.cz, technické dotazy na dev@driveto.cz (rádi poradíme!).

## Zpřístupnění feedu
Partner zpřístupňuje export v XML formátu na konkrétní URL, ze které si ho Driveto průběžně stahuje a aktualizuje podle něj data.

## Validace XML feedu

Připravili jsme pro vás [XSD schéma](leasing-import.xsd) pomocí kterého můžete ověřit, že vyexportované XML je ve správné struktuře a obsahuje validní hodnoty.


## Formát XML feedu
XML vypadá následovně (jednotlivé elementy jsou detailně popsány níže):

```xml
<?xml version="1.0" encoding="utf-8"?>
<leasing xmlns="urn:x-driveto.cz:leasing:feed:v1">
    <carOffers>
        <carOffer>
            <partnerCarOfferId>123456</partnerCarOfferId>
            <car>
                <carSpecification>
                    <make>Škoda</make>
                    <model>Rapid</model>
                    <modelLine>Rapid Spaceback</modelLine>
                    <modelLineYear>2017</modelLineYear>
                    <trim>Ambition Plus</trim>
                    <engineVolume>1.0</engineVolume>
                    <engineSuffix>TSI</engineSuffix>
                    <enginePowerKw>70</enginePowerKw>
                    <engineType>otto</engineType>
                    <is4x4>false</is4x4>
                    <isAutomaticTransmission>true</isAutomaticTransmission>
                </carSpecification>
                <drivetoUrl>
                    https://www.driveto.cz/skoda/rapid/rapid-spaceback-2017/1-0-tsi-70-kw-benzinovy-automaticka/ambition-plus/
                </drivetoUrl>
            </car>
            <color>barva podle výrobce</color>
            <targetLessee>business</targetLessee>
            <freeKm>5000</freeKm>
            <url></url>
            <trimInfo>
                <security>
                    <item>Ukazatel tlaku v pneumatikách</item>
                    <item>Výstražný trojúhelník a lékárnička</item>
                    <item>BMW Emergency Call</item>
                </security>
                <exterior>
                    <item>LED světlomety</item>
                </exterior>
                <interior>
                    <item>Navigace</item>
                </interior>
                <functionality>
                    <item>Přední a zadní parkovací asistent</item>
                </functionality>
            </trimInfo>
            <stockAvailability>
                <status>in_stock</status>
                <deliveryDate>2017-05-31</deliveryDate>
            </stockAvailability>
            <listPrice>
                <withVat>1004692</withVat>
                <vatRate>21</vatRate>
            </listPrice>
            <priceOffers>
                <priceOffer>
                    <package>Platinový program</package>
                    <mileageYearly>20000</mileageYearly>
                    <contractLength>24</contractLength>
                    <price>
                        <withoutVat>11268</withoutVat>
                        <vatRate>21</vatRate>
                    </price>
                    <downPayment>
                        <withoutVat>0</withoutVat>
                        <vatRate>21</vatRate>
                    </downPayment>
                    <tradeIn>false</tradeIn>
                </priceOffer>
                <priceOffer>
                    <package>Platinový program</package>
                    <mileageYearly>20000</mileageYearly>
                    <contractLength>36</contractLength>
                    <price>
                        <withoutVat>10844</withoutVat>
                        <vatRate>21</vatRate>
                    </price>
                    <downPayment>
                        <withoutVat>0</withoutVat>
                        <vatRate>21</vatRate>
                    </downPayment>
                    <tradeIn>false</tradeIn>
                </priceOffer>
            </priceOffers>
        </carOffer>
        <carOffer>
            <!-- další nabídka ve stejné struktuře -->
        </carOffer>
    </carOffers>
</leasing>
```

### `leasing`:
Kořenový element, v dokumentu je pouze jednou. Musí být v namespace `urn:x-driveto.cz:leasing:feed:v1`, jak je naznačeno v ukázkovém XML.

### `carOffers`:
Element, který zabaluje jednotlivé nabídky aut (jeho účelem je možnost rozšíření XML v budoucnu o další elementy na stejné úrovni).

### `carOffer`:
Nejdůležitější element, jde o nabídku na konkrétní automobil v konkrétní výbavě. Pokud konkrétní auto nabízíte na různá období na různé nájezdy, všechny budou v jedné `carOffer`.

### `partnerCarOfferId` (volitelné):
Vaše ID nabídky (pro snadnější komunikaci, případně aktualizaci nabídek) 

### `car`:
Slouží pro identifikaci automobilu v databázi Driveto.cz. 

Je možné použít tag `carSpecification` se specifikací automobilu nebo tag `drivetoUrl` s URL detailu automobilu v konkrétní výbavě v katalogu Driveto.cz (případně oboje).
 
#### `carSpecification`
- `make` - značka auta (`Škoda`)
- `model` - model auta (`Rapid`)
- `modelLine` - modelová řada (`Rapid Spaceback`, někdy se může shodovat s modelem - např. Škoda Fabia bude jen `Fabia`)
- `modelLineYear` - rok uvedení dané modelové řady (`2017`) - pro odlišení faceliftů
- `trim` - výbava auta (`Ambition Plus`, případně `Základní`, pokud není uvedeno)
- `engineVolume` - např. `1.0` nebo `3.6`
- `engineSuffix` - např. `TDI` nebo `TSI`
- `enginePowerKw` - výkon v kW
- `engineType` - typ motoru - jedna z hodnot `diesel`, `electric`, `gas`, `hybrid`, `otto`, `wankel` (benzínový je `otto`)
- `is4x4` (boolean): jde o 4×4?
- `isAutomaticTransmission` (boolean): má automatickou převodovku 

### `color` (nepovinné):
Barva automobilu podle definice výrobce.

### `targetLessee`:
Může danou nabídku využít jen firma či OSVČ (hodnota `business`) nebo kdokoliv (hodnota `anyone`).

### `freeKm`:
Povolené překročení nájezdu km (za celý pronájem dohromady).

### `url`:
URL nabídky na webu partnera.

### `trimInfo`:
Informace o výbavě automobilu. Může obsahovat následující sekce:

- `security`
- `exterior`
- `interior`
- `functionality`

Každá z nich může obsahovat jeden a více elementů `item` s názvy jednotlivých prvků výbavy.

### `stockAvailability`:
Obsahuje informace o dostupnosti.

#### `status`:
Může obsahovat hodnotu `in_stock` (skladem) nebo `on_order` (na objednávku).

#### `deliveryDate` (nepovinné):
Datum očekávaného dodání (např. `2017-05-31`). Pokud datum dodání není známé, tak neuvádět.

### `listPrice`:
Ceníková cena nového automobilu. Obsahuje jeden z elementů `withVat` nebo `withoutVat` a vždy `vatRate`.

### `priceOffers`:
V tomto elementu jsou obsaženy jednotlivé cenové nabídky (pro různé nájezdy a délky pronájmu).

### `priceOffer`:
Konkrétní cenová nabídka na konkrétní nájezd a dobu pronájmu.

#### `package`:
Název balíčku pro daný leasing (typicky je to něco jako `Zlatý program` nebo podobně).

#### `mileageYearly`:
Povolený roční nájezd v km.

#### `contractLength`:
Délka smlouvy v měsících.

#### `price`:
Měsíční splátka v CZK. Obsahuje jeden z elementů `withVat` nebo `withoutVat` a vždy `vatRate`.

#### `downPayment`:
Akontace v CZK. Obsahuje jeden z elementů `withVat` nebo `withoutVat` a vždy `vatRate`. Pokud je daná cenová nabídka bez akontace, uveďte prosím hodnotu `0`.

#### `tradeIn`:
Jde o protiúčet? `true` nebo `false`.
