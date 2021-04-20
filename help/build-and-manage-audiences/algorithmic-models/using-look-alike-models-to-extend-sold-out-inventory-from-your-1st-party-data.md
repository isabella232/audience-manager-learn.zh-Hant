---
title: 使用類似模型擴充第一方資料的已售庫存
description: 在本教學課程中，我們將逐步介紹您設定和使用相似模型時應採取的步驟，以便您能夠建立新的相似觀眾，並將其作為轉換區段的延伸銷售。
feature: Algorithmic Models
topics: null
activity: use
doc-type: feature video
team: Technical Marketing
thumbnail: 23523.jpg
kt: 1688
role: "Business Practitioner, Developer, Data Engineer, Architect, Data Architect, Administrator, Leader"
level: Intermediate
translation-type: tm+mt
source-git-commit: a7dc335e75697a7b1720eccdadbb9605fdeda798
workflow-type: tm+mt
source-wordcount: '838'
ht-degree: 0%

---


# 使用類似[!UICONTROL Models]來擴充[!UICONTROL First Party]資料{#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}中的已售庫存

在本教學課程中，我們將逐步介紹您設定和使用Look-Lake [!UICONTROL Models]時應採取的步驟，以便您能建立新的類似對象，並將它們作為轉換[!UICONTROL segment]的延伸銷售。

## 使用案例詳細資訊{#use-case-details}

您是內容發佈者。 如果您已經為網站上的轉換器售出了庫存，您可能會認為您的機會到此為止。 輸AAM入相似的[!UICONTROL Models]。 透過使用此功能，您可以進一步擴充已售完的庫存，並銷售可能尚未轉換，但外觀／行為與已轉換者相似的受眾。 此對象[!UICONTROL segment]通常的售價低於實際的轉換器，但是，您仍可為想要在您網站上投放廣告的廣告商提供額外的對象選項，以增加獲利底限。 此使用案例的額外好處是，在您的第一方資料上執行此模型並不需要花費任何成本。

本教學課程的步驟如下：

1. 識別／建立理想的使用者（轉換）[!UICONTROL trait]或[!UICONTROL segment]
1. 使用此轉換[!UICONTROL trait]/[!UICONTROL segment]作為基本項目建立[!UICONTROL model]
1. 在[!UICONTROL model]中選擇[!UICONTROL First party]資料源並運行[!UICONTROL model]
1. 從[!UICONTROL model]結果建立演算法[!UICONTROL Trait]，並將[!UICONTROL trait]新增至[!UICONTROL segment]
1. 將[!UICONTROL segment]提供給感興趣的廣告商，以延伸轉換[!UICONTROL segment]銷售

## 識別／建立理想的使用者（轉換）[!UICONTROL trait]或[!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

您想要在您的網站上吸引訪客做什麼？ 您的轉換事件是什麼？ 當然，這個問題有許多不同的答案，視您的網站類型／垂直和組織目標而定。 無論如何，通常都AAM會為符合這些條件的訪客建立[!UICONTROL trait]。

在此使用案例中，已假設這是因為您已將庫存售罄給轉換器使用者。 不過，在本教學課程中，最好將它作為其餘使用案例的參考。

此外，當使用事件建立[!UICONTROL traits]時，您需要記住一個重大問題，以免收集到的使用者數量超過您在[!UICONTROL trait]中應收集的數量。 觀看以下影片，瞭解大的顯示效果。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 在上述影片中，我展示的範例假設您有Adobe Analytics。顯然，情況可能並非如此。 如果您有Google Analytics(GA)，我們有一個模組可讓您用來傳送資料至（請參閱[documentation](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html)）AAM，如果您的網站轉換活動是由GA傳送AAM至，則您可以從中建立轉換特徵。 如果您有不同的分析解決方案（或沒有分析解決方案），您仍可以透過我們的AAMDIL程式碼和`submit`函式傳送資料給。 （請參閱[documentation](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html)）。 然後，再根據在網站上執行轉換活動時所傳送的資料，建立轉換特徵。

## 從[!UICONTROL First Party]資料{#creating-a-look-alike-model-from-first-party-data}建立相似[!UICONTROL Model]

在此步驟中，我們將建立[!UICONTROL First Party]相似[!UICONTROL Model]。 這表示我們不僅將使用[!UICONTROL first party]轉換[!UICONTROL trait]/[!UICONTROL segment]作為基本[!UICONTROL trait]/[!UICONTROL segment]（這對大多數[!UICONTROL models]來說都是正常的），而且我們也只會針對更多看起來像轉換器的人使用[!UICONTROL first party]資料庫。 我們不會查看[!UICONTROL second party]或[!UICONTROL third party]資料。

在此使用案例中，這很重要，因為我們嘗試在我們的網站上建立[!UICONTROL segment]個看起來像是轉換器但尚未轉換的使用者，因此我們可以將這個類似[!UICONTROL segment]的產品銷售給感興趣的廣告商。

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## 建立演算法[!UICONTROL Trait] {#creating-an-algorithmic-trait}

接下來，我們需要建立演算法[!UICONTROL Trait]，以便使用[!UICONTROL model]的結果。 若不建立[!UICONTROL trait]，則[!UICONTROL model]無用。 因此，在[!UICONTROL model]執行後，請務必進入[!UICONTROL trait]對話方塊並建立演算法[!UICONTROL Trait]。 以下視訊將逐一檢視，並顯示一些提示。

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## 將演算法[!UICONTROL Segment]提供給廣告商{#offering-the-algorithmic-segment-to-advertisers}

建立演算法[!UICONTROL Trait]後，您可以建立新的[!UICONTROL segment]來放入，如此您就可以啟動資料(無法啟動[!UICONTROL trait]，而是使用演算法[!UICONTROL Trait]建立新的[!UICONTROL trait] [!UICONTROL segment]，以便啟動（使用）[!UICONTROL segment]。

一旦您建立了[!UICONTROL first party]的[!UICONTROL segment]訪客，在類似[!UICONTROL model]中獲得高分（亦即，看起來像是轉換器但尚未碰巧轉換），即使您已售出網站上所有實際轉換器的庫存，您仍可將此[!UICONTROL segment]提供給網站上的廣告商。 這是擴充此受眾並透過在Audience Manager中使用Look-Lake [!UICONTROL Models]繼續觀看額外收入的絕佳方式。
