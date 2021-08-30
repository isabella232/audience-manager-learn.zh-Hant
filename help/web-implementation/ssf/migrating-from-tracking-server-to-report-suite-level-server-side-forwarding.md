---
title: 從追蹤伺服器移轉至報表套裝層級的伺服器端轉送
description: 本文和影片將示範如何啟用Analytics資料的伺服器端轉送，以便在報表套裝層級（而非追蹤伺服器層級）Audience Manager。
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
source-git-commit: 4d4c12e9f9a33760a89460258c3802fcf3a4e22b
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 0%

---

# 從[!UICONTROL Tracking Server]移轉至[!UICONTROL Report Suite]-Level [!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

本文和影片將示範如何啟用[!DNL Analytics]的[!UICONTROL server-side forwarding]資料以在[!UICONTROL report suite]層級Audience Manager，而非在[!UICONTROL tracking server]層級。

## 簡介 {#introduction}

如果您有Adobe Audience Manager AND Adobe Analytics，則可以實作[!DNL Analytics]資料的「[!UICONTROL Server-side Forwarding]」以Audience Manager。 這表示，與其讓頁面傳送2次點擊(一次至[!DNL Analytics]，一次至Audience Manager)，它只需傳送點擊至[!DNL Analytics]，而[!DNL Analytics]會將該資料轉送至Audience Manager。 如果您已在2017年10月前啟用/實作此功能，則您的[!UICONTROL server-side forwarding]可能會以您的「[!UICONTROL Tracking Server]」為基礎，而「」必須由Adobe客戶服務或Adobe諮詢啟用。 自2017年10月起，您現在可以自行設定[!UICONTROL server-side forwarding]，並在[!UICONTROL Report Suite]層級執行（根據PER [!UICONTROL Report Suite]轉送）。 這將帶來重大好處，將於下文討論。

## [!UICONTROL Tracking Server] 轉送 {#tracking-server-forwarding}

您的[!UICONTROL tracking server]是您傳送[!DNL Analytics]資料的位置，也是寫入影像要求和Cookie的網域。 應在DTM或[!DNL Experience Platform Launch]或[!DNL AppMeasurement.js]檔案中設定，且通常會如下所示，您的網站或企業名稱會取代「mysite」：

`s.trackingServer = "mysite.sc.omtrdc.net";`

如果在[!UICONTROL tracking server]層級將[!UICONTROL server-side forwarding]設為前進，則傳送至此[!UICONTROL tracking server]的任何點擊(如果也啟用了Experience CloudID服務)將轉送至Audience Manager。 這必須由Adobe客戶服務或Adobe諮詢啟用。 在您切換至[!UICONTROL report suite]轉送後，也可以停用該功能，如下所述。

如果您不確定是否為您啟用了[!DNL tracking server forwarding]，請聯絡Adobe客戶服務或Adobe諮詢，他們應該能通知您。

## [!UICONTROL Report Suite]-層級 [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

從[!UICONTROL tracking server]轉送移至[!UICONTROL report suite]轉送的最大優點之一，是您現在可以使用「Audience Analytics」，即將Audience Manager[!UICONTROL segments]轉送回Adobe Analytics，以進行詳細的[!UICONTROL segment]分析。 如果您仍在[!UICONTROL tracking server]轉送，而非[!UICONTROL report suite]轉送，則「不」支援這項絕佳功能。 請參閱[檔案](https://experienceleague.adobe.com/docs/analytics/integration/audience-analytics/mc-audiences-aam.html)中有關Audience Analytics的詳細資訊。

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要提示 {#additional-resources}

如上述影片所述，一旦您將所有[!UICONTROL report suites]設為轉送至Audience Manager，您應聯絡Adobe客戶服務或Adobe諮詢，並讓他們停用[!UICONTROL tracking server]轉送。 執行此作業並非緊急情況，因為同時具有[!UICONTROL tracking server]轉送和[!UICONTROL report suite]轉送並不會導致重複點擊。 不過，最佳實務是只開啟[!UICONTROL report suite]轉送。 如果您保留[!UICONTROL tracking server]轉送開啟，不僅會轉送您不想轉送的[!UICONTROL report suites]資料，未來當您（及您公司的所有人）忘記[!UICONTROL tracking server]轉送已開啟後，您可能會認為資料並未針對特定[!UICONTROL report suite]進行轉送（因為未在報表套裝層級開啟），但資料仍會因為[!UICONTROL tracking server]而轉送。 然後，您會浪費時間和金錢，弄明白為何會轉送，並支付您未料到的AAM伺服器呼叫費用。 因此，當您將所有[!UICONTROL report suites]設定為轉發時，最好立即禁用[!UICONTROL tracking server]轉發，這對您的業務需求有意義。
