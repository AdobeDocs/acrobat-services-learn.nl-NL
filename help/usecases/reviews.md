---
title: Revisies en goedkeuringen
description: Leer hoe u een workflow voor documentrevisie en goedkeuring bouwt voor samenwerking tussen verschillende teams
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8094
thumbnail: KT-8094.jpg
exl-id: d704620f-d06a-4714-9d09-3624ac0fcd3a
source-git-commit: b7a20f30a2eb175053c7a25be0411f80dd88899f
workflow-type: tm+mt
source-wordcount: '1540'
ht-degree: 0%

---

# Revisies en goedkeuringen

![&#x200B; Hoofdletterbanner van het Gebruik &#x200B;](assets/UseCaseReviewsHero.jpg)

De verre cross-teamsamenwerking werd noodzakelijk voor vele bedrijven tijdens de pandemie COVID-19, [&#x200B; delend en het herzien digitale documenten &#x200B;](https://developer.adobe.com/document-services/use-cases/collaboration/review-and-approval) stelt een reeks uitdagingen voor teams en dwars-functionele middelen voor.

Deze uitdagingen omvatten het delen van documenten in verschillende bestandsindelingen, het effectief bekijken van en opmerkingen maken over de inhoud en het synchroniseren met de meest recente bewerkingen. [!DNL Adobe Acrobat Services] API&#39;s zijn ontworpen om toepassingsontwikkelaars in staat te stellen deze uitdagingen voor hun gebruikers op te lossen.

## Wat je kunt leren

Deze praktische zelfstudie laat zien hoe u een documentrevisie- en goedkeuringswerkstroom bouwt in een Node.js- en Express-webtoepassing. Als je mee wilt doen met deze tutorial, heb je enige ervaring nodig met Node.js.

De toepassing heeft de volgende functies:

* Verschillende bestandstypen converteren naar PDF

* Bestanden uploaden inschakelen

* Geef gebruikers de mogelijkheid om opmerkingen en annotaties toe te voegen

* De PDF samen met deze opmerkingen weergeven

* Gebruikersprofielen toestaan om auteurs van opmerkingen te identificeren

* Bestanden combineren tot een definitieve PDF die de gebruikers kunnen downloaden

## Relevante API&#39;s en bronnen

* [&#x200B; de Diensten API van de PDF &#x200B;](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [&#x200B; PDF bedt API &#x200B;](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html) in

* [&#x200B; Code van het Project &#x200B;](https://github.com/contentlab-io/adobe_reviews_and_approvals)

## API-referenties voor Adobe maken

Alvorens de code te beginnen, moet u [&#x200B; geloofsbrieven &#x200B;](https://www.adobe.com/go/dcsdks_credentials) voor Adobe PDF creëren Embed API en de Diensten API van Adobe PDF. PDF Insluiter-API is gratis. De Diensten API van PDF is vrij om voor zes maanden te gebruiken, dan kunt u aan a [&#x200B; betaal-aangezien-u-go plan &#x200B;](https://developer.adobe.com/document-services/pricing/main) bij enkel \$0.05 per documenttransactie schakelen.

Wanneer het creëren van geloofsbrieven voor de Diensten API van de PDF, selecteer **creeer gepersonaliseerde codesteekproef** optie en selecteer Node.js voor de taal. Sla het ZIP-bestand op en extraheer pdftools-api-credentials.json en private.key naar de hoofdmap van uw Node.js Express-project.

## Een project en afhankelijkheden instellen

Stel uw Node.js- en Express-project in om statische bestanden te leveren vanuit een map met de naam &quot;public&quot;. U kunt projectmanieren instellen, afhankelijk van uw voorkeuren. Om snel omhoog en lopend te worden, kunt u de [&#x200B; Uitdrukkelijke app generator &#x200B;](https://expressjs.com/en/starter/generator.html) gebruiken. Of als u dingen eenvoudig wilt houden, kunt u [&#x200B; begin van kras &#x200B;](https://expressjs.com/en/starter/hello-world.html) en uw code in één enkel dossier houden JavaScript. In het voorbeeldproject dat hierboven is gekoppeld, gebruikt u de één-bestandsbenadering en bewaart u al uw code in index.js.

Kopieer de `pdftools-api-credentials.json` - en `private.key` -bestanden van het gepersonaliseerde codevoorbeeld naar de hoofdmap van het project. U kunt ze ook toevoegen aan het .gitignore-bestand, als u er een hebt, zodat uw aanmeldingsbestanden niet per ongeluk naar een opslagplaats worden verzonden.

Voer vervolgens `npm install @adobe/documentservices-pdftools-node-sdk` uit om de Node.js SDK voor PDF Services te installeren. Importeer deze module en maak het API-aanmeldingsobject in uw code (index.js in uw voorbeeldproject), nadat de rest van uw afhankelijkheid als volgt is geïmporteerd:

```
  const PDFToolsSdk = require( "@adobe/documentservices-pdftools-node-sdk" );

  // Create Credentials
  const credentials =  PDFToolsSdk.Credentials
      .serviceAccountCredentialsBuilder()
      .fromFile( "pdftools-api-credentials.json" )
      .build();
```

Uw begincode moet er als volgt uitzien:

```
  
  const express = require( "express" );
  const PDFToolsSdk = require( "@adobe/documentservices-pdftools-node-sdk" );

  // Create Credentials
  const credentials =  PDFToolsSdk.Credentials
      .serviceAccountCredentialsBuilder()
      .fromFile( "pdftools-api-credentials.json" )
      .build();

  const app = express();

  app.use( express.static( "public" ) );

  app.listen( 8889, function() {
      console.log( "Server started on port", 8889 );
  } );
```

U kunt nu werken met [!DNL Acrobat Services] -API&#39;s.

## Een bestand converteren naar PDF

Voor het eerste deel van de documentworkflow moet de eindgebruiker documenten uploaden om te delen. Om dit mogelijk te maken, voegt u een uploadfunctie toe en consolideert u de verschillende documentbestandsindelingen in PDF om ze voor te bereiden op het revisieproces.

Begin door een functie te creëren om documenten in PDF om te zetten die op het [&#x200B; voorbeeldfragment voor de Diensten API van PDF &#x200B;](https://developer.adobe.com/document-services/apis/pdf-services) wordt gebaseerd. In dit voorbeeld worden ook fragmenten weergegeven voor vele andere essentiële functies, zoals OCR (optische tekenherkenning), wachtwoordbeveiliging en -verwijdering, en compressie.

```
function fileToPDF( filename, outputFilename, callback ) {
      // Create an ExecutionContext using credentials and create a new operation
  instance.
      const executionContext = PDFToolsSdk.ExecutionContext.create( credentials ),
          createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();

      // Set operation input from a source file.
      const input = PDFToolsSdk.FileRef.createFromLocalFile( filename );
      createPdfOperation.setInput( input );

      // Execute the operation and Save the result to the specified location.
      createPdfOperation.execute( executionContext )
          .then( result => {
              result.saveAsFile( outputFilename );
              callback( outputFilename );
          } );
  }
```

U kunt deze functie nu gebruiken om PDF te maken van geüploade documenten.

## Bestanden uploaden afhandelen

Vervolgens heeft de server een bestandsupload-eindpunt op de webserver nodig om de documenten te ontvangen en te verwerken.

Maak eerst een map in een uploadmap en geef deze de naam &quot;concepten&quot;. U slaat de geüploade bestanden en de omgezette PDF-bestanden hier op. Voer vervolgens `npm install express-fileupload` uit om de module Express-FileUpload te installeren en voeg de middleware toe aan Express in uw code:

```
const fileUpload = require( "express-fileupload" );
app.use( fileUpload() );
```

Voeg nu een `/upload` -eindpunt toe en sla het geüploade bestand met dezelfde bestandsnaam op in de map Concepten. Vervolgens roept u de functie aan die u eerder hebt geschreven om een PDF-bestand van hetzelfde document te maken als dit nog niet de PDF-indeling heeft. U kunt een bestandsnaam voor het nieuwe PDF-bestand genereren op basis van de naam van het oorspronkelijke geüploade document:

```
// Create a PDF file from an uploaded file
app.post( "/upload", ( req, res ) => {
    if( !req.files || Object.keys( req.files ).length === 0 ) {
        return res.status( 400 ).send( "No files were uploaded." );
    }
    
    // Create PDF from the uploaded file
    let file = req.files.myFile;
    file.mv( __dirname + "/uploads/drafts/" + file.name, ( err ) => {
        if( err ) {
            return res.status( 500 ).send( err );
        }
        if( file.name.endsWith( ".pdf" ) ) {
            res.redirect( "/" );
        }
        else {
            // Convert to PDF
            fileToPDF( __dirname + "/uploads/drafts/" + file.name, __dirname + "/uploads/drafts/" + file.name.replace( /\./g, "-" ) + ".pdf", ( file ) => {
                res.redirect( "/" );
            } );
        }
    });
} );
```

## Een uploadpagina maken

Als u nu bestanden wilt uploaden vanuit de webtoepassing, maakt u een `index.html` -webpagina in de map Uploads. Voeg op de pagina een formulier voor het uploaden van bestanden toe dat het bestand naar het eindpunt /upload verzendt:

```
<form ref="uploadForm" 
      action="/upload"
      method="post" 
      encType="multipart/form-data">
      <input type="file" name="myFile" accept=".doc,.docx,.ppt,.pptx,.xls,.xlsx,.txt,.rtf,.bmp,.jpg,.gif,.tiff,.png">
      <input type="submit" value="Upload File" />
  </form>
```

![&#x200B; Schermafbeelding van uploadmogelijkheden op Web-pagina &#x200B;](assets/reviews_1.png)

U kunt nu documenten uploaden naar de Node.js-server. De server slaat het bestand op in de map Uploads/Concepts en maakt er een versie van de PDF-indeling naast.

U kunt nu de geüploade documenten insluiten. Gebruik daarom de API voor insluiten van PDF om gebruikers in staat te stellen eenvoudig opmerkingen en annotaties aan de documenten toe te voegen.

## Opsomming van PDF-bestanden

Omdat een workflow voor documenten meerdere documenten kan bevatten, moet u een lijst met documenten weergeven en elke document koppelen aan een nieuwe pagina voor documentrevisie in uw app.

Eerst, binnen de servercode, voeg een /files eindpunt toe dat een lijst van alle PDF dossiers krijgt en terugkeert die in de uploads/concepten omslag worden opgeslagen:

```
const fs = require( "fs" );

app.get( "/files", ( req, res ) =\> {

fs.readdir( \_\_dirname + "/uploads/drafts/", ( err, files ) =\> {

if( err ) {

return res.status( 500 ).send( err );using

}

return res.json( files.filter( f =\> f.endsWith( ".pdf" ) ) );

} );

} );
```

Voeg een `/download/:file` -route toe die toegang biedt tot het geüploade PDF-bestand voor het insluiten in de webpagina.

>[!NOTE]
>
>In een productietoepassing, moet u authentificatie en vergunning toevoegen om ervoor te zorgen dat het verzoek van een geldige gebruiker komt en dat de gebruiker tot het document wordt toegelaten.

```
app.get( "/download/:file", function( req, res ){
    // Note: In production code, this should check authentication and user access permissions
    res.download( __dirname + "/uploads/drafts/" + req.params[ "file" ] );
});
```

Werk de pagina index.html bij met een bestandenlijstelement dat tijdens het laden wordt gevuld. Elk item kan worden gekoppeld aan een webpagina concept.html en u geeft de bestandsnaam door aan de pagina met behulp van querytekenreeksparameters.

>[!NOTE]
>
>U gebruikt jQuery om elk item toe te voegen, dus u moet de jQuery-bibliotheek op uw webpagina laden of het element met een andere methode toevoegen.

```
  <ul id="filelist">
      <li>Loading documents...</li>
  </ul>

  ...

  <script>
      // Load current files
      fetch( "/files" )
      .then( r => r.json() )
      .then( files => {
          if( files && files.length > 0 ) {
              $( "#filelist" ).empty();
              files.forEach( file => {
                  $( "#filelist" ).append( `<li><a
  href="/draft.html?file=${file}">${file}</a></li>` );
              })
          } else {
                  $("#filelist").append("<div>No documents found.</div>");
                }
      });
  </script>
```

![&#x200B; Screenshot van het selecteren van een dossier voor overzicht &#x200B;](assets/reviews_2.png)

## Een PDF insluiten

U kunt PDF-bestanden insluiten en weergeven in uw webtoepassing.

Maak een webpagina met de naam &quot;concept.html&quot; en voeg een div-element op de pagina toe voor de ingesloten PDF:

```
  <div id="adobe-dc-view"></div>
```

Neem de [!DNL Acrobat Services] -bibliotheek op:

```
  <script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
```

In een aangepaste scripttag parseert u de bestandsnaam van de parameters van de queryreeks, zodat u weet welk bestand u op de pagina wilt insluiten:

```
  <script type="text/javascript">
          let params = new URLSearchParams( window.location.search );
          let filename = params.get( "file" );
  </script>
```

Voeg een documentgebeurtenislistener toe voor de gebeurtenis adobe_dc_view_sdk.ready die het opgegeven PDF-bestand in een ingesloten weergave in het div-element laadt. Gebruik uw client-id uit de PDF Embed API-referenties. U wilt opmerkingen en annotaties inschakelen, dus sluit u de weergave in de modus FULL_WINDOW in en stelt u de optie showAnnotationsTools in op true.

```
  document.addEventListener( "adobe_dc_view_sdk.ready", () => { 
      var adobeDCView = new AdobeDC.View( { 
          clientId: "YOUR CLIENT ID HERE",
          divId: "adobe-dc-view",
          locale: "en-US",
      } );
      adobeDCView.previewFile( {
          content: { location: { url: "download/" + filename } },
          metaData: { fileName: "Draft Version.pdf" }
      }, {
          embedMode: "FULL_WINDOW",
          showAnnotationTools: true,
          showPageControls: true
      } );
  });
```

## Een gebruikersprofiel maken

Standaard worden opmerkingen en annotaties in deze weergave weergegeven als &quot;Gast&quot;. U kunt de naam van de huidige revisor voor de opmerkingen en annotaties instellen door een callback van het gebruikersprofiel in de paginacode te registreren voor de weergave PDF. Hier volgt een voorbeeldprofiel. In een volwaardige toepassing die gebruikersverificatie bevat, kunnen de profielgegevens van de aangemelde gebruikerssessie op deze manier worden ingesteld om elk commentaar van het document in de revisiewerkstroom te identificeren.

```
  adobeDCView.registerCallback(
      AdobeDC.View.Enum.CallbackType.GET_USER_PROFILE_API,
      () => {
          return new Promise( ( resolve, reject ) => {
              resolve({
                  code: AdobeDC.View.Enum.ApiResponseCode.SUCCESS,
                  data: {
                      userProfile: {
                          name: "YOUR NAME",
                          firstName: "FIRST",
                          lastName: "LAST",
                          email: "document.editor@adobe.com"
                      }
                  }
              });
          });
      }
  );
```

Uw profiel identificeert u als een specifieke gebruiker wanneer u geüploade documenten met deze webpagina ziet en er notities aan toevoegt.

## Documentfeedback opslaan

Na een gebruikerscommentaren op een document, klikken zij **sparen.** door gebrek, downloadt het klikken **sparen** het bijgewerkte dossier van de PDF. Wijzig deze handeling om het huidige PDF-bestand op de server bij te werken.

Voeg een `/save` eindpunt aan de servercode toe die het dossier van de PDF in de uploads/concepten omslag beschrijft:

```
  // Overwrite the PDF file with latest PDF changes and annotations
  app.post( "/save", ( req, res ) => {
      if( !req.files || Object.keys( req.files ).length === 0 ) {
          return res.status( 400 ).send( "No files were uploaded." );
      }

      let file = req.files.pdf;
      file.mv( __dirname + "/uploads/drafts/" + file.name, ( err ) => {
          if( err ) {
              return res.status( 500 ).send( err );
          }
          res.send( "File uploaded" );
      });
  } );
```

Registreer een callback van de weergave PDF voor SAVE_API die de inhoud uploadt naar het /save eindpunt. U kunt de waarde voor autoSaveFrequency wijzigen, zodat uw toepassing wijzigingen op een timer automatisch kan opslaan en na voltooiing aanvullende metagegevens kan opnemen in het ingesloten bestand.

```
  adobeDCView.registerCallback(
      AdobeDC.View.Enum.CallbackType.SAVE_API,
      ( metaData, content, options ) => {
          return new Promise( ( resolve, reject ) => {
              let formData = new FormData();
              formData.append( "pdf", new Blob( [ content ] ), "drafts/" + filename
  );
              fetch( "/save", {
                  method: "POST",
                  body: formData
              }).then( resp => {
                  resolve({
                      code: AdobeDC.View.Enum.ApiResponseCode.SUCCESS,
                      data: {
                          /* Updated file metadata after successful save operation */
                          metaData: Object.assign( metaData, {} )
                      }
                  });
              });
          });
      },
      {
          autoSaveFrequency: 0,
          enableFocusPolling: false,
          showSaveButton: true
      }
  );
```

Opmerkingen en annotaties op de conceptdocumenten worden nu opgeslagen op de server. U kunt [&#x200B; meer over lezen hoe callbacks &#x200B;](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#callbacks-workflows) in uw werkschema past. Bijvoorbeeld, [&#x200B; statuscallbacks &#x200B;](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtos_ui.html#status-callback) help handvatdossierconflicten als de veelvoudige mensen op het zelfde document gelijktijdig willen herzien en becommentariëren.

In de laatste stap combineert u alle bewerkte documenten in één PDF-bestand met behulp van de PDF Services API.

## PDF-bestanden combineren

De PDF-combinatiecode is vergelijkbaar met de code voor het maken van de PDF, maar gebruikt de bewerking CombineFiles en voegt elk bestand toe als invoer.

```
  function combineFilesToPDF( files, outputFilename, callback ) {
      // Create an ExecutionContext using credentials and create a new operation
  instance.
      const executionContext = PDFToolsSdk.ExecutionContext.create( credentials ),
          combineFilesOperation = PDFToolsSdk.CombineFiles.Operation.createNew();

      // Set operation inputs from source files.
      files.forEach( file => {
          const input = PDFToolsSdk.FileRef.createFromLocalFile( file );
          combineFilesOperation.addInput( input );
      } );

      // Execute the operation and Save the result to the specified location.
      combineFilesOperation.execute( executionContext )
          .then( result => {
              result.saveAsFile( outputFilename );
              callback( outputFilename );
          } );
 }
```

## De uiteindelijke PDF downloaden

Voeg een eindpunt genaamd /finalize toe dat de functie aanroept om alle PDF-bestanden in de map `uploads/drafts` te combineren in een `Final.pdf` -bestand en dat vervolgens downloadt.

```
  app.get( "/finalize", ( req, res ) => {
      fs.readdir( __dirname + "/uploads/drafts/", ( err, files ) => {
          if( err ) {
              return res.status( 500 ).send( err );
          }
          combineFilesToPDF(
              files.filter( f => f.endsWith( ".pdf" ) ).map( f => __dirname + 
  "/uploads/drafts/" + f ),
              __dirname + "/uploads/Final.pdf", ( file ) => {
              res.download( file );
          } );
      } );
  } );
```

Tot slot voeg een verbinding in de belangrijkste index.html Web-pagina aan dit /finalize eindpunt toe. Met deze koppelingen kunnen gebruikers het resultaat van de documentworkflow downloaden.

```
<a href="/finalize">Download final PDF</a>
```

![&#x200B; Screenshot van het downloaden van definitief document &#x200B;](assets/reviews_3.png)

## Volgende stappen

Dit hands-on leerprogramma toonde aan hoe [!DNL Acrobat Services] APIs a [&#x200B; document-delend en overzichtswerkschema &#x200B;](https://developer.adobe.com/document-services/use-cases/collaboration/review-and-approval) in een Webtoepassing integreert. De toepassing stelt externe workers in staat bestanden te delen en samen te werken met hun teamgenoten. Dit is vooral handig voor werknemers en contractanten die thuis werken.

U kunt deze technieken gebruiken om samenwerking in uw app toe te laten of {de Steekproeven van SDK van de Knoop van de Diensten van 0} te onderzoeken [&#x200B; en &#x200B;](https://github.com/adobe/pdftools-node-sdk-samples) PDF bedt API Steekproeven [&#x200B; op GitHub voor inspiratie op hoe anders om Adobe APIs te gebruiken.](https://github.com/adobe/pdf-embed-api-samples)

Klaar om het delen en reviseren van documenten in uw eigen app in te schakelen? Meld u aan bij uw [[!DNL Adobe Acrobat Services] &#x200B;](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) -ontwikkelaarsaccount. Toegang tot Adobe PDF Embed gratis en geniet van een gratis proefversie van zes maanden van de andere API&#39;s. Na uw proef, kunt u [&#x200B; betalen-als-u-gaat &#x200B;](https://developer.adobe.com/document-services/pricing/main) voor enkel \$0.05 per documenttransactie aangezien uw zaken groeit.
