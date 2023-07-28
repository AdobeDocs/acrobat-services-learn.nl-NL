---
title: Verwerking van facturen
description: Leer hoe je automatisch klantfacturen genereert, met een wachtwoord beveiligt en levert
role: Developer
level: Intermediate
type: Tutorial
feature: Use Cases
thumbnail: KT-8145.jpg
jira: KT-8145
exl-id: 5871ef8d-be9c-459f-9660-e2c9230a6ceb
source-git-commit: b65ffa3efa3978587564eb0be0c0e7381c8c83ab
workflow-type: tm+mt
source-wordcount: '1427'
ht-degree: 1%

---

# Facturen verwerken

![Hoofdlettergebruik hoofdbanner gebruiken](assets/UseCaseInvoicesHero.jpg)

Het is fantastisch als het bedrijfsleven bloeit, maar productiviteit lijdt als het tijd is om al die facturen voor te bereiden. Het handmatig genereren van facturen is tijdrovend, plus het risico dat er een fout optreedt, waardoor er mogelijk geld verloren gaat of een klant met een onjuist bedrag wordt geconfronteerd.

Denk bijvoorbeeld aan Danielle die werkt in de [boekhoudafdeling](https://www.adobe.io/apis/documentcloud/dcsdk/invoices.html) [van een bedrijf voor medische verzorging](https://www.adobe.io/apis/documentcloud/dcsdk/invoices.html). Het is het einde van de maand, dus trekt ze informatie uit verschillende systemen, controleert de nauwkeurigheid ervan en maakt de facturen op. Na al dat werk is ze eindelijk klaar om de documenten te converteren naar PDF (zodat iedereen ze kan bekijken zonder specifieke software aan te schaffen) en om elke klant zijn persoonlijke factuur te sturen.

Zelfs als de maandelijkse facturering is voltooid, kan Danielle deze facturen gewoon niet ontlopen. Sommige klanten hebben niet-maandelijkse factureringscycli, dus ze maakt altijd een factuur voor iemand. Af en toe bewerkt een klant zijn factuur en betaalt hij de facturen. Danielle besteedt dan tijd aan het oplossen van problemen met deze facturen. In dit tempo moet ze een assistent aannemen om het hele werk bij te houden!

Wat Danielle nodig heeft, is een manier om snel en nauwkeurig facturen te genereren, zowel in batch aan het einde van de maand als ad hoc op andere momenten. Idealiter zou ze, als ze deze facturen kon beschermen tegen bewerkingen, zich geen zorgen hoeven te maken over het oplossen van problemen met niet-overeenkomende bedragen.

## Wat je kunt leren

In deze praktische zelfstudie leert u hoe u de API voor het genereren van Adobe-documenten kunt gebruiken om automatisch facturen te genereren, de PDF te beschermen met een wachtwoord en elke klant een factuur te sturen. Het enige wat nodig is, is een beetje kennis van Node.js, JavaScript, Express.js, HTML en CSS.

De volledige code voor dit project is [beschikbaar op GitHub](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation). U moet de openbare map instellen met uw sjabloon en de onbewerkte gegevensmappen. In productie moet u de gegevens ophalen van een externe API. U kunt ook deze gearchiveerde versie van de toepassing verkennen die de sjabloonbronnen bevat.

## Relevante API&#39;s en bronnen

* [PDF Services API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Adobe-API voor documentgeneratie](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html)

* [Adobe Sign-API](https://www.adobe.io/apis/documentcloud/sign.html)

* [Projectcode](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation)

## De gegevens voorbereiden

In deze zelfstudie wordt niet gekeken naar hoe de data uit je datawarehouse worden geïmporteerd. Je klantenbestellingen kunnen bestaan in een database, externe API of aangepaste software. Adobe Document Generation API verwacht een JSON-document met de factureringsgegevens, zoals informatie van je CRM (Customer Relationship Management) of eCommerce-platform. In deze zelfstudie wordt ervan uitgegaan dat de gegevens al in JSON-indeling zijn opgeslagen.

Voor het gemak gebruikt u de volgende JSON-structuur voor facturering:

```
{ 
    "customerName": "John Doe", 
    "customerEmail": "john-doe@example.com", 
    "order": [ 
        { 
            "productId": 26, 
            "productTitle": "Bandages", 
            "price": 15.82 
        }, 
        { 
            "productId": 54, 
            "productTitle": "Masks", 
            "price": 25 
        }, 
        { 
            "productId": 76, 
            "productTitle": "Gloves", 
            "price": 7.59 
        } 
    ] 
} 
```

Het JSON-document bevat zowel de klantgegevens als bestelgegevens. Met dit gestructureerde document kunt u uw factuur samenstellen en de elementen in PDF-indeling weergeven.

## Een factuursjabloon maken

De Adobe-API voor het genereren van documenten verwacht dat een Microsoft Word-sjabloon en een JSON-document een dynamisch PDF- of Word-document maken. Maak een Microsoft Word-sjabloon voor uw factureringstoepassing en gebruik de [gratis invoegtoepassing voor het genereren van documenten](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo) om de sjabloontags te genereren. Installeer de invoegtoepassing en open het tabblad in Microsoft Word.

![Screenshot van de invoegtoepassing voor documentgeneratietags](assets/invoices_1.png)

Als u de JSON-inhoud in de invoegtoepassing hebt geplakt, zoals hierboven is weergegeven, klikt u op Codes genereren. Deze plug-in toont de indeling van uw object. In uw standaardsjabloon kunt u de naam en het e-mailadres van de klant gebruiken, maar de bestelgegevens worden niet weergegeven. De orderinformatie wordt later in deze zelfstudie besproken.

![Screenshot van de sjabloon Auteur van tag voor documentgeneratie](assets/invoices_2.png)

Begin in je Microsoft Word-document met het schrijven van de factuursjabloon. Laat de cursor op de plaats staan waar u dynamische gegevens moet invoegen en selecteer vervolgens de tag in het venster van de invoegtoepassing Adobe. Klikken **Tekst invoegen** Zo kan de invoegtoepassing Adobe Document Generation Tagger de tags genereren en invoegen. Voor personalisatie voegen we de naam en het e-mailadres van de klant in.

Ga nu naar de gegevens die veranderen met elke nieuwe factuur. Selecteer de **Gedeeld** van de invoegtoepassing. Als u de beschikbare opties wilt zien om een dynamische tabel te genereren op basis van de producten die een klant heeft besteld, klikt u op **Tabellen en lijsten** .

Selecteren **Volgorde** vanaf de eerste vervolgkeuzelijst. Selecteer in de tweede vervolgkeuzelijst de kolommen voor deze tabel. Selecteer in deze zelfstudie alle drie kolommen voor het object dat de tabel moet renderen.

![Screenshot van het tabblad Geavanceerde documentgeneratietags](assets/invoices_3.png)

De API voor het genereren van documenten kan ook complexe bewerkingen uitvoeren, zoals het samenvoegen van elementen binnen een array. In het dialoogvenster **Gedeeld** tab, selecteer **Numerieke berekeningen** en in de **Samenvoeging** selecteert u het veld waarop u de berekening wilt toepassen.

![Screenshot van de numerieke berekeningen van Tagger voor documentgeneratie](assets/invoices_4.png)

Klik op de knop **Berekening invoegen** om deze tag waar nodig in het document in te voegen. De volgende tekst wordt nu weergegeven in uw Microsoft Word-bestand:

![Screenshot van tags in Microsoft Word-document](assets/invoices_5.png)

Dit factuurvoorbeeld bevat klantgegevens, de bestelde producten en het totale verschuldigde bedrag.

## Een factuur genereren met de API voor het genereren van Adobe-documenten

Gebruik Adobe PDF Services Node.js Software Development Kit (SDK) om de Microsoft Word- en JSON-documenten te combineren. Maak een Node.js-toepassing om de factuur te maken met behulp van de Document Generation API.

De PDF Services-API bevat Document Generation Service, zodat u voor beide dezelfde referenties kunt gebruiken. Geniet van [gratis proefversie van zes maanden](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html)en betaal vervolgens slechts USD 0,05 per documenttransactie.

Hier volgt de code om de PDF samen te voegen:

```
async function compileDocFile(json, inputFile, outputPdf) { 
    try { 
        // configurations 
        const credentials =  adobe.Credentials 
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

Deze code neemt informatie op uit het invoer-JSON-document en het invoersjabloonbestand. Vervolgens wordt een bewerking voor het samenvoegen van documenten gemaakt om de bestanden te combineren tot één PDF-rapport. Tot slot wordt de bewerking met uw API-referenties uitgevoerd. Als u deze nog niet hebt, [referenties maken](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getting-credentials) (Document Generation en PDF Services API gebruiken dezelfde referenties).

Gebruik deze code binnen de Uitdrukkelijke router om het documentverzoek te behandelen:

```
// Create one report and send it back
try {
    console.log(\`[INFO] generating the report...\`);
    const fileContent = fs.readFileSync(\`./public/documents/raw/\${vendor}\`,
    'utf-8');
    const parsedObject = JSON.parse(fileContent);

    await pdf.compileDocFile(parsedObject,
    \`./public/documents/template/Adobe-Invoice-Sample.docx\`,
    \`./public/documents/processed/output.pdf\`);

    await pdf.applyPassword("p@55w0rd", './public/documents/processed/output.pdf',
    './public/documents/processed/output-secured.pdf');

    console.log(\`[INFO] sending the report...\`);
    res.status(200).render("preview", { page: 'invoice', filename: 'output.pdf' });
} catch(error) {
    console.log(\`[ERROR] \${JSON.stringify(error)}\`);
    res.status(500).render("crash", { error: error });
}
```

Nadat deze code is uitgevoerd, wordt een PDF-document met de dynamisch gegenereerde factuur weergegeven op basis van de verschafte gegevens. Met de JSON-voorbeeldgegevens (zoals hierboven vermeld) is de uitvoer van deze code:

![Screenshot van dynamisch gegenereerde PDF-factuur](assets/invoices_6.png)

Deze factuur bevat uw dynamische gegevens uit het JSON-document.

## Facturen beveiligen met een wachtwoord

Aangezien Danielle de accountant zich zorgen maakt over het wijzigen van de factuur door klanten, kunt u een wachtwoord toepassen om het bewerken te beperken. [PDF Services API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html) U kunt automatisch een wachtwoord toepassen op documenten. Hier gebruikt u Adobe PDF Services SDK om de documenten met een wachtwoord te beveiligen. De code is:

```
async function applyPassword(password, inputFile, outputFile) {
    try {
        // Initial setup, create credentials instance.
        const credentials = adobe.Credentials
        .serviceAccountCredentialsBuilder()
        .fromFile("./src/pdftools-api-credentials.json")
        .build();

        // Create an ExecutionContext using credentials
        const executionContext = adobe.ExecutionContext.create(credentials);
        // Create new permissions instance and add the required permissions
        const protectPDF = adobe.ProtectPDF,
        protectPDFOptions = protectPDF.options;
        // Build ProtectPDF options by setting an Owner/Permissions Password, Permissions,
        // Encryption Algorithm (used for encrypting the PDF file) and specifying the type of content to encrypt.
        const options = new protectPDFOptions.PasswordProtectOptions.Builder()
        .setOwnerPassword(password)
        .setEncryptionAlgorithm(protectPDFOptions.EncryptionAlgorithm.AES_256)
        .build();

        // Create a new operation instance.
        const protectPDFOperation = protectPDF.Operation.createNew(options);

        // Set operation input from a source file.
        const input = adobe.FileRef.createFromLocalFile(inputFile);
        protectPDFOperation.setInput(input);

        // Execute the operation and Save the result to the specified location.
        let result = await protectPDFOperation.execute(executionContext);

        result.saveAsFile(outputFile);
    } catch (err) {
        console.log('Exception encountered while executing operation', err);
    }
}
```

Wanneer u deze code gebruikt, wordt uw document beveiligd met een wachtwoord en wordt een nieuwe factuur naar het systeem geüpload. Zie voor meer informatie over het gebruik van deze code of om deze uit te proberen de [codevoorbeeld](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation).

Als u klaar bent met de factuur, kunt u deze automatisch naar de klant e-mailen. Er zijn een paar manieren om automatisch uw klanten te e-mailen. De snelste manier is om een e-mail-API van derden te gebruiken in combinatie met een hulpbibliotheek zoals [sendgrid-nodejs](https://github.com/sendgrid/sendgrid-nodejs). Als u al toegang hebt tot een SMTP-server, kunt u ook [nodemailer](https://www.npmjs.com/package/nodemailer) om e-mails te verzenden via SMTP.

## Volgende stappen

In deze praktische zelfstudie hebt u een eenvoudige app gemaakt om Danielle te helpen bij het maken van een boekhouding met [facturering](https://www.adobe.io/apis/documentcloud/dcsdk/invoices.html). Met behulp van de PDF Services-API en de SDK voor het genereren van documenten hebt u een Microsoft Word-sjabloon gevuld met bestelgegevens van klanten uit een JSON-document, waardoor een PDF-factuur werd gemaakt. Vervolgens wordt elk document met een wachtwoord beveiligd door [PDF Services API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html).

Aangezien Danielle automatisch facturen kan genereren en zich geen zorgen hoeft te maken over het bewerken van facturen door klanten, hoeft ze geen assistent in te huren voor al het handmatige werk. Ze kan haar extra tijd gebruiken om kostenbesparingen te vinden in de te betalen bestanden.

Nu je ziet hoe eenvoudig het is, kun je deze eenvoudige app uitbreiden met andere Adobe-tools om facturen in te sluiten op je website. Zo kunnen klanten hun facturen of saldo op elk gewenst moment bekijken. [Adobe PDF Embed-API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) is gratis. Je kunt zelfs naar de afdeling Personeelszaken of Verkoop gaan, zodat je hun overeenkomsten kunt automatiseren en elektronische handtekeningen kunt verzamelen.

Als u alle mogelijkheden wilt verkennen en uw eigen handige toepassing wilt ontwikkelen, maakt u een gratis [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) account om vandaag nog aan de slag te gaan. Profiteer van een gratis proefversie van zes maanden [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html)
voor slechts USD 0,05 per documenttransactie op het moment dat je bedrijf schaalt.
