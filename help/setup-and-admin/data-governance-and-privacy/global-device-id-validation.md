---
title: 全域裝置ID驗證
description: 裝置廣告識別碼（即iDFA、GAID、Roku ID）具有必須符合的格式標準，才能在數位廣告生態系統中使用。 現在，客戶和合作夥伴可以以任何格式將ID上傳至我們的全域資料來源，而不需收到ID格式是否正確的通知。 此功能將引入對傳送至全域資料來源的裝置ID進行驗證，以取得正確的格式，並在ID格式不正確時提供錯誤訊息。 我們將支援在啟動時驗證iDFA、Google Advertising和Roku ID。
feature: Data Governance & Privacy
topics: mobile
activity: implement
doc-type: article
team: Technical Marketing
kt: 2977
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 0ff3f123-efb3-4124-bdf9-deac523ef8c9
source-git-commit: a2bf5c6bdc7611cd6bc5d807e60ac6aa22d5c176
workflow-type: tm+mt
source-wordcount: '778'
ht-degree: 1%

---

# 全域裝置ID驗證 {#global-device-id-validation}

裝置廣告識別碼（即iDFA、GAID、Roku ID）具有必須符合的格式標準，才能在數位廣告生態系統中使用。 現在，客戶和合作夥伴可以以任何格式將ID上傳至我們的全域[!UICONTROL data sources]，而不需收到ID格式是否正確的通知。 此功能將引入對傳送至全域[!UICONTROL data sources]的裝置ID的驗證，以驗證格式是否正確，並在ID格式不正確時提供錯誤訊息。 我們將支援在啟動時驗證[!DNL iDFA]、[!DNL Google Advertising]和[!DNL Roku IDs]。

## 格式標準概述 {#overview-of-format-standards}

以下是目前由AAM識別並支援的全域裝置廣告ID池。 這些實作方式為共用[!UICONTROL Data Sources]，可供與這些平台使用者相連的資料搭配運作的任何客戶或資料合作夥伴使用。

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
   <td>32十六進位數，通常呈現為8-4-4-4-12<em>範例，97987bca-ae59-4c7d-94ba-ee4f19ab8c21<br/> </em> </td>
   <td>1352</td>
   <td>此ID必須以原始/未雜湊/未更改的表單參考 — <a href="https://play.google.com/about/monetization-ads/ads/ad-id/">https://play.google.com/about/monetization-ads/ads/ad-id/</a>收集</td>
  </tr>
  <tr>
   <td>Apple iOS(IDFA)</td>
   <td>20915</td>
   <td>32十六進位數，通常顯示為8-4-4-4-12 <em>示例，6D92078A-8246-4BA4-AE5B-76104861E7DC<br /> </em> </td>
   <td>3560</td>
   <td>此ID必須以原始/未雜湊/未更改的表單參考 — <a href="https://support.apple.com/en-us/HT205223">https://support.apple.com/en-us/HT205223</a>收集</td>
  </tr>
  <tr>
   <td>Roku(RIDA)</td>
   <td>121963</td>
   <td>32十六進位數，通常以8-4-4-4-12 <em>範例呈現，</em> <em> fcb2a29c-315a-5e6b-bcfd-d889ba19aada</em></td>
   <td>11536</td>
   <td>此ID必須以原始/未雜湊/未更改的表單參考 — <a href="https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework">https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework</a>收集 </td>
  </tr>
  <tr>
   <td>Microsoft Advertising ID(MAID)</td>
   <td>389146</td>
   <td>英數字串</td>
   <td>14593</td>
   <td>此ID必須以原始/未雜湊/未更改的表單參考 — <a href="https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid">https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid</a><br/><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx">https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx</a>收集</td>
  </tr>
  <tr>
   <td>Samsung DUID</td>
   <td>404660</td>
   <td>英數字串示例， 7XCBNROQJQPYW</td>
   <td>15950</td>
   <td>此ID必須以原始/未雜湊/未更改的表單參考 — <a href="https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api">https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api</a>收集 </td>
  </tr>
</table>

## 在應用程式中設定廣告識別碼 {#setting-an-advertising-identifier-in-the-app}

在應用程式中設定廣告商ID實際上是兩個步驟的程式，先擷取廣告商ID，然後將其傳送至Experience Cloud。 請在下方找到執行這些步驟的連結。

1. 擷取ID
   1. [!DNL Apple] 如需相關資 [!DNL advertising ID] 訊，請參 [閱此處](https://developer.apple.com/documentation/adsupport/asidentifiermanager)。
   1. 有關為[!DNL Android]開發人員設定[!DNL advertiser ID]的一些資訊，請參見[HERE](http://android.cn-mirrors.com/google/play-services/id.html)。
1. 使用SDK中的[!DNL setAdvertisingIdentifier]方法將其傳送至Experience Cloud
   1. `setAdvertisingIdentifier`的使用資訊位於[documentation](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference#set-an-advertising-identifier)中，適用於[!DNL iOS]和[!DNL Android]。

`// iOS (Swift) example for using setAdvertisingIdentifier:`
`ACPCore.setAdvertisingIdentifier([AdvertisingId]) // ...where [AdvertisingId] is replaced by the actual advertising ID`

## ID不正確的DCS錯誤訊息  {#dcs-error-messaging-for-incorrect-ids}

將錯誤的全域裝置ID（IDFA、GAID等）即時提交至Audience Manager時，點擊會傳回錯誤代碼。 以下是傳回錯誤的範例，因為ID是以[!DNL Apple IDFA]傳入，其中應僅包含大寫字母，而ID中則有小寫&#39;x&#39;。

![錯誤影像](assets/image_4_.png)

請參閱[檔案](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-error-codes.html?lang=en#api-and-sdk-code)以取得錯誤代碼清單。

## 上線全域裝置ID {#onboarding-global-device-ids}

除了即時提交全域裝置ID外，您還可以對ID「[!DNL onboard]」（上傳）資料。 此程式與針對客戶ID上線資料時相同（通常透過索引鍵/值配對），但您只需使用正確的資料來源ID，即可將資料指派給全域裝置ID。 有關上線程式的檔案，請參閱[檔案](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/sending-audience-data/batch-data-transfer-process/batch-data-transfer-overview.html?lang=en#implementation-integration-guides)。 請記得根據您使用的平台，使用全域[!UICONTROL data source] ID。

如果透過上線程式提交錯誤的全域裝置ID，錯誤會顯示在[[!DNL Onboarding Status Report]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reporting/onboarding-status-report.html?lang=en#reporting)中。

以下是該報表中可能出現的錯誤範例：

![錯誤影像](assets/image_5_.png)
