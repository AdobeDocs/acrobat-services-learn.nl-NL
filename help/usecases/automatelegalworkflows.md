---
title: Automatiseer juridische workflows
description: Leer hoe je juridische workflows automatiseert met voorwaardelijke content
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-10202
thumbnail: KT-10202.jpg
exl-id: 2a1752b8-9641-40cc-a0af-1dce6cf49346
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '2826'
ht-degree: 0%

---

# Automatiseer juridische workflows

![&#x200B; Hoofdletterbanner van het Gebruik &#x200B;](assets/usecaseautomatelegalhero.jpg)

In een ideaal scenario worden de overeenkomstvoorwaarden zonder wijzigingen geaccepteerd. Vaak is echter aanpassing van overeenkomsten nodig, die dan juridisch moet worden herzien. Juridische recensies brengen aanzienlijke kosten met zich mee en vertragen het proces van het leveren van overeenkomstvoorwaarden. Met vooraf gedefinieerde sjablonen die worden gewijzigd op basis van goedgekeurde talen, kunnen juridische teams de overeenkomstvoorwaarden beheren en veiliger uitvoeren.

Deze zelfstudie maakt gebruik van een juridische overeenkomst die van staat tot staat verschilt. Om deze variaties te verhelpen, wordt een overeenkomstsjabloon met voorwaardelijke secties gemaakt, die alleen worden opgenomen wanneer aan bepaalde criteria wordt voldaan. Het gegenereerde document kan een Word- of PDF-document zijn. U kunt ook enkele manieren leren om uw document te beveiligen met de Adobe PDF Services API of Acrobat Sign.

## Referenties ophalen

Registreer eerst voor gratis Adobe PDF Services-gebruikersgegevens:

1. Navigeer [&#x200B; hier &#x200B;](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html) om uw geloofsbrieven te registreren.
1. Meld u aan met uw Adobe ID.
1. Stel uw referentie in.

   ![&#x200B; Screenshot van het plaatsen van uw geloofsbrieven naam &#x200B;](assets/automatelegal_1.png)

1. Kies een taal om uw voorbeeldcode te downloaden (bijvoorbeeld Node.js).
1. Controle om met **[!UICONTROL ontwikkelaarstermijnen]** akkoord te gaan.
1. Selecteer **[!UICONTROL creeer geloofsbrieven]**.
Er wordt een bestand naar uw computer gedownload met een ZIP-bestand met de voorbeeldbestanden pdfservices-api-credentials.json en private.key voor verificatie.

   ![&#x200B; Scherenshot van geloofsbrieven &#x200B;](assets/automatelegal_2.png)

1. Selecteer **[!UICONTROL krijgen toe:voegen-binnen Microsoft Word]** of ga naar [&#x200B; AppSource &#x200B;](https://appsource.microsoft.com/en-cy/product/office/WA200002654) om te installeren.

   >[!NOTE]
   >
   >Voor de installatie van de invoegtoepassing Word moet u toestemming hebben om invoegtoepassingen te installeren in Microsoft 365. Neem contact op met uw Microsoft 365-beheerder als u geen toestemming hebt.

## Uw gegevens

In dit scenario wordt informatie doorgegeven om het document te genereren en om te bepalen of bepaalde secties al dan niet moeten worden opgenomen:

```
{
    "customer": {
        "name": "Home Services Company",
        "street": "123 Any Street",
        "city": "Anywhere",
        "state": "CA",
        "zip": "12345",
        "country":"USA",
        "signer": {
            "email": "johnnyechostone@gmail.com",
            "firstName": "John",
            "lastName": "Echostone"
        }
    },
    "company": {
        "name": "Projected Consultants",
        "signer": {
            "email": "maryburostone@gmail.com",
            "firstName": "Mary",
            "lastName": "Burostone"
        }
    },
    "conditions": {
        "includeGeneralTerms": true,
        "includeConsumerDiscloure": true
    }
}
```

In de data staat informatie over de klant, zijn of haar naam, zijn of haar ondertekeningsstatus, hun status enzovoort. Daarnaast zijn er secties voor informatie over het bedrijf dat de overeenkomst- en voorwaardenmarkeringen genereert die worden gebruikt om bepaalde secties van de overeenkomst op te nemen.

## Basiscodes toevoegen aan uw document

Dit scenario gebruikt een document van de Voorwaarden en van de Voorwaarden, dat [&#128279;](https://github.com/benvanderberg/adobe-document-generation-samples/blob/main/Agreement/exercise/TermsAndConditions_Sample.docx?raw=true) kan worden gedownload.

![&#x200B; Schermafbeelding van het document van de Voorwaarden en van de Voorwaarden &#x200B;](assets/automatelegal_3.png)

1. Open het {*steekproefdocument 0} TermsAndConditions.docx in Microsoft Word.*
1. Als de [&#128279;](https://appsource.microsoft.com/en-cy/product/office/WA200002654) stop van de Generatie van het Document  geïnstalleerd is, selecteer **[!UICONTROL de Generatie van het Document]** in het Lint. Als u het genereren van documenten niet ziet in uw lint, volgt u deze instructies.
1. Selecteer **[!UICONTROL worden begonnen]**.
1. Kopieer de JSON-voorbeeldgegevens die hierboven zijn geschreven naar het veld JSON-gegevens.

   ![&#x200B; Screenshot van document en JSON gegevens &#x200B;](assets/automatelegal_4.png)

Navigeer aan het *paneel van de Tagger van de Generatie van het Document* om markeringen in het document te plaatsen.

## De bedrijfsnaam invoegen

1. Selecteer de tekst die u wilt vervangen. In dit scenario vervangt u de COMPANY in het openingsgedeelte van het document.
1. In *Tagger van de Generatie van het Document*, onderzoek naar &quot;naam&quot;.
1. Onder bedrijf, kies *naam*.

   ![&#x200B; Screenshot van het zoeken naar naam in Tagger van de Generatie van het Document &#x200B;](assets/automatelegal_5.png)

1. Selecteer **[!UICONTROL Tekst van het Tussenvoegsel]**.

Hiermee plaatst u een tag met de naam `{{company.name}}` omdat de tag zich onder dat pad bevindt in de JSON.

```
{
    "company": {
        "name": "Projected Consultants",
        ...
    }
    ...
}
```

Herhaal deze stap in de openingssectie voor de tekst KLANT. Herhaal **stappen 1-4**, die KLANT met &quot;naam&quot;onder klant vervangen. De uitvoer moet `{{customer.name}}` zijn, wat aangeeft dat de tekst afkomstig is van onder het klantobject.

Met de API voor het genereren van documenten in de Adobe kunt u ook codes toevoegen aan uw kop- en voetteksten en aan het einde waar de titels voor de handtekeningen moeten komen.

Herhaal dit proces opnieuw met **stappen 1-4** voor de BEDRIJF en tekst van de KLANT in de footer.

![&#x200B; Screenshot van het toevoegen van de markeringen van het BEDRIJF en van de KLANT in footer &#x200B;](assets/automatelegal_6.png)

Tot slot moet u stappen 1-4 **herhalen** om de NAAM VAN DE EERSTE EN NAAM VAN DE LAST onder de sectie van de Klant van de handtekeningspagina met de markeringen voor `{{customer.signer.firstName}}` en `{{customer.signer.lastName}}` respectievelijk te vervangen. Maak u geen zorgen als de tag lang is en opnieuw doorloopt naar de volgende regel omdat de tag wordt vervangen wanneer het document wordt gegenereerd.

Het begin van uw document en de voettekst moeten er ongeveer als volgt uitzien:

* Begin sectie:

![&#x200B; Screenshot van het begin sectie &#x200B;](assets/automatelegal_7.png)

* Voettekst:

![&#x200B; Screenshot van footer &#x200B;](assets/automatelegal_8.png)

* Handtekeningspagina:

![&#x200B; Schermafbeelding van handtekeningspagina &#x200B;](assets/automatelegal_9.png)

Nu uw labels in het document zijn geplaatst, kunt u een voorvertoning van de gegenereerde overeenkomst bekijken.

## Een voorvertoning van het gegenereerde document bekijken

Direct in Microsoft Word kunt u een voorvertoning van het gegenereerde document weergeven op basis van de JSON-voorbeeldgegevens.

1. In *Tagger van de Generatie van het Document*, uitgezocht **[!UICONTROL produceer document]**.
1. De eerste keer dat u wordt gevraagd u aan te melden bij uw Adobe ID. Selecteer **[!UICONTROL Teken binnen]** en voltooi de herinneringen aan login met uw geloofsbrieven.

   ![&#x200B; Schermafbeelding van het selecteren produceer documentknoop &#x200B;](assets/automatelegal_10.png)

1. Selecteer **[!UICONTROL het document van de Mening]**.

   ![&#x200B; Screenshot van het documentknoop van de Mening &#x200B;](assets/automatelegal_11.png)

1. Er wordt een browservenster geopend waarin u een voorvertoning van de documentresultaten kunt bekijken.

   ![&#x200B; Schermafbeelding van staat-specifieke tekst &#x200B;](assets/automatelegal_12.png)

## Voorwaardelijke voorwaarden toevoegen voor elk frame

In deze volgende sectie stelt u op basis van bepaalde criteria voor invoergegevens alleen bepaalde secties in die moeten worden opgenomen. In het voorbeelddocument hebben de secties 4 en 5 alleen betrekking op een specifieke status. Voor dit scenario zouden alleen de state-specific termijnen moeten worden opgenomen wanneer een klant in die staat verblijft. Ook de nummering in Microsoft Word mag die sectie niet bevatten als deze wordt verwijderd. Gebruik de voorwaardelijke inhoudsfunctie van de API voor documentgeneratie om dit te coderen.

![&#x200B; Schermafbeelding van staat-specifieke tekst &#x200B;](assets/automatelegal_13.png)

![&#x200B; Screenshot van het selecteren van de Californische sectie van de Bekendmaking &#x200B;](assets/automatelegal_14.png)

1. Selecteer in het document de sectie California Disclosure en alle subopsommingstekens.

   ![&#x200B; Screenshot van de voorwaardelijk-sectietag &#x200B;](assets/automatelegal_15.png)

1. In *[!UICONTROL Tagger van de Generatie van het Document]*, uitgezochte **[!UICONTROL Geavanceerd]**.
1. Breid **[!UICONTROL Voorwaardelijke inhoud]** uit.
1. In *[!UICONTROL Uitgezochte verslagen]* gebied, onderzoek, en selecteer **[!UICONTROL customer.state]**.
1. In *[!UICONTROL Uitgezochte exploitant]* gebied, selecteer **=**.
1. In *[!UICONTROL gebied van de Waarde]*, type *CA*.
1. Selecteer **[!UICONTROL Voorwaarde van het Tussenvoegsel]**.

De sectie bevat nu enkele tags die voorwaardelijke sectietags worden genoemd. Als u de labels hebt toegevoegd, is mogelijk de code voor de voorwaardelijke sectie toegevoegd als een genummerde regel. U kunt dit verwijderen door een backspacing voor de tag te maken. Als u dit niet doet, worden de items genummerd alsof de tag er niet was toen het document werd gegenereerd. De sectie die voorwaardelijk is, eindigt met de tag `{% end-section %}` .

![&#x200B; Screenshot van de voorwaardelijk-sectietag &#x200B;](assets/automatelegal_16.png)

**herhaal stappen 1-7** voor de *Disclosure van Washington* sectie, die de *waarde van CA* met *WA* vervangt om te vertegenwoordigen dat de sectie slechts wordt getoond als de staat van de klant Washington is.

![&#x200B; Screenshot van de voorwaardelijk-sectietag voor WA &#x200B;](assets/automatelegal_17.png)

## Testen met voorwaardelijke secties

Zodra uw voorwaardelijke secties op zijn plaats zijn, kunt u voorproef uw document door **te selecteren produceert document**.

Wanneer u uw document genereert, ziet u dat de opgenomen sectie alleen de sectie is die aan de gegevenscriteria voldoet. In het onderstaande voorbeeld is alleen de sectie Californië opgenomen, omdat de status gelijk was aan CA.

![&#x200B; Screenshot van de informatie van de Bekendmaking Californië &#x200B;](assets/automatelegal_18.png)

Een andere opmerkelijke verandering is dat de nummering voor de volgende sectie, Gebruik van Services en Software, het nummer 5 heeft. Dit betekent dat als de sectie Washington wordt weggelaten, de nummering wordt voortgezet.

![&#x200B; Schermafbeelding van verdere nummering &#x200B;](assets/automatelegal_19.png)

Om te testen of het malplaatje correct gedraagt wanneer de klant in de staat van Washington eerder dan Californië is, verander de steekproefgegevens voor het malplaatje:

1. In *Tagger van de Generatie van het Document*, uitgezocht **[!UICONTROL geef inputgegevens]** uit.

   ![&#x200B; Schermafbeelding van Tagger van de Generatie van het Document &#x200B;](assets/automatelegal_20.png)

1. Selecteer **[!UICONTROL geef]** uit.

1. In de gegevens JSON, verander *CA* in *WA*.

   ![&#x200B; Screenshot van JSON gegevens &#x200B;](assets/automatelegal_21.png)

1. Selecteer **[!UICONTROL produceer Markeringen]**.
1. Selecteer **[!UICONTROL Genereer document]** om het document opnieuw te genereren.

U ziet dat het document alleen de sectie met de staat Washington bevat.

![&#x200B; Schermafbeelding van document dat slechts de de staatssectie van Wasington omvat &#x200B;](assets/automatelegal_22.png)

## Een voorwaardelijke zin toevoegen

Net als voorwaardelijke secties kunt u ook specifieke zinnen hebben die worden opgenomen wanneer aan bepaalde voorwaarden wordt voldaan. In dit voorbeeld verschilt het retourbeleid tussen Californië en Washington.

1. Selecteer in sectie 3.1 de eerste zin &quot;Bij aankoop in de staat Washington moet er binnen 30 dagen na de oorspronkelijke transactie een via MAIL worden geretourneerd voor een volledige terugbetaling.&quot;
1. In *[!UICONTROL Tagger van de Generatie van het Document]*, uitgezochte **[!UICONTROL Geavanceerd]**.
1. Breid **[!UICONTROL Voorwaardelijke inhoud]** uit.
1. Onder *[!UICONTROL type van Inhoud]*, uitgezochte **[!UICONTROL Woorden]**.
1. In *[!UICONTROL Uitgezochte verslagen]* gebied, onderzoek, en selecteer **[!UICONTROL customer.state]**.
1. In *[!UICONTROL Uitgezochte exploitant]* gebied, selecteer **=**.
1. In *[!UICONTROL gebied van de Waarde]*, type *CA*.
1. Selecteer **[!UICONTROL Voorwaarde van het Tussenvoegsel]**.

Hoewel de naam van de tag gelijk is, is het belangrijkste verschil tussen Woorden en Sectie dat de sectie geen nieuwe regels bevat. Het label voor de voorwaardensectie en de sectie -end moeten zich in dezelfde alinea bevinden.

![&#x200B; Screenshot van woordmarkering &#x200B;](assets/automatelegal_23.png)

## Tags toevoegen voor Acrobat Sign

Met Acrobat Sign kunt u overeenkomsten ter ondertekening verzenden of insluiten in de webervaring, zodat iemand deze eenvoudig kan bekijken en ondertekenen. Met de Adobe Document Generation Tagger in Microsoft Word kunt u eenvoudig pre-taggen toepassen op documenten voordat deze worden verzonden met Acrobat Sign. Handtekeningen worden daarom altijd op de juiste locatie geplaatst. In dit scenario zijn er twee ondertekenaars die een plaats nodig hebben om het document te ondertekenen en te dateren.

1. Navigeer naar de plaats waar de klant moet ondertekenen.
1. Plaats de cursor op de plaats waar de handtekening moet komen.

   ![&#x200B; Screenshot van waar de handtekening moet gaan &#x200B;](assets/automatelegal_24.png)

1. In *[!UICONTROL Tagger van de Generatie van het Document]*, uitgezochte **[!UICONTROL Adobe Sign]**.
1. In *[!UICONTROL specificeer aantal ontvanger]* gebied, plaats het aantal ontvangers (dit voorbeeld gebruikt 2).
1. In *[!UICONTROL Ontvangers]* gebied, uitgezochte **[!UICONTROL Ondertekenaar-1]**.
1. In *[!UICONTROL het type van het Gebied]*, uitgezochte **[!UICONTROL Handtekening]**.
1. Selecteer **[!UICONTROL markering van de Tekst van Adobe Sign van het Tussenvoegsel]**.

   ![&#x200B; Screenshot van de Markering van de Tekst van het Tussenvoegsel Adobe Sign in Tagger van de Generatie van het Document &#x200B;](assets/automatelegal_25.png)

>[!NOTE]
>
>Als de **knoop van de Markering van de Tekst van Adobe Sign van het Tussenvoegsel** lijkt te ontbreken, scrol neer.

Hiermee wordt een handtekeningveld geplaatst waarin de eerste ondertekenaar moet ondertekenen.

![&#x200B; Screenshot van handtekeningtekstmarkering &#x200B;](assets/automatelegal_26.png)

Plaats vervolgens een gegevensveld voor de ondertekenaar die automatisch invult wanneer deze ondertekent.

1. Plaats de cursor op de plaats voor de datum.

   ![&#x200B; Screenshot van waar de datum zou moeten worden gevestigd &#x200B;](assets/automatelegal_27.png)

1. Stel het veldtype in op Datum.
1. Selecteer **[!UICONTROL markering van de Tekst van Adobe Sign van het Tussenvoegsel]**.

De tag Date die wordt geplaatst, is tamelijk lang: `{{Date 3_es_:signer1:date:format(mm/dd/yyyy):font(size=Auto)}}` . De Acrobat Sign-tekstcode moet op dezelfde regel blijven staan, anders dan de labels voor het genereren van documenten. De parameters `:format()` en `font()` zijn optioneel, dus voor dit scenario kunnen we de tag verkorten tot `{{Date 3_es_:signer1:date}}` .

Herhaal de stappen boven de *sectie van de Handtekening van het Bedrijf*. Wanneer u dit doet, moet u het gebied van Ontvangers in **Ondertekenaar-2** veranderen, anders worden alle handtekeningsgebieden toegewezen aan de zelfde persoon.

## Uw overeenkomst genereren

U hebt uw document nu gelabeld en bent klaar om te gaan. In deze volgende sectie leert u hoe u een document genereert met de API-voorbeelden voor het genereren van documenten voor Node.js. Deze voorbeelden werken in alle talen.

Open het pdfservices-node-sdk-samples-master-bestand dat u hebt gedownload bij het registreren van uw referenties. Deze bestanden bevatten de bestanden pdfservices-api-credentials.json en private.key.

1. Open uw **[!UICONTROL Eind]** om gebiedsdelen te installeren gebruikend `npm install`.
1. Kopieer uw steekproef *data.json* in de *middelen* omslag.
1. Kopieer het malplaatje van Word dat u in de *middelen* omslag creeerde.
1. Creeer een nieuw dossier in de wortelfolder van de steekproefomslag genoemd *generate-salesOrder.js*.

   ```
   const PDFServicesSdk = require('@adobe/pdfservices-node-sdk').
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

1. Vervang `<JSON FILE>` door de naam van het JSON-bestand in /resources.
1. Vervang `<INSERT DOCX>` door de naam van het DOCX-bestand.
1. Om te lopen, gebruik **[!UICONTROL Eind]** om knoop `generate-salesOrder.js` uit te voeren.

Het uitvoerbestand bevindt zich in de /uitvoermap met het document dat correct is gegenereerd.

U kunt de opmaak wijzigen door de onderstaande regel te wijzigen. De DOCX-indeling is handig als dit document wordt verzonden voor iemand die het kan bewerken in Word of voor contractrevisie.

PDF:

```
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge,
documentMergeOptions.OutputFormat.PDF);
```

Woord:

```
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.DOCX);
```

U moet ook de naam van het uitvoerbestand wijzigen in .pdf of .docx voor respectievelijk de PDF- of DOCX-uitvoerindeling:

```
var outputFileName = path.join('output', 'salesOrder_'+Date.now()+".docx");
```

## Overeenkomst verzenden ter ondertekening

[&#x200B; Adobe Acrobat Sign &#x200B;](https://www.adobe.com/acrobat/business/sign.html) staat u toe om overeenkomsten naar één of vele ontvangers voor hen te verzenden om documenten te bekijken en te ondertekenen. Naast een gebruiksvriendelijke gebruikerservaring om een document ter ondertekening te verzenden, zijn er REST-API&#39;s beschikbaar waarmee u Word, PDF, HTML en andere indelingen kunt gebruiken en ter ondertekening kunt verzenden.

In het onderstaande voorbeeld wordt uitgelegd hoe u de documentatiepagina van de REST API kunt gebruiken om het eerder gegenereerde document ter ondertekening te verzenden. Leer eerst hoe u dit kunt doen via de Acrobat Sign-webinterface en vervolgens hoe u dit kunt doen met de REST-API.

## Een Acrobat Sign-account ophalen

Als u geen rekening van Acrobat Sign hebt, teken omhoog voor een ontwikkelaarrekening en herzie de documentatie [&#x200B; hier &#x200B;](https://developer.adobe.com/adobesign-api/), en selecteer **de Registratie van de Rekening van de Ontwikkelaar**. U wordt gevraagd een formulier in te vullen en een verificatiebericht te ontvangen. Wanneer u dat doet, wordt u naar een website geleid om uw wachtwoord en account in te stellen, waar u zich vervolgens kunt aanmelden bij Acrobat Sign.

## Een overeenkomst verzenden vanuit een webinterface

1. Selecteer **[!UICONTROL verzend]** van de navigatiebar.

   ![&#x200B; Screenshot van Send lusje in Acrobat Sign &#x200B;](assets/automatelegal_28.png)

1. In *Ontvangers* gebied, specificeer twee e-mailadressen. Het wordt aanbevolen een e-mailadres te gebruiken dat niet aan uw Acrobat Sign-account is gekoppeld.

   ![&#x200B; Schermafbeelding van Ontvangervelden &#x200B;](assets/automatelegal_29.png)

1. Plaats een **[!UICONTROL Naam van de Overeenkomst]** en **[!UICONTROL Bericht]**.
1. Selecteer **[!UICONTROL voeg Dossiers]** toe en upload het geproduceerde dossier van uw computer.
1. Selecteer **[!UICONTROL Voorvertoning en handtekeningvelden toevoegen]**.
1. Selecteer **[!UICONTROL Volgende]**.
1. Wanneer u omlaag schuift naar de handtekeningpagina, ziet u de geplaatste handtekeningvelden op basis van de labels.

   ![&#x200B; Schermafbeelding van handtekeningsgebieden &#x200B;](assets/automatelegal_30.png)

1. Selecteer **[!UICONTROL Verzenden]**.
1. In uw e-mail verschijnt een bericht met een koppeling voor weergave en ondertekening.

   ![&#x200B; Schermafbeelding van e-mailbericht &#x200B;](assets/automatelegal_31.png)

1. Selecteer **[!UICONTROL Overzicht en teken]**.
1. Selecteer **[!UICONTROL ga]** verder om termijnen van gebruik goed te keuren.
1. Selecteer **[!UICONTROL Begin]** om aan waar te springen u moet ondertekenen.

   ![&#x200B; Screenshot van beginmarkering &#x200B;](assets/automatelegal_32.png)

1. Selecteer **[!UICONTROL klik hier om te ondertekenen]**.

   ![&#x200B; Screenshot van Klik hier om te ondertekenen &#x200B;](assets/automatelegal_33.png)

1. Typ uw handtekening.

   ![&#x200B; Screenshot van het typen van handtekening &#x200B;](assets/automatelegal_34.png)

1. Selecteer **[!UICONTROL toepassen]**.
1. Selecteer **[!UICONTROL klik om]** te ondertekenen.

Er wordt een e-mail verzonden naar de volgende ondertekenaar. Herhaal stap 9-16 om de tweede ondertekenaar weer te geven en te ondertekenen.

Zodra de overeenkomst is voltooid, wordt een ondertekende kopie van de overeenkomst via e-mail naar elk van de partijen verzonden. Bovendien kan een ondertekende overeenkomst van de het Webinterface van Acrobat Sign in **worden teruggewonnen leidt** pagina.

![&#x200B; Screenshot van Manage lusje in Acrobat Sign &#x200B;](assets/automatelegal_35.png)

Leer vervolgens hoe u hetzelfde scenario kunt uitvoeren via REST API-documentatie.

## Referenties ophalen

1. Navigeer aan [&#x200B; Documentatie van de REST van Acrobat Sign &#x200B;](https://secure.na1.adobesign.com/public/docs/restapi/v6).
1. Breid *transientDocuments* en [&#x200B; POST /transientDocuments &#x200B;](https://benprojecteddemo.na1.adobesign.com/public/docs/restapi/v6#!/transientDocuments/createTransientDocument) uit.
1. Selecteer **[!UICONTROL OAUTH TOEGANG-TOKEN]**.

   ![&#x200B; Screenshot van waar te om OAUTH TOEGANG-TOKEN &#x200B;](assets/automatelegal_36.png) te selecteren

1. Controleer de toestemmingen OAUTH voor *agreement_write*, *agreement_sign*, *widget_write*, en *library_write*.
1. Selecteer **[!UICONTROL machtigt]**.
1. U wordt via een pop-up gevraagd om u aan te melden bij uw Acrobat Sign-account. Meld u aan met de gebruikersnaam en het wachtwoord van uw beheerder.
1. U wordt gevraagd toegang te verlenen tot de REST-documentatie. Selecteer **[!UICONTROL Toegang verlenen]**.

Een dragertoken wordt dan toegevoegd aan het **gebied van de Vergunning**.

Om meer te leren hoe te om een toestemmingstoken voor Acrobat Sign tot stand te brengen, kunt u de stap volgen die [&#x200B; hier &#x200B;](https://opensource.adobe.com/acrobat-sign/developer_guide/helloworld.html) wordt geschetst.

## Een tijdelijk document uploaden

Aangezien de machtigingstoken uit de vorige stappen is toegevoegd, moet u een document uploaden om de API-aanroep te maken:

1. In *gebied van het Dossier*, upload het document van de PDF dat in vorige stappen werd geproduceerd.

   ![&#x200B; Screenshot van waar te om PDF &#x200B;](assets/automatelegal_37.png) te uploaden

1. Selecteer **[!UICONTROL probeer het uit!]**.
1. In **[!UICONTROL Hoofdtekst van de Reactie]**, kopieer de *transientDocumentId* waarde.

*transientDocumentId* wordt gebruikt om een document van verwijzingen te voorzien dat tijdelijk in Acrobat Sign wordt opgeslagen zodat het in verdere API vraag kan worden van verwijzingen voorzien.

## Verzenden ter ondertekening

Zodra een document is geüpload, moet u de overeenkomst ter ondertekening verzenden.

1. Vouw de overeenkomstsectie en de overeenkomstsecties uit.
1. In het *AgreementInfo* gebied, bevolk het met volgende JSON:

   ```
   {
   "fileInfos": [
      {
         "transientDocumentId": "3AAABLblqZhAJeoswpyslef8_toTGT1WgBLk3TlhfJXy_uSLlKyre2hjF0-J1meBDn0PlShk0uQy6JghlqEoqXNnskq7YawteF6QWtHefP9wN2CW_Xbt0O9kq1tkpznG0a5-mEm4bYAV1FGOnD1mt_ooYdzKxm7KzTB11DLX2-81Zbe2Z1suy7oXiWNR3VSb-zMfIb5D4oIxF8BiNfN0q08RwT108FcB1bx4lekkATGld3nRbf8ApVPhB72VNrAIF0F1rAFBWTtfgvBKZaxrYSyZq73R_neMdvZEtxWTk5fii_bLVe7VdNZMcO55sofH61eQC_QIIsoYswZP4rw6dsTa68ZRgKUNs"
      }
   ],
   "name": "Terms and Conditions",
   "participantSetsInfo": [
      {
         "memberInfos": [
         {
            "email": "adobesigndemo+customer@outlook.com"
         }
         ],
         "order": 1,
         "role": "SIGNER"
      },
      {
         "memberInfos": [
            {
               "email": "adobesigndemo+company@outlook.com"
            }
         ],
         "order": 1,
         "role": "SIGNER"
         }
   ],
   "signatureType": "ESIGN",
   "state": "IN_PROCESS"
   }
   ```

1. Selecteer **[!UICONTROL probeer het uit!]**.

**POST overeenkomsten API** keert een identiteitskaart voor de overeenkomst terug. Om een malplaatje voor het JSON modelschema te krijgen, selecteer **Minimaal ModelSchema**. Een volledige lijst van parameters is beschikbaar in de **Volledige Modelsectie van het Schema**.

## Status van overeenkomst controleren

Zodra u een overeenkomst-id hebt, kunt u een overeenkomststatus verzenden.

1. Breid **[!UICONTROL GET /agreements/{agreementId} uit]**.
1. Omdat u extra OAUTH werkingsgebied kunt nodig hebben, selecteer **[!UICONTROL OAUTH-ACCESS-TOKEN]** opnieuw.
1. Kopieer de agreementId van de vorige API-aanroepreactie naar het veld agreementId.
1. Selecteer **[!UICONTROL probeer het uit!]**.

Nu heb je informatie over die overeenkomst.

```
{
    "id": "CBJCHBCAABAAc6LyP4SVuKXP_pNstzIzyripanRdz4IB",
    "name": "Terms and Conditions",
    "groupId": "CBJCHBCAABAAoyMb1yIgczAGhBuJeHf99mglPtM7ElEu",
    "type": "AGREEMENT",
    "participantSetsInfo": [
      {
        "id": "CBJCHBCAABAAzZE-IcHHkt05-AVbxas4Jz7DUl3oEBO6",
        "memberInfos": [
          {
            "email": "adobesigndemo+customer@outlook.com",
            "id": "CBJCHBCAABAAyWgMMReqbxUFM7ctI5xz16c2kOmEy-IQ",
            "securityOption": {
              "authenticationMethod": "NONE"
            }
          }
        ],
        "role": "SIGNER",
        "order": 1
      },
      {
        "id": "CBJCHBCAABAAaRHz3gY2W0w5n_6pj1GMMuZAfhBihc1j",
        "memberInfos": [
          {
            "email": "adobesigndemo+company@outlook.com",
            "id": "CBJCHBCAABAAOZQwjPwJXFiX8YDKPYtzMpftsmxYrIo9",
            "securityOption": {
              "authenticationMethod": "NONE"
            }
          }
        ],
        "role": "SIGNER",
        "order": 1
      }
    ],
    "senderEmail": "adobesigndemo+new@outlook.com",
    "createdDate": "2022-03-22T02:59:36Z",
    "lastEventDate": "2022-03-22T02:59:41Z",
    "signatureType": "ESIGN",
    "locale": "en_US",
    "status": "OUT_FOR_SIGNATURE",
    "documentVisibilityEnabled": true,
    "hasFormFieldData": false,
    "hasSignerIdentityReport": false,
    "documentRetentionApplied": false
  }
```

De efficiëntere methode om berichten te krijgen wanneer de updates worden veranderd is via Webhooks, die u meer over [&#x200B; hier &#x200B;](https://opensource.adobe.com/acrobat-sign/developer_guide/webhookapis.html) kunt leren.

## Een ondertekend document opslaan

Nadat het document is ondertekend, kan het worden opgehaald met behulp van het bestand GET /agreements/combinedDocument.

1. Breid **[!UICONTROL GET /agreements/{agreementId}/combinedDocument]** uit.
1. Plaats **[!UICONTROL agreementId]** aan *agreementId* verstrekt van de vorige API vraag.
1. Selecteer **[!UICONTROL probeer het uit!]**.

Aanvullende parameters voor het toevoegen van een controlerapport of ondersteunende documenten kunnen worden ingesteld met behulp van de parameters attachSupportingDocuments en attachAuditReport.

In het **Lichaam van de Reactie**, kan het dan aan uw computer worden gedownload en worden opgeslagen waar u houdt van.

## Meer opties

Naast het genereren en verzenden van een document ter ondertekening, zijn er nog meer acties beschikbaar.

Als het document bijvoorbeeld geen handtekening heeft, biedt de Adobe PDF Services API veel manieren om documenten te transformeren nadat de overeenkomst is gegenereerd, zoals:

* Document beveiligen met een wachtwoord
* PDF comprimeren als er grote afbeeldingen zijn
* Als u meer wilt weten over andere beschikbare handelingen, bekijkt u de scripts in de map /src in de voorbeeldbestanden voor de Adobe PDF Services API. U kunt ook meer leren door de documentatie van de verschillende handelingen te bekijken die kunnen worden gebruikt.

Bovendien biedt Acrobat Sign verschillende extra functies, zoals:

* Ondertekeningservaring insluiten in een toepassing
* Verificatiemethoden voor ondertekenaars toevoegen
* Instellingen voor e-mailmeldingen configureren
* Afzonderlijke documenten downloaden als onderdeel van een overeenkomst

## Verder leren

Wil je meer leren? Bekijk enkele aanvullende manieren om [!DNL Adobe Acrobat Services] te gebruiken:

* Leer meer van [&#x200B; documentatie &#x200B;](https://developer.adobe.com/document-services/docs/overview/)
* Meer zelfstudies op Adobe Experience League bekijken
* Gebruik de voorbeeldscripts in de map /src om te zien hoe u PDF kunt gebruiken
* Volg [&#x200B; Blog van de Technologie van de Adobe &#x200B;](https://medium.com/adobetech/tagged/adobe-document-cloud) voor recentste uiteinden en trucs
* Abonneer aan [&#x200B; Clips van het Papier (de maandelijkse levende stroom) &#x200B;](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF) om over het automatiseren te leren gebruiken [!DNL Adobe Acrobat Services].
