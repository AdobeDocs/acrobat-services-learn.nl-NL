---
title: Een NDA maken
description: Leer hoe je een dynamische NDA-PDF maakt voor samenwerking
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8098
thumbnail: KT-8098.jpg
exl-id: f4ec0182-a46e-43aa-aea3-bf1d19f1a4ec
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1072'
ht-degree: 0%

---

# Een NDA maken

![&#x200B; Hoofdletterbanner van het Gebruik &#x200B;](assets/UseCaseNDAHero.jpg)

Organisaties werken samen met externe medewerkers om hun services en producten te bouwen. Een geheimhoudingsovereenkomst (NDA) is een belangrijk onderdeel van deze samenwerking. Het verplicht alle partijen om vertrouwelijke informatie vrij te geven die een van beide entiteiten zou kunnen schaden.

De meest gebruikte NDA-indeling is een PDF-document. Organisaties stellen een NDA op en sturen deze naar alle partijen. Zodra iedereen heeft ondertekend, start hij het contract. In een team met hoge snelheid vertraagt het handmatig maken van PDF de voortgang.

## Wat je kunt leren

In deze praktische zelfstudie wordt uitgelegd hoe u een speciale Microsoft Word NDA-sjabloon voor uw bedrijf kunt maken. De vrije toe:voegen-binnen van de Adobe voor Microsoft Word, {Tagger van de Generatie van het Document van 0}, neemt &quot;markeringen&quot;op om de dynamische waarden in te voeren. [&#128279;](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo) Leer hoe u de JSON-gegevens aan de sjabloon doorgeeft en een dynamische PDF maakt. De resulterende PDF kan aan uw medewerkers in hun browser worden gemaild of getoond, afhankelijk van uw bedrijfsvereisten en doelstellingen. Als je mee wilt doen, heb je slechts een kleine ervaring nodig met Node.js, JavaScript, Express.js, HTML en CSS.

## Relevante API&#39;s en bronnen

Met [!DNL Adobe Acrobat Services] kunt u direct PDF-documenten genereren met behulp van dynamische gegevens. [!DNL Acrobat Services] biedt een reeks hulpmiddelen van de PDF, met inbegrip van de Generatie API van het Document van de Adobe aan om [&#x200B; verwezenlijking NDA &#x200B;](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/nda-creation) te automatiseren.

* [&#x200B; de Generatie API van het Document van de Adobe &#x200B;](https://developer.adobe.com/document-services/apis/doc-generation)

* [&#x200B; Adobe Sign API &#x200B;](https://developer.adobe.com/adobesign-api/)

* [&#x200B; Tagger van de Generatie van het Document van de Adobe &#x200B;](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

* [&#x200B; code van het Project &#x200B;](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample)

* [[!DNL Acrobat Services]  sleutels &#x200B;](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getcred)

## Het JSON-model maken

De Microsoft Word-sjabloon is afhankelijk van het JSON-model, dus u maakt die sjabloon eerst. Voor deze zelfstudie gebruikt u een basis-JSON-structuur die bedrijfsgegevens bevat, zoals contactgegevens.

```
{
"vendor": {
"companyName": "GlobalCorp",
"street": "123 Any Street",
"street2": "",
"city":"Anywhere",
"state":"CA",
"primaryContact": {
"firstName":"John",
"lastName":"Doe",
"email":"john-doe@example.com",
"phone":"123-456-7890"
}
},
"authorizedSigner": {
"firstName": "Sarah",
"lastName": "Rose",
"email": "sarah@example.com",
"phone":"555-555-1234"
}
}
```

U gebruikt deze structuur in Microsoft Word om een sjabloon te genereren. Deze gegevens kunnen uit elke gegevensbron afkomstig zijn, zolang deze in de JSON-indeling staan. Voor het gemak maakt u meerdere bestanden in de toepassing Node.js, maar voor uw gebruiksscenario is mogelijk een databaseverbinding nodig om de informatie van de leverancier op te vragen.

## De Microsoft Word-sjabloon maken

Maak de NDA-sjabloon in een Microsoft Word-document. Adobe PDF Services API verwacht dat het Microsoft Word-document codes bevat waarin de service waarden uit JSON-documenten kan inspuiten. Hoewel de sjabloon hetzelfde is voor alle aanvragen tot Adobe, veranderen de dynamische gegevens in JSON. Deze tags helpen in dit geval bij het maken van PDF-documenten voor elke leverancier, door gebruik te maken van één Microsoft Word-sjabloon en het proces te versnellen door het genereren van NDA-documenten te automatiseren.

U kunt de [&#x200B; vrije Tagger van de Generatie van het Document toe:voegen-binnen &#x200B;](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo) aan Microsoft Word installeren. Als u deel uitmaakt van een organisatie, kunt u uw Microsoft Office-beheerder vragen de gratis invoegtoepassing voor iedereen te installeren.

Als u de invoegtoepassing hebt geïnstalleerd, vindt u deze op het tabblad Start onder de categorie Adobe. Om het lusje te openen, selecteer **de Generatie van het Document**:

![&#x200B; Schermafbeelding van de invoegtoepassing voor documentgeneratie in Word &#x200B;](assets/nda_1.png)

Binnen het tabblad kunt u het JSON-voorbeelddocument uploaden. Dit document kan een voorbeeld zijn, omdat u het alleen gebruikt om een Microsoft Word-sjabloon te maken.

![&#x200B; Schermafbeelding van steekproefgegevens in toe:voegen-binnen de Generatie van het Document &#x200B;](assets/nda_2.png)

Selecteer **produceer Markeringen** om punten te bekijken u binnen uw malplaatje kunt gebruiken. Hier volgen de eigenschappen die uit de JSON-structuur zijn geëxtraheerd en klaar zijn voor gebruik in de sjabloon:

![&#x200B; Schermafbeelding van tekstmarkeringen in toe:voegen-binnen de Generatie van het Document &#x200B;](assets/nda_3.png)

Dit zijn de functies in het veld `authorizedSigner` . Andere velden worden omlopen en u kunt de weergave in Microsoft Word uitbreiden. De invoegtoepassing biedt ook geavanceerde gegevensopties, zoals tabellen, lijsten, berekende waarden en meer.

## Tags maken

Voel vrij om een malplaatje te creëren of een [&#x200B; bestaand malplaatje &#x200B;](https://developer.adobe.com/document-services/apis/doc-generation#sample-blade) in Microsoft Word in te voeren. Nadat u het document hebt ingesteld, kunt u codes toevoegen aan elk veld door op de bijbehorende tokens in de invoegtoepassing te klikken.

De volgende sjabloon in een Microsoft Word-bestand:

![&#x200B; Screenshot van steekproefmalplaatje &#x200B;](assets/nda_4.png)

Dit bestand bevat verschillende tags. Wanneer u het programma uitvoert, worden deze velden gevuld met de leveranciersinformatie.

Tagger voor het genereren van documenten kan worden geïntegreerd met Adobe Sign API. Dankzij deze integratie kunt u automatisch Sign-tekstcodes maken, zodat het gegenereerde document ter ondertekening naar Adobe Sign kan worden verzonden.

## NDA genereren voor leveranciers

In de voorbeeldtoepassing hebt u mappen voorbereid voor de invoer en uitvoer. Zoals eerder vermeld, gebruikt u JSON-bestanden, zodat er twee bestanden zijn om de beschikbare leveranciers in het systeem weer te geven. De bestanden worden weergegeven in een formulier dat wordt afgedrukt op de browser:

```
<h1><b>NDA</b>: Generate for vendor.</h1>
<hr />
<p>Following ({{files.length}}) vendors are ready, select to generate NDA and deliver for signature:</p>
<form method="POST">
<ul>
{{#each files }}
<li><input type="checkbox" name="vendor" value="{{this}}" id="file-{{@index}}" /> <label for="file-{{@index}}">{{this}}</label></li>
{{/each}}
</ul>
<input type="submit" value="Create NDA" />
</form>
```

Deze code genereert de volgende gebruikersinterface (UI) in de browser:

![&#x200B; Screenshot van Create NDA gebruikersinterface &#x200B;](assets/nda_5.png)

Als de beheerder een persoon selecteert, gebruikt de app Adobe PDF Services om onderweg de NDA te genereren.

```
async function compileDocFile(json, inputFile, outputPdf) {
try {
// configurations
const credentials = adobe.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("./src/pdftools-api-credentials.json")
.build();
// Capture the credential from app and show create the context
const executionContext = adobe.ExecutionContext.create(credentials);
// create the operation
const documentMerge = adobe.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(json, documentMergeOptions.OutputFormat.PDF);
const operation = documentMerge.Operation.createNew(options);
// Pass the content as input (stream)
const input = adobe.FileRef.createFromLocalFile(inputFile);
operation.setInput(input);
// Async create the PDF
let result = await operation.execute(executionContext);
await result.saveAsFile(outputPdf);
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
```

Gebruik deze code binnen de Uitdrukkelijke router:

```
// Create one report and send it back
try {
console.log(`[INFO] generating the report...`);
const fileContent = fs.readFileSync(`./public/documents/raw/${vendor}`, 'utf-8');
const parsedObject = JSON.parse(fileContent);
await pdf.compileDocFile(parsedObject, `./public/documents/template/Adobe-NDA-Sample.docx`, `./public/documents/processed/output.pdf`);
console.log(`[INFO] sending the report...`);
res.status(200).render("preview", { page: 'nda', filename: 'output.pdf' });
} catch(error) {
console.log(`[ERROR] ${JSON.stringify(error)}`);
res.status(500).render("crash", { error: error });
}
```

U kunt [&#x200B; de volledige steekproefcode &#x200B;](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample) op GitHub bekijken.

Deze code gebruikt een JSON-document en de Microsoft Word-sjabloon in de API-aanroep naar de [!DNL Adobe Acrobat Services] SDK. In het antwoord ontvangt u de uitvoer en slaat u deze op in het bestandssysteem van de app. U kunt het geproduceerde document door:sturen aan uw cliënten via e-mail of hen een voorproef tonen binnen browser gebruikend vrije [&#x200B; Adobe PDF inbedt API &#x200B;](https://developer.adobe.com/document-services/apis/pdf-embed).

Met deze aanroep wordt het volgende NDA-document gemaakt:

![&#x200B; Screenshot van de NDA documentvoorproef &#x200B;](assets/nda_6.png)

[!DNL Adobe Acrobat Services] API&#39;s voegen inhoud in om een PDF-document te maken. Zonder deze hulpmiddelen, zou u de code kunnen moeten schrijven om de documenten van het Bureau te verwerken en met ruwe PDF dossierformaten te werken. Met behulp van Adobe PDF Services kunt u al deze stappen uitvoeren met één API-aanroep.

Gebruik nu [&#x200B; Adobe Sign API &#x200B;](https://developer.adobe.com/adobesign-api/) om handtekeningen op NDAs te verzoeken en het definitieve, ondertekende document aan alle partijen te leveren. Adobe Sign brengt u [&#x200B; op de hoogte gebruikend een Webhook &#x200B;](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/webhooks.md). Als u luistert naar deze webhook, kunt u de status van de NDA ophalen.

Voor een diepere verklaring van het proces van Adobe Sign, [&#x200B; raadpleeg de documentatie &#x200B;](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html) of lees dit diepgaande blogpost.

## Volgende stappen

In deze praktische zelfstudie werd de Adobe voor documentgeneratie gebruikt om PDF-documenten dynamisch te genereren met Microsoft Word-sjablonen en JSON-gegevensbestanden. De toe:voegen-binnen hielp aan [&#x200B; automatisch NDAs &#x200B;](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/nda-creation) te creëren die voor elke partij wordt aangepast, dan verzamel handtekeningen gebruikend Teken API.

U kunt deze technieken gebruiken om dynamisch uw eigen NDA&#39;s of andere documenten te maken, waardoor uw team tijd heeft om zich te concentreren op productief werk. Ontdek [[!DNL Adobe Acrobat Services] &#x200B;](https://developer.adobe.com/document-services/apis/pdf-services) om API&#39;s en SDK&#39;s te zoeken voor uw taal en runtime van uw keuze, zodat u rechtstreeks PDF-functies aan uw toepassingen kunt toevoegen om snel PDF-documenten te maken. [&#x200B; begin &#x200B;](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) met een zes maanden vrije proef toen
[&#x200B; betaal-als-u-gaat &#x200B;](https://developer.adobe.com/document-services/pricing/main) voor slechts $0.05 per documenttransactie.
