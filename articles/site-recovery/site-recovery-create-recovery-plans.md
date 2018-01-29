---
title: "Criar e personalizar planos de recuperação para ativação pós-falha e recuperação no Azure Site Recovery | Microsoft Docs"
description: "Descreve como criar e personalizar planos de recuperação no Azure Site Recovery, para efetuar a ativação pós-falha e recuperação de VMs e servidores físicos"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 01/26/2018
ms.author: raynew
ms.openlocfilehash: 9839a989246b28c1a194b8d1f0e99c1bd80ac2e5
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="create-recovery-plans"></a>Criar planos de recuperação


Este artigo fornece informações sobre como criar e personalizar planos de recuperação no [do Azure Site Recovery](site-recovery-overview.md).

Publique comentários ou perguntas na parte inferior deste artigo ou no [Fórum dos Serviços de Recuperação do Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

 Crie planos de recuperação para fazer o seguinte:

* Defina grupos de computadores que efetuam a ativação pós-falha em conjunto e, em seguida, iniciar a cópia de segurança em conjunto.
* Modelo dependências entre máquinas, ao agrupá-los em conjunto para uma recuperação planear grupo. Por exemplo, para efetuar a ativação pós-falha e trazer uma aplicação específica, agrupa todas as VMs para essa aplicação para o mesmo grupo de plano de recuperação.
* Execute uma ativação pós-falha. Pode executar um teste de ativação pós-falha planeada, ou a ativação pós-falha não planeada num plano de recuperação.

## <a name="why-use-recovery-plans"></a>Porquê utilizar planos de recuperação?

Planos de recuperação ajudam a planear um processo de recuperação systematic através da criação de pequenas unidades independentes que pode gerir. Estas unidades, normalmente, irão representar uma aplicação no seu ambiente. Plano de recuperação não só permite-lhe definir a sequência em que as máquinas virtuais iniciar, mas também o ajuda a automatizar tarefas comuns durante a recuperação.


**Essencialmente, uma forma de verificar que está preparado para migração para a nuvem ou recuperação de desastre é, garantindo que todas as aplicações do seu faz parte de um plano de recuperação e cada um dos planos de recuperação é testada para a recuperação do Microsoft Azure. Com esta preparedness, é possível migrar confidencialidade ou ativação pós-falha seu centro de dados concluído para o Microsoft Azure.**
 
Seguem-se as três propostas de valor de chave de um plano de recuperação:

### <a name="model-an-application-to-capture-dependencies"></a>Uma aplicação para capturar as dependências de modelo

Um plano de recuperação é um grupo de máquinas virtuais, geralmente, que inclui uma aplicação que a ativação pós-falha em conjunto. O plano de recuperação a utilizar construções, pode melhorar a este grupo para capturar as propriedades específicas da aplicação.
 
Informe-na executar o exemplo de uma aplicação de três camadas típica com

* um back-end SQL
* um middleware
* um web frontend

O plano de recuperação pode ser personalizado para se certificar de que as máquinas virtuais pensar na publicação de ordem correta uma ativação pós-falha. Back-end do SQL Server deve detetar um primeiro, deve obter o middleware seguinte e o front-end da web deve detetar um último. Esta ordem torna determinada-se de que a aplicação está a funcionar dentro do tempo que é apresentada a última máquina virtual. Por exemplo, quando o middleware é apresentada, tentará ligar para o escalão SQL e o plano de recuperação tem certificar-se de que a camada de SQL Server já está em execução. Servidores front-end provenientes dos últimos também garante que os utilizadores finais não ligar ao URL da aplicação por engano até que todos os componentes estão operacionais estão a utilizar e a aplicação esteja pronta para aceitar pedidos. Para criar estas dependências, pode personalizar o plano de recuperação para adicionar grupos. Em seguida, selecione uma máquina virtual e altere o respetivo grupo de movê-lo entre grupos.

![Exemplo de plano de recuperação](./media/site-recovery-create-recovery-plans/rp.png)

Depois de concluir a personalização, pode visualizar os passos exatos para a recuperação. Segue-se a ordem dos passos executado durante a ativação pós-falha de um plano de recuperação:

* Primeiro, há um passo de encerramento tenta desativar a máquinas virtuais no local (exceto na ativação pós-falha de teste em que o site primário tem de continuar para estar em execução)
* Em seguida aciona a ativação pós-falha de todas as máquinas virtuais do plano de recuperação em paralelo. O passo de ativação pós-falha prepara discos as máquinas virtuais a partir dos dados replicados.
* Por fim, os grupos de arranque executar, por ordem, iniciar as máquinas virtuais em cada grupo - grupo 1 em primeiro lugar, em seguida, grupo 2 e, finalmente, grupo 3. Se existir mais do que um máquinas virtuais em qualquer grupo (por exemplo, com balanceamento de carga web front-end) todos eles são arranca cópias de segurança em paralelo.

**Sequenciação em grupos de assegura que as dependências entre várias camadas da aplicação são cumpridas paralelismo apropriado melhora o RTO de recuperação de aplicações.**

   > [!NOTE]
   > As máquinas que fazem parte de um único grupo efetuará a ativação pós-falha em paralelo. As máquinas que fazem parte de diferentes grupos efetuará a ativação pós-falha no oder dos grupos. Depois de todas as máquinas do grupo 1 tem a ativação pós-falha e reiniciado, as máquinas do grupo 2 iniciará respetiva ativação pós-falha.

### <a name="automate-most-recovery-tasks-to-reduce-rto"></a>Automatizar a maioria das tarefas de recuperação para reduzir o RTO

Recuperar grande aplicações pode ser uma tarefa complexa. Também é difícil lembrar-se de que os passos exatos personalização após a ativação pós-falha ou migração. Por vezes, não é, mas alguém que não tem conhecimento do intricacies aplicação, que necessita para acionar a ativação pós-falha. A memorizar o manuais demasiados em tempos de chaos é difícil e propensas ao erro. Um plano de recuperação dá-lhe uma forma de automatizar as ações necessárias, que precisa de tomar cada passo utilizando runbooks de automatização do Microsoft Azure. Com runbooks, pode automatizar tarefas de recuperação comuns, como os exemplos indicados abaixo. Para essas tarefas que não podem ser automatizadas, planos de recuperação também fornecem a capacidade de inserir ações manuais.

* Tarefas na máquina virtual do Azure após a ativação pós-falha – estes são necessários, normalmente, para que possam ligar à máquina virtual, por exemplo:
    * Criar um IP público a ativação pós-falha post de máquina virtual
    * Atribuir um NSG para a ativação pós-falha NIC da máquina virtual
    * Adicionar um balanceador de carga para um conjunto de disponibilidade
* Tarefas dentro da máquina virtual após a ativação pós-falha – estes reconfigurar a aplicação para que este continua a funcionar corretamente no novo ambiente, por exemplo:
    * Modificar a cadeia de ligação de base de dados dentro da máquina virtual
    * Alterar a configuração de servidor web, as regras

**Com um plano de recuperação completa que automatiza as tarefas de recuperação de post através de runbooks de automatização, pode realizar a ativação pós-falha de um clique e otimizar o RTO.**

### <a name="test-failover-to-be-ready-for-a-disaster"></a>Testar a ativação pós-falha para estar preparado para um desastre

Um plano de recuperação pode ser utilizado para acionar uma ativação pós-falha ou uma ativação pós-falha de teste. Deve sempre efetuar uma ativação pós-falha de teste da aplicação antes de efetuar uma ativação pós-falha. Ativação pós-falha de teste ajuda-o para verificar se a aplicação irá detetar um no site de recuperação.  Caso não tenham ocorrido algo, pode facilmente acionar a limpeza e repita a ativação pós-falha de teste. Efetue ativação pós-falha de teste várias vezes, até que conhece com certainty que a aplicação recupera facilmente.

![Plano de recuperação de teste](./media/site-recovery-create-recovery-plans/rptest.png)

**Cada aplicação é diferente e que necessita para criar planos de recuperação que são personalizados para cada. Além disso, no mundo Centro de dados dinâmico, as aplicações e as respetivas dependências mantém a alteração. Ativação pós-falha de teste as aplicações de uma vez de um trimestre para verificar se o plano de recuperação é atual.**

## <a name="how-to-create-a-recovery-plan"></a>Como criar um plano de recuperação

1. Clique em **planos de recuperação** > **criar plano de recuperação**.
   Especifique um nome para o plano de recuperação e uma origem e de destino. A localização de origem tem de ter as máquinas virtuais que estão ativadas para ativação pós-falha e recuperação. Escolha uma origem e de destino com base nas máquinas virtuais que pretende fazer parte do plano de recuperação. 

   |Cenário                   |Origem               |Alvo           |
   |---------------------------|---------------------|-----------------|
   |Azure para o Azure             |Região do Azure         |Região do Azure     |
   |VMware para Azure            |Servidor de configuração |Azure            |
   |VMM para o Azure               |Nome amigável do VMM    |Azure            |
   |Site Hyper-v para o Azure      |Nome de site Hyper-v    |Azure            |
   |Máquinas físicas no Azure |Servidor de configuração |Azure            |
   |VMM para o VMM                 |Nome amigável do VMM    |Nome amigável do VMM|

   > [!NOTE]
   > Um plano de recuperação pode conter máquinas virtuais que tenham a mesma origem e destino. Máquinas virtuais de VMware e o VMM não pode ser parte do plano de recuperação. Máquinas virtuais VMware e máquinas físicas no entanto podem ser adicionada para o mesmo plano como origem para ambas é um servidor de configuração.

2. No **selecionar máquinas virtuais**, selecione as máquinas virtuais (ou grupo de replicação) que pretende adicionar ao grupo predefinido (grupo 1) no plano de recuperação. Apenas as máquinas virtuais que foram protegidas na origem (como selecionado no plano de recuperação) e estão protegidas para o destino (como selecionado no plano de recuperação) serão permitidas para seleção.

## <a name="how-to-customize-and-extend-recovery-plans"></a>Como personalizar e expandir os planos de recuperação

Pode personalizar e expandir os planos de recuperação, avançar para o painel de recursos de plano de recuperação do Site Recovery e clicando no separador de personalizar.

Pode personalizar e expandir os planos de recuperação:

- **Adicionar novos grupos**— adicionar grupos de plano de recuperação adicionais (até sete) para o grupo predefinido e, em seguida, adicione mais computadores ou grupos de replicação a esses grupos do plano de recuperação. Grupos são numerados pela ordem na qual pode adiciona. Uma máquina virtual ou grupo de replicação, só pode ser incluído no grupo do plano de recuperação de um.
- **Adicionar uma ação manual**— pode adicionar ações manuais que são executados antes ou depois de um grupo do plano de recuperação. Quando executa o plano de recuperação, interrompe o momento em que a ação manual que inserido. Uma caixa de diálogo pede-lhe para especificar que a ação manual foi concluída.
- **Adicionar um script**— pode adicionar scripts que são executados antes ou depois de um grupo do plano de recuperação. Quando adiciona um script, adiciona um novo conjunto de ações para o grupo. Por exemplo, será criado um conjunto de pré-passos de para grupo 1 com o nome: grupo 1: pré-passos. Todos os pré-passos de estão listados dentro deste conjunto. Só é possível adicionar um script no site primário se tiver um servidor VMM implementado. [Saiba mais](site-recovery-how-to-add-vmmscript.md).
- **Adicionar os runbooks do Azure**— pode expandir a planos de recuperação com os runbooks do Azure. Por exemplo, para automatizar tarefas ou criar recuperação único passo. [Saiba mais](site-recovery-runbook-automation.md).


## <a name="how-to-add-a-script-runbook-or-manual-action-to-a-plan"></a>Como adicionar uma ação de script, o runbook ou manual para um plano

Pode adicionar um script ou a ação manual para o grupo predefinido do plano de recuperação depois de ter adicionado VMs ou grupos de replicação para a mesma e criado o plano.

1. Abra o plano de recuperação.
2. Clique num item de **passo** lista e, em seguida, clique em **Script** ou **ação Manual**.
3. Especifique se pretende adicionar o script ou ação antes ou depois do item selecionado. Para mover a posição do script ou reduzir verticalmente, utilize o **mover para cima** e **mover para baixo** botões.
4. Se adicionar um script do VMM, selecione **ativação pós-falha para o script VMM**. No **caminho do Script**, escreva o caminho relativo para a partilha. No exemplo VMM abaixo, especifique o caminho: **\RPScripts\RPScript.PS1**.
5. Se adicionar uma run book a automatização do Azure, especifique a conta de automatização do Azure no qual está localizado o runbook e selecione o script do runbook do Azure adequada.
6. Para certificar-se de que o script funciona conforme esperado, efetue uma ativação pós-falha do plano de recuperação.

As opções de script ou um runbook estão disponíveis apenas nos seguintes cenários ao efetuar uma ativação pós-falha ou a reativação pós-falha. Uma ação manual está disponível para ativação pós-falha e a reativação pós-falha.


|Cenário               |Ativação pós-falha |Reativação pós-falha |
|-----------------------|---------|---------|
|Azure para o Azure         |Runbooks |Runbook  |
|VMware para Azure        |Runbooks |ND       | 
|VMM para o Azure           |Runbooks |Script   |
|Site Hyper-v para o Azure  |Runbooks |ND       |
|VMM para o VMM             |Script   |Script   |


## <a name="next-steps"></a>Passos Seguintes

[Saiba mais](site-recovery-failover.md) sobre como executar as ativações pós-falha.

Veja este vídeo para ver a planear na ação de recuperação.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]
