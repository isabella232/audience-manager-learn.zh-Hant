---
title: 從追蹤伺服器移轉至報表套裝層級的伺服器端轉送
description: 瞭解如何啟用Adobe Analytics資料的伺服器端轉送，以便在報表套裝層級Audience Manager，而非追蹤伺服器層級。
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

# 從追蹤伺服器移轉至報表套裝層級的伺服器端轉送 {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

本文和影片將說明如何啟用伺服器端轉送 [!DNL Analytics] 要在中Audience Manager的資料 [!UICONTROL report suite] 層級而非 [!UICONTROL tracking server] 層級。

## 簡介 {#introduction}

如果您有Adobe Audience Manager和Adobe Analytics，您可以實作的 [!DNL Analytics] 要Audience Manager的資料。 這表示，您的頁面不會傳送兩個點選(一個到 [!DNL Analytics] 和1個以Audience Manager)，則可將點選傳送至 [!DNL Analytics]、和 [!DNL Analytics] 會將資料轉送至Audience Manager。

如果您已啟動並執行此專案，且已在2017年10月前啟用/實作此專案，則您的伺服器端轉送可能會根據 [!UICONTROL Tracking Server]，此專案必須由Adobe客戶服務或Adobe諮詢啟用。 自2017年10月起，您現在可以自行設定伺服器端轉送，並在報表套裝層級進行（每個報表套裝的轉送）。 這有許多優點，如下所述。

## [!UICONTROL Tracking server] 轉送 {#tracking-server-forwarding}

您的 [!UICONTROL tracking server] 是您傳送至的位置 [!DNL Analytics] 資料，以及寫入影像要求和Cookie的網域。 應在DTM中設定或 [!DNL Experience Platform Launch]，或中的 [!DNL AppMeasurement.js] 檔案，通常看起來像這樣，您的網站或企業名稱會取代「mysite」：

`s.trackingServer = "mysite.sc.omtrdc.net";`

如果伺服器端轉送設定為在以下位置轉送： [!UICONTROL tracking server] 層級，任何傳送至此的點選 [!UICONTROL tracking server] (如果Experience CloudID服務也啟用)將轉送到Audience Manager。 必須由Adobe客戶服務或Adobe諮詢啟用此功能。 在您切換至後，他們也可以將其停用 [!UICONTROL report suite] 轉送，如下所述。

如果您不確定如果 [!DNL tracking server forwarding] 已為您啟用，請聯絡Adobe客戶服務或Adobe諮詢，他們應該能夠通知您。

## [!UICONTROL Report-suite] — 層級伺服器端轉送 {#report-suite-level-server-side-forwarding}

遷移至以下網站的最大優點之一： [!UICONTROL report suite] 轉寄來源 [!UICONTROL tracking server] 轉送是指您現在可以使用「Audience Analytics」，也就是轉送Audience Manager的功能 [!UICONTROL segments] 返回Adobe Analytics以取得詳細的區段分析。 如果您仍在，則不支援這項絕佳功能 [!UICONTROL tracking server] 轉送而非 [!UICONTROL report suite] 轉送。 請參閱中有關Audience Analytics的詳細資訊 [檔案](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html).

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要提示 {#additional-resources}

如上述影片所述，一旦您擁有 [!UICONTROL report suites] 若設為「轉寄」而您希望轉寄至「Audience Manager」，則您應聯絡Adobe客戶服務或Adobe諮詢，要求他們停用 [!UICONTROL tracking server] 轉送。 您這麼做並非緊急，因為兩者皆有 [!UICONTROL tracking server] 轉送和 [!UICONTROL report suite] 轉送不會產生重複的點選。 不過，最佳實務是只要 [!UICONTROL report suite] 啟用轉送。

如果您離開 [!UICONTROL tracking server] 轉送功能開啟，不僅可以從以下來源轉送資料 [!UICONTROL report suites] 您不想轉寄，但在未來，在您（以及您公司的每個人）忘記了這點之後 [!UICONTROL tracking server] 轉送功能已開啟，您可能會認為資料未轉送特定的 [!UICONTROL report suite]. 這是因為並未在報表套裝層級開啟此功能，但資料仍會因為 [!UICONTROL tracking server]. 然後，您將會浪費時間和金錢來找出轉送的原因，並支付您未預期的AAM伺服器呼叫。 因此，最好是停用 [!UICONTROL tracking server] 一旦您擁有所有 [!UICONTROL report suites] 根據您的業務需求，積極推進這項工作。
