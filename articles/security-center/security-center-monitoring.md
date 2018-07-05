---
title: Monitorização de segurança no Centro de Segurança do Azure | Microsoft Docs
description: Este artigo ajuda-o a começar a trabalhar com as capacidades de monitorização no Centro de Segurança do Azure.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2018
ms.author: terrylan
ms.openlocfilehash: 434b73d4625f86fab195dbda1fed9c841791f5b6
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37099464"
---
# <a name="security-health-monitoring-in-azure-security-center"></a>Monitorização de estado de funcionamento de segurança no Centro de Segurança do Azure
Este artigo ajuda-o a utilizar as capacidades de monitorização no Centro de Segurança do Azure para monitorizar a conformidade com políticas.

## <a name="what-is-security-health-monitoring"></a>O que é a monitorização do estado de funcionamento de segurança?
Pensamos frequentemente que monitorizar é observar e esperar que um evento ocorra, para que possamos reagir à situação. A monitorização de segurança refere-se ao facto de ter uma estratégia proativa que audita os seus recursos para identificar os sistemas que não cumprem as normas ou melhores práticas organizacionais.

## <a name="monitoring-security-health"></a>Monitorizar o estado de funcionamento de segurança
Depois de ativar [políticas de segurança](security-center-policies.md) para os recursos de uma subscrição, o Centro de Segurança irá analisar a segurança dos seus recursos para identificar potenciais vulnerabilidades. As informações sobre a configuração da rede estão disponíveis de forma instantânea. Consoante o número de VMs e de computadores em que tenha o agente instalado, a recolha das informações sobre a configuração das VMs e dos computadores, como, por exemplo, o estado de atualizações de segurança e a configuração do sistema operativo, pode demorar uma hora ou mais a ser disponibilizada. Pode ver o estado de segurança dos seus recursos, juntamente com quaisquer problemas na secção **Prevenção**. Também pode ver uma lista dos problemas no mosaico **Recomendações**.

Para obter mais informações sobre como aplicar recomendações, leia [Implementing security recommendations in Azure Security Center (Implementar recomendações de segurança no Centro de Segurança do Azure)](security-center-recommendations.md).

Em **Monitorização do estado de funcionamento do recurso**, pode monitorizar o estado de segurança dos seus recursos. No exemplo seguinte, pode ver que em cada mosaico do recurso (Computação e Aplicações, Rede, Segurança de dados e Identidade e acesso) tem o número total de problemas que foram identificados.

![Mosaico de estado de funcionamento da segurança dos recursos](./media/security-center-monitoring/security-center-monitoring-fig1-newUI-2017.png)


### <a name="monitor-compute--apps"></a>Monitorizar a computação e aplicações
Veja [Proteger o seus computadores e aplicações no Centro de Segurança do Azure](security-center-virtual-machine-recommendations.md) para obter mais informações.

### <a name="monitor-virtual-networks"></a>Monitorizar redes virtuais
Quando clica no mosaico **Redes**, o painel **Redes** é apresentado com mais detalhes, conforme mostrado na seguinte captura de ecrã:

![Painel Redes](./media/security-center-monitoring/security-center-monitoring-fig9-new3.png)

#### <a name="networking-recommendations"></a>Recomendações de redes
À semelhança das informações de estado de funcionamento do recurso da máquina virtual, pode ver, aqui, uma lista resumida dos problemas, na parte superior, e uma lista das redes monitorizadas, na parte inferior.

A secção da divisão de estado de funcionamento de redes apresenta uma lista de potenciais problemas de segurança e oferece [recomendações](security-center-network-recommendations.md). Os possíveis problemas podem incluir:

* Firewall da Próxima Geração (NGFW) não instalada
* Grupos de segurança de rede em sub-redes não ativados
* Grupos de segurança de rede em máquinas virtuais não ativados
* Restringir o acesso externo através do ponto final externo público
* Pontos finais com acesso à Internet com bom estado de funcionamento

Quando clica numa recomendação, pode ver mais detalhes sobre a mesma, conforme mostrado no exemplo seguinte.

![Detalhes de uma recomendação em Redes](./media/security-center-monitoring/security-center-monitoring-fig9-ga.png)

Neste exemplo, **Configurar Grupos de Segurança de Rede em Falta para Sub-redes** tem uma lista de sub-redes e máquinas virtuais nas quais está em falta a proteção de grupo de segurança de rede. Se clicar na sub-rede na qual pretende aplicar o grupo de segurança de rede, verá **Escolher grupo de segurança de rede**. Aqui, pode selecionar o grupo de segurança de rede mais adequado para a sub-rede ou criar um novo.

#### <a name="internet-facing-endpoints-section"></a>Secção pontos finais com acesso à Internet
Na secção **Pontos finais com acesso à Internet**, pode ver as máquinas virtuais que estão atualmente configuradas com um ponto final com acesso à Internet e o respetivo estado atual.

![Máquinas virtuais configuradas com estado e ponto final com acesso à Internet](./media/security-center-monitoring/security-center-monitoring-fig10-ga.png)

Esta tabela tem o nome do ponto final que representa a máquina virtual, o endereço IP com acesso à Internet, o estado de gravidade atual do grupo de segurança de rede e da NGFW. A tabela está ordenada por gravidade:

* Vermelho (na parte superior): alta prioridade e deve ser resolvido imediatamente
* Cor de laranja: prioridade média e deve ser resolvido com a maior brevidade possível
* Verde (por último): bom estado de funcionamento

#### <a name="networking-topology-section"></a>Secção Topologia de redes
Na secção **Topologia de redes**, existe uma vista hierárquica dos recursos, conforme mostrado na seguinte captura de ecrã:

![Vista hierárquica dos recursos na secção de Topologia de redes](./media/security-center-monitoring/security-center-monitoring-fig121-new4.png)

Esta tabela está ordenada (máquinas virtuais e sub-redes) por gravidade:

* Vermelho (na parte superior): alta prioridade e deve ser resolvido imediatamente
* Cor de laranja: prioridade média e deve ser resolvido com a maior brevidade possível
* Verde (por último): bom estado de funcionamento

Nesta vista de topologia, o primeiro nível tem [máquinas virtuais](../virtual-network/virtual-networks-overview.md), [gateways da rede virtual](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) e [redes virtuais (clássicas)](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). O segundo nível tem sub-redes e o terceiro nível tem as máquinas virtuais que pertencem a essas sub-redes. A coluna da direita tem o estado atual do grupo de segurança de rede para esses recursos, conforme mostrado no exemplo seguinte:

![Estado do grupo de segurança de rede na secção Topologia de redes](./media/security-center-monitoring/security-center-monitoring-fig12-ga.png)

A parte inferior deste painel tem as recomendações para esta máquina virtual, semelhantes ao que foi descrito anteriormente. Pode clicar numa recomendação para obter mais informações ou aplicar a configuração ou o controlo de segurança necessário.

### <a name="monitor-data-security"></a>Monitorizar segurança de dados

Quando clica em **Segurança de dados** na secção **Prevenção**, é aberto **Recursos de Dados** com recomendações para SQL e Armazenamento. Também tem [recomendações](security-center-sql-service-recommendations.md) para o estado de funcionamento geral da base de dados. Para obter mais informações sobre a encriptação de armazenamento, leia o artigo [Ativar a encriptação para a conta de armazenamento do Azure no Centro de Segurança do Azure](security-center-enable-encryption-for-storage-account.md).

![Recursos de Dados](./media/security-center-monitoring/security-center-monitoring-fig13-newUI-2017.png)

Em **Recomendações SQL**, pode clicar em qualquer recomendação e obter mais detalhes sobre mais ações para resolver um problema. O exemplo seguinte mostra a expansão da recomendação **Auditoria de Base de Dados e Deteção de ameaças em bases de dados SQL**.

![Detalhes sobre uma recomendação de SQL](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

**Ativar Auditoria e Deteção de ameaças nas bases de dados SQL** tem as seguintes informações:

* Uma lista de bases de dados SQL
* O servidor no qual estão localizadas
* Informações sobre se esta definição foi herdada a partir do servidor ou se é exclusiva desta base de dados
* O estado atual
* A gravidade do problema

Quando clica na base de dados para abordar esta recomendação, é aberto **Deteção de Ameaças e Auditoria**, conforme mostrado no ecrã seguinte.

![Auditoria e Deteção de Ameaças](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Para ativar a auditoria, selecione **ATIVAR**, na opção **Auditoria**.

### <a name="monitor-identity--access"></a>Monitorizar identidade e acesso

Para obter mais informações, veja [Monitorizar a identidade e o acesso no Centro de Segurança do Azure](security-center-identity-access.md).

## <a name="see-also"></a>Consulte também
Neste artigo, aprendeu a utilizar as capacidades de monitorização no Centro de Segurança do Azure. Para saber mais acerca do Centro de Segurança do Azure, consulte o seguinte:

* [Definir políticas de segurança no Centro de Segurança do Azure](security-center-policies.md): saiba como configurar definições de segurança no Centro de Segurança do Azure.
* [Gerir e responder a alertas de segurança no Centro de Segurança do Azure](security-center-managing-and-responding-alerts.md): saiba como gerir e responder a alertas de segurança.
* [Monitorizar soluções de parceiros com o Centro de Segurança do Azure](security-center-partner-solutions.md): saiba como monitorizar o estado de funcionamento das suas soluções de parceiros.
* [Azure Security Center FAQ (FAQ do Centro de Segurança do Azure)](security-center-faq.md): encontre as perguntas mais frequentes acerca de como utilizar o serviço.
* [Blogue de Segurança do Azure](http://blogs.msdn.com/b/azuresecurity/): encontre publicações no blogue acerca da segurança e conformidade do Azure.
