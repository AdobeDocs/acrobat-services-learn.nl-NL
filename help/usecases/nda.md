---
title: Een NDA maken
description: Leer hoe je een dynamische NDA-PDF maakt voor samenwerking
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8098.jpg
kt: 8098
exl-id: f4ec0182-a46e-43aa-aea3-bf1d19f1a4ec
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '1164'
ht-degree: 3%

---

# Een NDA maken

![Hoofdlettergebruik hoofdbanner gebruiken](assets/UseCaseNDAHero.jpg)

Organisaties werken samen met externe medewerkers om hun services en producten te bouwen. Een geheimhoudingsovereenkomst (NDA) is een belangrijk onderdeel van deze samenwerking. Het verplicht alle partijen om vertrouwelijke informatie vrij te geven die een van beide entiteiten zou kunnen schaden.

De meest gebruikte NDA-indeling is een PDF-document. Organisaties stellen een NDA op en sturen deze naar alle partijen. Zodra iedereen heeft ondertekend, start hij het contract. In een team met hoge snelheid vertraagt het handmatig maken van PDF de voortgang.

## Wat je kunt leren

In deze praktische zelfstudie wordt uitgelegd hoe u een speciale Microsoft Word NDA-sjabloon voor uw bedrijf kunt maken. Adobe, invoegtoepassing voor Microsoft [Tagger voor het genereren van Adobe-documenten](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo), voegt &quot;tags&quot; in om de dynamische waarden in te voeren. Leer hoe u de JSON-gegevens aan de sjabloon doorgeeft en een dynamische PDF maakt. De resulterende PDF kan aan uw medewerkers in hun browser worden gemaild of getoond, afhankelijk van uw bedrijfsvereisten en doelstellingen. Als je mee wilt doen, heb je slechts een kleine ervaring nodig met Node.js, JavaScript, Express.js, HTML en CSS.

## Relevante API&#39;s en bronnen

Met [!DNL Adobe Acrobat Services]kunt u PDF-documenten direct genereren met behulp van dynamische gegevens. [!DNL Acrobat Services] biedt een reeks PDF-tools, waaronder de API voor het genereren van Adobe-documenten om te automatiseren [NDA-creatie](https://www.adobe.io/apis/documentcloud/dcsdk/nda-creation.html).

* [Adobe-API voor documentgeneratie](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [Adobe Sign-API](https://www.adobe.io/apis/documentcloud/sign.html)

* [Tagger voor het genereren van Adobe-documenten](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo)

* [Projectcode](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample)

* [[!DNL Acrobat Services] toetsen](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getcred)

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

Maak de NDA-sjabloon in een Microsoft Word-document. Adobe PDF Services API verwacht dat het Microsoft Word-document codes bevat waarin de service waarden uit JSON-documenten kan inspuiten. Hoewel de sjabloon hetzelfde is voor alle aanvragen om Adobe, veranderen de dynamische gegevens in JSON. Deze tags helpen in dit geval bij het maken van PDF-documenten voor elke leverancier, door gebruik te maken van één Microsoft Word-sjabloon en het proces te versnellen door het genereren van NDA-documenten te automatiseren.

U kunt de [gratis invoegtoepassing voor het genereren van documenten](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo) naar Microsoft Word. Als u deel uitmaakt van een organisatie, kunt u uw Microsoft Office-beheerder vragen de gratis invoegtoepassing voor iedereen te installeren.

Als u de invoegtoepassing hebt geïnstalleerd, vindt u deze op het tabblad Start onder de categorie Adobe. Als u het tabblad wilt openen, selecteert u **Documenten genereren**:

![Schermafbeelding van de invoegtoepassing voor het genereren van documenten in Word](assets/nda_1.png)

Binnen het tabblad kunt u het JSON-voorbeelddocument uploaden. Dit document kan een voorbeeld zijn, omdat u het alleen gebruikt om een Microsoft Word-sjabloon te maken.

![Screenshot van voorbeeldgegevens in Document Generation add-in](assets/nda_2.png)

Selecteren **Labels genereren** om items weer te geven die u binnen uw sjabloon kunt gebruiken. Hier volgen de eigenschappen die uit de JSON-structuur zijn geëxtraheerd en klaar zijn voor gebruik in de sjabloon:

![Screenshot van tekstlabels in Document Generation add-in](assets/nda_3.png)

Dit zijn de functies van het `authorizedSigner` veld. Andere velden worden omlopen en u kunt de weergave in Microsoft Word uitbreiden. De invoegtoepassing biedt ook geavanceerde gegevensopties, zoals tabellen, lijsten, berekende waarden en meer.

## Tags maken

Maak een sjabloon of importeer een [bestaand sjabloon](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html#sample-blade) naar Microsoft Word. Nadat u het document hebt ingesteld, kunt u codes toevoegen aan elk veld door op de bijbehorende tokens in de invoegtoepassing te klikken.

De volgende sjabloon in een Microsoft Word-bestand:

![Screenshot van voorbeeldsjabloon](assets/nda_4.png)

Dit bestand bevat verschillende tags. Wanneer u het programma uitvoert, worden deze velden gevuld met de leveranciersinformatie.

Tagger voor documentgeneratie kan worden geïntegreerd met Adobe Sign API. Dankzij deze integratie kunt u automatisch Sign-tekstcodes maken, zodat het gegenereerde document ter ondertekening naar Adobe Sign kan worden verzonden.

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

![Screenshot van de Create NDA-gebruikersinterface](assets/nda_5.png)

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

U kunt [de volledige voorbeeldcode](https://github.com/afzaal-ahmad-zeeshan/adobe-docugen-sample) op GitHub.

Deze code gebruikt een JSON-document en de Microsoft Word-sjabloon in de API-aanroep naar de [!DNL Adobe Acrobat Services] SDK. In het antwoord ontvangt u de uitvoer en slaat u deze op in het bestandssysteem van de app. U kunt het gegenereerde document via e-mail doorsturen naar uw klanten of ze met de gratis [Adobe PDF Embed API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

Met deze aanroep wordt het volgende NDA-document gemaakt:

![Screenshot van de NDA-documentvoorvertoning](assets/nda_6.png)

[!DNL Adobe Acrobat Services] API&#39;s voegen inhoud in om een PDF-document te maken. Zonder deze hulpmiddelen, zou u de code kunnen moeten schrijven om de documenten van het Bureau te verwerken en met ruwe PDF dossierformaten te werken. Met behulp van Adobe PDF Services kunt u al deze stappen uitvoeren met één API-aanroep.

Nu gebruiken [Adobe Sign API](https://www.adobe.io/apis/documentcloud/sign.html) om handtekeningen op te vragen bij de NDA&#39;s en het definitieve, ondertekende document aan alle partijen te leveren. Adobe Sign brengt u op de hoogte [met een webhook](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/webhooks.md). Als u luistert naar deze webhook, kunt u de status van de NDA ophalen.

Voor een nadere uitleg van het Adobe Sign-proces: [raadpleeg de documentatie](https://www.adobe.io/apis/documentcloud/sign/docs.html) of lees dit uitgebreide blogbericht.

## Volgende stappen

In deze praktische zelfstudie werd de Adobe-documentgeneratietag gebruikt om PDF-documenten dynamisch te genereren met Microsoft Word-sjablonen en JSON-gegevensbestanden. Dankzij de invoegtoepassing [automatisch NDA&#39;s maken](https://www.adobe.io/apis/documentcloud/dcsdk/nda-creation.html) worden aangepast voor elke partij en verzamel vervolgens handtekeningen met de Sign-API.

U kunt deze technieken gebruiken om dynamisch uw eigen NDA&#39;s of andere documenten te maken, waardoor uw team tijd heeft om zich te concentreren op productief werk. Verkennen [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html) om API&#39;s en SDK&#39;s voor uw taal en runtime van uw keuze te vinden, zodat u PDF-functies rechtstreeks aan uw toepassingen kunt toevoegen om snel PDF-documenten te maken. [Aan de slag](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) met een gratis proefversie van zes maanden
[pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) voor slechts USD 0,05 per documenttransactie.
