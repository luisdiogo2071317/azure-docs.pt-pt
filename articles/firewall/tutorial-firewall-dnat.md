---
title: Filtrar tráfego de entrada com DNAT do Azure Firewall com o portal do Azure
description: Neste tutorial, vai aprender a implementar e configurar a DNAT do Azure Firewall com o portal do Azure.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 9/27/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 894389ec07fb8e371a269f895473fe82985de7c3
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405976"
---
# <a name="tutorial-filter-inbound-traffic-with-azure-firewall-dnat-using-the-azure-portal"></a>Tutorial: Filtrar tráfego de entrada com DNAT do Azure Firewall com o portal do Azure

Pode configurar a Tradução de Endereços de Rede de Destino (DNAT) do Azure Firewall para traduzir e filtrar o tráfego de entrada para as suas sub-redes. Quando configura a DNAT, a ação da coleção de regras NAT é definida como **Tradução de Endereços de Rede de Destino (DNAT)**. Cada regra na coleção de regras NAT pode então ser utilizada para traduzir o endereço IP e a porta de firewall públicos para um IP e porta privados. As regras DNAT adicionam implicitamente uma regra de rede correspondente para permitir o tráfego traduzido. Pode substituir esse comportamento, ao adicionar explicitamente uma coleção de regras de rede com regras de negar que correspondem ao tráfego traduzido. Para saber mais sobre a lógica de processamento de regras do Azure Firewall, veja [Lógica de processamento de regras do Azure Firewall](rule-processing.md).

> [!NOTE]
> O DNAT não funciona para a porta 80 e 22. Estamos a trabalhar para corrigir este problema no futuro próximo. Enquanto isso, utilize qualquer outra porta como a porta de destino nas regras NAT. A porta 80 ou 22 continua a poder ser utilizada como a porta traduzida. Por exemplo, pode mapear o ip:81 público para o ip:80 privado.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Configurar um ambiente de rede de teste
> * Implementar uma firewall
> * Criar uma rota predefinida
> * Configurar uma regra DNAT
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

1. Abra **RG-DNAT-Test**e clique na firewall **FW-DNAT-test**. 
1. Na página **FW-DNAT-test**, em **Definições**, clique em **Regras**. 
2. Clique em **Adicionar coleção de regras DNAT**. 
3. Em **Nome**, escreva **RC-DNAT-01**. 
1. Em **Prioridade**, escreva **200**. 
6. Em **Regras**, em **Nome**, escreva **RL-01**. 
7. Em **Endereços de Origem**, escreva *. 
8. Em **Endereços de Destino**, escreva o endereço IP público da firewall. 
9. Em **Portas de Destino**, escreva **3389**. 
10. Em **Endereço Traduzido**, escreva o endereço IP privado da máquina virtual Srv-Workload. 
11. Em **Porta traduzida**, escreva **3389**. 
12. Clique em **Adicionar**. 

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
> * Testar a firewall

Em seguida, pode monitorizar os registos do Azure Firewall.

> [!div class="nextstepaction"]
> [Tutorial: monitorizar registos do Azure Firewall](./tutorial-diagnostics.md)
