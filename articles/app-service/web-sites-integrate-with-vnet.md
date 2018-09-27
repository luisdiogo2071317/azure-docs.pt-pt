---
title: Integrar uma aplicação com uma rede Virtual do Azure
description: Mostra-lhe como ligar uma aplicação no App Service do Azure a uma rede de virtual do Azure nova ou existente
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2018
ms.author: ccompy
ms.openlocfilehash: 5eab09d5dffe16517e8c18eb0281716618ca0286
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166236"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Integrar a sua aplicação com uma rede Virtual do Azure
Este documento descreve a funcionalidade de integração de rede virtual do App Service do Azure e mostra como configurá-lo com as aplicações no [App Service do Azure](http://go.microsoft.com/fwlink/?LinkId=529714). Se não estiver familiarizado com as redes virtuais do Azure (VNets), isso é uma funcionalidade que permite que colocar muitos dos seus recursos do Azure numa rede de endereçáveis não internet que controlam o acesso a. Estas redes, em seguida, podem ser ligadas às suas redes no local utilizando uma variedade de tecnologias VPN. Para saber mais sobre redes virtuais do Azure, comece com as informações aqui: [descrição geral de rede Virtual do Azure][VNETOverview]. 

O serviço de aplicações do Azure tem duas formas. 

1. Os sistemas de vários inquilinos que suportam a gama completa de planos de preços
2. O recurso de premium ambiente de serviço de aplicações (ASE), que implementa na sua VNet. 

Este documento aborda a integração de VNet e de ambiente de serviço de aplicações não. Se quiser saber mais sobre a funcionalidade do ASE, comece com as informações aqui: [introdução de ambiente de serviço de aplicações][ASEintro].

Integração VNet oferece o acesso a aplicações web a recursos na sua rede virtual, mas não lhe concede acesso privado à sua aplicação web da rede virtual. Acesso a sites privada refere-se para tornar a sua aplicação apenas acessível a partir de uma rede privada, tais como a partir de dentro de uma rede virtual do Azure. Acesso de sites privados só está disponível com um ASE configurado com uma carga balanceador interno (ILB). Para obter detalhes sobre como utilizar um ASE de ILB, começar com o artigo aqui: [criando e usando um ASE de ILB][ILBASE]. 

Um cenário comum onde usaria integração VNet está a permitir o acesso da sua aplicação web para uma base de dados ou um serviço da web em execução numa máquina virtual na sua rede virtual do Azure. Com a integração de VNet, não terá de expor um ponto final público para aplicações na sua VM, mas pode utilizam os endereços de encaminháveis privados não internet em vez disso. 

A funcionalidade de integração de VNet:

* requer uma Standard, Premium ou o plano de preços isolada 
* funciona com clássica ou VNet do Resource Manager 
* oferece suporte a TCP e UDP
* funciona com a Web, móveis, aplicações API e aplicações de funções
* permite que uma aplicação para ligar a VNet apenas 1 cada vez
* permite que até cinco VNets a ser integrado com um plano do serviço de aplicações 
* permite que a mesma VNet ser utilizado por várias aplicações num plano do serviço de aplicações
* suporta um SLA de 99,9% devido ao SLA no Gateway de VNet

Existem algumas coisas que a integração de VNet não suporta, incluindo:

* montar uma unidade
* Integração do AD 
* NetBios
* acesso de sites privados

### <a name="getting-started"></a>Introdução
Seguem-se alguns aspetos a ter em mente antes de ligar a sua aplicação web a uma rede virtual:

* Integração VNet apenas funciona com aplicações numa **padrão**, **Premium**, ou **Isolated** plano de preços. Se ativar a funcionalidade e, depois, aumentar o seu plano do App Service para um plano de preços sem suporte as suas aplicações perdem as suas ligações para VNets que estão a utilizar. 
* Se a rede virtual de destino já existir, tem de ter ativado com um gateway de encaminhamento dinâmico antes de ele pode ser conectado a uma aplicação VPN de ponto a site. Se o seu gateway está configurado com o encaminhamento estático, não é possível ativar o ponto-para-site de rede privada Virtual (VPN).
* A VNet tem de ser na mesma subscrição que sua Plan(ASP) do serviço de aplicações.
* Se o gateway já existe com ponto a site ativado e não se encontra no SKU básico, IKEV2 tem de ser desativada na configuração ponto a site.
* As aplicações que se integram com uma utilização de VNet, o DNS especificado para essa VNet.
* Por predefinição é sua integração de aplicações apenas encaminham o tráfego para a VNet com base nas rotas definidas na sua VNet. 

## <a name="enabling-vnet-integration"></a>Ativar a integração de VNet

Tem a opção para ligar a aplicação a uma rede virtual nova ou existente. Se criar uma nova rede como parte de sua integração, em seguida, além de simplesmente criar a VNet, um gateway de encaminhamento dinâmico está previamente configurado por si e ponto-para-Site VPN estiver ativada. 

> [!NOTE]
> Configurar uma nova integração da rede virtual pode demorar vários minutos. 
> 
> 

Para ativar a integração de VNet, abra as definições de aplicação e selecione o sistema de rede. A interface do Usuário que se abre oferece três opções de funcionamento em rede. Este guia é apenas entrar em integração de VNet entanto ligações híbridas e ambientes de serviço de aplicações são abrangidas mais adiante neste documento. 

Se a sua aplicação é não correto no plano de preços, a interface do Usuário permite-lhe dimensionar o seu plano para um plano de preços superior à sua escolha.

![][1]

### <a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>Ativar a integração de VNet com uma VNet já existente
A interface do Usuário da integração de VNet permite-lhe selecionar a partir de uma lista das suas VNets. As VNets clássicas indicam que estão, tais com a palavra "Clássico" parênteses junto ao nome da VNet. A lista é ordenada, de modo a que as VNets do Resource Manager são listadas primeiro. Na imagem abaixo, pode ver o que pode ser selecionada apenas uma VNet. Existem vários motivos que uma VNet pode ser a cinzento incluindo:

* a VNet é noutra subscrição que sua conta tem acesso ao
* a VNet não tem ponto a Site ativado
* a VNet não tem um gateway de encaminhamento dinâmico

![][2]

Para ativar a integração, clique na VNet em modo de que pretende integrar. Depois de selecionar a VNet, a aplicação é automaticamente reiniciada para que as alterações entrem em vigor. 

##### <a name="enable-point-to-site-in-a-classic-vnet"></a>Ativar ponto a Site numa rede virtual clássica

Se a VNet não tem um gateway nem tem o ponto a Site, terá de configurar essa opção primeiro. Para configurar um gateway para uma VNet clássica, aceda ao portal e abrir a lista de Virtual Networks(classic). Selecione a rede que pretende integrar. Selecione ligações VPN. A partir daqui, pode criar seu ponto de VPN de site e até mesmo fazê-lo a criar um gateway. O gateway demora cerca de 30 minutos a surgir.

![][8]

##### <a name="enabling-point-to-site-in-a-resource-manager-vnet"></a>Ativar o ponto a Site numa VNet do Resource Manager
Para configurar uma VNet do Resource Manager com um gateway e o ponto a Site, pode utilizar o PowerShell conforme documentado aqui, [configurar uma ligação de ponto a Site a uma rede virtual com o PowerShell] [ V2VNETP2S] ou utilizar o portal do Azure conforme documentado aqui [configurar uma ligação ponto a Site a uma VNet com o portal do Azure][V2VNETPortal]. A interface do Usuário para realizar esta capacidade ainda não está disponível. Não precisa de criar certificados para a configuração de ponto a Site. Certificados são criados automaticamente quando se liga a sua aplicação Web para a VNet com o portal. 

### <a name="creating-a-pre-configured-vnet"></a>Criar uma VNet previamente configurada
Se quiser criar uma nova VNet que está configurada com um gateway e ponto a Site, em seguida, o serviço de aplicações de rede da interface do Usuário tem a capacidade de fazer isso, mas apenas para uma VNet do Resource Manager. Se quiser criar uma VNet clássica com um gateway e o ponto a Site, terá de fazer isso manualmente por meio da interface de utilizador do sistema de rede. 

Para criar uma VNet do Resource Manager através da IU de integração de VNet, selecione **criar nova rede Virtual** e fornecer a:

* Nome da rede virtual
* Bloco de Endereços de Rede Virtual
* Nome da sub-rede
* Bloco de Endereços de Sub-rede
* Bloco de endereços de gateway
* Bloco de Endereços Ponto a Site

Se pretender que esta VNet ligar a quaisquer outras redes, em seguida, deve evitar escolher espaço de endereços IP que se sobreponha essas redes. 

> [!NOTE]
> Criação de VNet do Resource Manager com um gateway demora cerca de 30 minutos e atualmente não se integra a VNet com a sua aplicação. Depois de criar a VNet com o gateway, terá de voltar à sua aplicação da interface do Usuário de integração de VNet e selecione a sua nova VNet.
> 
> 

![][3]

VNets do Azure normalmente são criados dentro de endereços de rede privada. Por predefinição, a integração de VNet a funcionalidade encaminha qualquer tráfego destinado a esses intervalos de endereços IP na sua VNet. Os intervalos de endereços IP privados são:

* 10.0.0.0/8 - Este é o mesmo que 10.0.0.0 - 10.255.255.255
* 172.16.0.0/12 - este é o mesmo que 172.16.0.0 - 172.31.255.255 
* 192.168.0.0/16 - este é o mesmo que 192.168.0.0 - 192.168.255.255

O espaço de endereços da VNet tem de ser especificado na notação CIDR. Se não estiver familiarizado com notação CIDR, é um método para especificar os blocos de endereços com um endereço IP e um número inteiro que representa a máscara de rede. Como uma referência rápida, considere que 10.1.0.0/24 seria 256 endereços e 10.1.0.0/25 seria 128 endereços. Um endereço IPv4 com um /32 seria apenas 1 endereço. 

Se definir aqui as informações do servidor DNS, que está definido para a sua VNet. Após a criação da VNet pode editar estas informações a partir das experiências de utilizador de VNet. Se alterar o DNS da VNet, em seguida, terá de efetuar uma operação de rede de sincronização.

Quando cria uma VNet clássica, utilizando a interface do Usuário da integração de VNet, ele cria uma VNet no mesmo grupo de recursos que a sua aplicação. 

## <a name="how-the-system-works"></a>Como o sistema funciona
Nos bastidores esta funcionalidade está baseado na tecnologia VPN ponto a Site para ligar a sua aplicação à sua VNet. As aplicações no serviço de aplicações do Azure têm uma arquitetura de sistema multi-inquilino, o que impede o aprovisionamento de uma aplicação diretamente numa VNet como é feito com máquinas virtuais. Com base em tecnologia de ponto a site, limitar o acesso de rede para apenas a máquina virtual que aloja a aplicação. Acesso à rede é mais restrito nos anfitriões de aplicação para que as suas aplicações apenas podem aceder as redes que configurá-las para aceder. 

![][4]

Se ainda não configurou um servidor DNS com a sua rede virtual, a aplicação terá de utilizar endereços IP para chegar a recursos na VNet. Ao utilizar endereços IP, lembre-se de que o principal benefício desta funcionalidade é que ele permite-lhe utilizar os endereços privados dentro da sua rede privada. Se definir a sua aplicação para usar o endereços IP públicos para uma das suas VMs, em seguida, não estiver a utilizar a funcionalidade de integração de VNet e está a comunicar através da internet.

## <a name="managing-the-vnet-integrations"></a>Gerir as integrações de VNet
A capacidade para se ligar e desligar a uma VNet é ao nível da aplicação. Operações que podem afetar a integração de VNet em várias aplicações estão num nível ASP. Desde a interface do Usuário que é mostrado ao nível da aplicação, pode obter os detalhes na sua VNet. A maioria das mesmas informações também é mostrado ao nível do ASP. 

![][5]

A página de estado da funcionalidade de rede, pode ver se a sua aplicação está ligada a sua VNet. Se o gateway de VNet estiver em baixo por algum motivo, em seguida, isso mostrará como não-conectado. 

As informações que tem agora disponível na aplicação da que IU de integração de VNet nível é o mesmo que as informações de detalhe, que obter a partir do ASP. Seguem-se esses itens:

* Nome da VNet - esta ligação abre-se a rede virtual do Azure da interface do Usuário
* Localização – isso reflete a localização da sua VNet. É possível integrar com uma VNet em outro local.
* Estado do certificado - existem certificados usados para proteger a ligação VPN entre a aplicação e a VNet. Isto reflete um teste para garantir que estão em sincronização.
* Estado do gateway - devem seus gateways ficar indisponíveis por algum motivo, em seguida, a aplicação não é possível aceder aos recursos na VNet. 
* Espaço de endereços da VNet – este é o espaço de endereços IP para a sua VNet. 
* Espaço de endereços de ponto a Site - este é o ponto de site IP espaço de endereços da vnet. A aplicação mostra a comunicação como proveniente de um dos IPs neste espaço de endereço. 
* Site de espaço de endereço do site - pode utilizar VPNs de Site a Site para ligar a VNet, aos seus recursos no local ou a outra VNet. Deve ter que configurado, em seguida, os intervalos de IP definidas com que a ligação de VPN mostra aqui.
* Servidores de DNS – se tiver configurado com a sua VNet, de servidores de DNS, em seguida, eles estão listados aqui.
* IPs encaminhado para a VNet - daí são uma lista de endereços IP que a VNet tem de encaminhamento definidas para e esses endereços mostrados aqui. 

É a única operação que pode demorar na vista de aplicação de sua integração de VNet para desligar a sua aplicação da VNet está atualmente ligada. Para desligar a sua aplicação a partir de uma VNet, desligue na parte superior. Esta ação não altera a sua VNet. A VNet e a respetiva configuração, incluindo os gateways permanece inalterado. Se desejar, em seguida, ao eliminar a sua VNet, terá de eliminar primeiro os recursos, incluindo os gateways. 

A vista do plano do serviço de aplicações tem um número de operações adicionais. Ele também é acessado diferente do que a partir da aplicação. Para aceder a IU de sistema de rede de ASP, abra a interface do Usuário do ASP e desloque-se para baixo. Selecione "Estado de recurso de rede" para abrir a IU de estado do recurso de rede. Selecione "Clique aqui para gerir" à lista as integrações de VNet neste ASP.

![][6]

A localização do ASP é bom lembrar ao observar as localizações das VNets que estiver a integrar com. Quando a VNet está em outro local são muito mais provável ver problemas de latência. 

As VNets integradas com é um lembrete em VNets quantas que as suas aplicações estão integradas neste ASP e quantos pode ter. 

Para ver foram adicionados detalhes sobre cada VNet, basta clicar na VNet em modo de interesse. Além dos detalhes que foram observados anteriormente, também pode ver uma lista de aplicações neste ASP que estão a utilizar essa VNet. 

Relativamente a ações, existem duas ações primárias. A primeira é a capacidade de adicionar as rotas que direcionam o tráfego que sai de seu aplicativo na sua VNet. A segunda ação é a capacidade de sincronizar certificados e informações de rede.

![][7]

**Encaminhamento** conforme observado anteriormente, as rotas definidas na sua VNet são o que é utilizado para direcionar o tráfego numa VNet a partir de seu aplicativo. Há alguns usos, embora em que os clientes pretendem enviar tráfego de saída adicional de uma aplicação na VNet e para os mesmos esta capacidade é fornecido. O que acontece ao tráfego depois que cabe como o cliente configura a sua VNet. 

**Certificados** o estado do certificado reflete uma verificação a ser efetuada pelo serviço de aplicações para validar se os certificados que estamos a utilizar para a ligação VPN estão ainda é válidos. Quando a integração VNet ativada, em seguida, se esta for a integração de primeira para essa VNet de todas as aplicações neste ASP, há um exchange necessário de certificados para garantir a segurança da ligação. Juntamente com os certificados obtemos a configuração de DNS, as rotas e outros elementos semelhantes que descrevem a rede.
Se esses certificados ou as informações de rede for alterado, terá de clicar em "Sincronizar rede". **Tenha em atenção**: ao clicar em "Sincronizar rede", em seguida, fará com que uma rápida interrupção na conectividade entre a aplicação e a sua VNet. Enquanto a aplicação não for reiniciada, a perda de conectividade pode fazer com que seu site não funcione corretamente. 

## <a name="accessing-on-premises-resources"></a>Aceder a recursos no local
Um dos benefícios da funcionalidade de integração de VNet é que, se a VNet está ligada à sua rede no local com uma VPN de Site a Site, em seguida, as suas aplicações podem ter acesso aos seus recursos no local da sua aplicação. Para isso funcionar, embora poderá ter de atualizar o gateway de VPN no local com as rotas para o seu IP ponto a Site no intervalo. Quando a VPN de Site a Site é configurada em primeiro lugar, em seguida, os scripts utilizados para configurá-lo devem configurar as rotas, incluindo a VPN ponto a Site. Se adicionar a VPN ponto a Site depois de criar a VPN de Site a Site, terá de atualizar manualmente as rotas. Obter detalhes sobre como fazer isso variam por gateway e não são descritas aqui. 

> [!NOTE]
> A funcionalidade de integração de VNet não integra a uma aplicação com uma VNet com um Gateway do ExpressRoute. Mesmo que o Gateway do ExpressRoute está configurado nas [modo de coexistência] [ VPNERCoex] a integração de VNet não funciona. Se precisa aceder aos recursos através de uma ligação ExpressRoute, pode utilizar um [ambiente do serviço de aplicações][ASE], que é executada na sua VNet.
> 
> 

## <a name="pricing-details"></a>Detalhes dos preços
Existem algumas nuances que deve estar ciente de quando utilizar a funcionalidade de integração de VNet de preços. Existem 3 custos relacionados a utilização desta funcionalidade:

* Requisitos de escalão de preço de ASP
* Custos da transferência de dados
* Custos de Gateway de VPN.

Para as suas aplicações poder utilizar esta funcionalidade, eles precisam estar num padrão ou o plano do serviço de aplicações Premium. Pode ver mais detalhes sobre esses custos aqui: [preços do App Service][ASPricing]. 

Devido a como são tratadas as VPNs ponto a Site, sempre tem um custo para dados de saída por meio de sua conexão de integração de VNet, mesmo que a VNet está no mesmo centro de dados. Para ver quais são esses custos, veja o seguinte: [detalhes de preços de transferência de dados][DataPricing]. 

O último item é o custo dos gateways de VNet. Se não precisar dos gateways para algo mais, como as VPNs Site a Site, em seguida, tem de pagar para gateways suportar a funcionalidade de integração de VNet. Existem detalhes sobre esses custos aqui: [preços de Gateway de VPN][VNETPricing]. 

## <a name="troubleshooting"></a>Resolução de problemas
Embora a funcionalidade seja fácil de configurar, isso não significa que a sua experiência será problema gratuito. Caso encontre problemas para acessar o ponto de final desejado aqui estão alguns utilitários que pode usar para testar a conectividade a partir da consola de aplicação. Há duas experiências de consola, que pode utilizar. Uma é a partir da consola Kudu e o outro é a consola que pode entrar no portal do Azure. Para obter para a consola Kudu da sua aplicação go para ferramentas -> Kudu. Este é o mesmo que vai [sitename]. scm.azurewebsites.net. Uma vez que se abre, aceda ao separador consola de depuração. Para obter a consola alojada portal do Azure, em seguida, da sua aplicação go para ferramentas -> consola. 

#### <a name="tools"></a>Ferramentas
As ferramentas **ping**, **nslookup** e **tracert** não funcionará através da consola devido a restrições de segurança. Para preencher o daí void foram duas ferramentas separadas adicionadas. Para testar a funcionalidade DNS, adicionamos uma ferramenta denominada nameresolver.exe. A sintaxe é:

    nameresolver.exe hostname [optional: DNS Server]

Pode usar **nameresolver** para verificar os nomes de anfitrião que depende da sua aplicação. Dessa forma pode testar se tiver qualquer coisa incorrectamente configurado com o seu DNS ou talvez não tem acesso ao seu servidor DNS.

A ferramenta da próxima permite-lhe testar a conectividade de TCP a uma combinação de porta e anfitrião. Essa ferramenta é chamada **tcpping** e a sintaxe é:

    tcpping.exe hostname [optional: port]

O **tcpping** utilitário informa se, pode entrar num anfitrião específico e a porta. Esta operação só pode mostrar êxito se: existe uma aplicação em escuta na combinação de porta e anfitrião e não existe acesso à rede da sua aplicação para o anfitrião especificado e a porta.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>Depuração de acesso a VNet recursos alojados
Há uma série de coisas que pode impedir a sua aplicação de atingir um anfitrião específico e a porta. Na maioria das vezes ele é um dos três coisas:

* **Existe uma firewall da forma** se tiver uma firewall da forma, atingirá o tempo limite TCP. Que neste caso, é 21 segundos. Utilize o **tcpping** ferramenta para testar a conectividade. Tempos limite TCP pode dever-se muitas coisas para além dos firewalls, mas começar por aqui. 
* **DNS não está acessível** o DNS tempo-limite é de três segundos por servidor DNS. Se tiver dois servidores DNS, o tempo limite é de 6 segundos. Utilize nameresolver para ver se o DNS está a funcionar. Lembre-se de que não é possível utilizar o nslookup à medida que não utiliza o DNS a VNet está configurada com.
* **Intervalo de IP de P2S inválido** o ponto de intervalo IP do site tem de ser nos intervalos de IP privados do RFC 1918 (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255). Se o intervalo de utiliza IPs fora isso, as coisas não funcionam. 

Se esses itens não responder seu problema, a olhar para as coisas simples, como: 

* O Gateway mostra como sendo a cópia de segurança no portal?
* Certificados mostram como sendo sincronizados?
* Qualquer pessoa alterou a configuração de rede sem fazer uma "rede de sincronização" nos ASPs afetados? 

Se o gateway estiver em baixo, em seguida, colocá-lo de cópia de segurança. Se os certificados estão fora de sincronização, em seguida, ir para a vista ASP de sua integração de VNet e pressionar "Sincronizar rede". Se suspeitar que houve uma alteração efetuada à sua configuração de VNet e não era sincronização faria com sua ASPs, em seguida, ir para a vista ASP de sua integração de VNet e pressionar "Sincronizar rede" apenas como um lembrete, isso faz com que uma rápida interrupção com a ligação de VNet e as suas aplicações. 

Se tudo isso é tudo bem, em seguida, terá de aprofundar um pouco mais para o serviço:

* Existem todas as outras aplicações a utilizar a integração de VNet para aceder aos recursos na mesma VNet? 
* Pode ir para a consola de aplicação e utilizar tcpping para chegar a outros recursos na sua VNet? 

Se qualquer um dos acima for VERDADEIRO, em seguida, a integração de VNet estiver tudo bem e o problema é em algum lugar. Isso é onde ele obtém a ser mais um desafio porque não é possível simple para ver por que não é possível contactar uma anfitrião: porta. Algumas das causas incluem:

* tiver uma firewall cópia de segurança no seu anfitrião impedir o acesso a porta da aplicação a partir do seu ponto de intervalo IP do site. Cruzamento entre sub-redes, muitas vezes, requer o acesso público.
* o anfitrião de destino está inativo
* seu aplicativo está inativo
* teve o IP ou nome de anfitrião errado
* a aplicação está a escutar numa porta diferente que esperava. Pode verificar isso indo até nesse anfitrião e usando "netstat - aon" prompt do cmd. Isto mostra que está a escutar na porta que ID de processo. 
* os grupos de segurança de rede são configurados de forma que eles impedem o acesso ao seu anfitrião de aplicação e a porta do seu ponto de intervalo IP do site

Lembre-se de que não sabe quais IP no seu intervalo de IP de ponto a Site que a aplicação irá utilizar, por isso terá de autorizar o acesso o todo um intervalo. 

Os passos de depuração adicionais incluem:

* ligar a uma VM na sua VNet e tentam aceder a recursos anfitrião: porta a partir daí. Para testar para TCP acesso utilize o comando do PowerShell **test-netconnection**. A sintaxe é:

      test-netconnection hostname [optional: -Port]

* Trazer um aplicativo numa VM e de teste de acesso ao anfitrião e na porta partir da consola da sua aplicação

#### <a name="on-premises-resources"></a>Recursos no local ####

Se a sua aplicação não é possível chegar a um recurso no local, em seguida, verifique se pode aceder o recursos da sua VNet. Utilize o **test-netconnection** comando do PowerShell para fazer isso. Se a VM não é possível contactar o seu recurso no local, em seguida, certifique-se da que ligação de VPN de Site para Site está a funcionar. Se estiver a funcionar, verifique as mesmas coisas observadas anteriormente, bem como a configuração do gateway no local e o estado. 

Se a VNet alojada VM possa alcançar seu sistema de no local, mas a aplicação não é possível, em seguida, o motivo pelo qual se tratar de um dos seguintes procedimentos:

* as rotas não estão configuradas com o seu ponto de intervalos IP do site no seu gateway no local
* os grupos de segurança de rede estão a bloquear o acesso para o seu intervalo IP de ponto a Site
* as firewalls no local estão a bloquear o tráfego do seu intervalo de IP de ponto a Site
* tem um Route(UDR) de definidas pelo utilizador na sua VNet que impede a atingir a sua rede no local seu tráfego de ponto a Site com base em

## <a name="powershell-automation"></a>Automatização de PowerShell

Pode integrar o serviço de aplicações com uma rede Virtual do Azure com o PowerShell. Para obter um script de prontos para executar, veja [ligar uma aplicação no serviço de aplicações do Azure a uma rede Virtual do Azure](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3).

## <a name="hybrid-connections-and-app-service-environments"></a>As ligações híbridas e ambientes de serviço de aplicações
Existem três recursos que permitem o acesso aos recursos da VNet alojado. São:

* Integração de VNet
* Ligações Híbridas
* Ambientes do App Service

Ligações híbridas necessitam que instale um agente de retransmissão chamado Manager(HCM) de ligação híbrida na sua rede. O HCM tem de conseguir ligar-se para o Azure e também a sua aplicação. Esta solução é especialmente ótima de uma rede remota, como a sua rede no local ou até mesmo noutra cloud alojadas rede porque ele não requer um ponto de final de acesso de internet. O HCM só é executado no Windows e pode ter até cinco instâncias em execução para proporcionar elevada disponibilidade. Ligações híbridas só suporta TCP no entanto, e cada ponto de final do HC tem a correspondência com uma combinação de anfitrião: porta específica. 

A funcionalidade Ambiente de serviço de aplicações permite-lhe executar uma instância do serviço de aplicações do Azure na sua VNet. Isso permite que os seus recursos de acesso de aplicações na sua VNet sem quaisquer etapas extras. Algumas das outras vantagens de um ambiente de serviço de aplicações são que pode usar o Dv2 com base em funções de trabalho com até 14 GB de RAM. Outra vantagem é que pode dimensionar o sistema para atender às suas necessidades. Ao contrário dos ambientes de multi-inquilinos em que o ASP está limitado a 20 instâncias, num ASE pode dimensionar até 100 instâncias ASP. Uma das coisas que obtém com um ASE que não com a integração de VNet é que um ambiente de serviço de aplicações pode trabalhar com uma VPN do ExpressRoute. 

Embora haja que algumas utilizam a sobreposição de caso, nenhum desses recursos pode substituir qualquer um dos outros. Saber que o recurso a usar está associado às suas necessidades. Por exemplo:

* Se for um desenvolvedor e pretende executar um site no Azure e tê-lo a acessar o banco de dados na estação de trabalho em sua mesa, em seguida, a coisa mais fácil de usar é ligações híbridas. 
* Se for uma empresa de grande porte que deseja colocar um grande número de propriedades da web em público e geri-los na sua própria rede na cloud, em seguida, quer chegar com o ambiente de serviço de aplicações. 
* Se tiver várias aplicações que precisam de aceder a recursos na sua VNet, integração de VNet é a melhor opção. 

Se a VNet já está ligada à sua rede no local, em seguida, utilizar a integração de VNet ou um ambiente de serviço de aplicações é uma forma fácil de consumir recursos no local. Se a VNet não está ligada à sua rede no local, em seguida, é muito mais sobrecarga para configurar uma VPN de site a site com a sua VNet em comparação com a instalação de HCM. 

Além das diferenças funcionais, lá são também as diferenças de preços. A funcionalidade Ambiente de serviço de aplicações é uma Premium oferta de serviço oferece, mas rede mais possibilidades de configuração, além de outros excelentes recursos. Integração VNet pode ser utilizada com Standard ou Premium ASPs e é perfeita para o consumo de forma segura recursos na sua VNet a partir do serviço de aplicações de multi-inquilino. Ligações híbridas atualmente depende de uma conta que possua a preços níveis que comece gratuitamente e, em seguida, se tornar cada vez mais caro consoante a quantidade que precisa de BizTalk. Quando se trata de trabalhar em várias redes, no entanto, não há nenhum outro recurso, como as ligações híbridas, que podem permitir-lhe aceder a recursos em mais de 100 redes separadas. 

## <a name="new-vnet-integration"></a>Nova integração de VNet ##

Há uma nova versão da capacidade de integração de VNet não depende de tecnologia VPN ponto a Site. Esta nova versão está em pré-visualização. A nova capacidade de integração de VNet tem as seguintes características.

- O novo recurso exige uma sub-rede não utilizada na sua VNet do Resource Manager
- Um endereço é utilizado para cada instância de plano de serviço de aplicações. Uma vez que o tamanho de sub-rede não pode ser alterado após a atribuição, utilize uma sub-rede que mais do que pode cobrir o tamanho de dimensionamento máximo. / 27 com 32 endereços é o tamanho recomendado, pois o que seria acomodar um plano de serviço de aplicações é dimensionado para 20 instâncias.
- Pode consumir o ponto final de serviço a recursos protegidos pelo utilizando a nova capacidade de integração de VNet. Ativar o acesso a partir da sub-rede atribuída à aplicação configurar pontos finais de serviço com a sua aplicação
- Pode aceder a recursos através de ligações do ExpressRoute sem qualquer configuração adicional
- Nenhum gateway é necessário para utilizar a nova funcionalidade de integração de VNet
- Seu plano do serviço de aplicações tem de ser um plano Standard, Premium ou PremiumV2
- A nova capacidade só está disponível a partir de unidades de escala mais recente do serviço de aplicações do Azure. O portal informará se a sua aplicação pode utilizar a nova funcionalidade de integração de VNet. 
- A aplicação e a VNet tem de estar na mesma região

A nova funcionalidade de integração de VNet inicialmente só está disponível nas regiões Europa do Norte e E.U.A. Leste.


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-upgradeplan.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-existingvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-createvnet.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-appmanage.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanage.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanagedetail.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-vnetp2s.png

<!--Links-->
[VNETOverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: http://portal.azure.com/
[ASPricing]: http://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: http://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: http://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: http://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[ASEintro]: environment/intro.md
[ILBASE]: environment/create-ilb-ase.md
[V2VNETPortal]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md
[VPNERCoex]: ../expressroute/expressroute-howto-coexist-resource-manager.md
[ASE]: environment/intro.md
