---
title: Recolha de dados no Centro de segurança do Azure | Microsoft Docs
description: " Saiba como ativar a recolha de dados no Centro de segurança do Azure. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/14/2018
ms.author: terrylan
ms.openlocfilehash: 847127c96f23bbeb3cf3a5d1c9768af6e0cc0dc4
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="data-collection-in-azure-security-center"></a>Recolha de dados no Centro de segurança do Azure
Centro de segurança recolhe dados de máquinas de virtuais (VMs) do Azure e computadores não do Azure para monitorizar vulnerabilidades de segurança e ameaças. Os dados são recolhidos com o Microsoft Monitoring Agent, que lê várias configurações relacionadas com segurança e registos de eventos a partir da máquina e copia os dados para a sua área de trabalho para análise. Os exemplos destes dados incluem: tipo e versão do sistema operativo, registos de sistema operativo (registos de eventos do Windows), processos em execução, nome da máquina, endereços IP, utilizador com sessão iniciada e ID do inquilino. O Microsoft Monitoring Agent também copia os ficheiros de informação de falha para a área de trabalho.

## <a name="enable-automatic-provisioning-of-microsoft-monitoring-agent"></a>Ativar aprovisionamento automático do Microsoft Monitoring Agent     
Aprovisionamento automático está desativado por predefinição. Quando o aprovisionamento automático está ativado, Aprovisiona o Centro de segurança do Microsoft Monitoring Agent em todos os suportadas VMs do Azure e quaisquer novos que são criados. Aprovisionamento Automático é vivamente recomendado, mas a instalação manual do agente também está disponível. [Saiba como instalar a extensão do Microsoft Monitoring Agent](../log-analytics/log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension).

> [!NOTE]
> Desativar o aprovisionamento automático limita a monitorização da segurança dos seus recursos. Para obter mais informações, consulte [desativar aprovisionamento automático](security-center-enable-data-collection.md#disable-automatic-provisioning) neste artigo. Coleção de artefacto e instantâneos de discos VM estão ativadas, mesmo se o aprovisionamento automático está desativado.
>
>

Para ativar o aprovisionamento automático do Microsoft Monitoring Agent:
1. No menu principal do Centro de segurança, selecione **política de segurança**.
2. Selecione uma subscrição.

  ![Selecionar subscrição][7]

3. Em **Política de segurança**, selecione **Recolha de Dados**.
4. Em **aprovisionamento automático**, selecione **no** para ativar o aprovisionamento automático.
5. Selecione **Guardar**.

  ![Ativar o aprovisionamento automático][1]

## <a name="default-workspace-configuration"></a>Configuração de área de trabalho predefinida
Dados recolhidos pelo centro de segurança são armazenados no workspace(s) de análise de registos.  Pode optar por ter dados recolhidos a partir de VMs do Azure armazenados no áreas de trabalho criadas pelo centro de segurança nem de uma área de trabalho existente que criou.

Para utilizar a sua área de trabalho de análise de registos existente:
- A área de trabalho tem de estar associada com a sua subscrição do Azure selecionada.
- No mínimo, é necessário ter leitura permissões para aceder à área de trabalho.

Para selecionar uma área de trabalho de análise de registos existente:

1. Em **configuração de área de trabalho predefinida**, selecione **utilizar outra área de trabalho**.

   ![Selecione a área de trabalho existente][2]

2. No menu pendente, selecione uma área de trabalho para armazenar os dados recolhidos.

  > [!NOTE]
  > Na solicitação menu pendente, todas as áreas de trabalho em todas as subscrições estão disponíveis. Consulte [cruzada seleção de área de trabalho de subscrição](security-center-enable-data-collection.md#cross-subscription-workspace-selection) para obter mais informações.
  >
  >

3. Selecione **Guardar**.
4. Depois de selecionar **guardar**, será pedido se gostaria de VMs de reconfigure monitorizado.

   - Selecione **não** se pretender que as novas definições de área de trabalho para aplicar novas VMs apenas. As novas definições de área de trabalho só se aplicam a novas instalações de agente; VMs detetadas recentemente que não têm o Microsoft Monitoring Agent instalada.
   - Selecione **Sim** se pretender que as novas definições de área de trabalho para aplicar a todas as VMs. Além disso, cada VM ligado a uma área de trabalho de criar o Centro de segurança é novamente ligado para a nova área de trabalho de destino.

   > [!NOTE]
   > Se selecionar Sim, não terá de eliminar o workspace(s) criados pelo centro de segurança até que todas as VMs tiverem sido sido religadas para a nova área de trabalho de destino. Esta operação irá falhar se uma área de trabalho é eliminada demasiado antigo.
   >
   >

   - Selecione **Cancelar** para cancelar a operação.

     ![Selecione a área de trabalho existente][3]

## <a name="cross-subscription-workspace-selection"></a>Cruzada seleção de área de trabalho de subscrição
Quando seleciona uma área de trabalho para armazenar os dados, todas as áreas de trabalho em todas as subscrições estão disponíveis. Subscrição cruzada seleção de área de trabalho permite-lhe recolher dados de máquinas virtuais em execução em diferentes subscrições e armazene-a na área de trabalho da sua preferência. Esta capacidade funciona para máquinas virtuais em execução no Linux e Windows.

> [!NOTE]
> Subscrição cruzada seleção de área de trabalho faz parte do escalão gratuito do Centro de segurança do Azure. Veja [Preços](security-center-pricing.md) para saber mais sobre os escalões de preços do Centro de Segurança.
>
>

## <a name="data-collection-tier"></a>Camada de recolha de dados
Centro de segurança pode reduzir o volume de eventos, mantendo em simultâneo suficiente eventos para investigação, auditoria e deteção de ameaças. Pode escolher o direito de filtragem de política para as suas subscrições e áreas de trabalho de quatro conjuntos de eventos recolhidos pelo agente.

- **Todos os eventos** – para os clientes que deve certificar-se de que todos os eventos são recolhidos. Esta é a predefinição.
- **Comuns** – este é um conjunto de eventos que satisfaça a maioria dos clientes e permite-lhes um registo de auditoria completa.
- **Mínimo** – um conjunto mais pequeno de eventos para os clientes que pretendem minimizar o volume de eventos.
- **Nenhum** – desativar a recolha de eventos de segurança de segurança e do App Locker registos. Para os clientes que escolha esta opção, os seus dashboards de segurança tem apenas registos de Firewall do Windows e avaliações proativa como antimalware, a linha de base e a atualização.

> [!NOTE]
> Estes conjuntos de eventos de segurança só estão disponíveis no escalão Standard do Centro de segurança. Veja [Preços](security-center-pricing.md) para saber mais sobre os escalões de preços do Centro de Segurança.
Estes conjuntos foram concebidos para abordar cenários típicos. Certifique-se ao avaliar quais um necessidades da sua antes de a implementar.
>
>

Para determinar os eventos que irão pertencer ao **comuns** e **mínima** conjuntos de evento, iremos trabalhou com os clientes e normas da indústria para saber mais sobre a frequência e não filtrada da respetiva utilização e cada evento. Utilizámos as seguintes diretrizes neste processo:

- **Mínimo** -Certifique-se de que este conjunto abrange apenas os eventos que possam indicar uma violação com êxito e eventos importantes que têm um volume muito baixo. Por exemplo, este conjunto contém utilizador com êxito e início de sessão (evento 4624 de IDs, 4625), mas não contém o fim de sessão que é importante para auditoria mas não significativo para deteção e tem relativamente elevado volume. A maioria do volume de dados deste conjunto é os eventos de início de sessão e os eventos de criação do processo (evento ID 4688).
- **Comuns** -forneça um registo de auditoria de utilizador completo deste conjunto. Por exemplo, este conjunto contém inícios de sessão do utilizador e de fim de sessão do utilizador (evento ID 4634). Incluímos auditoria ações como alterações ao grupo de segurança, operações de Kerberos do controlador de domínio de chave e outros eventos que são recomendados por organizações da indústria.

Eventos com um volume muito baixa foram incluídos no comuns definido como a motivação principal para escolher o ao longo de todos os eventos é para reduzir o volume e não para filtrar os eventos específicos.

Eis uma divisão de segurança e do App Locker de IDs de evento para cada conjunto concluída:

| Camada de dados | Indicadores de evento recolhidos |
| --- | --- |
| Mínimo | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Common | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,461,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

> [!NOTE]
> Se estiver a utilizar o objeto de política de grupo (GPO), recomenda-se que ative as políticas de auditoria 4688 de eventos do processo de criação e a *CommandLine* campo no interior do evento 4688. Para mais informações sobre o processo de criação de evento 4688, consulte o Centro de segurança [FAQ](security-center-faq.md#what-happens-when-data-collection-is-enabled). Para obter mais informações sobre estas políticas de auditoria, consulte [recomendações de política de auditoria](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations).
>
>

Para escolher a política de filtragem:
1. No **recolha de dados de política de segurança** painel, selecione a política de filtragem em **eventos de segurança**.
2. Selecione **Guardar**.

   ![Escolha a política de filtragem][5]

## <a name="disable-automatic-provisioning"></a>Desativar aprovisionamento automático
Pode desativar aprovisionamento automático de recursos em qualquer altura desativar esta definição na política de segurança. Aprovisionamento Automático é altamente recomendável para obter os alertas de segurança e recomendações sobre as atualizações do sistema, endpoint protection e vulnerabilidades do SO.

> [!NOTE]
> Desativar o aprovisionamento automático não remove o Microsoft Monitoring Agent das VMs do Azure onde o agente tiver sido aprovisionado.
>
>

1. Regressar ao menu principal do Centro de segurança e selecione a política de segurança.
2. Selecione a subscrição para a qual pretende desativar o aprovisionamento automático.
3. No **política de segurança – recolha de dados** painel, em **aprovisionamento automático** selecione **desativar**.
4. Selecione **Guardar**.

  ![Desativar aprovisionamento automático][6]

Quando o aprovisionamento automático está desativado (desativado), a secção de configuração de área de trabalho predefinido não é apresentado.

## <a name="next-steps"></a>Passos Seguintes
Este artigo como mostrado a recolha de dados e o aprovisionamento automático no Centro de segurança funciona. Para saber mais acerca do Centro de Segurança, consulte o seguinte:

* [Definir políticas de segurança no Centro de Segurança do Azure](security-center-policies.md) – Saiba como configurar políticas de segurança para as suas subscrições e grupos de recursos do Azure.
* [Gerir recomendações de segurança no Centro de segurança do Azure](security-center-recommendations.md) – Saiba como recomendações ajudam a proteger os seus recursos do Azure.
* [Monitorização do estado de funcionamento de segurança no Centro de Segurança do Azure](security-center-monitoring.md) – Saiba como monitorizar o estado de funcionamento dos seus recursos do Azure.
* [Gerir e responder a alertas de segurança no Centro de Segurança do Azure](security-center-managing-and-responding-alerts.md) – Saiba como gerir e responder a alertas de segurança.
* [Monitorização de soluções de parceiros com o Centro de Segurança do Azure](security-center-partner-solutions.md) – Saiba como monitorizar o estado de funcionamento das suas soluções de parceiros.
- [Segurança de dados do Centro de segurança do Azure](security-center-data-security.md) -Saiba como os dados são geridos e salvaguardados no Centro de segurança.
* [FAQ do Centro de Segurança do Azure](security-center-faq.md) – Encontre as perguntas mais frequentes acerca de como utilizar o serviço.
* [Blogue de Segurança do Azure](http://blogs.msdn.com/b/azuresecurity/) – Obtenha as notícias e informações mais recentes de segurança do Azure.

<!--Image references-->
[1]: ./media/security-center-enable-data-collection/enable-automatic-provisioning.png
[2]: ./media/security-center-enable-data-collection/use-another-workspace.png
[3]: ./media/security-center-enable-data-collection/reconfigure-monitored-vm.png
[5]: ./media/security-center-enable-data-collection/data-collection-tiers.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png
[7]: ./media/security-center-enable-data-collection/select-subscription.png
