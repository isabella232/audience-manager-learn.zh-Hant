---
title: 從追蹤伺服器移轉至報表套裝層級伺服器端轉送
description: 本文和影片將示範如何在報表套裝層級而非在追蹤伺服器層級啟用Analytics資料的伺服器端轉送至Audience Manager。
product: audience manager, analytics
feature: integration with analytics
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1776
translation-type: tm+mt
source-git-commit: dfd549508cc223714bdb07ac6fd2aa31e6ca5586
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 0%

---


# 從[!UICONTROL Tracking Server]移轉至[!UICONTROL Report Suite]-Level [!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

本文和影片將示範如何在[!UICONTROL report suite]層級而非[!UICONTROL tracking server]層級啟用[!DNL Analytics]資料至Audience Manager的[!UICONTROL server-side forwarding]。

## 簡介 {#introduction}

如果您有Adobe Audience Manager和Adobe Analytics，您可以將[!DNL Analytics]資料的「[!UICONTROL Server-side Forwarding]」實作至Audience Manager。 這表示，您的頁面不會傳送2個點擊（一個到[!DNL Analytics]，另一個到Audience Manager），而只是傳送點擊到[!DNL Analytics]，而[!DNL Analytics]會將該資料轉送至Audience Manager。 如果您已啟動並執行此功能，且您在2017年10月之前已啟用／實作此功能，則您的[!UICONTROL server-side forwarding]可能是以您的「[!UICONTROL Tracking Server]」為基礎，必須由Adobe客戶服務或Adobe諮詢啟用。 從2017年10月開始，您現在可以自行設定[!UICONTROL server-side forwarding]，並在[!UICONTROL Report Suite]層級執行（轉寄PER [!UICONTROL Report Suite]）。 這有很大的好處，將在下面討論。

## [!UICONTROL Tracking Server] 轉發  {#tracking-server-forwarding}

您的[!UICONTROL tracking server]是您傳送[!DNL Analytics]資料的位置，也是影像要求和Cookie寫入所在的網域。 它應設定在DTM或[!DNL Experience Platform Launch]或[!DNL AppMeasurement.js]檔案中，且通常會如此，您的網站或企業名稱會取代&quot;mysite&quot;:

`s.trackingServer = "mysite.sc.omtrdc.net";`

如果[!UICONTROL server-side forwarding]設定為在[!UICONTROL tracking server]層級轉送，則傳送至此[!UICONTROL tracking server]的任何點擊（如果也啟用Experience Cloud ID服務）都會轉送至Audience Manager。 這必須由Adobe客戶服務或Adobe諮詢服務啟用。 您已切換至[!UICONTROL report suite]轉送，但您也可以停用此功能，如下所述。

如果您不確定是否為您啟用[!DNL tracking server forwarding]，請聯絡Adobe客戶服務或Adobe諮詢，他們應該能通知您。

## [!UICONTROL Report Suite]-級別  [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

從[!UICONTROL tracking server]轉送移至[!UICONTROL report suite]轉送的最大優點之一，就是您現在可以使用「Audience Analytics」，即將Audience Manager [!UICONTROL segments]轉送回Adobe Analytics，以進行詳細的[!UICONTROL segment]分析。 如果您仍在[!UICONTROL tracking server]轉送，而不是[!UICONTROL report suite]轉送，則不支援這項好用的功能。 請參閱[檔案](https://marketing.adobe.com/resources/help/en_US/analytics/audiences/)中有關觀眾分析的詳細資訊。

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要提示{#additional-resources}

如上述影片所述，當您將所有[!UICONTROL report suites]設為轉寄至Audience Manager時，您應聯絡Adobe客戶服務或Adobe諮詢，並讓他們停用[!UICONTROL tracking server]轉寄。 執行此動作並非緊急事件，因為同時擁有[!UICONTROL tracking server]轉送和[!UICONTROL report suite]轉送不會導致重複點擊。 不過，最好只開啟[!UICONTROL report suite]轉發。 如果您保持[!UICONTROL tracking server]轉送狀態，不僅會轉送您不想轉送的[!UICONTROL report suites]資料，而且在您（及您公司的每個人）忘記[!UICONTROL tracking server]轉送已開啟後，您可能會認為資料未轉送特定[!UICONTROL report suite]（因為未在報表套裝層級開啟），但仍會轉送資料，因為[!UICONTROL tracking server]。 然後，您將浪費時間和金錢來瞭解轉送的原因，並支付您意想不到的AAM伺服器呼叫費用。 因此，當所有[!UICONTROL report suites]設定為轉送時，最好立即停用[!UICONTROL tracking server]轉送，以符合您的業務需求。
