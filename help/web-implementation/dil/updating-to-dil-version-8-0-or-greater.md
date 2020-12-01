---
title: 更新至Adobe Audience Manager的DIL 8.0版（或更新版本）
description: 本文將提供有關將Adobe Audience Manager(AAM)資料整合庫(DIL)程式碼更新至8.0版或更新版本的步驟和建議。 這是指「用戶端」DIL實作，而非伺服器端轉送Adobe Analytics資料，並涵蓋DTM、Launch by Adobe和不需Adobe標籤管理解決方案的實作。
feature: dil implementation
topics: null
audience: implementer
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 1841
translation-type: tm+mt
source-git-commit: dfd549508cc223714bdb07ac6fd2aa31e6ca5586
workflow-type: tm+mt
source-wordcount: '1149'
ht-degree: 1%

---


# 更新至Adobe Audience Manager的DIL 8.0版（或更新版本）{#updating-to-adobe-audience-manager-s-dil-version-or-greater}

本文將提供有關將Adobe Audience Manager(AAM)[!DNL Data Integration Library](DIL)程式碼更新為8.0版或更新版本的步驟和建議。 這是指「用戶端」DIL實作，而非伺服器端轉送Adobe Analytics資料，並涵蓋DTM、Launch by Adobe和不需Adobe標籤管理解決方案的實作。

## 概述 {#overview}

Audience Manager的[!DNL Data Integration Library](DIL)程式碼可讓您在您的網站*上實作AAM。 在實作舊版DIL時，不需要也實作Adobe的Experience Cloud ID Service(ECID)（雖然這是個好主意）。 從DIL 8.0版開始，ECID 3.3版或更新版本已受到嚴格依賴。 如果您實作DIL 8.0或更新版本而未使用ECID 3.3，或使用舊版，您會收到錯誤，而且它無法運作。 由於您有多種實作AAM的方式，因此我們建立此頁面，以提供您一些要執行的步驟，以及一些建議。 以下是依平台／實作方法劃分的步驟和建議。 有關DIL的更多資訊，請參閱[documentation](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)。

* 如本頁說明所述，這將僅涵蓋「用戶端」DIL實作，AAM客戶使用的AM客戶沒有Adobe Analytics。 如果您有Adobe Analytics，則應使用實作AAM的伺服器端轉送方法。 此方法在[documentation](https://marketing.adobe.com/resources/help/en_US/reference/ssf.html)中有說明。

## 複製和已過時的元素和方法{#duplicate-and-deprecated-elements-and-methods}

在舊版DIL和ECID中，有重複方法（在DIL和ECID中都執行相同功能的方法），造成使用者混淆。 通常，您需要同時使用這兩種資料並加以比對，而且該訊息無法與我們的客戶溝通。 從DIL 8.0開始，DIL已淘汰這些重複的方法和元素，建議您使用ECID版本。

例如：

* 使用[!DNL DIL.create]時，有些元素已過時，您應改用ECID元素。 這些元素在[[!DNL DIL.create] documentation](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html)中被調用。
* [!DNL idSync]例項層級方法也已過時，如方法的[documentation](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_idsync.html)中所呼叫。

## ID與客戶ID同步{#id-syncing-with-a-customer-id}

在AAM中，您可以將電腦上的UUID（匿名唯一使用者ID）與客戶ID同步，以便上傳有關該客戶的離線資料，並將其與其線上行為聯繫起來，以便更好地瞭解客戶。 過去，這種做法有兩種：

* [!DNL idSync]實例級方法
* [!DNL DIL.create]中的[!DNL declaredId]元素

如果您使用其中一種舊式方法與客戶ID同步，強烈建議您更新為使用[!DNL setCustomerIDs]方法，此方法是ECID服務的一部分。 有關[!DNL setCustomerIDs]的詳細資訊，請參閱方法的[檔案](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)。

**快速提示：** 先前使用上述任一方法時，您使用 [!UICONTROL Data Source]  [!UICONTROL Data Source] ID參考AAM（亦即「DPID」）。更新至[!DNL setCustomerIDs]時，您需要改用AAM [!UICONTROL Data Source]的「[!UICONTROL Integration Code]」。 它仍指向相同的[!UICONTROL Data Source]，但只是不同的識別碼。 這會顯示在以下影片中。

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

以下章節列出根據您的實作方法更新至DIL 8.0的步驟和建議：

## 在Adobe Experience Platform Launch {#updating-to-dil-in-experience-platform-launch}中更新至DIL 8.0

更新至DIL 8.0的基本步驟

1. 如果您使用8.0版以前版本的DIL，在升級之前，請進入AAM擴充功能中的DIL設定，並記下您使用的任何進階選項（以用於後續步驟）
1. 將您的AAM擴充功能更新至8.0版或更新版本
1. 驗證您的Experience Cloud ID服務擴充功能是否為3.3.0版或更新版本
1. 對於任何在8.0之前版本的AAM擴充功能或DIL自訂程式碼中停用的方法／元素（例如&quot;[!DNL disableIDSyncs]&quot;），請在ECID擴充功能中啟用ECID方法。

   1. (DIL)disableDestinationPublishingIframe ->(ECID)disableIdSyncs
   1. (DIL)disableIDSyncs ->(ECID)disableIdSyncs
   1. (DIL)iframeAkamaiHTTPS ->(ECID)dSyncSSLUseAkamai
   1. (DIL)declaredId ->(ECID)setCustomerIDs

1. 發佈變更

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## 在Adobe DTM {#updating-to-dil-in-adobe-dtm}中更新至DIL 8.0

1. 將您的AAM工具更新至8.0版或更新版本。 此版本設定位於AAM工具的「一般」區段下。
1. 對於8.0之前版本AAM工具的DIL自訂程式碼中任何已過時的方法／元素（例如「disableIDSyncs」），請記下這些方法／元素（以便您將它們新增至ECID工具），然後從AAM工具的自訂[!DNL DIL code]中移除。
1. 將您的Experience Cloud ID服務擴充功能更新至3.3.0版或更新版本
1. 將進階選項新增至您從AAM工具的自訂程式碼中移除的ECID工具。
1. 發佈變更

## 更新至DIL 8.0而無Adobe標籤管理解決方案{#additional-resources}

如果您直接在頁面上更新程式碼，則只需將舊項目取代為較新項目，除非您需要將方法／元素從DIL移至ECID，如上所述。 在這種情況下，您只需將DIL位置中的舊方法／元素取代為ECID位置中的ECID方法／元素。

非Adobe標籤管理程式也是如此。 無論您在該標籤管理解決方案中有哪些舊版，請依照下列步驟，以新程式碼取代。

1. 將您的DIL程式庫更新至最新版本（8.0或更新版本）-您將需要從Adobe Consulting或Adobe客戶服務取得最新的DIL程式碼，因為目前不提供在公共位置。 然後，只需將舊的DIL程式庫程式碼取代為新的DIL程式庫程式碼，然後繼續下一個步驟（不要立即停止，否則您會遇到問題，ha）。
1. 安裝[!DNL ECID Service]或將您現有的版本更新為3.3.0或更新版本。 您可以從我們的GitHub頁面](https://github.com/Adobe-Marketing-Cloud/id-service/releases)下載最新的Experience Cloud ID服務版本[。 如果您需要相關協助，請參閱[檔案](https://marketing.adobe.com/resources/help/en_US/mcvid/)或與Adobe顧問聯絡。

1. 確認DIL的自訂程式碼中任何已過時的方法或元素都會移至ECID方法：

   1. (DIL)disableDestinationPublishingIframe ->(ECID)disableIdSyncs

      [文件](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL)disableIDSyncs ->(ECID)disableIdSyncs

      [文件](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid-disableidsync.html)

   1. (DIL)iframeAkamaiHTTPS ->(ECID)idSyncSSLUseAkamai

      [文件](https://marketing.adobe.com/resources/help/en_US/aam/r_dil_create.html)

   1. (DIL)declaredId ->(ECID)setCustomerIDs

      [文件](https://marketing.adobe.com/resources/help/en_US/mcvid/mcvid_setcustomerids.html)