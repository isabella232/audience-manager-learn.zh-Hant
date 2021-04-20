---
title: 從追蹤伺服器移轉至報表套裝層級伺服器端轉送
description: 本文和影片將示範如何啟用伺服器端轉送Analytics資料至報表套裝層級，而非追蹤伺服器層級的Audience Manager。
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
role: "Developer, Data Engineer"
level: Intermediate
exl-id: 08b81e52-a28a-43e4-a284-df2460a43016
translation-type: tm+mt
source-git-commit: 256edb05f68221550cae2ef7edaa70953513e1d4
workflow-type: tm+mt
source-wordcount: '580'
ht-degree: 0%

---

# 從[!UICONTROL Tracking Server]移轉至[!UICONTROL Report Suite]-Level [!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

本文和影片將示範如何啟用[!DNL Analytics]資料的[!UICONTROL server-side forwarding]在[!UICONTROL report suite]層級Audience Manager，而非在[!UICONTROL tracking server]層級。

## 簡介 {#introduction}

如果您有Adobe Audience Manager和Adobe Analytics，則可將[!DNL Analytics]資料的「[!UICONTROL Server-side Forwarding]」實作至Audience Manager。 這表示，您的頁面不會傳送2次點擊(一次至[!DNL Analytics]，一次至Audience Manager)，而只會傳送點擊至[!DNL Analytics]，而[!DNL Analytics]會將該資料轉送至Audience Manager。 如果您已啟動並執行此功能，且您在2017年10月之前已啟用／實作此功能，則您的[!UICONTROL server-side forwarding]可能是以您的&quot;[!UICONTROL Tracking Server]&quot;為基礎，必須由Adobe客戶服務或Adobe諮詢部門啟用。 從2017年10月開始，您現在可以自行設定[!UICONTROL server-side forwarding]，並在[!UICONTROL Report Suite]層級執行（轉寄PER [!UICONTROL Report Suite]）。 這有很大的好處，將在下面討論。

## [!UICONTROL Tracking Server] 轉發  {#tracking-server-forwarding}

您的[!UICONTROL tracking server]是您傳送[!DNL Analytics]資料的位置，也是影像要求和Cookie寫入所在的網域。 它應設定在DTM或[!DNL Experience Platform Launch]或[!DNL AppMeasurement.js]檔案中，且通常會如此，您的網站或企業名稱會取代&quot;mysite&quot;:

`s.trackingServer = "mysite.sc.omtrdc.net";`

如果[!UICONTROL server-side forwarding]設定為在[!UICONTROL tracking server]層級轉送，則傳送至此[!UICONTROL tracking server]的任何點擊(如果Experience CloudID服務也已啟用)都會轉送至Audience Manager。 這必須由Adobe客戶服務或Adobe諮詢部門啟用。 您已切換至[!UICONTROL report suite]轉送，但您也可以停用此功能，如下所述。

如果您不確定是否為您啟用[!DNL tracking server forwarding]，請聯絡Adobe客戶服務或Adobe諮詢，他們應該能通知您。

## [!UICONTROL Report Suite]-級別  [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

從[!UICONTROL tracking server]轉寄移至[!UICONTROL report suite]轉寄的最大優點之一，是您現在可以使用&quot;Audience Analytics&quot;，即將Audience Manager[!UICONTROL segments]轉寄回Adobe Analytics，以進行詳細的[!UICONTROL segment]分析。 如果您仍在[!UICONTROL tracking server]轉送，而不是[!UICONTROL report suite]轉送，則不支援這項好用的功能。 請參閱[documentation](https://marketing.adobe.com/resources/help/en_US/analytics/audiences/)中有關Audience Analytics的更多資訊。

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要提示{#additional-resources}

如上所述，一旦您將所有[!UICONTROL report suites]設為要轉發至Audience Manager，您應聯絡Adobe客戶服務或Adobe諮詢，並讓他們停用[!UICONTROL tracking server]轉寄。 執行此動作並非緊急事件，因為同時擁有[!UICONTROL tracking server]轉送和[!UICONTROL report suite]轉送不會導致重複點擊。 不過，最好只開啟[!UICONTROL report suite]轉發。 如果您保持[!UICONTROL tracking server]轉送狀態，不僅會轉送您不想轉送的[!UICONTROL report suites]資料，而且在您（及您公司的每個人）忘記[!UICONTROL tracking server]轉送已開啟後，您可能會認為資料未轉送特定[!UICONTROL report suite]（因為未在報表套裝層級開啟），但仍會轉送資料，因為[!UICONTROL tracking server]。 然後，您將浪費時間和金錢來瞭解轉送的原因，並支付您AAM未料到的伺服器呼叫費用。 因此，當所有[!UICONTROL report suites]設定為轉送時，最好立即停用[!UICONTROL tracking server]轉送，以符合您的業務需求。
