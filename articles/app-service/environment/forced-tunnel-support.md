---
title: "Configurar o Ambiente de Serviço de Aplicações do Azure com túnel forçado"
description: "Permita que o seu Ambiente de Serviço de Aplicações funcione quando o tráfego de saída está configurado com túnel forçado"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 384cf393-5c63-4ffb-9eb2-bfd990bc7af1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/10/2017
ms.author: ccompy
ms.custom: mvc
ms.openlocfilehash: 4caaf0df3f1dd4b2cb9b76283a6beed897531c1c
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="configure-your-app-service-environment-with-forced-tunneling"></a>Configurar o Ambiente de Serviço de Aplicações com túnel forçado

O Ambiente de Serviço de Aplicações é uma implementação do Serviço de Aplicações do Azure na instância de um cliente da Rede Virtual do Azure. Muitos clientes configuram as suas redes virtuais para serem extensões das respetivas redes no local com VPNs ou ligações do ExpressRoute do Azure. Devido a políticas empresariais ou outras limitações de segurança, configuram rotas para enviar todo o tráfego de saída no local antes de poder ir para a Internet. A alteração do encaminhamento da rede virtual de modo a que o tráfego de saída da rede virtual flua através da ligação VPN ou ExpressRoute para o local é chamada de túnel forçado. 

O túnel forçado pode causar problemas a um Ambiente de Serviço de Aplicações. O Ambiente de Serviço de Aplicações tem um número de dependências externas, que são enumeradas no documento [Arquitetura de Rede do Ambiente de Serviço de Aplicações][network]. Por predefinição, o Ambiente de Serviço de Aplicações exige que todas as comunicações de saída passem pelo VIP que é aprovisionado com o Ambiente de Serviço de Aplicações.

As rotas são um aspeto fundamental do conceito de túnel forçado e de como lidar com o mesmo. Numa rede virtual do Azure, o encaminhamento é feito com base no LPM (Longest Prefix Match). Se houver mais de uma rota com a mesma correspondência LPM, uma rota é selecionada com base na respetiva origem pela seguinte ordem:

* Rota definida pelo utilizador (UDR)
* Rota BGP (quando é utilizado o ExpressRoute)
* Rota de sistema

Para saber mais sobre o encaminhamento numa rede virtual, veja [Rotas definidas pelo utilizador e reencaminhamento IP][routes]. 

Se pretender que o Ambiente de Serviço de Aplicações funcione numa rede virtual com túnel forçado, tem duas opções:

* Ative o Ambiente de Serviço de Aplicações para ter acesso direto à Internet.
* Altere o ponto final de saída para o Ambiente de Serviço de Aplicações.

## <a name="enable-your-app-service-environment-to-have-direct-internet-access"></a>Ativar o Ambiente de Serviço de Aplicações para ter acesso direto à Internet

Para que o Ambiente de Serviço de Aplicações funcione enquanto a rede virtual está configurada com uma ligação do ExpressRoute, pode:

* Configurar o ExpressRoute para anunciar 0.0.0.0/0. Por predefinição, aplica um túnel forçado a todo o tráfego de saída no local.
* Criar um UDR. Aplique-o à sub-rede que contém o Ambiente de Serviço de Aplicações com o prefixo de endereço 0.0.0.0/0 e um tipo de próximo salto de Internet.

Se fizer estas duas alterações, o tráfego destinado à Internet que provém da sub-rede do Ambiente de Serviço de Aplicações não fica forçado à ligação do ExpressRoute e o Ambiente de Serviço de Aplicações funciona.

> [!IMPORTANT]
> As rotas definidas num UDR têm de ser suficientemente específicas para terem precedência sobre quaisquer rotas anunciadas pela configuração do ExpressRoute. O exemplo anterior utiliza o intervalo de endereços abrangente 0.0.0.0/0. Podem ser potencialmente substituídas por acidente por anúncios de rota que utilizam intervalos de endereço mais específicos.
>
> Os Ambientes de Serviço de Aplicações não são suportados com configurações do ExpressRoute que anunciem transversalmente rotas do caminho de peering público para o caminho de peering privado. As configurações do ExpressRoute com o peering público configurado recebem anúncios de rota da Microsoft. Os anúncios contêm um vasto conjunto de intervalos de endereços IP do Microsoft Azure. Se os intervalos de endereços forem anunciados transversalmente no caminho de peering privado, todos os pacotes de rede de saída da sub-rede do Ambiente de Serviço de Aplicações são submetidos a um túnel forçado para a infraestrutura de rede no local do cliente. Atualmente, este fluxo de rede não é suportado por predefinição com Ambientes de Serviço de Aplicações. Uma solução para este problema é deixar de anunciar transversalmente as rotas do caminho de peering público para o caminho de peering privado. Outra solução é ativar o Ambiente de Serviço de Aplicações para funcionar numa configuração de túnel forçado.

## <a name="change-the-egress-endpoint-for-your-app-service-environment"></a>Alterar o ponto final de saída para o Ambiente de Serviço de Aplicações ##

Esta secção descreve como ativar um Ambiente de Serviço de Aplicações para funcionar numa configuração de túnel forçado ao alterar o ponto final de saída utilizado pelo Ambiente de Serviço de Aplicações. Se o tráfego de saída do Ambiente de Serviço de Aplicações for configurado com túnel forçado para uma rede no local, terá de permitir que esse tráfego provenha de endereços IP que não sejam o endereço VIP do Ambiente de Serviço de Aplicações.

Um Ambiente de Serviço de Aplicações não só tem dependências externas, como também tem de escutar o tráfego de entrada e responder a esse tráfego. As respostas não podem ser reenviadas a partir de outro endereço, uma vez que interrompe o TCP. Existem três passos necessários para alterar o ponto final de saída para o Ambiente de Serviço de Aplicações:

1. Defina uma tabela de rotas para garantir que o tráfego de entrada de gestão pode voltar a partir do mesmo endereço IP.

2. Adicione os endereços IP que vão ser utilizados para a saída à firewall do Ambiente de Serviço de Aplicações.

3. Defina as rotas para o tráfego de saída do Ambiente de Serviço de Aplicações para serem em túnel.

   ![Fluxo de rede do túnel forçado][1]

Pode configurar o Ambiente de Serviço de Aplicações com endereços de saída diferentes depois de o Ambiente de Serviço de Aplicações já estar operacional, ou os endereços de saída podem ser definidos durante a implementação do Ambiente de Serviço de Aplicações.

### <a name="change-the-egress-address-after-the-app-service-environment-is-operational"></a>Alterar o endereço de saída depois de o Ambiente de Serviço de Aplicações estar operacional ###
1. Obtenha os endereços IP que pretende utilizar como IPs de saída para o Ambiente de Serviço de Aplicações. Se estiver a utilizar o túnel forçado, estes endereços provêm dos seus NATs ou IPs de gateway. Se pretender encaminhar o tráfego de saída do Ambiente de Serviço de Aplicações através de uma NVA, o endereço de saída é o IP público da NVA.

2. Defina os endereços de saída nas informações de configuração do seu Ambiente de Serviço de Aplicações. Aceda a resource.azure.com e aceda a Subscription/<subscription id>/resourceGroups/<ase resource group>/providers/Microsoft.Web/hostingEnvironments/<ase name>. Depois, poderá ver o JSON que descreve o Ambiente de Serviço de Aplicações. Certifique-se de que diz **leitura/escrita** na parte superior. Selecione **Editar**. Desloque o ecrã para baixo até à parte inferior e altere o valor de **userWhitelistedIpRanges** de **nulo** para algo semelhante ao seguinte. Utilize os endereços que pretende definir como o intervalo de endereços de saída. 

        "userWhitelistedIpRanges": ["11.22.33.44/32", "55.66.77.0/24"] 

   Selecione **PUT** na parte superior. Esta opção aciona uma operação de dimensionamento no seu Ambiente de Serviço de Aplicações e ajusta a firewall.
 
3. Crie ou edite uma tabela de rotas e preencha as regras para permitir o acesso aos/dos endereços de gestão que mapeiam para a localização do Ambiente de Serviço de Aplicações. Para obter os endereços de gestão, veja [Endereços de gestão do Ambiente de Serviço de Aplicações][management].

4. Ajuste as rotas aplicadas à sub-rede do Ambiente de Serviço de Aplicações com uma tabela de rotas ou rotas BGP. 

Se o Ambiente de Serviço de Aplicações ficar sem resposta a partir do portal, há um problema nas suas alterações. O problema poderá ser a lista de endereços de saída estar incompleta, o tráfego ter-se perdido ou o tráfego ter sido bloqueado. 

### <a name="create-a-new-app-service-environment-with-a-different-egress-address"></a>Criar um novo Ambiente de Serviço de Aplicações com um endereço de saída diferente ###

Se a rede virtual já estiver configurada para forçar o túnel de todo o tráfego, terá de executar passos adicionais para criar o Ambiente de Serviço de Aplicações de forma a que possa surgir com êxito. Tem de ativar a utilização de outro ponto final de saída durante a criação do Ambiente de Serviço de Aplicações. Para tal, tem de criar o Ambiente de Serviço de Aplicações com um modelo que especifique os endereços de saída permitidos.

1. Obtenha os endereços IP a utilizar como os endereços de saída para o Ambiente de Serviço de Aplicações.

2. Crie previamente a sub-rede que vai ser utilizada pelo Ambiente de Serviço de Aplicações. É necessária para que possa definir as rotas e também porque o modelo precisa dela.

3. Crie uma tabela de rotas com os IPs de gestão que mapeiam para a localização do Ambiente de Serviço de Aplicações. Atribua-a ao seu Ambiente de Serviço de Aplicações.

4. Siga as instruções em [Criar um Ambiente de Serviço de Aplicações com um modelo][template]. Obtenha o modelo apropriado.

5. Edite a secção "resources" do ficheiro azuredeploy.json. Inclua uma linha para **userWhitelistedIpRanges** com os seus valores, desta forma:

       "userWhitelistedIpRanges":  ["11.22.33.44/32", "55.66.77.0/30"]

Se esta secção for corretamente configurada, o Ambiente de Serviço de Aplicações deve ser iniciado sem problemas. 


<!--IMAGES-->
[1]: ./media/forced-tunnel-support/forced-tunnel-flow.png

<!--Links-->
[management]: ./management-addresses.md
[network]: ./network-info.md
[routes]: ../../virtual-network/virtual-networks-udr-overview.md
[template]: ./create-from-template.md
