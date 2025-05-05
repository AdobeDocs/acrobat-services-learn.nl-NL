---
title: Maak je eerste workflow in Microsoft Power Automate
description: Leer hoe u de Adobe PDF Services-connector kunt gebruiken in Microsoft Power Automate
feature: PDF Services API
role: Developer
level: Beginner
type: Tutorial
jira: KT-10379
thumbnail: KT-10379.jpg
exl-id: 095b705f-c380-42cc-9329-44ef7de655ee
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1955'
ht-degree: 1%

---


# Maak je eerste flow in Microsoft Power Automate

Leer hoe te om uw eerste stroom in [ Macht van Microsoft te creëren automatiseer ](https://flow.microsoft.com) gebruikend de [ 3&rbrace; schakelaar van de Diensten van Adobe PDF &lbrace;.](https://us.flow.microsoft.com/en-us/connectors/shared_adobepdftools/adobe-pdf-services/)

In deze praktische zelfstudie leert u hoe u:

* Word-documenten converteren naar PDF
* PDF-documenten combineren tot één PDF
* Protect een PDF-document met een wachtwoord

## Voorbereiding

### Wat je nodig hebt

* **Proefings of productiegeloofsbrieven voor de Diensten van Adobe PDF**
Leer meer over hoe te om geloofsbrieven in de Macht van Microsoft te krijgen en te vormen automatiseer [ hier ](https://experienceleague.adobe.com/nl/docs/acrobat-services-learn/tutorials/pdfservices/getting-credentials-power-automate).
* **de Macht van Microsoft automatiseert met de schakelaars van de Premium**
Leer hoe te om het niveau van vergunningen voor Macht te controleren automatiseer [ hier ](https://docs.microsoft.com/en-us/power-platform/admin/power-automate-licensing/types).
* **OneDrive**
In deze zelfstudie wordt de OneDrive-opslagconnector gebruikt, maar elke opslagconnector kan worden vervangen.

### Voorbeeldbestanden

Er zijn twee [ steekproefdossiers ](assets/sample-assets.zip) die u aan unzip en aan OneDrive moet uploaden:

* WordDocument01.docx
* WordDocument02.docx

### Inloggegevens ophalen

Om deze zelfstudie te voltooien, hebt u uw aanmeldingsgegevens nodig die al zijn geconfigureerd in Microsoft Power Automate for Adobe PDF Services. Als u deze stap niet hebt voltooid, zie hier de [ instructies ](https://experienceleague.adobe.com/nl/docs/acrobat-services-learn/tutorials/pdfservices/getting-credentials-power-automate).

## Deel 1: Nieuwe flow maken en Word converteren naar PDF

### De flow maken

In dit deel, creeert u een nieuwe stroom in [ Microsoft Macht ](https://flow.microsoft.com) gebruikend een onmiddellijke stroom automatiseren, parameters toevoegen, uw dossiers krijgen van OneDrive, en hen omzetten in PDF.

1. Navigeer aan [ de Macht van Microsoft automatisch ](https://flow.microsoft.com) en login met uw geloofsbrieven.
1. In sidebar, uitgezochte **[!UICONTROL creeer]**.

   ![ creeer knoop ](assets/createButtonPowerAutomate.png)

1. Selecteer **[!UICONTROL Onmiddellijke Stroom]**.
1. Geef uw flow een naam.
1. Onder *verkies hoe te om deze stroom* te teweegbrengen, selecteer **[!UICONTROL manueel een stroom]** teweegbrengen.
1. Selecteer **[!UICONTROL Maken]**.

### Bestandsinhoud ophalen van bestanden

Vervolgens haalt u de bestandsinhoud op van de voorbeeldbestanden.

>[!PREREQUISITES]
>
>Als u niet de [ steekproefdossiers ](assets/sample-assets.zip) in OneDrive hebt geüpload, decomprimeer hen en upload hen.


1. In [ Macht automatiseer ](https://flow.microsoft.com), uitgezochte **[!UICONTROL + Nieuwe stap]**.
1. Onderzoek naar *OneDrive* in de onderzoeksbar.
1. Kies of uw werk of persoonlijke rekening OneDrive door **[!UICONTROL OneDrive voor Zaken]** te selecteren of **[!UICONTROL OneDrive]**.
1. Zoek *krijgt dossierinhoud* in de onderzoeksbar.
1. Op het **[!UICONTROL gebied van het Dossier]**, selecteer het pictogram van de Omslag om aan het *WordDocument01.docx* dossier in OneDrive te navigeren.

   ![ krijg dossierinhoud actie OneDrive in Microsoft Power Automate ](assets/getFileContentOneDrive.png)

### Bestand converteren naar PDF

Nu u de bestandsinhoud hebt, kunt u het document converteren naar PDF.

1. In [ Macht automatiseer ](https://flow.microsoft.com), uitgezochte **[!UICONTROL + Nieuwe stap]**.
1. Onderzoek naar *Diensten van Adobe PDF* in de onderzoeksbar.
1. Selecteer **[!UICONTROL de Diensten van Adobe PDF]**.
1. Onderzoek naar *zet Word in PDF* in de onderzoeksbar om.
1. In **[!UICONTROL Naam van het Dossier]**, noem uw dossier zoals gewenst maar het moet met *.docx* beëindigen. Deze extensie is nodig voor het converteren van documenten van Word naar PDF.
1. Plaats uw curseur op het **[!UICONTROL gebied van de Inhoud van het Dossier]**.
1. Gebruikend het **[!UICONTROL Dynamische inhoud]** paneel, selecteer **[!UICONTROL inhoud van het Dossier]**.

   ![ Converteer Word in de Actie van PDF in de Macht van Microsoft automatiseren ](assets/convertWordToPDFActionPowerAutomate.png)

### Sla het bestand op in OneDrive

Nadat het document is gegenereerd, slaat u het bestand weer op in OneDrive.

1. In [ Microsoft Macht automate ](https://flow.microsoft.com), uitgezochte **[!UICONTROL + Nieuwe stap]**.
1. Onderzoek naar *OneDrive* in de onderzoeksbar.
1. Kies of uw werk of persoonlijke rekening OneDrive door **[!UICONTROL OneDrive voor Zaken]** te selecteren of **[!UICONTROL OneDrive]**.
1. Zoek *krijgt dossierinhoud* in de onderzoeksbar.
1. Zoek naar *creeer dossier* in de onderzoeksbar.
1. Selecteer **[!UICONTROL creeer dossier]**.
1. In het **gebied van de Weg van de Omslag 0&rbrace;, selecteer het omslagpictogram om te specificeren waar te om het dossier in OneDrive op te slaan.**
1. In **[!UICONTROL Naam van het Dossier]**, noem uw dossier zoals gewenst maar het moet met *.docx* beëindigen. Deze extensie is nodig voor het converteren van documenten van Word naar PDF.
1. Op het **gebied van de Inhoud van het Dossier**, gebruik **[!UICONTROL Dynamische inhoud]** paneel om de de inhoudsvariabele van het Dossier van de PDF op te nemen.

### Stroom uitproberen

1. In top-left, selecteer **[!UICONTROL Naamloos]** om de stroom anders te noemen.
1. Selecteer **[!UICONTROL Opslaan]**.
1. Selecteer **[!UICONTROL Test]**.
1. Selecteer **[!UICONTROL manueel]** en dan **[!UICONTROL sparen &amp; Test]**.
1. Selecteer **[!UICONTROL Doorgaan]**.
1. Selecteer **[!UICONTROL Stroom van de Looppas]**.

In de map OneDrive ziet u nu de omgezette PDF.

![ Geselecteerd omgezet document van PDF in OneDrive ](assets/selectedGeneratedFileInOneDrive.png)

## Deel 2: Een dynamisch document genereren op basis van een sjabloon

Dit volgende deel bouwt op Deel 1 voort en gebruikt *produceer document van het malplaatje van Word* om gegevens dynamisch samen te voegen in uw document.

### De documentsjabloon bekijken

Open *WordDocument02_.docx* van uw steekproefdossiers in OneDrive. Het Word-document bevat verschillende tekstcodes die plaatsen aangeven waar gegevens in het document worden ingevuld.

### Te activeren parameters toevoegen

Als u dynamische gegevens in het document wilt opnemen, moet u een paar parameters voor de trigger maken om naar waarden te vragen.

1. Wanneer het uitgeven van uw stroom, selecteer **[!UICONTROL manueel teweegbrengt een stroom]** om de actie uit te breiden.
1. Selecteer **[!UICONTROL voeg een input]** toe.
1. Selecteer **[!UICONTROL Tekst]**.
1. Noem het gebied *Voornaam*.

Herhaal stap 2-4 om de volgende velden toe te voegen:

* Achternaam
* Salary

![ Trigger in Macht automate met parametergebieden ](assets/triggerParametersInPowerAutomate.png)

### Bestandsinhoud van een sjabloon ophalen

Als u een document wilt genereren, moet u eerst de bestandsinhoud van de Word-sjabloon ophalen.

1. In Macht automatiseer, selecteer + **[!UICONTROL Nieuwe stap]**.
1. Onderzoek naar *OneDrive* in de onderzoeksbar.
1. Kies of uw werk of persoonlijke rekening OneDrive door **[!UICONTROL OneDrive voor Zaken]** te selecteren of **[!UICONTROL OneDrive]**.
1. Zoek *krijgt dossierinhoud* in de onderzoeksbar.
1. Op het **[!UICONTROL gebied van het Dossier]**, selecteer het pictogram van de Omslag om aan het *WordDocument02.docx* dossier in OneDrive te navigeren.

![ krijg de actie van de dossierinhoud van OneDrive in Microsoft Power Automate ](assets/getFileContentAction02.png)

### Document genereren op basis van sjabloon

1. Selecteer **[!UICONTROL + Nieuwe stap]** in Power Automate.
1. Onderzoek naar *Diensten van Adobe PDF* in de onderzoeksbar.
1. Selecteer **[!UICONTROL de Diensten van Adobe PDF]**.
1. Selecteer **[!UICONTROL produceer document van het malplaatje van Word]** actie.
1. Op het **gebied van de Naam van het Dossier van het Malplaatje**, noem uw dossier zoals gewenst maar het moet met *.docx* beëindigen.

#### Gegevens samenvoegen

Gebruikend *produceer document van het malplaatje van Word* actie, kunt u gegevens in uw document van om het even welke verschillende variabelen samenvoegen eerder in de stroom gebruikend Dynamische inhoud.

Kopieer de JSON gegevens hieronder in het **gebied van de Gegevens van de Fusie**:

```
{
    "FirstName": "",
    "LastName": "",
    "Salary": ""
}
```

1. Plaats uw curseur op het gebied tussen de twee aanhalingstekens voor de ** waarde FirstName.
1. Gebruikend het **[!UICONTROL Dynamische paneel van de Inhoud]**, neem de *Voornaam* waarde van manueel teweeg een stroomactie.

   ![ produceer document met gegevensmarkeringen in JSON ](assets/generateDocumentJSONAction.png)

1. Herhaal stappen 7-8 voor de **[!UICONTROL LastName]** en **[!UICONTROL Salary]** gebieden.
1. In het **gebied van de Inhoud van het Dossier van het Malplaatje**&#x200B;[!UICONTROL &#x200B; Dynamische inhoud &#x200B;]&#x200B;**paneel gebruiken om de**&#x200B;[!UICONTROL &#x200B; inhoud van het Dossier &#x200B;]&#x200B;**waarde van *op te nemen krijg dossierinhoud* stap.**

![ produceer document van het malplaatjeactie van Word in Macht automatiseert met alle voltooide waarden ](assets/generateDocumentJSONActionCompleted.png)

>[!TIP]
>
>De *produceer document van het malplaatje van Word* actie gebruikt de Generatie API van het Document van de Adobe. Hier volgen een paar bronnen voor meer informatie over het maken van sjablonen:
>
>* [ Leer meer over de Generatie van het Document van de Adobe ](https://developer.adobe.com/document-services/apis/doc-generation/)
>* [ Tagger van de Generatie van het Document van de Adobe voor Microsoft Word ](https://appsource.microsoft.com/en-US/product/office/WA200002654)
>* [ de Productie API van het Document van de Adobe Documentatie ](https://developer.adobe.com/document-services/docs/overview/document-generation-api/)

### Sla het bestand op in OneDrive

Nadat het document is gegenereerd, kunt u het bestand weer opslaan in OneDrive.

1. In Macht automatiseer, selecteer **+ [!UICONTROL &#x200B; Nieuwe stap]**.
1. Onderzoek naar *OneDrive* in de onderzoeksbar.
1. Kies of uw werk of persoonlijke rekening OneDrive door **[!UICONTROL OneDrive voor Zaken]** te selecteren of **[!UICONTROL OneDrive]**.
1. Zoek naar *creeer dossier* in de onderzoeksbar.
1. Selecteer **[!UICONTROL creeer dossier]**.
1. In het **gebied van de Weg van de Omslag 0&rbrace;, selecteer het omslagpictogram om te specificeren waar te om het dossier in OneDrive op te slaan.**
1. Op het **[!UICONTROL gebied van de Naam van het Dossier]**, plaats de naam van het dossier. Omdat de uitvoer een PDF is, moet uw bestandsnaam eindigen met de extensie .pdf.
1. Gebruik het **[!UICONTROL Dynamische inhoudspaneel]** om de variabele van de Inhoud van het Dossier van de PDF in het **[!UICONTROL 3&rbrace; gebied van de Inhoud van het Dossier op te nemen &lbrace;.]**

### Stroom uitproberen

![ de stroomscherm van de Looppas in de Macht van Microsoft automatisch het veroorzaken van input ](assets/runFlowParameters.png)

1. Selecteer **[!UICONTROL Opslaan]**.
1. Selecteer **[!UICONTROL Test]**.
1. Selecteer **[!UICONTROL manueel]** en dan **[!UICONTROL sparen &amp; Test]**.
1. Selecteer **[!UICONTROL Doorgaan]**.
1. Ga waarden voor *Voornaam* in, *Achternaam*, en *Salaris*.
1. Selecteer **[!UICONTROL Stroom van de Looppas]**.

In de map OneDrive ziet u nu een PDF die uit het Word-document is gegenereerd. Wanneer u het PDF-document opent in OneDrive, ziet u dat de gegevens worden samengevoegd met de tekstlabellocaties.


## Deel 3: PDF combineren tot één

Nu u een Word-document hebt gegenereerd en omgezet in een PDF, bestaat het volgende onderdeel uit het combineren van meerdere PDF-documenten.

>[!NOTE]
>
>In de vorige handelingen hebt u een kopie van het document als bestand opgeslagen in OneDrive. Als u gereedschappen wilt gebruiken zoals PDF samenvoegen, hoeft u het bestand niet op te slaan in OneDrive. In plaats daarvan kunt u de uitvoer rechtstreeks van de ene handeling naar de andere doorgeven. Dit is beter dan het na elke handeling opslaan in OneDrive. Maar voor demonstratiedoeleinden slaat u deze bestanden op in OneDrive.

### PDF-stap samenvoegen toevoegen

1. Als u uw flow bewerkt, selecteert u **[!UICONTROL + Volgende stap]** om een handeling toe te voegen aan het einde van de flow.
1. Onderzoek naar *Diensten van Adobe PDF* in de onderzoeksbar.
1. Selecteer **[!UICONTROL de Diensten van Adobe PDF]**.
1. Selecteer de **[!UICONTROL actie van de PDF van de Fusie]**.
1. In het **gebied van de Naam van het Dossier van de Samenvoeging**, ga uw gewenste dossiernaam (d.w.z. in, *combinedDocument.pdf*).
1. In het **[!UICONTROL Inhoud van het Dossier - 1]** gebied, gebruik het **[!UICONTROL Dynamische inhoudspaneel]** om de *waarde van de Inhoud van het Dossier van de PDF* van de **[!UICONTROL Converteer Word in PDF]** stap op te nemen.
1. Om het volgende document toe te voegen, voeg **+ [!UICONTROL &#x200B; nieuw punt]** toe.
1. In het **[!UICONTROL Inhoud van het Dossier - 2]** gebied, gebruik het **[!UICONTROL Dynamische inhoudspaneel]** om de **[!UICONTROL waarde van de Inhoud van het Dossier van de Output in te voegen]** van *produceer document van het malplaatje van Word* stap.

![ de actie van de PDF van de Fusie in de Macht van Microsoft automatiseren ](assets/mergePDFAction.png)

### Samengevoegde PDF opslaan naar OneDrive

Nadat het document is gecombineerd, kunt u het weer opslaan in OneDrive.

1. In Macht automatiseer, selecteer **+ [!UICONTROL &#x200B; Nieuwe stap]**.
1. Onderzoek naar *OneDrive* in de onderzoeksbar.
1. Kies of uw werk of persoonlijke rekening OneDrive door **[!UICONTROL OneDrive voor Zaken]** te selecteren of **[!UICONTROL OneDrive]**.
1. Zoek naar *creeer dossier* in de onderzoeksbar.
1. Selecteer **[!UICONTROL creeer dossier]**.
1. In het **gebied van de Weg van de Omslag 0&rbrace;, selecteer het omslagpictogram om te specificeren waar te om het dossier in OneDrive op te slaan.**
1. Op het **[!UICONTROL gebied van de Naam van het Dossier]**, plaats de naam van het dossier. Aangezien de uitvoer een PDF is, moet uw bestandsnaam eindigen op .pdf.
1. Op het **gebied van de Inhoud van het Dossier, gebruik**&#x200B;[!UICONTROL &#x200B; Dynamische inhoud &#x200B;]&#x200B;**paneel om de *waarde van de Inhoud van het Dossier van de PDF* van de**&#x200B;[!UICONTROL &#x200B; stap van de PDF &#x200B;]&#x200B;**van de Fusie op te nemen.**

   ![ Stroom in Microsoft Power Automate overzicht ](assets/flowOverviewSavedMergedDocument.png)

### Stroom uitproberen

1. Selecteer **[!UICONTROL Opslaan]**.
1. Selecteer **[!UICONTROL Test]**.
1. Selecteer **[!UICONTROL manueel]** en dan **[!UICONTROL sparen &amp; Test]**.
1. Selecteer **[!UICONTROL Doorgaan]**.
1. Ga waarden voor *Voornaam* in, *Achternaam*, en *Salaris*.
1. Selecteer **[!UICONTROL Stroom van de Looppas]**.

In de map OneDrive ziet u de gecombineerde PDF met pagina&#39;s uit het eerste en tweede document.

## Deel 4: Protect PDF-document

Nadat u het document hebt gegenereerd, kunt u het beschermen tegen bewerking door een extra stap op te nemen voordat u het opslaat op OneDrive.

### PDF beveiligen

1. Terwijl het uitgeven van uw stroom in Macht automatiseer, selecteer **+** binnen tussen de **[!UICONTROL PDF van de Fusie]** actie en **[!UICONTROL creeer dossier 3]** actie.

   ![ plus symbool tussen twee acties om een nieuwe actie toe te voegen ](assets/addActionToProtect.png)

1. Selecteer **[!UICONTROL voeg een actie]** toe.
1. Onderzoek naar *Diensten van Adobe PDF* in de onderzoeksbar.
1. Selecteer **[!UICONTROL de Diensten van Adobe PDF]**.
1. Selecteer de **[!UICONTROL PDF van Protect van het Bekijken van]** actie.
1. In het **[!UICONTROL gebied van de Naam van het Dossier]**, plaats de naam aan uw gewenste naam, zolang het met een .pdf uitbreiding beëindigt.
1. Plaats het **[!UICONTROL gebied van het Wachtwoord]** aan uw gespecificeerd wachtwoord om het document te openen.
1. Op het **gebied van de Inhoud van het Dossier**, gebruik het **[!UICONTROL Dynamische 3&rbrace; paneel van de Inhoud van het Inhoud van het Dossier *PDF* waarde van de**&#x200B;[!UICONTROL &#x200B; stap van de PDF van de Samenvoeging &#x200B;]&#x200B;**.]**

### Opslaan naar OneDrive bijwerken

Als het document is beveiligd, kunt u het bestand weer opslaan in OneDrive. In dit voorbeeld, werkt u het reeds bestaande **dossier 3** actie met een nieuwe *waarde van de Inhoud van het Dossier* bij.

1. Selecteer uw curseur op het **[!UICONTROL gebied van de Inhoud van het Dossier]** in **[!UICONTROL dossier 3]** actie creëren.
1. Gebruik het **[!UICONTROL Dynamische paneel van de Inhoud]** om de *waarde van de Inhoud van het Dossier van de PDF* van de **PDF van Protect van het Bekijken van** stap op te nemen.

### Stroom uitproberen

1. Selecteer **[!UICONTROL Opslaan]**.
1. Selecteer **[!UICONTROL Test]**.
1. Selecteer **[!UICONTROL manueel]** en dan **[!UICONTROL sparen &amp; Test]**.
1. Selecteer **[!UICONTROL Doorgaan]**.
1. Ga waarden voor *Voornaam* in, *Achternaam*, en *Salaris*.
1. Selecteer **[!UICONTROL Stroom van de Looppas]**.

In de OneDrive-map ziet u de gecombineerde PDF waarin u nu wordt gevraagd een wachtwoord in te voeren om het document te bekijken.

## Volgende stappen

In deze zelfstudie hebt u een Word-document geconverteerd naar een PDF, een document gegenereerd op basis van gegevens, documenten samengevoegd en beveiligd met een wachtwoord. Voor meer informatie verkent u enkele andere acties die beschikbaar zijn in de Adobe PDF Services-connector in Microsoft Power Automate:

* Bekijk de vooraf gemaakte sjablonen die beschikbaar zijn in Microsoft Power Automate.
* Leer van [ artikelen ](https://medium.com/adobetech/tagged/microsoft-power-automate) op Blog van de Tech van de Adobe.
* Het overzicht [ documentatie ](https://developer.adobe.com/document-services/docs/overview/document-generation-api/) voor de Generatie API van het Document van de Adobe.
