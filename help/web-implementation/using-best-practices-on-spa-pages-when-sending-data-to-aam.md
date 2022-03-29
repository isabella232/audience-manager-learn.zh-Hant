---
title: 在將資料發送SPA到時使用頁面上的最佳做AAM法
description: 瞭解從單頁應用程式()向Adobe Audience Manager(SPA)發送資料的最AAM佳做法。 本文重點介紹了Experience Platform標籤的使用，推薦的實現方法。
feature: Implementation Basics
topics: spa
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1390
topic: SPA
role: Developer, Data Engineer
level: Experienced
exl-id: 99ec723a-dd56-4355-a29f-bd6d2356b402
source-git-commit: d4874d9f6d7a36bb81ac183eb8b853d893822ae0
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 0%

---

# 在將資料發送SPA到時使用頁面上的最佳做AAM法 {#using-best-practices-on-spa-pages-when-sending-data-to-aam}

本文檔介紹從單頁應用程式()向Adobe Audience Manager(SPA)發送資料的幾種最佳AAM做法。 本文重點介紹 [!UICONTROL Experience Platform tags]，推薦的實現方法。

## 初始注釋

* 以下項目將假定您正在使用平台標籤在站點上實施。 如果您不使用平台標籤，則仍然存在考慮事項，但您需要將它們調整為您的實現方法。
* 所有SPA項都不同，因此您可能需要調整以下某些項以最好地滿足您的要求，但Adobe希望分享一些最佳實踐，當您將資料從頁面發送到Audience Manager時，您需要考慮這些SPA最佳實踐。

## 使用Experience Platform標SPA簽和AAM在標籤（以前稱「啟動」）的簡單圖{#simple-diagram-of-working-with-spas-and-aam-in-experience-platform-launch}

![標籤中aam的spa](assets/spa_for_aam_in_launch.png)

>[!NOTE]
>如前所述，這是一個簡化的圖SPA表，說明在Adobe Audience Manager實施(不使用Adobe Analytics)中，如何使用平台標籤處理頁面。 正如您所看到的，它是相當直接的，最重要的決定是如何將視圖更改（或操作）與「平台」標籤進行通信。

## 從頁面觸發標SPA記 {#triggering-launch-from-the-spa-page}

在Platform標籤中觸發規則(並因此將資料發送到Audience Manager)的兩種更常見方法是：

* 設定JavaScript自定義事件（請參見示例） [這裡](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html) Adobe Analytics
* 使用 [!UICONTROL Direct Call Rule]

在此Audience Manager示例中，使用 [!UICONTROL Direct Call rule] 在平台標籤中觸發命中進入Audience Manager。 正如您在下一節中將看到的，通過設定 [!UICONTROL Data Layer] 到新值，以便 [!UICONTROL Data Element] 在平台標籤中。

## 演示頁 {#demo-page}

這是一個小頁面，它演示了如何更改資料層中的值，並將其發送到Audience Manager中，就像您在頁面上所做的SPA那樣。 此功能可以建模，以便進行更詳細的更改。 您可以找到此演示頁 [這裡](https://aam.enablementadobe.com/SPA-Launch.html)。

## 設定資料層 {#setting-the-data-layer}

如前所述，當新內容載入到頁面上或有人在站點上執行操作時，需要在頁面的首部動態設定資料層，然後調用並運行 [!UICONTROL rules]，以便平台標籤可以從資料層中提取新值並將其推入Audience Manager。

如果您轉到上面列出的演示站點並查看頁面源，您將看到：

* 資料層位於頁面首部，在調用平台標籤之前
* 模擬連結中的JavaScriptSPA將更改 [!UICONTROL Data Layer]，然後調用平台標籤( `_satellite.track()` 呼叫)。 如果您使用的是JavaScript自定義事件而不是此事件 [!UICONTROL Direct Call Rule]但教訓是一樣的。 首先更改 [!DNL data layer]，然後調用平台標籤。

>[!VIDEO](https://video.tv.adobe.com/v/23322/?quality=12)

## 其他資源 {#additional-resources}

* [關於SPAAdobe論壇的討論](https://forums.adobe.com/thread/2451022)
* [參考體系結構站點，以顯示如何在平台SPA標籤中實施](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
* [在Adobe Analytics跟蹤時使SPA用最佳做法](https://helpx.adobe.com/analytics/kt/using/spa-analytics-best-practices-feature-video-use.html)
* [用於本文的演示網站](https://aam.enablementadobe.com/SPA-Launch.html)
