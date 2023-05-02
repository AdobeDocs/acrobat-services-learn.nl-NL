---
title: Verkoopvoorstellen en contracten beheren
description: Leer hoe je een efficiënte workflow bouwt om verkoopvoorstellen te automatiseren en te vereenvoudigen
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8099.jpg
kt: 8099
exl-id: 219c70de-fec1-4946-b10e-8ab5812562ef
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '1395'
ht-degree: 2%

---

# Verkoopvoorstellen en contracten beheren

![Hoofdlettergebruik hoofdbanner gebruiken](assets/UseCaseSalesHero.jpg)

Verkoopvoorstellen vormen de eerste stap in het traject van een bedrijf naar klantenwerving. Net als bij alles, zijn de eerste impressies het laatst. Dus je eerste interactie met klanten stelt hun verwachtingen voor je bedrijf in. Uw voorstel moet beknopt, accuraat en handig zijn.

Contracten en voorstellen bevatten verschillende soorten gegevens binnen hun documentstructuur. Ze bevatten zowel dynamische gegevens (naam van de client, aantal aanhalingstekens, enzovoort) als statische gegevens (vaste tekst, zoals vaste mogelijkheden, teamprofielen en standaardvoorwaarden voor SOW). Het creëren van malplaatjedocumenten, zoals verkoopvoorstellen, impliceert vaak monotone taken, zoals manueel het vervangen van projectdetails in een boilerplate malplaatje. In deze zelfstudie gebruikt u dynamische gegevens en workflows om een efficiënt proces te bouwen voor [voorstellen voor verkoopvoorstellen maken](https://www.adobe.io/apis/documentcloud/dcsdk/sales-proposals-and-contracts.html).

## Wat je kunt leren

In deze praktische zelfstudie leert u hoe u dynamische gegevens en workflows kunt implementeren met behulp van verschillende tools, waarvan de belangrijkste zijn [!DNL Adobe Acrobat Services] API&#39;s. Deze API&#39;s worden gebruikt om verkoopvoorstellen en contracten voor jou en je bedrijf gemakkelijker te maken. Deze zelfstudie demonstreert praktische technieken voor het automatisch maken, samenvoegen en weergeven van PDF-documenten. Het handmatig uitvoeren van deze taken is tijdrovend en vervelend. Door te profiteren van [!DNL Acrobat Services] API&#39;s kunt u de tijd die aan deze taken is besteed, verkorten.

## Relevante API&#39;s en bronnen

* [Microsoft Word](https://www.office.com/)

* [Node.js](https://nodejs.org/en/)

* [npm](https://www.npmjs.com/get-npm)

* [[!DNL Acrobat Services] API&#39;s](https://www.adobe.io/apis/documentcloud/dcsdk/)

* [Adobe-API voor documentgeneratie](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [Adobe Sign-API](https://www.adobe.io/apis/documentcloud/sign.html)

* [Tagger voor het genereren van Adobe-documenten](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

## Het probleem oplossen

Nu u de gereedschappen hebt geïnstalleerd, kunt u beginnen met het oplossen van het probleem. De voorstellen hebben zowel statische inhoud als dynamische inhoud die uniek is voor elke klant. Er treden knelpunten op omdat beide soorten gegevens noodzakelijk zijn telkens wanneer u een voorstel doet. Het is tijdrovend om de statische tekst in te voeren, zodat u deze gaat automatiseren en alleen handmatig met de dynamische gegevens van elke client gaat werken.

Maak eerst een formulier voor het vastleggen van gegevens in [Microsoft Forms](https://www.office.com/launch/forms?auth=1) (of de voorkeursfunctie voor het maken van formulieren). Dit formulier is bedoeld voor de dynamische gegevens van klanten die worden toegevoegd aan een verkoopvoorstel. Vul dit formulier in met vragen om de benodigde gegevens van klanten op te halen, bijvoorbeeld bedrijfsnaam, datum, adres, projectbereik, prijzen en aanvullende opmerkingen. Gebruik deze [formulier](https://forms.office.com/Pages/ShareFormPage.aspx id=DQSIkWdsW0yxEjajBLZtrQAAAAAAAAAAAAN__rtiGj5UNElTR0pCQ09ZNkJRUlowSjVQWDNYUEg2RC4u&amp;sharetoken=1AJe MavBAzzxuISRKmUy). Het doel is dat potentiële klanten het formulier invullen en vervolgens hun reacties exporteren als JSON-bestanden, die worden doorgegeven aan het volgende gedeelte van uw workflow.

In sommige formulierbuilders kunt u gegevens alleen exporteren als CSV-bestanden. Het kan dus handig zijn om [converteren](http://csvjson.com/csv2json) het gegenereerde CSV-bestand in een JSON-bestand.

De statische gegevens worden opnieuw gebruikt in elk verkoopvoorstel. Zo, kunt u een malplaatje van het verkoopvoorstel in Microsoft Word gebruiken om de statische tekst te verstrekken. U kunt dit [template](https://1drv.ms/w/s!AiqaN2pp7giKkmhVu2_2pId9MiPa?e=oeqoQ2), maar u kunt uw eigen [Adobe-sjabloon](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html).

Nu, hebt u iets nodig dat zowel de dynamische gegevens van cliënten in het formaat JSON als de statische tekst in het malplaatje van Microsoft Word neemt om een uniek verkoopvoorstel voor een cliënt te maken. De [!DNL Acrobat Services] API&#39;s worden gebruikt om de twee te verenigen en een PDF te genereren die kan worden ondertekend.

U kunt dit doen door tags te gebruiken. Tags zijn gebruiksvriendelijke tekenreeksen die getallen, woorden, arrays of zelfs complexe objecten kunnen vertegenwoordigen. Codes fungeren als plaatsaanduiding voor dynamische gegevens, in dit geval clientgegevens die in het formulier worden ingevoerd. Nadat u tags in de sjabloon hebt ingevoegd, kunt u formuliervelden uit het JSON-bestand toewijzen aan de Word-sjabloon.

## Tags gebruiken

Open uw verkoopvoorstelsjabloon en selecteer de **Invoegen** tabblad. In het dialoogvenster **Invoegtoepassingen** groep, selecteert u **Invoegtoepassingen ophalen**. Selecteer vervolgens **Invoegtoepassing voor het genereren van Adobe-documenten** om toe te voegen. Als het document is toegevoegd, ziet u de markering voor documentgeneratie op het tabblad **Home** in de **Adobe** groep.

Op de **Home** in de **Adobe** groep, selecteert u **Documenten genereren** om het document te gaan coderen. Een nuttige demonstratievideo wordt weergegeven in een deelvenster aan de rechterkant van het venster.

![Screenshot van de invoegtoepassing Document Tagger in Word](assets/sales_1.png)

Selecteren **Aan de slag**. U wordt vervolgens gevraagd voorbeeldgegevens te verstrekken. Plak het JSON-formulierreactiebestand in of upload het zoals hieronder weergegeven.

![Screenshot van het plakken van voorbeeldcode](assets/sales_2.png)

Selecteren **Labels genereren** om een lijst met velden op te halen uit het JSON-bestand dat u hebt geplakt of geüpload. De tags worden hieronder weergegeven, in de rechterzijbalk.

![Screenshot van beschikbare tags](assets/sales_3.png)

Nadat u de codes hebt gegenereerd, kunt u deze in het document invoegen. Er worden codes aan het document toegevoegd op de locatie van de cursor. Zoals hierboven getoond, zou u moeten toevoegen **Projectbereik** tag rechts onder de **Projectbereik** ondertitel. Op deze manier, wanneer een client het bereik van het project in het formulier invoert, blijft de reactie onder de **Projectbereik** subtitel, waarbij u de zojuist toegevoegde tag vervangt. Nadat u klaar bent met het toevoegen van codes, moet een deel van uw document er uitzien als de schermvastlegging hieronder.

![Screenshot van het toevoegen van codes aan Word-document](assets/sales_4.png)

## API&#39;s gebruiken

Ga naar de [!DNL Acrobat Services] API&#39;s [homepage](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html). Begin met het gebruik [!DNL Acrobat Services] API&#39;s hebt u aanmeldingsgegevens voor uw toepassing nodig. Schuif omlaag en selecteer **Proefversie starten** om referenties te maken. U kunt deze services gebruiken [gratis voor zes maanden, vervolgens naar keuze betalen](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) voor slechts USD 0,05 per documenttransactie, dus betaal je alleen voor wat je nodig hebt.

Selecteren **PDF Services API** als uw keuze-service en vul de overige gegevens in zoals hieronder weergegeven.

![Screenshot van het opvragen van referenties](assets/sales_5.png)

Nadat u uw referenties hebt gemaakt, krijgt u enkele codevoorbeelden. Selecteer de voorkeurstaal (in deze zelfstudie wordt Node.js gebruikt). Uw API-referenties bevinden zich in een zip-bestand. Extraheer de bestanden naar PDFToolsSDK-Node.jsSamples.

Maak om te beginnen een lege map met de naam auto-doc\*\.\*\* Voer in de map de volgende opdracht uit om een Node.js-project te initialiseren: `npm init`. Geef uw project de naam &quot;auto-doc&quot;*.*

In de map ./PDFToolsSDK-Node.jsSamples/adobe-dc-pdf-tools-sdk-node-samples, is er een bestand met de naam pdftools-api-credentials.json. Verplaats de map en private.key naar de map auto-doc. Deze bevat uw API-referenties. Maak in de map voor automatische documenten ook een submap met de naam &quot;resources&quot;. Het bevat de JSON-gegevens die van klanten worden ontvangen wanneer u een verkoopvoorstel genereert. Sla in dezelfde map de sjabloon voor het verkoopvoorstel op vanuit Microsoft Word.

Nu ben je klaar om wat magie te maken! Omdat u Node.js in deze zelfstudie gebruikt, moet u Node.js installeren [!DNL Acrobat Services] SDK. Voer hiertoe in de map auto-doc het garen @adobe/documentservices-pdftools-node-sdk toe.

Maak nu een bestand met de naam merge.js en plak de volgende code in het bestand.

```
javascript
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk'),
fs = require('fs');
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Setup input data for the document merge process
const jsonString = fs.readFileSync('resources/Proposal.json'),
jsonDataForMerge = JSON.parse(jsonString);
// Create an ExecutionContext using credentials
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials);
// Create a new DocumentMerge options instance
const documentMerge = PDFToolsSdk.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge, documentMergeOptions.OutputFormat.PDF);
// Create a new operation instance using the options instance
const documentMergeOperation = documentMerge.Operation.createNew(options)
// Set operation input document template from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile('resources/Proposal.docx');
documentMergeOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
.then(result => result.saveAsFile('output/Proposal.pdf'))
.catch(err => {
if (err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log('Exception encountered while executing operation', err);
} else {
console.log('Exception encountered while executing operation', err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
```

Deze code haalt uw JSON-bestand op van het Microsoft-formulier met behulp van de tags die u hebt gemaakt met [!DNL Acrobat Services]. Vervolgens worden de gegevens samengevoegd met de sjabloon voor het verkoopvoorstel die u in Microsoft Word hebt gemaakt om een gloednieuwe PDF te genereren. De PDF wordt opgeslagen in het nieuwe bestand./uitvoermap.

De code gebruikt ook [Adobe Sign API](https://www.adobe.io/apis/documentcloud/sign.html) beide ondernemingen het gegenereerde verkoopvoorstel laten ondertekenen. Lees dit blogbericht voor een gedetailleerde uitleg van deze API.

## Volgende stappen

Je begon met een inefficiënt, vervelend proces dat automatisering nodig had. Je bent van het handmatig maken van documenten voor elke klant naar het creëren van een gestroomlijnde workflow om te automatiseren en te vereenvoudigen [het proces van het verkoopvoorstel](https://www.adobe.io/apis/documentcloud/dcsdk/sales-proposals-and-contracts.html).

Met Microsoft Forms kreeg je kritieke data van je klanten die in hun unieke voorstellen zouden komen. U hebt een sjabloon voor een verkoopvoorstel gemaakt in Microsoft Word om de statische tekst op te geven die u niet telkens opnieuw wilt maken. Daarna hebt u [!DNL Acrobat Services] API&#39;s om data uit het formulier en de sjabloon samen te voegen om een PDF voor verkoopvoorstellen voor je klanten op een efficiëntere manier te maken.

Deze praktische zelfstudie is slechts een glimp van wat er mogelijk is met deze API&#39;s. Ga voor meer oplossingen naar de [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) APIs-pagina. Al deze tools zijn zes maanden gratis. Vervolgens betaalt u slechts USD 0,05 per documenttransactie op de [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) plannen, zodat je alleen betaalt zoals je team je verkooppijplijn nog meer uitzicht biedt.
