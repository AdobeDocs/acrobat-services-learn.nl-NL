---
title: Beheer je online PDF-ervaring en verzamel Analytics
description: In deze praktische zelfstudie leert u hoe u de Adobe PDF Embed-API kunt gebruiken om het uiterlijk te beheren, samenwerking in te schakelen en analytics te verzamelen over de interactie van de gebruiker met de PDF, inclusief de tijd die op een pagina is doorgebracht en zoekacties
type: Tutorial
role: Developer
level: Intermediate
thumbnail: KT-7487.jpg
jira: KT-7487
exl-id: 64ffdacb-d6cb-43e7-ad10-bbd8afc0dbf4
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '1496'
ht-degree: 0%

---

# Beheer je online PDF-ervaring en verzamel analytics

Plaatst uw organisatie PDF op uw website? Leer hoe u de Adobe PDF Embed API kunt gebruiken om het uiterlijk te beheren, samenwerking in te schakelen en analytics te verzamelen over de interactie van de gebruiker met PDF, waaronder de tijd die op een pagina wordt doorgebracht en zoekopdrachten. Selecteer *Aan de slag met de PDF Embed-API*.

<table style="table-layout:fixed">
<tr>
  <td>
    <a href="controlpdfexperience.md#part1">
        <img alt="Deel 1: Aan de slag met de PDF Embed-API" src="assets/ControlPDFPart1_thumb.png" />
    </a>
    <div>
    <a href="controlpdfexperience.md#part1"><strong>Deel 1: Aan de slag met de PDF Embed-API</strong></a>
    </div>
  </td>
  <td>
    <a href="controlpdfexperience.md#part2">
        <img alt="Deel 2: API voor insluiten van PDF toevoegen aan een webpagina" src="assets/ControlPDFPart2_thumb.png" />
    </a>
    <div>
    <a href="controlpdfexperience.md#part2"><strong>Deel 2: API voor insluiten van PDF toevoegen aan een webpagina</strong></a>
    </div>
  </td>
  <td>
   <a href="controlpdfexperience.md#part3">
      <img alt="Deel 3: Toegang tot API&apos;s van Analytics" src="assets/ControlPDFPart3_thumb.png" />
   </a>
    <div>
    <a href="controlpdfexperience.md#part3"><strong>Deel 3: Toegang tot API's van Analytics</strong></a>
    </div>
  </td>
  <td>
   <a href="controlpdfexperience.md#part4">
      <img alt="Deel 4: Interactiviteit toevoegen op basis van gebeurtenissen" src="assets/ControlPDFPart4_thumb.png" />
   </a>
    <div>
    <a href="controlpdfexperience.md#part4"><strong>Deel 4: Interactiviteit toevoegen op basis van gebeurtenissen</strong></a>
    </div>
  </td>
</tr>
</table>

## Deel 1: Aan de slag met de PDF Embed-API {#part1}

In deel 1 leert u hoe u aan de slag kunt gaan met alles wat u nodig hebt voor onderdelen 1-3. U begint met het ophalen van API-referenties.

**Wat je nodig hebt**

* Bronnen voor zelfstudies [downloaden](https://github.com/benvanderberg/adobe-pdf-embed-api-tutorial)
* Adobe ID [hier een ophalen](https://accounts.adobe.com/nl)
* Webserver (Node JS, PHP, enz.)
* Werkkennis van HTML / JavaScript / CSS

**Wat we gebruiken**

* Een basiswebserver (knooppunt)
* Visual Studio Code
* GitHub

### Referenties ophalen

1. Ga naar de [Adobe.io-website](https://www.adobe.io/).
1. Klikken **[!UICONTROL Meer informatie]** onder Maak aansprekende documentervaringen.

   ![Screenshot van de knop Meer informatie](assets/ControlPDF_1.png)

   Hiermee gaat u naar de [!DNL Adobe Acrobat Services] startpagina.

1. Klikken **[!UICONTROL Aan de slag]** in de navigatiebalk.

   U ziet een optie in **Aan de slag met [!DNL Acrobat Services] API&#39;s** aan **Nieuwe referenties maken** of **Bestaande referenties beheren**.

1. Klikken **[!UICONTROL Aan de slag]** knop onder **[!UICONTROL Nieuwe referenties maken]**.

   ![Screenshot van de knop Aan de slag](assets/ControlPDF_2.png)

1. Kies de **[!UICONTROL PDF Embed-API]** en voeg in het volgende venster een referentienaam van uw keuze en een toepassingsdomein toe.

   >[!NOTE]
   >
   >Deze referenties kunnen alleen worden gebruikt in het toepassingsdomein dat hier wordt vermeld. U kunt elk gewenst domein gebruiken.

   ![Schermafbeelding van referenties](assets/ControlPDF_3.png)

1. Klikken **[!UICONTROL Referenties maken]**.

   Op de laatste pagina van de wizard vindt u de gegevens van de clientreferenties. Laat dit venster open zodat u er weer naar kunt terugkeren en de client-id (API-sleutel) voor later gebruik kunt kopiëren.

1. Klikken **[!UICONTROL Documentatie weergeven]** voor meer informatie over het gebruik van deze API in de documentatie.

   ![Screenshot van knop met aanmeldingsgegevens maken](assets/ControlPDF_4.png)

## Deel 2: API voor insluiten van PDF toevoegen aan een webpagina {#part2}

In deel 2 leert u hoe u de PDF Embed-API gemakkelijk kunt insluiten in een webpagina. U doet dit door de online demo van de Adobe PDF Embed API te gebruiken om onze code te maken.

### De oefencode ophalen

We hebben code gemaakt die je kunt gebruiken. Terwijl u uw eigen code kunt gebruiken, zullen de demonstraties plaatsvinden in de context van de leermiddelen. Voorbeeldcode downloaden [hier](https://github.com/benvanderberg/adobe-pdf-embed-api-tutorial).

1. Ga naar [[!DNL Adobe Acrobat Services] website](https://www.adobe.io/apis/documentcloud/dcsdk/).

   ![Screenshot van [!DNL Adobe Acrobat Services] website](assets/ControlPDF_6.png)

1. Klikken **[!UICONTROL API&#39;s]** in de navigatiebalk, ga dan naar de **[!UICONTROL PDF Embed-API]** in de vervolgkeuzelijst.

   ![Schermafbeelding van vervolgkeuzelijst PDF Embed API](assets/ControlPDF_7.png)

1. Klikken **[!UICONTROL Demo uitproberen]**.

   Er verschijnt een nieuw venster met de ontwikkelaarssandbox voor de PDF Embed-API.

   ![Screenshot van de demo van Try](assets/ControlPDF_8.png)

   Hier ziet u de opties voor de verschillende weergavemodi.

1. Klik op de verschillende weergavemodi voor Volledig venster, Container met grootte, In-line en Lichtbak.

   ![Schermafbeelding van weergavemodi](assets/ControlPDF_9.png)

1. Klikken **[!UICONTROL Volledig venster]** weergavemodus, klikt u op de knop **[!UICONTROL Aanpassen]** om opties in en uit te schakelen.

   ![Screenshot van knop Aanpassen](assets/ControlPDF_10.png)

1. Uitschakelen **[!UICONTROL Downloaden]** PDF.
1. Klikken **[!UICONTROL Code genereren]** om de codevoorvertoning weer te geven.
1. Kopiëren **[!UICONTROL Client-id]** in het venster Clientreferenties uit deel 1.

   ![Screenshot van client-id](assets/ControlPDF_11.png)

1. Open de **[!UICONTROL Web]** -> **[!UICONTROL middelen]** -> **[!UICONTROL js]** -> **[!UICONTROL dc-config.js]** in uw code-editor.

   U zult zien dat de clientID variabele daar is.

1. Plak uw clientreferenties tussen de dubbele aanhalingstekens om de client-id in te stellen op uw referentie.

1. Ga terug naar de codevoorvertoning van de ontwikkelaarssandbox.

1. Kopieer de tweede regel met het Adobe-script:

   ```
   <script src=https://documentccloud.adobe.com/view-sdk/main.js></script>
   ```

   ![Schermafbeelding van script](assets/ControlPDF_12.png)

1. Ga naar uw code-editor en open de **[!UICONTROL Web]** -> **[!UICONTROL oefening]** -> **[!UICONTROL index.html]** bestand.

1. Plak de scriptcode in de `<head>` van het bestand op regel 18 onder de opmerking waarin staat: **TAAK: UITOEFENING 1: INVOEGEN, API-SCRIPTTAG**.

   ![Screenshot van waar scriptcode moet worden geplakt](assets/ControlPDF_13.png)

1. Ga terug naar de codevoorvertoning van de ontwikkelaarssandbox en kopieer de eerste coderegel die het volgende bevat:

   ```
   <div id="adobe-dc-view"></div>
   ```

   ![Screenshot van waar code moet worden gekopieerd](assets/ControlPDF_14.png)

1. Ga naar uw code-editor en open de **[!UICONTROL Web]** -> **[!UICONTROL oefening]** -> **[!UICONTROL index.html]** bestand opnieuw.

1. Plak de `<div>` code in de `<body>` van het bestand op regel 67 onder de opmerking die **TAAK: UITOEFENING 1: API-CODE VOOR PDF INSLUITEN INVOEGEN**.

   ![Screenshot van waar code moet worden geplakt](assets/ControlPDF_15.png)

1. Ga terug naar de codevoorvertoning van de ontwikkelaarssandbox en kopieer de coderegels voor de `<script>` hieronder:

   ```
   <script type="text/javascript">
       document.addEventListener("adobe_dc_view_sdk.ready",             function(){ 
           var adobeDCView = new AdobeDC.View({clientId:                     "<YOUR_CLIENT_ID>", divId: "adobe-dc-view"});
           adobeDCView.previewFile({
               content:{location: {url: "https://documentcloud.                adobe.com/view-sdk-demo/PDFs/Bodea Brochure.                    pdf"}},
               metaData:{fileName: "Bodea Brochure.pdf"}
           }, {showDownloadPDF: false});
       });
   </script>
   ```

1. Ga naar uw code-editor en open de **[!UICONTROL Web]** -> **[!UICONTROL oefening]** -> **[!UICONTROL index.html]** bestand opnieuw.

1. Plak de `<script>` code in de `<body>` van het bestand op regel 68 onder de `<div>` tag.

1. Regel 70 van hetzelfde wijzigen **index.html** bestand om de clientID-variabele op te nemen die eerder is gemaakt.

   ![Screenshot van regel 70](assets/ControlPDF_16.png)

1. Regel 72 van hetzelfde wijzigen **index.html** bestand om de locatie van het PDF-bestand bij te werken zodat een lokaal bestand wordt gebruikt.

   Er is één beschikbaar in de zelfstudiebestanden in **/resources/pdfs/whitepaper.pdf**.

1. Sla de gewijzigde bestanden op en bekijk een voorvertoning van uw website door naar **`<your domain>`/Summit21/web/oefening/**.

   U moet de technische whitepaper weergeven in de modus Volledig venster van uw browser.

## Deel 3: Toegang tot API&#39;s van Analytics {#part3}

Nu u een webpagina hebt gemaakt met PDF Embed API-rendering voor een PDF, kunt u in deel 3 nu onderzoeken hoe u JavaScript-gebeurtenissen kunt gebruiken om analyses te meten om te begrijpen hoe gebruikers PDF gebruiken.

### Documentatie zoeken

Er zijn veel verschillende JavaScript-gebeurtenissen beschikbaar als onderdeel van de PDF Embed-API. U hebt vanuit [!DNL Adobe Acrobat Services] documentatie.

1. Navigeer naar de [documentatie](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html) site.
1. Bekijk de verschillende gebeurtenistypen die beschikbaar zijn als onderdeel van de API. Deze zijn handig als referentie en zijn ook handig voor toekomstige projecten.

   ![Screenshot van referentiegids](assets/ControlPDF_17.png)

1. Kopieer de voorbeeldcode die op de website staat.

   Gebruik dit als basis voor onze code en wijzig deze.

   ![Screenshot van voorbeeldcode om te kopiëren](assets/ControlPDF_18.png)

   ```
   const eventOptions = {
     //Pass the PDF analytics events to receive.
      //If no event is passed in listenOn, then all PDF         analytics events will be received.
   listenOn: [ AdobeDC.View.Enum.PDFAnalyticsEvents.    PAGE_VIEW, AdobeDC.View.Enum.PDFAnalyticsEvents.DOCUMENT_DOWNLOAD],
     enablePDFAnalytics: true
   }
   
   
   adobeDCView.registerCallback(
     AdobeDC.View.Enum.CallbackType.EVENT_LISTENER,
     function(event) {
       console.log("Type " + event.type);
       console.log("Data " + event.data);
     }, eventOptions
   );
   ```

1. Zoek de codesectie die u eerder hebt toegevoegd, die eruitziet als de onderstaande code en voeg de bovenstaande code na deze code toe in **index.html**:

   ![Screenshot van waar code moet worden geplakt](assets/ControlPDF_19.png)

1. Laad de pagina in uw webbrowser en open de Console om de consoleuitvoer van de verschillende gebeurtenissen te bekijken terwijl u met de PDF viewer werkt.

   ![Screenshot van het laden van de pagina](assets/ControlPDF_20.png)

   ![Screenshot van de code voor het laden van de pagina](assets/ControlPDF_21.png)

### Schakel toevoegen voor het vastleggen van gebeurtenissen

Nu u de gebeurtenissen hebt die aan console.log worden uitgevoerd, laten we het gedrag veranderen op basis van welke gebeurtenissen. Om dit te doen, zult u een schakelaarvoorbeeld gebruiken.

1. Navigeren naar **snippets/eventsSwitch.js** en kopieert u de inhoud van het bestand in de zelfstudiecode.

   ![Screenshot van waar code moet worden gekopieerd](assets/ControlPDF_22.png)

1. Plak de code in de gebeurtenislistenerfunctie.

   ![Screenshot van waar code moet worden geplakt](assets/ControlPDF_23.png)

1. Bevestig dat de consoleoutput correct wanneer de pagina wordt geladen en u met de Kijker van de PDF werkt.

### Adobe Analytics

Als u Adobe Analytics-ondersteuning wilt toevoegen aan uw viewer, kunt u de instructies op de website volgen.

>[!IMPORTANT]
>
>Adobe Analytics moet al op de pagina in de koptekst zijn geladen.

Navigeer naar de [Adobe Analytics-documentatie](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/howtodata.html#adobe-analytics) en bekijk of Adobe Analytics al op uw webpagina is ingeschakeld. Volg de instructies om een rapportsuite in te stellen.

### Google Analytics

![Screenshot van hoe je integreert met Google Analytics](assets/ControlPDF_24.png)

Adobe PDF Embed API biedt kant-en-klare integratie met Adobe Analytics. Omdat alle gebeurtenissen echter beschikbaar zijn als JavaScript-gebeurtenissen, is het mogelijk om met Google Analytics te integreren door PDF-gebeurtenissen vast te leggen en de functie ga() te gebruiken om de gebeurtenis toe te voegen aan Adobe Analytics.

1. Navigeren naar **snippets/eventsSwitchGA.js** om te zien hoe je kunt integreren met Google Analytics.
1. Bekijk en gebruik deze code als voorbeeld als uw webpagina wordt bijgehouden met Adobe Analytics en al is ingesloten op de webpagina.

   ![Screenshot van Adobe Analytics-code](assets/ControlPDF_25.png)

## Deel 4: Interactiviteit toevoegen op basis van gebeurtenissen {#part4}

In deel 4 gaat u door hoe u boven op uw PDF-viewer een paywall kunt toevoegen die wordt weergegeven nadat u voorbij de tweede pagina hebt geschoven.

### Paywall-voorbeeld

Hiernaar navigeren [voorbeeld van een PDF achter een paywall](https://www3.technologyevaluation.com/research/white-paper/the-forrester-wave-digital-decisioning-platforms-q4-2020.html). In dit voorbeeld leert u interactiviteit toevoegen boven op een PDF-kijkervaring.

### paywall-code toevoegen

1. Ga naar snippets/paywallCode.html en kopieer de inhoud.
1. Zoeken naar `<!-- TODO: EXERCISE 3: INSERT PAYWALL CODE -->` in exercise/index.html.

   ![Screenshot van waar code moet worden gekopieerd](assets/ControlPDF_26.png)

1. Plak de gekopieerde code na de opmerking.
1. Ga naar **snippets/paywallCode.js** en kopieert u de inhoud.

   ![Screenshot van waar code moet worden geplakt](assets/ControlPDF_27.png)

1. Plak de code op die locatie.

### Demo uitproberen met Paywall

Nu kunt u de demo bekijken.

1. Opnieuw laden **index.html** op uw website.
1. Schuif omlaag naar een pagina > 2.
1. Het dialoogvenster weergeven verschijnt om de gebruiker na de tweede pagina uit te dagen.

   ![Screenshot van het bekijken van de demo](assets/ControlPDF_28.png)

## Extra bronnen

Aanvullende bronnen vindt u [hier](https://www.adobe.io/apis/documentcloud/dcsdk/docs.html).
