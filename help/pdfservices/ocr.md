---
title: Adobe PDF Services-API gebruiken voor OCR-PDF-bestanden
description: Met OCR (optische tekenherkenning) kun je gescande PDF ontgrendelen om tekst te extraheren en doorzoekbare bestanden te maken
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-6677
thumbnail: KT-6677.jpg
exl-id: 61a9a2d1-94c3-41c2-8f90-a56a938ef245
source-git-commit: 5222e1626f4e79c02298e81d621216469753ca72
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 4%

---

# Adobe PDF Services API gebruiken voor OCR-PDF-bestanden

![PDF-hoofdafbeelding maken](assets/OCR_hero.jpg)

Met OCR (Optical Character Recognition) kun je gescande PDF ontgrendelen om tekst te extraheren en doorzoekbare bestanden te maken. Met onze krachtige cloudgebaseerde API’s integreer je OCR in elke documentworkflow voor de perfecte oplossing voor het archiveren, kopiëren van tekst en het creëren van doorzoekbare documentindexen. Maak doorzoekbare archieven vanuit gescande opslagplaatsen voor PDF om belangrijke informatie te ontgrendelen en tijd te besparen dankzij snelle doorzoekbaarheid. Of pas OCR van geüploade scans toe op uw PDF om deze te kunnen bewerken voor gebruik in onboardingworkflows.

Ontwikkelaars kunnen binnen een paar minuten aan de slag met de voorbeeldbestanden die zijn meegeleverd voor OCR.

In deze zelfstudie wordt uitgelegd hoe u uw eerste OCR-bewerking van de PDF Services-API uitvoert met behulp van voorbeeldbestanden voor Node.js, Java en .Net-talen.

## Stap 1: Maak uw referenties en stel uw omgeving in

Gebruik de onderstaande Aan de slag-zelfstudies om uw API-referenties te maken, voorbeeldbestanden te downloaden en uw omgeving in te stellen.

[Aan de slag met PDF Services API en Java](gettingstartedjava.md)

[Aan de slag met PDF Services API en .Net](gettingstartednet.md)

[Aan de slag met PDF Services API en Node.js](createpdffromhtml.md)

## Het OCR-voorbeeld in de voorbeeldbestanden uitvoeren

Bij onze OCR-bewerking is standaard de Engelse landinstelling mogelijk, maar ook ondersteuning voor Duits, Frans, Deens en [overige talen](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#ocr-with-explicit-language). De standaardwaarde is &#39;en-us&#39;.

Wanneer u opties doorgeeft met OCR-bewerking, inclusief een specifieke landinstelling, accepteert de methode ook de parameter &#39;type&#39; met twee opties:

* SEARCHABLE_IMAGE: Hiermee wijzigt u de oorspronkelijke afbeelding tijdens het opschoonproces (bijvoorbeeld heft u de schuintrekking op) voordat u er een onzichtbare tekstlaag op plaatst. Dit type verwijdert ongewenste artefacten en kan in sommige gevallen leiden tot een beter leesbaar document.

* SEARCHABLE_IMAGE_EXACT: Hiermee zorgt u ervoor dat de tekst doorzoekbaar en selecteerbaar is. Met deze optie behoudt u de oorspronkelijke afbeelding en plaatst u er een onzichtbare tekstlaag overheen. Aanbevolen voor gevallen waarin een maximale getrouwheid van de oorspronkelijke afbeelding is vereist.

**Java**

1. Open een opdrachtprompt.

1. Wijzig mappen in uw map met voorbeeldcodes.

   Bijvoorbeeld C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-java-samples>.

1. Voer de volgende opdracht uit:

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.ocrpdf.OcrPDF`

Uw PDF wordt gemaakt in de map src/main/resources.

**.Net**

1. Open een opdrachtprompt.

1. Wijzig mappen in uw map met voorbeeldcodes.

   Bijvoorbeeld C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. Wijzig mappen opnieuw in de map OcrPDF.

1. Voer de volgende opdracht uit:

   `dotnet run OcrPDF.csproj`

Uw PDF wordt in dezelfde map gemaakt.

**Node.js**

1. Open een opdrachtprompt.

1. Wijzig mappen in uw map met voorbeeldcodes.

   Bijvoorbeeld C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. Voer de volgende opdracht uit:

   `node src/ocr/ocr-pdf.js`

De PDF wordt gemaakt op de locatie die is aangegeven in de uitvoer. Standaard is dit de uitvoermap.

## Definitieve gedachten

Met deze eenvoudige stappen in de voorbeeldbestanden hebt u een werkvoorbeeld waarop u kunt bouwen. Naast het OCR-voorbeeld dat we in deze zelfstudie hebben gebruikt, is er nog een voorbeeld voor OCR met behulp van het ondersteunde type- en landinstellingsopties die eerder zijn besproken.

Vanaf hier kunt u uw invoer- en uitvoerbestanden in het voorbeeld gewoon vervangen om uw eigen PDF te gebruiken om uw concepttest af te ronden voor uw eigen gebruik.

![Conceptproef](assets/OCR_poc.png)

## Bronnen en volgende stappen

* Ga voor meer hulp en ondersteuning naar de Adobe [[!DNL Acrobat Services] API&#39;s](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) communautair forum

* PDF Services API [Documentatie](https://www.adobe.com/go/pdftoolsapi_doc)

* [Veelgestelde vragen](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) voor PDF Services API-vragen

* [Contact opnemen](https://www.adobe.com/go/pdftoolsapi_requestform) voor vragen over licenties en prijzen
