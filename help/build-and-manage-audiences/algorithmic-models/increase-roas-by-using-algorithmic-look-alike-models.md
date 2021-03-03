---
title: 在Audience Manager中使用演算法（類似）模型來提高ROAS
description: Audience Manager相似模型的真正威力，是您針對來自第二方與第三方資料來源的優質、全新的使用者群，尋求擴大基準受眾。 在本教學課程中，瞭解從這些資料建立模型的步驟。
feature: 演算法模型
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 25188.jpg
kt: 1849
role: 「業務從業人員、開發人員、資料工程師、架構師、資料架構師、管理員、領導者」
level: 中級
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '873'
ht-degree: 0%

---


# 在Audience Manager{#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}中使用演算法（相似）[!UICONTROL Models]來增加ROAS

當您設法針對[!UICONTROL second party]和[!UICONTROL third party] [!UICONTROL data sources]的優質、全新使用者群，擴展您的基準受眾時，Audience Manager的相似[!UICONTROL Modeling]才是真正的力量。 在本教學課程中，瞭解從此資料建立[!UICONTROL model]所需的步驟。

## 從Audience Marketplace{#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}啟用[!UICONTROL Second Party]或[!UICONTROL Third Party]資料流

為了在類似[!UICONTROL model]中使用[!UICONTROL second party]和[!UICONTROL third party]資料，我們必須先將此資料啟用至您的Audience Manager介面。 Adobe有大量[!UICONTROL second party]和[!UICONTROL third party]資料提供者，您可從中選擇。 您可透過Audience Marketplace，在自助介面中取AAM得這些功能。 導覽至Audience Marketplace並瀏覽各種可能。 以下視訊將告訴您如何做到，包括如何啟用免費的「購買前先試用」串流，以便您鎖定對您的組織最有用的資料，然後再提交資料供應商的價格。

此外，為協助您研究並決定要使用哪個資料提供者，[[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/)是一項絕佳的資源。

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 識別／建立理想的使用者（轉換）[!UICONTROL trait]或[!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

您想要在您的網站上吸引訪客做什麼？ 您的轉換事件是什麼？ 當然，這個問題有許多不同的答案，視您的網站類型／垂直和組織目標而定。 無論如何，通常都AAM會為符合這些條件的訪客建立[!UICONTROL trait]。

在下面的影片中，我將示範如何建立轉換[!UICONTROL trait]，當您繼續進行本教學課程並建立類似的[!UICONTROL model]時，您會希望轉換。

此外，當使用Adobe Analytics事件建立[!UICONTROL traits]時，您需要記住一個重大問題，如此您才不會收集到超過您在[!UICONTROL trait]中應收集的使用者。 觀看以下影片，瞭解大的顯示效果。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 在上述影片中，我展示的範例假設您有Adobe Analytics。顯然，情況可能並非如此。 如果您有Google Analytics(GA)，我們有一個模組，您可以用它來傳送資料AAM（請參閱[documentation](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)），如果您的網站轉換活動是由GA傳送AAM至，則您可以從中建立轉換[!UICONTROL trait]。 如果您有不同的分析解決方案（或沒有分析解決方案），您仍可以透過我們的AAMDIL程式碼和`submit`函式傳送資料給。 （請參閱[documentation](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)）。 然後，根據在網站上執行轉換活動時傳送的資料來建立轉換[!UICONTROL trait]。

## 從[!UICONTROL Second Party]或[!UICONTROL Third Party]資料{#create-a-look-alike-model-from-2nd-or-3rd-party-data}建立類似的[!UICONTROL Model]

完成上述步驟後，我們現在可以建立演算法（類似）[!UICONTROL Model]。 在設定[!UICONTROL model]時，我們會使用轉換[!UICONTROL trait]作為我們的基本[!UICONTROL trait]（我們要複製的關鍵訪客），我們會使用啟用的[!UICONTROL third party]資料串流作為我們的人員集合。

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 重要的最佳實踐{#an-important-best-practice}

當在Audience Manager中建立演算法[!UICONTROL model]時，顯然我們希望[!UICONTROL model]盡可能有效。 由於[!UICONTROL model]正在考慮基本[!UICONTROL trait]/[!UICONTROL segment]成員屬於的所有[!UICONTROL traits]，因此如果所有成員都在[!UICONTROL trait]/[!UICONTROL segment]中，則對[!UICONTROL model]沒有幫助。 因此，如果您有任何超級通用[!UICONTROL traits]（如前往您網站的所有人，或收到您任何廣告的所有人等），請確定其所屬的[!UICONTROL data source]未包含在[!UICONTROL model]的[!UICONTROL data sources]中。 在本文的使用案例中，您不太可能這麼做，因為我們致力於查看[!UICONTROL third party]資料，以取得我們的新外觀，但無論如何都值得一提，並且適用於您的演算法[!UICONTROL models]。

## 建立演算法[!UICONTROL Trait] {#creating-an-algorithmic-trait}

接下來，我們需要建立演算法[!UICONTROL Trait]，以便使用[!UICONTROL model]的結果。 如果不建立[!UICONTROL trait]，則模型無用。 因此，在[!UICONTROL model]執行後，請務必進入[!UICONTROL trait]對話方塊並建立演算法[!UICONTROL Trait]。 以下視訊將逐步檢視，並顯示一些提示。

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## 從[!UICONTROL Model]資料建立[!UICONTROL Segment]並傳送至DSP{#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

建立演算法[!UICONTROL Trait]後，您可以建立新的[!UICONTROL segment]來放入，以便啟動資料(您無法啟動[!UICONTROL trait]，而是使用演算法[!UICONTROL Trait]建立新的[!UICONTROL trait] [!UICONTROL segment]，以便啟動（使用）[!UICONTROL segment])。

一旦您從此演算法[!UICONTROL trait]建立[!UICONTROL segment]後，您就會有潛在客戶的觀眾，這些潛在客戶看起來就像您網站上已轉換的人。 現在，您可以將此[!UICONTROL segment]對應至您的任DSP何Audience Manager中的[!UICONTROL destinations]。 您可以將行銷目標鎖定在那些在網站上轉換的訪客，而不只是一般大眾，從而提高廣告支出的回報。 祝你好運！
