---
title: Monitorizar e resolver problemas do Azure Site Recovery | Documentos da Microsoft
description: Monitorizar e resolver problemas de replicação do Azure Site Recovery e operações com o portal
services: site-recovery
documentationcenter: ''
author: bsiva
manager: abhemraj
editor: raynew
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/06/2018
ms.author: bsiva
ms.openlocfilehash: 84b5bf3be09083a69216802fc7f557de1a7f0ee6
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37917538"
---
# <a name="monitoring-and-troubleshooting-azure-site-recovery"></a>Monitorando e solucionando problemas do Azure Site Recovery

Neste artigo, saiba como utilizar o Azure Site Recovery em funcionalidades de monitorização incorporadas para monitorização e resolução de problemas. Aprenda a:
> [!div class="checklist"]
> - Utilize o dashboard do Azure Site Recovery (página de descrição geral do cofre)
> - Monitorizar e resolver problemas de replicação
> - Monitor do Azure Site Recovery/operações de tarefas
> - Subscrever notificações de e-mail

## <a name="using-the-azure-site-recovery-dashboard"></a>Utilizar o dashboard do Azure Site Recovery

O dashboard do Azure Site Recovery na página de descrição geral do cofre consolida todas as informações de monitorização para o Cofre numa única localização. Comece no dashboard do cofre e aprofunde-se para obter mais detalhes ao navegar pelas partes do dashboard. As principais partes do dashboard do Azure Site Recovery são os seguintes:

### <a name="1-switch-between-azure-backup-and-azure-site-recovery-dashboards"></a>1. Alternar entre a cópia de segurança do Azure e o Azure Site Recovery dashboards

O comutador de estado na parte superior da página de descrição geral permite alternar entre as páginas de dashboard para o Site Recovery e a cópia de segurança. Esta seleção, uma vez feita, é memorizada e predefinida na próxima vez que abrir a página de descrição geral do cofre. Selecione a opção de recuperação de sites para ver o dashboard do Site Recovery. 

As várias partes da página do dashboard do Azure Site Recovery atualizar automaticamente a cada 10 minutos, para que o dashboard reflete as informações mais recentes disponíveis.

![Funcionalidades de monitorização na página de descrição geral do Azure Site Recovery](media/site-recovery-monitor-and-troubleshoot/site-recovery-overview-page.png)

### <a name="2-replicated-items"></a>2. Itens replicados

A secção de itens replicados do dashboard apresenta uma visão geral do Estado de funcionamento de replicação de servidores protegidos no cofre. 

<table>
<tr>
    <td>Bom estado de funcionamento</td>
    <td>A replicação está a progredir normalmente para estes servidores e nenhum erro ou sintomas de aviso foram detetados.</td>
</tr>
<tr>
    <td>Aviso </td>
    <td>Foi detetado um ou mais sintomas de aviso que podem afetar a replicação ou indicar que a replicação não está a progredir normalmente para esses servidores.</td>
</tr>
<tr>
    <td>Crítica</td>
    <td>Foram detetados um ou mais sintomas de erro de replicação crítica para esses servidores. Esses sintomas de erro, normalmente, são indicadores de que a replicação está bloqueada ou ou não está a progredir o mais rápido que os dados alterados taxa para esses servidores.</td>
</tr>
<tr>
    <td>Não aplicável</td>
    <td>Servidores que não são atualmente esperados para replicar, tais como servidores que têm falhados.</td>
</tr>
</table>

Para ver uma lista de servidores protegidos filtrado por Estado de funcionamento da replicação, clique a descrição de estado de funcionamento de replicação ao lado de anel. A vista que todos ligar perto o título de secção é um atalho para a página de itens replicados para o cofre. Utilize a vista que todos ligação para ver a lista de todos os servidores no cofre.

### <a name="3-failover-test-success"></a>3. Êxito do teste de ativação pós-falha

A secção de sucesso de teste de ativação pós-falha do dashboard apresenta uma quebra de segurança de máquinas virtuais no cofre com base no estado de ativação pós-falha de teste. 

<table>
<tr>
    <td>Teste recomendado</td>
    <td>Máquinas virtuais que não tenham tido uma ativação pós-falha de teste desde que o atingiram um estado protegido.</td>
</tr>
<tr>
    <td>Executada com êxito</td>
    <td>Máquinas virtuais que tenha tido um ou mais ativações pós-falha de teste com êxito.</td>
</tr>
<tr>
    <td>Não aplicável</td>
    <td>Máquinas virtuais que não são atualmente elegíveis para uma ativação pós-falha de teste. Os exemplos são: efetuar a ativação pós-falha de servidores, servidores para que a replicação inicial está em curso, servidores para os quais uma ativação pós-falha está em curso, servidores para os quais uma ativação pós-falha de teste já está em curso.</td>
</tr>
</table>

Clique com o estado de ativação pós-falha de teste ao lado de anel, para ver a lista de servidores protegidos com base no respetivo estado de ativação pós-falha de teste.
 
> [!IMPORTANT]
> Como melhor prática, recomenda-se que efetue uma ativação pós-falha de teste nos seus servidores protegidos pelo menos uma vez a cada seis meses. Executar uma ativação pós-falha de teste é uma forma não disruptivas de ativação pós-falha das suas aplicações para um ambiente isolado e servidores de teste e ajuda-o a avaliar a preparação de continuidade de negócio.

 Uma operação de ativação pós-falha de teste num servidor ou um plano de recuperação é considerada concluída com êxito apenas depois da operação de ativação pós-falha de teste e a operação de ativação pós-falha de teste de limpeza foram concluídas com êxito.

### <a name="4-configuration-issues"></a>4. Problemas de configuração

A secção de problemas de configuração mostra uma lista dos problemas que possam afetar sua capacidade de máquinas de virtuais de ativação pós-falha com êxito. As classes de problemas indicados nesta secção são:
 - **Em falta configurações:** em falta as configurações necessárias, como uma rede de recuperação ou um grupo de recursos de recuperação de servidores protegidos.
 - **Recursos em falta:** configurados os recursos de destino/recuperação não foi encontrados ou não está disponíveis na subscrição. Por exemplo, o recurso foi eliminado ou foi migrado para uma subscrição diferente ou um grupo de recursos. As seguintes configurações de destino/recuperação são monitorizadas para disponibilidade: grupo de recursos de destino, o destino virtual de rede e sub-rede, a conta de armazenamento de registo/destino, destino conjunto de disponibilidade, o endereço IP de destino.
 - **Quota de subscrição:** o saldo de quota de recursos de subscrição disponíveis é comparado com o equilíbrio necessário para conseguir a ativação pós-falha de todas as máquinas virtuais no cofre. Se o saldo disponível for encontrado insuficiente, é comunicado o saldo de quota insuficiente. As quotas para os seguintes recursos do Azure são monitorizadas: número de núcleos de Máquina Virtual, a contagem de núcleos de família de máquina virtual, a contagem de cartões (NIC) de interface de rede.
 - **Atualizações de software:** a disponibilidade de novas atualizações de software, as versões de software a expirar.

Problemas de configuração (que não seja a disponibilidade de atualizações de software), são detetados por uma operação de validador periódica que é executado a cada 12 horas por predefinição. Pode forçar a operação de validador executar imediatamente ao clicar no ícone de atualização junto a *problemas de configuração* cabeçalho de secção.

Clique nas hiperligações para obter mais detalhes sobre os problemas listados e as máquinas virtuais afetadas por eles. Para problemas que afetam o máquinas virtuais específicas, pode obter mais detalhes clicando a **necessita de atenção** ligação na coluna de configurações de destino para a máquina virtual. Os detalhes incluem recomendações sobre como pode corrigir os problemas detetados.

### <a name="5-error-summary"></a>5. Resumo do erro

A seção de resumo de erro, mostra os sintomas de erro de replicação ativo no momento que possam afetar a replicação de servidores no cofre, juntamente com o número de entidades afetados devido a cada erro.

Os sintomas de erro de replicação para servidores num Estado de funcionamento da replicação crítica ou de aviso podem ser vistos na tabela de resumo de erros. 

- Erros que afetam o componentes de infraestrutura no local, como o não-recebimento de um heartbeat a partir do Azure Site Recovery Provider em execução no servidor de configuração no local, o servidor VMM ou anfitrião Hyper-V estão listados no início do resumo de erros secção
- Os sintomas de erro de replicação protegido a afetar servidores está listado em seguida. As entradas de tabela de resumo de erro são ordenadas por ordem decrescente de gravidade do erro e, em seguida, por ordem decrescente de contagem de servidores afetados.
 

> [!NOTE]
> 
>  Vários sintomas de erro de replicação podem ser observados num único servidor. Se existirem vários sintomas de erro num único servidor cada sintoma erro contabilizaria nesse servidor na lista de seus servidores afetados. Assim que o problema subjacente, resultando numa sintoma de erro é resolvido, melhorar a parâmetros de replicação e o erro seja limpa da máquina virtual.
>
> > [!TIP]
> A contagem de servidores afetados é uma forma útil para compreender se um único problema subjacente, poderá estar a afetar vários servidores. Por exemplo, uma falha de rede pode afetar todos os servidores de replicar a partir de um site no local para o Azure. Esta vista transmite rapidamente essa correção que um problema subjacente corrigirá a replicação para vários servidores.
>

### <a name="6-infrastructure-view"></a>6. Vista de infraestrutura

A vista de infraestrutura fornece uma cenário wise representação visual dos componentes de infraestrutura envolvidos na replicação. Ele também visualmente ilustra o estado de funcionamento da conetividade entre os vários servidores e entre os servidores e os serviços do Azure envolvido na replicação. 

Uma linha verde indica que a ligação está em bom estado, enquanto uma linha vermelha com o ícone de erro por cima indica a existência de um ou mais sintomas de erro a afetar a conectividade entre os componentes envolvidos. Passar o ponteiro do mouse sobre o ícone de erro na linha mostra o erro e o número de entidades afetados. 

Ao clicar no ícone de erro mostra uma lista filtrada de entidades afetadas para o erro (s).

![Vista de infraestrutura de recuperação do site (cofre)](media/site-recovery-monitor-and-troubleshoot/site-recovery-vault-infra-view.png)

> [!TIP]
> Certifique-se de que os componentes de infraestrutura no local (servidor de configuração, servidores de processos adicionais, replicar máquinas virtuais VMware, anfitriões Hyper-V, servidores VMM) estão a executar a versão mais recente do software Azure Site Recovery. Para poder utilizar todas as funcionalidades da vista de infraestrutura, tem de estar em execução [Update rollup 22](https://support.microsoft.com/help/4072852) ou posterior para o Azure Site Recovery

Para utilizar a vista de infraestrutura, selecione o cenário de replicação apropriado (máquinas virtuais do Azure, servidor físico/máquinas virtuais de VMware ou Hyper-V), dependendo do seu ambiente de origem. A vista de infraestrutura apresentada na página de descrição geral do Cofre é uma exibição agregada para o cofre. Pode desagregar para baixo mais detalhadamente os componentes individuais ao clicar nas caixas de.

Uma vista de infraestrutura confinada ao contexto de uma única máquina de replicação está disponível na página de descrição geral do item replicado. Para ir para a página de descrição geral para um servidor de replicação, vá para itens replicados no menu do cofre e selecione o servidor para ver os detalhes para.

### <a name="infrastructure-view---faq"></a>Vista de infraestrutura - FAQ

**P.** Por que não estou a ver a vista de infraestrutura para a minha VM? </br>
**R.** A funcionalidade de vista de infraestrutura só está disponível para máquinas virtuais que estão a replicar para o Azure. A funcionalidade não está atualmente disponível para máquinas virtuais que estão a replicar entre sites no local.

**P.** Por que a contagem de máquinas virtuais na vista de infraestrutura de cofre difere da contagem total é mostrada no anel itens replicados?</br>
**R.** A vista de infraestrutura do cofre tem um âmbito por cenários de replicação. Apenas as máquinas virtuais a participar no cenário de replicação atualmente selecionado estão incluídas na contagem de máquinas virtuais, mostrados na exibição de infraestrutura. Além disso, para o cenário selecionado, apenas as máquinas virtuais que estão atualmente configuradas para replicar para o Azure estão incluídas na contagem de máquinas virtuais, mostrados na exibição de infraestrutura (exemplo de Fo: efetuar a ativação pós-falha de máquinas virtuais, fazer cópias de máquinas virtuais a replicar para um site no local não estão incluídos na vista de infraestrutura.)

**P.** Por que é a contagem de itens replicados, mostrado na gaveta de essentials na página de descrição geral difere da contagem total de itens replicados mostrado no gráfico em anel no dashboard?</br>
**R.** Apenas as máquinas virtuais para que a replicação inicial foi concluída, estão incluídas na contagem mostrada na gaveta de essentials. Anel itens replicados total inclui todas as máquinas virtuais no cofre, incluindo servidores para que a replicação inicial está atualmente em curso.

**P.** Quais cenários de replicação está disponível para a vista de infraestrutura? </br>
**R.**
>[!div class="mx-tdBreakAll"]
>|Cenário de replicação  | Estado da VM  | Vista de infraestrutura disponível  |
>|---------|---------|---------|
>|Máquinas virtuais a replicar entre dois sites no local     | -        | Não      |
>|Todos     | A ativação pós-falha         |  Não       |
>|Máquinas virtuais a replicar entre duas regiões do Azure     | Replicação inicial em progresso ou protegidos         | Sim         |
>|Máquinas de virtuais de VMware replicar para o Azure     | Replicação inicial em progresso ou protegidos        | Sim        |
>|Máquinas de virtuais de VMware replicar para o Azure     | Efetuar a ativação pós-falha de máquinas virtuais que está a ser replicadas para um Site de VMware no local         | Não        |
>|Máquinas de virtuais de Hyper-V replicar para o Azure     | Replicação inicial em progresso ou protegidos        | Sim       |
>|Máquinas de virtuais de Hyper-V replicar para o Azure     | A ativação pós-falha / reativação pós-falha em curso        |  Não       |


### <a name="7-recovery-plans"></a>7. Planos de recuperação

A secção de planos de recuperação mostra a contagem de planos de recuperação no cofre. Clique no número para ver a lista de planos de recuperação, criar novos planos de recuperação ou editar as já existentes. 

### <a name="8-jobs"></a>8. Tarefas

Tarefas do Azure Site Recovery rastrear o status de operações do Azure Site Recovery. A maioria das operações no Azure Site Recovery são executadas de forma assíncrona, com uma tarefa de controlo a ser utilizada para controlar o progresso da operação.  Para saber como monitorizar o estado de uma operação, consulte a [Monitor do Azure Site Recovery tarefas/operações](#monitor-azure-site-recovery-jobsoperations) secção.

Esta secção de tarefas do dashboard fornece as seguintes informações:

<table>
<tr>
    <td>Com Falhas</td>
    <td>Falha de tarefas do Azure Site Recovery nas últimas 24 horas</td>
</tr>
<tr>
    <td>Em Curso</td>
    <td>Tarefas de recuperação de Site do Azure que estão atualmente em curso</td>
</tr>
<tr>
    <td>À espera de entrada</td>
    <td>Tarefas de recuperação de Site do Azure que estão atualmente em pausa a aguardar a entrada do usuário.</td>
</tr>
</table>

A ligação ver tudo junto ao cabeçalho de secção é um atalho para ir para a página de lista de tarefas.

## <a name="monitor-and-troubleshoot-replication-issues"></a>Monitorizar e resolver problemas de replicação

Além das informações disponíveis na página de dashboard do cofre, pode obter detalhes adicionais e informações de resolução de problemas na página de lista de máquinas virtuais e a página de detalhes da máquina virtual. Pode ver a lista de máquinas virtuais protegidas no cofre, selecionando o **itens replicados** opção no menu do cofre. Em alternativa, pode obter uma lista filtrada dos itens protegidos ao clicar em qualquer um dos atalhos âmbito disponíveis na página de dashboard do cofre.

![Recuperação de site replicados a vista de lista de itens](media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-list-view.png)

A opção de filtro na página de lista de itens replicados permite-lhe aplicar vários filtros, como o estado de funcionamento de replicação e a política de replicação. 

A opção de Seletor de coluna permite-lhe especificar colunas adicionais a serem apresentados como RPO, problemas de configuração de destino e erros de replicação. Pode iniciar operações numa máquina virtual ou ver erros com impacto na máquina virtual clicando numa linha específica da lista de máquinas.

Para desagregar ainda mais, selecione uma máquina virtual ao clicar no mesmo. Esta ação abre a página de detalhes da máquina virtual. A página de descrição geral em detalhes da máquina virtual contém um dashboard onde encontrará informações adicionais relativas à máquina. 

Na página de descrição geral para o computador de replicação, encontrará:
- RPO (objetivo de ponto de recuperação): RPO atual para a máquina virtual e a hora em que o RPO pela última vez foi computado.
- Pontos de recuperação disponível mais recente para a máquina
- Problemas de configuração se qualquer um que pode afetar a preparação de ativação pós-falha da máquina. Clique na ligação para obter mais detalhes.
- Detalhes do erro: lista de sintomas de erro de replicação atualmente observado na máquina, juntamente com as causas possíveis e recomendado remediações
- Eventos: Uma lista cronológica dos eventos recentes que afetem a máquina. Apesar de detalhes do erro mostra os sintomas de erro atualmente observable na máquina, eventos é um registo histórico de vários eventos que pode ter afetado o computador, incluindo os sintomas de erro que podem ter sido observados anteriormente para a máquina.
- Vista de infraestrutura para máquinas replicadas para o Azure

![Recuperação de site replicados item detalhes/descrição geral](media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-details.png)

O menu de ação na parte superior da página fornece opções para executar várias operações, como testar a ativação pós-falha na máquina virtual. O botão de detalhes do erro no menu ação permite-lhe ver todos os erros atualmente ativos, incluindo erros de replicação, problemas de configuração e avisos de melhores práticas com base em configuração para a máquina virtual.

> [!TIP]
> Como é o objetivo de ponto de RPO ou de recuperação diferente a partir do ponto de recuperação mais recentes disponíveis?
> 
>O Azure Site Recovery utiliza um processo assíncrono do passo de várias para replicar máquinas virtuais para o Azure. No passo a mais de replicação, as alterações recentes na máquina virtual juntamente com metadados são copiadas para uma conta de armazenamento de registo/cache. Assim que estas alterações, juntamente com a etiqueta para identificar um ponto de recuperável foi escrito para a conta de armazenamento na região de destino, o Azure Site Recovery tem as informações necessárias para gerar um ponto de recuperável para a máquina virtual. Neste momento, foi alcançado o RPO para que as alterações carregadas para a conta de armazenamento até agora. Em outras palavras, o RPO para a máquina virtual neste momento expressa em unidades de tempo, é igual ao período de tempo decorrido desde o carimbo de hora correspondente para o ponto de recuperável.
>
>O serviço Azure Site Recovery, a funcionar em segundo plano, seleciona os dados carregados da conta de armazenamento e aplica-as para os discos de réplica criados para a máquina virtual. Em seguida, gera um ponto de recuperação e faz com que este ponto disponíveis para recuperação a ativação pós-falha. O ponto de recuperação disponível mais recente indica o carimbo de hora correspondente para o ponto de recuperação mais recente que já tenha sido processado e aplicado para os discos de réplica.
>> [!WARNING]
> Um relógio distorcido ou a hora do sistema incorreto no computador de origem de replicação ou os servidores de infraestrutura no local irá inclinar o valor RPO calculado. Para garantir a criação de relatórios precisos de RPO valores, certifique-se de que o relógio do sistema nos servidores envolvidos na replicação está correta. 
>

## <a name="monitor-azure-site-recovery-jobsoperations"></a>Monitor do Azure Site Recovery/operações de tarefas

O Azure Site Recovery executa as operações que especificar de forma assíncrona. Exemplos de operações que pode executar são ativar a replicação, a criar plano de recuperação, a ativação pós-falha de teste, a atualizar definições de replicação etc. Toda essa operação tem uma tarefa correspondente, que é criada para controlar e auditar a operação. O objeto de tarefa tem todas as informações necessárias, necessárias controlar o estado e o progresso da operação. Pode controlar o estado das várias operações de recuperação de Site para o Cofre da página de tarefas. 

Para ver a lista de tarefas do Site Recovery do cofre, aceda a **monitorização e relatórios** secção do menu do cofre e selecionadas tarefas > tarefas do Site Recovery. Selecione uma tarefa na lista de tarefas na página clicando nele para obter mais detalhes sobre o trabalho especificado. Se uma tarefa ainda não foi concluída com êxito ou se tem erros, pode ver mais informações sobre correções de erros e possíveis ao clicar no botão de detalhes do erro na parte superior da página de detalhes de tarefa (também acessível a partir da página de lista de tarefas clicando com o sem êxito no tarefa.) Pode utilizar a opção de filtro no menu de ação no topo da página de lista de tarefas para filtrar a lista com base em critérios específicos e utilize o botão de exportação para exportar detalhes das tarefas selecionadas para o excel. Também pode acessar a vista de lista de tarefas a partir do atalho disponível na página do dashboard de recuperação de sites. 

 Para operações que executar a partir do portal do Azure, a tarefa de criação e o respetivo estado atual podem também ser controlados da secção de notificações (o ícone de sino na parte superior direita) do portal do Azure.

## <a name="subscribe-to-email-notifications"></a>Subscrever notificações de e-mail

A funcionalidade de notificação de e-mail incorporados permite-lhe subscrever a receção de notificações por e-mail para eventos críticos. Se inscrito, notificações de e-mail são enviadas para os seguintes eventos:
- Estado de funcionamento de replicação do computador de replicação degradar para crítico.
- Sem conectividade entre os componentes de infraestrutura no local e o serviço Azure Site Recovery. Conectividade para o serviço de componentes de infraestrutura no local como o servidor de configuração (VMware) ou o System Center Virtual Machine Manager(Hyper-V) registado no cofre do Site Recovery é detetada utilizando um mecanismo de heartbeat.
- Falhas de operação de ativação pós-falha se existirem.

Para subscrever para receber notificações de e-mail para o Azure Site Recovery, aceda a **monitorização e relatórios** secção do menu do cofre e:
1. Selecione os alertas e eventos > eventos de recuperação de sites.
2. Selecione "Notificações de E-Mail" no menu na parte superior da página de eventos que é aberta.
3. Utilize o Assistente de notificações de e-mail para ativar ou desativar notificações por e-mail e para selecionar destinatários de notificações. Pode especificar que todos os administradores da subscrição sejam enviadas notificações, e/ou fornecer uma lista de endereços de e-mail para enviar notificações para. 
