---
title: 使用算法（相似）模型提高ROAS
description: Audience Manager外觀建模的真正威力在於您尋求將基準受眾擴展到來自第二和第三方資料源的全新高質量用戶群。 在本教程中，瞭解從此資料建立模型的步驟。
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

# 在Audience Manager中使用算法（相似）模型提高ROAS {#increase-roas-by-using-algorithmic-look-alike-models-in-audience-manager}

Audience Manager長相相似的真正力量 [!UICONTROL Modeling] 當您尋求將基準受眾擴展到來自第二方和第三方資料源的全新高質量用戶集時， 在本教程中，瞭解從此資料建立模型所需的步驟。

## 從Audience Marketplace啟用第二方或第三方資料流 {#enable-2nd-or-3rd-party-data-streams-from-the-audience-marketplace}

為了在外觀相似的模型中使用第二方和第三方資料，我們首先必須將這些資料啟用到您的Audience Manager介面中。 Adobe擁有大量可供您選擇的第二方和第三方資料提供商。 這些可通過Audience Marketplace在中的自助式介面AAM中獲得。 導航到Audience Marketplace並瀏覽各種可能性。 以下視頻將向您介紹如何執行此操作，包括如何啟用免費「購買前嘗試」流，以便您可以在提交資料提供商的定價之前鎖定對組織最有用的資料。

此外，為了幫助您研究和確定要使用哪個資料提供商，一個很好的資源是 [[!DNL Adobe Audience Finder]](https://www.adobe-audience-finder.com/)。

>[!VIDEO](https://video.tv.adobe.com/v/25188/?quality=12)

## 識別或建立理想的用戶（轉換）特性或段 {#identify-create-an-ideal-user-conversion-trait-or-segment}

您試圖在您的站點上讓人員執行什麼操作？ 您的轉換事件是什麼？ 當然，根據您的站點類型/垂直和組織目標，有許多不同的答案。 無論如何，為符合這些AAM標準的訪客創造一種特質是很普遍的。

在下面的視頻中，我將向您演示如何建立轉換特性，在您繼續完成本教程並建立外觀相似的模型時，您將希望該特性保持不變。

此外，當使用Adobe Analytics事件來創造特徵時，你需要記住一個重要的收穫，這樣你就不會收集到更多的用戶，而不是你應該收集更多的用戶。 請觀看以下視頻，瞭解大的顯示。 :)

>[!VIDEO](https://video.tv.adobe.com/v/23431/?quality=12)

**注：** 在上面的視頻中，我展示的示例假定您有Adobe Analytics。 顯然，情況可能並非如此。 如果您有Google Analytics(GA)，則我們有一個模組，您可以使用它將資料發送AAM到(請參閱 [文檔](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-modules.html))，並且如果您的站點上的轉換活動是由AAMGA發送到的，則可以從中建立轉換特性。 如果您有不同的分析解決方案（或沒有分析解決方案），您仍然可以通過我們的代AAM碼和DIL將資料發送到 `submit` 函式等。 (請參閱 [文檔](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html))。 然後，根據在站點上執行轉換活動時發送的資料建立轉換特性。

## 從第二方或第三方資料建立外觀相似模型 {#create-a-look-alike-model-from-2nd-or-3rd-party-data}

完成上述步驟後，我們現在準備建立一個算法（相似）模型。 在建立模型時，我們將使用轉換特性作為我們的基本特性（我們希望複製的關鍵訪問者），並將啟用的第三方資料流用作我們要從中抽取的人員池。

>[!VIDEO](https://video.tv.adobe.com/v/25190/?quality-12)

## 重要的最佳做法 {#an-important-best-practice}

當在Audience Manager中建立算法模型時，顯然我們希望模型盡可能有效。 由於模型考慮的是基本特徵/片段的成員所參與的所有特徵，所以如果所有人都屬於特徵/片段，則對模型沒有幫助。 因此，如果您具有超級通用性特徵（如訪問您的站點的所有人，或從您處收到任何廣告的所有人等），請確保他們所屬的資料源未包含在模型中的資料源中。 在本文的使用案例中，你不太可能這麼做，因為我們專注於為我們的新外觀設計尋找第三方資料，但無論如何，它都值得一提，並且適用於你所有的算法模型。

## 建立 [!UICONTROL Algorithmic Trait] {#creating-an-algorithmic-trait}

接下來，我們需要建立  [!UICONTROL Algorithmic Trait]，以便模型的結果可用。 沒有創造特質，模型是無用的。 所以，在模型運行後，一定要進入特質對話並建立 [!UICONTROL Algorithmic Trait]。 下面的視頻瀏覽了一下，顯示了幾個提示。

>[!VIDEO](https://video.tv.adobe.com/v/25191/?quality=12)

## 從模型資料建立段並將其發送DSP到 {#creating-a-segment-from-the-model-data-and-sending-it-to-dsps}

建立 [!UICONTROL Algorithmic Trait]，可以建立新段以將其放入，以便激活資料(不能激活一個特徵，而是使用 [!UICONTROL Algorithmic Trait] 中，以便激活（使用）段)。

一旦你根據這種「算法」特徵建立了一個片段，你就會有一群潛在客戶，他們看起來就像你網站上已經轉換過的人。 現在，您可以將此段映射到您的任DSP何目標Audience Manager。 您將能夠將您的營銷目標定位給那些外觀上的人，他們更可能在您的網站上轉換，而不僅僅是普通公眾，從而提高您的廣告支出回報。
