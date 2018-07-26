---
title: Utilizar a WAN Virtual do Azure para criar uma ligação site a site ao Azure | Microsoft Docs
description: Neste tutorial, vai aprender a utilizar uma WAN Virtual do Azure para criar uma ligação VPN de site a site ao Azure.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 07/13/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect my local site to my VNets using Virtual WAN and I don't want to go through a Virtual WAN partner.
ms.openlocfilehash: ea36a3d4a2471cee6a18d70275aaf2e83ffc6f39
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/19/2018
ms.locfileid: "39159656"
---
# <a name="tutorial-create-a-site-to-site-connection-using-azure-virtual-wan-preview"></a>Tutorial: Criar uma ligação site a site com a WAN Virtual do Azure (Pré-visualização)

Este tutorial mostra-lhe como utilizar a WAN Virtual para se ligar aos seus recursos no Azure através de uma ligação VPN IPsec/IKE (IKEv2). Este tipo de ligação requer um dispositivo VPN localizado no local que tenha um endereço IP público com acesso exterior atribuído ao mesmo. Para obter mais informações sobre a WAN Virtual, veja [Virtual WAN Overview](virtual-wan-about.md) (Descrição Geral da WAN Virtual)

> [!NOTE]
> Se tiver muitos sites, teria de utilizar, normalmente, um [parceiro de WAN Virtual](https://aka.ms/virtualwan) para criar esta configuração. Contudo, pode criá-la sozinho se se sentir à vontade com o trabalho em rede e souber configurar o seu próprio dispositivo VPN.
>

![Diagrama da WAN Virtual](./media/virtual-wan-about/virtualwan.png)

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma WAN
> * Criar um site
> * Criar um hub
> * Ligar um hub a um site
> * Ligar uma VNet a um hub
> * Transferir e aplicar a configuração do dispositivo VPN
> * Ver a WAN Virtual
> * Ver o estados de funcionamento dos recursos
> * Monitorizar uma ligação

> [!IMPORTANT]
> A WAN Virtual do Azure está atualmente em pré-visualização pública gerida. Para utilizar a WAN Virtual, tem de [Inscrever-se na Pré-visualização](#enroll).
>
> Esta pré-visualização pública é disponibilizada sem um contrato de nível de serviço e não deve ser utilizada para cargas de trabalho de produção. Algumas funcionalidades podem não ser suportadas, podem ter capacidades restringidas ou podem não estar disponíveis em todas as localizações do Azure. Veja os [Termos Suplementares de Utilização para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obter mais informações.

## <a name="before-you-begin"></a>Antes de começar

Antes de iniciar a configuração, verifique se cumpre os seguintes critérios:

* Confirme que tem um dispositivo VPN baseado em rotas compatível que possa utilizar IKEv2 e alguém capaz de o configurar. Se estiver a trabalhar com um parceiro de WAN Virtual, as definições de configuração são criadas automaticamente e não precisa de saber como configurar o dispositivo à mão.
* Verifique se tem um endereço IP IPv4 público com acesso exterior para o seu dispositivo VPN. Este endereço IP não pode estar localizado atrás de um NAT.
* Se já tem uma rede virtual e quer ligar-se à mesma, confirme que nenhuma das sub-redes da sua rede no local se sobrepõe às redes virtuais às quais se quer ligar. A rede virtual não precisa de uma sub-rede de gateway e não pode ter nenhum gateway de rede virtual. Se não tiver uma rede virtual, pode utilizar os passos neste artigo para criar uma.
* Obtenha um intervalo de endereços IP para a região do seu hub. O hub é uma rede virtual e o intervalo de endereços que especificar para a região do mesmo não se pode sobrepor a nenhuma das redes virtuais já existentes a que se queira ligar. Também não se pode sobrepor aos intervalos de endereços a que se ligue no local. Se não estiver familiarizado com os intervalos de endereços IP localizados na configuração de rede no local, tem de se coordenar com alguém que consiga fornecer esses detalhes.
* Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="enroll"></a>1. Inscrever-se na Pré-visualização

Antes de poder configurar uma WAN Virtual, tem de inscrever primeiro a sua subscrição na Pré-visualização. Caso contrário, não poderá trabalhar com a WAN Virtual no portal. Para se inscrever, envie um e-mail para **azurevirtualwan@microsoft.com** com o ID da subscrição. Receberá um e-mail assim que a sua subscrição tiver sido inscrita.

## <a name="vnet"></a>2. Criar uma rede virtual

Se ainda não tiver uma VNet, pode criar uma rapidamente com o PowerShell. Também pode criar uma rede virtual com o portal do Azure.

* Verifique que o espaço de endereços da VNet que criar não se sobrepõe a nenhum dos intervalos de endereços de outras VNets às quais pretenda ligar nem a nenhum dos espaços de endereços da sua rede no local. 
* Se já tiver uma VNet, confirme que a mesma cumpre os critérios obrigatórios e que não tem um gateway de rede virtual.

Pode criar facilmente a VNet ao clicar em “Experimentar”, no artigo, para abrir uma consola do PowerShell. Ajuste os valores e copie e cole os comandos na janela da consola.

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

Ajuste os comandos do PowerShell e crie um grupo de recursos.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Criar uma VNet

Ajuste os comandos do PowerShell para criar a VNet que é compatível com o seu ambiente.

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzureRmVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```

## <a name="wan"></a>3. Criar uma WAN Virtual

1. Num browser, navegue para o [Portal do Azure](https://portal.azure.com) e inicie sessão com a sua conta do Azure.
2. Nesta fase, pode navegar para **Todos os serviços** e localizar a sua WAN Virtual. Em alternativa, também pode procurar a WAN Virtual na caixa de pesquisa na parte superior do portal do Azure. Clique em **WAN Virtual** para abrir a página.
3. Clique em **Criar** para abrir a página **Criar WAN**. Se a página não estiver disponível, significa que ainda não foi aprovado para esta Pré-visualização.

  ![Criar WAN](./media/virtual-wan-site-to-site-portal/createwan.png)
4. Na página Criar WAN, preencha os seguintes campos.

  * **Name** (Nome) - selecione o nome que pretende dar à WAN.
  * **Subscription** (subscrição) - selecione a subscrição que quer utilizar.
  * **Resource Group** (Grupo de Recursos) - crie um novo ou utilize um já existente.
  * **Resource Location** (Localização de Recurso) - escolha a localização de um recurso no menu pendente. Uma WAN é um recurso global e não reside numa região específica. Contudo, tem de selecionar uma região para poder gerir e localizar mais facilmente o recurso WAN que criou.
5. Clique em **Criar** para criar a WAN.

## <a name="site"></a>4. Criar um site

Crie tantos sites qantos necessários para corresponder às suas localizações físicas. Por exemplo, se tiver uma sucursal em Nova Iorque, uma em Londres e outra em Lisboa, tem de criar três sites separados. Esses sites contêm os pontos finais dos seus dispositivos VPN no local. Neste momento, só pode especificar um espaço de endereços privado para o site.

1. Navegue para **Todos os recursos**.
2. Clique na WAN virtual que criou.
3. Clique em **+Criar site**, na parte superior da página, para abrir a página **Criar site**.

  ![novo site](media/virtual-wan-site-to-site-portal/createsite.png)
4. Na página **Criar site**, preencha os seguintes campos:

  *  **Name** (Nome) - é o nome pelo qual se quer referir ao site no local.
  *  **Public IP address** (endereço IP público) - é o endereço IP público do dispositivo VPN que reside no site no local.
  *  **Private address space** (espaço de endereços privados) - é o espaço de endereços IP que está localizado no site no local. O tráfego destinado a este espaço de endereços é encaminhado para o site local.
  *  **Subscription** (Subscrição) - verifique a subscrição.
  *  **Resource Group** (Grupo de Recursos) - o grupo de recursos que quer utilizar.
5. Clique em **Mostrar avançadas** para ver mais definições. Pode **ativar o BGP** (campo opcional), que ativará esta funcionalidade em todas as ligações criadas para este site no Azure. Também pode introduzir **Informações do dispositivo** (campo opcional). Essas informações podem ajudar a Equipa do Azure a compreender melhor o seu ambiente de modo a acrescentar possibilidades de otimização adicionais no futuro ou a ajudá-lo a resolver problemas.

  ![BGP](media/virtual-wan-site-to-site-portal/sitebgp.png)
6. Clique em **Confirmar** para criar o site.
7. Repita estes passos para cada site que queira criar.

## <a name="hub"></a>5. Criar um hub e ligar os sites

1. Na página da WAN Virtual, clique em **Sites**.
2. Em **Sites associados**, vais ver uma lista de sites que ainda não foram ligados a um hub.
3. Selecione os sites que quer associar.
4. No menu pendente, selecione a região ao qual o hub vai ser associado. Deve associá-lo à região onde as VNets às quais quer ligar residem.
5. Clique em **Confirmar**. Se ainda não tiver um hub nessa região, é criado automaticamente uma VNet de hub virtual. Nesse caso, é apresentada a página **Criar hubs regionais**.
6. Na página **Criar hubs regionais**, introduza o intervalo de endereços da VNet do hub. Essa é a VNet que vai conter os serviços do hub. O intervalo que introduzir tem de ser um intervalo de endereços IP privados e não se pode sobrepor a nenhum espaço de endereços no local nem aos espaços de endereços da sua VNet. Será criado um ponto final da VPN subsequente na VPN do hub. (A criação automática do hub e do gateway só está disponível no portal.)
7. Clique em **Criar**.

## <a name="vnet"></a>6. Ligar a VNet a um hub

Neste passo, vai criar a ligação de peering entre o hub e uma VNet. Repita estes passos para cada VNet que queira ligar.

1. Na página da WAN virtual, clique em **Virtual network connection** (Ligação de rede virtual).
2. Na página de ligação da rede virtual, clique em **+Add connection** (+Adicionar ligação).
3. Na página **Add connection** (Adicionar ligação), preencha os seguintes campos:

    * **Connection name** (Nome da ligação) - dê um nome à ligação.
    * **Hubs** - selecione o hub que pretende associar a esta ligação.
    * **Subscription** (Subscrição) - verifique a subscrição.
    * **Virtual network** (Rede virtual) - selecione a rede virtual que pretende ligar a este hub. A rede virtual não pode ter um gateway de rede virtual já existente.

## <a name="device"></a>7. Transferir a configuração da VPN

Utilize a configuração do dispositivo VPN para configurar o seu dispositivo VPN no local.

1. Na página da WAN virtual, clique em **Overview** (Descrição geral).
2. Na parte superior da página Overview, clique em **Download VPN configuration** (Transferir configuração da VPN). O Azure cria uma conta de armazenamento no grupo de recursos “microsoft-network-[localização]”, em que “localização” é a localização da WAN. Depois de aplicar a configuração aos dispositivos VPN, pode eliminar esta conta de armazenamento.
3. Após a conclusão da criação do ficheiro, pode clicar na ligação para transferi-lo.
4. Aplique a configuração ao dispositivo VPN.

### <a name="understanding-the-vpn-device-configuration-file"></a>Compreender o ficheiro de configuração do dispositivo VPN

O ficheiro de configuração do dispositivo contém as definições que vão ser utilizadas para configurar o dispositivo VPN no local. Quando vir este ficheiro, repare nas informações seguintes:

* **vpnSiteConfiguration -** esta secção mostra os detalhes do dispositivo configurados como site que se vai ligar à WAN virtual. Inclui o nome e o endereço IP público do dispositivo da sucursal.
* **vpnSiteConnections -** esta secção disponibiliza informações sobre o seguinte:

    * **Espaço de endereços** da VNet do hub ou hubs virtuais.<br>Exemplo:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * **Espaço de endereços** das VNets que estão ligadas ao hub<br>Exemplo:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.30.0.0/16"]
         ```
    * **Endereços IP** do vpngateway do hub virtual. Uma vez que o vpngateway tem ambas as ligações compostas por dois túneis na configuração ativo-ativo, verá os dois endereços IP indicados neste ficheiro. Neste exemplo, vê "Instance0" e "Instance1" para cada site.<br>Exemplo:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * **Detalhes da configuração da ligação Vpngateway**, como BGP, chave pré-partilhada, etc. O PSK é a chave pré-partilhada que é gerada automaticamente para si. Pode sempre editar a ligação na página Overview (Descrição geral) de um PSK personalizado.
  
### <a name="example-device-configuration-file"></a>Exemplo de ficheiro de configuração de dispositivo

  ```
  { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"r403583d-9c82-4cb8-8570-1cbbcd9983b5"
      },
      "vpnSiteConfiguration":{ 
         "Name":"testsite1",
         "IPAddress":"73.239.3.208"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe",
               "ConnectedSubnets":[ 
                  "10.2.0.0/16",
                  "10.30.0.0/16"
               ]
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.186",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"bkOWe5dPPqkx0DfFE3tyuP7y3oYqAEbI",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"1f33f891-e1ab-42b8-8d8c-c024d337bcac"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite2",
         "IPAddress":"66.193.205.122"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"XzODPyAYQqFs4ai9WzrJour0qLzeg7Qg",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"cd1e4a23-96bd-43a9-93b5-b51c2a945c7"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite3",
         "IPAddress":"182.71.123.228"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"YLkSdSYd4wjjEThR3aIxaXaqNdxUwSo9",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   }
  ```

### <a name="configuring-your-vpn-device"></a>Configurar o dispositivo VPN

>[!NOTE]
> Se estiver a trabalhar com uma solução de um parceiro de WAN Virtual, a configuração do dispositivo VPN acontece de forma automática, na qual o controlador do dispositivo obtém o ficheiro de configuração a partir do Azure e o aplica ao dispositivo para configurar a ligação ao Azure. Isto significa que não é preciso saber como configurar o dispositivo VPN manualmente.
>

Se precisar de instruções para configurar o dispositivo, pode utilizar as instruções na [página VPN device configuration scripts](~/articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#configscripts) (Scripts de configuração de dispositivos VPN), tendo em conta os seguintes avisos:

* As instruções a página de dispositivos VPN não foram escritas para a WAN Virtual, mas pode utilizar os valores desta a partir do ficheiro de configuração para configurar o seu dispositivo VPN manualmente. 
* Os scripts de configuração do dispositivo transferíveis que se destinam ao Gateway de VPN não funcionam para a WAN Virtual, uma vez que a configuração é diferente.
* A WAN Virtual só pode utilizar IKEv2, não IKEv1.
* A WAN Virtual só pode utilizar dispositivos VPN baseados em rotas e instruções do dispositivo.

## <a name="viewwan"></a>8. Ver a WAN Virtual

1. Navegue para a WAN virtual.
2. Na página Overview, cada ponto no mapa representa um hub. Coloque o cursor sobre um ponto para ver o resumo do estado de funcionamento do hub.
3. Na secção Hubs e ligações, pode ver o estado do hub, o site, a região, o estado da ligação VPN e os bytes de entrada e saída.

## <a name="viewhealth"></a>9. Ver o estado de funcionamento do seu recurso

1. Navegue para a WAN.
2. Na página WAN, na secção **SUPPORT + Troubleshooting** (SUPORTE + Resolução de Problemas), clique em **Health** (Estado de funcionamento) e veja o recurso.

## <a name="connectmon"></a>10. Monitorizar uma ligação

Crie uma ligação para monitorizar a comunicação entre uma VM do Azure e um site remoto. Para obter informações sobre como configurar um monitor de ligação, veja [Monitorizar a comunicação de rede](~/articles/network-watcher/connection-monitor.md). O campo de origem é o IP da VM no Azure e o IP de destino é o IP do site.

## <a name="cleanup"></a>11. Limpar recursos

Quando já não precisar desses recursos, pode utilizar [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos e todos os recursos que o mesmo contém. Substitua "myResourceGroup" pelo nome do grupo de recursos e execute o seguinte comando do PowerShell:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="feedback"></a>Comentários sobre a pré-visualização

Agradecemos os seus comentários. Envie um e-mail para <azurevirtualwan@microsoft.com> para reportar quaisquer problemas ou para faça comentários (positivos ou negativos) sobre a WAN Virtual. Inclua o nome da sua empresa em "[ ]" na linha de assunto. Inclua também o ID da subscrição se estiver a reportar um problema.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma WAN
> * Criar um site
> * Criar um hub
> * Ligar um hub a um site
> * Ligar uma VNet a um hub
> * Transferir e aplicar a configuração do dispositivo VPN
> * Ver a WAN Virtual
> * Ver o estados de funcionamento dos recursos
> * Monitorizar uma ligação

Para saber mais sobre a WAN Virtual, veja a página [Virtual WAN Overview](virtual-wan-about.md) (Descrição Geral da WAN Virtual).
