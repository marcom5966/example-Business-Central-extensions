# Bank Rec. Description Mgt.

Een eenvoudige extensie voor Microsoft Dynamics 365 Business Central SaaS die transactieteksten in het betalingsreconciliatiedagboek automatisch verrijkt. Hierdoor worden banktransacties beter herkenbaar en neemt de kwaliteit van automatische afstemming toe.

## Probleem

Bij het importeren van bankafschriften via CSV-bestanden bevatten transacties vaak beperkte of cryptische omschrijvingen. Hierdoor kan Business Central transacties minder goed koppelen aan leveranciers, klanten of bestaande betalingsregels.

Voorbeelden:

| Oorspronkelijke tekst | Gewenste tekst |
|----------------------|----------------|
| 1447 Veldhoven | Action - 1447 Veldhoven |
| BCK*Warenhuis Witte Ve | Hema: BCK*Warenhuis Witte Ve |
| VELDHOVEN, 5508ME, NLD, 12:11 | Nuvoc kantine VELDHOVEN, 5508ME, NLD, 12:11 |

Deze extensie lost dit probleem automatisch op tijdens het importeren van banktransacties.

## Functionaliteit

De extensie luistert naar het `OnAfterInsertEvent` van de tabel **Bank Acc. Reconciliation Line**.

Na het aanmaken van een nieuwe transactieregel wordt:

1. De transactietekst gecontroleerd op bekende patronen.
2. De tekst indien nodig vervangen door een duidelijkere omschrijving.
3. De naam van de verwante partij toegevoegd wanneer deze nog niet in de omschrijving voorkomt.
4. De aangepaste transactietekst opgeslagen voordat de gebruiker begint met afstemmen.

## Ondersteunde herkenningen

### Action

Van:

```text
1447 Veldhoven
```

Naar:

```text
Action - 1447 Veldhoven
```

### HEMA

Van:

```text
BCK*Warenhuis Witte Ve
```

Naar:

```text
Hema: BCK*Warenhuis Witte Ve
```

### Verwante partij toevoegen

Wanneer een verwante partij aanwezig is, wordt deze automatisch toegevoegd aan het begin van de transactietekst.

Voorbeeld:

**Related Party Name**

```text
Belastingdienst
```

**Transaction Text**

```text
Teruggaaf Motorrijtuigenbelasting
```

Wordt:

```text
Belastingdienst Teruggaaf Motorrijtuigenbelasting
```

## Voordelen

- Hogere automatische herkenningsgraad
- Minder handmatige aflettering
- Betere werking van betalingsregels
- Meer inzichtelijke transactieteksten
- Eenvoudig uitbreidbaar met extra herkenningen
- Geen aanpassingen nodig in de standaard Business Central applicatie

## Installatie

1. Clone of download deze repository.
2. Open het project in Visual Studio Code.
3. Pas indien nodig de `app.json` aan.
4. Publiceer de extensie naar een Business Central Sandbox of Productieomgeving.
5. Installeer de extensie vanuit **Extensiebeheer**.

## Technische details

### Objecten

| Type | ID | Naam |
|--------|--------|--------|
| Codeunit | 50100 | Bank Rec. Description Mgt. |

### Gebruikte Event Subscriber

```al
OnAfterInsertEvent
```

Tabel:

```al
Bank Acc. Reconciliation Line
```

## Uitbreiden

Nieuwe herkenningsregels kunnen eenvoudig worden toegevoegd.

Voorbeeld:

```al
if StrPos(TransactionText, 'Jumbo Valkenswrd Waa') > 0 then
    NewTransactionText :=
        ReplaceText(
            TransactionText,
            'Jumbo Valkenswrd Waa',
            'Jumbo Valkenswaard');
```

Hierdoor kan de extensie worden aangepast aan de eigen administratie of bankformaten.

## Toekomstige uitbreidingen

Mogelijke uitbreidingen zijn:

- Configuratietabel met herkenningsregels
- Onderhoud van regels vanuit de Business Central gebruikersinterface
- Ondersteuning voor reguliere expressies
- Importspecifieke herkenningssets per bank
- Logging van toegepaste vervangingen

## Geschikt voor

- Business Central SaaS
- CSV-bankimporten
- Betalingsreconciliatiedagboek
- Automatische afstemming van banktransacties

## Licentie

MIT License

## Auteur

Marco Mels

## Blogartikel

Meer achtergrond over deze oplossing:

**Bankafschriften importeren via CSV in Business Central SaaS: slimme herkenning van transactieomschrijvingen voor betere automatische afstemming**

[Dit artikel](https://www.digitalemels.nl/banktransacties-importeren-via-csv/) beschrijft de aanleiding, technische werking en praktijkvoordelen van deze extensie.
