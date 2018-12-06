---
title: Detalhes de configuração de rede para trabalhar com o Express Route
description: Detalhes de configuração de rede para a execução de ambientes de serviço de aplicações numa rede Virtual ligado a um circuito do ExpressRoute.
services: app-service
documentationcenter: ''
author: stefsch
manager: nirma
editor: ''
ms.assetid: 34b49178-2595-4d32-9b41-110c96dde6bf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/14/2016
ms.author: stefsch
ms.openlocfilehash: 6c9dcf5812d1d8efe7adbadc3647085664093bb1
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52969197"
---
# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>Detalhes da Configuração de Rede para Ambientes de Serviço de Aplicações com o ExpressRoute
## <a name="overview"></a>Descrição geral
Os clientes podem ligar-se um [Azure ExpressRoute] [ ExpressRoute] circuito para a sua infraestrutura de rede virtual, assim, expandir a sua rede no local para o Azure.  Um ambiente de serviço de aplicações podem ser criado numa sub-rede isso [rede virtual] [ virtualnetwork] infraestrutura.  Aplicações em execução no ambiente de serviço de aplicações, em seguida, podem estabelecer ligações seguras a recursos de back-end acessíveis apenas através da ligação de ExpressRoute.  

Um ambiente de serviço de aplicações podem ser criado no **ambos** uma rede virtual do Azure Resource Manager, **ou** uma rede virtual do modelo de implementação clássica.  Com uma alteração recente feita em Junho de 2016, os ASEs agora também podem ser implementados em redes virtuais que utilizam intervalos de endereços públicos ou espaços de endereços de RFC1918 (ou seja, endereços privados). 

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="required-network-connectivity"></a>Conectividade de rede necessária
Existem requisitos de conectividade de rede para ambientes de serviço de aplicações que não podem ser cumpridos inicialmente numa rede virtual ligada ao ExpressRoute.  Os ambientes de serviço de aplicações exigem todas as seguintes para funcionar corretamente:

* Conectividade de rede de saída para pontos finais de armazenamento do Azure em todo o mundo em ambas as portas 80 e 443.  Isto inclui pontos de extremidade localizados na mesma região que o ambiente de serviço de aplicações, bem como pontos finais de armazenamento localizados na **outros** regiões do Azure.  Pontos finais de armazenamento do Azure resolver sob os seguintes domínios DNS: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* e *file.core.windows.net*.  
* Conectividade de rede de saída para o serviço de ficheiros do Azure na porta 445.
* Conectividade de rede de saída para pontos finais de BD Sql localizado na mesma região que o ambiente de serviço de aplicações.  Pontos finais de BD SQL resolver sob o domínio seguinte: *database.windows.net*.  Isto requer a abertura de acesso a portas 1433, 11000 11999 e 14000 14999.  Para obter mais detalhes, consulte [este artigo sobre a utilização de portas de Sql da base de dados V12](../../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
* Conectividade de rede de saída para os pontos de extremidade de plano de gestão do Azure (ASM e ARM pontos de extremidade).  Isto inclui a conectividade de saída para ambos *management.core.windows.net* e *management.azure.com*. 
* Conectividade de rede de saída para *ocsp.msocsp.com*, *mscrl.microsoft.com* e *crl.microsoft.com*.  Isto é necessário para suportar a funcionalidade SSL.
* A configuração de DNS para a rede virtual tem de ser capaz de resolver todos os pontos de extremidade e domínios mencionados no pontos anteriores.  Se não conseguir resolver estes pontos finais, tentativas de criação do ambiente de serviço de aplicações irão falhar e ambientes de serviço de aplicações existentes serão marcados como mau estado de funcionamento.
* Acesso de saída na porta 53 é necessário para a comunicação com servidores DNS.
* Se existir um servidor DNS personalizado na outra extremidade de um gateway VPN, o servidor DNS tem de ser acessível a partir da sub-rede que contém o ambiente de serviço de aplicações. 
* O caminho de rede de saída não é possível viajar por meio de proxies corporativos internos, nem pode ser forçado a no local.  Isso altera o endereço NAT em vigor a partir de tráfego de rede de saída do ambiente de serviço de aplicações.  Alterar o endereço NAT de tráfego de rede de saída de um ambiente de serviço de aplicações irá causar falhas de conectividade para muitos dos pontos finais listados acima.  Isso resulta em tentativas falhadas de criação do ambiente de serviço de aplicações, bem como ambientes de serviço de aplicações anteriormente em bom estado marcado como mau estado de funcionamento.  
* Entrada de acesso à rede para as portas necessárias para ambientes de serviço de aplicações têm de ser permitidos conforme descrito neste [artigo][requiredports].

Podem ser cumpridos os requisitos de DNS, garantindo uma infraestrutura de DNS válida é configurada e mantida para a rede virtual.  Se por algum motivo, a configuração de DNS for alterada depois de criar um ambiente de serviço de aplicações, os desenvolvedores podem forçar um ambiente de serviço de aplicações de retirada a nova configuração de DNS.  Acionar um reinício sem interrupção de ambiente com o ícone "Reiniciar" localizado na parte superior do painel de gestão do ambiente de serviço de aplicações no [portal do Azure] [ NewPortal] fará com que o ambiente para pegar o novo DNS configuração.

Podem ser cumpridos os requisitos de acesso de rede de entrada através da configuração de um [grupo de segurança de rede] [ NetworkSecurityGroups] na sub-rede do ambiente de serviço de aplicações para permitir o acesso necessário, conforme descrito neste [ artigo][requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>Ativar a conectividade de rede de saída para um ambiente de serviço de aplicações
Por predefinição, um circuito ExpressRoute criado recentemente anuncia uma rota padrão que permite a conectividade de Internet de saída.  Com esta configuração de um ambiente de serviço de aplicações será possível estabelecer ligação com outros pontos finais do Azure.

No entanto, uma configuração de cliente comum é definir sua própria rota predefinida (0.0.0.0/0) que força o tráfego de Internet de saída para o fluxo em vez disso, no local.  Este fluxo de tráfego quebra Invariavelmente ambientes do serviço de aplicações, porque o tráfego de saída está bloqueado no local ou o NAT iria para um conjunto irreconhecível de endereços que já não funcionam com vários pontos de extremidade do Azure.

A solução é definir uma (ou mais) rotas definidas pelo utilizador (UDRs) na sub-rede que contém o ambiente de serviço de aplicações.  Um UDR define as rotas de sub-rede específica que serão cumpridas em vez da rota predefinida.

Se possível, é recomendado que utilize a seguinte configuração:

* A configuração de ExpressRoute anuncia 0.0.0.0/0 e por padrão, force túneis de todo o tráfego de saída no local.
* O UDR aplicado à sub-rede que contém o ambiente de serviço de aplicações define 0.0.0.0/0 com um tipo de próximo salto de Internet (um exemplo disso é mais baixo neste artigo).

O efeito combinado uma dessas etapas é que o nível de sub-rede UDR irá ter precedência sobre o ExpressRoute imposição de túnel, que garante o acesso de Internet de saída do ambiente de serviço de aplicações.

> [!IMPORTANT]
> As rotas definidas num UDR **tem** ser específico o suficiente para têm precedência sobre quaisquer rotas anunciadas pela configuração do ExpressRoute.  O exemplo abaixo utiliza o intervalo de endereços abrangente 0.0.0.0/0 e assim pode potencialmente ser substituído por acidente anúncios de rota de utilização de intervalos de endereço mais específicos.
> 
> Ambientes de serviço de aplicações não são suportados com configurações do ExpressRoute que **anunciem transversalmente rotas do caminho de peering público para o caminho de peering privado**.  Configurações do ExpressRoute com peering público configurado, receberá anúncios de rota da Microsoft para um grande conjunto de intervalos de endereços IP do Microsoft Azure.  Se estes intervalos de endereços forem anunciados transversalmente no caminho de peering privado, o resultado final é que todos os pacotes de rede de saída da sub-rede do ambiente de serviço de aplicações será a imposição de túnel para a infraestrutura de rede no local de um cliente.  Este fluxo de rede não é atualmente suportado com ambientes de serviço de aplicações.  Uma solução para esse problema é parar anunciar transversalmente rotas do caminho de peering público para o caminho de peering privado.
> 
> 

Informações básicas sobre rotas definidas pelo utilizador estão disponíveis neste [descrição geral][UDROverview].  

Detalhes sobre como criar e configurar rotas definidas pelo utilizador estão disponíveis neste [como a guia][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>Exemplo de configuração de UDR para um ambiente de serviço de aplicações
**Pré-requisitos**

1. Instalar o Azure Powershell a partir da [página de transferências do Azure] [ AzureDownloads] (meio desatualizada Junho de 2015 ou posterior).  Em "Ferramentas de linha de comandos" há uma ligação de "Instalar" em "Windows Powershell", que irá instalar os cmdlets do Powershell mais recente.
2. Recomenda-se que é criada uma sub-rede exclusiva para utilização exclusiva por um ambiente de serviço de aplicações.  Isto garante que as UDRs aplicadas à sub-rede apenas abrir o tráfego de saída para o ambiente de serviço de aplicações.
3. **Importante**: não implementa o ambiente de serviço de aplicações até **depois** são seguidos os seguintes passos de configuração.  Isto garante que a conectividade de rede de saída está disponível antes de tentar implementar um ambiente de serviço de aplicações.

**Passo 1: Criar uma tabela de rotas com nome**

O fragmento seguinte cria uma tabela de rotas chamada "DirectInternetRouteTable" na região Oeste nos Azure:

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**Passo 2: Criar uma ou mais rotas na tabela de rotas**

Precisará adicionar uma ou mais rotas à tabela de rotas para ativar o acesso de Internet de saída.  

A abordagem recomendada para a configuração de acesso de saída à Internet é definir uma rota para 0.0.0.0/0, conforme mostrado abaixo.

    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Lembre-se de que 0.0.0.0/0 é um intervalo de endereços abrangente e, como tal, irá ser substituído pelo mais específicos intervalos de endereços anunciados através do ExpressRoute.  Para iterar novamente a recomendação anterior, um UDR com uma rota de 0.0.0.0/0 deve ser utilizado em conjunto com uma configuração de ExpressRoute que anuncia apenas 0.0.0.0/0 também. 

Como alternativa, pode transferir uma lista abrangente e atualizada de intervalos CIDR em utilização pelo Azure.  O arquivo Xml contendo todos os intervalos de endereços IP do Azure está disponível a partir da [Microsoft Download Center][DownloadCenterAddressRanges].  

No entanto, observe que estes intervalos mudam ao longo do tempo, assim que exige atualizações manuais periódicas para as rotas definidas pelo utilizador para manter em sincronia.  Além disso, uma vez que existe um limite superior de padrão de 100 rotas num UDR único, terá de "resumir" os intervalos de endereços IP do Azure para caber dentro do limite de 100 rotas, tendo em mente que o UDR definidas rotas têm de ser mais específicas que as rotas anunciadas pela sua ExpressRo ute.  

**Passo 3: Associar a tabela de rotas à sub-rede que contém o ambiente de serviço de aplicações**

A última etapa de configuração é associar a tabela de rotas à sub-rede onde o ambiente de serviço de aplicações será implementado.  O comando seguinte associa o "DirectInternetRouteTable" para o "ASESubnet" que eventualmente irá conter um ambiente de serviço de aplicações.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**Passo 4: Etapas finais**

Depois da tabela de rotas é vinculada à sub-rede, recomenda-se de primeiro testar e confirmar o efeito pretendido.  Por exemplo, implementar uma máquina virtual para a sub-rede e confirme que:

* O tráfego de saída para o Azure e pontos de extremidade não pertencente ao Azure mencionado anteriormente neste artigo é **não** que flui para baixo o circuito do ExpressRoute.  É muito importante verificar este comportamento, uma vez que se o tráfego de saída da sub-rede é ainda está sendo forçado encapsulados no local, ambiente de serviço de aplicações criação irá falhar sempre. 
* Pesquisas de DNS para os pontos finais mencionado anteriormente todos a resolver corretamente. 

Assim que forem confirmados os passos acima, terá de eliminar a máquina virtual porque a sub-rede tem de ser "vazia" no momento que é criado o ambiente de serviço de aplicações.

Em seguida, continue com a criação de um ambiente de serviço de aplicações!

## <a name="getting-started"></a>Introdução
Para começar a utilizar com ambientes de serviço de aplicações, veja [introdução ao ambiente de serviço de aplicações][IntroToAppServiceEnvironment]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: https://azure.microsoft.com/services/expressroute/
[requiredports]: app-service-app-service-environment-control-inbound-traffic.md
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: https://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: https://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md
[AzureDownloads]: https://azure.microsoft.com/downloads/ 
[DownloadCenterAddressRanges]: https://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md
[NewPortal]:  https://portal.azure.com


<!-- IMAGES -->
