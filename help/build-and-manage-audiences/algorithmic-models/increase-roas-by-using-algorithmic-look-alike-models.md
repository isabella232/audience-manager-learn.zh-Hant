---
title: 使用演演算法（相似）模型增加ROAS
description: 當您尋求針對來自第二方和第三方資料來源的高品質、全新使用者集擴展您的基線對象時，Audience Manager相似建模的真正力量就來了。 在本教學課程中，瞭解根據此資料建立模型的步驟。
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: User, Developer, Data Engineer, Architect, Data Architect, Admin, Leader
level: Intermediate
exl-id: 6626ae11-8709-4302-9e03-0d55878d2409
source-git-commit: 2094d3bcf658913171afa848e4228653c71c41de
workflow-type: tm+mt
source-wordcount: '909'
ht-degree: 0%

---

# 在Audience Manager中使用演演算法（相似）模型來增加ROAS {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

Audience Manager相似人群的真正力量 [!UICONTROL Modeling] 當您尋求根據來自第二方和第三方資料來源的全新高品質使用者集來擴展基準受眾時。 在本教學課程中，瞭解從此資料建立模型所需的步驟。

## 啟用來自Audience Marketplace的第二方或第三方資料串流 {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

為了在相似模型中使用第二方和第三方資料，我們首先必須將此資料啟用至您的Audience Manager介面。 Adobe有大量第二方和第三方資料提供者可供您選擇。 這些可透過Audience Marketplace在AAM的自助式介面中提供。 導覽至Audience Marketplace並瀏覽各種可能性。 以下影片將說明如何執行此操作，包括如何啟用免費的「購買前試用」資料流，以便在您認可資料提供者的定價之前，鎖定對您的組織最有用的資料。

此外，為了協助您研究並決定要使用哪個資料提供者，一個絕佳的資源是 [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/).

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 識別或建立理想的使用者（轉換）特徵或區段 {#identify-create-an-ideal-user-conversion-trait-or-segment}

您想讓訪客在您的網站上執行什麼動作？ 什麼是轉換事件？ 當然，根據您的網站型別/垂直和您的組織目標，此問題有許多不同的答案。 無論如何，在AAM中，為符合這些條件的訪客建立特徵是很常見的。

在下方影片中，我會說明如何建立轉換特徵，當您繼續完成本教學課程並建立相似模型時，就會想要具備此特徵。

此外，使用Adobe Analytics事件建立特徵時，請牢記以下主要需知，以免將超過應有數量的使用者收集進特徵中。 觀看以下影片以瞭解重大揭露。 ：)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 在上述影片中，我顯示的範例假設您有Adobe Analytics。 顯然，情況可能並非如此。 如果您有Google Analytics(GA)，我們有一個模組，可用來將資料傳送至AAM (請參閱 [檔案](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html))，而如果您網站上的轉換活動是在GA前傳送至AAM，則您可以從中建立轉換特徵。 AAM如果您有其他分析解決方案（或沒有分析解決方案），您仍可透過我們的DIL代碼和 `submit` 函式等 (請參閱 [檔案](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html))。 然後，根據網站上執行轉換活動時傳送的資料建立轉換特徵。

## 從第二方或第三方資料建立相似模型 {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

完成上述步驟後，我們現在已準備好建立演演算法（相似）模型。 在設定模型時，我們會使用轉換特徵作為基本特徵（我們要複製的關鍵訪客），並使用已啟用的協力廠商資料流作為我們要提取的人員集區。

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 重要的最佳實務 {#an-important-best-practice}

在Audience Manager中建立演演算法模型時，我們顯然希望模型儘可能有效。 由於模型會考慮基本特徵/區段成員屬於的所有特徵，因此如果所有人員都位於特徵/區段中，對模型沒有幫助。 因此，如果您有任何超級一般特徵（就像造訪您網站的人，或收到您任何廣告的人等），請確保其所屬的資料來源不會包含在您模型中的資料來源中。 就本文的使用案例而言，您不太可能這麼做，因為我們專注於檢視第三方資料以瞭解我們的新相似外觀，但無論如何，這值得一提，並適用於您的所有演演算法模型。

## 建立 [!UICONTROL Algorithmic Trait] {#creating-an-algorithmic-trait}

接下來，我們需要建立  [!UICONTROL Algorithmic Trait]，以便可以使用模型的結果。 如果不建立特徵，模型就毫無用處。 因此，在模型執行後，請務必進入特徵對話方塊並建立 [!UICONTROL Algorithmic Trait]. 以下影片會逐步解說，並顯示一些秘訣。

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## 從模型資料建立區段並傳送至DSP {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

建立 [!UICONTROL Algorithmic Trait]，您可以建立新區段來放置它，這樣您就可以啟用資料(您無法啟用特徵，而是要使用建立新的單一特徵區段 [!UICONTROL Algorithmic Trait] 以啟動（使用）區段)。

一旦您根據此演演算法特徵建立區段後，就會有一批潛在客戶，看起來就像是網站上已轉換的人。 現在，您可以在Audience Manager中將此區段對應至您的任何DSP目的地。 您將能夠將您的行銷目標鎖定於看起來類似的人，這些人更有可能在您的網站上轉化，而不是平常的大眾，從而提高您的廣告支出報酬率。
