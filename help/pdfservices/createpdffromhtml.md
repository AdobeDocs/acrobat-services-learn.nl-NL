---
title: Maak binnen een paar minuten een PDF van HTML of MS Office met de PDF Services-API en Node.js
description: Binnen de PDF Services API zijn er verschillende beschikbare services om PDF te maken en te manipuleren, of om te exporteren van PDF naar MS Office en andere indelingen
type: Tutorial
role: Developer
level: Beginner
thumbnail: KT-6673.jpg
jira: KT-6673
exl-id: 1bd01bb8-ca5e-4a4a-8646-3d97113e2c51
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '711'
ht-degree: 0%

---

# Maak binnen een paar minuten een PDF van HTML of MS Office met de PDF Services-API en Node.js

![PDF-hoofdafbeelding maken](assets/createpdffromhtml_hero.jpg)

Het digitaliseren van documentworkflows is nog nooit zo eenvoudig geweest met de nieuwe Adobe PDF Services API, die ontwikkelaars een vrij bereik biedt om te kiezen tussen verschillende krachtige PDF-manipulatieservices om te voldoen aan de behoeften van gecompliceerde bedrijfsworkflows. Gecompliceerde architecturen, implementatiestrategieën en technologieuitbreiding kunnen worden gestroomlijnd met deze direct beschikbare cloudgebaseerde webservices.

Binnen de PDF Services API zijn er verschillende beschikbare services om PDF te maken en te manipuleren, of om te exporteren van PDF naar MS Office en andere indelingen.

* Een PDF-bestand maken van statische of dynamische HTML, MS Word, PowerPoint, Excel en meer
* Export PDF naar MS Word, PowerPoint, Excel en meer
* OCR om tekst in PDF-bestanden te herkennen en het doorzoeken van documenten in te schakelen
* Protect PDF met een wachtwoord bij het openen van documenten
* PDF-pagina&#39;s of PDF-documenten combineren tot één PDF
* PDF comprimeren om de grootte voor delen via e-mail of online te beperken
* Lineariseren om een PDF te optimaliseren voor snelle weergave op het web
* PDF-pagina&#39;s ordenen met services voor invoegen, vervangen, herschikken, verwijderen en roteren

Ontwikkelaars kunnen binnen een paar minuten aan de slag met de gebruiksklare voorbeeldbestanden voor toegang tot alle beschikbare webservices. Zo begin je.

## Referenties verkrijgen en voorbeeldbestanden downloaden

De eerste stap is het verkrijgen van een referentie (API-sleutel) om het gebruik te ontgrendelen. [Meld je hier aan voor de gratis proefversie](https://www.adobe.com/go/dcsdks_credentials) en klik op Aan de slag om uw nieuwe referenties te maken.

![API-sleutel](assets/apikey.png)

Het is belangrijk om een &#39;Persoonlijk account&#39; te kiezen om u aan te melden voor de gratis proefversie:

![Persoonlijk account](assets/personalaccount.png)

In de volgende stap kiest u de PDF Services API Service en voegt u vervolgens een naam en een beschrijving toe voor uw referenties.

Er is een selectievakje voor &#39;Een gepersonaliseerd codevoorbeeld maken&#39;. Kies deze optie als u uw nieuwe referenties automatisch wilt laten toevoegen aan uw voorbeeldbestanden en de handmatige stap wilt overslaan.

Kies vervolgens Node.js als uw taal om de specifieke voorbeelden Node.js te ontvangen en klik op de knop &#39;Referenties maken&#39;.

![Referenties maken](assets/createcredentials.png)

U ontvangt een zip-bestand met de naam PDFToolsSDK-Node.jsSamples.zip dat u kunt downloaden naar uw lokale bestandssysteem.

## Uw referenties toevoegen aan de codevoorbeelden

Als u de optie &#39;Voorbeeld van gepersonaliseerde code maken&#39; kiest, hoeft u niet handmatig uw client-id toe te voegen aan de voorbeeldbestanden van de code. U kunt de volgende stap overslaan en rechtstreeks naar de onderstaande sectie Voorbeelden van code uitvoeren gaan.

Als u de optie &#39;Een gepersonaliseerd codevoorbeeld maken&#39; niet hebt gekozen, moet u de client-id (API-sleutel) uit de Adobe.io-console kopiëren:

![Codevoorbeeld](assets/codesample.png)

Pak de inhoud van PDFToolsSDK-Node.jsSamples.zip uit.

Ga naar de hoofdmap in de map adobe-dc-pdf-tools-sdk-node-samples.

Open pdftools-api-credentials.json met een willekeurige teksteditor of IDE.

Plak de referentie in het veld voor de client-id in de code:

```javascript
{
 "client_credentials": {
  "client_id": "abcdefghijklmnopqrstuvwxyz",
```

Sla het bestand op en ga door met de volgende stap om de codevoorbeelden uit te voeren.

## Het eerste codevoorbeeld uitvoeren

Gebruik de opdrachtprompt om naar de hoofdmap te gaan onder de map adobe-dc-pdf-tools-sdk-node-samples.

Type npm installeren:

C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples>npm installeren

U kunt nu de voorbeeldbestanden uitvoeren!

Voor uw eerste voorbeeld maakt u een PDF:

Zorg dat de opdrachtprompt blijft bestaan en voer het voorbeeld PDF maken met de volgende opdracht uit:

C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples>src/createpdf/create-pdf-from-docx.js

Voorbeeld van uitvoer:

![Voorbeelduitvoer](assets/exampleoutput.png)

Uw PDF wordt gemaakt op de locatie die in de uitvoer is aangegeven. Dit is standaard de map pdfServicesSdkResult.

## Bronnen en volgende stappen

* Ga voor meer hulp en ondersteuning naar de Adobe [[!DNL Acrobat Services] API&#39;s](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) communautair forum

PDF Services API [Documentatie](https://www.adobe.com/go/pdftoolsapi_doc)

* [Veelgestelde vragen](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) voor PDF Services API-vragen

* [Contact opnemen](https://www.adobe.com/go/pdftoolsapi_requestform) voor vragen over licenties en prijzen

* Verwante artikelen:
  [De nieuwe PDF Services API biedt nog meer functies voor documentworkflows](https://community.adobe.com/t5/document-services-apis/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170)

  [Release van juli [!DNL Adobe Acrobat Services]: PDF Embed and PDF Services](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d)
