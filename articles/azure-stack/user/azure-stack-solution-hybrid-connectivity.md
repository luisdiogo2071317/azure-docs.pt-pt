---
title: Configurar a conectividade de nuvem híbrida com o Azure e Azure pilha | Microsoft Docs
description: Saiba como configurar a conectividade de nuvem híbrida com o Azure e pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 048e2636aabe406728c8fe1b93ef861f13346256
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="tutorial-configure-hybrid-cloud-connectivity-with-azure-and-azure-stack"></a>Tutorial: configurar a conectividade de nuvem híbrida com o Azure e pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode aceder a recursos com segurança no global Azure e pilha do Azure utilizando o padrão de conectividade híbrida. 

Neste tutorial, irá criar um ambiente de exemplo para:

> [!div class="checklist"]
> - Manter os dados no local para privacidade ou requisitos de regulamentação, mas tem acesso a recursos do Azure global.
> - Ao utilizar a implementação da aplicação ampliada de nuvem e de recursos no global Azure de manter um sistema de legado.

## <a name="prerequisites"></a>Pré-requisitos

Alguns componentes são necessários para criar uma implementação de conectividade de híbrida e podem demorar algum tempo para preparar. 

Um parceiro de OEM/Hardware do Azure, pode implementar uma pilha de Azure de produção e todos os utilizadores podem implementar uma ASDK. Um operador de pilha do Azure deve também implementar o serviço de aplicações, criar planos e ofertas, criar uma subscrição de inquilino e adicionar a imagem do Windows Server 2016. 

Se já tiver alguns destes componentes, certifique-se de que cumprem os requisitos antes de começar.

Este tópico também parte do princípio que tiver algum conhecimento do Azure e a pilha do Azure. Se pretender obter mais informações antes de continuar, é necessário começar com estes tópicos:
 - [Introdução ao Azure](https://azure.microsoft.com/overview/what-is-azure/)
 - [Conceitos chave de pilha do Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure"></a>Azure
 - Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.
 - Criar um [aplicação Web](https://docs.microsoft.com/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?view=vsts&tabs=vsts#create-an-azure-web-app-using-the-portal) no Azure. Anote o URL da aplicação Web novas, porque é utilizado mais tarde.

### <a name="azure-stack"></a>Azure Stack
 - Utilize a pilha do Azure de produção ou Kit de desenvolvimento de pilha do Azure implementar hiperligações em abaixo:
    - https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1 
    - A instalação normalmente demora algumas horas a concluir, por isso planeie em conformidade.
 - Implementar [do serviço de aplicações](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) PaaS serviços à pilha do Azure. 
 - [Criar plano/ofertas](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) no ambiente de pilha do Azure. 
 - [Criar subscrição de inquilino](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) no ambiente de pilha do Azure. 


### <a name="before-you-begin"></a>Antes de começar

Antes de iniciar a configuração, verifique se cumpre os seguintes critérios:

 - Verifique se tem um endereço IP IPv4 público com acesso exterior para o seu dispositivo VPN. Este endereço IP não pode estar localizado atrás de um NAT.
 - Certifique-se de que todos os recursos são implementados na mesma região/localização.

#### <a name="example-values"></a>Valores de exemplo

Os exemplos neste artigo utilizam os seguintes valores. Pode utilizar estes valores para criar um ambiente de teste ou consultá-las para compreender melhor os exemplos neste artigo. Para obter mais informações sobre as definições do Gateway de VPN em geral, veja [Sobre as Definições do Gateway de VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings).

Especificações da ligação:
 - **Tipo de VPN**: baseadas na rota
 - **Tipo de ligação**: Site a site (IPsec)
 - **Tipo de gateway**: VPN
 - **Nome da ligação do Azure**: Azure-Gateway-AzureStack-S2SGateway (o portal irá preenchimento automático este valor)
 - **Nome de ligação de pilha do Azure**: AzureStack-Gateway-Azure-S2SGateway (o portal irá preenchimento automático este valor)
 - **Chave partilhada**: quaisquer compatível com o hardware VPN, com valores em ambos os lados da ligação correspondentes
 - **Subscrição**: preferencial qualquer subscrição
 - **Grupo de recursos**: teste infra-estrutura

| Ligação de pilha do Azure/Azure | Nome | Subrede | Endereço IP |
|-------------------------------------|---------------------------------------------|---------------------------------------|-----------------------------|
| VNet do Azure | ApplicationvNet<br>10.100.102.9/23 | ApplicationSubnet<br>10.100.102.0/24 |  |
|  |  | GatewaySubnet<br>10.100.103.0/24 |  |
| VNet de pilha do Azure | ApplicationvNet<br>10.100.100.0/23 | ApplicationSubnet <br>10.100.100.0/24 |  |
|  |  | GatewaySubnet <br>10.100101.0/24 |  |
| Gateway de rede Virtual do Azure | Gateway do Azure |  |  |
| Gateway de rede Virtual de pilha do Azure | Gateway de AzureStack |  |  |
| IP público do Azure | GatewayPublicIP do Azure |  | Determinar durante a criação do |
| IP público de pilha do Azure | AzureStack GatewayPublicIP |  | Determinar durante a criação do |
| Gateway de rede Local do Azure | AzureStack S2SGateway<br>   10.100.100.0/23 |  | Valor IP público de pilha do Azure |
| Gateway de rede Local de pilha do Azure | S2SGateway do Azure<br>10.100.102.0/23 |  | Valor IP público do Azure |

## <a name="create-a-virtual-network-in-global-azure-and-azure-stack"></a>Criar uma rede virtual no global do Azure e pilha do Azure

> [!Note]  
> Tem de se certificar de que não há nenhuma sobreposição de IPs no Azure ou do Azure pilha espaços de endereços vNet. 

Para criar uma vNet no modelo de implementação Resource Manager através do portal do Azure. Use os [valores de exemplo](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#values) se estiver a seguir estes passos como um tutorial. Se não estiver a seguir estes passos como um tutorial, certifique-se de substitui os valores pelos seus próprios. 

1. Num browser, navegue para o [Portal do Azure](http://portal.azure.com/) e inicie sessão com a sua conta do Azure.
2. Clique em **Criar um recurso**. No **procurar no marketplace** campo, introduza `virtual network`'. Localizar **rede Virtual** da lista devolvida e abra o **rede Virtual** página.
3. Perto da parte inferior da página de rede Virtual, do **selecionar um modelo de implementação** lista, selecione **Resource Manager**e, em seguida, selecione **criar**. É aberta a página "Criar rede virtual".
4. Na página **Criar rede virtual**, configure as definições da VNet. Depois de preencher os campos, o sinal de exclamação vermelho muda para uma marca de verificação verde se os caracteres introduzidos nos campos forem válidos.
5. Repita estes passos do **portal inquilino** da pilha do Azure.

## <a name="add-a-gateway-subnet"></a>Adicionar uma sub-rede do gateway

Antes de ligar a rede virtual a um gateway, primeiro tem de criar a sub-rede do gateway para a rede virtual à qual pretende ligar. Os serviços do gateway utilizam os endereços IP especificados na sub-rede de gateway.

No [portal](http://portal.azure.com/), navegue para a rede virtual do Gestor de Recursos para a qual pretende criar um gateway de rede virtual.

1. No **definições** secção da sua página de VNet, selecione **sub-redes** para expandir o **sub-redes** página.
2. No **sub-redes** página, selecione **+ sub-rede do Gateway** para abrir o **adicionar sub-rede** página.

    ![](media/azure-stack-solution-hybrid-connectivity/image4.png)

3. O **Nome** da sub-rede é preenchido automaticamente com o valor "GatewaySubnet". Este valor é necessário para o Azure reconhecer a sub-rede como a sub-rede do gateway. Ajustar o preenchimento automático **intervalo de endereços** valores para corresponder aos seus requisitos de configuração, em seguida, selecione **OK** na parte inferior da página para criar a sub-rede.

## <a name="create-a-virtual-network-gateway-in-azure-and-azure-stack"></a>Criar um Gateway de rede Virtual no Azure e Azure pilha

1. No lado esquerdo da página de portal, selecione + e introduza 'Gateway de rede Virtual' na pesquisa. No **resultados**, localize e selecione **gateway de rede Virtual**.
2. Na parte inferior do **gateway de rede Virtual** página, selecione **criar**. É aberta a página **Criar gateway de rede virtual**.
3. No **criar gateway de rede virtual** página, especifique os valores para o gateway de rede virtual, conforme detalhado nos valores de exemplo e os valores adicionais detalhados abaixo.
    - **SKU**: básico
    - **Rede virtual**: selecione a rede Virtual que criou anteriormente. Selecione automaticamente a sub-rede do gateway que criou anteriormente. 
    - **Configuração de IP primeiro**: Este é o IP público do seu Gateway. 
        - Clique em de **configuração de IP do gateway criar** e Isto leva-o para o **escolher endereço IP público** página.
        - Clique em **+ criar nova** para abrir o **Criar endereço IP público** página.
        - Introduza um **nome** para o seu endereço IP público. Deixe o SKU como **básico**, em seguida, selecione **OK** na parte inferior desta página para guardar as alterações.

    > [!Note]  
    > Gateway de VPN atualmente apenas suporta a atribuição de endereços IP públicos dinâmicos. No entanto, isto não significa que o endereço IP é alterado após ser atribuído ao gateway de VPN. O endereço IP Público só é alterado quando o gateway é eliminado e recriado. Não é alterado ao redimensionar, repor ou ao realizar qualquer outra manutenção/atualização interna do gateway de VPN.

4. Verifique as definições. 
5. Clique em **Criar** para começar a criar o gateway de VPN. As definições são validadas e verá o mosaico "A implementar gateway de rede Virtual" no dashboard. A criação de um gateway pode demorar até 45 minutos. Poderá ter de atualizar a página do portal para ver o estado concluído.

    Uma vez criado o gateway, visualize o endereço IP que lhe foi atribuído vendo a rede virtual no portal. O gateway aparece como um dispositivo ligado. Pode selecionar o dispositivo ligado (o gateway de rede virtual) para visualizar mais informações.
6. Repita estes passos da sua implementação de pilha do Azure.

## <a name="create-the-local-network-gateway-in-azure-and-azure-stack"></a>Criar o gateway de rede local no Azure e pilha do Azure

O gateway de rede local refere-se normalmente à sua localização no local. O site, dar um nome pelo qual Azure ou a pilha do Azure pode fazem referência ao mesmo, em seguida, especifique o endereço IP do dispositivo VPN no local para os quais irá criar uma ligação. Também pode especificar os prefixos do endereço IP que vai ser encaminhado através do gateway de VPN para o dispositivo VPN. Os prefixos do endereço que especificar são os que estão localizados na sua rede no local. Se a sua rede no local for alterada ou se precisar de alterar o endereço IP público para o dispositivo VPN, pode atualizar facilmente os valores mais tarde.

1. No portal, selecione **+ criar um recurso**.
2. Na caixa de pesquisa, introduza **gateway de rede Local**, em seguida, prima **Enter** para procurar. Isto irá devolver uma lista de resultados. Clique em **gateway de rede Local**, em seguida, selecione o **criar** botão para abrir o **criar gateway de rede local** página.
3. No **página de criação de gateway de rede local**, especifique os valores para o gateway de rede local, conforme detalhado no nossos valores de exemplo e os valores adicionais detalhados abaixo.
    - **Endereço IP**: Este é o endereço IP público do dispositivo VPN que pretende que o Azure ou a pilha do Azure ao ligar. Especifique um endereço IP público válido. O endereço IP não pode estar protegido por NAT e tem de estar acessível ao Azure. Se não tiver o endereço IP no momento, pode usar os valores mostrados no exemplo, mas precisará voltar atrás e substituir o endereço IP do marcador de posição pelo endereço IP público do seu dispositivo VPN. Caso contrário, o Azure não será capaz de se ligar.
    - O **Espaço de endereços** refere-se aos intervalos de endereços da rede que esta rede local representa. Pode adicionar vários intervalos de espaço de endereços. Certifique-se de que os intervalos que especificar aqui não se sobrepõem aos intervalos de outras redes às quais pretende ligar. O Azure irá encaminhar o intervalo de endereços que especificou no endereço IP do dispositivo VPN no local. Utilize os seus próprios valores, se pretender ligar ao seu site no local, não os valores mostrados no exemplo.
    - **Configurar definições de BGP**: Utilize apenas quando configurar o BGP. Caso contrário, não selecione esta opção.
    - **Subscrição**: Certifique-se de que a subscrição correta está visível.
    - **Grupo de recursos**: selecione o grupo de recursos que pretende utilizar. Pode criar um novo grupo de recursos ou selecionar um que já tenha criado.
    - **Localização**: selecione a localização que este objeto será criado. Poderá selecionar a mesma localização em que a VNet se encontra, mas não tem obrigatoriamente de o fazer.
4. Quando tiver terminado de especificar os valores, selecione o **criar** na parte inferior da página para criar o gateway de rede local.
5. Repita estes passos da sua implementação de pilha do Azure.

## <a name="configure-your-connection"></a>Configurar a ligação

As ligações de Site a Site para uma rede no local requerem um dispositivo VPN. Neste passo, configure o seu dispositivo VPN, conhecido como uma ligação. Quando configurar a ligação, precisa do seguinte:
    - Uma chave partilhada. Esta é a mesma chave partilhada que especifica ao criar a ligação VPN de Site a Site. Nos nossos exemplos, iremos utilizar uma chave partilhada básica. Deve gerar uma chave mais complexa para utilizar.
    - O endereço IP Público do gateway de rede virtual. Pode ver o endereço IP público através do portal do Azure, do PowerShell ou da CLI. Para localizar o endereço IP público do seu gateway VPN no portal do Azure, navegue para gateways de rede Virtual, em seguida, selecione o nome do seu gateway.
Crie a ligação de Rede de VPNs entre o gateway de rede virtual e o dispositivo VPN no local.
1. No portal, selecione **+ criar um recurso**.
2. Na caixa de pesquisa, introduza **ligações**, em seguida, prima **Enter** para procurar. Isto irá devolver uma lista de resultados. Clique em **ligações**, em seguida, selecione o botão para criar para abrir o **criar ligações** página.
3. No **criar ligações** página, configure os valores para a sua ligação.
     - Noções básicas:
        1. **Tipo de ligação**: selecione o Site para site (IPSec).
        2. **Grupo de recursos**: (selecione o grupo de recursos de teste)
     - Definições:
        1. **Gateway de rede virtual**: selecione o gateway de rede Virtual que criou anteriormente.
        2. **Gateway de rede local**: selecione o Gateway de rede Local que criou anteriormente. 
        3. **Nome da ligação**: Isto será automaticamente preenchido com os valores dos dois gateways. 
        4. **Chave partilhada**: o valor aqui tem de corresponder ao valor que está a utilizar para o dispositivo VPN local no local. O exemplo utiliza "abc123", mas pode e (deve) utilizar algo mais complexo. O importante é que o valor que especificar aqui seja igual ao valor especificado ao configurar o dispositivo VPN.
    - Os restantes valores para **Subscrição**, **Grupo de Recursos** e **Localização** são fixos.
4. Clique em **OK** para criar a ligação. Verá criar ligação flash no ecrã.
5. Pode ver a ligação no * * página de ligações do gateway de rede virtual. Serão enviados para o estado de **desconhecido** para **ligação**e, em seguida, **com êxito**.

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre os padrões de nuvem do Azure, consulte [padrões de conceção de nuvem](https://docs.microsoft.com/azure/architecture/patterns).
