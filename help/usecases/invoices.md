---
title: Verwerking van facturen
description: Leer hoe je automatisch klantfacturen genereert, met een wachtwoord beveiligt en levert
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8145
thumbnail: KT-8145.jpg
exl-id: 5871ef8d-be9c-459f-9660-e2c9230a6ceb
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1343'
ht-degree: 0%

---

# Facturen verwerken

![&#x200B; Hoofdletterbanner van het Gebruik &#x200B;](assets/UseCaseInvoicesHero.jpg)

Het is fantastisch als het bedrijfsleven bloeit, maar productiviteit lijdt als het tijd is om al die facturen voor te bereiden. Het handmatig genereren van facturen is tijdrovend, plus het risico dat er een fout optreedt, waardoor er mogelijk geld verloren gaat of een klant met een onjuist bedrag wordt geconfronteerd.

Denk van Danielle, bijvoorbeeld, werkend in de [&#x200B; boekhoudingsafdeling &#x200B;](https://developer.adobe.com/document-services/use-cases/financial/invoices) [&#x200B; van een medisch leveringsbedrijf &#x200B;](https://developer.adobe.com/document-services/use-cases/financial/invoices). Het is het einde van de maand, dus trekt ze informatie uit verschillende systemen, controleert de nauwkeurigheid ervan en maakt de facturen op. Na al dat werk is ze eindelijk klaar om de documenten te converteren naar PDF (zodat iedereen ze kan bekijken zonder specifieke software aan te schaffen) en om elke klant zijn persoonlijke factuur te sturen.

Zelfs als de maandelijkse facturering is voltooid, kan Danielle deze facturen gewoon niet ontlopen. Sommige klanten hebben niet-maandelijkse factureringscycli, dus ze maakt altijd een factuur voor iemand. Af en toe bewerkt een klant zijn factuur en betaalt hij de facturen. Danielle besteedt dan tijd aan het oplossen van problemen met deze facturen. In dit tempo moet ze een assistent aannemen om het hele werk bij te houden!

Wat Danielle nodig heeft, is een manier om snel en nauwkeurig facturen te genereren, zowel in batch aan het einde van de maand als ad hoc op andere momenten. Idealiter zou ze, als ze deze facturen kon beschermen tegen bewerkingen, zich geen zorgen hoeven te maken over het oplossen van problemen met niet-overeenkomende bedragen.

## Wat je kunt leren

In deze praktische zelfstudie leert u hoe u met de API voor het genereren van Adoben documenten automatisch facturen kunt genereren, de PDF kunt beschermen met een wachtwoord en elke klant een factuur kunt sturen. Het enige wat nodig is, is een beetje kennis van Node.js, JavaScript, Express.js, HTML en CSS.

De volledige code voor dit project is [&#x200B; beschikbaar op GitHub &#x200B;](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation). U moet de openbare map instellen met uw sjabloon en de onbewerkte gegevensmappen. In productie moet u de gegevens ophalen van een externe API. U kunt ook deze gearchiveerde versie van de toepassing verkennen die de sjabloonbronnen bevat.

## Relevante API&#39;s en bronnen

* [&#x200B; de Diensten API van de PDF &#x200B;](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [&#x200B; de Generatie API van het Document van de Adobe &#x200B;](https://developer.adobe.com/document-services/apis/doc-generation)

* [&#x200B; Adobe Sign API &#x200B;](https://developer.adobe.com/adobesign-api/)

* [&#x200B; code van het Project &#x200B;](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation)

## De gegevens voorbereiden

In deze zelfstudie wordt niet gekeken naar hoe de data uit je datawarehouse worden geïmporteerd. Je klantenbestellingen kunnen bestaan in een database, externe API of aangepaste software. Adobe Document Generation API verwacht een JSON-document met de factureringsgegevens, zoals informatie van je CRM (customer relationship management) of eCommerce-platform. In deze zelfstudie wordt ervan uitgegaan dat de gegevens al in JSON-indeling zijn opgeslagen.

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

De Adobe-API voor het genereren van documenten verwacht dat een Microsoft Word-sjabloon en een JSON-document een dynamisch PDF- of Word-document maken. Creeer een malplaatje van Microsoft Word voor uw het factureren toepassing en gebruik de [&#x200B; vrije Tagger van de Generatie van het Document toe:voegen-binnen &#x200B;](https://opensource.adobe.com/pdftools-sdk-docs/docgen/latest/wordaddin.html#add-in-demo) om de malplaatjemarkeringen te produceren. Installeer de invoegtoepassing en open het tabblad in Microsoft Word.

![&#x200B; Screenshot van de Tagger van de Generatie van het Document toe:voegen-binnen &#x200B;](assets/invoices_1.png)

Als u de JSON-inhoud in de invoegtoepassing hebt geplakt, zoals hierboven is weergegeven, klikt u op Codes genereren. Deze plug-in toont de indeling van uw object. In uw standaardsjabloon kunt u de naam en het e-mailadres van de klant gebruiken, maar de bestelgegevens worden niet weergegeven. De orderinformatie wordt later in deze zelfstudie besproken.

![&#x200B; Screenshot van het malplaatje van de Auteur van de Tagger van de Productie van het Document &#x200B;](assets/invoices_2.png)

Begin in je Microsoft Word-document met het schrijven van de factuursjabloon. Laat de cursor op de plaats staan waar u dynamische gegevens moet invoegen en selecteer vervolgens de tag in het venster van de invoegtoepassing Adobe. Klik **Tekst van het Tussenvoegsel** zodat kan de Teller van de Generatie van het Document van de Adobe toe:voegen-binnen de markeringen produceren en opnemen. Voor personalisatie voegen we de naam en het e-mailadres van de klant in.

Ga nu naar de gegevens die veranderen met elke nieuwe factuur. Selecteer het **Geavanceerde** lusje van toe:voegen-binnen. Om de beschikbare opties te zien om een dynamische lijst te produceren die op de producten wordt gebaseerd een klant bevolen, klik **Lijsten en Lijsten**.

Selecteer **Orde** van eerste dropdown. Selecteer in de tweede vervolgkeuzelijst de kolommen voor deze tabel. Selecteer in deze zelfstudie alle drie kolommen voor het object dat de tabel moet renderen.

![&#x200B; Schermafbeelding van het Geavanceerde lusje van Tagger van de Generatie van het Document &#x200B;](assets/invoices_3.png)

De API voor het genereren van documenten kan ook complexe bewerkingen uitvoeren, zoals het samenvoegen van elementen binnen een array. Op het **Geavanceerde** lusje, selecteer **Numerieke Berekeningen**, en op het **lusje van de Samenvoeging**, selecteer het gebied waar u de berekening wilt toepassen.

![&#x200B; Schermafbeelding van de Numerieke berekeningen van Tagger van de Generatie van het Document &#x200B;](assets/invoices_4.png)

Klik de **knoop van de Berekening van het Tussenvoegsel** om deze markering op te nemen waar nodig binnen het document. De volgende tekst wordt nu weergegeven in uw Microsoft Word-bestand:

![&#x200B; Screenshot van markeringen in het document van Microsoft Word &#x200B;](assets/invoices_5.png)

Dit factuurvoorbeeld bevat klantgegevens, de bestelde producten en het totale verschuldigde bedrag.

## Een factuur genereren met de API voor het genereren van Adoben

Gebruik Adobe PDF Services Node.js Software Development Kit (SDK) om de Microsoft Word- en JSON-documenten te combineren. Maak een Node.js-toepassing om de factuur te maken met behulp van de Document Generation API.

De PDF Services-API bevat Document Generation Service, zodat u voor beide dezelfde referenties kunt gebruiken. Geniet van a [&#x200B; zes maanden vrije proef &#x200B;](https://developer.adobe.com/document-services/pricing/main), dan betaal enkel $0.05 per documenttransactie.

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

Deze code neemt informatie op uit het invoer-JSON-document en het invoersjabloonbestand. Vervolgens wordt een bewerking voor het samenvoegen van documenten gemaakt om de bestanden te combineren tot één PDF-rapport. Tot slot wordt de bewerking met uw API-referenties uitgevoerd. Als u hen nog niet hebt, [&#x200B; creeer geloofsbrieven &#x200B;](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html#getting-credentials) (de Generatie van het Document en de Diensten API van de PDF gebruiken de zelfde geloofsbrieven).

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

![&#x200B; Screenshot van dynamisch geproduceerde PDF factuur &#x200B;](assets/invoices_6.png)

Deze factuur bevat uw dynamische gegevens uit het JSON-document.

## Facturen beveiligen met een wachtwoord

Aangezien Danielle de accountant zich zorgen maakt over het wijzigen van de factuur door klanten, kunt u een wachtwoord toepassen om het bewerken te beperken. [&#x200B; de Diensten API van de PDF &#x200B;](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html) kan een wachtwoord op documenten automatisch toepassen. Hier gebruikt u Adobe PDF Services SDK om de documenten met een wachtwoord te beveiligen. De code is:

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

Wanneer u deze code gebruikt, wordt uw document beveiligd met een wachtwoord en wordt een nieuwe factuur naar het systeem geüpload. Voor meer op hoe deze code wordt gebruikt, of om het uit te proberen, zie de [&#x200B; codesteekproef &#x200B;](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-invoice-generation).

Als u klaar bent met de factuur, kunt u deze automatisch naar de klant e-mailen. Er zijn een paar manieren om automatisch uw klanten te e-mailen. De snelste manier is een derde e-mail API samen met een helperbibliotheek zoals [&#x200B; sendgrid-nodejs &#x200B;](https://github.com/sendgrid/sendgrid-nodejs) te gebruiken. Alternatief, als u reeds toegang tot een server hebt SMTP kunt u [&#x200B; nodemailer &#x200B;](https://www.npmjs.com/package/nodemailer) gebruiken om e-mails via SMTP te verzenden.

## Volgende stappen

In dit hands-on leerprogramma, creeerde u een eenvoudige app om Danielle in boekhouding met [&#x200B; het factureren &#x200B;](https://developer.adobe.com/document-services/use-cases/financial/invoices) te helpen. Met behulp van de PDF Services-API en de SDK voor het genereren van documenten hebt u een Microsoft Word-sjabloon gevuld met bestelgegevens van klanten uit een JSON-document, waardoor een PDF-factuur werd gemaakt. Dan, wachtwoord-beschermde elk document gebruikend de diensten van de wachtwoordbescherming door [&#x200B; PDF Services API &#x200B;](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html).

Aangezien Danielle automatisch facturen kan genereren en zich geen zorgen hoeft te maken over het bewerken van facturen door klanten, hoeft ze geen assistent in te huren voor al het handmatige werk. Ze kan haar extra tijd gebruiken om kostenbesparingen te vinden in de te betalen bestanden.

Nu je ziet hoe eenvoudig het is, kun je deze eenvoudige app uitbreiden met behulp van andere tools voor Adobe om facturen in te sluiten op je website. Zo kunnen klanten hun facturen of saldo op elk gewenst moment bekijken. [&#x200B; Adobe PDF Embed API &#x200B;](https://developer.adobe.com/document-services/apis/pdf-embed) is vrij te gebruiken. Je kunt zelfs naar de afdeling Personeelszaken of Verkoop gaan, zodat je hun overeenkomsten kunt automatiseren en elektronische handtekeningen kunt verzamelen.

Als u alle mogelijkheden wilt verkennen en uw eigen handige toepassing wilt gaan ontwikkelen, maakt u een gratis [[!DNL Adobe Acrobat Services] &#x200B;](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) -account om vandaag aan de slag te gaan. Geniet van een zes maanden vrije proef toen [&#x200B; betaal-als-u-gaat &#x200B;](https://developer.adobe.com/document-services/pricing/main)
voor slechts USD 0,05 per documenttransactie op het moment dat je bedrijf schaalt.
