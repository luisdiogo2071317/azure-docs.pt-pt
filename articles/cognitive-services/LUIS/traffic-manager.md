---
title: Utilizar o Gestor de tráfego do Microsoft Azure para aumentar a quota de ponto final no idioma compreender (LUIS) - Azure | Microsoft Docs
description: Utilize o Gestor de tráfego do Microsoft Azure para propagar-se a quota de ponto final entre várias subscrições no idioma compreender (LUIS) para aumentar a quota de ponto final
author: v-geberr
manager: kaiqb
services: cognitive-services
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/07/2018
ms.author: v-geberr
ms.openlocfilehash: b5849e0b4d9b7fb1f681a52aef2c403031ff0144
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35356352"
---
# <a name="use-microsoft-azure-traffic-manager-to-manage-endpoint-quota-across-keys"></a>Utilizar o Gestor de tráfego do Microsoft Azure para gerir a quota de ponto final através de chaves
Compreensão de idiomas (LUIS) oferece a capacidade de aumentar a quota de pedido de ponto final para além da quota de uma chave única. Isto é feito através da criação de chaves mais para LUIS e adicioná-los para a aplicação de LUIS no **publicar** página no **recursos e as chaves** secção. 

A aplicação de cliente tem de gerir o tráfego entre as chaves. LUIS não fazê-lo. 

Este artigo explica como gerir o tráfego através de chaves com o Azure [Gestor de tráfego][traffic-manager-marketing]. Já tem de ter uma aplicação de LUIS preparada e publicada. Se não tiver uma, siga o domínio Prebuilt [início rápido](luis-get-started-create-app.md). 

## <a name="connect-to-powershell-in-the-azure-portal"></a>Ligar para o PowerShell no portal do Azure
No [Azure] [ azure-portal] portal, abra a janela do PowerShell. O ícone para a janela do PowerShell é a **> _** na barra de navegação superior. Através do PowerShell do portal, obter a versão mais recente do PowerShell e são autenticados. Requer o PowerShell no portal de um [Storage do Azure](https://azure.microsoft.com/services/storage/) conta. 

![Abra o portal de captura de ecrã do Azure com a janela do Powershell](./media/traffic-manager/azure-portal-powershell.png)

As secções seguintes utilize [cmdlets do PowerShell do Gestor de tráfego](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/?view=azurermps-6.2.0#traffic_manager).

## <a name="create-azure-resource-group-with-powershell"></a>Criar grupo de recursos do Azure com o PowerShell
Antes de criar os recursos do Azure, crie um grupo de recursos que contém todos os recursos. O grupo de recursos de nome `luis-traffic-manager` e utilize a região é `West US`. A região do grupo de recursos armazena os metadados sobre o grupo. Que não será mais lentos os recursos que estejam noutra região. 

Criar grupo de recursos com **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-6.2.0)** cmdlet:

```PowerShell
New-AzureRmResourceGroup -Name luis-traffic-manager -Location "West US"
```

## <a name="create-luis-keys-to-increase-total-endpoint-quota"></a>Criar LUIS chaves para aumentar a quota de ponto final total
1. No portal do Azure, criar dois **compreensão de idiomas** de chaves, um no `West US` e um no `East US`. Utilize o grupo de recursos existente, criado na secção anterior, com o nome `luis-traffic-manager`. 

    ![Portal de captura de ecrã do Azure com duas chaves LUIS no grupo de recursos do Gestor de tráfego luis](./media/traffic-manager/luis-keys.png)

2. No [LUIS] [ LUIS] Web site, o **publicar** página, adicionar as chaves para a aplicação e voltar a publicar a aplicação. 

    ![Portal de captura de ecrã de LUIS com duas chaves LUIS na página de publicar](./media/traffic-manager/luis-keys-in-luis.png)

    O URL de exemplo no **endpoint** coluna utiliza um pedido GET com a chave de subscrição como um parâmetro de consulta. Copie o URL de ponto final de duas novas chaves. São utilizados como parte da configuração do Gestor de tráfego neste artigo.

## <a name="manage-luis-endpoint-requests-across-keys-with-traffic-manager"></a>Gerir pedidos de ponto final de LUIS através de chaves com o Gestor de tráfego
Gestor de tráfego cria um novo ponto de acesso DNS para os pontos finais. Se não atuar como gateway ou proxy, mas estritamente ao nível do DNS. Este exemplo não irá alterar os registos DNS. Utiliza uma biblioteca DNS para comunicar com o Gestor de tráfego para obter o ponto final correto para esse pedido específico. _Cada_ pedido pretendido para LUIS primeiro requer um pedido de Gestor de tráfego para determinar o ponto final de LUIS a utilizar. 

### <a name="polling-uses-luis-endpoint"></a>Consulta utiliza o ponto final de LUIS
Gestor de tráfego consulta os pontos finais periodicamente para se certificar de que o ponto final ainda está disponível. O URL do Gestor de tráfego consultados tem de estar acessível com um pedido GET e devolver um 200. O URL de ponto final no **publicar** página efetua este procedimento. Uma vez que cada chave de subscrição tem uma rota diferentes e os parâmetros de cadeia de consulta, cada chave de subscrição tem um caminho de consulta diferentes. Sempre que consulta o Gestor de tráfego, custo um pedido de quota. O parâmetro de cadeia de consulta **q** o LUIS ponto final é utterance enviado para LUIS. Este parâmetro, em vez de enviarem uma utterance, é utilizado para adicionar Gestor de tráfego consulta o registo do ponto final de LUIS como técnica de depuração ao obter o Gestor de tráfego configurado.

Porque cada ponto final LUIS tem o suas próprias caminho, tem a suas próprias perfil do Traffic Manager. Para poder gerir através de perfis, criar um [ _aninhada_ Gestor de tráfego](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-nested-profiles) arquitetura. Um perfil de principal aponta para os perfis de elementos subordinados e gerir o tráfego entre eles.

Depois de ter configurado o Gestor de tráfego, não se esqueça de alterar o caminho para o registo de = parâmetro de cadeia de consulta false para que o início de sessão não é preenchida com a consulta.

## <a name="configure-traffic-manager-with-nested-profiles"></a>Configurar o Gestor de tráfego com perfis aninhados
As secções seguintes criar dois perfis de subordinados, um para a chave do Leste LUIS e outro para a chave de LUIS oeste. Em seguida, é criado um perfil de principal e os perfis de dois subordinados são adicionados ao perfil de principal. 

### <a name="create-the-east-us-traffic-manager-profile-with-powershell"></a>Criar o perfil do Gestor de tráfego de EUA Leste com o PowerShell
Para criar o perfil do Gestor de tráfego de EUA leste, existem vários passos: criar o perfil, adicionar o ponto final e definir o ponto final. Um perfil de Gestor de tráfego pode ter vários pontos finais, mas cada ponto final não tem o mesmo caminho de validação. Porque os URL de ponto final de LUIS para as subscrições orientais e ocidentais estão diferente devido a chave de subscrição e região, cada ponto final LUIS tem de ser um único ponto final do perfil. 

1. Criar perfil com **[New-AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile?view=azurermps-6.2.0)** cmdlet

    Utilize o cmdlet seguinte para criar o perfil. Certifique-se de que altera o `appIdLuis` e `subscriptionKeyLuis`. O subscriptionKey destina-se a chave de LUIS do Leste dos EUA. Se o caminho é não está correto, incluindo o LUIS ID e a subscrição chave da aplicação, a consulta do Gestor de tráfego é um Estado de `degraded` porque não é possível gerir o tráfego de pedidos com êxito o ponto final LUIS. Certifique-se de que o valor de `q` é `traffic-manager-east` para que possa ver este valor nos registos de ponto final LUIS.

    ```PowerShell
    $eastprofile = New-AzureRmTrafficManagerProfile -Name luis-profile-eastus -ResourceGroupName luis-traffic-manager -TrafficRoutingMethod Performance -RelativeDnsName luis-dns-eastus -Ttl 30 -MonitorProtocol HTTPS -MonitorPort 443 -MonitorPath "/luis/v2.0/apps/<appID>?subscription-key=<subscriptionKey>&q=traffic-manager-east"
    ```
    
    Esta tabela explica cada variável no cmdlet:
    
    |Parâmetro de configuração|Nome da variável ou valor|Objetivo|
    |--|--|--|
    |-Name|Luis-perfil-eastus|Nome do Traffic Manager no portal do Azure|
    |-ResourceGroupName|Gestor de tráfego Luis|Criado na secção anterior|
    |-TrafficRoutingMethod|Desempenho|Para obter mais informações, consulte [métodos de encaminhamento do Traffic Manager][routing-methods]. Se utilizar o desempenho, o pedido de URL para o Gestor de tráfego tem de ser provenientes a região do utilizador. Se passar uma chatbot ou outra aplicação, é responsabilidade a chatbot para imitar a região na chamada para o Gestor de tráfego. |
    |-RelativeDnsName|Luis-dns-eastus|Este é o subdomínio para o serviço: luis-dns-eastus.trafficmanager.net|
    |-O valor de Ttl|30|Intervalo de consulta, 30 segundos|
    |-MonitorProtocol<BR>-MonitorPort|HTTPS<br>443|Protocolo para LUIS e a porta é HTTPS/443|
    |-MonitorPath|`/luis/v2.0/apps/<appIdLuis>?subscription-key=<subscriptionKeyLuis>&q=traffic-manager-east`|Substitua <appIdLuis> e <subscriptionKeyLuis> com os seus próprios valores.|
    
    Um pedido com êxito tem sem resposta.

2. Adicionar o ponto final de EUA Leste com **[adicionar AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/add-azurermtrafficmanagerendpointconfig?view=azurermps-6.2.0)** cmdlet

    ```PowerShell
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName luis-east-endpoint -TrafficManagerProfile $eastprofile -Type ExternalEndpoints -Target eastus.api.cognitive.microsoft.com -EndpointLocation "eastus" -EndpointStatus Enabled
    ```
    Esta tabela explica cada variável no cmdlet:

    |Parâmetro de configuração|Nome da variável ou valor|Objetivo|
    |--|--|--|
    |-EndpointName|Luis-Leste-ponto final|Nome do ponto final apresentado sob o perfil|
    |-TrafficManagerProfile|$eastprofile|Utilizar o objeto de perfil que criou no passo 1|
    |-Type|ExternalEndpoints|Para obter mais informações, consulte [ponto final do Gestor de tráfego][traffic-manager-endpoints] |
    |-Destino|eastus.API.cognitive.microsoft.com|Este é o domínio para o ponto final LUIS.|
    |-EndpointLocation|"eastus"|Região do ponto final|
    |-EndpointStatus|Ativado|Ativar o ponto final quando é criado|

    A resposta com êxito se pareça com:

    ```cmd
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-eastus
    Name                             : luis-profile-eastus
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-eastus
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /luis/v2.0/apps/<luis-app-id>?subscription-key=f0517d185bcf467cba5147d6260bb868&q=traffic-manager-east
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {luis-east-endpoint}
    ```

3. Definir o ponto final de EUA Leste com **[conjunto AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/set-azurermtrafficmanagerprofile?view=azurermps-6.2.0)** cmdlet

    ```PowerShell
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $eastprofile
    ```

    Uma resposta com êxito será a mesma resposta como passo 2.

### <a name="create-the-west-us-traffic-manager-profile-with-powershell"></a>Criar o perfil do Gestor de tráfego de EUA oeste com o PowerShell
Para criar o perfil do Gestor de tráfego de EUA oeste, siga os mesmos passos: criar o perfil, adicionar o ponto final e definir o ponto final.

1. Criar perfil com **[New-AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/New-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)** cmdlet

    Utilize o cmdlet seguinte para criar o perfil. Certifique-se de que altera o `appIdLuis` e `subscriptionKeyLuis`. O subscriptionKey destina-se a chave de LUIS do Leste dos EUA. Se o caminho não é correto, incluindo a chave de ID e a subscrição de aplicação LUIS, a consulta do Gestor de tráfego é um Estado de `degraded` porque não é possível gerir o tráfego de pedidos com êxito o ponto final LUIS. Certifique-se de que o valor de `q` é `traffic-manager-west` para que possa ver este valor nos registos de ponto final LUIS.

    ```PowerShell
    $westprofile = New-AzureRmTrafficManagerProfile -Name luis-profile-westus -ResourceGroupName luis-traffic-manager -TrafficRoutingMethod Performance -RelativeDnsName luis-dns-westus -Ttl 30 -MonitorProtocol HTTPS -MonitorPort 443 -MonitorPath "/luis/v2.0/apps/<appIdLuis>?subscription-key=<subscriptionKeyLuis>&q=traffic-manager-west"
    ```
    
    Esta tabela explica cada variável no cmdlet:
    
    |Parâmetro de configuração|Nome da variável ou valor|Objetivo|
    |--|--|--|
    |-Name|Luis-perfil-westus|Nome do Traffic Manager no portal do Azure|
    |-ResourceGroupName|Gestor de tráfego Luis|Criado na secção anterior|
    |-TrafficRoutingMethod|Desempenho|Para obter mais informações, consulte [métodos de encaminhamento do Traffic Manager][routing-methods]. Se utilizar o desempenho, o pedido de URL para o Gestor de tráfego tem de ser provenientes a região do utilizador. Se passar uma chatbot ou outra aplicação, é responsabilidade a chatbot para imitar a região na chamada para o Gestor de tráfego. |
    |-RelativeDnsName|Luis-dns-westus|Este é o subdomínio para o serviço: luis-dns-westus.trafficmanager.net|
    |-O valor de Ttl|30|Intervalo de consulta, 30 segundos|
    |-MonitorProtocol<BR>-MonitorPort|HTTPS<br>443|Protocolo para LUIS e a porta é HTTPS/443|
    |-MonitorPath|`/luis/v2.0/apps/<appIdLuis>?subscription-key=<subscriptionKeyLuis>&q=traffic-manager-west`|Substitua <appId> e <subscriptionKey> com os seus próprios valores. Lembre-se de que esta chave de subscrição é diferente da chave de subscrição do leste|
    
    Um pedido com êxito tem sem resposta.

2. Adicionar o ponto final de EUA oeste com **[adicionar AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0)** cmdlet

    ```PowerShell
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName luis-west-endpoint -TrafficManagerProfile $westprofile -Type ExternalEndpoints -Target westus.api.cognitive.microsoft.com -EndpointLocation "westus" -EndpointStatus Enabled
    ```

    Esta tabela explica cada variável no cmdlet:

    |Parâmetro de configuração|Nome da variável ou valor|Objetivo|
    |--|--|--|
    |-EndpointName|Luis-oeste-ponto final|Nome do ponto final apresentado sob o perfil|
    |-TrafficManagerProfile|$westprofile|Utilizar o objeto de perfil que criou no passo 1|
    |-Type|ExternalEndpoints|Para obter mais informações, consulte [ponto final do Gestor de tráfego][traffic-manager-endpoints] |
    |-Destino|westus.API.cognitive.microsoft.com|Este é o domínio para o ponto final LUIS.|
    |-EndpointLocation|"westus"|Região do ponto final|
    |-EndpointStatus|Ativado|Ativar o ponto final quando é criado|

    A resposta com êxito se pareça com:

    ```cmd
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-westus
    Name                             : luis-profile-westus
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-westus
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /luis/v2.0/apps/c3fc5d1e-5187-40cc-af0f-fbde328aa16b?subscription-key=e3605f07e3cc4bedb7e02698a54c19cc&q=traffic-manager-west
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {luis-west-endpoint}
    ```

3. Definir o ponto final de EUA oeste com **[conjunto AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Set-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)** cmdlet

    ```PowerShell
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $westprofile
    ```

    Uma resposta com êxito é a mesma resposta como passo 2.

### <a name="create-parent-traffic-manager-profile"></a>Criar perfil do Gestor de tráfego de principal
Criar o perfil do Traffic Manager do elemento principal e ligá dois perfis de Gestor de tráfego subordinado para o elemento principal.

1. Criar perfil de principal com **[New-AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/New-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)** cmdlet

    ```PowerShell
    $parentprofile = New-AzureRmTrafficManagerProfile -Name luis-profile-parent -ResourceGroupName luis-traffic-manager -TrafficRoutingMethod Performance -RelativeDnsName luis-dns-parent -Ttl 30 -MonitorProtocol HTTPS -MonitorPort 443 -MonitorPath "/"
    ```

    Esta tabela explica cada variável no cmdlet:

    |Parâmetro de configuração|Nome da variável ou valor|Objetivo|
    |--|--|--|
    |-Name|Luis-perfil-principal|Nome do Traffic Manager no portal do Azure|
    |-ResourceGroupName|Gestor de tráfego Luis|Criado na secção anterior|
    |-TrafficRoutingMethod|Desempenho|Para obter mais informações, consulte [métodos de encaminhamento do Traffic Manager][routing-methods]. Se utilizar o desempenho, o pedido de URL para o Gestor de tráfego tem de ser provenientes a região do utilizador. Se passar uma chatbot ou outra aplicação, é responsabilidade a chatbot para imitar a região na chamada para o Gestor de tráfego. |
    |-RelativeDnsName|Luis-dns-principal|Este é o subdomínio para o serviço: luis-dns-parent.trafficmanager.net|
    |-O valor de Ttl|30|Intervalo de consulta, 30 segundos|
    |-MonitorProtocol<BR>-MonitorPort|HTTPS<br>443|Protocolo para LUIS e a porta é HTTPS/443|
    |-MonitorPath|`/`|Este caminho irrelevante porque os caminhos de ponto final de subordinados são utilizados em vez disso.|

    Um pedido com êxito tem sem resposta.

2. Adicionar o perfil de subordinados de EUA Leste principal com **[adicionar AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0)** e **NestedEndpoints** tipo

    ```PowerShell
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint-useast -TrafficManagerProfile $parentprofile -Type NestedEndpoints -TargetResourceId $eastprofile.Id -EndpointStatus Enabled -EndpointLocation "eastus" -MinChildEndpoints 1
    ```

    Esta tabela explica cada variável no cmdlet:

    |Parâmetro de configuração|Nome da variável ou valor|Objetivo|
    |--|--|--|
    |-EndpointName|useast de ponto final de subordinados|Perfil do leste|
    |-TrafficManagerProfile|$parentprofile|Perfil para atribuir este ponto final|
    |-Type|NestedEndpoints|Para obter mais informações, consulte [adicionar AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0). |
    |-TargetResourceId|$eastprofile. ID|ID do perfil subordinado|
    |-EndpointStatus|Ativado|Estado de ponto final após a adição ao principal|
    |-EndpointLocation|"eastus"|[Nome da região do Azure](https://azure.microsoft.com/global-infrastructure/regions/) do recurso|
    |-MinChildEndpoints|1|Número mínimo de pontos finais subordinado|

    O aspeto de resposta com êxito com o seguinte e inclui a nova `child-endpoint-useast` ponto final:    

    ```cmd
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-parent
    Name                             : luis-profile-parent
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-parent
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {child-endpoint-useast}
    ```

3. Adicionar o perfil de subordinados de EUA oeste principal com **[adicionar AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0)** cmdlet e **NestedEndpoints** tipo

    ```PowerShell
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint-uswest -TrafficManagerProfile $parentprofile -Type NestedEndpoints -TargetResourceId $westprofile.Id -EndpointStatus Enabled -EndpointLocation "westus" -MinChildEndpoints 1
    ```

    Esta tabela explica cada variável no cmdlet:

    |Parâmetro de configuração|Nome da variável ou valor|Objetivo|
    |--|--|--|
    |-EndpointName|uswest de ponto final de subordinados|Perfil de oeste|
    |-TrafficManagerProfile|$parentprofile|Perfil para atribuir este ponto final|
    |-Type|NestedEndpoints|Para obter mais informações, consulte [adicionar AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0). |
    |-TargetResourceId|$westprofile. ID|ID do perfil subordinado|
    |-EndpointStatus|Ativado|Estado de ponto final após a adição ao principal|
    |-EndpointLocation|"westus"|[Nome da região do Azure](https://azure.microsoft.com/global-infrastructure/regions/) do recurso|
    |-MinChildEndpoints|1|Número mínimo de pontos finais subordinado|

    O aspeto de resposta com êxito como e inclui os dois anterior `child-endpoint-useast` ponto final e a nova `child-endpoint-uswest` ponto final:

    ```cmd
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-parent
    Name                             : luis-profile-parent
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-parent
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {child-endpoint-useast, child-endpoint-uswest}
    ```

4. Defina pontos finais com **[conjunto AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Set-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)** cmdlet 

    ```PowerShell
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $parentprofile
    ```

    Uma resposta com êxito é a mesma resposta como passo 3.

### <a name="powershell-variables"></a>Variáveis de PowerShell
Nas secções anteriores, foram criadas três variáveis de PowerShell: `$eastprofile`, `$westprofile`, `$parentprofile`. Estas variáveis são utilizadas para o fim da configuração do Gestor de tráfego. Se optar por não criar as variáveis, ou se esqueceu de ou janela do PowerShell exceder o tempo limite, pode utilizar o cmdlet do PowerShell,  **[Get-AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Get-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)**, de forma a obter novamente o perfil e atribuí-la para uma variável. 

Substitua os itens parênteses angulares, `<>`, com os valores corretos para cada um dos três perfis precisa. 

```PowerShell
$<variable-name> = Get-AzureRmTrafficManagerProfile -Name <profile-name> -ResourceGroupName luis-traffic-manager
```

## <a name="verify-traffic-manager-works"></a>Verifique o Gestor de tráfego funciona
Para verificar se o Gestor de tráfego de perfis de trabalho, os perfis têm de ter o estado de `Online` este estado é com base no caminho de consulta do ponto final. 

### <a name="view-new-profiles-in-the-azure-portal"></a>Vista de novos perfis no portal do Azure
Pode verificar que todos os três perfis são criados ao observar os recursos a `luis-traffic-manager` grupo de recursos.

![Captura de ecrã do recurso grupo luis-traffic manager do Azure-](./media/traffic-manager/traffic-manager-profiles.png)

### <a name="verify-the-profile-status-is-online"></a>Certifique-se de que o estado do perfil está Online
O Gestor de tráfego consultam o caminho de cada ponto final para se certificar de que está online. Se estiver online, o estado dos perfis de subordinados são `Online`. Esta informação é apresentada no **descrição geral** de cada perfil. 

![Perfil de captura de ecrã do Traffic Manager do Azure que mostra Monitor de estado de Online de descrição geral](./media/traffic-manager/profile-status-online.png)

### <a name="validate-traffic-manager-polling-works"></a>Validar o Gestor de tráfego funciona de consulta
Outra forma de validar o Gestor de tráfego funciona de consulta é com os registos de ponto final LUIS. No [LUIS] [ LUIS] lista de aplicações do Web site página, exportar o registo do ponto final para a aplicação. Porque o Gestor de tráfego, muitas vezes, consulta para os pontos dois finais, existem entradas nos registos do mesmo que possam tem sido apenas em alguns minutos. Lembre-se procure entradas onde a consulta começa com `traffic-manager-`.

```text
traffic-manager-west    6/7/2018 19:19  {"query":"traffic-manager-west","intents":[{"intent":"None","score":0.944767}],"entities":[]}
traffic-manager-east    6/7/2018 19:20  {"query":"traffic-manager-east","intents":[{"intent":"None","score":0.944767}],"entities":[]}
```

### <a name="validate-dns-response-from-traffic-manager-works"></a>Validar a resposta DNS do Gestor de tráfego funciona
Para validar que a resposta DNS devolve um ponto final LUIS, solicitar o perfil de tráfego gerir principal DNS a utilizar uma biblioteca de cliente DNS. Os nomes do DNS para o perfil de principal é `luis-dns-parent.trafficmanager.net`.

O seguinte código Node.js faz um pedido para o perfil de principal e devolve um ponto final LUIS:

```javascript
const dns = require('dns');

dns.resolveAny('luis-dns-parent.trafficmanager.net', (err, ret) => {
  console.log('ret', ret);
});
```

A resposta com êxito com o ponto final LUIS é:

```cmd
[
    {
        value: 'westus.api.cognitive.microsoft.com', 
        type: 'CNAME'
    }
]
```

## <a name="use-the-traffic-manager-parent-profile"></a>Utilizar o perfil de principal do Gestor de tráfego
Para gerir o tráfego entre pontos finais, tem de inserir uma chamada para o Gestor de tráfego de DNS para localizar o ponto final LUIS. Esta chamada é efetuada para todos os pedidos de ponto final LUIS e tem de modo a simular a localização geográfica do utilizador da aplicação de cliente LUIS. Adicione o código de resposta DNS entre a aplicação de cliente LUIS e o pedido LUIS para a predição de ponto final. 


## <a name="clean-up"></a>Limpeza
Remova duas chaves de subscrição de LUIS, três perfis do Traffic Manager e o grupo de recursos que continha estes cinco recursos. Isto é feito a partir do portal do Azure. Eliminar os recursos de cinco na lista de recursos. Em seguida, elimine o grupo de recursos. 

## <a name="next-steps"></a>Passos Seguintes

Reveja [middleware](https://docs.microsoft.com/azure/bot-service/bot-builder-create-middleware?view=azure-bot-service-4.0&tabs=csaddmiddleware%2Ccsetagoverwrite%2Ccsmiddlewareshortcircuit%2Ccsfallback%2Ccsactivityhandler) opções no BotFramework v4 para compreender a forma como este código de gestão de tráfego pode ser adicionado a um bot BotFramework. 

[traffic-manager-marketing]: https://azure.microsoft.com/services/traffic-manager/
[traffic-manager-docs]: https://docs.microsoft.com/azure/traffic-manager/
[LUIS]:luis-reference-regions.md#luis-website
[azure-portal]:https://portal.azure.com/
[azure-storage]:https://azure.microsoft.com/services/storage/
[routing-methods]:https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods
[traffic-manager-endpoints]:https://docs.microsoft.com/azure/traffic-manager/traffic-manager-endpoint-types
