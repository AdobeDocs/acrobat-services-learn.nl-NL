---
title: Aan de slag met Acrobat Sign API
description: Leer hoe u de Acrobat Sign API in uw toepassing opneemt om handtekeningen en andere informatie te verzamelen
role: Developer
level: Intermediate
type: Tutorial
thumbnail: KT-8089.jpg
kt: 8089
exl-id: ae1cd9db-9f00-4129-a2a1-ceff1c899a83
source-git-commit: 799b37e526073893fe7c078db547798d6c31d1b2
workflow-type: tm+mt
source-wordcount: '2058'
ht-degree: 2%

---

# Aan de slag met de Adobe Sign API

![Hoofdlettergebruik hoofdbanner gebruiken](assets/UseCaseStartedHero.jpg)

[Acrobat Sign API](https://www.adobe.io/apis/documentcloud/sign.html) is een fantastische manier om de manier waarop u ondertekende overeenkomsten beheert te verbeteren. Ontwikkelaars kunnen hun systemen eenvoudig integreren met de Sign-API, die een betrouwbare, eenvoudige manier biedt om documenten te uploaden, deze ter ondertekening te verzenden, herinneringen te verzenden en e-handtekeningen te verzamelen.

## Wat je kunt leren

In deze praktische zelfstudie wordt uitgelegd hoe ontwikkelaars Sign API kunnen gebruiken om toepassingen en workflows te verbeteren die zijn gemaakt met [!DNL Adobe Acrobat Services]. [!DNL Acrobat Services] include [Adobe PDF Services API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-tools.html), [Adobe PDF Embed API](https://www.adobe.io/apis/documentcloud/viesdk) (gratis), en [Adobe-API voor documentgeneratie](https://www.adobe.io/apis/documentcloud/dcsdk/doc-generation.html).

Leer meer bepaald hoe u de Acrobat Sign API in uw toepassing opneemt om handtekeningen en andere informatie te verzamelen, zoals werknemersinformatie op een verzekeringsformulier. Algemene stappen met vereenvoudigde HTTP-aanvragen en -reacties worden gebruikt. U kunt deze verzoeken in uw favoriete taal implementeren. U kunt een PDF maken met een combinatie van [[!DNL Acrobat Services] API&#39;s](https://www.adobe.io/apis/documentcloud/dcsdk/), uploadt u deze naar de Sign-API als een [voorbijgaand](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/overview/terminology.md) documenten te verzenden en handtekeningen van eindgebruikers aan te vragen met de overeenkomst of [widget](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/overview/terminology.md) workflow.

## Een PDF-document maken

Maak eerst een Microsoft Word-sjabloon en sla deze op als een PDF. U kunt de pijplijn ook automatiseren met de API voor het genereren van documenten om een sjabloon te uploaden dat in Word is gemaakt en vervolgens een PDF-document te genereren. De API voor het genereren van documenten maakt deel uit van [!DNL Acrobat Services], [gratis voor zes maanden en vervolgens &#39;pay-as-you-go&#39; voor slechts USD 0,05 per documenttransactie](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html).

In dit voorbeeld is de sjabloon slechts een eenvoudig document met een paar ondertekenaarsvelden die moeten worden ingevuld. Geef de velden nu een naam en voeg later de daadwerkelijke velden in deze zelfstudie in.

![Screenshot van het verzekeringsformulier met een paar velden](assets/GSASAPI_1.png)

## Het geldige API-toegangspunt detecteren

Voordat u gaat werken met de Sign-API, [een gratis ontwikkelaarsaccount maken](https://acrobat.adobe.com/ca/en/sign/developer-form.html) om de API te openen, test u de documentuitwisseling en -uitvoering en test u de e-mailfunctie.

Adobe verspreidt de Acrobat Sign API over de hele wereld in veel implementaties, genaamd &#39;shards&#39;. Elk shard dient de account van een klant, zoals NA1, NA2, NA3, EU1, JP1, AU1, IN1 en andere. De shard-namen komen overeen met geografische locaties. Deze shards vormen de basis-URI (toegangspunten) van de API-eindpunten.

Als u toegang wilt tot de Sign-API, moet u eerst het juiste toegangspunt voor uw account vinden. Dit kan api.na1.adobesign.com, api.na4.adobesign.com, api.eu1.adobesign.com of andere zijn, afhankelijk van uw locatie.

```
  GET /api/rest/v6/baseUris HTTP/1.1
  Host: https://api.adobesign.com
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  Accept: application/json

  Response Body (example):

  {
    "apiAccessPoint": "https://api.na4.adobesign.com/", 
    "webAccessPoint": "https://secure.na4.adobesign.com/" 
  }
```

In het bovenstaande voorbeeld is dit een reactie met de waarde als toegangspunt.

>[!IMPORTANT]
>
>In dit geval moeten alle volgende verzoeken die u voor de Sign-API indient, gebruikmaken van dat toegangspunt. Als u een toegangspunt gebruikt dat uw regio niet aanbiedt, krijgt u een fout.

## Een tijdelijk document uploaden

Met Adobe Sign kunt u verschillende stromen maken die documenten voorbereiden voor ondertekening of gegevensverzameling. Ongeacht de stroom van uw toepassing, moet u eerst een document uploaden, dat slechts zeven dagen beschikbaar blijft. De volgende API-aanroepen moeten dan verwijzen naar dit tijdelijke document.

Het document wordt met een verzoek van een POST geüpload naar de `/transientDocuments` eindpunt. De multipart-aanvraag bestaat uit de bestandsnaam, een bestandsstroom en het MIME-type (media) van het documentbestand. De eindpuntreactie bevat een id die het document identificeert.

Bovendien kan uw toepassing een callback-URL opgeven die door Acrobat Sign wordt gepingeld, zodat de toepassing op de hoogte wordt gesteld wanneer het ondertekeningsproces is voltooid.


```
  POST /api/rest/v6/transientDocuments HTTP/1.1
  Host: {YOUR-API-ACCESS-POINT}
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  x-api-user: email:your-api-user@your-domain.com
  Content-Type: multipart/form-data
  File-Name: "Insurance Form.pdf"
  File: "[path]\Insurance Form.pdf"
  Accept: application/json

  Response Body (example):

  {
     "transientDocumentId": "3AAA...BRZuM"
  }
```

## Een webformulier maken

Webformulieren (voorheen ondertekeningswidgets genoemd) zijn gehoste documenten die iedereen die toegang heeft, kan ondertekenen. Voorbeelden van webformulieren zijn aanmeldingsformulieren, afstandsverklaringen en andere documenten die veel mensen online openen en ondertekenen.

Als u een nieuw webformulier wilt maken met de Sign-API, moet u eerst een tijdelijk document uploaden. Het verzoek van de POST aan de `/widgets` het eindpunt gebruikt het teruggekeerde `transientDocumentId` .

In dit voorbeeld is het webformulier `ACTIVE`, maar u kunt het maken in een van de volgende drie statussen:

* CONCEPT — het webformulier stapsgewijs samenstellen

* ONTWERPEN — formuliervelden toevoegen of bewerken in het webformulier

* ACTIEF — om het webformulier direct te hosten

De informatie over de deelnemers van het formulier moet ook worden gedefinieerd. De `memberInfos` eigenschap bevat gegevens over de deelnemers, zoals e-mail. Momenteel ondersteunt deze set niet meer dan één lid. Maar omdat de e-mail van de webformulierondertekenaar onbekend is op het moment dat u een webformulier maakt, moet de e-mail leeg blijven, zoals in het volgende voorbeeld. De `role` eigenschap definieert de rol die de leden in `memberInfos` (zoals de ONDERTEKENAAR en DE GOEDKEURER).

```
  POST /api/rest/v6/widgets HTTP/1.1
  Host: {YOUR-API-ACCESS-POINT}
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  x-api-user: email:your-api-user@your-domain.com
  Content-Type: application/json

  Request Body:

  {
    "fileInfos": [
      {
      "transientDocumentId": "YOUR-TRANSIENT-DOCUMENT-ID"
      }
     ],
    "name": "Insurance Form",
      "widgetParticipantSetInfo": {
          "memberInfos": [{
              "email": ""
          }],
      "role": "SIGNER"
      },
      "state": "ACTIVE"
  }

  Response Body (example):

  {
     "id": "CBJ...PXoK2o"
  }
```

U kunt een webformulier maken als `DRAFT` of `AUTHORING`wijzigt u vervolgens de status wanneer het formulier door de toepassingspijplijn stroomt. Als u de status van een webformulier wilt wijzigen, raadpleegt u de [PUT /widgets/{widgetId}/state](https://secure.na4.adobesign.com/public/docs/restapi/v6#!/widgets/updateWidgetState) eindpunt.

## De webformulier-host-URL lezen

De volgende stap bestaat uit het zoeken naar de URL die als host fungeert voor het webformulier. Het /widgets eindpunt wint een lijst van de gegevens van de Vorm van het Web, met inbegrip van ontvangen URL van de Vorm van het Web terug die u aan uw gebruikers door:sturen, om handtekeningen en andere vormgegevens te verzamelen.

Dit eindpunt retourneert een lijst, zodat u het specifieke formulier kunt vinden op basis van de id in het dialoogvenster `userWidgetList` voordat u de URL ophaalt die als host fungeert voor het webformulier:

```
  GET /api/rest/v6/widgets HTTP/1.1
  Host: {YOUR-API-ACCESS-POINT}
  Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
  Accept: application/json

  Response Body:

  {
    "userWidgetList": [
      {
        "id": "CBJCHB...FGf",
        "name": "Insurance Form",
        "groupId": "CBJCHB...W86",
        "javascript": "<script type='text/javascript' ...
        "modifiedDate": "2021-03-13T15:52:41Z",
        "status": "ACTIVE",
        "Url":
        "https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIB...Rag*",
        "hidden": false
      },
      {
        "id": "CBJCHB...I8_",
        "name": "Insurance Form",
        "groupId": "CBJCHBCAABAAyhgaehdJ9GTzvNRchxQEGH_H1ya0xW86",
        "javascript": "<script type='text/javascript' language='JavaScript'
        src='https://sec
        "modifiedDate": "2021-03-13T02:47:32Z",
        "status": "ACTIVE",
        "Url":
        "https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIB...AAB",
        "hidden": false
      },
      {
        "id": "CBJCHB...Wmc",
```

## Uw webformulier beheren

Dit formulier is een PDF-document dat gebruikers kunnen invullen. U moet de editor van het formulier echter wel vertellen welke velden gebruikers moeten invullen en waar ze zich in het document bevinden:

![Screenshot van het verzekeringsformulier met een paar velden](assets/GSASAPI_1.png)

In het document hierboven worden de velden nog niet weergegeven. Ze worden toegevoegd tijdens het definiëren van de velden die de informatie van de ondertekenaar en hun grootte en positie verzamelen.

Ga nu naar de [Webformulieren](https://secure.na4.adobesign.com/public/agreements/#agreement_type=webform) op de pagina &quot;Uw overeenkomsten&quot; en zoek naar het formulier dat u hebt gemaakt.

![Screenshot van het Acrobat Sign-beheertabblad](assets/GSASAPI_2.png)

![Screenshot van het Acrobat Sign-tabblad Beheren met webformulieren geselecteerd](assets/GSASAPI_3.png)

Klikken **Bewerken** om de documentbewerkingspagina te openen. De beschikbare vooraf gedefinieerde velden staan in het rechterdeelvenster.

![Screenshot van de Acrobat Sign-formulierontwerpomgeving](assets/GSASAPI_4.png)

Met de editor kunt u tekst- en handtekeningvelden slepen en neerzetten. Nadat u alle vereiste velden hebt toegevoegd, kunt u deze vergroten, verkleinen en uitlijnen om het formulier te perfectioneren. Tot slot klikt u op **Opslaan** om het formulier te maken.

![Screenshot van de Acrobat Sign-formulierontwerpomgeving met toegevoegde formuliervelden](assets/GSASAPI_5.png)

## Een webformulier verzenden voor ondertekening

Nadat u het webformulier hebt voltooid, moet u het verzenden, zodat gebruikers het kunnen invullen en ondertekenen. Nadat u het formulier hebt opgeslagen, kunt u de URL en de ingesloten code weergeven en kopiëren.

**URL van webformulier kopiëren**: Gebruik deze URL om gebruikers naar een gehoste versie van deze overeenkomst te sturen voor revisie en ondertekening. Bijvoorbeeld:

[https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIBAA3...babw\*](https://secure.na4.adobesign.com/public/esignWidget?wid=CBFCIBAA3AAABLblqZhCndYscuKcDMPiVfQlpaGPb-5D7ebE9NUTQ6x6jK7PIs8HCtTzr3HOx8U6D5qqbabw*)

**Insluitcode webformulier kopiëren**: U kunt de overeenkomst toevoegen aan uw website door deze code te kopiëren en in uw HTML te plakken.

Bijvoorbeeld:

```
<iframe
src="https://secure.na4.adobesign.com/public/esignWidget?wid=CBFC
...yx8*&hosted=false" width="100%" height="100%" frameborder="0"
style="border: 0;
overflow: hidden; min-height: 500px; min-width: 600px;"></iframe>
```

![Screenshot van het definitieve webformulier](assets/GSASAPI_6.png)

Wanneer uw gebruikers toegang krijgen tot de gehoste versie van uw formulier, bekijken ze het tijdelijke document dat eerst is geüpload met de velden die op de opgegeven positie staan.

![Screenshot van het definitieve webformulier](assets/GSASAPI_7.png)

De gebruiker vult vervolgens de velden in en ondertekent het formulier.

![Screenshot van gebruiker die handtekeningveld selecteert](assets/GSASAPI_8.png)

Vervolgens ondertekent uw gebruiker het document met een eerder opgeslagen handtekening of met een nieuwe handtekening.

![Screenshot van ondertekeningservaring](assets/GSASAPI_9.png)

![Schermafbeelding van handtekening](assets/GSASAPI_10.png)

Wanneer de gebruiker klikt **Toepassen**, instrueert Adobe hen om hun e-mail te openen en de handtekening te bevestigen. De handtekening blijft in behandeling totdat de bevestiging is ontvangen.

![Screenshot van nog een stap](assets/GSASAPI_11.png)

Deze verificatie voegt meervoudige verificatie toe en versterkt de beveiliging van het ondertekeningsproces.

![Screenshot van bevestigingsbericht](assets/GSASAPI_12.png)

![Screenshot van bericht van voltooiing](assets/GSASAPI_13.png)

## Voltooide webformulieren lezen

Nu is het tijd om de formuliergegevens op te halen die gebruikers hebben ingevuld. De `/widgets/{widgetId}/formData` Hiermee haalt u gegevens op die de gebruiker in een interactief formulier heeft ingevoerd bij de ondertekening van het formulier.

```
GET /api/rest/v6/widgets/{widgetId}/formData HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
Accept: text/csv
```

De resulterende CSV-bestandsstroom bevat formuliergegevens.

```
Response Body:
"Agreement
name","completed","email","role","first","last","title","company","agreementId",
"email verified","web form signed/approved"
"Insurance Form","","myemail@email.com","SIGNER","John","Doe","My Job Title","My
Company Name","","","2021-03-07 19:32:59"
```

## Een overeenkomst maken

Als alternatief voor webformulieren kunt u overeenkomsten maken. In de volgende secties vindt u enkele eenvoudige stappen voor het beheren van overeenkomsten met de Sign-API.

Als u een document ter ondertekening of goedkeuring naar opgegeven ontvangers verzendt, wordt er een overeenkomst gemaakt. U kunt de status en voltooiing van een overeenkomst volgen met behulp van API&#39;s.

U kunt een overeenkomst maken met een [tijdelijk document](https://helpx.adobe.com/sign/kb/how-to-send-an-agreement-through-REST-API.html), [bibliotheekdocument](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/samples/send_using_library_doc.md), of URL. In dit voorbeeld is de overeenkomst gebaseerd op de `transientDocumentId`, net als het webformulier dat eerder is gemaakt.

```
POST /api/rest/v6/agreements HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
x-api-user: email:your-api-user@your-domain.com
Content-Type: application/json
Accept: application/json
Request Body:
{
    "fileInfos": [
      {
      "transientDocumentId": "{transientDocumentId}"
      }
     ],
    "name": "{agreementName}",
    "participantSetsInfo": [
      {
      "memberInfos": [
          {
          "email": "{signerEmail}"
          }
        ],
        "order": 1,
        "role": "SIGNER"
      }
    ],
    "signatureType": "ESIGN",
    "state": "IN_PROCESS"
  }
```

In dit voorbeeld wordt de overeenkomst gemaakt als IN_PROCESS, maar u kunt de overeenkomst maken in een van de volgende drie statussen:

* CONCEPT — de overeenkomst stapsgewijs opbouwen voordat deze wordt verzonden

* ONTWERPEN — formuliervelden toevoegen of bewerken in de overeenkomst

* IN_PROCESS — om de overeenkomst onmiddellijk te verzenden

Als u een overeenkomststatus wilt wijzigen, gebruikt u de opdracht `PUT /agreements/{agreementId}/state` om een van de toegestane statusovergangen hieronder uit te voeren:

* CONCEPT NAAR ONTWERPEN

* ONTWERPEN NAAR IN_PROCESS

* IN_PROCES NAAR GEANNULEERD

De `participantSetsInfo` eigenschap hierboven bevat e-mails van personen die geacht worden deel te nemen aan de overeenkomst en welke actie zij uitvoeren (ondertekenen, goedkeuren, erkennen, enzovoort). In het bovenstaande voorbeeld is er slechts één deelnemer: de ondertekenaar. Schriftelijke handtekeningen zijn beperkt tot vier per document.

Wanneer u een overeenkomst maakt, verzendt Adobe deze in tegenstelling tot webformulieren automatisch ter ondertekening. Het eindpunt retourneert de unieke id van de overeenkomst.


```
  Response Body:

  {
     id (string): The unique identifier of the agreement
  }
```

## Informatie over overeenkomstleden ophalen

Als u een overeenkomst hebt gemaakt, kunt u de opdracht `/agreements/{agreementId}/members` eindpunt om informatie over leden van de overeenkomst op te halen. U kunt bijvoorbeeld controleren of een deelnemer de overeenkomst heeft ondertekend.

```
GET /api/rest/v6/agreements/{agreementId}/members HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
Accept: application/json
```

De resulterende JSON-responsstructuur bevat informatie over de deelnemers.

```
  Response Body:

  {
     "participantSets":[
        {
           "memberInfos":[
              {
                 "id":"CBJ...xvM",
                 "email":"participant@email.com",
                 "self":false,
                 "securityOption":{
                    "authenticationMethod":"NONE"
                 },
                 "name":"John Doe",
                 "status":"ACTIVE",
                 "createdDate":"2021-03-16T03:48:39Z",
                 "userId":"CBJ...vPv"
              }
           ],
           "id":"CBJ...81x",
           "role":"SIGNER",
           "status":"WAITING_FOR_MY_SIGNATURE",
           "order":1
        }
     ],
```

## Overeenkomstopinneringen verzenden

Afhankelijk van de bedrijfsregels kan een deadline deelnemers verhinderen de overeenkomst na een bepaalde datum te ondertekenen. Als de overeenkomst een vervaldatum heeft, kunt u deelnemers eraan herinneren dat die datum nadert.

Gebaseerd op de informatie van de leden van de overeenkomst die u hebt ontvangen na de oproep aan de `/agreements/{agreementId}/members` in de laatste sectie kunt u e-mailherinneringen versturen aan alle deelnemers die de overeenkomst nog niet hebben ondertekend.

Een verzoek van de POST aan de `/agreements/{agreementId}/reminders` Hiermee wordt een herinnering gemaakt voor de opgegeven deelnemers aan een overeenkomst die door de `agreementId` parameter.

```
POST /agreements/{agreementId}/reminders HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
x-api-user: email:your-api-user@your-domain.com
Content-Type: application/json
Accept: application/json
  Request Body:

  {
    "recipientParticipantIds": [{agreementMemberIdList}],
    "agreementId": "{agreementId}",
    "note": "This is a reminder that you haven't signed the agreement yet.",
    "status": "ACTIVE"
  }

  Response Body:

  {
     id (string, optional): An identifier of the reminder resource created on the
     server. If provided in POST or PUT, it will be ignored
  }
```

Nadat u de herinnering hebt geplaatst, ontvangen de gebruikers een e-mail met de gegevens van de overeenkomst en een koppeling naar de overeenkomst.

![Schermafbeelding van Herinneringsbericht](assets/GSASAPI_14.png)

## Voltooide overeenkomsten lezen

Net als webformulieren kunt u gegevens lezen over overeenkomsten die de ontvangers hebben ondertekend. De `/agreements/{agreementId}/formData` het eindpunt wint gegevens terug die door de gebruiker werden ingegaan toen zij de Vorm van het Web ondertekenden.

```
GET /api/rest/v6/agreements/{agreementId}/formData HTTP/1.1
Host: {YOUR-API-ACCESS-POINT}
Authorization: Bearer {YOUR-INTEGRATION-KEY-HERE}
Accept: text/csv
Response Body:
"completed","email","role","first","last","title","company","agreementId"
"2021-03-16 18:11:45","myemail@email.com","SIGNER","John","Doe","My Job Title","My
Company Name","CBJCHBCAABAA5Z84zy69q_Ilpuy5DzUAahVfcNZillDt"
```

## Volgende stappen

Met de Acrobat Sign API kunt u documenten, webformulieren en overeenkomsten beheren. De vereenvoudigde maar volledige workflows die zijn gemaakt met webformulieren en -overeenkomsten, worden op een algemene manier uitgevoerd, zodat ontwikkelaars ze in elke taal kunnen implementeren.

Voor een overzicht van de werking van de Sign-API vindt u voorbeelden in het [Handleiding voor ontwikkelaars van API-gebruik](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/api_usage.md). Deze documentatie bevat korte artikelen over veel van de stappen die in het hele artikel worden gevolgd en andere gerelateerde onderwerpen.

Acrobat Sign API is beschikbaar via verschillende lagen van [lidmaatschappen voor één en meerdere gebruikers voor elektronische ondertekening](https://acrobat.adobe.com/nl/nl/sign/pricing/plans.html), zodat je een prijsmodel kunt kiezen dat het best aansluit bij je behoeften. Nu je weet hoe eenvoudig het is om Sign API in je apps op te nemen, kun je geïnteresseerd zijn in andere functies zoals [Acrobat Sign Webhooks](https://www.adobe.io/apis/documentcloud/sign/docs.html#!adobedocs/adobe-sign/master/webhooks.md), een op push-gebaseerd programmeermodel. In plaats van te vereisen dat uw app frequente controles uitvoert in Acrobat Sign-gebeurtenissen, kunt u met Webhooks een HTTP-URL registreren waarvoor de Sign-API een callback-aanvraag voor POSTEN uitvoert wanneer een gebeurtenis plaatsvindt. Webhooks bieden krachtige programmeermogelijkheden door uw toepassing te voorzien van real-time en onmiddellijke updates.

Bekijk de [pay-as-you-go-pricing](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-pricing.html), voor wanneer uw gratis proefversie van de Adobe PDF Services API van zes maanden eindigt en de gratis Adobe PDF Embed-API.

Ga aan de slag met [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html).
