---
title: 如何識別您的合作夥伴ID或子網域
description: 瞭解如何在實作某些Experience Cloud功能時識別您的合作夥伴ID或子網域，以及可在Audience ManagerUI中取得此ID的約兩個位置。
feature: Implementation Basics
topics: null
activity: implement
doc-type: technical video
team: Technical Marketing
kt: 2359
role: Developer, Data Engineer
level: Intermediate
exl-id: d3f4a12d-acc5-47b7-a38a-a6a14152bf3a
source-git-commit: 62b43b5627dabf754cf821f974a56c60989ef7ef
workflow-type: tm+mt
source-wordcount: '315'
ht-degree: 0%

---

# 如何識別您的Audience Manager子網域 {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

實作某些Experience Cloud功能時，您需要知道您的Audience Manager `Subdomain` 是(有時也稱為 `client ID` 或 `Partner ID`)。 在本影片中，我們將說明您可以在Audience ManagerUI中取得此資訊的兩個位置。

## 正在放棄結局…… {#giving-away-the-ending}

如果您只想在不觀看這段短片的情況下直接跳進去尋找它，您可以找到 `Partner Subdomain` 在UI中的兩個位置：

1. 如果您已建立 [!UICONTROL rule-based] 特徵，按一下 **[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] 位於該資料夾中特徵清單的特徵旁，且URL會將您的子網域包含在URL中。
1. 如果您前往 **[!UICONTROL Tools]** > **[!UICONTROL Tags]** 介面並按一下 **[!UICONTROL Get code]** 對於您的容器，子網域會靠近Akamai行的結尾

如果您無法透過這些快速參考快速找到它，則影片為簡短承諾。 ：)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>系統會為Adobe Experience Cloud的每個客戶指派一個數值ID，這通常稱為「PID」或合作夥伴ID。 這不是我們在這篇文章和影片中談論的ID。 「合作夥伴子網域」（有時稱為「合作夥伴ID」）通常是使用者端名稱的版本，且是資料傳送至的伺服器子網域。 例如，如果您的公司是「Bob&#39;s Knobs」（所有東西都是門把手，當然，哈哈），那麼您的合作夥伴子網域很可能是「bobsknobs」，而「PID」可能更像「12345」。 您通常不需要知道您的PID，但您的子網域非常重要，因此您可以設定Audience Manager實施。
