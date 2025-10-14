---
title: Versnel je verkoopproces
description: Leer hoe u de verkoop kunt versnellen door documentervaringen te integreren met  [!DNL Adobe Acrobat Services]
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-10222
thumbnail: KT-10222.jpg
exl-id: 9430748f-9e2a-405f-acac-94b08ad7a5e3
source-git-commit: b7a20f30a2eb175053c7a25be0411f80dd88899f
workflow-type: tm+mt
source-wordcount: '1704'
ht-degree: 0%

---

# Versnel je verkoopproces

![&#x200B; Hoofdletterbanner van het Gebruik &#x200B;](assets/UseCaseAccelerateSalesHero.jpg)

Van whitepapers tot contracten en overeenkomsten, tijdens het aankooptraject zijn veel documenten nodig. In deze zelfstudie leert u hoe [[!DNL Adobe Acrobat Services] &#x200B;](https://developer.adobe.com/document-services/) documentervaringen tijdens dit traject kan integreren om de verkoop te versnellen.

## Overeenkomsten en verkooporders genereren op basis van gegevens

Verkoopovereenkomsten, contracten en andere documenten kunnen sterk variëren op basis van specifieke criteria. Een verkoopovereenkomst kan bijvoorbeeld alleen bepaalde voorwaarden bevatten die op een uniek criterium zijn gebaseerd, zoals in een bepaald land of een bepaalde staat, of bepaalde producten als onderdeel van de overeenkomst opnemen. Door deze documenten handmatig te maken of veel verschillende sjabloonvariaties te behouden, kunnen de juridische kosten voor het handmatig controleren van wijzigingen aanzienlijk toenemen.

[&#x200B; de Generatie API van het Document van de Adobe &#x200B;](https://developer.adobe.com/document-services/apis/doc-generation/) staat u toe om gegevens van uw CRM of ander gegevenssysteem te nemen om verkoopdocumenten dynamisch te produceren die op die gegevens worden gebaseerd.

## Referenties ophalen

Registreer eerst voor gratis Adobe PDF Services-gebruikersgegevens:

1. Navigeer [&#x200B; hier &#x200B;](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html) om uw geloofsbrieven te registreren.
1. Meld u aan met uw Adobe ID.
1. Stel uw aanmeldingsnaam in (bijv. Verkoopovereenkomsten demo).

   ![&#x200B; Screenshot van het plaatsen van uw geloofsbrieven naam &#x200B;](assets/accsales_1.png)

1. Kies een taal om uw voorbeeldcode te downloaden (bijvoorbeeld Node.js).
1. Controle om met **[!UICONTROL ontwikkelaarstermijnen]** akkoord te gaan.
1. Selecteer **[!UICONTROL creeer geloofsbrieven]**.
Er wordt een bestand naar uw computer gedownload met een ZIP-bestand met de voorbeeldbestanden pdfservices-api-credentials.json en private.key voor verificatie.

   ![&#x200B; Scherenshot van geloofsbrieven &#x200B;](assets/accsales_2.png)

1. Selecteer **[!UICONTROL krijgen toe:voegen-binnen Microsoft Word]** of ga naar [&#x200B; AppSource &#x200B;](https://appsource.microsoft.com/en-cy/product/office/WA200002654) om te installeren.

   >[!NOTE]
   >
   >Voor de installatie van de invoegtoepassing Word moet u toestemming hebben om invoegtoepassingen te installeren in Microsoft 365. Neem contact op met uw Microsoft 365-beheerder als u geen toestemming hebt.

## Uw gegevens

Als u gegevens uit een specifiek gegevenssysteem haalt, moet u die gegevens uitvoeren als JSON-gegevens of uw eigen schema genereren. In dit scenario wordt de volgende vooraf gemaakte set met voorbeeldgegevens gebruikt:

```
{
    "salesOrder": {
        "comment": "Make sure to call 555-555-1234 when you arrive. The front door is broken."
    },
    "company": {
        "name":"Home Services Co.",
        "address": {
            "city": "Homestead",
            "state": "NY",
            "zip": "14623",
            "streetAddress": "123 Demohome Street"
        }
    },
    "customer": {
        "address": {
            "city": "Seattle",
            "state": "WA",
            "zip": "98052",
            "streetAddress": "20341 Whitworth Institute 405 N. Whitworth"
        },
        "email": "mailto:jane-doe@xyz.edu",
        "jobTitle": "Professor",
        "name": "Jane Doe",
        "telephone": "(425) 123-4567",
        "url": "http://www.janedoe.com"
    },
    "tax": {
        "state":"WA",
        "rate": 0.08
    },
    "referencesOrder": [
        {
            "description": "Carpet Cleaning Service - 3BR 2BA",
            "totalPaymentDue": {
                "price": 359.54
            },
            "orderedItem": {
                "description": "Carpet Cleaning Service"
            }
        },
        {
            "description": "Home Cleaning Service - 3BR 2BA",
            "totalPaymentDue": {
                "price": 299.99
            },
            "orderedItem": {
                "description": "House Cleaning Service"
            }
        }
    ]
}
```

## Basiscodes toevoegen aan uw document

Dit scenario gebruikt een document van de Orde van de Verkoop, dat [&#x200B; hier &#x200B;](https://github.com/benvanderberg/adobe-document-generation-samples/blob/main/SalesOrder/Exercise/SalesOrder_Base.docx?raw=true) kan worden gedownload.

![&#x200B; Schermafbeelding van document van de steekproefverkooporde &#x200B;](assets/accsales_3.png)

1. Open het {*steekproefdocument 0} SalesOrder.docx in Microsoft Word.*
1. Als uw stop van de Generatie van het Document geïnstalleerd is, selecteer **[!UICONTROL de Generatie van het Document]** in het Lint. Als u het genereren van documenten niet ziet in uw lint, volgt u deze instructies.
1. Selecteer **[!UICONTROL worden begonnen]**.
1. Kopieer de JSON steekproefgegevens hierboven in het *JSON gebied van Gegevens* worden geschreven dat.

   ![&#x200B; Screenshot van het kopiëren van JSON- gegevens &#x200B;](assets/accsales_4.png)

Navigeer vervolgens naar het deelvenster Tags voor het genereren van documenten om codes in het document te plaatsen.

1. Selecteer de tekst die u wilt vervangen (b.v., *BEDRIJFSNAAM*).
1. In het *paneel van de Tagger van de Generatie van het Document*, onderzoek naar &quot;naam&quot;.
1. Selecteer een naam onder Bedrijf in de lijst met tags.
1. Selecteer **[!UICONTROL Tekst van het Tussenvoegsel]**.

   ![&#x200B; Screenshot van het opnemen van markering &#x200B;](assets/accsales_5.png)

   Tijdens dit proces wordt een tag met de naam `{{company.name}}` geplaatst, omdat de tag zich onder het pad in de JSON bevindt.

   ```
   {
   …
   "company": {
       "name":"Home Services Co.",
       …
   },
   …
   }
   ```

Herhaal deze handelingen voor enkele extra tags in het document, zoals STREET ADDRESS, CITY, STATE, ZIP enzovoort.

## Een voorvertoning van het gegenereerde document bekijken

Direct in Microsoft Word kunt u een voorvertoning van het gegenereerde document weergeven op basis van de JSON-voorbeeldgegevens.

1. In het *paneel van de Tagger van de Generatie van het Document*, uitgezocht **[!UICONTROL produceer document]**. De eerste keer dat u wordt gevraagd u aan te melden bij uw Adobe ID. Selecteer **[!UICONTROL Teken binnen]** en voltooi de herinneringen aan login met uw geloofsbrieven.

   ![&#x200B; Screenshot van hoe te om geproduceerd document &#x200B;](assets/accsales_6.png) voor te vertonen

1. Selecteer **[!UICONTROL het document van de Mening]**.

   ![&#x200B; Screenshot van het documentknoop van de Mening &#x200B;](assets/accsales_7.png)

1. Er wordt een browservenster geopend waarin u een voorvertoning van de documentresultaten kunt bekijken.

   ![&#x200B; Schermafbeelding van document in browser venster &#x200B;](assets/accsales_8.png)

U kunt de labels in het document zien die zijn vervangen door de gegevens uit de oorspronkelijke voorbeeldgegevens.

![&#x200B; Schermafbeelding van markeringen die met gegevens worden vervangen &#x200B;](assets/accsales_9.png)

## Een tabel toevoegen aan een sjabloon

In dit volgende scenario voegt u een lijst met producten toe aan een tabel in het document.

1. Plaats de cursor op de plaats waar de tabel moet worden geplaatst.
1. In het *paneel van de Tagger van de Generatie van het Document*, uitgezochte **[!UICONTROL Geavanceerd]**.
1. Breid **[!UICONTROL Lijsten en Lijsten uit]**.
1. In het *verslag van de Lijst* gebied, uitgezochte *referencesOrder*, dat een serie is die van alle productpunten een lijst maakt.
1. In het Uitgezochte gebied van kolomverslagen, type om *beschrijving* en *totalPaymentDue.price* gebied te omvatten.
1. Selecteer **[!UICONTROL lijst van het Tussenvoegsel]**.

   ![&#x200B; Screenshot van het opnemen van lijst &#x200B;](assets/accsales_10.png)

Bewerk de tabel om deze aan te passen aan stijlen, grootten en andere parameters, net als elke andere tabel in Microsoft Word.

## Numerieke berekening toevoegen

Met numerieke berekeningen kunt u sommen en andere berekeningen berekenen op basis van een verzameling gegevens, zoals een array. In dit scenario voegt u een veld toe om het subtotaal te berekenen.

1. Selecteer *$0.00* naast de titel van het subtotaal.
1. In het *[!UICONTROL paneel van de Tagger van de Generatie van het 0&rbrace; Document, breid]* Numerieke berekeningen **[!UICONTROL uit.]**
1. Onder *[!UICONTROL Uitgezochte berekeningstype]*, kies **[!UICONTROL Samenvoeging]**.
1. Onder *[!UICONTROL Uitgezochte type]*, kies **[!UICONTROL Som]**.
1. Onder *[!UICONTROL Uitgezochte verslagen]*, kies **[!UICONTROL ReferencesOrder]**.
1. Onder *[!UICONTROL &#x200B; Uitgezochte punt om samenvoeging &#x200B;] **&#x200B; uit te voeren, kies &#x200B;** [!UICONTROL totalPaymentsDue.price]**.
1. Selecteer **[!UICONTROL Berekening van het Tussenvoegsel]**.

Hierbij wordt een berekeningscode ingevoegd die de som van waarden bevat. U kunt geavanceerdere berekeningen maken met behulp van JSONata-berekeningen. Bijvoorbeeld:

* Subtotaal: `${{expr($sum(referencesOrder.totalPaymentDue.price))}}`
Berekent de som van referencesOrder.totalPaymentDue.price.

* BTW: `${{expr($sum(referencesOrder.totalPaymentDue.price)*0.08)}}`
Berekent de prijs en vermenigvuldigt met 8% om de belasting te berekenen.

* Totaal eind: `${{expr($sum(referencesOrder.totalPaymentDue.price)*1.08)}}`
Berekent de prijs en vermenigvuldigt met 1,08 om het subtotaal plus belasting te berekenen.

## Voorwaardelijke voorwaarden toevoegen

Met voorwaardelijke secties kunt u alleen een zin of alinea opnemen als aan een bepaalde voorwaarde is voldaan. In dit scenario wordt alleen een sectie opgenomen als deze overeenkomt met een bepaalde status.

1. In het document, vind de sectie genoemd *CALIFORNIA PRIVACY VERKLARINGEN*.
1. Selecteer de sectie met de cursor.

   ![&#x200B; Schermafbeelding van selectie &#x200B;](assets/accsales_11.png)

1. In de *[!UICONTROL Tagger van de Generatie van het Document]*, uitgezochte **[!UICONTROL Geavanceerd]**.
1. Breid **[!UICONTROL Voorwaardelijke inhoud]** uit.
1. In het *[!UICONTROL Uitgezochte verslagen]* gebied, onderzoek en selecteer **[!UICONTROL customer.address.state]**.
1. In het *[!UICONTROL Uitgezochte exploitant]* gebied, selecteer **=**.
1. Op het *[!UICONTROL gebied van de Waarde]*, type *CA*.
1. Selecteer **[!UICONTROL Voorwaarde van het Tussenvoegsel]**.

De sectie California wordt alleen weergegeven in het gegenereerde document als customer.address.state = CA.

Selecteer vervolgens de sectie voor WASHINGTON PRIVACY-INSTRUCTIES en herhaal de bovenstaande stappen en vervang de waarde CA door WA.

## Een dynamische afbeelding toevoegen

Met de API voor het genereren van documenten kunt u afbeeldingen dynamisch invoegen op basis van gegevens. Dit is handig wanneer u verschillende submerken hebt en logo&#39;s, portretafbeeldingen of afbeeldingen wilt wijzigen om ze relevanter te maken voor een bepaalde branche.

Afbeeldingen kunnen via een URL worden doorgegeven in de gegevens- of base64-inhoud. In dit voorbeeld wordt een URL gebruikt.

1. Plaats de cursor op de plaats waar u een afbeelding wilt invoegen.
1. In het *[!UICONTROL paneel van de Tagger van de Generatie van het Document]*, uitgezochte **[!UICONTROL Geavanceerd]**.
1. Breid **[!UICONTROL Beelden]** uit.
1. Op het *[!UICONTROL Uitgezochte markeringen]* gebied, kies **[!UICONTROL embleem]**.
1. Op het *[!UICONTROL Facultatieve afwisselende tekstgebied]*, verstrek een beschrijving (d.w.z., embleem). Hierbij wordt een tijdelijke aanduiding voor afbeeldingen ingevoegd die er als volgt uitziet:

   ![&#x200B; Screenshot van placeholder beeld &#x200B;](assets/accsales_12.png)

U wilt de afbeelding echter dynamisch instellen voor een afbeelding die zich al in de lay-out bevindt. Dat kunt u als volgt doen:

1. Klik met de rechtermuisknop op de ingevoegde voorlopige afbeelding.

   ![&#x200B; Screenshot van placeholder beeld &#x200B;](assets/accsales_13.png)

1. Selecteer **[!UICONTROL uitgeven de Tekst van Alt]**.
1. Kopieer in het deelvenster de tekst die er als volgt uitziet:
   `{ "location-path": "logo", "image-props": { "alt-text": "Logo" }}`
1. Selecteer een andere afbeelding in het document dat u dynamisch wilt maken.

   ![&#x200B; Screenshot van nieuw beeld in document &#x200B;](assets/accsales_14.png)

1. Klik op het beeld met de rechtermuisknop aan en selecteer **[!UICONTROL uitgeven de Tekst van Alt]**.
1. Plak de waarde in het deelvenster.

Bij dit proces wordt de afbeelding vervangen door een afbeelding in de logovariabele in de gegevens.

## Tags toevoegen voor Acrobat Sign

Met Adobe Acrobat Sign kunt u elektronische handtekeningen vastleggen op uw documenten. Acrobat Sign biedt een eenvoudige manier om velden binnen de webinterface te slepen en neer te zetten, maar u kunt ook de plaatsing van handtekeningen en andere velden regelen met een tekstcode. Met Adobe generatietag kunt u deze tekstlabelvelden eenvoudig plaatsen.

1. Navigeer naar de plaats waar een handtekening is vereist in het voorbeelddocument.
1. Plaats de cursor op de plaats waar de handtekening nodig is.
1. In het *[!UICONTROL paneel van de Generatie van het Document van de Adobe Tagger]*, selecteer **[!UICONTROL Adobe Sign]**.
1. Op *[!UICONTROL specificeer aantal ontvanger]* gebied, plaats het aantal ontvangers (in dit voorbeeld is het één).
1. Op het *[!UICONTROL gebied van Ontvangers]*, uitgezochte **[!UICONTROL Ondertekenaar-1]**.
1. In het *[!UICONTROL type van het Gebied]*, uitgezochte **[!UICONTROL Handtekening]**.
1. Selecteer **[!UICONTROL markering van de Tekst van Adobe Sign van het Tussenvoegsel]**.

Er wordt een label in het document ingevoegd.

![&#x200B; Screenshot van handtekeningmarkering in document &#x200B;](assets/accsales_15.png)

Acrobat Sign biedt verschillende andere typen velden die u kunt plaatsen, zoals datumvelden.

1. In het *type van het Gebied*, uitgezochte **[!UICONTROL Datum]**.
1. Plaats de cursor boven de datumlocatie in het document.
1. Selecteer **[!UICONTROL markering van de Tekst van Adobe Sign van het Tussenvoegsel]**.

![&#x200B; Screenshot van datummarkering in document &#x200B;](assets/accsales_16.png)

## Uw overeenkomst genereren

U hebt uw document nu gelabeld en bent klaar om te gaan. In deze volgende sectie wordt besproken hoe u een document kunt genereren aan de hand van de API-voorbeelden voor het genereren van documenten voor Node.js, maar deze werken in alle talen.

Open de pdfservices-node-sdk-samples-master die is gedownload toen u uw referenties registreerde. De bestanden pdfservices-api-credentials.json en private.key moeten in deze bestanden worden opgenomen.

1. Open een Terminal om gebiedsdelen te installeren gebruikend npm installeert.
1. Kopieer de sample data.json naar de map resources.
1. Kopieer de Word-sjabloon naar de bronnenmap.
1. Maak een nieuw bestand in de hoofdmap van de map samples met de naam generate-salesOrder.js.

```
const PDFServicesSdk = require('@adobe/pdfservices-node-sdk');
const fs = require('fs');
const path = require('path');

var dataFileName = path.join('resources', '<INSERT JSON FILE');
var outputFileName = path.join('output', 'salesOrder_'+Date.now()+".pdf");
var inputFileName = path.join('resources', '<INSERT DOCX>');

//Loads credentials from the file that you created.
const credentials =  PDFServicesSdk.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdfservices-api-credentials.json")
    .build();

// Setup input data for the document merge process
const jsonString = fs.readFileSync(dataFileName),
jsonDataForMerge = JSON.parse(jsonString);

// Create an ExecutionContext using credentials
const executionContext = PDFServicesSdk.ExecutionContext.create(credentials);

// Create a new DocumentMerge options instance
const documentMerge = PDFServicesSdk.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.PDF);

// Create a new operation instance using the options instance
const documentMergeOperation = documentMerge.Operation.createNew(options)

// Set operation input document template from a source file.
const input = PDFServicesSdk.FileRef.createFromLocalFile(inputFileName);
documentMergeOperation.setInput(input);

// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
.then(result => result.saveAsFile(outputFileName))
.catch(err => {
    if(err instanceof PDFServicesSdk.Error.ServiceApiError
        || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
        console.log('Exception encountered while executing operation', err);
    } else {
        console.log('Exception encountered while executing operation', err);
    }
});
```

1. Vervang `<INSERT JSON FILE>` door de naam van het JSON-bestand in /resources.
1. Vervang `<INSERT DOCX>` door de naam van het DOCX-bestand.
1. Om te lopen, gebruik Terminal om knoop uit te voeren generate-salesOrder.js.

Het uitvoerbestand moet zich in de /uitvoermap bevinden en het document moet correct zijn gegenereerd.

## Meer opties

Nadat het document is gegenereerd, kunt u aanvullende acties uitvoeren, zoals:

* Document beveiligen met een wachtwoord
* PDF comprimeren als er grote afbeeldingen zijn
* Elektronische handtekeningen vastleggen op het document

Als u meer wilt weten over enkele andere beschikbare handelingen, bekijkt u de scripts in de map /src in de voorbeeldbestanden. U kunt ook meer leren door de documentatie van de verschillende handelingen te bekijken.

## Aanvullende gebruiksscenario’s

Met de workflows voor digitale documenten van [!DNL Adobe Acrobat Services] kunt u een groot aantal onderdelen van een verkoopcyclus stroomlijnen:

* Gebruik Adobe PDF Embed API om whitepapers en andere inhoud in te sluiten op websites en om analyses te meten en te verzamelen over viewers
* Gebruik Acrobat Sign om elektronische handtekeningen vast te leggen op uw gegenereerde overeenkomsten
* Overeenkomstgegevens uit uw PDF-documenten extraheren met Adobe PDF Extract API

## Verder leren

Wil je meer leren? Bekijk enkele aanvullende manieren om [!DNL Adobe Acrobat Services] te gebruiken:

* Leer meer van [&#x200B; documentatie &#x200B;](https://developer.adobe.com/document-services/docs/overview/)
* Meer zelfstudies op Adobe Experience League bekijken
* Gebruik de voorbeeldscripts in de map /src om te zien hoe u PDF kunt gebruiken
* Volg [&#x200B; Blog van de Technologie van de Adobe &#x200B;](https://medium.com/adobetech/tagged/adobe-document-cloud) voor recentste uiteinden en trucs
* Abonneer aan [&#x200B; Clips van het Papier (de maandelijkse levende stroom) &#x200B;](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF) om over het automatiseren te leren gebruiken [!DNL Adobe Acrobat Services].
======
* Leer meer van [&#x200B; documentatie &#x200B;](https://developer.adobe.com/document-services/docs/overview/)
* Meer zelfstudies op Adobe Experience League bekijken
* Gebruik de voorbeeldscripts in de map /src om te zien hoe u PDF kunt gebruiken
* Volg [&#x200B; Blog van de Technologie van de Adobe &#x200B;](https://medium.com/adobetech/tagged/adobe-document-cloud) voor recentste uiteinden en trucs
* Abonneren op [&#x200B; Clips van het Document (de maandelijkse levende stroom) &#x200B;](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF) om over het automatiseren te leren gebruikend [!DNL Adobe Acrobat Services]
