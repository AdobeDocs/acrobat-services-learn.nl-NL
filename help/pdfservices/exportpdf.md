---
title: PDF Services-API gebruiken om PDF naar Word, PowerPoint en meer te exporteren
description: Leer hoe u de PDF Services API-exportbewerking uitvoert met behulp van voorbeeldbestanden voor Node.js-, Java- en .Net-talen
type: Tutorial
role: Developer
level: Intermediate
thumbnail: KT-6674.jpg
kt: 6674
exl-id: 55f5b04e-0249-47d9-9131-2f9ec01db7e8
source-git-commit: 2d1151c17dfcfa67aca05411976f4ef17adf421b
workflow-type: tm+mt
source-wordcount: '500'
ht-degree: 5%

---

# PDF Services-API gebruiken om PDF naar Word, PowerPoint en meer te exporteren

![PDF-hoofdafbeelding maken](assets/ExportPDF_hero.jpg)

Adobe PDF Services API converteert PDF-bestanden naar MS Office, tekst en afbeeldingen met behulp van API&#39;s. Er zijn veel veelvoorkomende gebruiksscenario&#39;s om bestaande PDF te ontgrendelen voor contentbewerking en -analyse en met PDF Services API-ontwikkelaars kunnen deze functionaliteit gemakkelijk worden geïntegreerd in bestaande systemen en toepassingen. Converteer PDF-bestanden naar MS Word voor het bewerken van inhoud, goedkeuringen en het later verzenden van handtekeningen om aangepaste contractworkflows te maken. Of exporteer PDF-content naar MS Excel-indeling voor facturen en financiële berekeningen of gegevensanalyse.

De bewerking Exporteren ondersteunt de volgende PDF-bestandsconversies:

* PDF naar Microsoft Word (DOC, DOCX)
* PDF naar Microsoft PowerPoint (PPTX)
* PDF naar Microsoft Excel (XLSX)
* PDF naar tekst (RTF)
* PDF naar afbeelding (JPEG, PNG)

In deze zelfstudie leert u de basisvaardigheden voor het uitvoeren van uw eerste API-exportbewerking voor PDF Services met behulp van voorbeeldbestanden voor Node.js, Java en .Net-talen.

## Stap 1: Maak uw referenties en stel uw omgeving in:

Gebruik de onderstaande Aan de slag-zelfstudies om uw API-referenties te maken, voorbeeldbestanden te downloaden en uw omgeving in te stellen.

[Aan de slag met PDF Services API en Java](gettingstartedjava.md)

[Aan de slag met PDF Services API en .Net](gettingstartednet.md)

[Aan de slag met de PDF Services-API en Node.js](createpdffromhtml.md)

## Stap 2: Export-pdf-bewerking uitvoeren met behulp van voorbeeldbestanden

**Java**

1. Open een opdrachtprompt.

1. Wijzig mappen in uw map met voorbeeldcodes.

   Bijvoorbeeld: C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-java-samples

1. Voer de volgende opdracht uit:

   `mvn -f pom.xml exec:java -Dexec.mainClass=com.adobe.platform.operation.samples.exportpdf.ExportPDFToDOCX`

Uw PDF wordt gemaakt in de map src/main/resources.

**.Net**

1. Open een opdrachtprompt.

1. Wijzig mappen in uw map met voorbeeldcodes.

   Bijvoorbeeld: C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-NetSamples

1. Wijzig mappen opnieuw in de map ExportPDFtoDocx.

1. Voer de volgende opdracht uit:

   `dotnet run ExportPDFToDocx.csproj`

Uw PDF wordt in dezelfde map gemaakt.

**Node.js**

1. Open een opdrachtprompt.

1. Wijzig mappen in uw map met voorbeeldcodes.

   Bijvoorbeeld: C:\Temp\PDFToolsAPI\adobe-dc-pdf-tools-sdk-node-samples

1. Voer de volgende opdracht uit:

   `node src/ocr/ocr-pdf.js`

Uw PDF wordt gemaakt op de locatie die in de uitvoer is aangegeven. Dit is standaard de map pdfServicesSdkResult.

## Definitieve gedachten

U hebt nu een werkvoorbeeld dat u in uw bestaande toepassingen kunt importeren om een concepttest te starten. In elke voorbeelddirectory ziet u nog een voorbeeld om PDF-bestanden te exporteren naar de afbeeldingsindeling. Met dezelfde stappen hierboven kunt u dat voorbeeld ook uitvoeren. Als u een andere indeling wilt gebruiken, kunt u de code bijwerken naar de gewenste nieuwe indeling:

SupportedTargetFormats.PPTX

En het doelresultaat:

output/exportPdfOutput.PPTX

Naar een andere indeling.

## Bronnen en volgende stappen

* Ga voor meer hulp en ondersteuning naar de [[!DNL Adobe Acrobat Services] API&#39;s](https://community.adobe.com/t5/document-cloud-sdk/bd-p/Document-Cloud-SDK?page=1&amp;sort=latest_replies&amp;filter=all) communautair forum

* PDF Services API [Documentatie](https://www.adobe.com/go/pdftoolsapi_doc)

* [Veelgestelde vragen](https://community.adobe.com/t5/document-cloud-sdk/faq-for-document-services-pdf-tools-api/m-p/10726197) voor PDF Services API-vragen

* [Contact opnemen](https://www.adobe.com/go/pdftoolsapi_requestform) voor vragen over licenties en prijzen
