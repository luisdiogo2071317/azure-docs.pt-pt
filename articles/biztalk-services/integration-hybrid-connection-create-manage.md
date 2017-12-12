---
title: "Criar e gerir ligações híbridas | Microsoft Docs"
description: "Saiba como criar uma ligação híbrida, a ligação de gerir e instalar o Gestor de ligações híbridas. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: erikre
editor: 
ms.assetid: aac0546b-3bae-41e0-b874-583491a395ea
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: ccompy
ms.openlocfilehash: 1751d33b5f6f6a506654daedd15bbd75ae271483
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="create-and-manage-hybrid-connections"></a>Criar e Gerir Ligações Híbridas

> [!IMPORTANT]
> As Ligações Híbridas do BizTalk são desativadas e substituídas por Ligações Híbridas do Serviço de Aplicações. Para obter mais informações, incluindo como gerir as Ligações Híbridas do BizTalk, veja [Azure App Service Hybrid Connections](../app-service/app-service-hybrid-connections.md) (Ligações Híbridas do Serviço de Aplicações do Azure).

>[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

## <a name="overview-of-the-steps"></a>Descrição geral dos passos
1. Criar uma ligação híbrida introduzindo o **nome de anfitrião** ou **FQDN** do recurso no local na sua rede privada.
2. Ligar a aplicações web do Azure ou mobile apps do Azure para a ligação híbrida.
3. Instalar o Gestor de ligações híbridas no seu recurso no local e ligue-se para a ligação híbrida específico. O portal do Azure fornece uma experiência de clique único para instalar e ligar.
4. Gerir ligações híbridas e as respetivas chaves de ligação.

Este tópico lista estes passos. 

> [!IMPORTANT]
> É possível definir um ponto final da ligação híbrida para um endereço IP. Se utilizar um endereço IP, poderá ou poderá não atingir o recurso no local, dependendo do seu cliente. A ligação híbrida depende do cliente efetuar uma pesquisa de DNS. Na maioria dos casos, o **cliente** é o código da aplicação. Se o cliente não efetua uma pesquisa de DNS (está a tentar resolver o endereço IP, como se fosse um nome de domínio (x.x.x. x)), em seguida, o tráfego não é enviado através da ligação híbrida.
> 
> Por exemplo (pseudocode), é possível definir **10.4.5.6** como o anfitrião no local:
> 
> **Funciona da seguinte cenário:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **Não utilizar o seguinte cenário:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`
> 
> 

## <a name="CreateHybridConnection"></a>Criar uma ligação híbrida
É possível criar uma ligação híbrida no [as ligações híbridas do serviço de aplicação do Azure](../app-service/app-service-hybrid-connections.md) **ou** utilizando [APIs REST do BizTalk Services](https://msdn.microsoft.com/library/azure/dn232347.aspx). 

<!-- **To create Hybrid Connections using Web Apps**, see [Connect Azure Web Apps to an On-Premises Resource](../app-service-web/web-sites-hybrid-connection-get-started.md). You can also install the Hybrid Connection Manager (HCM) from your web app, which is the preferred method.  -->

#### <a name="additional"></a>Adicionais
* Podem ser criadas várias ligações híbridas. Consulte o [BizTalk Services: gráfico de edições](biztalk-editions-feature-chart.md) para o número de ligações permitido. 
* Cada ligação híbrida é criada com um par de cadeias de ligação: esse chaves de envio e no local que ESCUTAM as chaves de aplicação. Cada par tem um site primário e uma chave secundária. 

## <a name="LinkWebSite"></a>Ligar a aplicação móvel ou aplicação de Web do App Service do Azure
Para ligar uma aplicação Web ou aplicação móvel no App Service do Azure para uma ligação híbrida existente, selecione **utilizar uma ligação híbrida existente** no painel ligações híbridas. 
<!-- See [Access on-premises resources using hybrid connections in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md). -->

## <a name="InstallHCM"></a>Instalar o Gestor de ligações híbridas no local
Depois de criar uma ligação híbrida, instale o Gestor de ligações híbridas no recurso no local. Pode ser transferido das suas aplicações web do Azure ou a partir do seu BizTalk Service. 

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]
 
[Ligações híbridas do serviço de aplicação do Azure](../app-service/app-service-hybrid-connections.md) também é um boa recursos.

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Adicionais
* Gestor de ligações híbridas pode ser instalado nos seguintes sistemas operativos:
  
  * Windows Server 2008 R2 (.NET Framework 4.5 + e Windows Management Framework 4.0 + necessário)
  * Windows Server 2012 (Windows Management Framework 4.0 + necessário)
  * Windows Server 2012 R2
* Depois de instalar o Gestor de ligações híbridas, ocorre o seguinte: 
  
  * A ligação híbrida alojada no Azure é automaticamente configurada para utilizar a cadeia de ligação de aplicação principal. 
  * O recurso no local é automaticamente configurado para utilizar a cadeia de ligação no local primário.
* O Gestor de ligações híbridas tem de utilizar uma cadeia de ligação válido no local para autorização. As Web Apps do Azure ou Mobile Apps, tem de utilizar uma cadeia de ligação de aplicação válida para autorização.
* Pode dimensionar as ligações híbridas instalando outra instância do Gestor de ligações híbridas noutro servidor. Configure o serviço de escuta no local para utilizar o mesmo endereço como o primeiro serviço de escuta no local. Nesta situação, o tráfego é distribuído aleatoriamente (o round robin) entre os serviços de escuta do Active Directory no local. 

## <a name="ManageHybridConnection"></a>Gerir ligações híbridas

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)] 

[Ligações híbridas do serviço de aplicação do Azure](../app-service/app-service-hybrid-connections.md) também é um boa recursos.

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Cópia/voltar a gerar as cadeias de ligação híbrida

[!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)] 

[Ligações híbridas do serviço de aplicação do Azure](../app-service/app-service-hybrid-connections.md) também é um boa recursos.

#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Política de grupo de utilização para controlar os recursos no local utilizados por uma ligação híbrida
1. Transferir o [modelos administrativos do Gestor de ligações híbridas](http://www.microsoft.com/download/details.aspx?id=42963).
2. Extraia os ficheiros.
3. No computador que modifica a política de grupo, faça o seguinte:  
   
   * Copiar o. Ficheiros ADMX para o *%WINROOT%\PolicyDefinitions* pasta.
   * Copiar o. ADML ficheiros para o *%WINROOT%\PolicyDefinitions\en-us* pasta.

Depois de copiar, pode utilizar o Editor de políticas de grupo para alterar a política.

## <a name="next"></a>Seguinte
[Descrição geral de ligações híbridas](integration-hybrid-connection-overview.md)

## <a name="see-also"></a>Veja Também
[API REST para gerir os BizTalk Services no Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[Serviços BizTalk: Gráfico de Edições](biztalk-editions-feature-chart.md)  
[Criar um BizTalk Service](biztalk-provision-services.md)  
[Serviços BizTalk: Separadores Dashboard, Monitorizar e Dimensionar](biztalk-dashboard-monitor-scale-tabs.md)

[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
