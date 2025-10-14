---
title: Brieven werknemersaanbieding beheren
description: Leer hoe je een aanbiedingsbrief genereert die ter ondertekening aan een nieuwe medewerker kan worden bezorgd
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8096
thumbnail: KT-8096.jpg
exl-id: 92f955f0-add5-4570-aa3a-ea63055dadb2
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1714'
ht-degree: 0%

---

# Brieven van werknemersaanbiedingen beheren

![&#x200B; Hoofdletterbanner van het Gebruik &#x200B;](assets/UseCaseOfferHero.jpg)

Aanbiedingsbrieven van werknemers zijn een van de eerste ervaringen die medewerkers hebben met je organisatie. Als gevolg hiervan wil je ervoor zorgen dat je aanbiedingsbrieven onmerkbaar zijn, maar je wilt niet elke keer een letter in je tekstverwerker maken. [!DNL Adobe Acrobat Services] APIs biedt een snelle, gemakkelijke, en efficiënte manier aan om zeer belangrijke delen van [&#x200B; te behandelen die en aanbiedingsbrieven aan nieuwe werknemers produceren &#x200B;](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/employee-offer-letters).

## Wat je kunt leren

Dit hands-on leerprogramma loopt door vestiging een Uitdrukkelijke project van de Knoop dat een Webvorm voor een gebruiker toont om met werknemersdetails te bevolken. Deze gegevens gebruiken [!DNL Acrobat Services] via het web om een aanbiedingsbrief te genereren als een PDF die ter ondertekening aan een klant kan worden bezorgd met behulp van de Adobe Sign API.

## Relevante API&#39;s en bronnen

* [&#x200B; de Diensten API van de PDF &#x200B;](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [&#x200B; de Generatie API van het Document van de Adobe &#x200B;](https://developer.adobe.com/document-services/apis/doc-generation)

* [&#x200B; Adobe Sign API &#x200B;](https://developer.adobe.com/adobesign-api/)

* [&#x200B; toe:voegen-binnen van de Tagger van de Generatie van het Document &#x200B;](https://developer.adobe.com/document-services/docs/overview/document-generation-api/wordaddin)

* [&#x200B; steekproef van het Project &#x200B;](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/employee-offer-letters)

## Aan de slag

[&#x200B; Node.js &#x200B;](https://nodejs.org/) is het programmeringsplatform. Het wordt geleverd met een enorme reeks bibliotheken, zoals de Express-webserver. [&#x200B; Download Node.js &#x200B;](https://nodejs.org/en/download/) en volg de stappen om deze grote open-bronontwikkelomgeving te installeren.

Om de Generatie API van het Document van de Adobe in Node.js te gebruiken, ga naar de [&#x200B; Generatie API van het Document &#x200B;](https://developer.adobe.com/document-services/apis/doc-generation) plaats om tot uw rekening toegang te hebben of omhoog voor nieuwe te ondertekenen. Uw rekening is [&#x200B; vrij voor zes maanden dan betaal-als-u-gaat &#x200B;](https://developer.adobe.com/document-services/pricing/main) voor slechts $0.05 per documenttransactie, zodat kunt u het uit risicovrij proberen dan slechts betaal aangezien uw bedrijf groeit.

Na het ondertekenen in de [&#x200B; Console van de Ontwikkelaar van Adobe &#x200B;](https://developer.adobe.com/console/), klik **[!UICONTROL Nieuw Project]** creëren. Het project heeft standaard de naam &quot;Project 1&quot;. Klik de **[!UICONTROL Edit knoop van het Project]** en verander de naam in &quot;de Generator van de Brief van de Aanbieding.&quot; In het centrum van het scherm is a **[!UICONTROL wordt begonnen met uw Nieuwe sectie van het Project]**. Ga als volgt te werk om de beveiliging van uw project in te schakelen:

Klik **voeg API** toe. U ziet een aantal API&#39;s waaruit u kunt kiezen. In de **[!UICONTROL Filter door de sectie van het Product]**, uitgezochte **[!UICONTROL Document Cloud]**, dan klik **[!UICONTROL daarna]**.

Genereer nu aanmeldingsgegevens voor toegang tot de API. De geloofsbrieven zijn in de vorm van een Token van het Web JSON ([&#x200B; JWT &#x200B;](https://jwt.io/)): een open norm voor veilige mededeling. Als u bekend bent met JWT en al sleutels hebt gegenereerd, kunt u hier uw openbare sleutel uploaden. Alternatief, ga door **Optie 1 te selecteren** te hebben Adobe produceren de sleutels voor u.

![&#x200B; Screenshot van het produceren van geloofsbrieven &#x200B;](assets/offer_1.png)

Klik **[!UICONTROL Genereer keypair]** knoop. U kunt een bestand config.zip downloaden. Pak het archiefbestand uit. Het bevat twee bestanden: certificate_pub.crt en private.key. Zorg ervoor dat deze gegevens beveiligd blijven, aangezien deze uw persoonlijke gegevens bevatten en kunnen worden gebruikt om onbetrouwbare documenten te genereren als u er geen controle over hebt.

Klik op **[!UICONTROL Volgende]**. Nee, hiermee wordt toegang tot de PDF Generation API ingeschakeld. Op het **[!UICONTROL Uitgezochte productprofielen]** scherm, controleer **[!UICONTROL Ontwikkelaar van de Diensten van de PDF van de Onderneming]**, en klik op **[!UICONTROL sparen gevormde API]** knoop. U kunt nu de API gaan gebruiken.

## Het project opzetten

Stel een Node-project in om uw code uit te voeren. Dit voorbeeld gebruikt [&#x200B; Code van Visual Studio &#x200B;](https://code.visualstudio.com/) (de Code van VS) als redacteur. Maak een map met de naam &quot;letter-generator&quot; en open deze in de VS-code. Van het **[!UICONTROL menu van het Dossier]**, selecteer **[!UICONTROL Eind]** \> **[!UICONTROL Nieuwe Eind]** om shell in deze omslag te openen. Controleer of Node is geïnstalleerd en op uw pad door het volgende in te voeren:

```
node -v
```

U zou de versie van Knoop moeten zien u installeerde.

Nu uw ontwikkelomgeving is geïnstalleerd, kunt u uw project maken.

Eerst, initialiseer het project gebruikend de Manager van het Pakket van de Knoop (npm). Typ het volgende:

```
npm init
```

U wordt gesteld sommige vragen over uw project van de Knoop. U kunt de meeste van deze vragen overslaan, maar zorg ervoor de projectnaam &quot;letter-generator&quot;is en het ingangspunt is **index.js**. Selecteer **Ja** om projectinitialisatie te voltooien.

U hebt nu het bestand package.json. Knooppunt gebruikt dit bestand om uw project te ordenen. Voordat u index.js maakt, moet u Adobe bibliotheken toevoegen met de volgende elementen
opdracht:

```
npm install --save @adobe/documentservices-pdftools-node-sdk
```

Er moet een nieuwe map met de naam node_modules aan uw project worden toegevoegd. In deze map worden alle bibliotheken (afhankelijkheden in knooppunt genoemd) gedownload. Het bestand package.json wordt ook bijgewerkt met een verwijzing naar Adobe PDF Services.

Nu wilt u Express installeren als uw lichtgewicht webframework. Voer de volgende opdracht in:

```
npm install express –save
```

Net als voorheen wordt de sectie voor afhankelijkheden van package.json overeenkomstig bijgewerkt.

## Een aanbiedingsbriefsjabloon maken

Maak nu in de hoofdmap van het project het bestand &quot;app.js&quot;. Laten we daar de volgende startcode in zetten:

```
const express = require('express');
const bodyParser = require('body-parser');
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk')
const path = require('path');
const app = express();
const port = 8000;
app.use(bodyParser.urlencoded({ extended: true }));
app.get('/', (req, res) => {
res.sendFile(path.join(__dirname + '/index.html'));
});
app.post('/', (req, res) => {
console.log('Got body:', req.body);
res.sendStatus(200);
});
app.listen(port, () => {
console.log(`Candidate offer letter app listening on port ${port}!`)
});
```

Merk op krijgt route een {**dossier 0} index.html terugkeert.** Laten we een HTML-bestand met die naam en het volgende eenvoudige formulier maken. U kunt CSS-stijlen en andere ontwerpelementen later toevoegen wanneer u dat nodig acht. Dit formulier bevat de basisgegevens van de kandidaat voor het genereren van een welkomstbrief:

```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Offer Letter Generator</title>
</head>
<body>
<h1>Enter Candidate Details</h1>
<form action="" method="post">
<div>
<label for="firstname">First name: </label>
<input type="text" name="firstname" id="firstname" required>
</div>
<div>
<label for="lastname">Last name: </label>
<input type="text" name="lastname" id="lastname" required>
</div>
<div>
<label for="salary">Salary ($): </label>
<input type="number" name="salary" id="salary" required>
</div>
<div>
<label for="startdate">Start Date: </label>
<input type="date" name="startdate" id="startdate" required>
</div>
<div>
<input type="submit" value="Generate Letter">
</div>
</form>
</body>
</html>
```

Voer de webserver uit met de volgende opdracht:

```
node app.js
```

Het bericht &#39;Kandidaataanbiedingsbrief-app luistert op poort 8000&#39; moet worden weergegeven. Als u uw browser opent voor <http://localhost:8000/> , moet het formulier er als volgt uitzien:

![&#x200B; Screenshot van Webvorm &#x200B;](assets/offer_2.png)

U ziet dat het formulier zichzelf plaatst. Als u in gegevens invult en **klikt produceer Brief,** u de volgende informatie over de console zou moeten zien:

```
Got body: { firstname: 'John',
lastname: 'Doe',
salary: '887888',
startdate: '2021-04-01' }
```

U vervangt de logboekregistratie van deze console door een webservice-aanroep van [!DNL Acrobat Services] . Eerst moet u een JSON-gebaseerd model van de informatie maken. De indeling van dit model ziet er als volgt uit:

```
{
    "offer_letter": {
    "firstname": "John",
    "lastname": "Doe",
    "salary": "887888",
    "startdate": "2021-04-01"
    }
}
```

U kunt dit model desgewenst verder uitwerken, maar voor deze zelfstudie houdt u zich aan dit eenvoudige voorbeeld. Er is geen validatie voor dit formulier omdat dit buiten het bereik van dit artikel valt. Als u de formulierhoofdtekst wilt converteren naar het hierboven beschreven gegevensmodel, wijzigt u de methode app.post handler zodat deze de volgende code heeft:

```
app.post('/', (req, res) => {
const docModel = {'offer_letter': req.body};
generateLetter(docModel);
res.sendStatus(200);
});
```

De eerste regel plaatst uw JSON-gegevens in de gewenste indeling. Nu geeft u deze gegevens door aan een functie generateLetter. Stop uw server en plak de volgende code aan het einde van app.js. Deze code neemt een Word-document als een sjabloon en vult plaatsaanduidingen in met informatie uit een JSON-document.

```
// Letter generation function
function generateLetter(jsonDataForMerge) {
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Create an ExecutionContext using credentials
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials);
// Create a new DocumentMerge options instance
const documentMerge = PDFToolsSdk.DocumentMerge,
documentMergeOptions = documentMerge.options,
options = new documentMergeOptions.DocumentMergeOptions(jsonDataForMerge,
documentMergeOptions.OutputFormat.PDF);
// Create a new operation instance using the options instance
const documentMergeOperation = documentMerge.Operation.createNew(options)
// Set operation input document template from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile(
'resources/OfferLetter-Template.docx');
documentMergeOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
documentMergeOperation.execute(executionContext)
.then(result => result.saveAsFile('output/OfferLetter.pdf'))
.catch(err => {
if(err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log(
'Exception encountered while executing operation', err);
} else {
console.log(
'Exception encountered while executing operation', err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
```

Er moet veel code uitpakken. Laten we eerst het belangrijkste deel nemen: de `documentMergeOperation` . In deze sectie kunt u uw JSON-gegevens samenvoegen met een Word-documentsjabloon. U kunt het [&#x200B; voorbeeld op de plaats van de Adobe &#x200B;](https://developer.adobe.com/document-services/apis/doc-generation#sample-blade) als verwijzing gebruiken, maar laten uw eigen eenvoudig voorbeeld maken. Open Word en maak een nieuw, leeg document. Je kunt het zo aanpassen als je wilt, maar in ieder geval iets als dit:

Beste X,

We bieden je graag een positie voor $X per jaar. De startdatum is X.

Welkom

Sla het document op als &quot;OfferLetter-Template.docx&quot; in een map met de naam &quot;resources&quot; in de hoofdmap van uw project. Let op de drie X&#39;s in het document. Deze Xs zijn tijdelijke plaatsaanduidingen voor uw JSON-gegevens. Hoewel u een speciale syntaxis kunt gebruiken om deze plaatsaanduidingen te vervangen, biedt Adobe een invoegtoepassing voor Word die deze taak vereenvoudigt. Om toe:voegen-binnen te installeren, ga naar de Adobe [&#128279;](https://developer.adobe.com/document-services/docs/overview/document-generation-api/wordaddin) plaats van de Tagger van Word van de Generatie van het Document toe:voegen-binnen.

In uw OfferLetter-Malplaatje, klik de nieuwe **knoop van de Generatie van het Document**. Er wordt een zijpaneel geopend. Klik **worden begonnen**. U hebt een tekstgebied om in de JSON-voorbeeldgegevens te plakken. Kopieer het &#39;offer-data&#39;-fragment van JSON van boven naar het tekstgebied. Het zou als het volgende moeten kijken:

![&#x200B; Screenshot van brief en code &#x200B;](assets/offer_3.png)

Klik **produceer de knoop van Markeringen**. U krijgt een vervolgkeuzemenu met codes die u in de juiste punten in het document wilt invoegen. Markeer eerste X in het document en selecteer **[!UICONTROL firstname]**. Klik **[!UICONTROL Tekst van het Tussenvoegsel]** en &quot;Beste X,&quot;wordt veranderd in &quot;Beste ```{{`offer_letter`.firstname}}```,&quot;. Dit label is de juiste notatie voor `documentMergeOperation` . Voeg de resterende drie tags toe aan de juiste Xs. Vergeet niet OfferLetter-template.docx op te slaan. Het moet er als volgt uitzien:

Beste ```{{`offer_letter`.firstname}} {{`offer_letter`.lastname}}```,

We bieden je een positie voor $ ```{{`offer_letter`.salary}}``` per jaar. Uw begindatum is ```{{`offer_letter`.startdate}}``` .

Welkom

De Word-sjabloon heeft nu markeringen die overeenkomen met de JSON-indeling. Zo wordt ```{{`offer_letter`.`firstname`}}``` aan het begin van een Word-document vervangen door de waarde in het gedeelte &quot;firstname&quot; van de JSON-gegevens.

Terug naar uw `generateLetter` functie. Om uw REST vraag te beveiligen, maak een nieuw dossier met de naam pdftools-api-credentials.json in de projectwortel. Plak in de volgende JSON- gegevens en pas het met details van de sectie van de Rekening van de Dienst (JWT) van uw [&#x200B; Console van de Ontwikkelaar &#x200B;](https://developer.adobe.com/console/) aan.

```
{
"client_credentials": {
"client_id": "<YOUR_CLIENT_ID>",
"client_secret": "<YOUR_CLIENT_SECRET>"
},
"service_account_credentials": {
"organization_id": "<YOUR_ORGANIZATION_ID>",
"account_id": "<YOUR_TECHNICAL_ACCOUNT_ID>",
"private_key_file": "<PRIVATE_KEY_FILE_PATH>"
}
}
```

* De cliënt identiteitskaart, het cliëntgeheim, en organisatie identiteitskaart kunnen direct van de **[!UICONTROL Credentials sectie van details]** van de console worden gekopieerd.

* De rekening identiteitskaart is **Technische identiteitskaart van de Rekening**.

* Kopieer het bestand private.key dat u eerder in het project hebt gegenereerd en typ de naam ervan in de sectie private_key_file van het deelvenster
Het bestand pdftools-api-credentials.json. Indien gewenst, kunt u hier een pad naar het bestand met de persoonlijke sleutel plaatsen. Vergeet niet de software veilig te houden omdat deze eenmaal verkeerd kan worden gebruikt.

Om een PDF met de gegevens te produceren JSON die binnen worden ingevuld, ga terug naar uw **[!UICONTROL Kandidaatdetails]** Webvorm ingaan en sommige gegevens posten. Het duurt even omdat het document moet worden gedownload van de Adobe, maar u moet een bestand met de naam OfferLetter.pdf in een nieuwe map hebben met de naam output.

## Volgende stappen

Dat is het! Dit is nog maar het begin. Als u de geavanceerde sectie van het tabblad Documentgeneratie van de Word-invoegtoepassing bestudeert, ziet u dat niet alle plaatsaanduidingsmarkeringen van de bijbehorende JSON-gegevens afkomstig zijn. U kunt ook handtekeninglabels toevoegen. Deze markeringen staan u toe om het resulterende document te nemen en het te uploaden aan [&#x200B; Adobe Sign &#x200B;](https://www.adobe.com/ca/sign.html) voor levering en het ondertekenen aan nieuwe werknemer. Lees Getting Started met Adobe Sign API voor meer informatie over hoe je dit doet. Dit proces is vergelijkbaar omdat u REST-aanroepen gebruikt die zijn beveiligd met een JWT-token.

Het enige die documentvoorbeeld hierboven wordt verstrekt kan als basis voor een toepassing worden gebruikt wanneer een organisatie [&#x200B; omhoog seizoensgebonden het huren &#x200B;](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/employee-offer-letters) van werknemers over veelvoudige plaatsen moet opvoeren. Zoals aangetoond, is de belangrijkste stroom gegevens van kandidaten door een online toepassing te nemen. De gegevens worden gebruikt om de velden van een aanbiedingsbrief te vullen en deze ter elektronische ondertekening te verzenden.

[!DNL Adobe Acrobat Services] is vrij om voor zes maanden te gebruiken, dan [&#x200B; betaal-als-u-gaat &#x200B;](https://developer.adobe.com/document-services/pricing/main) bij slechts $0.05 per documenttransactie, zodat kunt u het proberen en uw workflow van de aanbiedingsbrief schalen aangezien uw zaken groeit. Aan [&#x200B; begin &#x200B;](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
bouwend uw eigen malplaatjes, [&#x200B; teken omhoog uw ontwikkelaarsrekening &#x200B;](https://developer.adobe.com/).
