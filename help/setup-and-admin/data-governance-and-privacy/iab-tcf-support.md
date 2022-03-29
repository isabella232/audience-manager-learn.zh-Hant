---
title: IAB TCF 2.0支援
description: 瞭解IAB TCF的Audience Manager插件，以及它如何與Adobe的選擇加入對象和您的同意管理提供程式(CMP)配合工作。
feature: Data Governance & Privacy
activity: implement
doc-type: technical video
team: Technical Marketing
thumbnail: 26434.jpg
kt: 5027
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 04b4e786-0457-4dcc-bcf9-a79eda67bb2e
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '1078'
ht-degree: 0%

---

# IAB TCF 2.0支援Audience Manager {#iab-tcf-support-in-audience-manager}

Adobe通過選擇加入功能和IAB透明度和同意框架2.0(TCF 2.0)支援的Audience Manager插件，為您提供管理和傳達用戶隱私選擇的手段。 本文與文檔一起使用，幫助您瞭解IAB TCF的Audience Manager插件，以及它如何與Adobe的Opt-in對象和您的同意管理提供商(CMP)一起使用。 要瞭解有關IAB的更多資訊，請訪問其網站： [https://www.iabeurope.eu/](https://www.iabeurope.eu/)。

## 第一步：瞭解Experience CloudID選擇加入 {#first-step-understand-ecid-s-opt-in}

要瞭解如何與IAB TCF合作，您必須首先理解 [!DNL Opt-in] 功能，是Experience CloudID服務(ECID)庫的一部分。 如果您不熟悉選擇加入的工作原理，請參閱 [這篇有益的文章](https://experienceleague.adobe.com/docs/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html) 。 您還應查看「選擇加入」 [文檔](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html)。 一旦您瀏覽了這些資源，請返回此頁並繼續。

## IAB TCF的Audience Manager插件 {#the-audience-manager-plug-in-for-iab-tcf}

現在，您至少對選擇加入服務的工作方式有了基本的瞭解，Audience Manager可以將它分層 [!DNL IAB Transparency and Consent Framework (TCF)] 支援，通過Opt-in對象的插件完成。

IAB TCF的Audience Manager插件擴展了Opt-in的功能，使客戶能夠根據IAB TCF評估、AAM評價和轉發用戶隱私選擇並轉發給下游合作夥伴。 它提供了資料控制器(即您作為Adobe客戶)和供應商(DMP 、 DSP SSP 、 Ad Servers等)的標準 可以用來理解同意。

## 啟用IAB TCF {#enabling-iab-tcf}

如果您使用Adobe Experience Platform Launch，則為IAB TCF啟用Audience Manager插件會很容易，因為它是一個簡單的複選框，如下面的短視頻所示：

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

或者，如果您不使用「啟動」(Launch)，則可以使用 `isIabContext=true` 以在實例化Experience Cloud訪問者時啟用它。 這將啟動IAB TCF流，即向同意收集添加另一步驟，使用IAB TCF查詢IAB TC字串並將其返回Opt-in,Opt-in隨後與Experience Cloud解決方案通信。

## IAB TC字串 {#iab-tcf-consent-string}

IAB提供的一個標準是「同意字串」（又稱「DaisyBit」），這實際上是兩個清單的集合：

1. 目的： **什麼** 是否同意？
1. 供應商： **誰** 是否同意？

### 目的 {#purposes}

IAB TCF 2.0有10個&quot;目的&quot;用於收集同意（供應商可以如何處理訪問者的資料）。 Adobe Audience Manager並不要求全部十項，而只要求在供應商同意之外，出於下列目的：

* **目的一：** 在設備上儲存和/或訪問資訊；
* **目的十：** 開發和改進產品；
* **特殊目的1:** 確保安全性、防止欺詐和調試。

這是IAB TC字串的第一部分，並且僅記錄為1和0，指示是否批准該目的/活動。

>[!NOTE]
>
>根據IAB法規，特殊目的1（確保安全性、防止欺詐和調試）始終是同意的，用戶不能反對。

### 供應商 {#vendors}

IAB TC字串的另一部分是包含數百個供應商的長清單，以便訪問者可以看到一個清單，其中列出了在站點上具有標籤的適用供應商，並可以選擇使用哪些供應商。 供應商在清單中保持其位置。 例如，此清單上的Adobe Audience Manager供應商編號為565。 如果清單中的該數字具有「1」，則Audience Manager可以從清單前面執行已批准的目的。 如AAM果spot有&quot;0&quot;，則它不能對資料執行任何操作。

**為了Audience Manager為客戶提供UI以使用IAB TCF選擇這些目的和供應商，或批准/不批准所有活動，您必須使用在IAB TCF中註冊的CMP合作夥伴，或構建支援IAB TCF並在IAB TCF中註冊的CMP合作夥伴。**

## 選擇加入：在IAB和Adobe應用程式之間轉換 {#opt-in-translating-between-iab-and-adobe-solutions}

使用IAB TCF的好處之一是，上述標準目的可能使最終用戶更多地瞭解他們正在批准什麼，而不是Adobe解決方案清單。 最終用戶可能不知道「批准」Audience Manager或 [!DNL Target]，但是「在設備上儲存和/或訪問資訊」或「開發和改進產品」可能更容易讓他們理解並同意。

為了Audience Manager獲得批准(即，為了將IAB目的翻譯為選擇加入AAM，給予&quot;贊成&quot;表決)，必須得到最終用戶的同意，如上所列。 如果其中任何一個未獲批准，或供應商未獲批准，則AAM不會執行像素激發或設定Cookie。 另外，很好的是，許多客戶只是選擇為最終用戶提供「全部或無」UI，這當然會允許或不允許使用Audience Manager(和其他Experience Cloud解決方案)。

有些資訊 [文檔](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=en) 關於IAB TCF流的Audience Manager插件如何應用於發佈者和廣告商使用案例。

## IAB:向下游發送同意 {#iab-sending-consent-downstream}

當使用IAB TCF的Audience Manager插件時，用戶的同意選擇也將被發送到平台級（第三方）ID同步，以便該合作夥伴擁有用戶同意資訊並能夠對其採取行動。 此資訊以兩個變數發送：

* gdpr = 1
* gdpr_connence = [編碼同意字串]

警告是，如果用戶在IAB上下文中並且不提供同意（或提供否定的同意），則Audience Manager根本不收集IAB TC字串，因此會刪除調用。 所以，在那種情況下……下游不得通過同意。

## 示範 {#demo}

在下面的視頻中，請參閱ECID和解決方案中的cookie和信標如何受IAB用戶選擇選擇的影響。

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

有關IAB TCF 2.0Audience Manager插件的更多詳細資訊，包括如何實施和test、使用案例和工作流，請參閱 [文檔](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html)。
