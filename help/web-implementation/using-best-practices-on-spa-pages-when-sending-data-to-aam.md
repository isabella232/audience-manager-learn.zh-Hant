---
title: 在傳送資料至AAM時，在SPA頁面上使用最佳實務
description: 在本檔案中，我們將說明從單頁應用程式(SPA)傳送資料至Adobe Audience Manager(AAM)時，您應遵循並注意的幾個最佳實務。 本檔案將著重於使用Launch by Adobe，這是建議的實作方法。
feature: implementation basics
topics: spa
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 0%

---


# 在傳送資料至AAM {#using-best-practices-on-spa-pages-when-sending-data-to-aam}時，在SPA頁面上使用最佳實務

在本檔案中，我們將說明從[!UICONTROL Single Page Applications](SPA)傳送資料至Adobe Audience Manager(AAM)時，您應遵循並注意的幾個最佳實務。 本檔案著重於使用[!UICONTROL Experience Platform Launch]，這是建議的實作方法。

## 初始附註

* 以下項目將假設您使用[!DNL Platform Launch]在您的網站上實施。 如果您未使用[!DNL Platform Launch]，則仍會有考量，但您必須將它們調整為您的實施方法。
* 所有SPA都不同，因此您可能需要調整下列部分項目以最符合您的需求，但我們想與您分享一些最佳實務；從SPA頁面傳送資料至Audience Manager時，您需要考慮的事項。

## 在Experience Platform Launch {#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}中使用SPA和AAM的簡單圖表

![aam中的spa  [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>如前所述，此圖說明如何在Adobe Audience Manager實作（不含Adobe Analytics）中使用[!DNL Platform Launch]處理SPA頁面。 如您所見，這是相當直接的，而最重要的決定是如何將視圖更改（或動作）傳達給[!DNL Platform Launch]。

## 從SPA頁{#triggering-launch-from-the-spa-page}觸發[!DNL Launch]

在[!DNL Platform Launch]中觸發規則（並因此傳送資料至Audience Manager）的兩種較常見方法為：

* 設定JavaScript自訂事件（請參閱使用Adobe Analytics的範例[HERE](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)）
* 使用[!UICONTROL Direct Call Rule]

在此Audience Manager範例中，我們將使用[!DNL Launch]中的[!UICONTROL Direct Call rule]來觸發進入Audience Manager的點擊。 如您在下節中所見，將[!UICONTROL Data Layer]設為新值，如此[!DNL Platform Launch]中的[!UICONTROL Data Element]就能擷取到它，這真的會很有用。

## 示範頁面{#demo-page}

我們已建立小型示範頁面，示範如何變更[!DNL data layer]中的值，並將它傳送至AAM，就像您在SPA頁面上所做的一樣。 此功能可建立模型，以進行更詳盡的變更。 您可以找到此示範頁面[HERE](https://aam.enablementadobe.com/SPA-Launch.html)。

## 設定 [!DNL data layer] {#setting-the-data-layer}

如前所述，當頁面上載入新內容或有人在網站上執行動作時，[!DNL data layer]必須在頁面標題中動態設定，才會呼叫[!DNL Launch]並執行[!UICONTROL rules]，如此[!DNL Platform Launch]才能從[!DNL data layer]擷取新值並將它們推送至Audience Manager。

如果您前往上述示範網站並檢視頁面來源，您會看到：

* [!DNL data layer]位於頁面標題中，呼叫[!DNL Platform Launch]之前
* 模擬SPA連結中的JavaScript會變更[!UICONTROL Data Layer]，然後呼叫[!DNL Platform Launch](_satellite.track()呼叫)。 如果您使用JavaScript自訂事件而非此[!UICONTROL Direct Call Rule]，則課程是相同的。 首先更改[!DNL data layer]，然後調用[!DNL Launch]。

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## 其他資源 {#additional-resources}

* [Adobe論壇上的SPA討論](https://forums.adobe.com/thread/2451022)
* [參考架構網站，以示範如何在Launch by Adobe中建置SPA](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [在Adobe Analytics中追蹤SPA時使用最佳實務](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [本文使用的示範網站](https://aam.enablementadobe.com/SPA-Launch.html)
