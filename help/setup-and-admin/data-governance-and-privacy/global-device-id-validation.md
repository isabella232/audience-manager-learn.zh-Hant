---
title: 全局設備ID驗證
description: 瞭解如何驗證發送到全局資料源的設備ID以正確格式化，以及當ID格式不正確時有關錯誤消息。
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

# 全局設備ID驗證 {#global-device-id-validation}

設備廣告標識符（即iDFA、GAID、Roku ID）具有必須滿足的格式標準才能在數字廣告生態系統中使用。 今天，客戶和合作夥伴可以以任何格式將ID上載到我們的全球資料源，而不會收到有關ID是否格式正確的通知。 此功能將引入對發送到全局資料源的設備ID的驗證，以便正確設定格式，並在ID格式不正確時提供錯誤消息。 我們將支援驗證 [!DNL iDFA]。 [!DNL Google Advertising] 和 [!DNL Roku IDs] 發射。

## 格式標準概述 {#overview-of-format-standards}

以下是當前已識別並支援的全局設備廣告ID池AAM。 這些是以共用方式實現的 [!UICONTROL Data Sources] 可供與這些平台用戶關聯的資料打交道的任何客戶或資料合作夥伴使用。

<table>
  <tr>
   <td>平台 </td>
   <td>AAM資料源ID </td>
   <td>ID格式 </td>
   <td>PIDAAM </td>
   <td>附註 </td>
  </tr>
  <tr>
   <td>Google安卓(GAID)</td>
   <td>20914</td>
   <td>32個十六進位數，通常顯示為8-4-4-4-12<em>示例， 97987bca-ae59-4c7d-94ba-ee4f19ab8c21<br/> </em> </td>
   <td>1352</td>
   <td>此ID必須以原始/未散列/未更改的表單引用 —  <a href="https://play.google.com/about/monetization-ads/ads/ad-id/">https://play.google.com/about/monetization-ads/ads/ad-id/</a></td>
  </tr>
  <tr>
   <td>Apple·iOS(IDFA)</td>
   <td>20915</td>
   <td>32個十六進位數，通常顯示為8-4-4-4-12 <em>例如，6D92078A-8246-4BA4-AE5B-76104861E7DC<br /> </em> </td>
   <td>3560</td>
   <td>此ID必須以原始/未散列/未更改的表單引用 —  <a href="https://support.apple.com/en-us/HT205223">https://support.apple.com/en-us/HT205223</a></td>
  </tr>
  <tr>
   <td>羅庫語(RIDA)</td>
   <td>121963</td>
   <td>32個十六進位數，通常顯示為8-4-4-4-12 <em>示例：</em> <em>fcb2a29c-315a-5e6b-bcfd-d889ba19aada</em></td>
   <td>11536</td>
   <td>此ID必須以原始/未散列/未更改的表單引用 —  <a href="https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework">https://sdkdocs.roku.com/display/sdkdoc/Roku+Advertising+Framework</a> </td>
  </tr>
  <tr>
   <td>Microsoft廣告ID(MAID)</td>
   <td>389146</td>
   <td>字母數字字串</td>
   <td>14593</td>
   <td>此ID必須以原始/未散列/未更改的表單引用 —  <a href="https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid">https://docs.microsoft.com/en-us/uwp/api/windows.system.userprofile.advertisingmanager.advertisingid</a><br/><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx">https://msdn.microsoft.com/en-us/library/windows/apps/windows.system.userprofile.advertisingmanager.advertisingid.aspx</a></td>
  </tr>
  <tr>
   <td>三星DUID</td>
   <td>404660</td>
   <td>字母數字字串示例， 7XCBNROQJQPYW</td>
   <td>15950</td>
   <td>此ID必須以原始/未散列/未更改的表單引用 —  <a href="https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api">https://developer.samsung.com/tv/develop/api-references/samsung-product-api-references/productinfo-api</a> </td>
  </tr>
</table>

## 在應用中設定廣告標識符 {#setting-an-advertising-identifier-in-the-app}

在應用中設定廣告商ID實際上是一個兩步過程，首先檢索廣告商ID，然後將其發送到Experience Cloud。 下面是執行這些步驟的連結。

1. 檢索ID
   1. [!DNL Apple] 有關 [!DNL advertising ID] 可找到 [這裡](https://developer.apple.com/documentation/adsupport/asidentifiermanager)。
   1. 有關設定 [!DNL advertiser ID] 為 [!DNL Android] 可以找到開發者 [這裡](http://android.cn-mirrors.com/google/play-services/id.html)。
1. 使用 [!DNL setAdvertisingIdentifier] SDK中的方法
   1. 使用資訊 `setAdvertisingIdentifier` 的 [文檔](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/mobile-core/identity/identity-api-reference#set-an-advertising-identifier) 兩者 [!DNL iOS] 和 [!DNL Android]。

`// iOS (Swift) example for using setAdvertisingIdentifier:`
`ACPCore.setAdvertisingIdentifier([AdvertisingId]) // ...where [AdvertisingId] is replaced by the actual advertising ID`

## DCS錯誤消息傳遞錯誤ID  {#dcs-error-messaging-for-incorrect-ids}

當向Audience Manager即時提交不正確的全局設備ID（IDFA、GAID等）時，在命中時將返回錯誤代碼。 以下是返回錯誤的示例，因為ID以 [!DNL Apple IDFA]，它只應包含大寫字母，但ID中存在小寫「x」。

![錯誤影像](assets/image_4_.png)

請參閱 [文檔](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-error-codes.html?lang=en#api-and-sdk-code) 的下界。

## 登錄全局設備ID {#onboarding-global-device-ids}

除了即時提交全局設備ID外，您還可以「 」[!DNL onboard]&quot;（上載）ID的資料。 此過程與您針對客戶ID（通常通過鍵/值對）登錄資料時的過程相同，但您只需使用正確的資料源ID，以便將資料分配給全局設備ID。 有關登機過程的文檔，請參見 [文檔](https://experienceleague.adobe.com/docs/audience-manager/user-guide/implementation-integration-guides/sending-audience-data/batch-data-transfer-process/batch-data-transfer-overview.html?lang=en#implementation-integration-guides)。 請記住，根據您使用的平台，使用全局資料源ID。

如果通過登機過程提交了不正確的全局設備ID，錯誤將顯示在 [[!DNL Onboarding Status Report]](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reporting/onboarding-status-report.html?lang=en#reporting)。

以下是該報告中出現的錯誤示例：

![錯誤影像](assets/image_5_.png)
