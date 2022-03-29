---
title: 從跟蹤伺服器遷移到報告套件級伺服器端轉發
description: 瞭解如何在報告套件級別而不是跟蹤伺服器級別啟用Adobe Analytics資料的伺服器端轉發到Audience Manager。
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: Developer, Data Engineer
level: Intermediate
exl-id: 08b81e52-a28a-43e4-a284-df2460a43016
source-git-commit: 4adaade180545bcf5f911b7453a7e9939e2ed178
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 0%

---

# 從跟蹤伺服器遷移到報告套件級伺服器端轉發 {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

本文和視頻將向您展示如何啟用 [!DNL Analytics] 要Audience Manager的資料 [!UICONTROL report suite] 級別，而不是 [!UICONTROL tracking server] 級別。

## 簡介 {#introduction}

如果您有Adobe Audience Manager和Adobe Analytics，則可以實施 [!DNL Analytics] 資料到Audience Manager。 這意味著，您的頁面不會發送兩個點擊(一到 [!DNL Analytics] 和一個Audience Manager)，它可以 [!DNL Analytics], [!DNL Analytics] 將資料轉發給Audience Manager。

如果您已經啟動並運行了此功能，並且在2017年10月之前啟用/實施了此功能，則您的伺服器端轉發可能基於您的 [!UICONTROL Tracking Server]必須由「Adobe客戶服務」或「Adobe咨詢」來啟用。 截至2017年10月，您現在可以自行配置伺服器端轉發，並以報告套件級別（每個報告套件的轉發）執行。 這有很大的好處，如下所述。

## [!UICONTROL Tracking server] 轉發 {#tracking-server-forwarding}

您 [!UICONTROL tracking server] 是您發送到的位置 [!DNL Analytics] 以及寫入映像請求和cookie的域。 應在DTM或 [!DNL Experience Platform Launch]，或 [!DNL AppMeasurement.js] 檔案，並且通常會這樣顯示，您的站點或業務名稱將替換「mysite」：

`s.trackingServer = "mysite.sc.omtrdc.net";`

如果將伺服器端轉發設定為在 [!UICONTROL tracking server] 級別，發送到此的任何命中 [!UICONTROL tracking server] (如果Experience CloudID服務也已啟用)將轉發到Audience Manager。 這必須通過Adobe客戶服務或Adobe咨詢來實現。 他們也可以禁用它，在您切換到 [!UICONTROL report suite] 轉發，如下所述。

如果你不確定 [!DNL tracking server forwarding] 已為您啟用，請與Adobe客戶服務或Adobe咨詢聯繫，他們應該能夠通知您。

## [!UICONTROL Report-suite] — 級伺服器端轉發 {#report-suite-level-server-side-forwarding}

遷移到 [!UICONTROL report suite] 轉發 [!UICONTROL tracking server] 轉發是您現在可以使用「Audience Analytics」，即轉發Audience Manager [!UICONTROL segments] 回Adobe Analytics進行詳細分部分析。 如果您仍在運行，則不支援此功能 [!UICONTROL tracking server] 轉發 [!UICONTROL report suite] 轉發。 請參閱中有關Audience Analytics的詳細資訊 [文檔](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html)。

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要提示 {#additional-resources}

如上所述，一旦您擁有 [!UICONTROL report suites] 設定為轉發到Audience Manager，您應聯繫Adobe客戶服務或Adobe咨詢，並讓他們禁用 [!UICONTROL tracking server] 轉發。 你這樣做不是緊急情況，因為有兩者 [!UICONTROL tracking server] 轉發 [!UICONTROL report suite] 轉發不會導致重複命中。 但是，最好的做法是 [!UICONTROL report suite] 正在轉發。

如果你離開 [!UICONTROL tracking server] 轉發時，它不僅可以轉發資料 [!UICONTROL report suites] 你不想轉發，但在你（和你公司的所有人）忘記 [!UICONTROL tracking server] 轉發已開啟，您可能認為資料不是針對特定的 [!UICONTROL report suite]。 這是因為在報表套件級別未啟用它，但仍在轉發資料，因為 [!UICONTROL tracking server]。 然後，您將浪費時間和金錢去弄明白它為什麼要轉發，同時還會為您沒AAM料到的伺服器呼叫付費。 所以，最好把 [!UICONTROL tracking server] 只要你擁有了 [!UICONTROL report suites] 為了滿足您的業務需要，我們將推出這種方案。
