<properties
   pageTitle="使用入口網站為應用程式閘道建立路徑型規則 | Microsoft Azure"
   description="了解如何使用入口網站為應用程式閘道建立路徑型規則"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/18/2016"
   ms.author="gwallace" />

# 使用入口網站為應用程式閘道建立路徑型規則

> [AZURE.SELECTOR]
- [Azure 入口網站](application-gateway-create-url-route-portal.md)
- [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)

URL 路徑型路由可讓您根據 Http 要求的 URL 路徑來關聯路由。它會檢查是否有路由連至針對應用程式閘道中的 URL 清單設定的後端集區，並將網路流量傳送至定義的後端集區。URL 型路由的常見用法是將不同內容類型的要求負載平衡至不同的後端伺服器集區。

URL 型路由會將新的規則類型引進應用程式閘道。應用程式閘道具有 2 種規則類型：基本和路徑型規則。基本規則類型會針對後端集區提供循環配置資源服務，而路徑型規則除了循環配置資源發佈之外，也會在選擇後端集區時將要求 URL 的路徑模式納入考慮。



## 案例

下列案例會完成在現有應用程式閘道中建立路徑型規則的程序。此案例假設您已經依照步驟[建立應用程式閘道](application-gateway-create-gateway-portal.md)。

![URL 路由][scenario]

## <a name="createrule"></a>建立路徑型規則

路徑型規則需要自己的接聽程式，在建立此規則之前，請務必確認您有接聽程式可供使用。

### 步驟 1

瀏覽至 http://portal.azure.com，然後選取現有的應用程式閘道。按一下 [規則]

![應用程式閘道概觀][1]

### 步驟 2

按一下 [路徑型] 按鈕來新增路徑型規則。

### 步驟 3

[新增路徑型規則] 刀鋒視窗有兩個區段。您會在第一個區段中，定義接聽程式、規則的名稱和預設路徑設定。預設路徑設定適用於未落在自訂路徑型路由之下的路由。您會在 [新增路徑型規則] 刀鋒視窗的第二個區段中，定義路徑型規則本身。

**基本設定**

- **名稱** - 這是可在入口網站中存取的易記規則名稱。
- **接聽程式** - 這是用於規則的接聽程式。
- **預設後端集區** - 此設定可定義要用於預設規則的後端
- **預設 HTTP 設定** - 此設定可定義要用於預設規則的 HTTP 設定。

**路徑型規則**

- **名稱** - 這是路徑型規則的易記名稱。
- **路徑** -此設定可定義規則在轉送流量時將要尋找的路徑
- **後端集區** - 此設定可定義要用於規則的後端
- **HTTP 設定** - 此設定可定義要用於規則的 HTTP 設定。

>[AZURE.IMPORTANT] 路徑︰要比對的路徑模式清單。每個路徑都必須以 / 開頭，而且只有結尾允許使用 "*"。有效範例包括 /xyz、/xyz* 或 /xyz/*。

![已填入資訊的新增路徑型規則刀鋒視窗][2]

將路徑型規則新增至現有應用程式閘道是在入口網站中進行的簡單程序。建立路徑型規則後，即可加以編輯以便輕鬆地新增其他規則。

![新增其他路徑型規則][3]

## 後續步驟

若要了解如何設定與「Azure 應用程式閘道」搭配運作的「SSL 卸載」，請參閱[設定 SSL 卸載](application-gateway-ssl-portal.md)。

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png

<!---HONumber=AcomDC_0824_2016-->