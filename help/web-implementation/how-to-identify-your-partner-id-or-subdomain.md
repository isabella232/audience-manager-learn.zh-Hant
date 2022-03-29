---
title: 如何識別您的合作夥伴ID或子域
description: 瞭解如何在實施某些Experience Cloud功能時識別合作夥伴ID或子域，以及在Audience ManagerUI中可獲取此ID的兩個位置。
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

# 如何識別Audience Manager子域 {#how-to-identify-your-audience-manager-partner-id-or-subdomain}

實施某些Experience Cloud功能時，您需要瞭解您的Audience Manager `Subdomain` 是(有時也稱為 `client ID` 或 `Partner ID`)。 在此視頻中，我們將向您顯示兩個可以在Audience ManagerUI中獲取此資訊的位置。

## 放棄結局…… {#giving-away-the-ending}

如果你寧願跳進來，在不看這段短視的情況下找到它，你可以找到 `Partner Subdomain` 在UI中的兩個位置：

1. 如果已建立 [!UICONTROL rule-based] 特性，按一下 **[!UICONTROL Get Trait URL]**
   [!UICONTROL Get Trait URL] 位於該資料夾中特徵清單中的特徵旁邊，URL將在URL中包含您的子域。
1. 如果你進入 **[!UICONTROL Tools]** > **[!UICONTROL Tags]** 介面，按一下 **[!UICONTROL Get code]** 對於您的容器，子域位於Akamai線的末尾

如果您無法快速找到這些快速參考，則視頻將是一個短暫的承諾。 :)

>[!VIDEO](https://video.tv.adobe.com/v/25922/?quality=12)

>[!IMPORTANT]
>
>為Adobe Experience Cloud的每個客戶分配了數字ID，這通常稱為「PID」，即合作夥伴ID。 這不是本文和視頻中談論的ID。 相反，「夥伴子域」（有時稱為夥伴ID）通常是客戶端名稱的版本，並且是資料發送到的伺服器的子域。 例如，如果你的公司是「鮑勃旋鈕」（當然，哈哈），那麼你的合作夥伴子域很可能是「鮑勃旋鈕」，而「PID」則更像「12345」。 您通常不需要知道您的PID，但是您的子域很重要，因此您可以配置Audience Manager實施。
