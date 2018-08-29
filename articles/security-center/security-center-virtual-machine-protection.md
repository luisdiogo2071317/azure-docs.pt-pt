---
title: Proteger as máquinas e aplicações no Centro de segurança do Azure | Documentos da Microsoft
description: Este documento aborda recomendações no Centro de segurança que o ajudam a proteger as suas máquinas virtuais e computadores e suas aplicações web e ambientes de serviço de aplicações.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/28/2018
ms.author: rkarlin
ms.openlocfilehash: 7cf5f86d9a2d121ff54c40e27c6bc50847a4dfdf
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43132788"
---
# <a name="protecting-your-machines-and-applications-in-azure-security-center"></a>Proteger as máquinas e aplicações no Centro de segurança do Azure
Centro de segurança do Azure analisa o estado de segurança dos seus recursos do Azure. Quando o Centro de segurança identifica potenciais vulnerabilidades de segurança, cria recomendações que descreve o processo de configuração de controlos necessários. Recomendações se aplicam a tipos de recursos do Azure: máquinas virtuais (VMs) e computadores, aplicações, redes, SQL e identidade e acesso.

Este artigo aborda recomendações que se aplicam a máquinas e aplicações.

## <a name="monitoring-security-health"></a>Monitorizar o estado de funcionamento de segurança
Pode monitorizar o estado de segurança dos seus recursos na **Centro de segurança – descrição geral** dashboard. O **recursos** seção fornece o número de problemas identificados e o estado de segurança para cada tipo de recurso.

Pode ver uma lista de todos os problemas, selecionando **recomendações**. Para obter mais informações sobre como aplicar recomendações, veja [implementar recomendações de segurança no Centro de segurança do Azure](security-center-recommendations.md).

Para recomendações de serviços de uma lista completa de computação e de aplicação, consulte [recomendações](security-center-virtual-machine-recommendations.md).

Para continuar, selecione **computação e aplicações** sob **recursos** ou o menu principal do Centro de segurança.
![Dashboard do Centro de segurança][1]

## <a name="monitor-compute-and-app-services"></a>Serviços de computação de monitor e aplicação
Sob **computação**, existem quatro separadores:

- **Descrição geral**: monitorização e recomendações identificadas pelo centro de segurança.
- **VMs e computadores**: lista das VMs, computadores e estado de segurança atual de cada.
- **Serviços cloud**: lista de funções da web e de trabalho monitorizadas pelo centro de segurança.
- **Serviços de aplicações (pré-visualização)**: lista de seus ambientes do serviço de aplicações e o estado de segurança atual de cada.
Para continuar, selecione **computação e aplicações** sob **recursos** ou o menu principal do Centro de segurança.

![Computação][2]

Existem várias secções em cada separador e pode selecionar, em cada uma delas, uma opção individual para ver mais detalhes sobre os passos recomendados para resolver esse problema em concreto.

### <a name="monitoring-recommendations"></a>Recomendações de monitorização
Esta secção mostra o número total de VMs e computadores que foram inicializados para aprovisionamento automático e os respetivos Estados atuais. Este exemplo inclui uma recomendação, **Monitorizar problemas de estado de funcionamento do agente**. Selecione esta recomendação.

![Monitorizar problemas de estado de funcionamento do agente][3]

**Monitorizar problemas de estado de funcionamento do agente** é aberto. São listados os computadores e VMs que o Centro de Segurança não consegue monitorizar. Selecione um computador ou VM para informações detalhadas. O **ESTADO DE MONITORIZAÇÃO** fornece um motivo pelo qual o Centro de Segurança não consegue monitorizar. Consulte o [guia de resolução de problemas do Centro de Segurança](security-center-troubleshooting-guide.md) para obter uma lista de valores, descrições e passos de resolução do **ESTADO DE MONITORIZAÇÃO**.

### Computadores e VMs não monitorizadas <a name="unmonitored-vms-and-computers"></a>
Uma VM ou o computador não é monitorizado pelo centro de segurança se a máquina não está em execução a extensão do Microsoft Monitoring Agent. Um computador pode ter um agente local já instalado, por exemplo o OMS direcionar o agente ou o agente do SCOM. As máquinas com estes agentes são identificadas como não monitorizado porque estes agentes não são totalmente suportados no Centro de segurança. Para beneficiar totalmente de todas as capacidades do Centro de Segurança, é necessária a extensão MMA.

Pode instalar a extensão no computador, além do agente local já instalado ou VM não monitorizada. Configure ambos os agentes da mesma forma, ligando-os à mesma área de trabalho. Isto permite ao Centro de Segurança interagir com a extensão MMA e recolher dados. Consulte [Ativar a extensão VM](../log-analytics/log-analytics-quick-collect-azurevm.md) para obter instruções sobre como instalar a extensão MMA.

Consulte [problemas de estado do agente de monitorização](security-center-troubleshooting-guide.md#mon-agent) para saber mais sobre os motivos pelos quais o Centro de Segurança não consegue monitorizar com êxito VMs e computadores inicializados para aprovisionamento automático.

### <a name="recommendations"></a>Recomendações
Esta secção tem um conjunto de recomendações para cada VM e o computador, a funções web e de trabalho, a aplicações de Web do serviço de aplicações do Azure e o ambiente de serviço de aplicações do Azure que monitoriza o Centro de segurança. A primeira coluna indica a recomendação. A segunda coluna mostra o número total de recursos que são afetados por essa recomendação. A terceira coluna mostra a gravidade do problema, conforme ilustrado na seguinte captura de ecrã:

![Recomendações][4]

Cada recomendação tem um conjunto de ações que pode realizar depois de a selecionar. Por exemplo, se selecionar **atualizações de sistema em falta**, o número de VMs e computadores que têm patches em falta e a gravidade da atualização em falta for apresentada, conforme mostrado na captura de ecrã seguinte:

![Aplicar atualizações do Sistema][5]

**Aplicar atualizações do sistema** tem um resumo das atualizações críticas num formato de gráfico, um para Windows e outro para Linux. A segunda parte tem uma tabela com as seguintes informações:

- **NOME**: o nome da atualização em falta.
- **N.º DE VMs e COMPUTADORES**: o número total de VMs e computadores nos quais esta atualização está em falta.
- **GRAVIDADE da ATUALIZAÇÃO**: descreve a gravidade dessa recomendação específica:

    - **Crítico**: uma vulnerabilidade existe com um recurso significativo (aplicação, máquina virtual ou grupo de segurança de rede) e necessita de atenção.
    - **Importante**: passos não críticos ou adicionais são necessários para concluir um processo ou eliminar uma vulnerabilidade.
    - **Moderado**: uma vulnerabilidade deve ser resolvida, mas não necessita de atenção imediata. (Por predefinição, as recomendações baixas não são apresentadas, mas pode filtrar por recomendações baixas se pretender visualizá-las).


- **ESTADO**: o estado atual da recomendação:

    - **Aberto**: a recomendação ainda não foi tratada.
    - **Em curso**: a recomendação está a ser aplicada a esses recursos e não tem de tomar qualquer medida.
    - **Resolvido**: a recomendação já foi concluída. (Quando o problema for resolvido, a entrada fica a cinzento).

Para ver os detalhes das recomendações, clique no nome da atualização em falta na lista.

![Detalhes da recomendação][6]

> [!NOTE]
> As recomendações de segurança aqui são as mesmas que as sob o **recomendações** mosaico. Ver [implementar recomendações de segurança no Centro de segurança do Azure](security-center-recommendations.md) para obter mais informações sobre como resolver recomendações.
>
>

### <a name="vms-and-computers"></a>VMs e computadores
A secção de VMs e computadores dá-lhe uma descrição geral de todas as recomendações de VM e o computador. Cada coluna representa um conjunto de recomendações como é mostrado na seguinte captura de ecrã:

![Recomendações de VM e de computador][7]

Existem quatro tipos de ícones representados nesta lista:

![Computador não pertencente ao Azure][8] Computador não Azure.

![VM do Azure Resource Manager][9] VM do Azure Resource Manager.

![VM clássica do Azure][10] VM clássica do Azure.

![VMs identificadas a partir da área de trabalho][11] VMs que só são identificadas a partir da área de trabalho que faz parte da subscrição vista. Inclui VMs de outras subscrições que reportam à área de trabalho desta subscrição e VMs que foram instaladas com o agente direto do SCOM e não têm ID de recurso.

O ícone apresentado em cada recomendação ajuda-o a identificar rapidamente a VM e o computador que precisa de atenção e o tipo de recomendação. Também pode utilizar a opção de filtro para selecionar que opções verá neste ecrã.

![Filtro][12]

No exemplo anterior, uma VM tem uma recomendação crítica relativa à proteção de ponto final. Selecione a VM para obter mais informações sobre ele:

![Recomendação crítica][13]

Aqui verá os detalhes de segurança para o computador ou VM. Na parte inferior, pode ver a ação recomendada e a gravidade de cada problema.

### <a name="cloud-services"></a>Serviços em nuvem
Relativamente aos serviços cloud, é criada uma recomendação quando a versão do sistema operativo está desatualizada, conforme mostrado na captura de ecrã seguinte:

![Serviços em nuvem][14]

Num cenário em que tem uma recomendação (que não é o caso no exemplo anterior), tem de seguir os passos na recomendação para atualizar a versão do sistema operativo. Quando está disponível uma atualização, verá um alerta (vermelho ou cor de laranja, dependendo da gravidade do problema). Quando seleciona este alerta no WebRole1 (executa o Windows Server com a sua aplicação web automaticamente implementada no IIS) ou WorkerRole1 (executa o Windows Server com a sua aplicação web automaticamente implementada no IIS) linhas, verá mais detalhes sobre esta recomendação, conforme mostra a captura de ecrã seguinte:

![WorkerRole1][15]

Para ver uma explicação mais prescritiva sobre esta recomendação, clique em **Atualizar versão do SO** na coluna **DESCRIÇÃO**.

![Atualizar a versão do SO][16]

### <a name="app-services-preview"></a>Serviços de aplicação (Pré-visualização)

> [!NOTE]
> Monitorização do serviço de aplicações está em pré-visualização e disponível apenas no escalão Standard do Centro de segurança. Veja [Preços](security-center-pricing.md) para saber mais sobre os escalões de preços do Centro de Segurança.
>
>

Sob **dos serviços de aplicações**, encontrar uma lista dos seus ambientes de serviço de aplicações e o estado de funcionamento resumo com base na avaliação do Centro de segurança efetuada.

![Serviços aplicacionais][17]

Existem três tipos de ícones representados nesta lista:

![Ambiente de serviços de aplicações][18] Ambiente de serviços de aplicações.

![Aplicação Web][19] Aplicação Web.

![Aplicação de função][24] Aplicação de função.

1. Selecione uma aplicação web. É aberta uma vista de resumida com três separadores:

  - **Recomendações**: com base nas avaliações realizadas pelo centro de segurança que falharam.
  - **Passados avaliações**: lista de avaliações realizadas pelo centro de segurança passado.
  - **Avaliações indisponíveis**: lista de avaliações de que não foi possível executar devido a um erro ou a recomendação não é relevante para o serviço de aplicações específico

  Sob **recomendações** é uma lista das recomendações para a aplicação web selecionada e a gravidade de cada recomendação.

  ![Vista de resumo][20]

2. Selecione uma recomendação para obter uma descrição da recomendação e uma lista de recursos de mau estado de funcionamento, de recursos de bom estado de funcionamento e de recursos não verificados.

  ![Descrição da recomendação][21]

  Sob **transmitido avaliações** é uma lista de avaliações aprovadas.  Gravidade dessas avaliações é sempre verde.

  ![Avaliações aprovadas][22]

3. Selecione uma avaliação com êxito da lista para obter uma descrição da avaliação, uma lista de recursos de mau e bom estado de funcionamento e uma lista de recursos não verificados. Há uma guia para os recursos de mau estado de funcionamento, mas essa lista está sempre vazia, uma vez que a avaliação passado.

    ![Recursos de estado de funcionamento][23]



## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre as recomendações que se aplicam a outros tipos de recursos do Azure, consulte o seguinte:


* [Recomendações do Centro de segurança do Azure de compreensão para máquinas virtuais](security-center-virtual-machine-recommendations.md)
* [Monitorizar a identidade e acesso no Centro de segurança do Azure](security-center-identity-access.md)
* [Proteger a sua rede no Centro de Segurança do Azure](security-center-network-recommendations.md)
* [Proteger o seu serviço do SQL do Azure no Centro de segurança do Azure](security-center-sql-service-recommendations.md)

Para saber mais acerca do Centro de Segurança, consulte o seguinte:

* [Definir políticas de segurança no Centro de Segurança do Azure](security-center-policies.md) – Saiba como configurar políticas de segurança para as suas subscrições e grupos de recursos do Azure.
* [Gerir e responder a alertas de segurança no Centro de Segurança do Azure](security-center-managing-and-responding-alerts.md) – Saiba como gerir e responder a alertas de segurança.
* [Azure Security Center FAQ (FAQ do Centro de Segurança do Azure)](security-center-faq.md) – Encontre as perguntas mais frequentes acerca de como utilizar o serviço.

<!--Image references-->
[1]: ./media/security-center-virtual-machine-recommendations/overview.png
[2]: ./media/security-center-virtual-machine-recommendations/compute.png
[3]: ./media/security-center-virtual-machine-recommendations/monitoring-agent-health-issues.png
[4]: ./media/security-center-virtual-machine-recommendations/compute-recommendations.png
[5]: ./media/security-center-virtual-machine-recommendations/apply-system-updates.png
[6]: ./media/security-center-virtual-machine-recommendations/missing-update-details.png
[7]: ./media/security-center-virtual-machine-recommendations/vm-computers.png
[8]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png
[9]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png
[10]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png
[11]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png
[12]: ./media/security-center-virtual-machine-recommendations/filter.png
[13]: ./media/security-center-virtual-machine-recommendations/vm-detail.png
[14]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png
[15]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new3.png
[16]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new4.png
[17]: ./media/security-center-virtual-machine-recommendations/app-services.png
[18]: ./media/security-center-virtual-machine-recommendations/ase.png
[19]: ./media/security-center-virtual-machine-recommendations/web-app.png
[20]: ./media/security-center-virtual-machine-recommendations/recommendation.png
[21]: ./media/security-center-virtual-machine-recommendations/recommendation-desc.png
[22]: ./media/security-center-virtual-machine-recommendations/passed-assessment.png
[23]: ./media/security-center-virtual-machine-recommendations/healthy-resources.png
[24]: ./media/security-center-virtual-machine-recommendations/function-app.png
