---
title: Maak binnen een paar minuten een PDF van HTML of MS Office met de PDF Services-API en Node.js
description: Binnen de PDF Services API zijn er verschillende beschikbare services om PDF te maken en te manipuleren, of om te exporteren van PDF naar MS Office en andere indelingen
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6673
thumbnail: KT-6673.jpg
exl-id: 1bd01bb8-ca5e-4a4a-8646-3d97113e2c51
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '673'
ht-degree: 0%

---

# Maak binnen een paar minuten een PDF van HTML of MS Office met de PDF Services-API en Node.js

![ creeer PDF hoofdbeeld ](assets/createpdffromhtml_hero.jpg)

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

De eerste stap is het verkrijgen van een referentie (API-sleutel) om het gebruik te ontgrendelen. [ Teken omhoog voor de vrije proef hier ](https://www.adobe.com/go/dcsdks_credentials) en klik op &quot;krijg Begonnen&quot;om uw nieuwe geloofsbrieven tot stand te brengen.

![ API Sleutel ](assets/apikey.png)

Het is belangrijk om een &#39;Persoonlijk account&#39; te kiezen om u aan te melden voor de gratis proefversie:

![ Persoonlijk Rekening ](assets/personalaccount.png)

In de volgende stap kiest u de PDF Services API Service en voegt u vervolgens een naam en een beschrijving toe voor uw referenties.

Er is een selectievakje voor &#39;Een gepersonaliseerd codevoorbeeld maken&#39;. Kies deze optie als u uw nieuwe referenties automatisch wilt laten toevoegen aan uw voorbeeldbestanden en de handmatige stap wilt overslaan.

Kies vervolgens Node.js als uw taal om de specifieke voorbeelden Node.js te ontvangen en klik op de knop &#39;Referenties maken&#39;.

![ creeer geloofsbrieven ](assets/createcredentials.png)

U ontvangt een zip-bestand met de naam PDFToolsSDK-Node.jsSamples.zip dat u kunt downloaden naar uw lokale bestandssysteem.

## Uw referenties toevoegen aan de codevoorbeelden

Als u de optie &#39;Voorbeeld van gepersonaliseerde code maken&#39; kiest, hoeft u niet handmatig uw client-id toe te voegen aan de voorbeeldbestanden van de code. U kunt de volgende stap overslaan en rechtstreeks naar de onderstaande sectie Voorbeelden van code uitvoeren gaan.

Als u de optie &#39;Een gepersonaliseerd codevoorbeeld maken&#39; niet hebt gekozen, moet u de client-id (API-sleutel) uit de Adobe.io-console kopiëren:

![ Voorbeeld van de Code ](assets/codesample.png)

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

![ Uitvoer van het Voorbeeld ](assets/exampleoutput.png)

Uw PDF wordt gemaakt op de locatie die in de uitvoer is aangegeven. Dit is standaard de map pdfServicesSdkResult.

## Bronnen en volgende stappen

* Voor extra hulp en steun, bezoek het Adobe [[!DNL Acrobat Services]  APIs ](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) communityforum

PDF Services API [ Documentatie ](https://www.adobe.com/go/pdftoolsapi_doc)

* [ Veelgestelde vragen ](https://community.adobe.com/t5/contentarchivals/contentarchivedpage/message-uid/10726197) voor de vragen van de Diensten API van PDF

* [ Contact ons ](https://www.adobe.com/go/pdftoolsapi_requestform) voor vragen over vergunning en tarifering

* Verwante artikelen:
  [ de Nieuwe Diensten API van de PDF biedt nog meer eigenschappen voor documentworkflows ](https://community.adobe.com/t5/acrobat-services-api-discussions/new-pdf-tools-api-brings-more-capabilities-for-document-services/m-p/11294170) aan

  [ Versie van juli van  [!DNL Adobe Acrobat Services]: PDF bedde en de Diensten van de PDF ](https://medium.com/adobetech/july-release-of-adobe-document-services-pdf-embed-and-pdf-tools-17211bf7776d) in
