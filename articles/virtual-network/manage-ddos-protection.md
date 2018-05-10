---
title: Gerir no portal do Azure do Azure DDoS proteção padrão | Microsoft Docs
description: Saiba como utilizar o Azure DDoS proteção padrão telemetria no Monitor do Azure para mitigar um ataque.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/29/2018
ms.author: jdial
ms.openlocfilehash: dd094f2b9cdb9b5eb164dda2925d094cafa7cd89
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="manage-azure-ddos-protection-standard-using-the-azure-portal"></a>Gerir no portal do Azure do Azure DDoS proteção padrão

Saiba como ativar e desativar distribuída recusa de protection service (DDoS distribuídos) e utilizar a telemetria de correção para atenuar um ataque DDoS com o Azure DDoS proteção Standard. Padrão de proteção DDoS protege os recursos do Azure como máquinas virtuais, balanceadores de carga e gateways de aplicação com um Azure [endereço IP público](virtual-network-public-ip-address.md) atribuída. Para saber mais sobre DDoS proteção padrão e das respetivas funções, consulte o artigo [descrição geral do padrão de proteção DDoS](ddos-protection-overview.md).

Antes de concluir os passos neste tutorial, inicie sessão no portal do Azure em https://portal.azure.com com uma conta atribuída para a [contribuinte de rede](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) função ou a um [função personalizada](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atribuída apropriados ações listadas no [permissões](#permissions).

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="create-a-ddos-protection-plan"></a>Criar um plano de proteção DDoS

Um plano de proteção DDoS define um conjunto de redes virtuais que tenham o padrão de proteção DDoS ativada, entre subscrições. Pode configurar um plano de proteção DDoS para a sua organização e redes virtuais ligação de várias subscrições para o mesmo plano. O plano de proteção DDoS propriamente dito está também associado uma subscrição, o que selecionar durante a criação do plano. A subscrição do plano está associado à implica a fatura mensal recorrente para o plano, bem como excedidos encargos, no caso do número de endereços IP públicos protegidos exceder os 100. Para obter mais informações sobre os preços DDoS, consulte [detalhes de preços](https://azure.microsoft.com/pricing/details/ddos-protection/).

A criação de mais do que um plano não é necessária para a maioria das organizações. Um plano não pode ser movido entre subscrições. Se pretender alterar a subscrição está a ser um plano, terá de [eliminar o plano existente](#work-with-ddos-protection-plans) e criar um novo.

1. Selecione **crie um recurso** no canto superior esquerdo do portal do Azure.
2. Procurar *DDoS*. Quando **plano de proteção DDos** aparece nos resultados da pesquisa, selecionados-lo.
3. Selecione **Criar**.
4. Introduza ou selecione os seus próprios valores, ou introduza, ou selecione os seguintes valores de exemplo e, em seguida, selecione **criar**:

    |Definição        |Valor                                              |
    |---------      |---------                                          |
    |Nome           | myDdosProtectionPlan                              |
    |Subscrição   | Selecione a sua subscrição.                         |
    |Grupo de recursos | Selecione **criar nova** e introduza *myResourceGroup* |
    |Localização       | EUA Leste                                           |

## <a name="enable-ddos-for-a-new-virtual-network"></a>Ativar DDoS para uma nova rede virtual

1. Selecione **crie um recurso** no canto superior esquerdo do portal do Azure.
2. Selecione **Redes** e, em seguida, selecione **Rede virtual**.
3. Introduza ou selecione os seus próprios valores, de enter ou selecione os seguintes valores de exemplo, aceite as restantes predefinições e, em seguida, selecione **criar**:

    | Definição         | Valor                                                        |
    | ---------       | ---------                                                    |
    | Nome            | myVirtualNetwork                                             |
    | Subscrição    | Selecione a sua subscrição.                                    |
    | Grupo de recursos  | Selecione **utilizar existente**e, em seguida, selecione **myResourceGroup** |
    | Localização        | EUA Leste                                                      |
    | Proteção DDos | Selecione **padrão** e, em **proteção DDoS**, selecione **myDdosProtectionPlan**. O plano que selecionar pode ser na subscrição que a rede virtual idêntica ou diferente, mas ambas as subscrições tem de estar associadas ao mesmo inquilino do Azure Active Directory.|

Não é possível mover uma rede virtual para outro grupo de recursos ou subscrição quando DDoS Standard está ativada para a rede virtual. Se for necessário mover uma rede virtual com o padrão de DDoS ativado, desativar DDoS padrão primeiro, mover a rede virtual e, em seguida, ativar DDoS padrão. Após a mudança, são repostos os limiares de política otimizados automaticamente para todos os os protegido endereços IP públicos na rede virtual.

## <a name="enable-ddos-for-an-existing-virtual-network"></a>Ativar DDoS para uma rede virtual existente

1. Criar um plano de proteção DDoS, efetuando os passos em [criar um plano de proteção DDoS](#create-a-ddos-protection-plan), se não tiver um plano de proteção DDoS existente.
2. Selecione **crie um recurso** no canto superior esquerdo do portal do Azure.
3. Introduza o nome da rede virtual que pretende ativar DDoS proteção padrão no **procurar recursos, serviços e caixa docs** na parte superior do portal. Quando o nome da rede virtual é apresentada nos resultados da pesquisa, selecione-a.
4. Selecione **proteção DDoS**, em **definições**.
5. Selecione **padrão**. Em **plano de proteção DDoS**, selecione um plano de proteção DDoS existente ou o plano que criou no passo 1 e, em seguida, selecione **guardar**. O plano que selecionar pode ser na subscrição que a rede virtual idêntica ou diferente, mas ambas as subscrições tem de estar associadas ao mesmo inquilino do Azure Active Directory.

## <a name="disable-ddos-for-a-virtual-network"></a>Desativar DDoS para uma rede virtual

1. Introduza o nome da rede virtual que pretende desativar o padrão de proteção DDoS para no **procurar recursos, serviços e caixa docs** na parte superior do portal. Quando o nome da rede virtual é apresentada nos resultados da pesquisa, selecione-a.
2. Selecione **proteção DDoS**, em **definições**.
3. Selecione **básico** em **plano de proteção DDoS** e, em seguida, selecione **guardar**.

## <a name="work-with-ddos-protection-plans"></a>Trabalhar com planos de proteção DDoS

1. Selecione **todos os serviços** na parte superior, à esquerda do portal.
2. Introduza *DDoS* no **filtro** caixa. Quando **planos de proteção DDoS** aparecer nos resultados, selecione-o.
3. Selecione o plano de proteção que pretende ver na lista.
4. Todas as redes virtuais associadas para o plano estão listadas.
5. Se pretender eliminar um plano, primeiro tem de dissociá todas as redes virtuais a partir do mesmo. Ao desassociar a um plano de uma rede virtual, consulte [desativar DDoS para uma rede virtual](#disable-ddos-for-a-virtual-network).

## <a name="configure-alerts-for-ddos-protection-metrics"></a>Configurar alertas para as métricas de proteção DDoS

Pode selecionar qualquer uma das métricas de proteção DDoS disponíveis para o alertar quando há uma mitigação ativa durante um ataque, utilizando a configuração de alerta de Monitor do Azure. Quando as condições são cumpridas, o endereço especificado recebe um e-mail de alerta:

1. Selecione **todos os serviços** na parte superior, à esquerda do portal.
2. Introduza *Monitor* no **filtro** caixa. Quando **Monitor** aparece nos resultados, selecionados-lo.
3. Selecione **métricas** em **serviços PARTILHADOS**.
4. Introduza, ou selecione os seus próprios valores, ou introduza os seguintes valores de exemplo, aceite as restantes predefinições e, em seguida, selecione **OK**:

    |Definição                  |Valor                                                                                               |
    |---------                |---------                                                                                           |
    |Nome                     | myDdosAlert                                                                                        |
    |Subscrição             | Selecione a subscrição que contém o endereço IP público que pretende receber alertas.        |
    |Grupo de recursos           | Selecione o grupo de recursos que contém o endereço IP público que pretende receber alertas.      |
    |Recurso                 | Selecione o endereço IP público que contém o endereço IP público que pretende receber alertas. DDoS monitoriza os endereços IP públicos atribuídos para recursos dentro de uma rede virtual. Se não tiver quaisquer recursos com endereços IP públicos na rede virtual, primeiro tem de criar um recurso com um endereço IP público. Pode monitorizar o endereço IP público de todos os recursos implementados através do Resource Manager (não clássica) listado no [rede Virtual para os serviços do Azure](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network), exceto para ambientes do App Service do Azure e o Gateway de VPN do Azure. Para continuar com este tutorial, pode criar rapidamente um [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) máquina virtual.                   |
    |Métrica                   | Em DDoS ataques ou não                                                                            |
    |Limiar                | 1 - **1** significa que está sob ataque. **0** significa que não está sob ataque.                         |
    |Período                   | Selecione qualquer valor que escolher.                                                                   |
    |Notificação por E-Mail         | Selecione a caixa de verificação                                                                                  |
    |Administrador adicional | Introduza o seu endereço de correio eletrónico se não tiver um proprietário do e-mail, contribuinte ou leitor para a subscrição. |

    Dentro de alguns minutos de deteção de ataques, receberá um e-mail de métricas de Monitor de Azure semelhante para a imagem seguinte:

    ![Alerta de ataque](./media/manage-ddos-protection/ddos-alert.png)


Para simular um ataque DDoS para validar o alerta, consulte [validar DDoS deteção](#validate-ddos-detection).

Também pode saber mais sobre [configurar webhooks](../monitoring-and-diagnostics/insights-webhooks-alerts.md?toc=%2fazure%2fvirtual-network%2ftoc.json) e [as logic apps](../logic-apps/logic-apps-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) para a criação de alertas.

## <a name="configure-logging-for-ddos-protection-metrics"></a>Configurar o registo para as métricas de proteção DDoS

1. Selecione **todos os serviços** na parte superior, à esquerda do portal.
2. Introduza *Monitor* no **filtro** caixa. Quando **Monitor** aparece nos resultados, selecionados-lo.
3. Em **definições**, selecione **definições de diagnóstico**.
4. Selecione o **subscrição** e **grupo de recursos** que contém o endereço IP público que pretende iniciar sessão.
5. Selecione **endereço IP público** para **tipo de recurso**, em seguida, selecione o endereço IP público específico que pretende registar as métricas para.
6. Selecione **ative os diagnósticos para recolher os seguintes dados** e, em seguida, selecione como muitas das seguintes opções forem necessárias:

    - **Arquivo para uma conta de armazenamento**: dados são escritos para uma conta de armazenamento do Azure. Para obter mais informações sobre esta opção, consulte o artigo [arquivar os registos de diagnóstico](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **Fluxo para um hub de eventos**: permite que um recetor de registo recolher registos com um Hub de eventos do Azure. Os Event hubs ativar a integração com Splunk ou outros sistemas SIEM. Para obter mais informações sobre esta opção, consulte o artigo [transmitir os registos de diagnóstico para um hub de eventos](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - **Enviar ao Log Analytics**: escreve registos para o serviço de análise de registos do Azure OMS. Para obter mais informações sobre esta opção, consulte o artigo [recolher registos de utilização na análise de registos](../log-analytics/log-analytics-azure-storage.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Para simular um ataque DDoS para validar o registo, consulte [validar DDoS deteção](#validate-ddos-detection).

## <a name="use-ddos-protection-telemetry"></a>Utilize a telemetria de proteção DDoS

Um ataque a telemetria é fornecida por meio do Monitor do Azure em tempo real. A telemetria está disponível apenas para a duração que é um endereço IP público em mitigação. Não pode ver a telemetria antes ou depois de um ataque é atenuado.

1. Selecione **todos os serviços** na parte superior, à esquerda do portal.
2. Introduza *Monitor* no **filtro** caixa. Quando **Monitor** aparece nos resultados, selecionados-lo.
3. Selecione **métricas**, em **serviços PARTILHADOS**.
4. Selecione o **subscrição** e **grupo de recursos** que contém o endereço IP público que pretende que a telemetria.
5. Selecione **endereço IP público** para **tipo de recurso**, em seguida, selecione o endereço IP público específico que pretende que a telemetria.
6. Uma série de **as métricas disponíveis** aparecem no lado esquerdo do ecrã. Estas métricas, quando selecionado, são graphed no **gráfico de métricas de Monitor de Azure** no ecrã descrição geral.

Os nomes de métricos apresentam tipos de pacote diferente e bytes vs pacotes, com uma construção básica dos nomes de etiqueta em cada métrica da seguinte forma:

- **O nome da etiqueta dropped** (por exemplo, **DDoS de ignorados de pacotes de entrada**): O número de pacotes ignorados/removida pelo sistema de proteção DDoS.
- **O nome da etiqueta forwarded** (por exemplo **entrada pacotes reencaminhados DDoS**): O número de pacotes reencaminhados pelo sistema DDoS para o VIP – tráfego que não foi filtrado de destino.
- **Nenhum nome de etiqueta** (por exemplo **DDoS de pacotes de entrada**): O número total de pacotes fornecido para o sistema limpeza – que representa a soma de pacotes ignorados e reencaminhados.

Para simular um ataque DDoS para validar a telemetria, consulte [validar DDoS deteção](#validate-ddos-detection).

## <a name="view-ddos-mitigation-policies"></a>Ver as políticas de mitigação DDoS

Padrão de proteção DDoS aplica-se três políticas de mitigação otimizados automaticamente (TCP SIN, TCP e UDP) para cada endereço IP público do recurso protegido, na rede virtual que tenha DDoS ativada. Pode ver os limiares de política, selecionando o **pacotes de TCP de entrada para acionar DDoS mitigação** e **pacotes UDP de entrada para acionar DDoS mitigação** métricas, conforme mostrado na imagem seguinte:

![Políticas de mitigação de vista](./media/manage-ddos-protection/view-mitigation-policies.png)

Limiares de política são automaticamente configurados através do Azure machine learning com base na rede tráfego a criação de perfis. Apenas quando o limiar de política é quebrado o DDoS mitigação ocorre para o endereço IP sob ataque.

## <a name="validate-ddos-detection"></a>Validar a deteção de DDoS

Microsoft tem parcerias com [BreakingPoint nuvem](https://www.ixiacom.com/products/breakingpoint-cloud) para criar uma interface onde pode gerar tráfego contra ativada para proteção DDoS os endereços IP públicos para simulações. A simulação de ponto de interrupção nuvem permite-lhe:

- Validar como proteção do Microsoft Azure DDoS protege os recursos do Azure contra ataques de DDoS
- Otimizar o seu processo de resposta a incidentes enquanto sob ataque DDoS
- Compatibilidade de DDoS do documento
- Preparar as equipas de segurança de rede

## <a name="permissions"></a>Permissões

Para trabalhar com planos de proteção DDoS, deve ser atribuída à conta para o [contribuinte de rede](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) função ou a um [personalizado](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) função atribuída as ações adequadas listadas na seguinte tabela:

| Ação                                            | Nome                                     |
| ---------                                         | -------------                            |
| Microsoft.Network/ddosProtectionPlans/read        | Ler um plano de proteção DDoS              |
| Microsoft.Network/ddosProtectionPlans/write       | Criar ou atualizar um plano de proteção DDoS  |
| Microsoft.Network/ddosProtectionPlans/delete      | Eliminar um plano de proteção DDoS            |
| Microsoft.Network/ddosProtectionPlans/join/action | Associar um plano de proteção DDoS              |

Para ativar a proteção DDoS para uma rede virtual, deve também ser atribuída à conta adequada [ações para as redes virtuais](manage-virtual-network.md#permissions).

## <a name="next-steps"></a>Passos Seguintes

- Criar e aplicar [política do Azure](policy-samples.md) para redes virtuais