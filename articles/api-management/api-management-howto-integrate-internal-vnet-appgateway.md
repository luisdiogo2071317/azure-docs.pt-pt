---
title: Como utilizar a API Management do Azure na rede Virtual com o Gateway de aplicação | Documentos da Microsoft
description: Saiba como definir e configurar a gestão de API do Azure na rede Virtual interna com o Application Gateway (WAF) como front-end
services: api-management
documentationcenter: ''
author: solankisamir
manager: kjoshi
editor: vlvinogr
ms.assetid: a8c982b2-bca5-4312-9367-4a0bbc1082b1
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: sasolank
ms.openlocfilehash: c7d4351a9691c9787c42107306220e075f8648a0
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37435128"
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a>Integrar a gestão de API numa VNET interna com o Gateway de aplicação

##<a name="overview"> </a> Descrição geral

O serviço de gestão de API pode ser configurado numa rede Virtual em modo interno que faz com que seja acessível apenas a partir de dentro da rede Virtual. O Gateway de aplicação do Azure é um serviço de PAAS que fornece um balanceador de carga de camada 7. Ele atua como um serviço de proxy reverso e fornece entre a sua oferta de uma Firewall de aplicações Web (WAF).

A combinação de gestão de API aprovisionadas numa VNET interna com o front-end do Gateway de aplicação permite que os seguintes cenários:

* Utilize o mesmo recurso de gestão de API para consumo por clientes internos e externos consumidores.
* Utilize um único recurso de gestão de API e com um subconjunto de APIs definidas na gestão de API disponíveis para os consumidores externos.
* Proporcionam uma forma chave na mão para mudar o acesso à gestão de API da Internet pública e desativar.

## <a name="prerequisites"></a>Pré-requisitos

Para efetuar os passos descritos neste artigo, tem de ter:

+ Uma subscrição ativa do Azure.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Uma instância APIM. Para obter mais informações, consulte [criar uma instância de gestão de API do Azure](get-started-create-service-instance.md).

##<a name="scenario"> </a> Cenário
Este artigo aborda como usar um único serviço de gestão de API para consumidores internos e externos e torná-lo para atuar como um único front-end para ambos no local e na cloud APIs. Também verá como expor apenas um subconjunto das APIs (no exemplo que eles são destacados em verde) para consumo externo com a funcionalidade de PathBasedRouting disponível no Gateway de aplicação.

No primeiro exemplo de configuração todas as suas APIs são geridas apenas a partir de dentro da sua rede Virtual. Os consumidores internos (destacado na cor de laranja) podem aceder a todas as suas APIs internos e externos. Tráfego nunca sai para Internet um elevado desempenho é entregue através de circuitos do Expressroute.

![rota de URL](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <a name="before-you-begin"> </a> Antes de começar

1. Instale a versão mais recente dos cmdlets Azure PowerShell com o Instalador de Plataforma Web. Pode transferir e instalar a versão mais recente a partir da secção **Windows PowerShell** da página [Transferências](https://azure.microsoft.com/downloads/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
2. Criar uma rede Virtual e crie sub-redes separadas para gestão de API e o Gateway de aplicação.
3. Se pretende criar um servidor DNS personalizado para a rede Virtual, faça-o antes de iniciar a implementação. Verifique se que ele funciona, garantindo uma máquina virtual criada numa nova sub-rede na rede Virtual pode resolver e aceder a todos os pontos finais de serviço do Azure.

## <a name="what-is-required-to-create-an-integration-between-api-management-and-application-gateway"></a>O que é necessário para criar uma integração entre a gestão de API e o Gateway de aplicação?

* **Agrupamento de servidores de back-end:** este é o endereço IP virtual interno do serviço de gestão de API.
* **Definições do conjunto de servidores de back-end:** cada conjunto tem definições como a porta, o protocolo e a afinidade com base em cookies. Estas definições são aplicadas a todos os servidores dentro do conjunto.
* **Porta de front-end:** é a porta pública aberta no gateway de aplicação. Tráfego-lo é redirecionado para um dos servidores de back-end.
* **Serviço de escuta:** o serviço de escuta tem uma porta de front-end, um protocolo (Http ou Https; estes valores são sensíveis às maiúsculas e minúsculas) e o nome do certificado SSL (se configurar a descarga de SSL).
* **Regra:** a regra vincula um serviço de escuta a um agrupamento de servidores de back-end.
* **Sonda de estado de funcionamento personalizada:** Gateway de aplicação, por predefinição, utiliza sondas de baseada em endereço IP para saber quais os servidores no BackendAddressPool são Active Directory. A gestão de API do serviço apenas responda a pedidos que têm o cabeçalho de anfitrião correto, pelo que as sondas de predefinição falharem. Uma sonda de estado de funcionamento personalizado deve ser definido para o ajudar a determinar que o serviço está ativo e deve encaminhar os pedidos de gateway de aplicação.
* **Certificado de domínio personalizado:** para aceder a gestão de API a partir da internet tem de criar um mapeamento CNAME do respetivo nome de anfitrião para o nome DNS de front-end do Gateway de aplicação. Isto garante que o cabeçalho de nome de anfitrião e o certificado enviado para o Gateway de aplicação é encaminhada para a gestão de API é uma APIM pode reconhecer como válido.

## <a name="overview-steps"> </a> Passos necessários para integrar a gestão de API e o Gateway de aplicação

1. Crie um grupo de recursos para o Resource Manager.
2. Crie uma rede Virtual, uma sub-rede e um IP público para o Gateway de aplicação. Crie outra sub-rede para a gestão de API.
3. Criar um serviço de gestão de API dentro da sub-rede VNET criada acima e certifique-se de que utilizar o modo interno.
4. Configure o nome de domínio personalizado no serviço de gestão de API.
5. Crie um objeto de configuração do Gateway de aplicação.
6. Crie um recurso de Gateway de aplicação.
7. Crie um CNAME do nome DNS público do Gateway de aplicação para o nome de anfitrião do proxy de gestão de API.

## <a name="create-a-resource-group-for-resource-manager"></a>Criar um grupo de recursos para o Resource Manager

Verifique se está a utilizar a versão mais recente do Azure PowerShell. Para obter mais informações, veja [Using Windows PowerShell with Resource Manager (Usar o Windows PowerShell com o Resource Manager)](https://docs.microsoft.com/azure/azure-resource-manager/powershell-azure-resource-manager).

### <a name="step-1"></a>Passo 1

Iniciar sessão no Azure

```powershell
Connect-AzureRmAccount
```

Autenticar com as suas credenciais.<BR>

### <a name="step-2"></a>Passo 2

Verifique as subscrições para a conta e selecione-o.

```powershell
Get-AzureRmSubscription -Subscriptionid "GUID of subscription" | Select-AzureRmSubscription
```

### <a name="step-3"></a>Passo 3

Crie um novo grupo de recursos (ignore este passo se estiver a utilizar um grupo de recursos existente).

```powershell
New-AzureRmResourceGroup -Name "apim-appGw-RG" -Location "West US"
```
O Azure Resource Manager requer que todos os grupos de recursos especifiquem uma localização, Isto é utilizado como a localização predefinida para recursos nesse grupo de recursos. Certifique-se de que todos os comandos para criar um gateway de aplicação utilizam o mesmo grupo de recursos.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Criar uma rede Virtual e uma sub-rede para o gateway de aplicação

O exemplo seguinte mostra como criar uma rede Virtual com o resource manager.

### <a name="step-1"></a>Passo 1

Atribua o endereço intervalo 10.0.0.0/24 à variável da sub-rede a utilizar para o Gateway de aplicação ao criar uma rede Virtual.

```powershell
$appgatewaysubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a>Passo 2

Atribua o endereço intervalo 10.0.1.0/24 à variável da sub-rede a utilizar para a gestão de API ao criar uma rede Virtual.

```powershell
$apimsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a>Passo 3

Criar uma rede Virtual denominada **appgwvnet** no grupo de recursos **apim-appGw-RG** para a região EUA Oeste utilizando o prefixo 10.0.0.0/16 com sub-redes 10.0.0.0/24 e 10.0.1.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name "appgwvnet" -ResourceGroupName "apim-appGw-RG" -Location "West US" -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a>Passo 4

Atribuir uma variável da sub-rede para os passos seguintes

```powershell
$appgatewaysubnetdata=$vnet.Subnets[0]
$apimsubnetdata=$vnet.Subnets[1]
```
## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a>Criar um serviço de gestão de API dentro de uma VNET configurado no modo interno

O exemplo seguinte mostra como criar um serviço de gestão de API numa VNET configurada para apenas o acesso interno.

### <a name="step-1"></a>Passo 1
Crie um objeto de rede Virtual de gestão de API com a sub-rede $apimsubnetdata criada acima.

```powershell
$apimVirtualNetwork = New-AzureRmApiManagementVirtualNetwork -Location "West US" -SubnetResourceId $apimsubnetdata.Id
```
### <a name="step-2"></a>Passo 2
Crie um serviço de gestão de API dentro da rede Virtual.

```powershell
$apimService = New-AzureRmApiManagement -ResourceGroupName "apim-appGw-RG" -Location "West US" -Name "ContosoApi" -Organization "Contoso" -AdminEmail "admin@contoso.com" -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```
Depois do comando acima for concluída com êxito, consulte a [necessária configuração de DNS para aceder ao serviço de gestão de API de VNET interno](api-management-using-with-internal-vnet.md#apim-dns-configuration) para aceder ao mesmo.

## <a name="set-up-a-custom-domain-name-in-api-management"></a>Configurar um nome de domínio personalizado na gestão de API

### <a name="step-1"></a>Passo 1
Carregue o certificado com chave privada para o domínio. Para este exemplo é `*.contoso.net`.

```powershell
$certUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName "apim-appGw-RG" -Name "ContosoApi" -HostnameType "Proxy" -PfxPath <full path to .pfx file> -PfxPassword <password for certificate file> -PassThru
```

### <a name="step-2"></a>Passo 2
Depois do certificado é carregado, criar um objeto de configuração de nome de anfitrião para o proxy com um nome de anfitrião do `api.contoso.net`, como o certificado de exemplo fornece autoridade para o `*.contoso.net` domínio.

```powershell
$proxyHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certUploadResult.Thumbprint -Hostname "api.contoso.net"
$result = Set-AzureRmApiManagementHostnames -Name "ContosoApi" -ResourceGroupName "apim-appGw-RG" -ProxyHostnameConfiguration $proxyHostnameConfig
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Criar um endereço IP público para a configuração de front-end

Criar um recurso IP público **publicIP01** no grupo de recursos **apim-appGw-RG** para a região EUA oeste.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -name "publicIP01" -location "West US" -AllocationMethod Dynamic
```

É atribuído um endereço IP ao gateway de aplicação quando o serviço é iniciado.

## <a name="create-application-gateway-configuration"></a>Criar a configuração do gateway de aplicação

Tem de configurar todos os itens de configuração antes de criar o gateway de aplicação. Com os seguintes passos, irá criar os itens de configuração necessários para um recurso do gateway de aplicação.

### <a name="step-1"></a>Passo 1

Crie uma configuração de IP do gateway de aplicação com o nome **gatewayIP01**. Ao iniciar, o Application Gateway escolhe um endereço IP na subrede configurada e encaminha o tráfego da rede para os endereços IP no conjunto de IPs de back-end. Note que cada instância terá um endereço IP.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a>Passo 2

Configure a porta IP de front-end para o ponto de final IP público. Esta porta é a porta que os utilizadores finais que se ligam a.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "port01"  -Port 443
```
### <a name="step-3"></a>Passo 3

Configure o IP de front-end com o ponto final de IP público.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a>Passo 4

Configure o certificado para o Gateway de aplicação, utilizada para desencriptar e reencriptar o tráfego passa pelas.

```powershell
$cert = New-AzureRmApplicationGatewaySslCertificate -Name "cert01" -CertificateFile <full path to .pfx file> -Password <password for certificate file>
```

### <a name="step-5"></a>Passo 5

Crie o serviço de escuta HTTP para o Gateway de aplicação. Atribua o certificado de ssl, porta e configuração de IP Front-end à mesma.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert
```

### <a name="step-6"></a>Passo 6

Criar uma sonda personalizada para o serviço de gestão de API `ContosoApi` ponto final de domínio do proxy. O caminho `/status-0123456789abcdef` é um ponto final de estado de funcionamento predefinido alojado em todos os serviços de gestão de API. Definir `api.contoso.net` como um nome de anfitrião de sonda personalizada para protegê-lo com o certificado SSL.

> [!NOTE]
> O nome do anfitrião `contosoapi.azure-api.net` é o nome de anfitrião de proxy predefinida configurada quando um serviço com o nome `contosoapi` é criado no Azure público.
>

```powershell
$apimprobe = New-AzureRmApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName "api.contoso.net" -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
```

### <a name="step-7"></a>Passo 7

Carregue o certificado a ser utilizado nos recursos de agrupamento de back-end do habilitados para SSL. Este é o mesmo certificado que indicou no passo 4 acima.

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile <full path to .cer file>
```

### <a name="step-8"></a>Passo 8

Configure definições de back-end HTTP para o Gateway de aplicação. Isto inclui a definição de um limite de tempo limite para o pedido de back-end após o qual são canceladas. Este valor é diferente do que o limite de tempo de pesquisa.

```powershell
$apimPoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a>Passo 9

Configurar um conjunto de endereços IP de back-end com o nome **apimbackend** com o IP virtual interno endereço do serviço de gestão de API criada acima.

```powershell
$apimProxyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.StaticIPs[0]
```

### <a name="step-10"></a>Passo 10

Crie definições para um back-end fictício do (não existente). Pedidos para os caminhos de API que não desejemos expor da gestão de API através do Gateway de aplicação irão atingir este back-end e devolver 404.

Configure definições de HTTP para o back-end fictício.

```powershell
$dummyBackendSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "dummySetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled
```

Configurar um back-end fictício **dummyBackendPool**, que aponta para um endereço FQDN **dummybackend.com**. Este endereço FQDN não existe na rede virtual.

```powershell
$dummyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "dummyBackendPool" -BackendFqdns "dummybackend.com"
```

Criar uma definição de regra que o Gateway de aplicação irá utilizar por predefinição, que aponta para o back-end de inexistente **dummybackend.com** na rede Virtual.

```powershell
$dummyPathRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "nonexistentapis" -Paths "/*" -BackendAddressPool $dummyBackendPool -BackendHttpSettings $dummyBackendSetting
```

### <a name="step-11"></a>Passo 11

Configure caminhos de regra de URL para os conjuntos de back-end. Isto permite selecionar apenas algumas das APIs de gestão de API para que está a ser expostos ao público. Por exemplo, se existirem `Echo API` (/ eco /), `Calculator API` apenas a marca (/calc/) etc. `Echo API` acessível a partir da Internet).

O exemplo seguinte cria uma regra simples para o "/ eco /" caminho encaminhar o tráfego para o back-end "apimProxyBackendPool".

```powershell
$echoapiRule = New-AzureRmApplicationGatewayPathRuleConfig -Name "externalapis" -Paths "/echo/*" -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
```

Se o caminho não corresponder as regras de caminho que queremos ativar da gestão de API, a configuração de mapa do caminho de regra também configura um conjunto de endereços de back-end predefinido com o nome **dummyBackendPool**. Por exemplo, http://api.contoso.net/calc/sum direciona-o para **dummyBackendPool** conforme ele é definido como o conjunto predefinido para tráfego não correspondente.

```powershell
$urlPathMap = New-AzureRmApplicationGatewayUrlPathMapConfig -Name "urlpathmap" -PathRules $echoapiRule, $dummyPathRule -DefaultBackendAddressPool $dummyBackendPool -DefaultBackendHttpSettings $dummyBackendSetting
```

O passo acima garante que apenas os pedidos para o caminho "/ ecoar" são permitidos através do Gateway de aplicação. Pedidos para outras APIs configurado na gestão de API irão gerar 404 erros do Gateway da aplicação quando acede a partir da Internet.

### <a name="step-12"></a>Passo 12

Crie uma definição de regra para o Gateway de aplicação utilizar o encaminhamento baseado no caminho do URL.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType PathBasedRouting -HttpListener $listener -UrlPathMap $urlPathMap
```

### <a name="step-13"></a>Passo 13

Configure o número de instâncias e o tamanho para o Gateway de aplicação. Aqui estamos a utilizar o [SKU da WAF](../application-gateway/application-gateway-webapplicationfirewall-overview.md) para maior segurança do recurso de gestão de API.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-14"></a>Passo 14

Configure a WAF para estar no modo de "Prevenção".
```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a>Criar Gateway de aplicação

Crie um Gateway de aplicação com todos os objetos de configuração dos passos anteriores.

```powershell
$appgw = New-AzureRmApplicationGateway -Name $applicationGatewayName -ResourceGroupName $resourceGroupName  -Location $location -BackendAddressPools $apimProxyBackendPool, $dummyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $dummyBackendSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener -UrlPathMaps $urlPathMap -RequestRoutingRules $rule01 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert -Probes $apimprobe
```

## <a name="cname-the-api-management-proxy-hostname-to-the-public-dns-name-of-the-application-gateway-resource"></a>O nome de anfitrião de proxy de gestão de API para o nome DNS público do recurso de Gateway de aplicação de CNAME

Depois de o gateway ser criado, o passo seguinte consiste em configurar o front-end para a comunicação. Quando utilizar um IP público, o Gateway de aplicação requer um nome DNS dinamicamente atribuído, o que pode não ser fácil de usar.

Nome DNS do Gateway de aplicação deve ser utilizado para criar um registo CNAME que aponta o nome de anfitrião do proxy APIM (por exemplo, `api.contoso.net` nos exemplos acima) para este nome DNS. Para configurar o registo CNAME de IP de front-end, obter os detalhes do Gateway de aplicação e o respetivo nome IP/DNS associado com o elemento PublicIPAddress. Não é recomendada a utilização de registos, uma vez que o VIP pode ser alterado no reinício do gateway.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "apim-appGw-RG" -Name "publicIP01"
```

##<a name="summary"> </a> Resumo
Gestão de API do Azure configurado numa VNET fornece uma interface de gateway único para todas as APIs de configurados, quer estejam alojado no local ou na cloud. Integrar o Gateway de aplicação com a gestão de API fornece a flexibilidade de forma seletiva ativar APIs específicas para ser acessível na Internet, bem como fornecendo uma Firewall de aplicações Web como um front-end para a sua instância de gestão de API.

##<a name="next-steps"> </a> Próximas etapas
* Saiba mais sobre o Gateway de aplicação do Azure
  * [Descrição geral do Gateway de aplicação](../application-gateway/application-gateway-introduction.md)
  * [Firewall de aplicações da Web de Gateway de aplicação](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [Gateway de aplicação com encaminhamento baseado no caminho](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* Saiba mais sobre a gestão de API e VNETs
  * [Utilizar a gestão de API disponíveis apenas dentro da VNET](api-management-using-with-internal-vnet.md)
  * [Utilizar a gestão de API na VNET](api-management-using-with-vnet.md)
