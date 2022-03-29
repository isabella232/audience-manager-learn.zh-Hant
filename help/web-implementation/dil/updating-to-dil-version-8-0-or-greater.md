---
title: 更新到Adobe Audience ManagerDIL8.0版（或更高版本）
description: 本文將就將Adobe Audience Manager(AAM)Data Integration Library(DIL)代碼更新為8.0版或更高版本提供步驟和建議。 這指的是「客戶端」DIL實現，而不是Adobe Analytics資料的伺服器端轉發，它將涵蓋DTM、Launch by Adobe和沒有Adobe標籤管理器解決方案的實現。
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

# 更新到Adobe Audience ManagerDIL8.0版（或更高版本） {#updating-to-adobe-audience-manager-s-dil-version-or-greater}

本文將就更新Adobe Audience Manager(AAM) [!DNL Data Integration Library] (DIL)8.0版或更高版本的代碼。 這指的是「客戶端」DIL實現，而不是Adobe Analytics資料的伺服器端轉發，它將涵蓋DTM、Launch by Adobe和沒有Adobe標籤管理器解決方案的實現。

## 概述 {#overview}

Audience Manager [!DNL Data Integration Library] (DIL)代碼允許您在AAM網站*上實現。 在實施以前版本的DIL時，不需要同時實施Adobe的Experience CloudID服務(ECID)（儘管這是個好主意）。 從DIL版本8.0開始，對ECID版本3.3或更高版本存在硬依賴關係。 如果您在沒有ECID 3.3的情況下實施DIL8.0或更高版本，或使用早期版本，則會出現錯誤，該錯誤將無法正常運行。 由於您可以採用多種方法AAM來實施，因此我們建立了此頁面，以向您提供一些步驟和建議。 在下面，您將看到這些步驟和建議按平台/實施方法分類。 有關DIL的詳細資訊，請參閱 [文檔](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en)。

* 如本頁說明所述，這將僅涵蓋「客戶端」DIL實施，由沒有Adobe AnalyticsAAM的客戶使用。 如果您有Adobe Analytics，則應使用實現的伺服器端轉發方AAM法。 此方法在 [文檔](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html)。

## 重複和不建議使用的元素和方法 {#duplicate-and-deprecated-elements-and-methods}

在DIL和ECID的先前版本中，存在重複方法(在DIL和ECID中都執行相同功能的方法)，這導致人們對使用哪一種方法產生混淆。 通常，您需要同時使用它們並將它們匹配起來，而且該資訊沒有很好地傳達給我們的客戶。 從DIL8.0開始，DIL中已棄用這些重複的方法和元素，建議使用ECID版本。

例如：

* 使用時 [!DNL DIL.create]，已棄用一些元素，您應改用ECID元素。 這些元素在 [[!DNL DIL.create] 文檔](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)。
* 的 [!DNL idSync] 實例級方法也已被棄用，如方法中所調用的 [文檔](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-instance-methods.html)。

## ID與客戶ID同步 {#id-syncing-with-a-customer-id}

在中AAM，您可以將電腦上的UUID（匿名唯一用戶ID）與客戶ID同步，以便您可以上載有關該客戶的離線資料並將其與他們的聯機行為聯繫起來，以便更好地瞭解您的客戶。 過去，這主要通過以下兩種方式之一實現：

* 的 [!DNL idSync] 實例級方法
* 的 [!DNL declaredId] 元素 [!DNL DIL.create]

如果您使用了以下任一較舊的方法與客戶ID同步，強烈建議您將更新為使用 [!DNL setCustomerIDs] 方法，是ECID服務的一部分。 有關 [!DNL setCustomerIDs] 在 [文檔](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)。

**快速提示：** 以前使用上述任何一種方法時，都引用了 [!UICONTROL Data Source] 和 [!UICONTROL Data Source] ID（又稱「DPID」）。 更新到時 [!DNL setCustomerIDs]，你需要使用AAM [!UICONTROL Data Source]&#39;s&#39;[!UICONTROL Integration Code]」。 它仍然指向同一 [!UICONTROL Data Source] 但只是另一個標識。 下面的視頻中顯示了這一點。

>[!VIDEO](https://video.tv.adobe.com/v/23873/?quality=12)

以下各節列出了根據您的實施方法更新到DIL8.0的步驟和建議：

## 在Adobe Experience Platform標籤中更新到DIL8.0 {#updating-to-dil-in-experience-platform-launch}

更新到DIL8.0的基本步驟

1. 如果您使用的是8.0之前版本的DIL，則在升級之前，請進入擴展中的DIL配置AAM，並記下您使用的任何高級選項（將在後續步驟中使用）。
1. 將擴展AAM更新為8.0版或更高版本。
1. 驗證Experience CloudID服務擴展是3.3.0版或更高版本。
1. 對於任何已過時的方法/元素(如 `disableIDSyncs`)中的ECID方法，AAM或在用於DIL的自定義代碼中啟用ECID擴展。

   1. (DIL) `disableDestinationPublishingIframe` ->(ECID) `disableIdSyncs`
   1. (DIL) `disableIDSyncs` ->(ECID) `disableIdSyncs`
   1. (DIL) `iframeAkamaiHTTPS` ->(ECID) `dSyncSSLUseAkamai`
   1. (DIL) `declaredId` ->(ECID) `setCustomerIDs`

1. 發佈更改。

>[!VIDEO](https://video.tv.adobe.com/v/23874/?quality=12)

## 在DILDTM中更新到Adobe8.0 {#updating-to-dil-in-adobe-dtm}

1. 將工AAM具更新為8.0版或更高版本。 此版本設定位於工具的「常規」部AAM分。
1. 對於任何已過時的方法/元素(如 `disableIDSyncs`)中用於DIL的自定義代碼AAM，記下這些代碼（以便將它們添加到ECID工具中），然後從自定義代碼中刪除 [!DNL DIL code] 的上AAM界。
1. 將Experience CloudID服務擴展更新為3.3.0版或更高版本
1. 將高級選項添加到從工具的自定義代碼中刪AAM除的ECID工具中。
1. 發佈更改

## 更新到DIL8.0，無AdobeTag Management解決方案 {#additional-resources}

如果您直接在頁面上更新代碼，則只需用較新的項目替換較舊的項目，除非您需要將方法/元素從DIL移到ECID，如上所述。 在這種情況下，您只需將DIL位置中的舊方法/元素替換為ECID位置中的ECID方法/元素。

非Adobe標籤經理也是如此。 無論您在該標籤管理解決方案中有哪些舊版本，請按照以下步驟中的說明將其替換為新代碼。

1. 將DIL庫更新為最新版本（8.0或更高版本） — 您需要從Adobe咨詢或Adobe客戶服務部獲取最新DIL代碼，因為該代碼當前在公共位置不可用。 然後，只需用新DIL庫代碼替換舊DIL庫代碼，然後轉到下一步（不要立即停止，否則您會遇到問題，哈哈）。
1. 安裝 [!DNL ECID Service] 或將現有版本更新為3.3.0或更高版本。 您可以下載最新的Experience CloudID服務版本 [從我們的GitHub頁面](https://github.com/Adobe-Marketing-Cloud/id-service/releases)。 如果需要幫助，請參閱 [文檔](https://experienceleague.adobe.com/docs/id-service/using/home.html) 或者跟Adobe顧問談談。

1. 驗證DIL的自定義代碼中的任何已過時方法或元素是否已移動到ECID方法：

   1. (DIL) `disableDestinationPublishingIframe` ->(ECID) `disableIdSyncs`

      [文件](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `disableIDSyncs` ->(ECID) `disableIdSyncs`

      [文件](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/configurations/disableidsync.html)

   1. (DIL) `iframeAkamaiHTTPS` ->(ECID) `idSyncSSLUseAkamai`

      [文件](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/class-level-dil-methods/dil-create.html)

   1. (DIL) `declaredId` ->(ECID) `setCustomerIDs`

      [文件](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html)
