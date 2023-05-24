---
title: 全域裝置識別碼驗證
description: 瞭解如何驗證傳送至全域資料來源的裝置ID以使用正確的格式，以及在ID格式不正確時傳送錯誤訊息。
feature: Data Governance & Privacy
topics: mobile
activity: implement
doc-type: article
team: Technical Marketing
kt: 2977
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 0ff3f123-efb3-4124-bdf9-deac523ef8c9
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '718'
ht-degree: 1%

---

# 全域裝置識別碼驗證 {#global-device-id-validation}

裝置廣告識別碼（即iDFA、GAID、Roku ID）具有必須符合的格式標準，才能在數位廣告生態系統中使用。 現在，客戶和合作夥伴可以任何格式將ID上傳至全域資料來源，而不需要通知ID的格式是否正確。 此功能將會驗證傳送至全域資料來源的裝置ID是否格式正確，並在ID格式不正確時提供錯誤訊息。 我們將支援驗證 [!DNL iDFA]， [!DNL Google Advertising] 和 [!DNL Roku IDs] 於啟動時。

## 格式標準概觀 {#overview-of-format-standards}

以下是AAM目前識別和支援的全域裝置廣告ID集區。 這些會實作為共用 [!UICONTROL Data Sources] 任何客戶或資料合作夥伴只要搭配與這些平台使用者繫結的資料運作，都可以使用這些標籤。

<table>
  <tr>
   <td>平台 </td>
   <td>AAM資料來源ID </td>
   <td>ID格式 </td>
   <td>AAM PID </td>
   <td>附註 </td>
  </tr>
  <tr>
   <td>Google Android (GAID)</td>
   <td>20914</td>
   <td>32個十六進位數字，通常顯示為8-4-4-4-12<em>範例： 97987bca-ae59-4c7d-94ba-ee4f19ab8c21<br/> </em> </td>
   <td>1352</td>
   <td>此ID必須以原始/未雜湊/未變更的格式收集參考資料 —  <a href="https://play.google.com/about/monetization-ads/ads/ad-id/">https://play.google.com/about/monetization-ads/ads/ad-id/</a></td>
  </tr>
  <tr>
   <td>Apple iOS (IDFA)</td>
   <td>20915</td>
   <td>32個十六進位數字，通常顯示為8-4-4-4-12 <em>例如，6D92078A-8246-4BA4-AE5B-76104861E7DC<br /> </em> </td>
   <td>3560</td>
   <td>此ID必須以原始/未雜湊/未變更的格式收集參考資料 —  <a href="https://support.apple.com/en-us/HT205223">https://support.apple.com/en-us/HT205223</a></td>
  </tr>
  <tr>
   <td>Roku (RIDA)</td>
   <td>121963</td>
   <td>32個十六進位數字，通常顯示為8-4-4-4-12 <em>例如，</em> <em>fcb2a29c-315a-5e6b-bcfd-d889ba19aada</em></td>
   <td>11536</td>
   <td>此ID必須以原始/未雜湊/未變更的格式收集參考資料 —  <a href="https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework">https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework</a> </td>
  </tr>
  <tr>
   <td>Microsoft Advertising ID (MAID)</td>
   <td>389146</td>
   <td>英數字串</td>
   <td>14593</td>
   <td>此ID必須以原始/未雜湊/未變更的格式收集參考資料 —  <a href="https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid">https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid</a><br/><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx">https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx</a></td>
  </tr>
  <tr>
   <td>Samsung DUID</td>
   <td>404660</td>
   <td>英數字串範例，7XCBNROQJQPYW</td>
   <td>15950</td>
   <td>此ID必須以原始/未雜湊/未變更的格式收集參考資料 —  <a href="https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api">https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api</a> </td>
  </tr>
</table>

## 在應用程式中設定廣告識別碼 {#setting-an-advertising-identifier-in-the-app}

在應用程式中設定廣告商ID實際上需要兩個步驟，首先擷取廣告商ID，然後將其傳送至Experience Cloud。 執行上述步驟的連結如下。

1. 擷取ID
   1. [!DNL Apple] 關於 [!DNL advertising ID] 可以找到 [此處](https://developer.apple.com/documentation/adsupport/asidentifiermanager).
   1. 關於設定 [!DNL advertiser ID] 的 [!DNL Android] 可以找到開發人員 [此處](http://android.cn-mirrors.com/google/play-services/id.html).
1. 使用將其傳送到Experience Cloud中 [!DNL setAdvertisingIdentifier] sdk中的方法
   1. 使用的資訊 `setAdvertisingIdentifier` 位於 [檔案](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference#set-an-advertising-identifier) 兩者 [!DNL iOS] 和 [!DNL Android].

`// iOS (Swift) example for using setAdvertisingIdentifier:`
`ACPCore.setAdvertisingIdentifier([AdvertisingId]) // ...where [AdvertisingId] is replaced by the actual advertising ID`

## DCS錯誤訊息，指出不正確的ID  {#dcs-error-messaging-for-incorrect-ids}

當不正確的全域裝置ID （IDFA、GAID等）即時提交給Audience Manager時，點選時將會傳回錯誤代碼。 以下為傳回錯誤的範例，因為ID會作為 [!DNL Apple IDFA]，其中只應包含大寫字母，但ID中有小寫字母「x」。

![錯誤影像](assets/image_4_.png)

請參閱 [檔案](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-error-codes.html?lang=en#api-and-sdk-code) 以取得錯誤碼清單。

## 加入全域裝置ID {#onboarding-global-device-ids}

除了即時提交全域裝置ID外，您也可以&quot;[!DNL onboard]&quot; （上傳）資料並與ID比對。 此程式等同於您根據客戶ID上線資料時（通常透過索引鍵/值配對），但您只需使用適當的資料來源ID，好讓資料指派給全域裝置ID。 上線流程的相關檔案可在以下網址找到： [檔案](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/sending-audience-data/batch-data-transfer-process/batch-data-transfer-overview.html?lang=en#implementation-integration-guides). 請記得根據您使用的平台，使用全域資料來源ID。

如果透過上線流程提交不正確的全域裝置ID，錯誤將顯示在 [[!DNL Onboarding Status Report]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reporting/onboarding-status-report.html?lang=en#reporting).

以下為通過該報表發生的錯誤範例：

![錯誤影像](assets/image_5_.png)
