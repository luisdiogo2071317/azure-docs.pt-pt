---
title: Monitorizar e resolver problemas do Azure Site Recovery | Microsoft Docs
description: "Monitorizar e resolver problemas de replicação do Azure Site Recovery e operações através do portal"
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhemraj
editor: raynew
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 01/06/2017
ms.author: bsiva
ms.openlocfilehash: 6dca032c8611ac4f7e66eb6f7e22e53f49209143
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="monitoring-and-troubleshooting-azure-site-recovery"></a>Monitorização e resolução de problemas do Azure Site Recovery

Neste artigo, irá aprender a utilizar o Azure Site Recovery em incorporado funcionalidades de monitorização para monitorização e resolução de problemas. Aprenda a:
> [!div class="checklist"]
> - Utilize o dashboard do Azure Site Recovery (página de descrição geral do cofre)
> - Monitorizar e resolver problemas de replicação
> - Monitor do Azure Site Recovery tarefas/operações
> - Subscrever notificações de e-mail

## <a name="using-the-azure-site-recovery-dashboard"></a>Utilizar o dashboard do Azure Site Recovery

O dashboard do Azure Site Recovery na página de descrição geral do cofre consolida todas as informações de monitorização para o Cofre numa única localização. Iniciar o dashboard do cofre e uma descrição mais aprofundada para obter mais detalhes por navegar as partes do dashboard. As principais partes do dashboard do Azure Site Recovery são os seguintes:

### <a name="1-switch-between-azure-backup-and-azure-site-recovery-dashboards"></a>1. Alternar entre a cópia de segurança do Azure e o Azure Site Recovery dashboards

O comutador de alternar no topo da página de descrição geral permite-lhe alternar entre as páginas do dashboard para a recuperação de Site e cópia de segurança. Esta seleção, uma vez efetuada, é memorizada e predefinida para a próxima vez que abre a página de descrição geral do cofre. Selecione a opção de recuperação de sites para ver o dashboard de recuperação de sites. 

As várias partes da página do dashboard do Azure Site Recovery atualizar automaticamente a cada 10 minutos, para que o dashboard reflete as informações mais recentes.

![Funcionalidades de monitorização na página de descrição geral do Azure Site Recovery](media/site-recovery-monitor-and-troubleshoot/site-recovery-overview-page.png)

### <a name="2-replicated-items"></a>2. Itens replicados

A secção de itens replicados do dashboard apresenta uma descrição geral do Estado de funcionamento de replicação dos servidores protegidos no cofre. 

<table>
<tr>
    <td>Bom estado de funcionamento</td>
    <td>A replicação está normalmente evoluir para estes servidores e nenhum erro ou sintomas aviso foram detetadas.</td>
</tr>
<tr>
    <td>Aviso </td>
    <td>Foi detetado um ou mais sintomas de aviso que podem ter impacto na replicação, ou indicar que a replicação não está a evoluir normalmente para esses servidores.</td>
</tr>
<tr>
    <td>Crítico</td>
    <td>Foram detetadas um ou mais sintomas de erro de replicação crítica para esses servidores. Estes sintomas erro normalmente são indicadores que a replicação está está bloqueada ou não está a evoluir rápido como as alterações dos dados taxa para esses servidores.</td>
</tr>
<tr>
    <td>Não aplicável</td>
    <td>Servidores que atualmente não são esperados para replicar, tais como servidores que foi efetuadas a ativação pós-falha.</td>
</tr>
</table>

Para ver uma lista de servidores protegidos filtrados por Estado de funcionamento de replicação, clique a descrição de estado de funcionamento de replicação junto do anel. A vista de que todos os ligação perto o título de secção é um atalho para a página de itens replicados para o cofre. Utilize a vista de que todos os ligação para ver a lista de todos os servidores no cofre.

### <a name="3-failover-test-success"></a>3. Sucesso de teste de ativação pós-falha

A secção de êxito de teste de ativação pós-falha do dashboard apresenta uma quebra de cópia de segurança de máquinas virtuais no cofre com base no estado de ativação pós-falha de teste. 

<table>
<tr>
    <td>Teste recomendado</td>
    <td>Máquinas virtuais que não tenha sido uma ativação pós-falha de teste com êxito desde a hora atingiu um estado protegido.</td>
</tr>
<tr>
    <td>Efetuada com êxito</td>
    <td>Máquinas virtuais que tenham um ou mais ativações pós-falha de teste com êxito.</td>
</tr>
<tr>
    <td>Não aplicável</td>
    <td>Máquinas virtuais que não são atualmente elegíveis para uma ativação pós-falha de teste. Os exemplos são: executar a ativação pós-falha de servidores, servidores, para que a replicação inicial está em curso, servidores para o qual uma ativação pós-falha está em curso, servidores para o qual uma ativação pós-falha de teste já está em curso.</td>
</tr>
</table>

Clique no estado de ativação pós-falha de teste junto do anel, por isso, ver a lista de servidores protegidos com base no respetivo estado de ativação pós-falha de teste.
 
> [!IMPORTANT]
> Como melhor prática, recomenda-se que efetue uma ativação pós-falha de teste em servidores protegidos pelo menos uma vez a cada seis meses. Efetuar uma ativação pós-falha de teste é uma forma não acontece para testar a ativação pós-falha dos seus servidores e aplicações a um ambiente isolado e ajuda a avaliar o seu preparedness de continuidade do negócio.

 Uma operação de ativação pós-falha de teste num servidor ou um plano de recuperação é considerada concluída com êxito apenas depois da operação de ativação pós-falha de teste e a operação de ativação pós-falha de teste de limpeza foram concluídos com êxito.

### <a name="4-configuration-issues"></a>4. Problemas de configuração

A secção de problemas de configuração mostra uma lista dos problemas que possam afetar a capacidade de máquinas de virtuais de ativação pós-falha com êxito. As classes de problemas indicados nesta secção são:
 - **Falta configurações:** protegidos em falta necessárias configurações, como uma rede de recuperação ou a um grupo de recursos de recuperação de servidores.
 - **Falta de recursos:** configurados os recursos de destino/recuperação não foi encontrados ou não está disponíveis na subscrição. Por exemplo, o recurso foi eliminado ou foi migrado para uma subscrição diferente ou o grupo de recursos. As seguintes configurações de destino/recuperação são monitorizadas para disponibilidade: grupo de recursos de destino, o destino virtual rede e sub-rede, a conta de armazenamento de registo/de destino, destino conjunto de disponibilidade, o endereço IP de destino.
 - **Quota de subscrição:** o equilíbrio de quota de recursos de subscrição disponível é comparado com o equilíbrio necessário para conseguir a ativação pós-falha de todas as máquinas virtuais no cofre. Se encontra-se o equilíbrio disponível insuficiente, é comunicado o saldo de quota suficiente. Quotas para os seguintes recursos do Azure são monitorizadas: contagem de núcleos de Máquina Virtual, a contagem de núcleos família de máquina virtual, a contagem de cartão (NIC) de interface de rede.
 - **As atualizações de software:** a disponibilidade de novas atualizações de software, irá expirar versões de software.

Problemas de configuração (que não seja de disponibilidade de atualizações de software), são detetados por uma operação de validador periódica que é executado a cada 12 horas por predefinição. Pode forçar a operação de validação para executar imediatamente clicando no ícone de atualização junto a *problemas Configuration* cabeçalho da secção.

Clique nas hiperligações para obter mais detalhes sobre as máquinas virtuais afetadas por-los e os problemas listados. Para problemas causar impacto nas máquinas virtuais específicas, pode obter mais detalhes clicando a **necessita de atenção** ligação sob a coluna de configurações de destino para a máquina virtual. Os detalhes incluem recomendações sobre como pode remediar problemas detetados.

### <a name="5-error-summary"></a>5. Resumo de erros

A secção de resumo de erro, mostra os sintomas de erro de replicação atualmente ativos podem ter impacto na replicação de servidores no cofre, juntamente com o número de entidades afetados devido a cada erro.

Os sintomas de erro de replicação para servidores num Estado de funcionamento crítico ou de aviso de replicação podem ser vistos na tabela de resumo de erro. 

- Erros afetar os componentes de infraestrutura no local como não receção de um heartbeat do fornecedor do Azure Site Recovery em execução no servidor de configuração no local, o servidor VMM ou anfitrião Hyper-V estão listados no início do erro resumo secção
- Sintomas de erro de replicação protegido a afetar servidores está listado em seguida. As entradas de tabela de resumo de erro são ordenadas por diminuir a ordem da gravidade de erro e, em seguida, ao diminuir a ordem da contagem de servidores afetados.
 

> [!NOTE]
> 
>  Vários sintomas de erro de replicação podem ser respeitadas num único servidor. Se existirem vários sintomas de erro num único servidor sintoma cada erro contabilizaria esse servidor na lista dos seus servidores afetados. Depois de corrigido o problema subjacente, resultando numa sintoma de erro, parâmetros de replicação melhorar e o erro está desmarcado da máquina virtual.
>
> > [!TIP]
> A contagem dos servidores afetados é uma forma útil para entender se um único problema subjacente pode estar a afetar vários servidores. Por exemplo, uma falha de rede pode afetar todos os servidores de replicar a partir de um site no local para o Azure. Esta vista rapidamente transmite esse corrigir que um problema subjacente irá corrigir a replicação para vários servidores.
>

### <a name="6-infrastructure-view"></a>6. Vista de infraestrutura

A vista de infraestrutura fornece uma cenário aconselhado representação visual dos componentes de infraestrutura envolvidos na replicação. -Ilustra também visualmente o estado de funcionamento da conetividade entre os vários servidores e entre os servidores e os serviços do Azure envolvido na replicação. 

Uma linha verde indica que a ligação está em bom funcionamento, enquanto uma linha com o ícone de erro overlaid vermelha indica a existência de um ou mais sintomas erro afetar a conectividade entre os componentes envolvidos. Posicionado o ponteiro do rato sobre o ícone de erro na linha mostra o erro e o número de entidades afetados. 

Clicar no ícone de erro mostra uma lista filtrada de entidades afetadas para os erros.

![Vista de infraestrutura de recuperação do site (cofre)](media/site-recovery-monitor-and-troubleshoot/site-recovery-vault-infra-view.png)

> [!TIP]
> Certifique-se de que os componentes de infraestrutura no local (servidor de configuração, mais servidores de processos, replicar máquinas virtuais VMware, anfitriões Hyper-V, servidores do VMM) estiver a executar a versão mais recente do software do Azure Site Recovery. Para poder utilizar todas as funcionalidades da vista de infraestrutura, tem de estar em execução [Update rollup 22](https://support.microsoft.com/help/4072852) ou posterior para o Azure Site Recovery

Para utilizar a vista de infraestrutura, selecione o cenário de replicação adequado (máquinas virtuais do Azure, o VMware máquinas virtual/física server ou Hyper-V), dependendo do ambiente de origem. A vista de infraestrutura apresentada na página de descrição geral do Cofre é uma vista de agregados do cofre. Pode desagregar ainda mais para baixo para os componentes individuais ao clicar nas caixas.

Uma vista de infraestrutura incluída contexto de uma única máquina replicação está disponível na página de descrição geral de itens replicados. Ir para a página de descrição geral de um servidor de replicação, aceda a itens replicados no menu do cofre e selecione o servidor para ver os detalhes para.

### <a name="infrastructure-view---faq"></a>Vista de infraestrutura - FAQ

**P.** Por que razão estou não a ver a vista de infraestrutura para a minha VM? </br>
**R.** A funcionalidade de vista de infraestrutura só está disponível para máquinas virtuais que estão a ser replicada para o Azure. A funcionalidade não está atualmente disponível para máquinas virtuais que estão a replicar entre sites no local.

**P.** Por que razão a contagem de máquinas virtuais na vista de infraestrutura de cofre diferente do que a contagem total é apresentada no anel de itens replicados?</br>
**R.** A vista de infraestrutura do Cofre é confinada pelos cenários de replicação. Apenas as máquinas virtuais que participam no cenário de replicação atualmente selecionadas estão incluídas na contagem de máquinas virtuais apresentadas na vista de infraestrutura. Além disso, para o cenário selecionado, apenas as máquinas virtuais que estão atualmente configuradas para replicar para o Azure são incluídas a contagem de máquinas virtuais apresentadas na vista de infraestrutura (para por exemplo: máquinas virtuais, máquinas virtuais replicar a ativação pós-falha para um site no local não estão incluídos na vista de infraestrutura.)

**P.** Por que motivo é a contagem de itens replicados mostrado na gaveta do essentials, na página de descrição geral diferente da contagem total de itens replicados apresentado no gráfico anel no dashboard?</br>
**R.** Apenas as máquinas virtuais para que a replicação inicial foi concluída estão incluídas na contagem de mostrado na gaveta de essentials. O anel de itens replicados total inclui todas as máquinas virtuais no cofre, incluindo servidores para que a replicação inicial está em curso.

**P.** Os cenários de replicação está disponível para a vista de infraestrutura? </br>
**R.**
>[!div class="mx-tdBreakAll"]
>|Cenário de replicação  | Estado de VM  | Vista de infraestrutura disponível  |
>|---------|---------|---------|
>|Máquinas virtuais a replicar entre dois sites no local     | -        | Não      |
>|Todos     | A ativação pós-falha         |  Não       |
>|Máquinas virtuais a replicar entre dois regiões do Azure     | Replicação inicial em curso ou protegidos         | Sim         |
>|Máquinas virtuais VMware replicar para o Azure     | Replicação inicial em curso ou protegidos        | Sim        |
>|Máquinas virtuais VMware replicar para o Azure     | Falha na ativação pós-falha de máquinas virtuais que está a ser replicadas para um Site de VMware no local         | Não        |
>|Máquinas virtuais de Hyper-V replicar para o Azure     | Replicação inicial em curso ou protegidos        | Sim       |
>|Máquinas virtuais de Hyper-V replicar para o Azure     | A ativação pós-falha / reativação pós-falha em curso        |  Não       |


### <a name="7-recovery-plans"></a>7. Planos de recuperação

A secção de planos de recuperação mostra a contagem de planos de recuperação no cofre. Clique no número para ver a lista de planos de recuperação, crie novos planos de recuperação ou editar relações existentes. 

### <a name="8-jobs"></a>8. Tarefas

Tarefas de recuperação de Site do Azure controlam o estado das operações do Azure Site Recovery. A maioria das operações no Azure Site Recovery são executadas de forma assíncrona, com uma tarefa de controlo que está a ser utilizada para controlar o progresso da operação.  Para saber como monitorizar o estado de uma operação, consulte o [Monitor do Azure Site Recovery tarefas/operações](#monitor-azure-site-recovery-jobsoperations) secção.

Esta secção de tarefas do dashboard fornece as seguintes informações:

<table>
<tr>
    <td>Com Falhas</td>
    <td>Tarefas do Azure Site Recovery falhadas nas últimas 24 horas</td>
</tr>
<tr>
    <td>Em Curso</td>
    <td>Tarefas de recuperação de sites do Azure que estão atualmente em curso</td>
</tr>
<tr>
    <td>À espera de entrada</td>
    <td>Tarefas de recuperação de Site do Azure que estão atualmente em pausa a aguardar que uma entrada do utilizador.</td>
</tr>
</table>

A hiperligação ver tudo junto do cabeçalho da secção é um atalho para ir para a página de lista de tarefas.

## <a name="monitor-and-troubleshoot-replication-issues"></a>Monitorizar e resolver problemas de replicação

Além das informações disponíveis na página do dashboard do cofre, pode obter mais detalhes e informações de resolução de problemas na página de lista de máquinas virtuais e a página de detalhes da máquina virtual. Pode ver a lista de máquinas virtuais protegidas no cofre, selecionando o **replicado itens** opção do menu do cofre. Em alternativa, pode obter a uma lista filtrada de itens protegidos ao clicar em qualquer um dos atalhos âmbito disponíveis na página do dashboard do cofre.

![Recuperação de site replicados vista de lista de itens](media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-list-view.png)

A opção de filtro na página de lista de itens replicados permite-lhe aplicar vários filtros, tais como estado de funcionamento de replicação e a política de replicação. 

A opção de Seletor de coluna permite-lhe especificar mais colunas a apresentar como RPO, problemas de configuração de destino e erros de replicação. Pode iniciar a operações de uma máquina virtual ou de erros do Vista com impacto na máquina virtual ao clicar numa linha específica da lista de máquinas.

Para desagregar ainda mais, selecione uma máquina virtual clicando nela. Esta ação abre a página de detalhes da máquina virtual. A página de descrição geral em detalhes da máquina virtual contém um dashboard onde irá encontrar informações adicionais relativas à máquina. 

Na página Descrição geral da máquina de replicação, encontrará:
- O RPO (objetivo de ponto de recuperação): RPO atual para a máquina virtual e a hora em que o RPO foi calculado pela última vez.
- Pontos de recuperação disponível mais recentes para a máquina
- Problemas de configuração se qualquer que podem afetar a preparação da ativação pós-falha da máquina. Clique na ligação para obter mais detalhes.
- Detalhes do erro: a lista dos sintomas de erro de replicação atualmente observados na máquina juntamente com as causas possíveis e recomendado remediações
- Eventos: Uma lista cronológica de eventos recentes afetar a máquina. Apesar de detalhes do erro mostra os sintomas atualmente observable erro na máquina, eventos é um registo histórico de vários eventos que poderão ter afetadas da máquina, incluindo sintomas de erro que é podem que tenha anteriormente foi reparadas para a máquina.
- Vista de infraestrutura para máquinas a replicar para o Azure

![Recuperação de site replicados item detalhes/descrição geral](media/site-recovery-monitor-and-troubleshoot/site-recovery-virtual-machine-details.png)

Menu ação na parte superior da página fornece opções para desempenhar várias operações como testar a ativação pós-falha na máquina virtual. Botão de detalhes do erro no menu ação permite-lhe ver todos os erros atualmente ativos, incluindo erros de replicação, problemas de configuração e avisos de melhores práticas com base em configuração da máquina virtual.

> [!TIP]
> Como objetivo de ponto de RPO ou de recuperação é diferente do ponto de recuperação mais recente disponível?
> 
>O Azure Site Recovery utiliza um processo assíncrona do passo de várias replicar máquinas virtuais no Azure. No passo penultimate da replicação, informações sobre alterações recentes na máquina virtual, juntamente com os metadados são copiadas para uma conta de armazenamento de cache/registo. Depois destas alterações, juntamente com a etiqueta para identificar um ponto de recuperável foi escrita para a conta do storage na região de destino, o Azure Site Recovery tem as informações necessárias para gerar um recuperável ponto para a máquina virtual. Neste momento, o RPO tem foram cumprido para que as alterações carregadas, por conseguinte, até que ponto para a conta de armazenamento. Por outras palavras, o RPO para a máquina virtual neste momento expresso em unidades de tempo, é igual ao período de tempo decorrido de timestamp correspondente para o ponto de recuperável.
>
>O serviço Azure Site Recovery, a funcionar em segundo plano, seleciona os dados carregados da conta do storage e aplica-as para os discos de réplica criados para a máquina virtual. Em seguida, gera um ponto de recuperação e disponibiliza este ponto de recuperação na ativação pós-falha. O ponto de recuperação disponível mais recente indica timestamp correspondente para o ponto de recuperação mais recente que já foram processado e aplicado aos discos de réplica.
>> [!WARNING]
> Um relógio distorcido ou a hora do sistema incorretos da máquina de origem de replicação ou os servidores de infraestrutura no local irá prejudicar o valor RPO calculado. Para garantir a relatórios precisos do RPO valores, certifique-se de que o relógio do sistema nos servidores envolvidos na replicação está correta. 
>

## <a name="monitor-azure-site-recovery-jobsoperations"></a>Monitor do Azure Site Recovery tarefas/operações

O Azure Site Recovery executa as operações que especificar no modo assíncrono. Os exemplos de operações que pode executar ativar a replicação, criar plano de recuperação, ativação pós-falha de teste, atualizar definições de replicação etc. Cada operação essa tem uma tarefa correspondente que é criada para controlar e auditar a operação. O objeto de tarefa tem todas as informações necessárias para monitorizar o estado e o progresso da operação. Pode controlar o estado das várias operações de recuperação de Site para o Cofre da página de tarefas. 

Para ver a lista de tarefas de recuperação de Site para o cofre, aceda a **monitorização e relatórios** secção do menu do cofre e selecionadas tarefas > tarefas de recuperação de Site. Selecione uma tarefa da lista de tarefas na página, ao clicar na mesma para obter mais detalhes sobre a tarefa especificada. Se uma tarefa ainda não foi concluída com êxito ou se tem erros, pode ver informações adicionais na remediação de erros e possíveis ao clicar no botão de detalhes de erro no topo da página de detalhes da tarefa (também é acessível a partir da página de lista de tarefas ao clicar no êxito tarefa.) Pode utilizar a opção de filtro no menu ação no topo da página de lista de tarefas para filtrar a lista com base em critérios específicos e utilize o botão de exportação para exportar os detalhes das tarefas selecionadas para o excel. Também pode aceder a vista de lista de tarefas do atalho disponível na página do dashboard de recuperação de sites. 

 Também podem ser controlados da secção de notificações (o ícone de sino na parte superior direita) do portal do Azure de operações que são efetuadas no portal do Azure, a tarefa de criação e o respetivo estado atual.

## <a name="subscribe-to-email-notifications"></a>Subscrever notificações de e-mail

A funcionalidade de notificação de correio eletrónico criada permite-lhe subscrever a receção de notificações por e-mail para eventos críticos. Se subscrito, são enviadas notificações por e-mail para os seguintes eventos:
- Estado de funcionamento de replicação de uma máquina replicação degradar para crítico.
- Sem conectividade entre os componentes de infraestrutura no local e o serviço do Azure Site Recovery. Conetividade para o serviço de componentes de infraestrutura no local, como o servidor de configuração (VMware) ou o System Center Virtual Machine Manager(Hyper-V) registado no cofre do Site Recovery é detetada utilizando um mecanismo de heartbeat.
- Falhas de operação de ativação pós-falha se existirem.

Para subscrever a receção de notificações por e-mail para o Azure Site Recovery, aceda a **monitorização e relatórios** secção do menu do cofre e:
1. Selecione os eventos e alertas > eventos de recuperação de sites.
2. Selecione "Notificações por correio eletrónico" no menu na parte superior da página de eventos que é aberta.
3. Utilize o Assistente de notificação de e-mail para ativar ou desativar notificações por e-mail e para selecionar destinatários de notificações. Pode especificar que todos os administradores da subscrição enviadas notificações, e/ou fornecer uma lista de endereços de e-mail para enviar notificações para. 