---
title: Student-Teacher Collaboration
description: Leer hoe je een online leerplatform creëert waarmee docenten en studenten eenvoudig resources delen in PDF
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8091.jpg
kt: 8091
exl-id: 570a635c-e539-4afc-a475-ecf576415217
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '1485'
ht-degree: 0%

---

# Samenwerking tussen studenten en docenten

![Hoofdlettergebruik hoofdbanner gebruiken](assets/UseCaseStudentHero.jpg)

Onderwijsinstellingen gebruiken PDF-documenten om leermateriaal met studenten te delen. PDF biedt docenten een uitwisselbaar documentformaat.

Integreren [Adobe PDF Services API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html) en [Adobe PDF Embed API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) in een app bieden docenten en studenten één platform waarop ze kunnen onderwijzen en leren. Uw app kan studenten bijvoorbeeld de mogelijkheid bieden vragen te stellen over hun opdrachten en rapportkaarten en samen te werken aan groepstoewijzingen.

Er is een officiële SDK voor Node.js-toepassingen voor toegang tot de PDF Services-API. Zo kunt u documenten zoals Microsoft Word of Microsoft Excel converteren naar PDF. Bovendien kunt u meer geavanceerde bewerkingen uitvoeren, zoals het combineren van meerdere rapporten, het opnieuw rangschikken van pagina&#39;s en het beschermen van PDF. Voor meer informatie kunt u de [productdocumentatie](https://www.adobe.io/apis/documentcloud/dcsdk/).

## Wat je kunt leren

Leer in deze praktische zelfstudie een online leerplatform te maken dat [stelt docenten en studenten in staat eenvoudig middelen te delen](https://www.adobe.io/apis/documentcloud/dcsdk/student-teacher-collaboration.html) in PDF. Deze zelfstudie gebruikt een [leerportaal](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers) gemaakt met Node.js JavaScript-runtime (Node.js) en PDF Services.

Het leerportaal heeft de volgende functies:

* Hiermee kunnen docenten bronnen uploaden

* Laat studenten toe om veelvoudige documenten te selecteren om in PDF om te zetten

* Schakelt de conversie van documenten naar PDF in

* Verstrekt een voorproef van de PDF voor studenten in Webbrowser en laat hen toe om de documenten zonder extra software aan te tekeningen te maken

* Laat studenten toe om commentaren te verlaten en hen te downloaden aan hun computers

Meer informatie [!DNL Adobe Acrobat Services] Geef je studenten een rijke ervaring met PDF. [!DNL Acrobat Services] API’s integreren naadloos in je bestaande toepassingen, zodat studenten bestanden kunnen uploaden, converteren en bekijken, vervolgens opmerkingen maken en opslaan — alles binnen je huidige setup.

## Relevante API&#39;s en bronnen

* [PDF Embed-API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [PDF Services API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [Projectcode](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers)

## Bronnen uploaden naar de leerportal

In de sectie van de leerkrachten van het leerportaal kunnen leerkrachten documenten uploaden zoals opdrachten en tests. De documenten kunnen elke indeling hebben, zoals Microsoft Word, Microsoft Excel, HTML, verschillende afbeeldingsindelingen, enzovoort.

![Screenshot van het leerprogramma van de docentenafdeling](assets/edu_1.png)

Geüploade documenten worden opgeslagen en aan de studenten getoond wanneer zij hun webpagina openen.

Als u wilt weten hoe de toepassing de bestanden uploadt, raadpleegt u de [projectcode](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers).

## Documenten omzetten in PDF

Studenten kunnen een of meer documenten van elk type omzetten in PDF, zoals Microsoft Word, Excel en PowerPoint, en in andere veelgebruikte tekst- en afbeeldingsbestandstypen. Het leerportaal gebruikt PDF Services om bestanden om te zetten in PDF.

Als u uw eigen leerportal wilt maken, moet u eerst uw eigen referenties maken. [Registreren](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) om PDF Services API gratis te gebruiken voor zes maanden en tot 1.000 documenttransacties. Daarna: [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) bij slechts \$0.05 per documenttransactie aangezien de klasse hun toewijzingen omhoog leidt.

Wanneer een student een document van het dashboard selecteert, zien zij het volgende:

![Screenshot van studentensectie van leerportaal](assets/edu_2.png)

De student selecteert gewoon de documenten voor conversie en klikt op **Rapport ophalen**.

De leerportal zet de documenten om in PDF en geeft een rapportpagina weer, samen met een voorvertoning van het PDF-bestand.

Hier volgt de voorbeeldcode voor deze stap:

```
async function createPdf(rawFile, outputPdf) {
    try {
            // configurations
            const credentials =  adobe.Credentials
            .serviceAccountCredentialsBuilder()
            .fromFile("./src/pdftools-api-credentials.json")
            .build();
 
            // Capture the credential from app and show create the context
            const executionContext = adobe.ExecutionContext.create(credentials),
            operation = adobe.CreatePDF.Operation.createNew();
 
            // Pass the content as input (stream)
            const input = adobe.FileRef.createFromLocalFile(rawFile);
            operation.setInput(input);
 
            // Async create the PDF
            let result = await operation.execute(executionContext);
            await result.saveAsFile(outputPdf);
    } catch (err) {
            console.log('Exception encountered while executing operation', err);
    }
}
```

De voorbeeldcode roept de `createPdf` methode binnen de Uitdrukkelijke routemanager om de PDF te produceren.

Zie voor meer informatie over het aanroepen van deze methode [de projectcode](https://github.com/afzaal-ahmad-zeeshan/adobe-pdf-tools-for-teachers/blob/master/src/helpers/pdf.js).

## Een voorvertoning weergeven van de leermiddelen

De gebruikersinterface gebruikt PDF Embed API om PDF in een webbrowser te renderen. Deze API kan gratis worden gebruikt.

PDF Embed API gebruikt een andere referentie dan PDF Services API, dus u moet [een referentie maken](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)
voordat u het kunt gebruiken. Vervolgens kunt u PDF insluiten volledig gratis gebruiken.

Zorg ervoor dat u de juiste URL van de website in het token invoert. Anders kunt u de PDF mogelijk niet met het token renderen.

De gebruikersinterface gebruikt de [Werkbalken](https://handlebarsjs.com/) sjabloontaal. De PDF wordt in een webbrowser weergegeven.

Hier volgt de code voor deze stap:

```
<div id="adobe-dc-view" style="height: 750px; width: 700px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
    document.addEventListener("adobe_dc_view_sdk.ready", function () {
        var adobeDCView = new AdobeDC.View({ clientId: "<your-credentials-here>", divId: "adobe-dc-view" });
        adobeDCView.previewFile(
            {
                content: {
                    location: { url: "<file-url>" }
                },
                    metaData: { fileName: "<file-name>" }
            },
           );
    });
</script>
 
<p>Material has been generated, <a href="/students/download/{{filename}}" target="_blank">click here</a> to download it.
</p>
```

Deze code geeft de uitvoer van de PDF en de koppeling voor het downloaden van het PDF-rapport weer, zoals in de onderstaande schermvastlegging wordt getoond:

![Screenshot van student PDF preview](assets/edu_3.png)

De studenten zouden het rapport moeten kunnen downloaden of aan het materiaal hier werken.

## PDF-documenten annoteren

Een leerplatform moet basisannotaties, opmerkingen en discussies in PDF ondersteunen. PDF Embed API biedt al deze functies. Hiermee wordt ondersteuning voor annotaties geactiveerd met `showAnnotationTools`, zodat docenten en studenten commentaar kunnen geven op de documenten en opmerkingen kunnen archiveren als onderdeel van de PDF.

Als u annotaties in PDF-documenten wilt inschakelen, hoeft u alleen het argument door te geven `showAnnotationTools` : de `previewFile` methode. Hiermee wordt het gereedschap Annotaties weergegeven in de voorvertoning van de PDF. Gebruik dit gereedschap in het menu met de drie puntjes rechtsboven in de voorvertoning.

![Schermafbeelding van gereedschappen voor opmerkingen in PDF](assets/edu_4.png)

In de documenten die door docenten zijn geüpload, kunnen studenten tekst markeren, opmerkingen toevoegen enzovoort.

![Screenshot van het toevoegen van opmerking in PDF](assets/edu_5.png)

In de bovenstaande schermvastlegging is de gebruiker gelabeld &quot;Gast&quot;, maar u kunt profielen voor gebruikers configureren, zoals studenten en docenten.

Wanneer een student een annotatie toepast, geeft de PDF Embed-API een **Opslaan** langs de bovenste banner. Bij opslaan worden de annotaties aan het bestand toegevoegd. Klik **Opslaan** om te zien hoe het bestand wordt opgeslagen met de annotatie die is ingesloten in het rapport.

Studenten kunnen annotaties gebruiken om vragen te stellen of hun opmerkingen over het leermateriaal te delen.

## Het gebruik van bijgehouden documenten

Het is belangrijk voor leraren en scholen om te zien hoe studenten gebruik maken van onlineplatforms. Dit helpt leraren hun studenten met middelen steunen die hen helpen beter op hun taken presteren. De API voor insluiten van PDF is geïntegreerd met analytics die u kunt gebruiken om alle gebeurtenissen te meten die plaatsvinden, zoals wanneer gebruikers documenten openen, lezen en sluiten. Met de PDF Services-API kunnen docenten ook afdrukken, downloaden en bestandswijzigingen uitschakelen om de academische integriteit te behouden.

Als u een [Adobe Analytics](https://www.adobe.io/apis/experiencecloud/analytics.html) licentie, kunt u de [kant-en-klare integratie](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfembed/controlpdfexperience.html?lang=en#adobe-analytics). Gebruik anders callbacks om uw PDF Services te integreren met andere analyseproviders, zoals [Google](https://experienceleague.adobe.com/docs/document-services/tutorials/pdfembed/controlpdfexperience.html?lang=en#google-analytics).

Als u het meten van documentgebeurtenissen wilt inschakelen, koppelt u de gebeurtenishandlers met de `registerCallback` methode met Adobe DC View-instantie. U kunt basisgegevens, zoals het openen van een document of het lezen van een pagina, weergeven op de console. U kunt de metrics ook opslaan in een logbestand of publiceren in andere analytics-winkels.

Hier volgt een voorbeeldcode voor het koppelen van de gebeurtenishandlers:

```
adobeDCView.registerCallback(
    AdobeDC.View.Enum.CallbackType.EVENT_LISTENER,
    function(event) {
           console.log(event);
    },
    {
           enablePDFAnalytics: true
    }
);
```

Leerkrachten kunnen zien hoeveel studenten de opdracht hebben gezien, hoeveel leerlingen alle pagina&#39;s van hun notities hebben bezocht en andere waardevolle details.

Hier volgt een schermvastlegging van webbrowserconsole:

![Screenshot van webbrowserconsole](assets/edu_6.png)

Deze schermopname toont dat de student het toewijzingsbestand heeft geopend, de eerste pagina heeft gelezen — ze hebben niet naar extra pagina&#39;s geschoven of het document heeft maar één pagina — en vervolgens heeft het bestand gedownload. Je kunt deze metrics verzamelen om analyses uit te voeren en het gedrag van je studenten te bestuderen.

Ook [Adobe Analytics](https://business.adobe.com/products/analytics/adobe-analytics.html) is geïntegreerd met de PDF Embed-API. Als u dus een abonnement hebt op de Adobe Analytics-suite, kunt u uw metrics in uw abonnement publiceren. Als u de metrics in Adobe Analytics wilt publiceren, hoeft u uw suite-id alleen door te geven aan de PDF Embed API-constructor. (Let op: u moet uw PDF Embed API-referenties gebruiken, niet uw PDF Services API-referenties).

Hier volgt een voorbeeldcode die laat zien hoe u de suite-id doorgeeft aan de PDF Embed API-constructor:

```
var adobeDCView = new AdobeDC.View({
    clientId: "<your-adobe-dc-credential>",
    divId: "<#element>"
    reportSuiteId: <your-id-here>,
}); 
```

## Volgende stappen

In deze praktische zelfstudie werd besproken hoe u de PDF Services API en de PDF Embed-API kunt gebruiken om een leerportaal te maken, waardoor u effectief [samenwerking tussen studenten en docenten](https://www.adobe.io/apis/documentcloud/dcsdk/student-teacher-collaboration.html). Met behulp van dit portaal kunnen docenten leermateriaal in elke indeling uploaden en dit converteren naar PDF met behulp van de PDF Services API. Studenten kunnen deze PDF vervolgens voorvertonen met de PDF Embed-API.

Nu u weet hoe u rapporten van de PDF kunt annoteren, de annotaties kunt archiveren, en het gebruik van PDF-rapporten kunt volgen, kunt u beginnen deze oplossingen in uw eigen projecten uit te voeren.

U kunt [!DNL Adobe Acrobat Services] API’s om gebruikersvriendelijke, interactieve PDF-ervaringen op je website te creëren. Geniet van het gratis gebruik van de Adobe PDF Services API gedurende zes maanden en dan gewoon [pay-as-you-go](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html) (via AWS of een directe overeenkomst) voor slechts \$0,05 per documenttransactie. Gebruik Adobe PDF Embed free zonder tijdlimiet. Een gratis account maken voor [aan de slag](https://www.adobe.com/go/dcsdks_credentials) vandaag.
