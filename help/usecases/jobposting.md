---
title: Taak posten
description: Leer hoe je een soepele en consistente webervaring ontwikkelt voor sollicitanten en werkgevers
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8092
thumbnail: KT-8092.jpg
exl-id: 0e24c8fd-7fda-452c-96f9-1e7ab1e06922
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1447'
ht-degree: 0%

---

# Taak posten

![ Hoofdletterbanner van het Gebruik ](assets/UseCaseJobHero.jpg)

Als u een website met meerdere gebruikers beheert, is het van cruciaal belang dat u een ervaring ontwerpt die iedereen een soepele ervaring biedt.

Stel het volgende scenario voor: u hebt een website die werkgevers toestaat om [ banen post ](https://developer.adobe.com/document-services/use-cases/content-publishing/job-posting) te uploaden. Voor werkzoekenden is het handig om alle documenten met betrekking tot een post eenvoudig in een consistent formaat weer te geven. Het is echter handig voor werkgevers om informatie toe te voegen in elke bestandsindeling die ze hebben. Als u beide soorten gebruikers gebruiksvriendelijkheid wilt bieden, kunt u alle geüploade documenten automatisch converteren naar PDF en deze insluiten in het posten.

## Wat je kunt leren

Dit hands-on leerprogramma loopt door een voorbeeld Node.js dat [!DNL Adobe Acrobat Services] en zijn [ Node.js SDK ](https://www.npmjs.com/package/@adobe/documentservices-pdftools-node-sdk) gebruikt om deze mogelijkheden aan een baan post plaats toe te voegen. Zo ontstaat een website die gemakkelijker te gebruiken is en die zowel voor werkgevers als voor werkzoekenden aantrekkelijker is. Hier is de [ volledige ](https://github.com/contentlab-io/adobe_job_posting) [ projectcode ](https://github.com/contentlab-io/adobe_job_posting), voor het geval u langs wilt volgen aangezien u leest.

Stel om te beginnen een eenvoudige, op Express gebaseerde Node.js-webtoepassing in. [ Uitdrukkelijke ](https://expressjs.com/) is een minimalistisch kader van de Webtoepassing dat eigenschappen zoals het verpletteren en het templating aanbiedt. De code voor de toepassing is beschikbaar op [ GitHub ](https://github.com/contentlab-io/adobe_job_posting). Ook, installeer het [ gegevensbestand PostgreSQL ](https://www.postgresql.org/) en opstelling het om de PDF op te slaan.

## Relevante [!DNL Acrobat Services] API&#39;s

* [ PDF bedt API ](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html) in

* [ de Diensten API van de PDF ](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

## API-referenties voor Adobe maken

Eerst, moet u [ geloofsbrieven ](https://www.adobe.com/go/dcsdks_credentials) voor Adobe PDF creëren Embed API (vrij om te gebruiken) en de Diensten API van Adobe PDF (vrij voor zes maanden toen [ betaal-als-u-gaat ](https://developer.adobe.com/document-services/pricing/main) voor enkel \$0.05 per documenttransactie). Als u referenties maakt voor de PDF Services-API, selecteert u de optie &quot;Voorbeeld van gepersonaliseerde code maken&quot;. Sla het ZIP-bestand op en extraheer pdftools-api-credentials.json en private.key naar de hoofdmap van uw Node.js Express-project.

U hebt ook een API-sleutel nodig voor de vrij beschikbare Embed-API. Van [ Projecten ](https://developer.adobe.com/console/projects), ga naar het project u creeerde. Dan, klik **toevoegen aan Project** en selecteer **API**. Tot slot klik **PDF bed API** in.

Geef het domein op voor de PDF Embed-API. De API-sleutel moet openbaar zijn (deze moet in de code staan die door de browser wordt uitgevoerd). Door een domein op te geven, zorgt u ervoor dat iemand anders in een ander domein de API-sleutel niet kan gebruiken.

U kunt &quot;localhost&quot; niet als domein gebruiken. Geef een domein op, bijvoorbeeld &quot;testing.local&quot;, en bewerk het hostbestand op uw computer om dat domein om te leiden naar 127.0.0.1 , de computer. Vervolgens kunt u de toepassing niet testen op localhost:3000, maar op testing.local:3000. Als u klaar bent, zoekt u de API-sleutel voor de PDF Embed-API op de projectpagina.

## Een uploadformulier en -handler toevoegen

Met een werkende Express-toepassing en API-referenties hebt u ook een formulier nodig waarmee gebruikers hun documenten kunnen uploaden naar de website. Bewerk de sjabloon index.jade voor dit doel.

Maak een invoerveld voor de naam van de geüploade taak en voor een document dat meer informatie bevat.

Voeg het volgende formulier toe in het inhoudsblok van de sjabloon:

```
extends layout

block content
  h1= title

  form(action="/upload", enctype="multipart/form-data", method="POST")
    label Job posting name:&nbsp;
    input(type="text", name="name", required="required")
    br
    br
    label Describing document:&nbsp;
    input(type="file", name="attachment", required="required")
    br
    br
    input(type="submit", value="Submit job posting")
```

Vervolgens voegt u een handler voor het verzoek van de POST toe aan de handeling /upload. Voeg vervolgens een route voor het uploaden naar het bestand routes/index.js toe. U kunt een nieuw bestand maken voor deze route, maar u moet het bestand app.js bijwerken om het nieuwe bestand weer te geven. Binnen deze routehandler hebt u toegang tot de opgegeven naam en het geüploade bestand.

```
router.post('/upload', async function (req, res, next) {
    const name = req.body.name;
    const fileContents = req.files.attachment.data;

    // code to work with the uploaded document
  });
```

De functie is asynchroon, zodat u het wachtende trefwoord in de functie kunt gebruiken. Dit is handig wanneer u de methoden aanroept die API-aanroepen uitvoeren.

![ Screenshot van baan het posten website ](assets/jobs_1.png)

## PDF Services-API gebruiken

Voordat u de PDF Services API gebruikt, moet u de volgende importbewerkingen boven aan het routebestand toevoegen:

```
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk');
  const { Readable } = require('stream');
```

Direct onder de invoer, kunt u API geloofsbrieven laden en een [ uitvoeringsinhoud ](https://www.javascripttutorial.net/javascript-execution-context/) creëren. Aangezien u een uitvoeringscontext kunt hergebruiken voor verschillende bewerkingen, is het verstandig om deze context maar één keer te gebruiken.

```
  const credentials = PDFToolsSdk.Credentials
  .serviceAccountCredentialsBuilder()
  .fromFile("pdftools-api-credentials.json")
  .build();

  const executionContext = PDFToolsSdk.ExecutionContext.create(credentials);
```

Ga nu terug naar het schrijven van code in de aanvraaghandler bij de opmerking in het `router.post` -blok. Converteer het document eerst naar PDF.

```
  const createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();

  const input = PDFToolsSdk.FileRef.createFromStream(Readable.from(fileContents),
  req.files.attachment.mimetype);

  createPdfOperation.setInput(input);

  let result = await createPdfOperation.execute(executionContext);

  result.saveAsFile('output-pdf' + new Date().getTime() + '.pdf');
  return res.send('success!');
```

Bij de meeste bewerkingen worden dezelfde vier stappen uitgevoerd. Initialiseer eerst het type bewerking met de methode createNew van de juiste klasse. Maak vervolgens de invoer voor de bewerking, namelijk FileRef. Opeenvolgende bewerkingen kunnen deze stap overslaan omdat het resultaat van een bewerking ook een FileRef is. Voor deze eerste bewerking maakt u een FileRef op basis van de bytes van het geüploade bestand. Ten derde moet u de invoer aan de bewerking toewijzen. Tot slot wordt de bewerking uitgevoerd, waarbij de uitvoeringscontext een parameter is in de uitvoeringsmethode. Deze methode retourneert een belofte, zodat u het resultaat kunt afwachten.

De code slaat de geretourneerde PDF op in een bestand en stuurt een eenvoudige &quot;geslaagde&quot; reactie naar de browser. Het gedeelte Datum van de bestandsnaam garandeert een unieke bestandsnaam. Het saveAsFile retourneert een fout als het doelbestand bestaat.

## Afbeeldingen omzetten in tekst en de PDF comprimeren

Gebruik nu OCR (optische tekenherkenning) om afbeeldingen om te zetten in tekst en het resultaat vervolgens te comprimeren. Dit doet u met de OCR- en CompressPDF-bewerkingen, vergelijkbaar met de CreatePDF-bewerking. Voeg het volgende toe aan het routebestand, in `router.post` :

```
  const name = req.body.name;
  const fileContents = req.files.attachment.data;

  const createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();
  const input = PDFToolsSdk.FileRef.createFromStream(Readable.from(fileContents),
  req.files.attachment.mimetype);
  createPdfOperation.setInput(input);

  let result = await createPdfOperation.execute(executionContext);

  const ocrOperation = PDFToolsSdk.OCR.Operation.createNew();
  ocrOperation.setInput(result);
  result = await ocrOperation.execute(executionContext);

  const compressPdfOperation = PDFToolsSdk.CompressPDF.Operation.createNew();
  compressPdfOperation.setInput(result);
  result = await compressPdfOperation.execute(executionContext);

  result.saveAsFile('output-pdf' + new Date().getTime() + '.pdf');
  return res.send('success!');
```

Het is alleen nodig deze bewerking één keer uit te voeren omdat het resultaat een FileRef is, die de code kan doorgeven aan setInput.

Er is een beter alternatief dan het bestand op een vaste schijf opslaan en een te vereenvoudigde HTTP-respons retourneren. Sla in plaats daarvan de PDF op in een database en geef een webpagina weer die de PDF insluit met de gratis PDF Embed-API van de Adobe. Op deze manier is de vacature of brochure van de werkgever zichtbaar op de website, zodat werkzoekenden ze kunnen vinden en bekijken, compleet met bedrijfslogo&#39;s en andere ontwerpelementen.

## De PDF opslaan in een database

Sla de PDF op in een PostSQL-database. Krijg het knoop-postgres pakket om met Postgres in Node.js te verbinden. Installeer het streambufferpakket omdat u op een bepaald moment de inhoud van de PDF in een buffer moet opslaan en FileRef werkt alleen met streams. Gebruik dus het streambufferpakket om de inhoud naar een buffer te schrijven.

```
npm install pg stream-buffers
```

Maak nu een databasetabel voor taakposten. Er is een kolom nodig voor een unieke id, een kolom voor een naam en een kolom voor de bijgevoegde PDF. U kunt een gegevensbestandlijst van de bevel-lijn van Postgres interface (CLI) creëren:

```
CREATE TABLE job_postings (id TEXT PRIMARY KEY, name TEXT NOT NULL, attachment
BYTEA NOT NULL);
```

Ga terug naar de Node.js-bestanden. Voeg wat importen toe boven aan het bestand:

```
  const { Client } = require('pg');
  const streamBuffers = require('stream-buffers');
```

Als u de PDF in de databasetabel wilt opslaan, wijzigt u de uploadfunctie. Vervang de laatste twee regels (saveAsFile en send) door dit codefragment:

```
  const pgClient = new Client();
  pgClient.connect();

  const id = Math.random().toString(36).substr(2, 6); // not securely random at all,
  but serves the purpose for this demo

  const writableStream = new streamBuffers.WritableStreamBuffer();
  writableStream.on("finish", async () => {    
    await pgClient.query("INSERT INTO job_postings VALUES ($1, $2, $3)", [
      id,
      name,
      writableStream.getContents()
    ]);
    res.redirect(`/job/${id}`);
  })
  result.writeToStream(writableStream);
```

Maak een WritableStreamBuffer om de inhoud te schrijven. Met de afsluitgebeurtenis is het tijd om de SQL-query uit te voeren. Het knooppunt-postgres-pakket zet de bufferparameter automatisch om in de BYTEA-indeling. De vraag richt de gebruiker aan /job/{id} opnieuw, een eindpunt dat later wordt gecreeerd.

Voor PDF Embed API, hebt u ook een eindpunt nodig dat enkel de inhoud van de PDF terugkeert:

```
  router.get('/pdf/:id', async function (req, res, next) {
    const id = req.params.id;
 
    const pgClient = new Client();
    pgClient.connect();

  const pgResult = await pgClient.query("SELECT attachment FROM job_postings WHERE id
  = $1", [id]);
  const buffer = pgResult.rows[0].attachment;
  res.type('pdf');
    return res.send(buffer);
  });
```

## De PDF insluiten

Nu, creeer het /job/ {id} eindpunt, dat een malplaatje teruggeeft die de naam van het gevraagde baan het posten en een ingebedde PDF bevatten.

```
router.get('/job/:id', async function(req, res, next) {
    const id = req.params.id;

    const pgClient = new Client();
    pgClient.connect();

    const pgResult = await pgClient.query("SELECT name FROM job_postings WHERE id =
  $1", [id]);
    const name = pgResult.rows[0].name;

    res.render('job', { pdf_url: `/pdf/${id}`, name });
  });
```

Maak in de map views/ een bestand job.jade met deze inhoud:

```
  extends layout

  block content
    h1= name
    div(id='adobe-dc-view')
    script(src='https://documentcloud.adobe.com/view-sdk/main.js')
    script.
      window.embedUrl = "!{pdf_url}";
    script(src='/javascripts/embed-pdf.js')
```

Het eerste script is de View SDK van de Adobe, waarmee u de PDF gemakkelijk kunt insluiten. Het tweede script is een in-line één-liner die de waarde van window.embedUrl aan URL van de PDF plaatst die door de Uitdrukkelijke routemanager wordt verstrekt. Maak het derde script zelf als volgt:

```
  document.addEventListener("adobe_dc_view_sdk.ready", function () {
    var adobeDCView = new AdobeDC.View({ clientId: "YOUR API KEY HERE", divId:
   "adobe-dc-view" });
    adobeDCView.previewFile({
      content: { location: { url: '//' + window.location.host + window.embedUrl }
         },
      metaData: { fileName: "Job posting" }
    });
  });
```

U kunt nu het hele proces testen voor het uploaden van een document, dat wordt omgeleid naar de pagina /job/id en dat de ingesloten PDF weergeeft. Uw gebruikers gaan dezelfde stappen uit om een taak of ander document aan uw website toe te voegen.

![ Schermafbeelding van het testen van een geüpload document van de PDF ](assets/jobs_2.png)

Om een in-lijn in actie te zien inbedden, controleer deze [ levende demo ](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/IN_LINE/Bodea%20Brochure.pdf).

## Volgende stappen

Dit hands-on leerprogramma liep door hoe te om Node.js met [!DNL Acrobat Services] te gebruiken om een geüploade [ baan het posten ](https://developer.adobe.com/document-services/use-cases/content-publishing/job-posting) in diverse formaten in een PDF om te zetten. De resulterende PDF is vervolgens ingesloten in een webpagina. Nu kun je dezelfde functie toevoegen aan je website, zodat werkgevers gemakkelijker taakbeschrijvingen, brochures en meer kunnen uploaden om werkzoekenden te vinden. Deze mogelijkheden helpen iedereen de informatie te krijgen die nodig is om hun droombaan te vinden.

Met [!DNL Acrobat Services] kunt u belangrijke documentverwerkingsfuncties toevoegen aan uw website of app. Raadpleeg de volgende documentatie om snel te starten als u dieper wilt ingaan op wat deze API&#39;s kunnen doen:

* [ PDF bedt API ](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html) in

* [ de Diensten API van de PDF ](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

Beginnen gebruikersvriendelijke document-behandelende eigenschappen aan uw website toe te voegen, [ teken omhoog voor uw vrije proef ](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html). Adobe PDF Embed API is altijd vrij te gebruiken en de Diensten API van Adobe PDF is gratis voor zes maanden, dan is het enkel \$0.05 per documenttransactie zodat kunt u [ betaal-zoals-u-gaat ](https://developer.adobe.com/document-services/pricing/main) aangezien uw zaken groeit.
