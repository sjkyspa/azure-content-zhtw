<properties
	pageTitle="要求增加 DocumentDB 帳戶配額 | Microsoft Azure"
	description="了解如何要求調整 DocumentDB 資料庫配額，例如文件儲存和每個集合的輸送量。"
	services="documentdb"
	authors="AndrewHoh"
	manager="jhubbard"
	editor="monicar"
	documentationCenter=""/>

<tags
	ms.service="documentdb"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/25/2016"
	ms.author="anhoh"/>

# 要求增加 DocumentDB 帳戶限制

[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) 有一組預設的配額，可以透過聯絡 Azure 支援人員來調整。本文說明如何要求增加配額。

閱讀本文後，您將能夠回答下列問題：

-	哪種 DocumentDB 資料庫配額可以透過聯絡 Azure 支援人員來調整？
-	如何要求 DocumentDB 帳戶配額調整？

##<a id="Quotas"></a> DocumentDB 帳戶配額

下表描述 DocumentDB 配額。具有星號 (*) 的配額可以透過聯絡 Azure 支援人員來進行調整：

[AZURE.INCLUDE [azure-documentdb-limits](../../includes/azure-documentdb-limits.md)]


##<a id="RequestQuotaIncrease"></a> 要求配額調整
下列步驟說明如何要求配額調整。

1. 在 [Azure 入口網站](https://portal.azure.com)中，按一下 [更多服務]，然後按一下 [說明 + 支援]。

	![啟動說明和支援的螢幕擷取畫面](media/documentdb-increase-limits/helpsupport.png)

2. 在 [說明 + 支援] 刀鋒視窗中，按一下 [新增支援要求]。

	![建立支援票證的螢幕擷取畫面](media/documentdb-increase-limits/getsupport.png)

3. 在 [新增支援要求] 刀鋒視窗中，按一下 [基本]。接下來，分別將 [問題類型] 設定為 [配額]、將 [訂用帳戶] 設定為託管 DocumentDB 帳戶的訂用帳戶、將 [配額類型] 設定為 [DocumentDB]，並將 [支援方案] 設定為 [配額支援 - 包含]。然後按 [下一步]。

	![支援票證要求類型的螢幕擷取畫面](media/documentdb-increase-limits/supportrequest1.png)

4. 在 [問題] 刀鋒視窗中選擇嚴重性，並在 [詳細資料] 中包含有關配額增加的資訊。按 [下一步]。

	![支援票證訂用帳戶選擇器的螢幕擷取畫面](media/documentdb-increase-limits/supportrequest2.png)

5. 最後，在 [連絡資訊] 刀鋒視窗中填入您的連絡資訊，然後按一下 [建立]。

建立支援票證之後，您應該會透過電子郵件收到支援要求編號。您也可以按一下 [說明 + 支援] 刀鋒視窗中的 [管理支援要求] 來檢視支援要求。

![支援要求刀鋒視窗的螢幕擷取畫面](media/documentdb-increase-limits/supportrequest4.png)


##<a name="NextSteps"></a> 後續步驟
- 若要深入了解 DocumentDB，請按一下[這裡](http://azure.com/docdb)。

<!---HONumber=AcomDC_0831_2016-->