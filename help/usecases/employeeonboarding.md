---
title: Modernisering van onboarding van medewerkers
description: Leer hoe te om werknemersonboarding met  [!DNL Adobe Acrobat Services]  APIs te moderniseren
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-10203
thumbnail: KT-10203.jpg
exl-id: 0186b3ee-4915-4edd-8c05-1cbf65648239
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1434'
ht-degree: 0%

---

# Modernisering van onboarding van werknemers

![ Hoofdletterbanner van het Gebruik ](assets/usecaseemployeeonboardinghero.jpg)

In een grote organisatie kan onboarding van werknemers een groot en traag proces zijn. Gewoonlijk is er een combinatie van aangepaste documentatie samen met bouwsteenmateriaal die moet worden gepresenteerd en ondertekend door een nieuwe medewerker. Deze combinatie van aangepast en bouwsteenmateriaal vereist meerdere stappen: kostbare tijd wegnemen van de mensen die bij het proces betrokken zijn. [!DNL Adobe Acrobat Services] en Acrobat Sign kunnen deze benadering moderniseren en automatiseren, waardoor uw HR persoonlijk wordt vrijgemaakt voor belangrijkere taken. Laten we eens kijken hoe dit wordt bereikt.

## Wat zijn [!DNL Adobe Acrobat Services]?

[[!DNL Adobe Acrobat Services] ](https://developer.adobe.com/document-services/homepage) is een reeks APIs verwant met het werken met documenten (en niet alleen PDF). Over het algemeen valt dit pakket services in drie hoofdcategorieën:

* Eerst is de ](https://developer.adobe.com/document-services/apis/pdf-services/) reeks van de Diensten van 0} PDF {van hulpmiddelen. [ Dit zijn hulpmethoden voor het werken met PDF en andere documenten. De services omvatten onder andere het omzetten in en van PDF, het uitvoeren van OCR en optimalisatie, het samenvoegen en splitsen van PDF, enzovoort. Het is de gereedschapset met documentverwerkingsfuncties.
* [ PDF verwijder API ](https://developer.adobe.com/document-services/apis/pdf-extract/) gebruikt krachtige AI/ML technieken om een PDF te analyseren en een ongelooflijke hoeveelheid detail over de inhoud terug te keren. Dit omvat de tekst, opmaak en positionele informatie, en kan ook tabelgegevens in CSV/XLS-indeling retourneren en afbeeldingen ophalen.
* Tot slot [ de Generatie API van het Document ](https://developer.adobe.com/document-services/apis/doc-generation/) laat ontwikkelaars Microsoft Word als &quot;malplaatje&quot;gebruiken, met hun gegevens (uit om het even welke bron) mengen, en dynamische gepersonaliseerde documenten (PDF en Word) produceren.

De ontwikkelaars kunnen [ omhoog ](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html) ondertekenen en elk van deze diensten met een vrije proef proberen. Het [!DNL Acrobat Services] -platform gebruikt een REST-API, maar ondersteunt ook SDK&#39;s voor Node, Java, .NET en Python (momenteel alleen Extraheren).

Terwijl niet API, kunnen de ontwikkelaars vrije [ PDF ook gebruiken bedt API ](https://developer.adobe.com/document-services/apis/pdf-embed/), die een verenigbare en flexibele het bekijken ervaring van documenten met uw Web-pagina&#39;s verstrekt.

## Wat is Acrobat Sign?

[ Acrobat Sign ](https://www.adobe.com/acrobat/business/sign.html) is de wereldleider in de elektronische handtekeningsdiensten. U kunt documenten ter ondertekening verzenden met behulp van verschillende workflows, waaronder meerdere handtekeningen. Acrobat Sign ondersteunt ook workflows waarvoor handtekeningen en aanvullende informatie vereist zijn. Al deze mogelijkheden worden ondersteund door een krachtig dashboard met een flexibel ontwerpsysteem.

Zoals met [!DNL Acrobat Services], heeft Acrobat Sign a [ vrije proef ](https://www.adobe.com/acrobat/business/sign.html#sign_free_trial) die ontwikkelaars het het ondertekenen proces zowel via het dashboard als met makkelijk te gebruiken REST-based API laat testen.

## Een onboardingscenario

Laten we eens kijken naar een echt scenario dat aantoont hoe de diensten van de Adobe kunnen helpen. Wanneer een nieuwe werknemer zich bij een bedrijf aansluit, hebben zij aangepaste informatie nodig die aan hun rol wordt aangepast. Bovendien hebben zij ook bedrijfsbreed materiaal nodig. Tot slot moeten ze de acceptatie van het bedrijfsbeleid aantonen door de documenten te ondertekenen. Laten we dit onderverdelen in concrete stappen:

* Ten eerste is een aangepaste begeleidende brief nodig die de nieuwe werknemer op naam begroet. De brief zou informatie over de naam van de werknemer, de rol, het salaris, en de plaats moeten bevatten.
* De aangepaste brief moet worden gecombineerd met een PDF die basis, bedrijfsbrede informatie bevat (denk aan diverse het beleid van u, voordelen, enz.)
* Er moet een definitief document worden opgenomen waarin wordt gevraagd om de handtekening en de datum van de werknemer.
* Al het bovenstaande moet worden gepresenteerd als één document dat ter ondertekening naar de medewerker wordt gestuurd.

Laten we nader ingaan op hoe we dit kunnen doen.

## Dynamische documenten genereren

Adobe ](https://developer.adobe.com/document-services/apis/doc-generation/) API van de Generatie van het Document van 0} laat ontwikkelaars dynamische documenten tot stand brengen door Microsoft Word en een eenvoudige het templating taal te gebruiken, als basis voor het produceren van PDF en de documenten van Word. [ Hier is een voorbeeld van hoe dit werkt.

Laten we beginnen met een Word-document met hard-gecodeerde waarden. Het document kan op elke gewenste manier worden opgemaakt, zoals afbeeldingen, tabellen, enzovoort. Hier is het eerste document.

![ Screenshot van eerste document ](assets/onboarding_1.png)

Het genereren van documenten werkt door tokens toe te voegen aan een Word-document die worden vervangen door uw gegevens. Terwijl deze tokens manueel kunnen zijn ingegaan, is er toe:voegen-binnen van a [ Microsoft Word ](https://developer.adobe.com/document-services/docs/overview/document-generation-api/wordaddin/) dat dit gemakkelijker maakt te doen. Als u deze functie opent, kunnen auteurs codes of gegevenssets definiëren die in uw document kunnen worden gebruikt.

![ Schermafbeelding van de Tagger van het Document ](assets/onboarding_2.png)

U kunt JSON-informatie uit een lokaal bestand uploaden, kopiëren in JSON-tekst of selecteren om door te gaan met de initiële gegevens. Zo kunt u uw tags ad hoc definiëren op basis van uw specifieke behoeften. In dit voorbeeld is alleen een tag voor naam, rol, salaris en locatie nodig. Dit wordt gedaan door **te gebruiken creeer de knoop van de Markering**:

![ Screenshot van het bepalen van een markering ](assets/onboarding_3.png)

Nadat u de eerste tag hebt gedefinieerd, kunt u zo veel definiëren als u nodig hebt:

![ Screenshot van bepaalde markeringen ](assets/onboarding_4.png)

Als u de labels hebt gedefinieerd, selecteert u de tekst in het document en vervangt u deze waar nodig door de codes. In dit voorbeeld worden tags toegevoegd voor naam, rol en salaris.

![ Screenshot van Markeringen ](assets/onboarding_5.png)

Het genereren van documenten ondersteunt niet alleen eenvoudige tags, maar ook logische expressies. De tweede alinea van het document bevat tekst die alleen van toepassing is op mensen in Louisiana. U kunt een voorwaardelijke expressie toevoegen door naar het tabblad Geavanceerd van de documentmarkering te gaan en een voorwaarde te definiëren. Hieronder wordt beschreven hoe u een eenvoudige voorwaarde voor gelijkheid definieert, maar u ziet dat numerieke vergelijkingen en andere vergelijkingstypen ook worden ondersteund.

![ Screenshot van Voorwaarde ](assets/onboarding_6.png)

Deze kan vervolgens worden ingevoegd en rond de alinea worden geplaatst:

![ Screenshot van Voorwaarde in doc ](assets/onboarding_7.png)

Om te testen hoe dit werkt, uitgezocht **produceer document**. De eerste keer dat u dit doet, moet u zich aanmelden met een Adobe ID. Na het aanmelden wordt standaard-JSON weergegeven die handmatig kan worden bewerkt.

![ Screenshot van Gegevens ](assets/onboarding_8.png)

Er wordt een PDF gegenereerd die vervolgens kan worden weergegeven of gedownload.

![ Screenshot van Gegenereerde PDF ](assets/onboarding_9.png)

Met de Document Tagger kunt u snel ontwerpen en testen, nadat u klaar bent en in productie, maar u kunt een van de SDK&#39;s gebruiken om dit proces te automatiseren. Terwijl de daadwerkelijke code gebaseerd op specifieke behoeften verschilt, is hier een voorbeeld van hoe deze code in Node.js kijkt:

```js
 const PDFServicesSdk = require('@adobe/pdfservices-node-sdk');

const credentials =  PDFServicesSdk.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdfservices-api-credentials.json")
    .build();

// Data would be dynamic...
let data = {
    "name":"Raymond Camden",
    "role":"Lead Developer",
    "salary":9000,
    "location":"Louisiana"
}

// Create an ExecutionContext using credentials.
const executionContext = PDFServicesSdk.ExecutionContext.create(credentials);

// Create a new DocumentMerge options instance.
const documentMerge = PDFServicesSdk.DocumentMerge,
    documentMergeOptions = documentMerge.options,
    options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.PDF);

// Create a new operation instance using the options instance.
const documentMergeOperation = documentMerge.Operation.createNew(options);

// Set operation input document template from a source file.
const input = PDFServicesSdk.FileRef.createFromLocalFile('documentMergeTemplate.docx');
documentMergeOperation.setInput(input);

// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
    .then(result => result.saveAsFile('documentOutput.pdf'))
    .catch(err => {
        if(err instanceof PDFServicesSdk.Error.ServiceApiError
            || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
            console.log('Exception encountered while executing operation', err);
        } else {
            console.log('Exception encountered while executing operation', err);
        }
    });
```

Kortom, de code stelt referenties in, maakt een bewerkingsobject, stelt de invoer en opties in en roept de bewerking aan. Tot slot wordt het resultaat als een PDF opgeslagen. (De resultaten kunnen ook worden uitgevoerd als Word.)

Het genereren van documenten ondersteunt veel complexere gebruiksscenario&#39;s, zoals de mogelijkheid om volledig dynamische tabellen en afbeeldingen te hebben. Zie de [ documentatie ](https://developer.adobe.com/document-services/docs/overview/document-generation-api/) voor meer details.

## PDF-bewerkingen uitvoeren

De [ PDF Services API ](https://developer.adobe.com/document-services/apis/pdf-services/) verstrekt een grote reeks &quot;nut&quot;verrichtingen voor het werken met PDF. Deze bewerkingen omvatten:

* PDF maken van Office-documenten
* PDF naar Office-documenten exporteren
* PDF combineren en splitsen
* OCR toepassen op PDF
* Beveiliging instellen, verwijderen en wijzigen voor PDF
* Pagina&#39;s verwijderen, invoegen, opnieuw ordenen en roteren
* PDF optimaliseren via compressie of linearisatie
* PDF-eigenschappen ophalen

Voor dit scenario, moet het resultaat van de vraag van de Generatie van het Document met een standaard PDF worden samengevoegd. Deze bewerking is vrij eenvoudig met de SDK&#39;s. Hier is een voorbeeld van Node.js:

```js
const PDFServicesSdk = require('@adobe/pdfservices-node-sdk');
 
// Initial setup, create credentials instance.
const credentials = PDFServicesSdk.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdfservices-api-credentials.json")
    .build();
 
// Create an ExecutionContext using credentials and create a new operation instance.
const executionContext = PDFServicesSdk.ExecutionContext.create(credentials),
    combineFilesOperation = PDFServicesSdk.CombineFiles.Operation.createNew();
 
// Set operation input from a source file.
const combineSource1 = PDFServicesSdk.FileRef.createFromLocalFile('documentOutput.pdf'),
      combineSource2 = PDFServicesSdk.FileRef.createFromLocalFile('standardCorporate.pdf');

combineFilesOperation.addInput(combineSource1);
combineFilesOperation.addInput(combineSource2);
 
// Execute the operation and Save the result to the specified location.
combineFilesOperation.execute(executionContext)
    .then(result => result.saveAsFile('combineFilesOutput.pdf'))
    .catch(err => {
        if (err instanceof PDFServicesSdk.Error.ServiceApiError
            || err instanceof PDFServicesSdk.Error.ServiceUsageError) {
            console.log('Exception encountered while executing operation', err);
        } else {
            console.log('Exception encountered while executing operation', err);
        }
    });
```

Deze code neemt de twee PDF, voegt deze samen en slaat het resultaat op in een nieuwe PDF. Eenvoudig en gemakkelijk! Zie [ documenten ](https://developer.adobe.com/document-services/docs/overview/pdf-services-api/) voor voorbeelden van wat kan worden gedaan.

## Het ondertekeningsproces

In de laatste fase van het onboardingproces moet de werknemer een overeenkomst ondertekenen waarin staat dat hij/zij alle beleidsregels heeft gelezen en ermee akkoord gaat die binnen deze overeenkomst zijn gedefinieerd. [ Acrobat Sign ](https://www.adobe.com/acrobat/business/sign.html) steunt vele verschillende werkschema&#39;s en integraties, met inbegrip van geautomatiseerde via een [ API ](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html). In grote lijnen kan het laatste gedeelte van het scenario als volgt worden voltooid:

Ontwerp eerst het document dat het formulier bevat dat moet worden ondertekend. Er zijn verschillende manieren om dit te doen, inclusief een visueel ontwerp in het Adobe Sign-gebruikersdashboard. U kunt ook de invoegtoepassing Word voor het genereren van documenten gebruiken om de labels voor u in te voegen. In dit voorbeeld worden een handtekening en een datum gevraagd.

![ Schermafbeelding van document met de Markeringen van het Teken ](assets/onboarding_10.png)

Dit document kan worden opgeslagen als een PDF en op dezelfde manier als hierboven is beschreven. Het document kan worden samengevoegd met alle documenten. Met dit proces maakt u één samenhangend pakket met een gepersonaliseerde begroeting, standaardbedrijfsdocumentatie en een uiteindelijke pagina die geschikt is voor ondertekening.

De sjabloon kan naar het Acrobat Sign-dashboard worden geüpload en vervolgens voor nieuwe overeenkomsten worden gebruikt. Door de REST-API te gebruiken, kan dit document vervolgens naar de potentiële werknemer worden verzonden om zijn of haar handtekening aan te vragen.

![ Screenshot van ondertekend doc ](assets/onboarding_11.png)

## Ervaar het zelf

Alles wat in dit artikel wordt beschreven, kan nu worden getest. De [!DNL Adobe Acrobat Services] API [ vrije proef ](https://documentcloud.adobe.com/dc-integration-creation-app-cdn/main.html) geeft u momenteel 1.000 vrije verzoeken over een periode van zes maanden. De vrije proef van Acrobat Sign ](https://www.adobe.com/acrobat/business/sign.html#sign_free_trial) laat u watermerken overeenkomsten voor het testen doeleinden verzenden.[

Hebt u vragen? Het [ steunforum ](https://community.adobe.com/t5/acrobat-services-api/ct-p/ct-Document-Cloud-SDK) wordt gecontroleerd door Adobe ontwikkelaars en steunmensen elke dag. Ten slotte, voor meer inspiratie, ben zeker om de volgende [ episode van de Clips van het Document te vangen ](https://www.youtube.com/playlist?list=PLcVEYUqU7VRe4sT-Bf8flvRz1XXUyGmtF). Er zijn regelmatig live vergaderingen met nieuws, demonstraties en gesprekken met klanten.
