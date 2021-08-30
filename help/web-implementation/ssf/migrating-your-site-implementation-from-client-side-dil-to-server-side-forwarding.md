---
title: 將網站的AAM實作從用戶端DIL移轉至伺服器端轉送
description: 如果您同時擁有Adobe Audience Manager(AAM)和Adobe Analytics，且您目前使用「DIL」(Data Integration Library)程式碼將點擊從頁面傳送至AAM，並同時從頁面傳送點擊至Adobe Analytics，本教學課程即適用。 由於您有這兩個解決方案，而且這兩個解決方案都屬於Adobe Experience Cloud的一部分，因此您可以遵循開啟「伺服器端轉送(SSF)」的最佳實務，這可讓Analytics資料收集伺服器將網站分析資料即時轉送至Audience Manager，而非讓用戶端代碼從頁面傳送額外點擊至AAM。 本教學課程將逐步引導您完成從舊版「用戶端DIL」實作切換至新版「伺服器端轉送」方法的步驟。
product: audience manager
feature: Adobe Analytics Integration
topics: null
activity: implement
doc-type: tutorial
team: Technical Marketing
kt: 1778
role: Developer, Data Engineer
level: Intermediate
exl-id: bcb968fb-4290-4f10-b1bb-e9f41f182115
source-git-commit: 4d4c12e9f9a33760a89460258c3802fcf3a4e22b
workflow-type: tm+mt
source-wordcount: '2318'
ht-degree: 0%

---

# 將網站的AAM實作從[!DNL Client-Side]DIL移轉至[!DNL Server-Side Forwarding] {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

如果您同時擁有Adobe Audience Manager(AAM)和Adobe Analytics，且您目前使用「DIL」([!DNL Data Integration Library])程式碼將點擊從頁面傳送至AAM，並同時從頁面傳送點擊至Adobe Analytics，本教學課程將適用於您。 由於您有這兩個解決方案，而且這兩個解決方案都屬於Adobe Experience Cloud的一部分，因此您有機會遵循開啟「[!DNL Server-Side Forwarding](SSF)」的最佳實務，這可讓[!DNL Analytics]資料收集伺服器將網站分析資料即時轉送至Audience Manager，而非讓[!DNL client-side]程式碼將額外的點擊從頁面傳送至AAM。 本教學課程將引導您完成從舊版「[!DNL Client-Side DIL]」實作切換至較新「[!DNL Server-Side forwarding]」方法的步驟。

## [!DNL Client-Side] (DIL)與  [!DNL Server-Side] {#client-side-dil-vs-server-side}

比較這兩種方法將Adobe Analytics資料匯入AAM時，首先將下列影像中的差異視覺化可能會有所幫助：

![用戶端對伺服器端](assets/client-side_vs_server-side_aam_implementation.png)

### [!DNL Client-side] DIL實作 {#client-side-dil-implementation}

如果您使用此方法將Adobe Analytics資料傳入AAM，表示您的網頁有兩個點擊：一個會前往[!DNL Analytics]，另一個會前往AAM（複製了網頁上的[!DNL Analytics]資料後）。 [!UICONTROL Segments] 會從AAM傳回至頁面，供個人化等作業使用。此為「舊版」實作，不再建議使用。

除了沒有遵循最佳做法以外，使用此方法的缺點包括：

* 來自頁面的兩次點擊，而非僅一次
* [!UICONTROL Server-Side Forwarding] 需要AAM對象的即時共用，因 [!DNL Analytics]此 [!DNL Client-side] 實作不會允許此功能（未來可能也不會有其他功能）

建議您移至[!UICONTROL Server-Side Forwarding] AAM實作方法。

### [!UICONTROL Server-Side Forwarding] 實施 {#server-side-forwarding-implementation}

如上圖所示，點擊會從網頁傳至Adobe Analytics。 [!DNL Analytics] 接著即時將該資料轉送至AAM，系統會評估訪客至 [!UICONTROL traits] AAM和 [!UICONTROL segments]，就像點擊是直接來自頁面一樣。

[!UICONTROL Segments] 會在同一次即時點擊傳回， [!DNL Analytics]這會將回應轉送至網頁以供個人化等。

移至伺服器端轉送沒有時間上的下限。 強烈建議同時擁有Audience Manager和[!DNL Analytics]的任何人使用此實作方法。

## 您有兩個主要任務 {#you-have-two-main-tasks}

這頁上有很多資訊，當然，這都很重要。 不過，**歸根到底可以歸結為您需要執行的兩項主要操作：**

1. 將程式碼從[!DNL Client-Side]DIL程式碼變更為[!UICONTROL Server-Side Forwarding]程式碼
1. 反向[!DNL Analytics] [!DNL Admin Console]中的交換機以開始實際資料轉發（根據[!UICONTROL report suite]）

如果您略過這兩個選項其中之一，SSF將無法正常運作。 已將步驟和其他資料新增至本檔案，協助您正確執行這兩個步驟以進行設定。

## 實作選項 {#implementation-options}

當您從[!DNL client-side]移至[!DNL server-side]時，您需要執行的其中一項任務是將程式碼變更為新的[!UICONTROL Server-Side Forwarding]程式碼。 這是使用下列其中一個選項來完成：

* Adobe Experience Platform Launch — 我們建議的Web屬性實作選項。 你將看到這是一項非常容易的任務，因為[!DNL Launch]已經為您做了所有的硬性工作。
* 在頁面上 — 如果您尚未使用AdobeLaunch，也可以將新的SSF程式碼直接放入[!DNL appMeasurement.js]檔案的`doPlugins`函式中
* 其他標籤管理程式 — 這些處理方式可像上一個（在頁面上）選項一樣，因為您仍會將SSF程式碼放在`doPlugins`中，其他標籤管理程式儲存[!DNL AppMeasurement]程式碼的位置

我們將在更新程式碼區段中查看下列各項。

## 實施步驟 {#implementation-steps}

### 步驟0:先決條件：Experience CloudID服務(ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

移至[!UICONTROL Server-Side Forwarding]的主要先決條件是實作Experience CloudID服務。 如果您使用Experience Platform Launch，這項作業最為輕鬆，在此情況下，您只需安裝ECID擴充功能，就能完成其餘工作。

如果您使用非AdobeTMS，或完全沒有TMS，則請實作ECID以在&#x200B;**任何其他Adobe解決方案之前執行**。 如需詳細資訊，請參閱[ECID檔案](https://experienceleague.adobe.com/docs/id-service/using/home.html) 。 唯一的其他先決條件是程式碼版本，因此您只需在下列步驟中套用程式碼的最新版本，即可正常運作。

>[!NOTE]
>
>實作前，請先閱讀整份檔案。 下方的「計時」一節包含有關&#x200B;*何時*&#x200B;您應實作每個片段的重要資訊，包括ECID（如果尚未實作）。

### 步驟1:從DIL代碼中記錄當前使用的選項 {#step-record-currently-used-options-from-dil-code}

當您準備好從[!DNL Client-Side]DIL程式碼移至[!UICONTROL Server-Side Forwarding]時，第一步就是識別您使用DIL程式碼執行的所有動作，包括自訂設定和傳送至AAM的資料。 需注意和考慮的事項包括：

* 一般[!DNL Analytics]變數，使用[!DNL siteCatalyst.init]DIL模組 — 您不需要擔心這個變數，因為其工作只是傳送一般[!DNL Analytics]變數過去，而這只是因為啟用了SSF而發生。
* 合作夥伴子網域 — 在DIL.create函式中，記下`partner`參數。 這稱為您的「合作夥伴子網域」，有時也稱為「合作夥伴ID」，當您放置新的SSF代碼時，將需要此ID。
* [!DNL Visitor Service Namespace]  — 也稱為「[!DNL Org ID]」或「[!DNL IMS Org ID]」，當您設定新的SSF程式碼時，也需要此項。記下來。
* containerNSID、uuidCookie和其他進階選項 — 記下您使用的任何其他進階選項，以便您也能在SSF程式碼中加以設定。
* 其他頁面變數 — 如果其他變數從頁面傳送至AAM（除了siteCatalyst.init處理的一般[!DNL Analytics]變數外），您還需要記下這些變數，以便透過SSF傳送(擾動程式警報：透過[!DNL contextData]變數)。

### 步驟2:更新程式碼 {#step-updating-the-code}

在上文「實作選項」一節中，針對您實作[!UICONTROL Server-Side Forwarding]的方式/位置提供多個選項。 為了讓本節生效，我們需要將其細分為以下幾節（其中兩節合併）。 移至本節最能說明您需求的方法。

#### Adobe Experience Platform Launch {#launch-by-adobe}

請觀看以下影片，了解如何將實作選項從[!DNL Client-Side]DIL程式碼移至Experience Platform Launch中的[!UICONTROL Server-Side Forwarding]。

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### 「在頁面上」或非Adobe Tag Manager {#on-the-page-or-non-adobe-tag-manager}

觀看以下影片，了解如何將實作選項從[!DNL Client-Side]DIL程式碼移至[!DNL AppMeasurement]程式碼中的[!UICONTROL Server-Side Forwarding]，且位於檔案或非Adobe標籤管理系統中。

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### 步驟3:啟用轉送（根據[!UICONTROL Report Suite]）{#step-enabling-the-forwarding-per-report-suite}

到目前為止，在本教學課程中，我們已花費所有時間將程式碼從[!DNL Client-Side DIL]程式碼切換至[!UICONTROL Server-Side Forwarding]。 這沒關係，因為這是更難的部分。 雖然您會看到此區段非常簡單，但與更新程式碼同樣重要。 在此影片中，您將了解如何翻轉開關，以實際將資料從Analytics轉送至Audience Manager。

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**注意：** 如影片所述，若要在Experience Cloud後端完全實作，啟用轉送最多需要4小時。

## 計時 {#timing}

提醒您，從[!DNL Client-Side DIL]移至[!UICONTROL Server-Side Forwarding]的主要任務有兩項：

1. 更新程式碼
1. 在[!DNL Analytics] [!DNL Admin Console]中翻轉開關

但問題是，你先做哪個？ 重要嗎？ 對不起，那是兩個問題。 但答案是……這取決於，是的，它&#x200B;*可以*&#x200B;重要。 這個怎麼樣？ 我們把它拆了。 但首先，如果您是擁有大量網站的大型組織，您可能會想出另外一個問題：我必須一次做所有事嗎？ 那個比較簡單。 不。 你可以逐件地做。:)

### 再深一點 {#a-little-deeper-dive}

時間和順序之所以重要，是因為轉送*實際運作方式，以下幾個技術事實可概括：

* 若您已實作Experience CloudID服務(ECID)，且[!DNL Analytics] [!DNL Admin Console](&quot;the switch&quot;)中的開關已開啟，即使您尚未更新程式碼，資料也會從[!DNL Analytics]轉送至AAM。
* 如果您尚未實作ECID，即使您已開啟開關，且有SSF程式碼，資料仍無法轉送。
* SSF程式碼（無論是在[!DNL Launch]中，還是在頁面上）會確實處理回應，且當然必須完成移轉。
* 請記住，SSF開關是由[!UICONTROL Report Suite]啟用，但是若您未使用[!DNL Launch]，則程式碼是由[!DNL Launch]中的屬性或[!DNL AppMeasurement]檔案處理

### 最佳實務 {#best-practices}

根據這些技術詳細資訊，以下是「何時該做什麼」的時間安排建議：

#### 如果您尚未實作ECID {#if-you-do-not-have-ecid-yet-implemented}

1. 針對要啟用SSF的每個[!UICONTROL report suite]，在[!DNL Analytics]中翻轉開關

   1. 因為您沒有ECID，轉送尚未開始

1. 根據網站，將您的程式碼從[!DNL Client-Side DIL]更新為SSF（這可能在[!DNL Launch]中，或在頁面上，如上方其他章節所述）

   1. 轉送功能現在會正常運作（如您已新增ECID），且您也應收到[!DNL Analytics]信標的適當JSON回應（如需詳細資訊，請參閱下方的驗證和疑難排解一節）

#### 如果您已實作ECID {#if-you-do-have-ecid-implemented}

1. 準備並計畫，好讓您準備好將程式碼從DIL更新為SSF PER [!UICONTROL report suite]，並為SSF啟用：

   1. 在[!DNL Analytics]中反轉開關以啟用SSF

      1. 轉送將啟動，因為您已啟用ECID
   1. 請盡快將您的程式碼從[!DNL Client-Side DIL]更新至SSF（這可能在[!DNL Launch]中，或在頁面上，如上方其他章節所述）

      1. 您應會收到[!DNL Analytics]信標的正確JSON回應（如需詳細資訊，請參閱下方的驗證和疑難排解一節）


**注意1:** 請務必盡量將這兩個步驟彼此接近，因為在上述步驟1和2之間，您會有進入AAM的重複資料。換言之，SSF將已開始將資料從[!DNL Analytics]傳送至AAM，由於DIL程式碼仍在頁面上，因此也會有點擊直接從頁面傳入AAM，因此資料加倍。 一旦您將程式碼從DIL更新為SSF，情況就會有所緩解。

**注2:** 如果您寧可略微差異資料，也不願略微重複資料，則可切換上述步驟1和2的順序。將程式碼從DIL移至SSF會停止資料流入AAM，直到您能夠扳動開關以開啟[!UICONTROL report suite]的SSF為止。 通常客戶寧願將資料加倍一小部分，也不願錯過將訪客帶入[!UICONTROL traits]和[!UICONTROL segments]的機會。

#### 有多個網站和[!UICONTROL Report Suites]時的遷移時間 {#migration-timing-when-you-have-many-sites-and-report-suites}

在前幾節中簡要介紹了本主題，主要戰略可歸納如下：

一次遷移一個站點/[!UICONTROL report suite]（或一組站點/[!UICONTROL report suites]）。

不過，根據一些可能的情況，這可能會有些棘手：

* 您的網站包含數個不同的[!UICONTROL report suites]
* 您有[!UICONTROL report suite]，其中包含數個網站（例如全域[!UICONTROL report suite]）
* 您使用一個[!DNL Launch]屬性來涵蓋多個網站
* 您有不同的開發團隊，負責不同的網站

因為有這些東西，它會變得有點複雜。 我建議的最好的事情是：

* 請根據上述說明，花點時間制定移轉至SSF的策略
* 根據[!DNL Launch]中的單一屬性（或單一[!DNL AppMeasurement]檔案）通常對應至1或2個相異[!UICONTROL report suites]的事實，您可能可以逐一建立適用於這些不同群組的計畫，將您的企業更新為SSF
* 如果您正在與Adobe諮詢服務合作，請洽詢客戶有關您的移轉計畫，以便他們視需要提供協助

## 驗證和疑難排解 {#validation-and-troubleshooting}

驗證[!UICONTROL Server-Side Forwarding]是否正常運作的主要方法是查看來自應用程式的任何Adobe Analytics點擊回應。

如果您未從[!DNL Analytics]執行[!UICONTROL server-side forwarding]至Audience Manager的資料，則[!DNL Analytics]信標（除了2x2像素以外）沒有任何回應。 不過，如果您執行SSF,[!DNL Analytics]要求和回應中就會有可驗證的項目，讓您知道[!DNL Analytics]正在與Audience Manager正確通訊、轉送點擊並取得回應。

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

**警告：** 請注意錯誤的「成功」 — 如果有回應，且一切看似正常運作，請確定您的回應中有「stuff」物件。如果沒有，您可能會看到顯示[!DNL "status":"SUCCESS"]的訊息。 雖然聽起來很瘋狂，但這實際上證明了它並非正常運作。 如果您看到這個訊息，表示您已在[!DNL Launch]或[!DNL AppMeasurement]中完成程式碼更新，但[!DNL Analytics] [!DNL Admin Console]中的轉送尚未完成。 在此情況下，您需要確認您已為[!UICONTROL report suite]在[!DNL Analytics] [!DNL Admin Console]中啟用SSF。 如果您已執行且尚未完成4小時，請耐心等候，因為在後端進行所有必要變更可能需要這麼久的時間。

![誤判](assets/falsesuccess.png)

有關[!UICONTROL Server-Side Forwarding]的詳細資訊，請參閱[文檔](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html)。
