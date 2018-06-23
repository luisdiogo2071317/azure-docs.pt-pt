---
title: Análise de tráfego do Azure | Microsoft Docs
description: Saiba como analisar os registos da fluxo de grupo de segurança de rede do Azure com a análise de tráfego.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/15/2018
ms.author: yagup;jdial
ms.openlocfilehash: ad26772650cf052926a2534d343f64765f47b78f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333399"
---
# <a name="traffic-analytics"></a>Análise de tráfego

Análise de tráfego é uma solução baseada na nuvem que fornece visibilidade para a atividade de utilizador e a aplicação em redes em nuvem. Análise de tráfego analisa registos fluxo (NSG) do grupo de segurança de rede de observador de rede para fornecer informações de fluxo de tráfego em nuvem do Azure. Análise de tráfego, pode:

- Visualize a atividade de rede nas suas subscrições do Azure e identificar oportunidades de frequente.
- Identificar ameaças de segurança para e proteger a sua rede, com informações tais como portas de aberto, a tentativa de acesso à internet e máquinas virtuais (VM) a ligar ao não autorizados redes de aplicações.
- Compreenda os padrões de fluxo de tráfego em regiões do Azure e a internet para otimizar a implementação de rede para o desempenho e a capacidade.
- Identificar configurações incorretas de rede à esquerda para ligações com falhas na sua rede.

## <a name="why-traffic-analytics"></a>Análise de tráfego por que motivo?

É vital para monitorizar, gerir e conhecer a sua própria rede de segurança não comprometida, conformidade e desempenho. Saber o seu ambiente é essencial da importância para proteger e otimizá-lo. Muitas vezes, precisa de saber o estado atual da rede, que está a ligar, onde está a ligar, que portas são abertas à internet, o comportamento de rede esperado, o comportamento de dados de rede, e aumenta repentino no tráfego.

Redes em nuvem são diferentes redes de empresa no local, onde pode contar netflow ou routers compatíveis com protocolo equivalentes e comutadores, que fornecem a capacidade para recolher o tráfego de rede IP conforme que entra ou sai de uma interface de rede. Ao analisar dados de fluxo de tráfego, pode criar uma análise do fluxo de tráfego de rede e de volume.

Redes virtuais do Azure, tiver registos de fluxo NSG, que lhe fornecem informações sobre a entrada e tráfego de IP de saída através de um grupo de segurança de rede associados a interfaces de rede individuais, VMs ou sub-redes. Ao analisar registos de fluxo NSG não processados e inserir intelligence do tráfego de segurança, a topologia e de geografia, análise pode fornecer informações sobre o fluxo de tráfego no seu ambiente. Análise de tráfego fornece informações como anfitriões mais comunicar mais comunicar protocolos de aplicação, mais uma conversação pares de anfitrião, tráfego permitidos/bloqueados, o tráfego de entrada/saída, abrir portas de internet, regras de bloqueio mais, tráfego distribuição por centro de dados do Azure, a rede virtual, sub-redes, ou, não autorizados redes.

## <a name="key-components"></a>Componentes principais

- **O grupo de segurança de rede (NSG)**: contém uma lista de regras de segurança que permitem ou negam o tráfego de rede para recursos ligados a uma rede Virtual do Azure. Os NSGs podem ser associados a sub-redes, VMs individuais (clássicas) ou a interfaces de rede individuais (NIC) ligadas a VMs (Resource Manager). Para obter mais informações, consulte [descrição geral de grupo de segurança de rede](../virtual-network/security-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Registos de fluxo de grupo (NSG) de segurança de rede**: permitem-lhe ver informações sobre o tráfego IP de entrada e de saída através de um grupo de segurança de rede. Fluxo NSG registos são escritos no formato json e mostram fluxos de saída e entrados numa base por regra, que o NIC o fluxo aplica-se a, cinco cadeias de identificação informações sobre o fluxo (endereço IP de origem/destino, porta de origem/destino e protocolo), e se o tráfego foi permitido ou negado. Para obter mais informações sobre os registos de fluxo NSG, consulte [registos de fluxo NSG](network-watcher-nsg-flow-logging-overview.md).
- **Análise de registo**: serviço de Azure um que recolhe dados de monitorização e armazena os dados num repositório central. Estes dados podem incluir eventos, dados de desempenho ou dados personalizados fornecidos através da API do Azure. Depois de recolhidos, os dados estão disponíveis para alertas, análises e exportação. Monitorização de aplicações, tais como a análise de tráfego e da monitorização do desempenho de rede é criadas através da análise do registo de base. Para obter mais informações, consulte [Iniciar análise](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Área de trabalho de análise de registo**: uma instância de análise de registos, onde os dados relativos a uma conta do Azure, são armazenados. Para obter mais informações sobre áreas de trabalho de análise de registo, consulte [criar uma área de trabalho de análise de registos](../log-analytics/log-analytics-quick-create-workspace.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).
- **Observador de rede**: um serviço regional que lhe permite monitorizar e diagnosticar condições a um nível de cenário de rede no Azure. Pode ativar o fluxo registos de NSG e desativar com observador de rede. Para obter mais informações, consulte [observador de rede](network-watcher-monitoring-overview.md).

## <a name="how-traffic-analytics-works"></a>Como funciona a análise de tráfego

Análise de tráfego examina os registos de fluxo NSG não processados e capturas de registos reduzidos por fluxos comuns entre o mesmo endereço IP de origem, o endereço IP de destino, a porta de destino e o protocolo de agregação. Por exemplo, 1 de anfitrião (endereço IP: 10.10.10.10) comunicar com o anfitrião 2 (endereço IP: 10.10.20.10), 100 vezes num período de 1 hora, utilizando a porta (por exemplo, 80) e protocolo (por exemplo, http). O registo reduzido tem uma entrada que o anfitrião 1 & anfitrião 2 comunicados 100 vezes num período de 1 hora, utilizando a porta *80* e protocolo *HTTP*, em vez de ter 100 entradas. Registos reduzidos melhorados com geografia, segurança e informações de topologia e, em seguida, armazenados numa área de trabalho de análise de registo. A imagem seguinte mostra o fluxo de dados:

![Fluxo de dados para os registos de fluxo NSG processamento](./media/traffic-analytics/data-flow-for-nsg-flow-log-processing.png)

## <a name="supported-regions"></a>Regiões suportadas

Pode utilizar a análise de tráfego para os NSGs em qualquer uma das seguintes regiões: Central EUA oeste, EUA leste, EUA Leste 2, Norte Central-nos, Sul Central-nos, EUA Central, EUA oeste, EUA oeste-2, Europa Ocidental, Europa do Norte, RU oeste, RU-Sul, leste da Austrália, Sudeste da Austrália, e Sudeste asiático. A área de trabalho de análise do registo tem de existir o Central EUA oeste, EUA leste, Europa Ocidental, RU-Sul, Sudeste da Austrália ou a região Sudeste asiático.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="user-access-requirements"></a>Requisitos de acesso de utilizador

A conta tem de ser um membro de um dos seguinte do Azure [funções incorporadas](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json):

|Modelo de implementação   | Função                   |
|---------          |---------               |
|Resource Manager   | Proprietário                  |
|                   | Contribuinte            |
|                   | Leitor                 |
|                   | Contribuinte de Rede    |
|Clássica            | Administrador de conta  |
|                   | Administrador de serviço  |
|                   | Coadministrador       |

Se a sua conta não está atribuída a uma das funções incorporadas, tem de ser atribuída a um [função personalizada](../role-based-access-control/custom-roles.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) atribuída as seguintes ações, ao nível da subscrição:

- "Microsoft.Network/applicationGateways/read"
- "Microsoft.Network/connections/read"
- "Microsoft.Network/loadBalancers/read"
- "Microsoft.Network/localNetworkGateways/read"
- "Microsoft.Network/networkInterfaces/read"
- "Microsoft.Network/networkSecurityGroups/read"
- "Microsoft.Network/publicIPAddresses/read"
- "Microsoft.Network/routeTables/read"
- "Microsoft.Network/virtualNetworkGateways/read"
- "Microsoft.Network/virtualNetworks/read"

Para obter informações sobre como verificar permissões de acesso de utilizador, consulte [análise perguntas mais frequentes sobre o tráfego](traffic-analytics-faq.md).

### <a name="enable-network-watcher"></a>Ativar o Observador de Rede

Analisar o tráfego, tem de ter um observador de rede existente, ou [ativar um observador de rede](network-watcher-create.md) em cada região que tem de NSGs que pretende analisar o tráfego para. Análise de tráfego pode ser ativada para NSGs alojados em qualquer um do [regiões suportadas](#supported-regions).

### <a name="re-register-the-network-resource-provider"></a>Volte a registar o fornecedor de recursos de rede

Antes de poder utilizar a análise de tráfego, tem de voltar a registar o fornecedor de recursos de rede. Clique em **tente-** na caixa de código seguinte para abrir a Shell de nuvem do Azure. A Shell de nuvem automaticamente os registos, para a sua subscrição do Azure. Assim que a Shell de nuvem está aberta, introduza o seguinte comando para voltar a registar o fornecedor de recursos de rede:

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Network"
```

### <a name="select-a-network-security-group"></a>Selecione um grupo de segurança de rede

Antes de ativar o registo de fluxo NSG, tem de ter um grupo de segurança de rede para iniciar sessão fluxos para. Se não tiver um grupo de segurança de rede, consulte [criar um grupo de segurança de rede](../virtual-network/manage-network-security-group.md#create-a-network-security-group) para criar um.

No lado esquerdo do portal do Azure, selecione **Monitor**, em seguida, **observador de rede**e, em seguida, selecione **registos de fluxo NSG**. Selecione o grupo de segurança de rede que pretende ativar um registo de fluxo NSG, conforme mostrado na imagem seguinte:

![Seleção de NSGs que necessitam de ativação do registo de fluxo do NSG](./media/traffic-analytics/selection-of-nsgs-that-require-enablement-of-nsg-flow-logging.png)

Se tentar ativar a análise de tráfego para um NSG que está alojado em qualquer região que o [regiões suportadas](#supported-regions), receberá um erro "Não encontrada".

## <a name="enable-flow-log-settings"></a>Ativar as definições de registo de fluxo

Antes de ativar as definições de registo de fluxo, tem de concluir as seguintes tarefas:

Registe o fornecedor de informações do Azure, se ainda não está registado para a sua subscrição:

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Insights
```

Se ainda não tiver uma conta de armazenamento do Azure para armazenar o fluxo NSG nos registos, tem de criar uma conta de armazenamento. Pode criar uma conta de armazenamento com o comando que se segue. Antes de executar o comando, substitua `<replace-with-your-unique-storage-account-name>` com um nome que é exclusivo em todas as localizações do Azure, entre 3 e 24 carateres de comprimento, a utilizar apenas números e letras minúsculas. Também pode alterar o nome do grupo de recursos, se necessário.

```azurepowershell-interactive
New-AzureRmStorageAccount `
  -Location westcentralus `
  -Name <replace-with-your-unique-storage-account-name> `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

Selecione as seguintes opções, conforme mostrado na imagem:

1. Selecione *no* para **Estado**
2. Selecione uma conta de armazenamento existente para armazenar os registos de fluxo no. Se pretende armazenar os dados indefinidamente, defina o valor como *0*. Implicar taxas do Storage do Azure para a conta de armazenamento.
3. Definir **retenção** para o número de dias que pretende armazenar os dados.
4. Selecione *no* para **estado de análise de tráfego**.
5. Selecione uma área de trabalho de análise de registos (OMS) existente ou selecione **criar nova área de trabalho** para criar um novo. Uma área de trabalho de análise de registos é utilizada pela análise de tráfego para armazenar os dados agregados e indexados que, em seguida, são utilizados para gerar a análise. Se selecionou uma área de trabalho existente, deve existir do [regiões suportadas](#traffic-analytics-supported-regions) e foram atualizados para o novo idioma de consulta. Se não pretender atualizar uma área de trabalho existente ou não dispõe de uma área de trabalho numa região suportada, crie um novo. Para obter mais informações sobre idiomas de consulta, consulte [Log Analytics do Azure atualizar para a nova pesquisa de registo](../log-analytics/log-analytics-log-search-upgrade.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

    A área de trabalho de análise do registo que aloja a solução de análise de tráfego e os NSGs não tem de estar na mesma região. Por exemplo, pode ter a análise de tráfego numa área de trabalho na região Europa Ocidental, embora possa ter NSGs nos EUA leste e EUA oeste. Vários NSGs podem ser configurados no mesmo espaço de trabalho.
6. Selecione **Guardar**.

    ![Seleção de conta de armazenamento, a área de trabalho de análise de registos e a ativação de análise de tráfego](./media/traffic-analytics/selection-of-storage-account-log-analytics-workspace-and-traffic-analytics-enablement.png)

Repita os passos anteriores para outros os NSGs para o qual pretende ativar a análise de tráfego para. Dados de registos de fluxo são enviados para a área de trabalho, por isso, certifique-se de que o locais leis e regulamentos no seu país de permitem o armazenamento de dados na região onde a área de trabalho existe.

Também pode configurar através da análise de tráfego a [conjunto AzureRmNetworkWatcherConfigFlowLog](/powershell/module/azurerm.network/set-azurermnetworkwatcherconfigflowlog) cmdlet do PowerShell no AzureRm PowerShell versão do módulo 6.2.1 ou posterior. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps).

## <a name="view-traffic-analytics"></a>Análise de tráfego de vista

No lado esquerdo do portal, selecione **todos os serviços**, em seguida, introduza *Monitor* no **filtro** caixa. Quando **Monitor** aparece nos resultados da pesquisa, selecionados-lo. Para começar a explorar e as respetivas capacidades de análise de tráfego, selecione **observador de rede**, em seguida, **análise de tráfego**.

![Aceder ao dashboard a análise de tráfego](./media/traffic-analytics/accessing-the-traffic-analytics-dashboard.png)

O dashboard pode demorar até 30 minutos a aparecer na primeira vez, porque a análise de tráfego de primeiro tem agregar dados suficientes para o mesmo derivar insights significativos, antes de pode gerar relatórios.

## <a name="usage-scenarios"></a>Cenários de utilização

Algumas das informações pode pretender obter depois de análise de tráfego completamente estar configurado, são as seguintes:

### <a name="find-traffic-hotspots"></a>Localizar hotspots de tráfego

**Procurar**

- Os anfitriões, sub-redes e redes virtuais estão enviar ou receber a maioria do tráfego, que atravessa tráfego malicioso máximo e bloquear fluxos significativos?
    - Verifique o gráfico comparative para anfitrião, a sub-rede e a rede virtual. Compreender os anfitriões, sub-redes e redes virtuais estão a enviar ou receber mais tráfego pode ajudar a identificar os anfitriões que estão a processar mais tráfego, e se a distribuição de tráfego é efetuada corretamente.
    - Pode avaliar se o volume de tráfego é adequado para um anfitrião. É o volume de comportamento normal de tráfego ou-merecer mais investigação?
- Quanto tráfego de entrada/saída existe?
    -   É esperado o anfitrião para receber a entrada mais tráfego de saída, ou vice-versa?
- Estatísticas de tráfego bloqueada.
    - Por que motivo está a um anfitrião bloquear um volume de tráfego benignas significativo? Este comportamento requer mais investigação e, provavelmente, a Otimização da configuração
- Estatísticas de tráfego malicioso permitidos/bloqueados
    - Por que motivo é um anfitrião de receção de tráfego malicioso e por que motivo é permitido flui da origem maliciosa? Este comportamento requer mais investigação e, provavelmente, a Otimização da configuração.

    Selecione **ver todos os**, em **anfitrião**, conforme mostrado na imagem seguinte:

    ![Dashboard que mostram o anfitrião com a maioria dos detalhes de tráfego](media/traffic-analytics/dashboard-showcasing-host-with-most-traffic-details.png)

- A imagem seguinte mostra o tempo tendências dos anfitriões talking cinco principais e os detalhes relacionados com o fluxo (permitidos – entrada/saída e negados - fluxos de entrada/saída) para um anfitrião:

    ![Tendência de anfitrião falar com a maior parte da parte superior a cinco](media/traffic-analytics/top-five-most-talking-host-trend.png)

**Procurar**

- Quais são os pares de anfitrião mais conversing?
    - Comportamento esperado, como a comunicação de front-end ou back-end ou comportamento dados, como o tráfego de internet de back-end.
- Estatísticas de tráfego permitidos/bloqueados
    - Por que motivo um anfitrião é permitir ou bloquear o volume de tráfego significativos
- Utilizado com maior frequência protocolo de aplicação entre a maioria dos pares de anfitrião conversing:
    - Estas aplicações são permitidas nesta rede?
    - As aplicações estão configuradas corretamente? Estes são utilizando o protocolo apropriado para comunicação? Selecione **ver todos os** em **induzirem frequentes conversação**, como a mostrar na imagem seguinte:

        ![Dashboard que mostram a conversação mais frequente](./media/traffic-analytics/dashboard-showcasing-most-frequent-conversation.png)

- A imagem seguinte mostra tendências de tempo para as conversações de cinco superiores e detalhes relacionados com o fluxo como e rejeições fluxos de entrada e saídos para um par de conversação:

    ![Detalhes de conversação chatty primeiras cinco e tendência](./media/traffic-analytics/top-five-chatty-conversation-details-and-trend.png)

**Procurar**

- Mais é utilizado o protocolo de aplicação no seu ambiente e os pares de anfitrião conversing estiver a utilizar o protocolo de aplicação mais?
    - Estas aplicações são permitidas nesta rede?
    - As aplicações estão configuradas corretamente? Estes são utilizando o protocolo apropriado para comunicação? Comportamento esperado é portas comuns, tais como 80 e 443. Para comunicação padrão, se são apresentadas as portas invulgares, poderá necessitam de uma alteração da configuração. Selecione **ver todos os** em **porta aplicação**, na imagem seguinte:

        ![Dashboard que mostram os protocolos de aplicações principais](./media/traffic-analytics/dashboard-showcasing-top-application-protocols.png)

- O tempo de mostrar imagens seguintes tendências para os protocolos de L7 primeiras cinco e detalhes relacionados com o fluxo (por exemplo e rejeições fluxos) para um protocolo de L7:

    ![Cinco superior camada 7 detalhes de protocolos e tendência](./media/traffic-analytics/top-five-layer-seven-protocols-details-and-trend.png)

    ![Fluxo de detalhes para o protocolo de aplicação na pesquisa de registo](./media/traffic-analytics/flow-details-for-application-protocol-in-log-search.png)

**Procurar**

- Tendências de utilização de capacidade de um gateway de VPN no seu ambiente.
    - Cada SKU de VPN que permite que uma determinada quantidade de largura de banda. Os gateways de VPN são subutilizados?
    - São gateways de atingir a capacidade? Deverá atualizar para o SKU superior seguinte?
- Os anfitriões mais conversing, através do gateway VPN, que são através da qual porta?
    - Neste padrão, é normal? Selecione **ver todos os** em **gateway de VPN**, conforme mostrado na imagem seguinte:

        ![Dashboard que mostram a superiores ligações VPN ativas](./media/traffic-analytics/dashboard-showcasing-top-active-vpn-connections.png)

- A imagem seguinte mostra o tempo tendências de utilização da capacidade de um Gateway de VPN do Azure e os detalhes de relacionados com o fluxo de mensagens em fila (por exemplo, portas e de fluxos permitidos):

    ![VPN gateway tendência e fluxo de detalhes de utilização](./media/traffic-analytics/vpn-gateway-utilization-trend-and-flow-details.png)

### <a name="visualize-traffic-distribution-by-geography"></a>Visualizar a distribuição de tráfego por geografia

**Procurar**

- Distribuição de tráfego por centro de dados, tais como origens principais de tráfego para um centro de dados, redes rogue superior uma conversação com o Centro de dados e a parte superior de uma conversação protocolos de aplicação.
    - Se observar mais carga no Centro de dados, pode planear para distribuição de tráfego eficiente.
    - Se rogue redes são uma conversação no Centro de dados, em seguida, corrija as regras do NSG para bloqueá-los.

    Selecione **mapa de vista** em **ambiente**, conforme mostrado na imagem seguinte:

    ![Distribuição de tráfego que mostram a dashboard](./media/traffic-analytics/dashboard-showcasing-traffic-distribution.png)

- O mapa de georreplicação mostra o Friso superior para a seleção de parâmetros, tais como centros de dados (ativar a análise de implementado/não-implementação/Active/Inactive/tráfego/análise de tráfego não ativado) e de países contribuir Benign/maliciosos o tráfego para o Active Directory implementação:

    ![Vista do mapa Georreplicação que mostram a implementação do Active Directory](./media/traffic-analytics/geo-map-view-showcasing-active-deployment.png)

- O mapa de georreplicação mostra a distribuição de tráfego para um centro de dados de países e continentes comunicar ao mesmo no blue (tráfego benignas) e o cliente vermelho (tráfego malicioso) coloridos linhas:

    ![Vista do mapa Georreplicação que mostram a distribuição de tráfego de países e continentes](./media/traffic-analytics/geo-map-view-showcasing-traffic-distribution-to-countries-and-continents.png)

    ![Fluxo de detalhes para a distribuição de tráfego na pesquisa de registo](./media/traffic-analytics/flow-details-for-traffic-distribution-in-log-search.png)

### <a name="visualize-traffic-distribution-by-virtual-networks"></a>Visualizar a distribuição de tráfego redes virtuais

**Procurar**

- Distribuição de tráfego por rede virtual, a topologia, origens principais de tráfego para a rede virtual, redes rogue superior uma conversação de rede virtual e superior uma conversação protocolos de aplicação.
    - Saber qual rede virtual é uma conversação que rede virtual. Se a conversação não é esperada, podem ser corrigida.
    - Se rogue redes são uma conversação com uma rede virtual, pode corrigir as regras do NSG para bloquear as redes rogue.
 
    Selecione **vista VNets** em **ambiente**, conforme mostrado na imagem seguinte:

    ![Dashboard que mostram a distribuição de rede virtual](./media/traffic-analytics/dashboard-showcasing-virtual-network-distribution.png)

- A topologia de rede Virtual mostra o Friso superior para a seleção de parâmetros como de uma rede virtual (Inter a rede virtual ligações/Active/Inactive), ligações externas, fluxos de Active Directory e maliciosos fluxos da rede virtual.
- A topologia de rede Virtual mostra a distribuição de tráfego para uma rede virtual relativamente fluxos (permitidos/bloqueado/entrada/saída/Benign/maliciosos), o protocolo de aplicação e grupos de segurança de rede, por exemplo:

    ![Topologia de rede virtual que mostram detalhes de distribuição e o fluxo de tráfego](./media/traffic-analytics/virtual-network-topology-showcasing-traffic-distribution-and-flow-details.png)

    ![Fluxo de detalhes para a distribuição de tráfego de rede virtual na pesquisa de registo](./media/traffic-analytics/flow-details-for-virtual-network-traffic-distribution-in-log-search.png)

**Procurar**

- Distribuição por sub-rede, a topologia, origens principais de tráfego para a sub-rede de tráfego redes rogue superior uma conversação para a sub-rede e a parte superior de uma conversação protocolos de aplicação.
    - Saber qual sub-rede é uma conversação que sub-rede. Se vir conversações inesperadas, pode corrigir a configuração.
    - Se rogue redes são uma conversação com uma sub-rede, conseguir corrigi-lo ao configurar regras NSG para bloquear as redes rogue.
- A topologia de sub-redes mostra o Friso superior para a seleção de parâmetros, tais como Active/Inactive sub-rede, ligações externas, Active Directory fluxos e maliciosos fluxos da sub-rede.
- A topologia de sub-rede mostra a distribuição de tráfego para uma rede virtual relativamente fluxos (permitidos/bloqueado/entrada/saída/Benign/maliciosos), o protocolo de aplicação e os NSGs, por exemplo:

    ![Topologia de sub-rede que mostram uma sub-rede de rede virtual relativamente fluxos a distribuição de tráfego](./media/traffic-analytics/subnet-topology-showcasing-traffic-distribution-to-a-virtual-subnet-with-regards-to-flows.png)

**Procurar**

Distribuição de tráfego por gateway de aplicação & Balanceador de carga, topologia, origens principais de tráfego, superior não autorizados redes uma conversação para o gateway de aplicação & o Balanceador de carga e a parte superior uma conversação protocolos de aplicação. 
    
 - Saber qual sub-rede é uma conversação para o gateway de aplicação ou o Balanceador de carga. Se observar conversações inesperadas, pode corrigir a configuração.
 - Se rogue redes são uma conversação com um gateway de aplicação ou o Balanceador de carga, conseguir corrigi-lo ao configurar regras NSG para bloquear as redes rogue. 

    ![subnet-Topology-showcasing-Traffic-Distribution-to-a-Application-Gateway-subnet-with-regards-to-flows](./media/traffic-analytics/subnet-topology-showcasing-traffic-distribution-to-a-application-gateway-subnet-with-regards-to-flows.png)

### <a name="view-ports-and-virtual-machines-receiving-traffic-from-the-internet"></a>Ver as portas e as máquinas virtuais a receber tráfego através da internet

**Procurar**

- As portas abertas são uma conversação através da internet?
    - Se encontram-se inesperado portas abertas, pode corrigir a configuração:

    ![Dashboard que mostram portas receber e enviar o tráfego à internet](./media/traffic-analytics/dashboard-showcasing-ports-receiving-and-sending-traffic-to-the-internet.png)

    ![Detalhes de portas de destino do Azure e anfitriões](./media/traffic-analytics/details-of-azure-destination-ports-and-hosts.png)

**Procurar**

Tem tráfego malicioso no seu ambiente? Onde está esta provenientes? Onde é destinado aos-lo?

![Detalhes de fluxos de tráfego malicioso na pesquisa de registo](./media/traffic-analytics/malicious-traffic-flows-detail-in-log-search.png)


### <a name="visualize-the-trends-in-nsgnsg-rules-hits"></a>Visualizar tendências em pedidos de regras do NSG/NSG

**Procurar**

- As regras NSG/NSG tem os maioria dos pedidos comparative gráfico com distribuição fluxos?
- Quais são os principais pares de conversação de origem e de destino por regras do NSG/NSG?

    ![Dashboard que mostram o NSG estatísticas de pedidos com êxito](./media/traffic-analytics/dashboard-showcasing-nsg-hits-statistics.png)

- O tempo de mostrar imagens seguintes tendências para pedidos de regras do NSG e detalhes de fluxo de origem de destino para um grupo de segurança de rede:

    - Detetar rapidamente quais os NSGs e NSG regras são atravessar maliciosos fluxos e que são o IP malicioso superior endereços aceder ao seu ambiente na nuvem
    - Identificar as regras NSG/NSG são permitir/bloquear o tráfego de rede significativo
    - Selecione superior filtra para granular inspeção de um NSG ou NSG regras

    ![Tempo que mostram a fins de tendência para pedidos de regras do NSG e superiores regras do NSG](./media/traffic-analytics/showcasing-time-trending-for-nsg-rule-hits-and-top-nsg-rules.png)

    ![Detalhes de estatísticas na pesquisa de registo de regras de NSG superior](./media/traffic-analytics/top-nsg-rules-statistics-details-in-log-search.png)

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

Para obter respostas a perguntas mais frequentes, consulte [análise perguntas mais frequentes sobre o tráfego](traffic-analytics-faq.md).
