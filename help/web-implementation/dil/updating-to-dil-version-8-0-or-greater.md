---
title: 更新至Adobe Audience ManagerDIL8.0版（或更新版本）
description: 本文將提供將Adobe Audience Manager(AAM)Data Integration Library(DIL)程式碼更新至8.0版或更新版本的步驟與建議。 這是指「用戶端」DIL實作，而非Adobe Analytics資料的伺服器端轉送，其中將涵蓋DTM、Launch by Adobe和實作，且沒有Adobe標籤管理解決方案。
feature: DIL Implementation
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
role: Developer, Data Engineer
level: Intermediate
exl-id: 8c1e6ed5-0f21-427b-a681-0ecb020a0e60
source-git-commit: 4d4c12e9f9a33760a89460258c3802fcf3a4e22b
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 1%

---

# 更新至Adobe Audience Manager的DIL8.0版（或更新版本） {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

本文將提供將Adobe Audience Manager(AAM)[!DNL Data Integration Library](DIL)程式碼更新至8.0版或更新版本的步驟和建議。 這是指「用戶端」DIL實作，而非Adobe Analytics資料的伺服器端轉送，其中將涵蓋DTM、Launch by Adobe和實作，且沒有Adobe標籤管理解決方案。

## 概述 {#overview}

Audience Manager的[!DNL Data Integration Library](DIL)代碼可讓您在網站上實作AAM*。 實作舊版DIL時，不需要也實作Adobe的Experience CloudID服務(ECID)（雖然這是個好主意）。 從DIL8.0版開始，您必須嚴格依賴ECID 3.3版或更新版本。 如果您在未使用ECID 3.3或舊版的情況下實作DIL8.0或更新版本，您會收到錯誤，但無法運作。 由於您有多種實作AAM的方式，因此我們建立了此頁面，提供一些逐步及一些建議。 以下是依平台/實作方法劃分的這些步驟和建議。 [檔案](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en)中提供有關DIL的詳細資訊。

* 如本頁說明所述，這僅涵蓋「用戶端」DIL實作，這些實作供沒有Adobe Analytics的AAM客戶使用。 如果您有Adobe Analytics，則應使用實作AAM的伺服器端轉送方法。 此方法在[documentation](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html)中有說明。

## 重複和已棄用的元素和方法 {#duplicate-and-deprecated-elements-and-methods}

在舊版DIL和ECID中，有重複方法(在DIL和ECID中都執行相同功能的方法)，這造成了使用方法的混淆。 通常，您需要同時使用兩者並加以比對，而且該訊息並未妥善傳達給客戶。 從DIL8.0開始，DIL中已棄用這些重複方法和元素，建議您使用ECID版本。

例如：

* 使用[!DNL DIL.create]時，已棄用一些元素，您應改用ECID元素。 這些元素在[[!DNL DIL.create] documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)中調用。
* [!DNL idSync]例項層級方法也已淘汰，如方法的[documentation](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html)中所呼叫。

## 與客戶ID同步的ID {#id-syncing-with-a-customer-id}

在AAM中，您可以將電腦上的UUID（匿名的不重複使用者ID）與客戶ID同步，以便上傳有關該客戶的離線資料，並將其與其線上行為關聯，以便更清楚了解您的客戶。 過去，這種做法有兩種：

* [!DNL idSync]實例級方法
* [!DNL DIL.create]中的[!DNL declaredId]元素

如果您使用其中一種舊方法來與客戶ID同步，強烈建議您使用[!DNL setCustomerIDs]方法（此方法屬於ECID服務的一部分）來更新。 有關[!DNL setCustomerIDs]的詳細資訊，請參閱方法的[documentation](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)。

**快速提示：** 先前使用上述任一方法時，您使用 [!UICONTROL Data Source] ID [!UICONTROL Data Source] 參照AAM（亦稱為「DPID」）。更新至[!DNL setCustomerIDs]時，您需要改用AAM [!UICONTROL Data Source]的「[!UICONTROL Integration Code]」。 它仍指向相同的[!UICONTROL Data Source]，但只是不同的識別碼。 這如以下影片所示。

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

以下小節列出根據您的實作方法，更新至DIL8.0的步驟和建議：

## 更新至Adobe Experience Platform Launch中的DIL8.0 {#updating-to-dil-in-experience-platform-launch}

更新至DIL8.0的基本步驟

1. 如果您使用的是8.0版之前的DIL，在升級前，請前往AAM擴充功能中的DIL設定，並記下您使用的任何進階選項（以用於後續步驟）
1. 將AAM擴充功能更新至8.0版或更新版本
1. 確認您的Experience CloudID服務擴充功能是3.3.0版或更新版本
1. 若您的AAM 8.0之前擴充功能或自訂程式碼中有任何已棄用的方法/元素（例如「[!DNL disableIDSyncs]」）需DIL，請在ECID擴充功能中啟用ECID方法。

   1. (DIL)disableDestinationPublishingIframe ->(ECID)disableIdSyncs
   1. (DIL)disableIDSyncs ->(ECID)disableIdSyncs
   1. (DIL)iframeAkamaiHTTPS ->(ECID)dSyncSSLUseAkamai
   1. (DIL)delacedId ->(ECID)setCustomerIDs

1. 發佈變更

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## 更新至DILDTM中的Adobe8.0 {#updating-to-dil-in-adobe-dtm}

1. 將AAM工具更新至8.0版或更新版本。 此版本設定位於AAM工具的「一般」區段下。
1. 對於8.0版之前AAM工具的自訂程式碼中用於DIL的任何已棄用的方法/元素（例如「disableIDSyncs」），請記下這些方法/元素（以便您將其新增至ECID工具），然後從AAM工具的自訂[!DNL DIL code]中移除這些方法/元素。
1. 將您的Experience CloudID服務擴充功能更新至3.3.0版或更新版本
1. 將進階選項新增至您從AAM工具的自訂程式碼中移除的ECID工具。
1. 發佈變更

## 更新至DIL8.0(沒有Adobe標籤管理解決方案) {#additional-resources}

如果您直接在頁面上更新程式碼，則只需將舊項目取代為較新項目，除非您需要將方法/元素從DIL移動至ECID，如上所述。 在此情況下，您只需以ECID位置的ECID方法/元素取代DIL位置的舊方法/元素即可。

非Adobe標籤管理程式也是如此。 無論您在該標籤管理解決方案中有哪個舊版本，請依照下列步驟以新程式碼取代。

1. 將您的DIL程式庫更新至最新版本（8.0或更新版本） — 您需要從Adobe諮詢或Adobe客戶服務取得最新的DIL程式碼，因為它目前未在公開位置提供。 然後，只需將舊的DIL程式庫程式碼取代為新的DIL程式庫程式碼，然後繼續執行下一個步驟（不要立即停止，否則您會遇到問題，哈）。
1. 安裝[!DNL ECID Service]或將現有版本更新為3.3.0或更高版本。 您可以從GitHub頁面](https://github.com/Adobe-Marketing-Cloud/id-service/releases)下載最新的Experience CloudID服務版本[。 若您需要相關協助，請參閱[檔案](https://experienceleague.adobe.com/docs/id-service/using/home.html)或與Adobe顧問對談。

1. 確認您自訂程式碼中要DIL的任何已棄用方法或元素已移至ECID方法：

   1. (DIL)disableDestinationPublishingIframe ->(ECID)disableIdSyncs

      [文件](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL)disableIDSyncs ->(ECID)disableIdSyncs

      [文件](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL)iframeAkamaiHTTPS ->(ECID)idSyncSSLUseAkamai

      [文件](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)

   1. (DIL)delacedId ->(ECID)setCustomerIDs

      [文件](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)
