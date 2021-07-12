---
title: 將資料傳送至AAM時，在SPA頁面上使用最佳實務
description: 在本檔案中，我們會說明從單頁應用程式(SPA)傳送資料至Adobe Audience Manager(AAM)時，您應遵循並注意的幾項最佳實務。 本檔案將著重於使用Launch by Adobe，此為建議的實作方法。
feature: 實作基本知識
topics: spa
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
topic: SPA
role: Developer, Data Engineer
level: Experienced
exl-id: 99ec723a-dd56-4355-a29f-bd6d2356b402
source-git-commit: 4b91696f840518312ec041abdbe5217178aee405
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 0%

---

# 將資料傳送至AAM時，在SPA頁面上使用最佳實務 {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

在本檔案中，我們將說明從[!UICONTROL Single Page Applications](SPA)傳送資料至Adobe Audience Manager(AAM)時，您應遵循並注意的幾項最佳實務。 本檔案著重於使用[!UICONTROL Experience Platform Launch]，此為建議的實作方法。

## 初始附註

* 以下項目會假設您使用[!DNL Platform Launch]在您的網站上實作。 若您未使用[!DNL Platform Launch]，則考量事項仍會存在，但您需要將其調整至實施方法。
* 所有SPA都不同，因此您可能需要調整下列一些項目，以最符合您的需求，但我們想與您分享一些最佳實務；從SPA頁面傳送資料至Audience Manager時需要思考的事項。

## 在Experience Platform Launch中使用SPA和AAM的簡單圖表 {#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![aam中的spa  [!DNL launch]](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>如前所述，此簡化圖表說明在使用[!DNL Platform Launch]的Adobe Audience Manager實作(不含Adobe Analytics)中，如何處理SPA頁面。 如您所見，這是相當直接的，最重要的決定是您將如何將視圖更改（或操作）傳達給[!DNL Platform Launch]。

## 從SPA頁面觸發[!DNL Launch] {#triggering-launch-from-the-spa-page}

在[!DNL Platform Launch]中觸發規則(並因此將資料傳送至Audience Manager)的兩種較常見方法為：

* 設定JavaScript自訂事件(請參閱使用Adobe Analytics的範例[HERE](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html))
* 使用[!UICONTROL Direct Call Rule]

在此Audience Manager範例中，我們將使用[!DNL Launch]中的[!UICONTROL Direct Call rule]來觸發進入Audience Manager的點擊。 如您在後幾節中所見，將[!UICONTROL Data Layer]設為新值後，這真的會很有用，讓[!DNL Platform Launch]中的[!UICONTROL Data Element]可擷取它。

## 示範頁面 {#demo-page}

我們已建立一個小型示範頁面，示範如何變更[!DNL data layer]中的值，並將其傳送至AAM，如同您在SPA頁面上所做的一樣。 此功能可針對所需的更詳盡變更進行模型化。 您可以找到此示範頁面[HERE](https://aam.enablementadobe.com/SPA-Launch.html)。

## 設定 [!DNL data layer] {#setting-the-data-layer}

如上所述，當頁面上載入新內容或有人在網站上執行動作時，[!DNL data layer]必須在頁面標題中動態設定，才會呼叫[!DNL Launch]並執行[!UICONTROL rules]，讓[!DNL Platform Launch]能夠從[!DNL data layer]擷取新值，並將其推入Audience Manager。

如果您前往上述示範網站並查看頁面來源，您會看到：

* [!DNL data layer]位於頁面標題中，呼叫[!DNL Platform Launch]之前
* 模擬SPA連結中的JavaScript會變更[!UICONTROL Data Layer]，而THEN會呼叫[!DNL Platform Launch](_satellite.track()呼叫)。 如果您使用JavaScript自訂事件，而非此[!UICONTROL Direct Call Rule]，則課程相同。 首先更改[!DNL data layer]，然後調用[!DNL Launch]。

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## 其他資源 {#additional-resources}

* [SPA在Adobe論壇上的討論](https://forums.adobe.com/thread/2451022)
* [參考架構網站，以說明如何在Launch by Adobe中實作SPA](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [在Adobe Analytics中追蹤SPA時使用最佳實務](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [用於本文的演示網站](https://aam.enablementadobe.com/SPA-Launch.html)
