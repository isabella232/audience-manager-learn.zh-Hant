---
title: 使用類似模型擴充第一方資料的已售庫存
description: 在本教學課程中，我們將逐步介紹您設定和使用相似模型時應採取的步驟，以便您能夠建立新的相似觀眾，並將其作為轉換區段的延伸銷售。
feature: algorithmic models
topics: null
audience: all
activity: use
doc-type: feature video
team: Technical Marketing
kt: 1688
translation-type: tm+mt
source-git-commit: dfd549508cc223714bdb07ac6fd2aa31e6ca5586
workflow-type: tm+mt
source-wordcount: '825'
ht-degree: 0%

---


# 使用類似功能來 [!UICONTROL Models] 擴充您資料的已售庫 [!UICONTROL First Party] 存 {#using-look-alike-models-to-extend-sold-out-inventory-from-your-st-party-data}

在本教學課程中，我們將逐步介紹您設定和使用Look-Like時應採取的步驟 [!UICONTROL Models]，以便您能夠建立新的Look-Like觀眾，並將其作為轉化的延伸銷售 [!UICONTROL segment]。

## 使用案例詳細資訊 {#use-case-details}

您是內容發佈者。 如果您已經為網站上的轉換器售出了庫存，您可能會認為您的機會到此為止。 進入AAM的相似產品 [!UICONTROL Models]。 透過使用此功能，您可以進一步擴充已售完的庫存，並銷售可能尚未轉換，但外觀／行為與已轉換者相似的受眾。 此受眾 [!UICONTROL segment] 通常的售價低於實際的轉換器，但是，您仍可為想要在您網站上投放廣告的廣告商提供額外的受眾選項，以增加獲利底限。 此使用案例的額外好處是，在您的第一方資料上執行此模型並不需要花費任何成本。

本教學課程的步驟如下：

1. 識別／建立理想的使用者（轉換） [!UICONTROL trait] 或 [!UICONTROL segment]
1. 使用 [!UICONTROL model] 此轉換 [!UICONTROL trait]/[!UICONTROL segment] 作為基本項目
1. 在 [!UICONTROL First party] 中選擇資料來源並 [!UICONTROL model] 執行 [!UICONTROL model]
1. 根據結果 [!UICONTROL Trait] 建立演 [!UICONTROL model] 算法，並將 [!UICONTROL trait] 演算法 [!UICONTROL segment]
1. 提供給感 [!UICONTROL segment] 興趣的廣告商，以延長轉換銷 [!UICONTROL segment] 售

## 識別／建立理想的使用者（轉換） [!UICONTROL trait] 或 [!UICONTROL segment] {#identify-create-an-ideal-user-conversion-trait-or-segment}

您想要在您的網站上吸引訪客做什麼？ 您的轉換事件是什麼？ 當然，這個問題有許多不同的答案，視您的網站類型／垂直和組織目標而定。 無論如何，在AAM中，為符合這些條件的訪 [!UICONTROL trait] 客建立訪客是很常見的。

在此使用案例中，已假設這是因為您已將庫存售罄給轉換器使用者。 不過，在本教學課程中，最好將它作為其餘使用案例的參考。

此外，當使用事件來建 [!UICONTROL traits]立時，您需要記住一個重大問題，以免收集到的使用者數量超過您的要求 [!UICONTROL trait]。 觀看以下影片，瞭解大的顯示效果。:)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注意：** 在上述影片中，我展示的範例假設您有Adobe Analytics。 顯然，情況可能並非如此。 如果您有Google Analytics(GA)，我們有一個模組可讓您用來將資料傳送至AAM(請參閱檔案 [](https://marketing.adobe.com/resources/help/en_US/aam/dil-google-universal-analytics.html))，如果您網站上的轉換活動是透過GA傳送至AAM，則您可以從中建立轉換特徵。 如果您有不同的分析解決方案（或沒有分析解決方案），您仍可透過我們的DIL程式碼和函式，將資料 `submit` 傳送至AAM，等等。 (請參閱 [檔案](https://marketing.adobe.com/resources/help/en_US/aam/c_dil.html))。 然後，再根據在網站上執行轉換活動時所傳送的資料，建立轉換特徵。

## 從資料建立類似 [!UICONTROL Model] 的內 [!UICONTROL First Party] 容 {#creating-a-look-alike-model-from-first-party-data}

在此步驟中，我們將建立一個 [!UICONTROL First Party] 相似的模 [!UICONTROL Model]式 這表示，我們不僅將使用轉換 [!UICONTROL first party] / [!UICONTROL trait][!UICONTROL segment] / [!UICONTROL trait]/[!UICONTROL segment][!UICONTROL models][!UICONTROL first party] （無論如何，這對大多數人來說都是正常的），而且我們也只會為更多看起來像轉換器的人尋找資料池。 我們不會查看或 [!UICONTROL second party] 數 [!UICONTROL third party] 據。

在此使用案例中，這很重要，因為我們嘗試在網站上建立一群看起來像是轉換器但尚未轉換的使用者，所以我們可以將這類似的產品賣給感興趣的 [!UICONTROL segment][!UICONTROL segment] 廣告商。

>[!VIDEO](https://video.tv.adobe.com/v/23504/?quality-12)

## 建立演算法 [!UICONTROL Trait] {#creating-an-algorithmic-trait}

接下來，我們需要建立演算 [!UICONTROL Trait]法，以便使用 [!UICONTROL model] 結果。 如果不創 [!UICONTROL trait]建，就 [!UICONTROL model] 無用。 所以在執 [!UICONTROL model] 行後，請務必進入對話方塊並 [!UICONTROL trait] 建立演算法 [!UICONTROL Trait]。 以下視訊將逐一檢視，並顯示一些提示。

>[!VIDEO](https://video.tv.adobe.com/v/23523/?quality=12)

## 向廣告商提供演 [!UICONTROL Segment] 算法 {#offering-the-algorithmic-segment-to-advertisers}

一旦您建立演算法 [!UICONTROL Trait]，就可以建立新的演算法，以便將其放入，如此您就可以啟動資料(您就無法啟動，而是使用演算法來建立新的單一 [!UICONTROL segment] - [!UICONTROL trait][!UICONTROL trait] ，以便您啟動（使用） [!UICONTROL segment][!UICONTROL Trait][!UICONTROL segment]。

一旦您建立了一些在外觀相似度上得分較高的訪客 [!UICONTROL segment][!UICONTROL first party][!UICONTROL model][!UICONTROL segment] （例如，外觀看起來像是轉換器，但尚未發生轉換），您就可以將它提供給網站上的廣告商，即使您已經將網站上所有實際轉換器的庫存都售出後，您仍然可以提供。 這是擴充此受眾，並透過在Audience Manager中使用Look-Lake繼續觀看額外收益的絕 [!UICONTROL Models] 佳方式。
