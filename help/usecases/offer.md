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
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '1794'
ht-degree: 2%

---

# Brieven van werknemersaanbiedingen beheren

![Hoofdlettergebruik hoofdbanner gebruiken](assets/UseCaseOfferHero.jpg)

Aanbiedingsbrieven van werknemers zijn een van de eerste ervaringen die medewerkers hebben met je organisatie. Als gevolg hiervan wil je ervoor zorgen dat je aanbiedingsbrieven onmerkbaar zijn, maar je wilt niet elke keer een letter in je tekstverwerker maken. [!DNL Adobe Acrobat Services] API&#39;s bieden een snelle, eenvoudige en effectieve manier om belangrijke onderdelen van [aanbiedingsbrieven genereren en leveren aan nieuwe werknemers](https://www.adobe.io/apis/documentcloud/dcsdk/employee-offer-letters.html).

## Wat je kunt leren

Dit hands-on leerprogramma loopt door vestiging een Uitdrukkelijke project van de Knoop dat een Webvorm voor een gebruiker toont om met werknemersdetails te bevolken. Deze details gebruiken [!DNL Acrobat Services] via het web om een aanbiedingsbrief te genereren als een PDF die ter ondertekening aan een klant kan worden geleverd met Adobe Sign API.

## Relevante API&#39;s en bronnen

* [PDF Services API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Adobe-API voor documentgeneratie](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [Adobe Sign-API](https://www.adobe.io/apis/documentcloud/sign.html)

* [Invoegtoepassing Word voor het genereren van documenten](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=docgen-addin)

* [Projectvoorbeeld](https://www.adobe.io/apis/documentcloud/dcsdk/employee-offer-letters.html)

## Aan de slag

[Node.js](https://nodejs.org/) is het programmeerplatform. Het wordt geleverd met een enorme reeks bibliotheken, zoals de Express-webserver. [Download Node.js](https://nodejs.org/en/download/) en voer de stappen uit om deze geweldige open-source ontwikkelomgeving te installeren.

Als u de API voor het genereren van Adobe-documenten wilt gebruiken in Node.js, gaat u naar de [API voor documentgeneratie](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html) om uw account te openen of u aan te melden voor een nieuwe account. Uw account is [gratis voor zes maanden, vervolgens betaal naar keuze](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) voor slechts USD 0,05 per documenttransactie, zodat je deze zonder risico kunt uitproberen en vervolgens alleen betalen wanneer je bedrijf groeit.

Na aanmelding bij de [Adobe Developer Console](https://console.adobe.io/), klik op **[!UICONTROL Nieuw project maken]**. Het project heeft standaard de naam &quot;Project 1&quot;. Klik op de knop **[!UICONTROL Project bewerken]** en wijzig de naam in &quot;Offer Letter Generator&quot;. In het midden van het scherm is een **[!UICONTROL Aan de slag met uw nieuwe project]** sectie. Ga als volgt te werk om de beveiliging van uw project in te schakelen:

Klikken **API toevoegen**. U ziet een aantal API&#39;s waaruit u kunt kiezen. In het dialoogvenster **[!UICONTROL Filteren op product]** sectie, selecteert u **[!UICONTROL Document Cloud]** en klik vervolgens op **[!UICONTROL Volgende]**.

Genereer nu aanmeldingsgegevens voor toegang tot de API. De referenties hebben de vorm van een JSON-webtoken ([JWT](https://jwt.io/)): een open standaard voor veilige communicatie. Als u bekend bent met JWT en al sleutels hebt gegenereerd, kunt u hier uw openbare sleutel uploaden. U kunt ook doorgaan door **Optie 1** om Adobe de sleutels voor u te laten genereren.

![Screenshot van het genereren van referenties](assets/offer_1.png)

Klik op de knop **[!UICONTROL Hoofdpaar genereren]** knop. U kunt een bestand config.zip downloaden. Pak het archiefbestand uit. Het bevat twee bestanden: certificate_pub.crt en private.key. Zorg ervoor dat deze gegevens beveiligd blijven, aangezien deze uw persoonlijke gegevens bevatten en kunnen worden gebruikt om onbetrouwbare documenten te genereren als u er geen controle over hebt.

Klik op **[!UICONTROL Volgende]**. Nee, hiermee wordt toegang tot de PDF Generation API ingeschakeld. Op de **[!UICONTROL Productprofielen selecteren]** scherm, controleren **[!UICONTROL Enterprise PDF Services Developer]** en klik op de knop **[!UICONTROL geconfigureerde API opslaan]** knop. U kunt nu de API gaan gebruiken.

## Het project opzetten

Stel een Node-project in om uw code uit te voeren. In dit voorbeeld wordt [Visual Studio Code](https://code.visualstudio.com/) (VS Code) als editor. Maak een map met de naam &quot;letter-generator&quot; en open deze in de VS-code. Van de **[!UICONTROL Bestand]** menu, selecteert u **[!UICONTROL Terminal]** \> **[!UICONTROL Nieuwe terminal]** om een shell in deze map te openen. Controleer of Node is geïnstalleerd en op uw pad door het volgende in te voeren:

```
node -v
```

U zou de versie van Knoop moeten zien u installeerde.

Nu uw ontwikkelomgeving is geïnstalleerd, kunt u uw project maken.

Eerst, initialiseer het project gebruikend de Manager van het Pakket van de Knoop (npm). Typ het volgende:

```
npm init
```

U wordt gesteld sommige vragen over uw project van de Knoop. U kunt de meeste van deze vragen overslaan, maar zorg ervoor dat de projectnaam &quot;letter-generator&quot;is en het ingangspunt is **index.js**. Selecteren **Ja** om de initialisatie van het project te voltooien.

U hebt nu het bestand package.json. Knooppunt gebruikt dit bestand om uw project te ordenen. Voordat u index.js maakt, moet u bibliotheken met Adobe toevoegen met de volgende opdracht:

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

Merk op krijg route terugkeert en **index.html** bestand. Laten we een HTML-bestand met die naam en het volgende eenvoudige formulier maken. U kunt CSS-stijlen en andere ontwerpelementen later toevoegen wanneer u dat nodig acht. Dit formulier bevat de basisgegevens van de kandidaat voor het genereren van een welkomstbrief:

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

Het bericht &#39;Kandidaataanbiedingsbrief-app luistert op poort 8000&#39; moet worden weergegeven. Als u uw browser opent voor <http://localhost:8000/>moet het formulier er als volgt uitzien:

![Screenshot van webformulier](assets/offer_2.png)

U ziet dat het formulier zichzelf plaatst. Als u gegevens invult en op **Letter genereren** u zou de volgende informatie op de console moeten zien:

```
Got body: { firstname: 'John',
lastname: 'Doe',
salary: '887888',
startdate: '2021-04-01' }
```

U vervangt deze console het registreren met een Webdienst vraag aan [!DNL Acrobat Services]. Eerst moet u een JSON-gebaseerd model van de informatie maken. De indeling van dit model ziet er als volgt uit:

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

Er moet veel code uitpakken. Laten we eerst het hoofdgedeelte nemen: de `documentMergeOperation`. In deze sectie kunt u uw JSON-gegevens samenvoegen met een Word-documentsjabloon. U kunt de [voorbeeld op de Adobe-site](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html#sample-blade) ter referentie, maar laten we je eigen eenvoudige voorbeeld maken. Open Word en maak een nieuw leeg document. Je kunt het zo aanpassen als je wilt, maar in ieder geval iets als dit:

Beste X,

We bieden je graag een positie voor $X per jaar. De startdatum is X.

Welkom

Sla het document op als &quot;OfferLetter-Template.docx&quot; in een map met de naam &quot;resources&quot; in de hoofdmap van uw project. Let op de drie X&#39;s in het document. Deze Xs zijn tijdelijke plaatsaanduidingen voor uw JSON-gegevens. Hoewel u een speciale syntaxis kunt gebruiken om deze plaatsaanduidingen te vervangen, biedt Adobe een invoegtoepassing voor Word die deze taak vereenvoudigt. Ga naar de Adobe om de invoegtoepassing te installeren [Invoegtoepassing Word voor het genereren van documenten](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html?view=docgen-addin) site.

Klik in uw OfferLetter-Template op de nieuwe **Documenten genereren** knop. Er wordt een zijpaneel geopend. Klik op **Aan de slag**. U hebt een tekstgebied om in de JSON-voorbeeldgegevens te plakken. Kopieer het &#39;offer-data&#39;-fragment van JSON van boven naar het tekstgebied. Het zou als het volgende moeten kijken:

![Screenshot van letter en code](assets/offer_3.png)

Klik op de knop **Labels genereren** knop. U krijgt een vervolgkeuzemenu met codes die u in de juiste punten in het document wilt invoegen. Markeer de eerste X in het document en selecteer **[!UICONTROL firstname]**. Klikken **[!UICONTROL Tekst invoegen]** en &quot;Beste X,&quot; wordt gewijzigd in &quot;Beste ```{{`offer_letter`.firstname}}```,&quot;. Dit label is de juiste indeling voor `documentMergeOperation`. Voeg de resterende drie tags toe aan de juiste Xs. Vergeet niet OfferLetter-template.docx op te slaan. Het moet er als volgt uitzien:

Beste ```{{`offer_letter`.firstname}} {{`offer_letter`.lastname}}```,

We bieden je een positie voor $ ```{{`offer_letter`.salary}}``` een jaar. Uw begindatum wordt ```{{`offer_letter`.startdate}}```.

Welkom

De Word-sjabloon heeft nu markeringen die overeenkomen met de JSON-indeling. Bijvoorbeeld: ```{{`offer_letter`.`firstname`}}``` aan het begin van een Word-document wordt vervangen door de waarde in het gedeelte &quot;firstname&quot; van de JSON-gegevens.

Terug naar uw `generateLetter` functie. Om uw REST vraag te beveiligen, maak een nieuw dossier met de naam pdftools-api-credentials.json in de projectwortel. Plak de volgende JSON-gegevens in en pas deze aan met gegevens uit de sectie Service Account (JWT) van uw [Developer Console](https://console.adobe.io/).

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

* De client-id, het clientgeheim en de organisatie-id kunnen rechtstreeks vanuit de **[!UICONTROL Referentiegegevens]** van de console.

* De account-id is de **Technische account-id**.

* Kopieer het bestand private.key dat u eerder hebt gegenereerd naar het project en typ de naam ervan in de sectie private_key_file van het bestand pdftools-api-credentials.json. Indien gewenst, kunt u hier een pad naar het bestand met de persoonlijke sleutel plaatsen. Vergeet niet de software veilig te houden omdat deze eenmaal verkeerd kan worden gebruikt.

Ga terug naar uw **[!UICONTROL Kandidaatgegevens invoeren]** webformulier en sommige gegevens plaatsen. Het duurt even omdat het document moet worden gedownload van Adobe, maar u moet een bestand met de naam OfferLetter.pdf hebben in een nieuwe map met de naam output.

## Volgende stappen

Dat is het! Dit is nog maar het begin. Als u de geavanceerde sectie van het tabblad Documentgeneratie van de Word-invoegtoepassing bestudeert, ziet u dat niet alle plaatsaanduidingsmarkeringen van de bijbehorende JSON-gegevens afkomstig zijn. U kunt ook handtekeninglabels toevoegen. Met deze tags kunt u het resulterende document naar [Adobe Sign](https://acrobat.adobe.com/ca/en/sign.html) voor levering aan en ondertekening bij de nieuwe werknemer. Lees Getting Started met Adobe Sign API voor meer informatie over hoe je dit doet. Dit proces is vergelijkbaar omdat u REST-aanroepen gebruikt die zijn beveiligd met een JWT-token.

Het bovenstaande voorbeeld met één document kan worden gebruikt als basis voor een toepassing wanneer een organisatie [seizoensgebonden werving opvoeren](https://www.adobe.io/apis/documentcloud/dcsdk/employee-offer-letters.html) van werknemers op meerdere locaties. Zoals aangetoond, is de belangrijkste stroom gegevens van kandidaten door een online toepassing te nemen. De gegevens worden gebruikt om de velden van een aanbiedingsbrief te vullen en deze ter elektronische ondertekening te verzenden.

[!DNL Adobe Acrobat Services] is gedurende zes maanden gratis te gebruiken, dan [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) voor slechts USD 0,05 per documenttransactie, zodat je deze kunt uitproberen en je aanbiedingsbrief kunt schalen naarmate je bedrijf groeit. Aan [aan de slag](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
het opbouwen van je eigen sjablonen, [aanmelden bij uw ontwikkelaarsaccount](https://www.adobe.io/).
