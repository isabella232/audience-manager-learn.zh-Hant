---
title: 更新至Adobe Audience Manager DIL 8.0版（或更新版本）
description: 本文會提供將Adobe Audience Manager (AAM)Data Integration Library(DIL)程式碼更新至8.0版或更新版本的步驟和建議。 這指的是「使用者端」DIL實作，而不是Adobe Analytics資料的伺服器端轉送，將涵蓋無Adobe標籤管理程式解決方案的DTM、Launch by Adobe和實作。
feature: DIL Implementation
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: Developer, Data Engineer
level: Intermediate
exl-id: 8c1e6ed5-0f21-427b-a681-0ecb020a0e60
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '1122'
ht-degree: 1%

---

# 更新至Adobe Audience Manager的DIL版本8.0 （或更新版本） {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

本文將提供更新Adobe Audience Manager (AAM)的步驟和建議 [!DNL Data Integration Library] (DIL)程式碼至8.0版或更新版本。 這指的是「使用者端」DIL實作，而不是Adobe Analytics資料的伺服器端轉送，將涵蓋無Adobe標籤管理程式解決方案的DTM、Launch by Adobe和實作。

## 概述 {#overview}

Audience Manager的 [!DNL Data Integration Library] (DIL)程式碼可讓您在網站上實作AAM*。 實作舊版DIL時，不需要同時實作Adobe的Experience CloudID服務(ECID) （不過這是個好主意）。 從DIL 8.0版開始，硬性相依於ECID 3.3版或更新版本。 若您實作DIL8.0或更新版本，但未使用ECID 3.3或較舊的版本，您會收到錯誤訊息，而系統無法運作。 由於您可以實作AAM的方法有很多種，因此我們建立了此頁面，為您提供一些要完成的步驟以及一些建議。 您將會在下方找到依平台/實作方法劃分的這些步驟和建議。 有關DIL的更多資訊，請參閱 [檔案](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en).

* 如本頁面的說明中所述，這僅涵蓋「使用者端」DIL實施，由沒有Adobe Analytics的AAM客戶使用。 如果您有Adobe Analytics，應使用實作AAM的伺服器端轉送方法。 此方法的說明請參閱 [檔案](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html).

## 重複和已棄用的元素和方法 {#duplicate-and-deprecated-elements-and-methods}

在舊版DIL和ECID中，有些重複方法(在DIL和ECID中執行相同功能的方法)會導致使用者不知道該使用哪一種方法。 通常您需要同時使用這兩個變數並將其比對，而該訊息未妥善傳達給我們的客戶。 從DIL8.0開始，DIL已棄用這些重複的方法和元素，建議您使用ECID版本。

例如：

* 使用時 [!DNL DIL.create]，一些元素已過時，您應改用ECID元素。 這些元素會在 [[!DNL DIL.create] 檔案](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html).
* 此 [!DNL idSync] 執行個體層級方法也已過時，方法的 [檔案](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html).

## 與客戶ID同步的ID {#id-syncing-with-a-customer-id}

在AAM中，您可以將電腦上的UUID （匿名不重複使用者ID）與客戶ID同步，以便上傳有關該客戶的離線資料，並將其與其線上行為繫結，進而加深對客戶的瞭解。 在過去，這是以兩種方式之一完成的：

* 此 [!DNL idSync] 執行個體層級方法
* 此 [!DNL declaredId] 中的元素 [!DNL DIL.create]

如果您一直使用其中一種舊方法來與客戶ID同步，強烈建議您使用 [!DNL setCustomerIDs] 方法，是ECID服務的一部分。 更多關於的資訊 [!DNL setCustomerIDs] 可在方法的下列位置取得： [檔案](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html).

**快速提示：** 先前使用上述任一種方法時，您已參考AAM [!UICONTROL Data Source] 使用 [!UICONTROL Data Source] ID （亦稱為「DPID」）。 更新至 [!DNL setCustomerIDs]，您需要使用AAM [!UICONTROL Data Source]的&quot;[!UICONTROL Integration Code]」而非。 仍然指向相同的 [!UICONTROL Data Source] 但只是不同的識別碼。 這如下影片所示。

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

以下小節列出根據您的實作方法更新至DIL8.0的步驟和建議：

## 更新至Adobe Experience Platform標籤中的DIL8.0 {#updating-to-dil-in-experience-platform-launch}

更新至DIL8.0的基本步驟

1. 如果您使用8.0之前的DIL，請在升級前進入AAM擴充功能中的DIL設定，並記下您使用的任何進階選項（用於後續步驟）。
1. 將您的AAM擴充功能更新至8.0版或更新版本。
1. 確認您的Experience CloudID服務擴充功能為3.3.0版或更新版本。
1. 適用於任何已棄用的方法/元素(例如 `disableIDSyncs`)，請在AAM 8.0之前的擴充功能或用於DIL的自訂程式碼中啟用ECID擴充功能中的ECID方法。

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`
   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`
   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `dSyncSSLUseAkamai`
   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

1. 發佈變更。

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## 更新至Adobe DTM中的DIL8.0 {#updating-to-dil-in-adobe-dtm}

1. 將AAM工具更新至8.0版或更新版本。 此版本設定位於AAM工具的「一般」區段下。
1. 適用於任何已棄用的方法/元素(例如 `disableIDSyncs`)，請記下它們（以便將其新增至ECID工具），然後從自訂中移除它們，以便DIL8.0之前的AAM工具 [!DNL DIL code] 在AAM工具中。
1. 將您的Experience CloudID服務擴充功能更新至3.3.0版或更新版本
1. 新增進階選項至您從AAM工具自訂程式碼移除的ECID工具。
1. 發佈變更

## 更新至DIL8.0 (無AdobeTag Management解決方案) {#additional-resources}

如果您直接在頁面上更新程式碼，則可以使用較新的專案來取代較舊的專案，除非您需要將方法/元素從DIL移動到ECID，如上所述。 在這種情況下，您只需以ECID位置的ECID方法/元素取代DIL位置中的舊方法/元素即可。

非Adobe標籤管理員的情況也一樣。 只要您有該標籤管理解決方案中的舊版本，請依照以下步驟所述，以新程式碼取代。

1. 將您的DIL程式庫更新至最新版本（8.0或更新版本） — 您需要從Adobe諮詢或Adobe客戶服務取得最新的DIL程式碼，因為它目前無法在公共位置使用。 然後只需用新的DIL程式庫程式碼取代舊的DIL程式庫程式碼，並繼續進行下一個步驟（不要現在停止，否則您會遇到問題，ha）。
1. 安裝 [!DNL ECID Service] 或將您現有的版本更新至3.3.0或更新版本。 您可以下載最新的Experience CloudID服務版本 [從我們的GitHub頁面](https://github.com/Adobe-Marketing-Cloud/id-service/releases). 如果您需要這方面的協助，請參閱 [檔案](https://experienceleague.adobe.com/docs/id-service/using/home.html) 或諮詢Adobe顧問。

1. 確認自訂程式碼中用於DIL的任何已棄用方法或元素已移至ECID方法：

   1. (DIL) `disableDestinationPublishingIframe` -> (ECID) `disableIdSyncs`

      [文件](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `disableIDSyncs` -> (ECID) `disableIdSyncs`

      [文件](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `iframeAkamaiHTTPS` -> (ECID) `idSyncSSLUseAkamai`

      [文件](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)

   1. (DIL) `declaredId` -> (ECID) `setCustomerIDs`

      [文件](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)
