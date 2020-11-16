---
title: 全域裝置ID驗證
description: 裝置廣告識別碼（即iDFA、GAID、Roku ID）具有必須符合的格式化標準，才能用於數位廣告生態系統。 現在，客戶和合作夥伴可以以任何格式將ID上傳至我們的全球資料來源，而不會收到ID格式正確的通知。 此功能將引入傳送至全域資料來源的裝置ID驗證，以取得正確的格式，並會在ID格式錯誤時提供錯誤訊息。 我們將支援iDFA、Google Advertising和Roku ID在啟動時的驗證。
feature: data governance & privacy
topics: mobile
audience: implementer, developer, architect
activity: implement
doc-type: article
team: Technical Marketing
kt: 2977
translation-type: tm+mt
source-git-commit: a108c51fdad66f4e7974eb96609b6d8f058cb6ff
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 1%

---


# 全域裝置ID驗證 {#global-device-id-validation}

裝置廣告識別碼（即iDFA、GAID、Roku ID）具有必須符合的格式化標準，才能用於數位廣告生態系統。 現在，客戶和合作夥伴可以以任何格式將ID上傳 [!UICONTROL data sources] 至我們的Global，而不會收到ID格式正確的通知。 此功能將引入傳送至「全域」的裝置ID驗證，以 [!UICONTROL data sources] 取得正確的格式，並在ID格式錯誤時提供錯誤訊息。 我們將支援驗證 [!DNL iDFA], [!DNL Google Advertising] 並在 [!DNL Roku IDs] 啟動時提供。

## 格式標準概觀 {#overview-of-format-standards}

以下是目前由AAM識別並支援的全域裝置廣告ID群組。 這些建置方式是共 [!UICONTROL Data Sources] 用的，可供與這些平台使用者相關資料搭配運作的任何客戶或資料合作夥伴使用。

<table>
  <tr>
   <td>平台 </td>
   <td>AAM資料來源ID </td>
   <td>ID格式 </td>
   <td>AAM PID </td>
   <td>附註 </td>
  </tr>
  <tr>
   <td>Google Android(GAID)</td>
   <td>20914</td>
   <td>32十六進位數，通常以8-4-4-4-12<em>範例顯示，97987bca-ae59-4c7d-94ba-ee4f19ab8c21<br/> </em> </td>
   <td>1352</td>
   <td>此ID必須以原始／未雜湊／未變更的表單參考- <a href="https://play.google.com/about/monetization-ads/ads/ad-id/">https://play.google.com/about/monetization-ads/ads/ad-id/收集</a></td>
  </tr>
  <tr>
   <td>Apple iOS(IDFA)</td>
   <td>20915</td>
   <td>32十六進位數，通常以8-4-4-4-12 <em>範例顯示，6D92078A-8246-4BA4-AE5B-76104861E7DC<br /> </em> </td>
   <td>3560</td>
   <td>此ID必須以原始／未雜湊／未變更的表單參考- <a href="https://support.apple.com/en-us/HT205223">https://support.apple.com/en-us/HT205223收集</a></td>
  </tr>
  <tr>
   <td>羅庫（里達）</td>
   <td>121963</td>
   <td>32十六進位數，通常以8-4-4-4-12 <em>範例</em><em>fcb2a29c-315a-5e6b-bcfd-d889ba19aada呈現</em></td>
   <td>11536</td>
   <td>此ID必須以原始／未雜湊／未變更的表單參考- <a href="https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework">https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework收集</a> </td>
  </tr>
  <tr>
   <td>Microsoft廣告ID(MAID)</td>
   <td>389146</td>
   <td>英數字串</td>
   <td>14593</td>
   <td>此ID必須以原始／未雜湊／未變更的表單參考- <a href="https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid">https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.</a><br/><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx">advertisingidhttps://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx收集</a></td>
  </tr>
  <tr>
   <td>Samsung DUID</td>
   <td>404660</td>
   <td>英數字串範例， 7XCBNROQJQPYW</td>
   <td>15950</td>
   <td>此ID必須以原始／未雜湊／未變更的表單參考- <a href="https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api">https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api收集</a> </td>
  </tr>
</table>

## 在應用程式中設定廣告識別碼 {#setting-an-advertising-identifier-in-the-app}

在應用程式中設定廣告商ID實際上是兩個步驟，先擷取廣告商ID，然後傳送至Experience Cloud。 執行這些步驟的連結如下。

1. 擷取ID
   1. [!DNL Apple] 有關此處 [!DNL advertising ID] 的資訊 [可找到](https://developer.apple.com/documentation/adsupport/asidentifiermanager)。
   1. 有關為開發人員設 [!DNL advertiser ID] 定 [!DNL Android] 的資訊，請 [參閱](http://www.androiddocs.com/google/play-services/id.html)。
1. 使用SDK中的方法將它 [!DNL setAdvertisingIdentifier] 傳送至Experience Cloud
   1. 有關使用 `setAdvertisingIdentifier` 的資訊，請 [參見](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference#set-an-advertising-identifier) , [!DNL iOS] 和 [!DNL Android]。

`// iOS (Swift) example for using setAdvertisingIdentifier:`
`ACPCore.setAdvertisingIdentifier([AdvertisingId]) // ...where [AdvertisingId] is replaced by the actual advertising ID`

## 錯誤ID的DCS錯誤訊息  {#dcs-error-messaging-for-incorrect-ids}

當將不正確的全域裝置ID（IDFA、GAID等）即時提交至Audience Manager時，點擊時會傳回錯誤碼。 以下是傳回錯誤的範例，因為ID會以 [!DNL Apple IDFA]an傳入，而且只應包含大寫字母，而ID中卻有小寫&#39;x&#39;。

![錯誤影像](assets/image_4_.png)

請參閱文 [件](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-error-codes.html?lang=en#api-and-sdk-code) ，以取得錯誤碼清單。

## 上線全域裝置ID {#onboarding-global-device-ids}

除了即時提交全域裝置ID外，您也可以對[!DNL onboard]ID「上傳」資料。 此程式與您針對客戶ID上線資料（通常透過金鑰／值配對）時的程式相同，但您只需使用適當的資料來源ID，即可將資料指派給全域裝置ID。 有關上線程式的檔案，請參閱文 [件](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/sending-audience-data/batch-data-transfer-process/batch-data-transfer-overview.html?lang=en#implementation-integration-guides)。 請記得使用全域 [!UICONTROL data source] ID，視您使用的平台而定。

如果透過入門程式提交不正確的全域裝置ID，錯誤將會顯示在中 [[!DNL Onboarding Status Report]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reporting/onboarding-status-report.html?lang=en#reporting)。

以下是該報表中可能出現的錯誤範例：

![錯誤影像](assets/image_5_.png)