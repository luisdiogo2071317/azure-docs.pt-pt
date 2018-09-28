---
title: Filtrar tráfego de entrada com DNAT do Azure Firewall com o portal do Azure
description: Neste tutorial, vai aprender a implementar e configurar a DNAT do Azure Firewall com o portal do Azure.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 766ad04251fbe404d43734115e41e23ae0a4be28
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982055"
---
# <a name="tutorial-filter-inbound-traffic-with-azure-firewall-dnat-using-the-azure-portal"></a>Tutorial: Filtrar tráfego de entrada com DNAT do Azure Firewall com o portal do Azure

Pode configurar a Tradução de Endereços de Rede de Destino (DNAT) do Azure Firewall para traduzir e filtrar o tráfego de entrada para as suas sub-redes. O Azure Firewall não tem um conceito de regras de entrada e de saída. Existem regras de aplicação e regras de rede que são aplicadas a qualquer tráfego que entra na firewall. As regras de rede são aplicadas primeiro, em seguida são aplicadas as regras de aplicação e as regras são terminais.

>[!NOTE]
>Atualmente, a funcionalidade DNAT da Firewall está disponível apenas no Azure PowerShell e REST.

Por exemplo, se uma regra de rede for correspondida, o pacote não é avaliado pelas regras de aplicação. Se não houver uma correspondência de regra de rede e o protocolo do pacote for HTTP/HTTPS, o pacote é então avaliado pelas regras de aplicação. Se ainda não for encontrada uma correspondência, o pacote é avaliado em relação à [coleção de regras de infraestrutura](infrastructure-fqdns.md). Se ainda não houver correspondência, então o pacote é negado por predefinição.

Quando configura a DNAT, a ação da coleção de regras NAT é definida como **Tradução de Endereços de Rede de Destino (DNAT)**. A porta e o IP público da firewall traduzem-se numa porta e endereço IP privado. Em seguida, as regras são aplicadas como habitual, as regras de rede primeiro e as regras de aplicação a seguir. Por exemplo, pode configurar uma regra de rede para permitir o tráfego do Ambiente de Trabalho Remoto na porta TCP 3389. A tradução de endereços acontece primeiro e, em seguida, são aplicadas as regras de rede e de aplicação com os endereços traduzidos.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Configurar um ambiente de rede de teste
> * Implementar uma firewall
> * Criar uma rota predefinida
> * Configurar uma regra DNAT
> * Configurar uma regra de rede
> * Testar a firewall

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

Neste tutorial, vai criar duas VNets em modo peering:
- **VNet-Hub** - a firewall está nesta VNet.
- **VN-Spoke** - o servidor de carga de trabalho está nesta VNet.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos
1. Inicie sessão no Portal do Azure em [http://portal.azure.com](http://portal.azure.com).
1. Na página de boas-vindas do portal do Azure, clique em **Grupos de recursos** e, em seguida, clique em **Adicionar**.
2. Em **Nome do grupo de recursos**, escreva **RG-DNAT-Test**.
3. Em **Subscrição**, selecione a sua subscrição.
4. Em **Localização do grupo de recursos**, selecione uma localização. Todos os recursos subsequentes que criar têm de estar na mesma localização.
5. Clique em **Criar**.

## <a name="set-up-the-network-environment"></a>Configurar o ambiente de rede
Em primeiro lugar, crie as VNets e, em seguida, configure o peering entre elas.

### <a name="create-the-hub-vnet"></a>Criar a VNet Hub
1. Na página de boas-vindas do portal do Azure, clique em **Todos os serviços**.
2. Em **Rede**, clique em **Redes virtuais**.
3. Clique em **Adicionar**.
4. Em **Nome**, escreva **VN-Hub**.
5. Em **Espaço de endereços**, escreva **10.0.0.0/16**.
7. Em **Subscrição**, selecione a sua subscrição.
8. Em **Grupo de recursos**, selecione **Utilizar existente** e, em seguida, selecione **RG-DNAT-Test**.
9. Em **Localização**, selecione a mesma localização que utilizou anteriormente.
10. Em **Sub-rede**, em **Nome**, escreva **AzureFirewallSubnet**.

     A firewall estará nesta sub-rede, e o nome da sub-rede **tem** de ser AzureFirewallSubnet.
     > [!NOTE]
     > O tamanho mínimo da sub-rede AzureFirewallSubnet é /25.
11. Em **Intervalo de endereços**, escreva **10.0.1.0/24**.
12. Utilize as outras predefinições e, em seguida, clique em **Criar**.

### <a name="create-a-spoke-vnet"></a>Criar uma VNet spoke

1. Na página de boas-vindas do portal do Azure, clique em **Todos os serviços**.
2. Em **Rede**, clique em **Redes virtuais**.
3. Clique em **Adicionar**.
4. Em **Nome**, escreva **VN-Spoke**.
5. Em **Espaço de endereços**, escreva **192.168.0.0/16**.
7. Em **Subscrição**, selecione a sua subscrição.
8. Em **Grupo de recursos**, selecione **Utilizar existente** e, em seguida, selecione **RG-DNAT-Test**.
9. Em **Localização**, selecione a mesma localização que utilizou anteriormente.
10. Em **Sub-rede**, para **Nome**, escreva **SN-Workload**.

    O servidor irá estar nesta sub-rede.
1. Em **Intervalo de endereços**, escreva **192.168.1.0/24**.
2. Utilize as outras predefinições e, em seguida, clique em **Criar**.

### <a name="peer-the-vnets"></a>Configurar o peering entre as VNets

Agora, configure o peering entre as duas VNets.

#### <a name="hub-to-spoke"></a>Hub para spoke

1. Clique na rede virtual **VN-Hub**.
2. Em **Definições**, clique em **Peerings**.
3. Clique em **Adicionar**.
4. Escreva **Peer-HubSpoke** para o nome.
5. Selecione **VN-Spoke** para a rede virtual.
7. Clique em **OK**.

#### <a name="spoke-to-hub"></a>Spoke para hub

1. Clique na rede virtual **VN-Spoke**.
2. Em **Definições**, clique em **Peerings**.
3. Clique em **Adicionar**.
4. Escreva **Peer-SpokeHub** para o nome.
5. Selecione **VN-Hub** para a rede virtual.
6. Clique em **Permitir tráfego reencaminhado**.
7. Clique em **OK**.

## <a name="create-a-virtual-machine"></a>Criar uma máquina virtual

Crie uma máquina virtual de carga de trabalho e coloque-a na sub-rede **SN-Workload**.

1. Na página de boas-vindas do portal do Azure, clique em **Todos os serviços**.
2. Em **Computação**, clique em **Máquinas virtuais**.
3. Clique em **Adicionar** e clique em **Windows Server**, clique em **Windows Server 2016 Datacenter** e, em seguida, clique em **Criar**.

**Noções básicas**

1. Em **Nome**, escreva **Srv-Workload**.
5. Escreva um nome de utilizador e uma palavra-passe.
6. Em **Subscrição**, selecione a sua subscrição.
7. Em **Grupo de recursos**, clique em **Utilizar existente** e, em seguida, selecione **RG-DNAT-Test**.
8. Em **Localização**, selecione a mesma localização que utilizou anteriormente.
9. Clique em **OK**.

**Tamanho**

1. Escolha um tamanho adequado para uma máquina virtual de teste com o Windows Server. Por exemplo, **B2ms** (8 GB de RAM, 16 GB de armazenamento).
2. Clique em **Selecionar**.

**Definições**

1. Em **Rede**, para **Rede virtual**, selecione **VN-Spoke**.
2. Em **Sub-rede**, selecione **SN-Workload**.
3. Clique em **Endereço IP público** e, em seguida, clique em **Nenhum**.
4. Em **Selecionar portas de entrada públicas**, selecione **Não existem portas de entrada públicas**. 
2. Mantenha as outras predefinições e clique em **OK**.

**Resumo**

Reveja o resumo e clique em **Criar**. Este processo irá demorar alguns minutos a concluir.

Após a conclusão da implementação, tome nota do endereço IP privado para a máquina virtual. Será utilizado mais tarde quando configurar a firewall. Clique no nome da máquina virtual e, em **Definições**, clique em **Rede** para encontrar o endereço IP privado.


## <a name="deploy-the-firewall"></a>Implementar a firewall

1. Na página de boas-vindas do portal, clique em **Criar um recurso**.
2. Clique em **Rede**e, depois de **Destaques**, clique em **Ver tudo**.
3. Clique em **Firewall**e, em seguida, clique em **Criar**. 
4. Na página **Criar uma firewall**, utilize a seguinte tabela para configurar a firewall:
   
   |Definição  |Valor  |
   |---------|---------|
   |Nome     |FW-DNAT-test|
   |Subscrição     |\<a sua subscrição\>|
   |Grupo de recursos     |**Utilizar existente**: RG-DNAT-Test |
   |Localização     |Selecionar a mesma localização que utilizou anteriormente|
   |Escolher uma rede virtual     |**Utilizar existente**: VN-Hub|
   |Endereço IP público     |**Crie um novo**. O endereço IP público tem de ser do tipo SKU Standard.|

2. Clique em **Rever + criar**.
3. Reveja o resumo e clique em **Criar** para criar a firewall.

   Este processo irá demorar alguns minutos a implementar.
4. Depois de concluída a implementação, vá para o grupo de recursos **RG-DNAT-Test** e clique na firewall **FW-DNAT-test**.
6. Anote o endereço IP privado. Vai utilizá-lo mais tarde quando criar a rota predefinida.


## <a name="create-a-default-route"></a>Criar uma rota predefinida

Na sub-rede **SN-Workload**, vai configurar a rota de saída predefinida para passar pela firewall.

1. Na página de boas-vindas do portal do Azure, clique em **Todos os serviços**.
2. Em **Rede**, clique em **Tabelas de rotas**.
3. Clique em **Adicionar**.
4. Em **Nome**, escreva **RT-FWroute**.
5. Em **Subscrição**, selecione a sua subscrição.
6. Em **Grupo de recursos**, selecione **Utilizar existente** e selecione **RG-DNAT-Test**.
7. Em **Localização**, selecione a mesma localização que utilizou anteriormente.
8. Clique em **Criar**.
9. Clique em **Atualizar**  e, em seguida, clique na tabela de rotas **RT-FWroute**.
10. Clique em **Sub-redes** e, em seguida, clique em **Associar**.
11. Clique em **Rede virtual** e selecione **VN-Spoke**.
12. Em **Sub-rede**, clique em **SN-Workload**.
13. Clique em **OK**.
14. Clique em **Rotas** e, em seguida, clique em **Adicionar**.
15. Em **Nome da rota**, escreva **FW-DG**.
16. Em **Prefixo de endereço**, escreva **0.0.0.0/0**.
17. Em **Tipo de salto seguinte**, selecione **Aplicação virtual**.

    O Azure Firewall é, de facto, um serviço gerido, mas a aplicação virtual funciona nesta situação.
1. Em **Endereço do próximo salto**, escreva o endereço IP privado para a firewall, que anotou anteriormente.
2. Clique em **OK**.


## <a name="configure-a-dnat-rule"></a>Configurar uma regra DNAT

```azurepowershell-interactive
 $rgName  = "RG-DNAT-Test"
 $firewallName = "FW-DNAT-test"
 $publicip = type the Firewall public ip
 $newAddress = type the private IP address for the Srv-Workload virtual machine 
 
# Get Firewall
    $firewall = Get-AzureRmFirewall -ResourceGroupName $rgName -Name $firewallName
  # Create NAT rule
    $natRule = New-AzureRmFirewallNatRule -Name RL-01 -SourceAddress * -DestinationAddress $publicip -DestinationPort 3389 -Protocol TCP -TranslatedAddress $newAddress -TranslatedPort 3389
  # Create NAT rule collection
    $natRuleCollection = New-AzureRmFirewallNatRuleCollection -Name RC-DNAT-01 -Priority 200 -Rule $natRule
  # Add NAT Rule collection to firewall:
    $firewall.AddNatRuleCollection($natRuleCollection)
  # Save:
    $firewall | Set-AzureRmFirewall
```
## <a name="configure-a-network-rule"></a>Configurar uma regra de rede

1. Abra **RG-DNAT-Test**e clique na firewall **FW-DNAT-test**.
1. Na página **FW-DNAT-test**, em **Definições**, clique em **Regras**.
2. Clique em **Adicionar coleção de regras de rede**.

Configure a regra com a seguinte tabela e clique em **Adicionar**:


|Parâmetro  |Valor  |
|---------|---------|
|Nome     |**RC-Net-01**|
|Prioridade     |**200**|
|Ação     |**Permitir**|

Em **Regras**:

|Parâmetro  |Definição  |
|---------|---------|
|Nome     |**RL-RDP**|
|Protocolo     |**TCP**|
|Endereços de Origem     |*|
|Endereços de Destino     |Endereço IP privado de **Srv-Workload**|
|Portas de destino|**3389**|


## <a name="test-the-firewall"></a>Testar a firewall

1. Ligue uma área de trabalho remota ao endereço IP público da firewall. Deverá estar ligado à máquina virtual **Srv-Workload**.
3. Feche o ambiente de trabalho remoto.
4. Altere a ação da coleção de regras de rede **RC-Net-01** para **Negar**.
5. Tente ligar novamente ao endereço IP público da firewall. Desta vez, a ligação não deve ter êxito devido à regra **Negar**.

## <a name="clean-up-resources"></a>Limpar recursos

Pode manter os recursos da firewall para o próximo tutorial. Se já não precisar dos mesmos, elimine o grupo de recursos **RG-DNAT-Test** para eliminar todos os recursos relacionados com a firewall.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configurar um ambiente de rede de teste
> * Implementar uma firewall
> * Criar uma rota predefinida
> * Configurar uma regra DNAT
> * Configurar uma regra de rede
> * Testar a firewall

Em seguida, pode monitorizar os registos do Azure Firewall.

> [!div class="nextstepaction"]
> [Tutorial: monitorizar registos do Azure Firewall](./tutorial-diagnostics.md)
