---
title: IAB TCF 2.0Audience Manager支援
description: Adobe可讓您透過選擇加入功能以及IAB透明度與同意框架2.0(TCF 2.0)支援的Audience Manager外掛程式，管理並傳達使用者的隱私權選擇。 本文與檔案一起使用，可協助您瞭解IAB TCF的Audience Manager外掛程式，以及它如何與Adobe的選擇加入物件及您的同意管理提供者(CMP)搭配使用。
feature: "Data Governance & Privacy"
activity: implement
doc-type: technical video
team: Technical Marketing
thumbnail: 26434.jpg
kt: 5027
role: Developer, Data Engineer, Architect
level: Experienced
exl-id: 04b4e786-0457-4dcc-bcf9-a79eda67bb2e
translation-type: tm+mt
source-git-commit: 256edb05f68221550cae2ef7edaa70953513e1d4
workflow-type: tm+mt
source-wordcount: '1126'
ht-degree: 0%

---

# IAB TCF 2.0在Audience Manager{#iab-tcf-support-in-audience-manager}中的支援

Adobe可讓您透過選擇加入功能以及IAB透明度與同意框架2.0(TCF 2.0)支援的Audience Manager外掛程式，管理並傳達使用者的隱私權選擇。 本文與檔案一起使用，可協助您瞭解IAB TCF的Audience Manager外掛程式，以及它如何與Adobe的選擇加入物件及您的同意管理提供者(CMP)搭配使用。 若要進一步瞭解IAB，請參閱其網站：[https://www.iabeurope.eu/](https://www.iabeurope.eu/)。

## 第一步：瞭解ECID的選擇加入{#first-step-understand-ecid-s-opt-in}

要瞭解如何使用IAB TCF，您必須先瞭解[!DNL Opt-in]功能，這是Experience CloudID服務(ECID)程式庫的一部分。 如果您不熟悉「選擇加入」的運作方式，請先參閱[這篇有用的文章](https://docs.adobe.com/content/help/en/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html)。 您也應檢閱選擇加入[檔案](https://docs.adobe.com/content/help/zh-Hant/id-service/using/implementation/opt-in-service/optin-overview.translate.html)。 一旦您瀏覽了這些資源，請返回本頁並繼續。

## IAB TCF {#the-audience-manager-plug-in-for-iab-tcf}的Audience Manager插件

現在您至少已基本瞭解加入服務的運作方式，Audience Manager可以將[!DNL IAB Transparency and Consent Framework (TCF)]支援層級化，這是透過外掛程式加入加入選擇加入物件。

IAB TCF的Audience Manager外掛程式擴充了選擇加入的功能，讓客戶AAM可以根據IAB TCF評估、認可並轉寄使用者隱私權選擇給下游合作夥伴。 它提供資料控制者(即您是Adobe客戶)和廠商(DMP、DSPSSP、廣告伺服器等)的標準 可用來瞭解許可範圍內的許可。

## 啟用IAB TCF {#enabling-iab-tcf}

如果您使用Adobe Experience Platform Launch，啟用IAB TCF的Audience Manager插件很簡單，因為它是一個簡單的複選框，如以下短視頻所示：

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

或者，如果您不使用Launch，則可在實例化Experience Cloud訪客時使用`isIabContext=true`啟用它。 這開始IAB TCF流，即為許可收集添加另一個步驟，使用IAB TCF查詢IAB TC字串並將其提供給選擇加入，而選擇加入則與Experience Cloud解決方案通信。

## IAB TC字串{#iab-tcf-consent-string}

IAB提供的標準之一是「同意字串」（又稱為「DaisyBit」），這實際上是兩個清單的集合：

1. 目的：**同意做什麼？**
1. 供應商：**誰同意？**

### 用途{#purposes}

有了IAB TCF 2.0，就有十種「目的」要收集同意（廠商可以如何處理訪客資料）。 Adobe Audience Manager不要求全部十項，但除了供應商同意外，僅要求供應商同意以下目的：

* **用途1:** 在裝置上儲存及／或存取資訊；
* **目的十：開** 發和改進產品；
* **特殊目的1：確** 保安全性、防止詐欺和除錯。

這是IAB TC字串的第一部分，只會記錄為1和0，並指定該目的／活動是否獲得核准。

>[!NOTE]
>
>根據IAB法規，特殊用途1（確保安全性、防止欺詐和除錯）一律同意，且使用者無法反對。

### 供應商{#vendors}

IAB TC字串的另一部分是數百個廠商的長清單，讓訪客可以看到網站上有標籤的適用廠商清單，並可選擇要使用的廠商。 供應商在清單中佔據一席之地。 例如，Adobe Audience Manager的廠商編號在此清單上為565。 如果清單中的該數字有&quot;1&quot;，則Audience Manager可從清單前面執行核准的目的。 如AAM果位置有「0」，則無法處理資料。

**為了Audience Manager為客戶提供UI以使用IAB TCF來選擇這些用途和供應商，或批准／不批准所有活動，您必須使用向IAB TCF註冊的CMP合作夥伴，或建立支援IAB TCF並向IAB TCF註冊的CMP合作夥伴。**

## 選擇加入：IAB與Adobe解決方案之間的轉換{#opt-in-translating-between-iab-and-adobe-solutions}

使用IAB TCF的好處之一是，上述標準用途可能讓使用者更多地瞭解他們正在核准的內容，而非Adobe解決方案清單。 使用者可能不知道「核准」Audience Manager或[!DNL Target]的含義，但「儲存和／或存取裝置上的資訊」或「開發和改善產品」可能更容易讓他們瞭解並同意。

為了Audience Manager獲得核准(即，為了將IAB目的轉譯為選擇加入，以表示「是」AAM)，上列目的1和10必須獲得最終用戶的同意。 如果其中一個未核准，或供應商未核准，則不AAM會執行像素點火或設定Cookie。 此外，很多客戶只要選擇提供「全部或全部」的UI給使用者，當然會允許或禁止使用Audience Manager(以及其他Experience Cloud解決方案)，這也很好。

[documentation](https://marketing.adobe.com/resources/help/en_US/aam/aam-iab-plugin.html)中有一些關於IAB TCFAudience Manager外掛程式如何套用至Publisher和Advertiser使用案例的絕佳資訊。

## IAB:向下游發送許可{#iab-sending-consent-downstream}

當使用IAB TCF的Audience Manager外掛程式時，使用者的同意選擇也會傳送至平台層級（第三方）ID同步，以供出現在全球廠商清單中的合作夥伴使用，如此，合作夥伴就擁有使用者同意資訊，並可依照其行事。 此資訊會以兩個變數傳送：

* gdpr = 1
* gdpr_connency = [編碼同意字串]

但須注意的是，如果使用者處於IAB情境中且未提供同意（或提供負面同意），則Audience Manager根本不會收集IAB TC字串，因此會放棄呼叫。 所以，在那種情況下…… ...下游不能通過同意。

## 示範{#demo}

在以下影片中，瞭解ECID和解決方案的Cookie和信標如何受到IAB使用者選擇選擇的影響。

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

有關IAB TCF 2.0Audience Manager插件的更多詳細資訊，包括如何實施和測試、使用案例和工作流程，請參見[文檔](https://docs.adobe.com/content/help/en/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html)。
