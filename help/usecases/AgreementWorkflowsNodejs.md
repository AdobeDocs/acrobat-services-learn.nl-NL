---
title: Overeenkomstworkflows in Node.js
description: "[!DNL Adobe Acrobat Services] API's integreren eenvoudig PDF-mogelijkheden in je webapplicaties."
feature: Use Cases
role: Developer
level: Beginner
type: Tutorial
jira: KT-7473
thumbnail: KT-7473.jpg
keywords: Aanbevolen
exl-id: 44a03420-e963-472b-aeb8-290422c8d767
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '2182'
ht-degree: 1%

---

# Overeenkomstworkflows in Node.js

![Hoofdlettergebruik hoofdbanner gebruiken](assets/UseCaseAgreementHero.jpg)

Veel bedrijfstoepassingen en -processen vereisen documentatie zoals voorstellen en overeenkomsten. PDF-documenten zorgen ervoor dat bestanden veiliger en minder wijzigbaar zijn. Ze bieden ook ondersteuning voor digitale handtekeningen, zodat je klanten hun documenten snel en eenvoudig kunnen voltooien. [!DNL Adobe Acrobat Services] API&#39;s integreren eenvoudig PDF-mogelijkheden in je webapplicaties.

## Wat je kunt leren

In deze praktische zelfstudie leert u hoe u PDF-services kunt toevoegen aan een Node.js-toepassing om een overeenkomstproces te digitaliseren.

## Relevante API&#39;s en bronnen

* [PDF Services API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [PDF Embed-API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Adobe Sign-API](https://www.adobe.io/apis/documentcloud/sign.html)

* [Projectcode](https://github.com/adobe/pdftools-node-sdk-samples)

## Instellen [!DNL Adobe Acrobat Services]

Om aan de slag te gaan, stelt u gebruikersgegevens in voor gebruik [!DNL Adobe Acrobat Services]. Registreer een account en gebruik de [Node.js QuickStart](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#node-js) om uw aanmeldingsgegevens te verifiëren voordat u de functionaliteit integreert in een grotere toepassing.

Ga eerst naar een Adobe-ontwikkelaarsaccount. Dan, op [Aan de slag](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html?ref=getStartedWithServicesSDK) pagina selecteert u de *Aan de slag* onder Nieuwe referenties maken. U kunt zich aanmelden voor een gratis proefversie met 1000 Document Transactions die u in zes maanden kunt gebruiken.

![Afbeelding van nieuwe referenties maken](assets/AWNjs_1.png)

Op de volgende pagina Nieuwe referenties maken wordt u gevraagd te beslissen tussen de PDF Embed-API en de PDF Services-API.

Selecteren *PDF Services API*.

Voer een naam voor de toepassing in en schakel het selectievakje in *Een gepersonaliseerd codevoorbeeld maken*. Als u dit selectievakje inschakelt, worden uw referenties automatisch ingesloten in het codevoorbeeld. Als u dit vak niet inschakelt, moet u uw referenties handmatig aan de toepassing toevoegen.

Selecteren *Node.js* voor het toepassingstype en klik op *Referenties maken*.

Een paar minuten later begint een .zip-bestand te downloaden met een voorbeeldproject met uw referenties. Het pakket Node.js voor [!DNL Acrobat Services] is al opgenomen als onderdeel van de voorbeeldprojectcode.

![Afbeelding van PDF Services API-referenties selecteren](assets/AWNjs_2.png)

## Handmatig het voorbeeldproject configureren

Als u ervoor kiest om geen voorbeeldproject te downloaden van de pagina Nieuwe referenties maken, kunt u het project ook handmatig instellen.

Download de code (zonder uw ingesloten referenties) van [GitHub](https://github.com/adobe/pdftools-node-sdk-samples). Als u deze versie van de code gebruikt, moet u uw referenties toevoegen aan het bestand pdftools-api-credentials.json voordat u het kunt gebruiken:

```
{
  "client_credentials": {
    "client_id": "<client_id>",
    "client_secret": "<client_secret>"
  },
  "service_account_credentials": {
    "organization_id": "<organization_id>",
    "account_id": "<technical_account_id>",
    "private_key_file": "<private_key_file_path>"
  }
}
```

Voor uw eigen toepassing moet u het bestand met de persoonlijke sleutel en de aanmeldingsgegevens naar de toepassingsbron kopiëren.

U moet het pakket Node.js installeren voor [!DNL Acrobat Services]. Gebruik de volgende opdracht om het pakket te installeren:

```
npm install --save @adobe/documentservices-pdftools-node-sdk
```

## Registratie instellen

De voorbeelden hier gebruiken Express voor het toepassingsframework. Ze gebruiken ook log4js voor het vastleggen van toepassingen. Met log4js, kunt u het programma gemakkelijk leiden aan de console of uit aan een dossier:

```
const log4js = require('log4js');
const logger = log4js.getLogger();
log4js.configure( {
    appenders: { fileAppender: { type:'file', filename: './logs/applicationlog.txt'}},
    categories: { default: {appenders: ['fileAppender'], level:'info'}}
});
 
logger.level = 'info';
logger.info('Application started')
```

Met de bovenstaande code worden geregistreerde gegevens naar een bestand in geschreven./logs/applicationlog.txt. Als u het aan de console in plaats daarvan wilt schrijven, kunt u de vraag aan log4js.configure opmerken.

## Word-bestanden converteren naar PDF

Overeenkomsten en voorstellen worden vaak geschreven in een productiviteitstoepassing, zoals Microsoft Word. Als u documenten in deze indeling wilt accepteren en het document wilt converteren naar PDF, kunt u functionaliteit voor uw toepassing toevoegen. Laten we kijken hoe u een document kunt uploaden en opslaan in een Express-toepassing en het kunt opslaan in het bestandssysteem.

Voeg in de HTML van de toepassing een bestandselement en een knop toe om het uploaden te starten:

```
<input type="file" name="source" id="source" />
<button onclick="upload()" >Upload</button>
```

Upload het bestand asynchroon in de JavaScript-code van de pagina met de functie Ophalen:

```
function upload()
{
  let formData = new FormData();
  var selectedFile = document.getElementById('source').files[0];
  formData.append("source", selectedFile);
  fetch('documentUpload', {method:"POST", body:formData});
}
```

Kies een map om de geüploade bestanden te accepteren. De toepassing heeft een pad naar deze map nodig. Zoek het absolute pad met een relatief pad dat is verbonden met \_\_dirname:

```
const uploadFolder = path.join(__dirname, "../uploads");
```

Aangezien het bestand via post wordt verzonden, moet u reageren op een bericht op de server:

```
router.post('/', (req, res, next) => {
  console.log('uploading')
  if(!req.files || Object.keys(req.files).length === 0) {
  return res.status(400).send('No files were uploaded');
  }
    
  const uploadPath = path.join(uploadFolder, req.files.source.name);
  var buffer = req.files.source.data;
  var result = {"success":true};
  fs.writeFile(uploadPath, buffer, 'binary', (err)=> {
    if(err) {
      result.success = false;
    }
    res.json(result);
  });       
});
```

Nadat deze functie is uitgevoerd, wordt het bestand opgeslagen in de uploadmap van de toepassingen en is het beschikbaar voor verdere verwerking.

Zet het bestand vervolgens om van de oorspronkelijke indeling in PDF. De voorbeeldcode die je eerder hebt gedownload, bevat een script met de naam `create-pdf-from-docx.js` voor het converteren van een document naar PDF. De volgende functie: `convertDocumentToPDF`, maakt een geüpload document en zet dit om in een PDF in een andere map:

```
function convertDocumentToPDF(sourcePath, destinationPath)
{    
  try {   
    const credentials = PDFToolsSDK.Credentials
    .serviceAccountCredentialsBuilder()
    .fromFile("pdftools-api-credentials.json")
    .build();
 
    const executionContext = 
      PDFToolsSDK.ExecutionContext.create(credentials),
    createPdfOperation = PDFToolsSDK.CreatePDF.Operation.createNew();
 
    const docxReadableStream = fs.createReadStream(sourcePath);
    const input = PDFToolsSDK.FileRef.createFromStream(
      docxReadableStream, 
      PDFToolsSDK.CreatePDF.SupportedSourceFormat.docx);
    createPdfOperation.setInput(input);
 
    createPdfOperation.execute(executionContext)
    .then(result => result.saveAsFile(destinationPath))
    .catch(err => {        
      logger.erorr('Exception encountered while executing operation');        
    })
  }
  catch(err) {        
    logger.error(err);
  }
}
```

U kunt een algemeen patroon met de code opmerken:

De code bouwt een verificatieobject en een uitvoeringscontext, initialiseert een bewerking en voert de bewerking vervolgens uit met de uitvoeringscontext. U kunt dit patroon in de voorbeeldcode zien.

Door een paar toevoegingen aan de uploadfunctie te maken, zodat deze functie wordt aangeroepen, worden de Word-documenten die gebruikers uploaden nu automatisch geconverteerd naar PDF.

Met de volgende code wordt het doelpad voor de omgezette PDF opgebouwd en wordt de conversie gestart:

```
const documentFolder = path.join(__dirname, "../docs");
var extPosition = req.files.source.name.lastIndexOf('.') - 1;
if(extPosition < 0 ) {
  extPosition = req.files.source.name.length
}
const destinationName = path.join(documentFolder,  
  req.files.source.name.substring(0, extPosition) + '.pdf');
console.log(destinationName);
 
logger.info('converting to ${destinationName}')
  convertDocumentToPDF(uploadPath, destinationName);
```

## Andere bestandstypen omzetten in PDF

De documenttoolkit zet andere indelingen om in PDF, zoals statische HTML, een ander veelvoorkomend documenttype. De toolkit accepteert HTML-documenten die zijn verpakt als een zip-bestand met alle bronnen waarnaar wordt verwezen door het document (CSS-bestanden, afbeeldingen en andere bestanden) in hetzelfde zip-bestand. Het HTML-document zelf moet de naam index.html hebben en in de hoofdmap van het ZIP-bestand worden geplaatst.

Gebruik de volgende code om een .zip-bestand met HTML om te zetten:

```
//Create an HTML to PDF operation and provide the source file to it
htmlToPDFOperation = PDFToolsSdk.CreatePDF.Operation.createNew();     
const input = PDFToolsSdk.FileRef.createFromLocalFile(sourceZipFile);
htmlToPDFOperation.setInput(input);
 
// custom function for setting options
setCustomOptions(htmlToPDFOperation);
 
// Execute the operation and Save the result to the specified location.
htmlToPDFOperation.execute(executionContext)
  .then(result => result.saveAsFile(destinationPdfFile))
  .catch(err => {
    logger.error('Exception encountered while executing operation');
});
```

De functie `setCustomOptions` geeft andere PDF-instellingen op, zoals het paginaformaat. Hier kunt u zien dat de functie het paginaformaat instelt op 11,5 x 11 inch:

```
const setCustomOptions = (htmlToPDFOperation) => {    
  const pageLayout = new PDFToolsSdk.CreatePDF.options.PageLayout();
  pageLayout.setPageSize(11.5, 8);

  const htmlToPdfOptions = 
    new PDFToolsSdk.CreatePDF.options.html.CreatePDFFromHtmlOptions.Builder()
    .includesHeaderFooter(true)
    .withPageLayout(pageLayout)
    .build();
  htmlToPDFOperation.setOptions(htmlToPdfOptions);
};
```

Wanneer u een HTML-document met bepaalde termen opent, ziet u het volgende in de browser:

![Afbeelding van computertermen](assets/AWNjs_3.png)

De bron voor dit document bestaat uit een CSS-bestand en een HTML-bestand:

![Afbeelding van CSS- en HTML-bestand](assets/AWNjs_4.png)

Na het verwerken van het HTML-bestand hebt u dezelfde tekst in de PDF-indeling:

![PDF-bestand met computertermen](assets/AWNjs_5.png)

## Pagina&#39;s toevoegen

Een andere veelvoorkomende bewerking met PDF-bestanden is het toevoegen van pagina&#39;s aan het einde met standaardtekst, zoals een lijst met termen. Met de documentgereedschapset kunt u verschillende PDF-documenten combineren tot één document. Als u een lijst met documentpaden hebt (hier in `sourceFileList`) kunt u de bestandsverwijzingen van elk bestand naar een object toevoegen voor een combinatiebewerking.

Wanneer de combinatiebewerking wordt uitgevoerd, wordt één bestand met de broninhoud geleverd. U kunt `saveAsFile` op het object om het op te slaan bestand te behouden.

```
const executionContext = PDFToolsSDK.ExecutionContext.create(credentials);
var combineOperation = PDFToolsSDK.CombineFiles.Operation.createNew();
 
sourceFileList.forEach(f => {
  var combinedSource = PDFToolsSDK.FileRef.createFromLocalFile(f);
  console.log(f);
  combineOperation.addInput(combinedSource);
});
    
 
combineOperation.execute(executionContext)
  .then(result=>result.saveAsFile(destinationFile))
  .catch(err => {
    logger.error(err.message);
});    
```

## PDF-documenten weergeven

U hebt verschillende bewerkingen uitgevoerd op PDF-bestanden, maar uiteindelijk moet de gebruiker de documenten bekijken. U kunt het document insluiten in een webpagina met behulp van de API voor insluiten via de Adobe.

Voeg op de pagina die de PDF weergeeft een `<div />` -element om het document vast te houden en het een id te geven. U gebruikt deze id binnenkort. Neem op de webpagina een `<script />` verwijzing naar de Adobe JavaScript-bibliotheek:

```
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

Het laatste stukje code dat u nodig hebt, is een functie waarmee het document wordt weergegeven nadat de Adobe PDF Embed API JavaScript is geladen. Als u een melding ontvangt dat het script is geladen via een gebeurtenis adobe_dc_view\_sdk.ready, maakt u een nieuw object AdobeDC.View. Voor dit object zijn uw Client-id en de id van het element nodig die u eerder hebt gemaakt. Zoek uw client-id in het dialoogvenster [Adobe Developer Console](https://console.adobe.io/). Wanneer u de instellingen voor de toepassing bekijkt die u eerder hebt gemaakt toen u referenties genereerde, wordt de client-id daar weergegeven.

![Afbeelding van API-clientsleutel](assets/AWNjs_6.png)

## Andere PDF-opties

De [Demo van Adobe PDF Embed API](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf) Hiermee kunt u een voorvertoning weergeven van de verschillende andere opties voor het insluiten van PDF-documenten.

![Afbeelding van opties voor het insluiten van PDF ](assets/AWNjs_7.png)

U kunt verschillende opties in- en uitschakelen en direct zien hoe ze worden gerenderd. Als je een gewenste combinatie vindt, klik je op de knop *\&lt;/\> Code genereren* om de werkelijke HTML-code te genereren met deze opties.

![Afbeelding van codevoorvertoning](assets/AWNjs_8.png)

## Digitale handtekeningen en beveiliging toevoegen

Zodra een document klaar is, kunt u digitale handtekeningen toevoegen voor goedkeuring met behulp van Adobe Sign. Deze functionaliteit werkt iets anders dan de functionaliteit die u tot nu toe hebt gebruikt. Voor digitale handtekeningen moet een toepassing zijn geconfigureerd om OAuth te gebruiken voor gebruikersverificatie.

De eerste stap bij het instellen van uw toepassing is: [registreer uw toepassing](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/gstarted/create_app.md) om OAuth voor Adobe Sign te gebruiken. Navigeer na aanmelding naar het scherm om toepassingen te maken door op *Account* en open vervolgens de *ADOBE SIGN API* en klik op *API-toepassingen* om de lijst met geregistreerde toepassingen te openen.

![Afbeelding van de eerste stap bij het registreren van uw toepassing](assets/AWNjs_9.png)

Als u een nieuwe toepassingsvermelding wilt maken, klikt u op het plusteken in de rechterbovenhoek.

![Afbeelding van het plusteken in de rechterbovenhoek van het scherm](assets/AWNjs_10.png)

Voer in het venster dat wordt geopend een toepassingsnaam en weergavenaam in. Selecteren *Klant* voor het domein en klik vervolgens op *Opslaan*.

![Afbeelding van de plaats waar de toepassingsnaam en weergavenaam moeten worden ingevoerd](assets/AWNjs_11.png)

Nadat de toepassing is gemaakt, kunt u deze selecteren in de lijst en klikken op *OAuth configureren voor toepassing*. Selecteer de opties. Voer in de URL omleiden de URL voor uw toepassing in. U kunt hier meerdere URL&#39;s invoeren. Voor de toepassing die u test, is de waarde:

```
http://localhost:3000/signed-in 
```

Het proces voor het verkrijgen van een token met OAuth is standaard. Uw toepassing leidt een gebruiker naar een URL voor het aanmelden. Nadat de gebruiker zich heeft aangemeld, wordt deze teruggeleid naar de toepassing met aanvullende informatie in de queryparameters van de pagina.

Voor de aanmeldings-URL moet uw toepassing uw client-id doorgeven, de URL omleiden en een lijst met de vereiste bereiken opgeven.

Het patroon voor de URL ziet er als volgt uit:

```
https://secure.adobesign.com/public/oauth?
  redirect_uri=&
  response_type=code&
  client_id=&
  scope=
```

De gebruiker wordt gevraagd zich aan te melden bij zijn/haar id voor Adobe Sign. Nadat u zich hebt aangemeld, wordt hen gevraagd of zij machtigingen voor de toepassing moeten verlenen.

![Afbeelding van toegangsscherm bevestigen](assets/AWNjs_12.png)

Als de gebruiker klikt *Toegang toestaan* op de omleidings-URL geeft een queryparameter met de naam code de machtigingscode door:

https://YourServer.com/?code=**\&lt;authorization_code>**\&amp;api_access_point=https://api.adobesign.com&amp;web_access_point=https://secure.adobesign.com

Als u deze code samen met uw client-id en clientgeheim op de Adobe Sign-server plaatst, krijgt u toegang tot de service via een toegangstoken. De waarden opslaan in de parameters `api_access_point` en `web_access_point`. Deze waarden worden gebruikt voor verdere aanvragen.

```
var requestURL = ' ${api_access_point}oauth/token?code=${code}'
  +'&client_id=${client_id}'
  +'&client_secret=${client_secret}&'
  +'redirect_uri=${redirect_url}&'
  +'grant_type=authorization_code';
request.post(requestURL, {form: { }
}, (err,response,body)=>{                
    var token_response = JSON.parse(body)
    var access_token = token_response.access_token;
    console.log(access_token);
});
```

Als een document een handtekening vereist, moet het document eerst worden geüpload. Uw toepassing kan het document uploaden naar `api_access_point` waarde die is ontvangen tijdens het aanvragen van de OAUTH-token. Het eindpunt is `/api/rest/v6/transientDocuments`. De aanvraaggegevens zien er als volgt uit:

```
POST /api/rest/v6/transientDocuments HTTP/1.1
Host: api.na1.adobesign.com
Authorization: Bearer MvyABjNotARealTokenHkYyi
Content-Type: multipart/form-data
Content-Disposition: form-data; name=";File"; filename="MyPDF.pdf"
<PDF CONTENT>
```

Binnen uw toepassing, bouw het verzoek met de volgende code:

```
var uploadRequest = {
  'method': 'POST',
  'url': '${oauthParameters.signin_domain}/api/rest/v6/transientDocuments',
  'headers': {
    'Authorization': 'Bearer  ${auth_token}'
  },
  formData: {
    'File': {
      'value': fs.createReadStream(documentPath),
      'options': {
        'filename': fileName,
        'contentType': null
      }
    }
  }
};
 
request(uploadRequest, (error, response) => {
  if (error) throw new Error(error);
  var jsonResponse = JSON.parse(response.body);
  var transientDocumentId = jsonResponse.transientDocumentId;
  logger.info('transientDocumentId:', transientDocumentId)
});
```

Het verzoek retourneert een `transientID` waarde. Het document is geüpload, maar is nog niet verzonden. Als u het document wilt verzenden, gebruikt u de `transientID` om het document te verzenden.

Maak eerst een JSON-object met de informatie voor het document dat moet worden ondertekend. In het volgende voorbeeld wordt de variabele `transientDocumentId` bevat de id uit de bovenstaande code en `agreementDescription` bevat tekst waarin de overeenkomst wordt beschreven die moet worden ondertekend. De personen die het document moeten ondertekenen, worden vermeld in `participantSetsInfo` via hun e-mailadres en rol.

```
var requestBody = {
  "fileInfos":[
    {"transientDocumentId":transientDocumentId}],
    "name":agreementDescription,
    "participantSetsInfo":[
      {"memberInfos":[{"email":"user@domain.com"}],
       "order":1,"role":"SIGNER"}
    ],
    "signatureType":"ESIGN","state":"IN_PROCESS"
};
```

Als u deze webaanvraag verzendt, wordt de ondertekeningsaanvraag samengesteld en wordt een JSON-object met een id voor de overeenkomstaanvraag geretourneerd:

```
request(requestBody, function (error, response) {
  if (error) throw new Error(error);
  var JSONResponse = JSON.parse(response.body);
  var requestId = JSONResponse.id;
});
```

Als de ondertekenaars vergeten te ondertekenen en een andere e-mail met een melding nodig hebben, verzendt u de meldingen opnieuw met de ID die u eerder hebt ontvangen. Het enige verschil is dat u ook de deelnemer-id&#39;s van de partijen moet toevoegen. U kunt de deelnemer-id&#39;s ophalen door een verzoek tot GET te verzenden aan `/agreements/{agreementID}/members`.

Als u wilt verzoeken de herinnering te verzenden, moet u eerst een JSON-object maken waarin de aanvraag wordt beschreven. Voor het minimale object is een lijst met de deelnemers-id&#39;s en een status voor de herinnering nodig (&quot;ACTIEF&quot;, &quot;COMPLETE&quot; of &quot;GEANNULEERD&quot;).

De aanvraag kan eventueel aanvullende informatie bevatten, zoals een waarde voor &quot;opmerking&quot; die aan de gebruiker wordt weergegeven. Of een vertraging (in uren) om te wachten tot de herinnering is verzonden (in `firstReminderDelay`) en een herinneringsfrequentie (in het veld &quot;frequentie&quot;), waarmee waarden zoals DAILY_UNTIL_SIGNED, EVERY_THIRD_DAY_UNTIL_SIGNED of WEEKLY_UNTIL_SIGNED worden geaccepteerd.

```
var requestBody = {
  //participantList is an array of participant ID strings
  "recipientParticipantIds":participantList
  ,"status":"ACTIVE",
  "note":"This is a reminder to sign out important agreement."
}
 
var reminderRequest = {
  'method': 'POST',
  'url': '${oauthParameters.signin_domain}/api/rest/v6/agreements/${agreementID}/reminders',
  'headers': {
    'Authorization': `Bearer ${access_token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(requestBody)
 
};

request(reminderRequest, function (error, response) {
});
```

En dat is alles wat nodig is om een herinneringsverzoek te verzenden.

![Afbeelding van webformulier](assets/AWNjs_13.png)

## Webformulieren maken

U kunt ook de Adobe Sign API gebruiken om webformulieren te maken. Met webformulieren kunt u een formulier insluiten in een webpagina of er rechtstreeks een koppeling naar maken. Zodra een Vorm van het Web wordt gecreeerd, toont het ook onder de Vormen van het Web in uw console van Adobe Sign. U kunt webformulieren met de status CONCEPT maken voor stapsgewijze opbouw, voor het bewerken van de webformuliervelden en voor de status ACTIEF als u het formulier direct wilt hosten.

![Afbeelding van webformulier in het scherm Adobe Sign Manage](assets/AWNjs_14.png)

Gebruik het formulier om een webformulier te maken `transientDocumentId`. Bepaal de titel van het formulier en de status om het te initialiseren.

```
var requestBody = {
  "fileInfos": [
    {
      "transientDocumentId": transientDocumentId
    }
  ],
  "name": webFormTitle,
  "state": status,
  "widgetParticipantSetInfo": {
    "memberInfos": [ { "email": "" } ],
    "role": "SIGNER"
  }
}
```

```
var createWebFormRequest = {
  'method': 'POST',
  'url': `${oauthParameters.signin_domain}/api/rest/v6/widgets`,
  'headers': {
    'Authorization': `Bearer ${access_token}`,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(requestBody)
}
```

```
request(createWebFormRequest, function (error, response) {
  var jsonResp = JSON.parse(response.body);
  var webFormID = jsonResp.id;
});
```

U kunt nu uw document insluiten of koppelen.

## Volgende stappen

Zoals u kunt zien aan de snelle start en de meegeleverde code, kunt u eenvoudig PDF- en digitale processen voor documentgoedkeuring implementeren met Node en de [!DNL Adobe Acrobat Services] API&#39;s. Adobe-API&#39;s integreren naadloos in uw bestaande toepassingen.

Om het vereiste werkingsgebied voor een vraag te ontdekken, of te zien hoe de vraag wordt gebouwd, kunt u steekproefvraag van bouwen [Rest API-documentatie](https://secure.na4.adobesign.com/public/docs/restapi/v6). De [QuickStart](https://github.com/adobe/pdftools-node-sdk-samples) toon ook andere functionaliteit en bestandsindelingen aan [!DNL Adobe Acrobat Services] API’s verwerken.

U kunt een groot aantal PDF-mogelijkheden aan uw toepassingen toevoegen, waardoor uw gebruikers hun documenten snel en eenvoudig kunnen bekijken en ondertekenen, en nog veel meer. Uitchecken om te beginnen [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/) vandaag.
