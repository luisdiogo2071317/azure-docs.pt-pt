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
ms.date: 05/25/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 72c5c4b0f0ab752bb02e6bee7cd038afca44ee1b
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605200"
---
# <a name="tutorial-configure-hybrid-cloud-connectivity-with-azure-and-azure-stack"></a>Tutorial: configurar a conectividade de nuvem híbrida com o Azure e pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode aceder a recursos com segurança no global Azure e pilha do Azure utilizando o padrão de conectividade híbrida.

Neste tutorial, irá criar um ambiente de exemplo para:

> [!div class="checklist"]
> - Manter os dados no local para satisfazer os requisitos de regulamentação ou de privacidade, mas tem acesso a recursos do Azure global.
> - Ao utilizar a implementação da aplicação ampliada de nuvem e de recursos no global Azure de manter um sistema de legado.

## <a name="prerequisites"></a>Pré-requisitos

Alguns componentes são necessários para criar uma implementação de conectividade híbrida. Alguns destes componentes irão demorar tempo para preparar, pelo que terá de planear em conformidade.

**Azure Stack**

Um parceiro de OEM/Hardware do Azure, pode implementar uma pilha de Azure de produção e todos os utilizadores podem implementar um Kit de desenvolvimento de pilha do Azure (ASDK).

**Componentes de pilha do Azure**

Um operador de pilha do Azure deve implementar o serviço de aplicações, criar planos e ofertas, criar uma subscrição de inquilino e adicionar a imagem do Windows Server 2016. Se já tiver alguns destes componentes, certifique-se de que as mesmas cumprem os requisitos antes de começar este tutorial.

Este tutorial parte do princípio de que tiver algum conhecimento básico do Azure e pilha do Azure. Para obter mais informações antes de iniciar o tutorial, leia os artigos seguintes:

 - [Introdução ao Azure](https://azure.microsoft.com/overview/what-is-azure/)
 - [Conceitos chave de pilha do Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-key-features)

### <a name="azure"></a>Azure

 - Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.
 - Criar um [aplicação Web](https://docs.microsoft.com/vsts/build-release/apps/cd/azure/aspnet-core-to-azure-webapp?view=vsts&tabs=vsts#create-an-azure-web-app-using-the-portal) no Azure. Anote o URL da aplicação Web, porque terá no tutorial.

### <a name="azure-stack"></a>Azure Stack

 - Utilize a pilha do Azure de produção ou implementar o Azure pilha Development Kit do https://github.com/mattmcspirit/azurestack/blob/master/deployment/ConfigASDK.ps1.
   >[!Note]
   >Implementar o ASDK pode demorar até 7 horas, por isso planeie em conformidade.

 - Implementar [do serviço de aplicações](https://docs.microsoft.com/azure/azure-stack/azure-stack-app-service-deploy) PaaS serviços à pilha do Azure.
 - [Criar planos e ofertas](https://docs.microsoft.com/azure/azure-stack/azure-stack-plan-offer-quota-overview) no ambiente de pilha do Azure.
 - [Criar subscrição de inquilino](https://docs.microsoft.com/azure/azure-stack/azure-stack-subscribe-plan-provision-vm) no ambiente de pilha do Azure.

### <a name="before-you-begin"></a>Antes de começar

Certifique-se de que cumpre os seguintes critérios antes de começar a configurar a conectividade de nuvem híbrida:

 - Precisa de um endereço IPv4 público com acesso exterior para o dispositivo VPN. Este endereço IP não pode estar localizado atrás de um NAT.
 - Todos os recursos são implementados na mesma região/localização.

#### <a name="tutorial-example-values"></a>Valores de exemplo do tutorial

Os exemplos neste tutorial utilizam os seguintes valores. Pode utilizar estes valores para criar um ambiente de teste ou consultá-las para uma melhor compreensão dos exemplos. Para obter mais informações sobre as definições do Gateway de VPN em geral, veja [Sobre as Definições do Gateway de VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings).

Especificações da ligação:

 - **Tipo de VPN**: baseadas na rota
 - **Tipo de ligação**: Site a site (IPsec)
 - **Tipo de gateway**: VPN
 - **Nome da ligação do Azure**: Azure-Gateway-AzureStack-S2SGateway (o portal irá preenchimento automático este valor)
 - **Nome de ligação de pilha do Azure**: AzureStack-Gateway-Azure-S2SGateway (o portal irá preenchimento automático este valor)
 - **Chave partilhada**: quaisquer compatível com o hardware VPN, com valores em ambos os lados da ligação correspondentes
 - **Subscrição**: preferencial qualquer subscrição
 - **Grupo de recursos**: teste infra-estrutura

Endereços IP de rede e sub-rede:

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

Utilize os seguintes passos para criar uma rede virtual através do portal. Pode utilizar estes [valores de exemplo](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#values) se estiver a utilizar este artigo como um tutorial. No entanto, se estiver a utilizar este artigo para configurar um ambiente de produção, substitua as definições de exemplo com os seus próprios valores.

> [!IMPORTANT]
> Tem de se certificar de que não é uma sobreposição de endereços IP no Azure ou do Azure pilha espaços de endereços vNet.

Para criar uma vNet no Azure:

1. Utilizar o browser para estabelecer ligação com o [portal do Azure](http://portal.azure.com/) e inicie sessão com a sua conta do Azure.
2. Selecione **crie um recurso**. No **procurar no marketplace** campo, introduza `virtual network`'. Localizar **rede Virtual** na lista de resultados e, em seguida, selecione **rede Virtual**.
3. Do **selecionar um modelo de implementação** lista, selecione **Resource Manager**e, em seguida, selecione **criar**.
4. No **criar rede virtual**, configure as definições da VNet. Os nomes de campos necessários são o prefixo com um asterisco vermelho.  Ao introduzir um valor válido, o asterisco é alterado para uma marca de verificação verde.

Para criar uma vNet na pilha do Azure:

* Repita os passos anteriores (1-4) a utilizar a pilha de Azure **portal inquilino**.

## <a name="add-a-gateway-subnet"></a>Adicionar uma sub-rede do gateway

Antes de ligar a rede virtual para um gateway, tem de criar a sub-rede do gateway da rede virtual que pretende ligar. Os serviços de gateway utilizam os endereços IP que especificou na sub-rede do gateway.

No [portal do Azure](http://portal.azure.com/), navegue para a rede virtual do Gestor de recursos onde pretende criar um gateway de rede virtual.

1. Selecione a vNet para abrir o **rede Virtual** página.
2. No **definições**, selecione **sub-redes**.
3. No **sub-redes** página, selecione **+ sub-rede do Gateway** para abrir o **adicionar sub-rede** página.

    ![Adicionar sub-rede do gateway](media/azure-stack-solution-hybrid-connectivity/image4.png)

4. O **nome** para a sub-rede é preenchida automaticamente com o valor "GatewaySubnet". Este valor é necessário para o Azure reconhecer a sub-rede como a sub-rede do gateway.
5. Alterar o **intervalo de endereços** valores que são fornecidos para corresponder aos seus requisitos de configuração e, em seguida, selecione **OK**.

## <a name="create-a-virtual-network-gateway-in-azure-and-azure-stack"></a>Criar um Gateway de rede Virtual no Azure e Azure pilha

Utilize os seguintes passos para criar um gateway de rede virtual no Azure.

1. No lado esquerdo da página de portal, selecione **+** e introduza o gateway de rede virtual no campo de pesquisa.
2. No **resultados**, selecione **gateway de rede Virtual**.
3. No **gateway de rede Virtual**, selecione **criar** para abrir o **criar gateway de rede virtual** página.
4. No **criar gateway de rede virtual**, especifique os valores para o gateway de rede, conforme mostrado no **valores de exemplo Tutorial**e os seguintes valores adicionais:

    - **SKU**: básico
    - **Rede virtual**: selecione a rede Virtual que criou anteriormente. A sub-rede do gateway que criou é selecionada automaticamente.
    - **Configuração de IP primeiro**: Este é o IP público do seu Gateway.
        - Selecione **configuração de IP do gateway criar**, que demora ao **escolher endereço IP público** página.
        - Selecione **+ criar nova** para abrir o **Criar endereço IP público** página.
        - Introduza um **nome** para o seu endereço IP público. Deixe o SKU como **básico**e, em seguida, selecione **OK** para guardar as alterações.

       > [!Note]
       > Atualmente, o Gateway de VPN só suporta atribuição de endereços IP públicos dinâmicos. No entanto, isto não significa que o endereço IP é alterado depois de ser atribuído ao gateway de VPN. O endereço IP Público só é alterado quando o gateway é eliminado e recriado. Redimensionar, ao repor ou outra manutenção/atualização interna para o gateway de VPN não alterar o endereço IP.

4. Verifique as definições do gateway.
5. Selecione **criar** para criar o gateway VPN. As definições de gateway são validadas e o mosaico "Gateway de rede Virtual da implementação" é apresentado no dashboard.

   >[!Note]
   >A criação de um gateway pode demorar até 45 minutos. Poderá ter de atualizar a página do portal para ver o estado concluído.

    Depois de criado o gateway, pode ver o endereço IP atribuído ao observar a rede virtual no portal. O gateway aparece como um dispositivo ligado. Para obter mais informações sobre o gateway, selecione o dispositivo.

6. Repita os passos anteriores (1-5) na sua implementação de pilha do Azure.

## <a name="create-the-local-network-gateway-in-azure-and-azure-stack"></a>Criar o gateway de rede local no Azure e pilha do Azure

O gateway de rede local refere-se normalmente à sua localização no local. O site, dar um nome que o Azure ou a pilha do Azure pode referir-se a e, em seguida, especifique:

- O endereço IP do dispositivo VPN no local que está a criar uma ligação para.
- Os prefixos de endereço IP que serão encaminhados através do gateway VPN no dispositivo VPN. Os prefixos do endereço que especificar são os que estão localizados na sua rede no local.

  >[!Note]
  >Se as alterações de rede no local ou precisar de alterar o endereço IP público do dispositivo VPN, pode atualizar facilmente estes valores mais tarde.

1. No portal, selecione **+ criar um recurso**.
2. Na caixa de pesquisa, introduza **gateway de rede Local**, em seguida, selecione **Enter** para procurar. Isto irá devolver uma lista de resultados.
3. Selecione **gateway de rede Local**, em seguida, selecione **criar** para abrir o **criar gateway de rede local** página.
4. No **criar gateway de rede local**, especifique os valores para o gateway de rede local, utilizando a nossa **valores de exemplo Tutorial**. Inclua os seguintes valores adicionais.

    - **Endereço IP**: Este é o endereço IP público do dispositivo VPN que pretende que o Azure ou a pilha do Azure ao ligar. Especifique um endereço IP público válido que não está protegido por um NAT para que o Azure pode alcançar o endereço. Se não tiver o endereço IP neste momento, pode utilizar um valor de exemplo como um marcador de posição, mas terá de voltar atrás e substitua o marcador de posição com o endereço IP público do seu dispositivo VPN. Azure não consegue ligar ao dispositivo até que forneça um endereço válido.
    - **Espaço de endereços**: Este é o intervalo de endereços da rede que representa esta rede local. Pode adicionar vários intervalos de espaço de endereços. Certifique-se de que os intervalos que especificou não se sobrepõem intervalos outras redes que pretende ligar. O Azure irá encaminhar o intervalo de endereços que especificou no endereço IP do dispositivo VPN no local. Utilize os seus próprios valores, se pretender ligar ao seu site no local, não é um valor de exemplo.
    - **Configurar definições de BGP**: Utilize apenas quando configurar o BGP. Caso contrário, não selecione esta opção.
    - **Subscrição**: Certifique-se de que a subscrição correta está visível.
    - **Grupo de recursos**: selecione o grupo de recursos que pretende utilizar. Pode criar um novo grupo de recursos ou selecione uma que já criou.
    - **Localização**: selecione a localização que este objeto será criado. Poderá pretender selecionar a mesma localização que a VNet reside num, mas não estiver a necessário fazê-lo.
5. Quando terminar de especificar os valores necessários, selecione **criar** para criar o gateway de rede local.
6. Repita estes passos (1-5) na sua implementação de pilha do Azure.

## <a name="configure-your-connection"></a>Configurar a ligação

As ligações de Site a Site para uma rede no local requerem um dispositivo VPN. O dispositivo VPN que configurou é referido como uma ligação. Para configurar a ligação, tem de:

- Uma chave partilhada. Esta é a mesma chave partilhada que especifica ao criar a ligação VPN de Site a Site. Nos nossos exemplos, iremos utilizar uma chave partilhada básica. Deve gerar uma chave mais complexa para utilizar.
- O endereço IP Público do gateway de rede virtual. Pode ver o endereço IP público através do portal do Azure, do PowerShell ou da CLI. Para localizar o endereço IP público do seu gateway VPN no portal do Azure, navegue para gateways de rede Virtual, em seguida, selecione o nome do seu gateway.

Utilize os seguintes passos para criar uma ligação de VPN de Site a Site entre o gateway de rede virtual e o dispositivo VPN no local.

1. No portal do Azure, selecione **+ criar um recurso**.
2. Procurar **ligações**.
3. No **resultados**, selecione **ligações**.
4. No **ligação**, selecione **criar**.
5. No **criar ligação**, configure as seguintes definições:

    - **Tipo de ligação**: selecione o Site para site (IPSec).
    - **Grupo de recursos**: selecione o grupo de recursos de teste.
    - **Gateway de rede virtual**: selecione o gateway de rede Virtual que criou.
    - **Gateway de rede local**: selecione o Gateway de rede Local que criou.
    - **Nome da ligação**: Este é preenchido automaticamente com os valores dos dois gateways.
    - **Chave partilhada**: este valor tem de corresponder ao valor que está a utilizar para o dispositivo VPN local no local. O exemplo tutorial utiliza 'abc123', mas pode (e deve) utilizar algo mais complexo. O mais importante é que este valor tem de ser o mesmo valor que especificar quando configurar o seu dispositivo VPN.
    - Os valores para **subscrição**, **grupo de recursos**, e **localização** sejam corrigidos.

6. Selecione **OK** para criar a ligação.

Pode ver a ligação no **ligações** página do gateway de rede virtual. Serão enviados para o estado de *desconhecido* para *ligação*e, em seguida, *com êxito*.

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre os padrões de nuvem do Azure, consulte [padrões de conceção de nuvem](https://docs.microsoft.com/azure/architecture/patterns).
