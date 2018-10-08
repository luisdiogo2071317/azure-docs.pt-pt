---
title: Implementar e configurar o Azure Firewall com o portal do Azure
description: Neste tutorial, irá aprender a implementar e configurar o Azure Firewall com o portal do Azure.
services: firewall
author: vhorne
manager: jpconnock
ms.service: firewall
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: ff8df51011ef664950ecfeb9eef0b201306c8ad5
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47221661"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-using-the-azure-portal"></a>Tutorial: implementar e configurar o Azure Firewall com o portal do Azure

O Azure Firewall tem dois tipos de regra para controlar o acesso de saída:

- **Regras de aplicação**

   Permite-lhe configurar nomes de domínio completamente qualificado (FQDNs) que podem ser acedidos a partir de uma sub-rede. Por exemplo, pode permitir o acesso a *github.com* a partir da sua sub-rede.
- **Regras de rede**

   Permite-lhe configurar as regras que contêm o endereço de origem, o protocolo, a porta de destino e o endereço de destino. Por exemplo, pode criar uma regra para permitir o tráfego para a porta 53 (DNS) para o endereço IP do seu servidor DNS a partir da sua sub-rede.

O tráfego de rede está sujeito às regras de firewall configuradas quando encaminha o tráfego de rede para a firewall como o gateway padrão de sub-rede.

As regras de aplicação e de rede são armazenadas em *coleções de regras*. Uma coleção de regras é uma lista de regras que partilham a mesma ação e prioridade.  Uma coleção de regras de rede é uma lista de regras de rede, e uma coleção de regras de aplicação é uma lista de regras de aplicação.

O Azure Firewall tem regras NAT, regras de rede e regras de aplicações. Para saber mais sobre a lógica de processamento de regras do Azure Firewall, veja [Lógica de processamento de regras do Azure Firewall](rule-processing.md).

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Configurar um ambiente de rede de teste
> * Implementar uma firewall
> * Criar uma rota predefinida
> * Configurar regras de aplicação
> * Configurar regras de rede
> * Testar a firewall



Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

Neste tutorial, criou uma VNet única com três sub-redes:
- **FW-SN** - a firewall está nesta sub-rede.
- **Workload-SN** - o servidor de carga de trabalho está nesta sub-rede. O tráfego de rede desta sub-rede passa pela firewall.
- **Jump-SN** - o servidor “de ligação” está nesta sub-rede. O servidor de ligação tem um endereço IP público ao qual pode ligar com o Ambiente de Trabalho Remoto. A partir daí, pode ligar (com outro Ambiente de Trabalho Remoto) ao servidor de carga de trabalho.

![Tutorial de infraestrutura de rede](media/tutorial-firewall-rules-portal/Tutorial_network.png)

Este tutorial utiliza uma configuração de rede simplificada para facilitar a implementação. Para implementações de produção, é recomendado um [modelo hub-and-spoke](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke), em que a firewall está na sua própria VNet, e os servidores de carga de trabalho estão nas VNets em modo de peering na mesma região, com uma ou mais sub-redes.



## <a name="set-up-the-network-environment"></a>Configurar o ambiente de rede
Em primeiro lugar, crie um grupo de recursos para conter os recursos necessários para implementar a firewall. Em seguida, crie uma VNet, sub-redes e servidores de teste.

### <a name="create-a-resource-group"></a>Criar um grupo de recursos
1. Inicie sessão no Portal do Azure em [http://portal.azure.com](http://portal.azure.com).
1. Na página de boas-vindas do portal do Azure, clique em **Grupos de recursos** e, em seguida, clique em **Adicionar**.
2. Em **Nome do grupo de recursos**, escreva **Test-FW-RG**.
3. Em **Subscrição**, selecione a sua subscrição.
4. Em **Localização do grupo de recursos**, selecione uma localização. Todos os recursos subsequentes que criar têm de estar na mesma localização.
5. Clique em **Criar**.


### <a name="create-a-vnet"></a>Criar uma VNet
1. Na página de boas-vindas do portal do Azure, clique em **Todos os serviços**.
2. Em **Rede**, clique em **Redes virtuais**.
3. Clique em **Adicionar**.
4. Em **Nome**, escreva **Test-FW-VN**.
5. Em **Espaço de endereços**, escreva **10.0.0.0/16**.
7. Em **Subscrição**, selecione a sua subscrição.
8. Em **Grupo de recursos**, selecione **Utilizar existente** e, em seguida, selecione **Test-FW-RG**.
9. Em **Localização**, selecione a mesma localização que utilizou anteriormente.
10. Em **Sub-rede**, em **Nome**, escreva **AzureFirewallSubnet**. A firewall estará nesta sub-rede, e o nome da sub-rede **tem** de ser AzureFirewallSubnet.
11. Em **Intervalo de endereços**, escreva **10.0.1.0/24**.
12. Utilize as outras predefinições e, em seguida, clique em **Criar**.

> [!NOTE]
> O tamanho mínimo da sub-rede AzureFirewallSubnet é /25.

### <a name="create-additional-subnets"></a>Criar sub-redes adicionais

Em seguida, crie sub-redes para o servidor de ligação e uma sub-rede para os servidores de carga de trabalho.

1. Na página de boas-vindas do portal do Azure, clique em **Grupos de recursos** e, em seguida, clique em **Test-FW-RG**.
2. Clique na rede virtual **Test-FW-VN**.
3. Clique em **Sub-redes** e, em seguida, clique em **+Sub-rede**.
4. Em **Nome**, escreva **Workload-SN**.
5. Em **Intervalo de endereços**, escreva **10.0.2.0/24**.
6. Clique em **OK**.

Crie outra sub-rede com o nome **Jump-SN** e com o intervalo de endereços **10.0.3.0/24**.

### <a name="create-virtual-machines"></a>Criar máquinas virtuais

Agora, crie as máquinas virtuais de ligação e de carga de trabalho, e coloque-as nas sub-redes adequadas.

1. Na página de boas-vindas do portal do Azure, clique em **Todos os serviços**.
2. Em **Computação**, clique em **Máquinas virtuais**.
3. Clique em **Adicionar** e clique em **Windows Server**, clique em **Windows Server 2016 Datacenter** e, em seguida, clique em **Criar**.

**Noções básicas**

1. Em **Nome**, escreva **Srv-Jump**.
5. Escreva um nome de utilizador e uma palavra-passe.
6. Em **Subscrição**, selecione a sua subscrição.
7. Em **Grupo de recursos**, clique em **Utilizar existente** e, em seguida, selecione **Test-FW-RG**.
8. Em **Localização**, selecione a mesma localização que utilizou anteriormente.
9. Clique em **OK**.

**Tamanho**

1. Escolha um tamanho adequado para uma máquina virtual de teste com o Windows Server. Por exemplo, **B2ms** (8 GB de RAM, 16 GB de armazenamento).
2. Clique em **Selecionar**.

**Definições**

1. Em **Rede**, em **Rede virtual**, selecione **Test-FW-VN**.
2. Em **Sub-rede**, selecione **Jump-SN**.
3. Em **Selecionar portas de entrada públicas**, selecione **RDP (3389)**. 

    Vai querer limitar o acesso ao seu endereço IP público, mas precisa de abrir a porta 3389 para conseguir ligar um ambiente de trabalho remoto ao servidor de ligação. 
2. Mantenha as outras predefinições e clique em **OK**.

**Resumo**

Reveja o resumo e clique em **Criar**. Este processo irá demorar alguns minutos a concluir.

Repita este processo para criar outra máquina virtual com o nome **Srv-Work**.

Utilize as informações na tabela seguinte para configurar as **Definições** para a máquina virtual Srv-Work. O resto da configuração é igual à da máquina virtual Srv-Jump.


|Definição  |Valor  |
|---------|---------|
|Subrede|Workload-SN|
|Endereço IP público|Nenhuma|
|Selecionar portas de entrada públicas|Sem portas de entrada públicas|


## <a name="deploy-the-firewall"></a>Implementar a firewall

1. Na página de boas-vindas do portal, clique em **Criar um recurso**.
2. Clique em **Rede**e, depois de **Destaques**, clique em **Ver tudo**.
3. Clique em **Firewall**e, em seguida, clique em **Criar**. 
4. Na página **Criar uma firewall**, utilize a seguinte tabela para configurar a firewall:
   
   |Definição  |Valor  |
   |---------|---------|
   |Nome     |Test-FW01|
   |Subscrição     |\<a sua subscrição\>|
   |Grupo de recursos     |**Utilizar existente**: Test-FW-RG |
   |Localização     |Selecionar a mesma localização que utilizou anteriormente|
   |Escolher uma rede virtual     |**Utilizar existente**: Test-FW-VN|
   |Endereço IP público     |**Crie um novo**. O endereço IP público tem de ser do tipo SKU Standard.|

2. Clique em **Rever + criar**.
3. Reveja o resumo e clique em **Criar** para criar a firewall.

   Este processo irá demorar alguns minutos a implementar.
4. Depois de concluída a implementação, vá para o grupo de recursos **Test-FW-RG** e clique na firewall **Test-FW01**.
6. Anote o endereço IP privado. Vai utilizá-lo mais tarde quando criar a rota predefinida.


## <a name="create-a-default-route"></a>Criar uma rota predefinida

Na sub-rede **Workload-SN**, vai configurar a rota de saída padrão para passar pela firewall.

1. Na página de boas-vindas do portal do Azure, clique em **Todos os serviços**.
2. Em **Rede**, clique em **Tabelas de rotas**.
3. Clique em **Adicionar**.
4. Em **Nome**, escreva **Firewall-route**.
5. Em **Subscrição**, selecione a sua subscrição.
6. Em **Grupo de recursos**, selecione **Utilizar existente** e selecione **Test-FW-RG**.
7. Em **Localização**, selecione a mesma localização que utilizou anteriormente.
8. Clique em **Criar**.
9. Clique em **Atualizar**  e, em seguida, clique na tabela de rotas **Firewall-route**.
10. Clique em **Sub-redes** e, em seguida, clique em **Associar**.
11. Clique em **Rede virtual** e, em seguida, selecione **Test-FW-VN**.
12. Em **Sub-rede**, clique em **Workload-SN**.
13. Clique em **OK**.
14. Clique em **Rotas** e, em seguida, clique em **Adicionar**.
15. Em **Nome da rota**, escreva **FW-DG**.
16. Em **Prefixo de endereço**, escreva **0.0.0.0/0**.
17. Em **Tipo de salto seguinte**, selecione **Aplicação virtual**.

    O Azure Firewall é, de facto, um serviço gerido, mas a aplicação virtual funciona nesta situação.
1. Em **Endereço do próximo salto**, escreva o endereço IP privado para a firewall, que anotou anteriormente.
2. Clique em **OK**.


## <a name="configure-application-rules"></a>Configurar regras de aplicação


1. Abra o **Test-FW-RG** e clique na firewall **Test-FW01**.
2. Na página **Test-FW01**, em **Definições**, clique em **Regras**.
3. Clique em **Adicionar coleção de regras de aplicação**.
4. Em **Nome**, escreva **App-Coll01**.
5. Em **Prioridade**, escreva **200**.
6. Em **Ação**, selecione **Permitir**.
7. Em **Regras**, em **Nome**, escreva **AllowGH**.
8. Em **Endereços de Origem**, escreva **10.0.2.0/24**.
9. Em **Protocolo:porta**, escreva **http, https**. 
10. Em **FQDNS de destino**, escreva **github.com**
11. Clique em **Adicionar**.

O Azure Firewall inclui uma coleção de regras incorporadas para os FQDNs de infraestrutura que são permitidos por predefinição. Estes FQDNs são específicos da plataforma e não podem ser utilizados para outros fins. Para obter mais informações, veja [FQDNs de Infraestrutura](infrastructure-fqdns.md).

## <a name="configure-network-rules"></a>Configurar regras de rede

1. Clique em **Adicionar coleção de regras de rede**.
2. Em **Nome**, escreva **Net-Coll01**.
3. Em **Prioridade**, escreva **200**.
4. Em **Ação**, selecione **Permitir**.

6. Em **Regras**, em **Nome**, escreva **AllowDNS**.
8. Em **Protocolo**, selecione **UDP**.
9. Em **Endereços de Origem**, escreva **10.0.2.0/24**.
10. Em Endereço de destino, escreva **209.244.0.3,209.244.0.4**
11. Em **Portas de Destino**, escreva **53**.
12. Clique em **Adicionar**.

### <a name="change-the-primary-and-secondary-dns-address-for-the-srv-work-network-interface"></a>Alterar o endereço DNS primário e secundário para a interface de rede **Srv-Work**

Para fins de teste neste tutorial, configure os endereços DNS primários e secundários. Não é um requisito geral do Azure Firewall. 

1. No portal do Azure, abra o grupo de recursos **Test-FW-RG**.
2. Clique na interface de rede da máquina virtual **Srv-Work**.
3. Em **Definições**, clique em **Servidores DNS**.
4. Em **Servidores DNS**, clique em **Personalizado**.
5. Escreva **209.244.0.3** na caixa de texto **Adicionar servidor DNS** e **209.244.0.4** na caixa de texto seguinte.
6. Clique em **Guardar**. 
7. Reinicie a máquina virtual **Srv-Work**.


## <a name="test-the-firewall"></a>Testar a firewall

1. No portal do Azure, reveja as definições de rede para a máquina virtual **Srv-Work** e anote o endereço IP privado.
2. Ligue um ambiente de trabalho remoto à máquina virtual **Srv-Jump** e, a partir daqui, abra uma ligação de ambiente de trabalho remoto ao endereço IP privado de **Srv-Work**.

5. Abra o Internet Explorer e navegue até http://github.com.
6. Clique em **OK** e em **Fechar** nos alertas de segurança.

   Deverá ver a página de boas-vindas do GitHub.

7. Navegue para http://www.msn.com.

   Deve estar bloqueado pela firewall.

Verificou que as regras de firewall estão a funcionar:

- Pode navegar para o único FQDN permitido, mas não para quaisquer outros.
- Pode resolver nomes DNS com o servidor DNS externo configurado.

## <a name="clean-up-resources"></a>Limpar recursos

Pode manter os recursos da firewall para o próximo tutorial. Se já não precisar dos mesmos elimine o grupo de recursos **Test-FW-RG** para eliminar todos os recursos relacionados com a firewall.


## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configurar a rede
> * Criar uma firewall
> * Criar uma rota predefinida
> * Configurar as regras de firewall de aplicação e rede
> * Testar a firewall

Em seguida, pode monitorizar os registos do Azure Firewall.

> [!div class="nextstepaction"]
> [Tutorial: monitorizar registos do Azure Firewall](./tutorial-diagnostics.md)
