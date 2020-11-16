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


# 從級 [!UICONTROL Tracking Server] 別移 [!UICONTROL Report Suite]轉 [!UICONTROL Server-Side Forwarding] {#migrating-from-tracking-server-to-report-suite-level-server-side-forwarding}

本文和影片將示範如何在層 [!UICONTROL server-side forwarding] 級而 [!DNL Analytics] 非層級上啟用 [!UICONTROL report suite] 「資料」至Audience Manager [!UICONTROL tracking server] 。

## 簡介 {#introduction}

如果您有Adobe Audience Manager和Adobe Analytics，您可以將資料「[!UICONTROL Server-side Forwarding]」實作 [!DNL Analytics] 至Audience Manager。 這表示，您的頁面不會傳送2次點擊(一次到Audience Manager, [!DNL Analytics] 一次到Audience Manager)，而只是傳送點擊 [!DNL Analytics], [!DNL Analytics] 並將該資料轉送至Audience Manager。 如果您已啟動並執行此功能，且如果您在2017年10月之前已啟用／實作，則您的 [!UICONTROL server-side forwarding] 「[!UICONTROL Tracking Server]」可能是以您的「」為基礎，而「」必須由Adobe客戶服務或Adobe Consulting啟用。 從2017年10月開始，您現在可以自行 [!UICONTROL server-side forwarding] 設定，並依等級( [!UICONTROL Report Suite] 轉送PER [!UICONTROL Report Suite])執行。 這有很大的好處，將在下面討論。

## [!UICONTROL Tracking Server] 轉發 {#tracking-server-forwarding}

您 [!UICONTROL tracking server] 的位置是您傳送資料的 [!DNL Analytics] 位置，也是影像要求和Cookie寫入所在的網域。 它應設定在DTM或檔 [!DNL Experience Platform Launch]案中， [!DNL AppMeasurement.js] 且通常會如此，您的網站或企業名稱會取代「mysite」:

`s.trackingServer = "mysite.sc.omtrdc.net";`

如 [!UICONTROL server-side forwarding] 果設定為在層級轉 [!UICONTROL tracking server] 送，任何傳送至此的點擊( [!UICONTROL tracking server] 如果也啟用Experience Cloud ID服務)都會轉送至Audience Manager。 這必須由Adobe客戶服務或Adobe諮詢服務啟用。 您已切換至轉發，但您也可以停用此功能，如 [!UICONTROL report suite] 下所述。

如果您不確定是否 [!DNL tracking server forwarding] 已為您啟用，請聯絡Adobe客戶服務或Adobe諮詢，他們應該能通知您。

## [!UICONTROL Report Suite]-級別 [!UICONTROL Server-Side Forwarding] {#report-suite-level-server-side-forwarding}

從轉送轉送轉送的最大優點 [!UICONTROL report suite] 之 [!UICONTROL tracking server] 一，就是您現在可以使用「Audience Analytics」，即將Audience Manager轉送回Adobe Analytics以進行詳細 [!UICONTROL segments][!UICONTROL segment] 分析。 如果您仍在轉送，而未轉送，則不支 [!UICONTROL tracking server] 援這項好 [!UICONTROL report suite] 功能。 請參閱檔案中有關觀眾分析的更多 [資訊](https://marketing.adobe.com/resources/help/en_US/analytics/audiences/)。

>[!VIDEO](https://video.tv.adobe.com/v/23701/?quality=12)

## 重要提示 {#additional-resources}

如上述影片所述，一旦您設定好要轉寄給Audience Manager的所 [!UICONTROL report suites] 有項目，您應聯絡Adobe客戶服務或Adobe諮詢，並讓他們停用轉寄功 [!UICONTROL tracking server] 能。 您不是緊急狀況，因為同時進行轉送和轉送 [!UICONTROL tracking server] 不會 [!UICONTROL report suite] 造成重複點擊。 但是，最好的做法是只開啟 [!UICONTROL report suite] 轉發。 如果您保 [!UICONTROL tracking server] 留轉送功能，不僅會轉送您不想轉送的資料，而且在您（及您公司的每個人）忘記轉送已開啟後，您可能會認為資料未轉送特定 [!UICONTROL report suites] （因為報表套裝層級未開啟），但資料仍會因為轉送而轉送 [!UICONTROL tracking server][!UICONTROL report suite][!UICONTROL tracking server]。 然後，您將浪費時間和金錢來瞭解轉送的原因，並支付您意想不到的AAM伺服器呼叫費用。 因此，當所有符合您業務需 [!UICONTROL tracking server] 求的轉寄設定一經推出，就 [!UICONTROL report suites] 可立即停用轉寄。
