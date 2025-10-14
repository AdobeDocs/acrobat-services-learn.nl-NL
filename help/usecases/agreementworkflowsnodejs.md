---
title: Overeenkomstworkflows in Node.js
description: '[!DNL Adobe Acrobat Services] API''s integreren eenvoudig PDF-mogelijkheden in uw webtoepassingen'
feature: Use Cases
role: Developer
level: Beginner
type: Tutorial
jira: KT-7473
thumbnail: KT-7473.jpg
keywords: Aanbevolen
exl-id: 44a03420-e963-472b-aeb8-290422c8d767
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '2094'
ht-degree: 0%

---

# Overeenkomstworkflows in Node.js

![&#x200B; Hoofdletterbanner van het Gebruik &#x200B;](assets/UseCaseAgreementHero.jpg)

Veel bedrijfstoepassingen en -processen vereisen documentatie zoals voorstellen en overeenkomsten. PDF-documenten zorgen ervoor dat bestanden veiliger en minder wijzigbaar zijn. Ze bieden ook ondersteuning voor digitale handtekeningen, zodat je klanten hun documenten snel en eenvoudig kunnen voltooien. API&#39;s van [!DNL Adobe Acrobat Services] integreren eenvoudig PDF-mogelijkheden in uw webtoepassingen.

## Wat je kunt leren

In deze praktische zelfstudie leert u hoe u PDF-services kunt toevoegen aan een Node.js-toepassing om een overeenkomstproces te digitaliseren.

## Relevante API&#39;s en bronnen

* [&#x200B; de Diensten API van de PDF &#x200B;](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [&#x200B; PDF bedt API &#x200B;](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html) in

* [&#x200B; Adobe Sign API &#x200B;](https://developer.adobe.com/adobesign-api/)

* [&#x200B; code van het Project &#x200B;](https://github.com/adobe/pdftools-node-sdk-samples)

## Instellen [!DNL Adobe Acrobat Services]

Stel gebruikersgegevens in voor gebruik van [!DNL Adobe Acrobat Services] om aan de slag te gaan. Registreer een rekening en gebruik [&#x200B; Node.js Quickstart &#x200B;](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#node-js) om uw geloofsbrieven te verifiëren werk alvorens de functionaliteit in een grotere toepassing te integreren.

Kies eerst een Adobe-ontwikkelaarsaccount. Dan, op [&#x200B; krijg Begonnen &#x200B;](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html?ref=getStartedWithServicesSDK) pagina, selecteer *krijgen Begonnen* optie onder Create Nieuwe Geloofsbrieven. U kunt zich aanmelden voor een gratis proefversie met 1000 Document Transactions die u in zes maanden kunt gebruiken.

![&#x200B; Beeld van het Creëren van Nieuwe Geloofsbrieven &#x200B;](assets/AWNjs_1.png)

Op de volgende pagina Nieuwe referenties maken wordt u gevraagd te beslissen tussen de PDF Embed-API en de PDF Services-API.

Selecteer *de Diensten API van de PDF*.

Ga een naam voor de toepassing in en controleer de doos geëtiketteerd *creeer gepersonaliseerde codesteekproef*. Als u dit selectievakje inschakelt, worden uw referenties automatisch ingesloten in het codevoorbeeld. Als u dit vak niet inschakelt, moet u uw referenties handmatig aan de toepassing toevoegen.

Selecteer *Node.js* voor het toepassingstype en klik *creeer Referenties*.

Een paar minuten later begint een .zip-bestand te downloaden met een voorbeeldproject met uw referenties. Het pakket Node.js voor [!DNL Acrobat Services] is reeds inbegrepen als deel van de code van het steekproefproject.

![&#x200B; Beeld van het Selecteren van de Server API Geloofsbrieven &#x200B;](assets/AWNjs_2.png)

## Handmatig het voorbeeldproject configureren

Als u ervoor kiest om geen voorbeeldproject te downloaden van de pagina Nieuwe referenties maken, kunt u het project ook handmatig instellen.

Download de code (zonder uw ingebedde geloofsbrieven) van [&#x200B; GitHub &#x200B;](https://github.com/adobe/pdftools-node-sdk-samples). Als u deze versie van de code gebruikt, moet u uw referenties toevoegen aan het bestand pdftools-api-credentials.json voordat u het kunt gebruiken:

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

U moet het pakket Node.js voor [!DNL Acrobat Services] installeren. Gebruik de volgende opdracht om het pakket te installeren:

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

Zet het bestand vervolgens om van de oorspronkelijke indeling in PDF. De voorbeeldcode die u eerder hebt gedownload, bevat een script met de naam `create-pdf-from-docx.js` voor het converteren van een document naar PDF. Met de volgende functie, `convertDocumentToPDF` , wordt een geüpload document gemaakt en omgezet in een PDF in een andere map:

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

![&#x200B; Beeld van de Termen van de Computer &#x200B;](assets/AWNjs_3.png)

De bron voor dit document bestaat uit een CSS-bestand en een HTML-bestand:

![&#x200B; Beeld van CSS en het dossier van de HTML &#x200B;](assets/AWNjs_4.png)

Na het verwerken van het HTML-bestand hebt u dezelfde tekst in de PDF-indeling:

![&#x200B; PDF dossier van de Termen van de Computer &#x200B;](assets/AWNjs_5.png)

## Pagina&#39;s toevoegen

Een andere veelvoorkomende bewerking met PDF-bestanden is het toevoegen van pagina&#39;s aan het einde met standaardtekst, zoals een lijst met termen. Met de documentgereedschapset kunt u verschillende PDF-documenten combineren tot één document. Als u een lijst met documentpaden hebt (hier in `sourceFileList` ), kunt u voor een combinatiebewerking de bestandsverwijzingen van elk bestand naar een object toevoegen.

Wanneer de combinatiebewerking wordt uitgevoerd, wordt één bestand met de broninhoud geleverd. U kunt `saveAsFile` op het object gebruiken om het bestand voor opslag voort te zetten.

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

U hebt verschillende bewerkingen uitgevoerd op PDF-bestanden, maar uiteindelijk moet de gebruiker de documenten bekijken. U kunt het document insluiten in een webpagina met de PDF Embed-API van de Adobe.

Voeg op de pagina die de PDF weergeeft een `<div />` -element toe om het document vast te houden en geef het een id. U gebruikt deze id binnenkort. Neem op de webpagina een `<script />` -verwijzing op naar de JavaScript-Adobe-bibliotheek:

```
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

Het laatste stukje code dat u nodig hebt, is een functie waarmee het document wordt weergegeven nadat de Adobe PDF Embed API JavaScript is geladen. Als u een melding ontvangt dat het script is geladen via een gebeurtenis adobe_dc_view\_sdk.ready, maakt u een nieuw object AdobeDC.View. Voor dit object zijn uw Client-id en de id van het element nodig die u eerder hebt gemaakt. Vind uw cliëntidentiteitskaart in de [&#x200B; Console van de Ontwikkelaar van Adobe &#x200B;](https://developer.adobe.com/console/). Wanneer u de instellingen voor de toepassing bekijkt die u eerder hebt gemaakt toen u referenties genereerde, wordt de client-id daar weergegeven.

![&#x200B; Beeld van API Sleutel van de Cliënt &#x200B;](assets/AWNjs_6.png)

## Andere PDF-opties

De [&#x200B; Adobe PDF bedt API demo &#x200B;](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf) laat u toe om de diverse andere opties voor het inbedden van de documenten van de PDF te tonen.

![&#x200B; Afbeelding van opties voor het insluiten van PDF &#x200B;](assets/AWNjs_7.png)

U kunt verschillende opties in- en uitschakelen en direct zien hoe ze worden gerenderd. Als u een gewenste combinatie vindt, klikt u op de knop *\&lt;/\> Code genereren* om de werkelijke HTML-code te genereren met behulp van deze opties.

![&#x200B; Beeld van de Voorproef van de Code &#x200B;](assets/AWNjs_8.png)

## Digitale handtekeningen en beveiliging toevoegen

Zodra een document klaar is, kunt u digitale handtekeningen toevoegen voor goedkeuring met behulp van Adobe Sign. Deze functionaliteit werkt iets anders dan de functionaliteit die u tot nu toe hebt gebruikt. Voor digitale handtekeningen moet een toepassing zijn geconfigureerd om OAuth te gebruiken voor gebruikersverificatie.

De eerste stap in vestiging moet uw toepassing [&#x200B; uw toepassing &#x200B;](https://opensource.adobe.com/acrobat-sign/developer_guide/index.html#!adobedocs/adobe-sign/master/gstarted/create_app.md) registreren om OAuth voor Adobe Sign te gebruiken. Zodra ondertekend, navigeer aan het scherm voor het creëren van toepassingen door op *Account* te klikken, dan open de *Adobe Sign API* sectie, en klik *API Toepassingen* om de lijst van geregistreerde toepassingen te openen.

![&#x200B; Beeld van eerste stap in het registreren van uw toepassing &#x200B;](assets/AWNjs_9.png)

Als u een nieuwe toepassingsvermelding wilt maken, klikt u op het plusteken in de rechterbovenhoek.

![&#x200B; Beeld van het plusteken in hoger-juiste hoek van het scherm &#x200B;](assets/AWNjs_10.png)

Voer in het venster dat wordt geopend een toepassingsnaam en weergavenaam in. Selecteer *Klant* voor het domein, dan klik *sparen*.

![&#x200B; Beeld van waar te om toepassingsnaam en vertoningsnaam in te gaan &#x200B;](assets/AWNjs_11.png)

Nadat de toepassing wordt gecreeerd, kunt u het van de lijst selecteren en op *klikken vormt OAuth voor Toepassing*. Selecteer de opties. Voer in de URL omleiden de URL voor uw toepassing in. U kunt hier meerdere URL&#39;s invoeren. Voor de toepassing die u test, is de waarde:

```
http://localhost:3000/signed-in 
```

Het proces voor het verkrijgen van een token met OAuth is standaard. Uw toepassing leidt een gebruiker naar een URL voor het aanmelden. Nadat de gebruiker zich heeft aangemeld,
zij worden opnieuw gericht naar de toepassing met extra informatie in de vraagparameters van de pagina.

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

![&#x200B; Beeld van bevestig toegangsscherm &#x200B;](assets/AWNjs_12.png)

Als de gebruiker *klikt sta Toegang* op redirect URL toe, gaat een vraagparameter genoemd code de vergunningscode over:

https://YourServer.com/?code=**\&lt;authentication_code\>**\&amp;api_access_point=https://api.adobesign.com&amp;web_access_point=https://secure.adobesign.com

Als u deze code samen met uw client-id en clientgeheim op de Adobe Sign-server plaatst, krijgt u toegang tot de service via een toegangstoken. Sla de waarden op in de parameters `api_access_point` en `web_access_point` . Deze waarden worden gebruikt voor verdere aanvragen.

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

Als een document een handtekening vereist, moet het document eerst worden geüpload. Uw toepassing kan het document uploaden naar de `api_access_point` -waarde die is ontvangen tijdens het aanvragen van de OAUTH-token. Het eindpunt is `/api/rest/v6/transientDocuments` . De aanvraaggegevens zien er als volgt uit:

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

De aanvraag retourneert een `transientID` -waarde. Het document is geüpload, maar is nog niet verzonden. Als u het document wilt verzenden, gebruikt u `transientID` om het document te verzenden.

Maak eerst een JSON-object met de informatie voor het document dat moet worden ondertekend. In het volgende voorbeeld bevat de variabele `transientDocumentId` de id uit de bovenstaande code en bevat `agreementDescription` tekst die de overeenkomst beschrijft die moet worden ondertekend. De personen die het document moeten ondertekenen, worden vermeld in `participantSetsInfo` op basis van hun e-mailadres en rol.

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

Als de ondertekenaars vergeten te ondertekenen en een andere e-mail met een melding nodig hebben, verzendt u de meldingen opnieuw met de ID die u eerder hebt ontvangen. Het enige verschil is dat u ook de deelnemer-id&#39;s van de partijen moet toevoegen. U kunt de deelnemer-id&#39;s ophalen door een verzoek tot GET naar `/agreements/{agreementID}/members` te verzenden.

Als u wilt verzoeken de herinnering te verzenden, moet u eerst een JSON-object maken waarin de aanvraag wordt beschreven. Voor het minimale object is een lijst met de deelnemers-id&#39;s en een status voor de herinnering nodig (&quot;ACTIEF&quot;, &quot;COMPLETE&quot; of &quot;GEANNULEERD&quot;).

De aanvraag kan eventueel aanvullende informatie bevatten, zoals een waarde voor &quot;opmerking&quot; die aan de gebruiker wordt weergegeven. Of een vertraging (in uren) die moet wachten tot de herinnering wordt verzonden (in `firstReminderDelay` ) en een herinneringsfrequentie (in het veld &quot;frequentie&quot;) die waarden accepteert zoals DAILY_UNTIL_SIGNED, EVERY_THIRD_DAY_UNTIL_SIGNED of WEEKLY_UNTIL_SIGNED.

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

![&#x200B; Beeld van de Vorm van het Web &#x200B;](assets/AWNjs_13.png)

## Webformulieren maken

U kunt ook de Adobe Sign API gebruiken om webformulieren te maken. Met webformulieren kunt u een formulier insluiten in een webpagina of er rechtstreeks een koppeling naar maken. Zodra een Vorm van het Web wordt gecreeerd, toont het ook onder de Vormen van het Web in uw console van Adobe Sign. U kunt webformulieren met de status CONCEPT maken voor stapsgewijze opbouw, voor het bewerken van de webformuliervelden en voor de status ACTIEF als u het formulier direct wilt hosten.

![&#x200B; Beeld van de Vorm van het Web in Adobe Sign leidt scherm &#x200B;](assets/AWNjs_14.png)

Als u een webformulier wilt maken, gebruikt u het formulier `transientDocumentId` . Bepaal de titel van het formulier en de status om het te initialiseren.

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

Zoals u kunt zien aan de snelle start en de meegeleverde code, kunt u eenvoudig PDF- en digitale processen voor documentgoedkeuring implementeren met Node met de API&#39;s van [!DNL Adobe Acrobat Services] . De API&#39;s van Adobe integreren naadloos in uw bestaande clienttoepassingen.

Om het vereiste werkingsgebied voor een vraag te ontdekken, of te zien hoe de vraag wordt gebouwd, kunt u steekproefvraag van de [&#x200B; Rest API documentatie &#x200B;](https://secure.na4.adobesign.com/public/docs/restapi/v6) bouwen. [&#x200B; QuickStart &#x200B;](https://github.com/adobe/pdftools-node-sdk-samples) toon ook andere functionaliteit en dossierformaten de [!DNL Adobe Acrobat Services] APIs processen aan.

U kunt een groot aantal PDF-mogelijkheden aan uw toepassingen toevoegen, waardoor uw gebruikers hun documenten snel en eenvoudig kunnen bekijken en ondertekenen, en nog veel meer. Om te beginnen, controleer [[!DNL Adobe Acrobat Services] &#x200B;](https://developer.adobe.com/document-services/homepage/) vandaag.
