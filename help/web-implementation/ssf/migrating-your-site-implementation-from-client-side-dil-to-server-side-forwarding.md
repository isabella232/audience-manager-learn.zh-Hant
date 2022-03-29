---
title: 將站點的Audience Manager實現從客戶端DIL遷移到伺服器端轉發
description: 瞭解如何將站點的Audience Manager(AAM)實施從客戶端DIL遷移到伺服器端轉發。 本教程適用於AAM您同時具有和Adobe Analytics，並且您從頁面發送命中到使用DIL(Data Integration Library)代AAM碼，並且還從頁面發送命中到Adobe Analytics。
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
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '2333'
ht-degree: 0%

---

# 將站點的Audience Manager實現從客戶端DIL遷移到伺服器端轉發 {#migrating-your-site-s-aam-implementation-from-client-side-dil-to-server-side-forwarding}

本教程適用於您(AAM如果您同時擁有Adobe Audience Manager()和Adobe Analytics，並且您當前正在從頁面發送命AAM中到使用DIL([!DNL Data Integration Library])代碼，並從頁面發送命令到Adobe Analytics。 由於您同時擁有這兩個解決方案，而且它們都是Adobe Experience Cloud的一部分，因此您有機會遵循開啟伺服器端轉發的最佳做法，這使 [!DNL Analytics] 資料收集伺服器將網站分析資料即時轉發到Audience Manager，而不是讓客戶端代碼從頁面向發送附加命中AAM。 本教程將引導您完成以下步驟：將交換機從較舊的客戶端DIL實現轉換到較新的伺服器端轉發方法。

## 客戶端(DIL)與伺服器端 {#client-side-dil-vs-server-side}

在比較和對比兩種將Adobe Analytics資料導入到其中的方AAM法時，首先可以直觀地顯示下列影像中的差異：

![客戶端到伺服器端](assets/client-side_vs_server-side_aam_implementation.png)

### 客戶端DIL實現 {#client-side-dil-implementation}

如果使用此方法將Adobe Analytics資料AAM導入，則您的Web頁面會有兩個點擊：一個 [!DNL Analytics]，將要(在AAM複製了 [!DNL Analytics] 網頁上的資料。 [!UICONTROL Segments] 從頁AAM面返回，這些頁面可用於個性化等。 這被視為舊式實施，不再推薦。

除了不遵循最佳做法之外，使用此方法的缺點包括：

* 從網頁上點擊兩次，而不是只點擊一次
* 伺服器端轉發是即時共用受眾到AAM的必需 [!DNL Analytics]，因此客戶端實現不允許使用此功能（將來可能還有其他功能）

建議您轉到伺服器端的轉發實現方AAM法。

### 伺服器端轉發實現 {#server-side-forwarding-implementation}

如上圖所示，網頁上有一條點擊線到Adobe Analytics。 [!DNL Analytics] 然後將資料實AAM時轉發給訪問者，並將其評AAM估為特徵 [!UICONTROL segments]就好像是直接從這頁來的。

[!UICONTROL Segments] 在同一即時回擊時返回 [!DNL Analytics]將響應轉發到網頁進行個性化等。

切換到伺服器端轉發沒有時間不足。 Adobe強烈建議任何同時具有Audience Manager和 [!DNL Analytics] 使用此實現方法。

## 您有兩個主要任務 {#you-have-two-main-tasks}

這頁上有很多資訊，當然，這都很重要。 但是， **都歸結為兩件你需要做的**:

1. 將代碼從客戶端DIL代碼更改為伺服器端轉發代碼
1. 在 [!DNL Analytics] [!DNL Admin Console] 開始資料的實際轉發(按 [!UICONTROL report suite])

如果跳過其中任一任務，伺服器端轉發將無法正常工作。 已將步驟和其他資料添加到此文檔，以幫助您正確執行這兩個步驟進行設定。

## 實施選項 {#implementation-options}

在從客戶端轉發到伺服器端轉發時，您將要執行的任務之一是將代碼更改為新的伺服器端轉發代碼。 這是使用以下選項之一完成的：

* Adobe Experience Platform標籤 — Adobe推薦的Web屬性實現選項。 您將看到，這是一項簡單的任務，因為平台標籤已經為您完成了所有的辛勤工作。
* 在頁面上 — 您還可以將新的SSF代碼直接放入 `doPlugins` 功能 `appMeasurement.js` 檔案，如果尚未使用Adobe啟動
* 其他標籤管理器 — 這些標籤可以像上一個（在頁面上）選項一樣處理，因為您仍將將SSF代碼放在 `doPlugins`，其他標籤管理器儲存的 [!DNL AppMeasurement] 代碼

我們將在下面的每一個 _更新代碼_ 的子菜單。

## 實施步驟 {#implementation-steps}

以下步驟描述了實施過程。

### 步驟0:先決條件：Experience CloudID服務(ECID) {#step-prerequisite-experience-cloud-id-service-ecid}

轉到伺服器端的主要先決條件是實現Experience CloudID服務。 如果您使用Experience Platform Launch，則最容易完成此操作，在這種情況下，您只需安裝ECID擴展，而它將完成其餘操作。

如果您使用的是非AdobeTMS，或根本沒有TMS，請實現ECID以運行 **先** 任何其他Adobe。 查看 [ECID文檔](https://experienceleague.adobe.com/docs/id-service/using/home.html) 的子菜單。 唯一的其他先決條件是有關代碼版本，因此，只需在以下步驟中應用代碼的最新版本，您就可以了。

>[!NOTE]
>
>請在實施前閱讀整個文檔。 下面的「Timing」部分提供了 *何時* 您應實現每個條目，包括ECID（如果尚未實現）。

### 步驟1:記錄當前使用的DIL代碼選項 {#step-record-currently-used-options-from-dil-code}

在您準備好從客戶端DIL代碼轉移到伺服器端轉發時，第一步是確定您正在使用DIL代碼執行的所有操作，包括自定義設定和發送到的數AAM據。 需要注意和考慮的事項包括：

* 正常 [!DNL Analytics] 變數，使用 `siteCatalyst.init` DIL模組 — 您無需擔心此模組，因為其工作只是發送正常 [!DNL Analytics] 變數已過，而這僅僅是因為啟用了伺服器端轉發。
* 合作夥伴子域 — 在 `DIL.create` 函式，記下 `partner` 的下界。 這稱為「合作夥伴子域」，有時稱為「合作夥伴ID」，在您放置新的伺服器端轉發代碼時，將需要它。
* [!DNL Visitor Service Namespace]  — 也稱為「」[!DNL Org ID]&quot;或&quot;[!DNL IMS Org ID]「設定新的伺服器端轉發代碼時，您也需要這個。 記下來。
* containerNSID、uuidCookie和其他高級選項 — 記下您正在使用的任何其他高級選項，以便您也可以在伺服器端轉發代碼中設定它們。
* 附加頁變數 — 如果從頁面發送其AAM他變數（除常規變數外） [!DNL Analytics] 由siteCatalyst.init處理的變數)，您需要記錄這些變數，以便通過伺服器端轉發(劇透警報：通過 [!DNL contextData] 變數)。

### 步驟2:更新代碼 {#step-updating-the-code}

在 [實施選項](#implementation-options) （上面），將提供多個選項，說明您如何以及在何處實現伺服器端轉發。 為了使本部分有效，我們需要將其分為以下幾部分（其中兩部分合併）。 轉至本節最能說明您需求的方法。

#### Adobe Experience Platform標籤 {#launch-by-adobe}

觀看下面的視頻，瞭解如何將實施選項從客戶端DIL代碼移入伺服器端Experience Platform Launch轉發。

>[!VIDEO](https://video.tv.adobe.com/v/26310/?quality=12)

#### 「在頁面上」或非Adobe標籤管理器 {#on-the-page-or-non-adobe-tag-manager}

觀看以下視頻，瞭解如何將實施選項從客戶端DIL代碼移入伺服器端轉發 [!DNL AppMeasurement] 代碼，駐留在檔案或非Adobe標籤管理系統中。

>[!VIDEO](https://video.tv.adobe.com/v/26312/?quality=12)

### 第3步：啟用轉發(每個 [!UICONTROL Report Suite]) {#step-enabling-the-forwarding-per-report-suite}

在本教程中，到目前為止，我們一直在將代碼從客戶端DIL代碼切換到伺服器端轉發。 這沒關係，因為那是更難的部分。 此部分雖然非常簡單，但與更新代碼同樣重要。 在此視頻中，您將看到如何翻轉使資料從分析實際轉發到Audience Manager的開關。

>[!VIDEO](https://video.tv.adobe.com/v/26355/?quality-12)

**注：** 如視頻中所述，請記住，在Experience Cloud後端完全實現轉發功能需要4小時。

## 計時 {#timing}

提醒一下，從客戶端DIL到伺服器端轉發有兩個主要任務：

1. 更新代碼
1. 在 [!DNL Analytics] [!DNL Admin Console]

但問題是，你先做哪個？ 重要嗎？ 好，抱歉，有兩個問題。 但答案是……取決於情況，是的 *能* 很重要。 這對Vague怎麼樣？ 我們先分開。 但首先，如果您是一個擁有眾多站點的大型組織，您還可以再問一個問題：我必須馬上做一切嗎？ 那個比較簡單。 不。 你可以一個接一個地做。

### 再深一點 {#a-little-deeper-dive}

時序和訂單之所以重要，是因為轉發方式 _真的_ 主要工作包括以下幾個技術事實：

* 如果實現了Experience CloudID服務(ECID)，並且 [!DNL Analytics] [!DNL Admin Console] （「交換機」）已開啟，資料將從 [!DNL Analytics] 至AAM，即使您尚未更新代碼。
* 如果未實現ECID，則即使您開啟了交換機，並且具有伺服器端轉發代碼，資料也不會轉發。
* 伺服器端轉發代碼（無論是在平台標籤中還是在頁面上）確實處理了響應，是完成遷移所必需的。
* 請記住，伺服器端轉發交換機由 [!UICONTROL report suite]，但代碼由Platform標籤中的屬性處理，或由 [!DNL AppMeasurement] 的子菜單。

### 最佳實務 {#best-practices}

根據這些技術詳細資訊，以下是關於何時執行和何時執行的建議：

#### 如果尚未實現ECID {#if-you-do-not-have-ecid-yet-implemented}

1. 開啟開關 [!DNL Analytics] 每 [!UICONTROL report suite] 用於伺服器端轉發。

   1. 轉發尚未啟動，因為您沒有ECID。

1. 根據站點，將代碼從客戶端DIL更新為伺服器端轉發（這可能位於平台標籤中）或在頁面上（如上面另一節所述）。

   1. 現在轉發流量（因為您已添加了ECID），您還應收到對您的JSON響應 [!DNL Analytics] 信標（有關詳細資訊，請參閱下面的驗證和故障排除部分）。

#### 如果確實實施了ECID {#if-you-do-have-ecid-implemented}

1. 準備並規劃，以便您準備好將代碼從DIL更新到伺服器端轉發PER [!UICONTROL report suite] 用於伺服器端轉發：

   1. 開啟開關 [!DNL Analytics] 啟用伺服器端轉發。

      1. 將啟動轉發，因為您已啟用ECID。
   1. 盡快將代碼從客戶端DIL更新為單端轉發（這可能位於平台標籤中或頁面上，如上面另一節所述）。

      1. 您應收到正確的JSON響應 [!DNL Analytics] 信標(請參閱 [驗證和故障排除](#validation-and-troubleshooting) 下面的部分，瞭解更多詳細資訊)。


>[!NOTE]
>
>這兩個步驟必須盡可能彼此接近，因為在以上步驟1和2之間，您將有重複的資料進入AAM。 換句話說，單端轉發將開始從 [!DNL Analytics] 至，AAM並且由於DIL代碼仍在頁面中，因此也會出現從頁面直接進入頁面的命AAM令，從而使資料倍增。 一旦將代碼從DIL更新到伺服器端轉發，這一點將得到緩解。

>[!NOTE]
>
>如果您更願意在資料方面有一點差異，而不是在資料上有一點重複，則可以切換上面步驟1和2的順序。 將代碼從DIL移到伺服器端轉發會停止資料流進入AAM，直到您能夠開啟交換機以開啟伺服器端轉發 [!UICONTROL report suite]。 通常，客戶寧可將資料略加倍，也不會錯過讓訪問者瞭解特性和 [!UICONTROL segments]。

#### 在您有多個站點和 [!UICONTROL report suites] {#migration-timing-when-you-have-many-sites-and-report-suites}

在前幾節中對本主題作了簡要的介紹，主要戰略可概括如下：

遷移一個站點/[!UICONTROL report suite] (或一組地點/[!UICONTROL report suites])。

不過，根據幾種可能的情形，這可能會有些棘手：

* 您的站點包含多個不同 [!UICONTROL report suites]
* 您有 [!UICONTROL report suite] 包括多個站點(如 [!UICONTROL report suite])
* 您使用一個平台標籤屬性覆蓋多個站點
* 您擁有不同站點的不同開發團隊

因為這些東西，它會變得有點複雜。 我能建議的最好事情是：

* 根據上面所介紹的內容制定遷移到伺服器端轉發的策略需要一些時間
* 基於Platform標籤中的單個屬性(或 [!DNL AppMeasurement] 檔案通常映射到1或2個不同 [!UICONTROL report suites]，您可能會制定一個計畫來逐個處理這些不同的組，將企業更新為伺服器端轉發
* 如果您正在與Adobe咨詢部門合作，請與他們討論您的遷移計畫，以便他們能夠根據需要提供幫助

## 驗證和故障排除 {#validation-and-troubleshooting}

驗證伺服器端轉發是否已啟動並正在運行的主要方法是查看對來自該應用的任何Adobe Analytics點擊的響應。

如果您不從伺服器端轉發資料 [!DNL Analytics] 到Audience Manager，那麼你根本沒有 [!DNL Analytics] 信標（除2x2像素外）。 但是，如果您正在執行伺服器端轉發，則可以在 [!DNL Analytics] 請求和回應，讓您知道 [!DNL Analytics] 正確與Audience Manager通信、轉發命中並獲得響應。

>[!VIDEO](https://video.tv.adobe.com/v/26359/?quality=12)

>[!WARNING]
>
>當心錯誤的&quot;成功&quot; 如果有反應，一切似乎都正常，請確保 `stuff` 對象。 如果你沒有，你可能會看到一條消息 `"status":"SUCCESS"`。 儘管這聽起來很瘋狂，但這實際上證明它沒有正確工作。
>
>如果您看到這一點，則表示您已完成平台標籤或 [!DNL AppMeasurement]但是，在 [!DNL Analytics] [!DNL Admin Console] 尚未完成。 在這種情況下，您需要驗證是否在 [!DNL Analytics] [!DNL Admin Console] 為 [!UICONTROL report suite]。 如果您有，而且還沒有4小時，請耐心等待，因為在後端進行所有必要的更改可能需要那麼長時間。


![失敗](assets/falsesuccess.png)

有關伺服器端轉發的詳細資訊，請參閱 [文檔](https://experienceleague.adobe.com/docs/analytics/admin/admin-tools/server-side-forwarding/ssf.html)。
