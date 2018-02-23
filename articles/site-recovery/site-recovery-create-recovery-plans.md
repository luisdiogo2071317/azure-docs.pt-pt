---
title: "Criar e personalizar um plano de recuperação para ativação pós-falha e recuperação no Azure Site Recovery | Microsoft Docs"
description: "Saiba como criar e personalizar planos de recuperação no Azure Site Recovery. Este artigo descreve como efetuar a ativação pós-falha e recuperação VMs e servidores físicos."
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
ms.openlocfilehash: 6ad82a8a2f8e16ab794ba90c109904bd45cb6b89
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-recovery-plan-by-using-site-recovery"></a>Criar um plano de recuperação utilizando a recuperação de Site

Este artigo descreve como criar e personalizar um plano de recuperação no [do Azure Site Recovery](site-recovery-overview.md).

Crie um plano de recuperação para fazer o seguinte:

* Defina grupos de computadores que efetuam a ativação pós-falha em conjunto e, em seguida, iniciar em conjunto.
* Modelo dependências entre máquinas por agrupá-los em conjunto num grupo de plano de recuperação. Por exemplo, para efetuar a ativação pós-falha e trazer uma aplicação específica, inclua todas as VMs para essa aplicação no mesmo grupo de plano de recuperação.
* Execute uma ativação pós-falha. Pode executar um teste de ativação pós-falha planeada, ou a ativação pós-falha não planeada num plano de recuperação.

## <a name="why-use-a-recovery-plan"></a>Porquê utilizar um plano de recuperação

Um plano de recuperação pode ajudar a preparar para um processo de recuperação systematic através da criação de pequenas unidades independentes que pode gerir. As unidades normalmente representam uma aplicação no seu ambiente. Um plano de recuperação pode ajudar a definir a sequência na qual as VMs iniciar. Também pode utilizar um plano de recuperação para automatizar tarefas comuns durante a recuperação.

> [!TIP]
> Uma forma de verificar se estão preparados para a recuperação de migração ou desastre de nuvem é Certifique-se de que cada uma das suas aplicações faz parte de um plano de recuperação. Além disso, certifique-se de que cada plano de recuperação é testado para a recuperação do Azure. Com esta preparedness confidencialidade pode migrar ou efetuar a ativação pós-falha do seu centro de dados concluído para o Azure.
 
Um plano de recuperação tem três propostas de valor da chave:

* Uma aplicação para capturar as dependências de modelo.
* Automatizar a maioria das tarefas de recuperação para reduzir o RTO.
* Testar a ativação pós-falha para estar preparado para um desastre.

### <a name="model-an-application-to-capture-dependencies"></a>Uma aplicação para capturar as dependências de modelo

Um plano de recuperação é um grupo de máquinas virtuais que compõem, geralmente, uma aplicação e que efetuar a ativação pós-falha em conjunto. Utilizar plano de recuperação constrói, pode melhorar a um grupo do plano de recuperação para capturar as propriedades específicas da aplicação. 

Neste artigo, utilizamos uma aplicação de três camadas típica pode ter um back-end, middleware e um front-end da web SQL. Pode personalizar o plano de recuperação para ajudar a garantir que as máquinas virtuais iniciam na ordem correta, após uma ativação pós-falha. O SQL Server de back-end deverá iniciar primeiro, o middleware deve começar seguinte e o front-end da web deverá começar último. Esta ordem garante que a aplicação está a funcionar dentro do tempo a última máquina virtual é iniciado. 

Por exemplo, quando é iniciado o middleware, este tenta ligar para o escalão SQL. O plano de recuperação garante que a camada de SQL Server já está em execução. Ter um início de servidor front-end pela última vez também ajuda a garantir que os utilizadores finais não ligue para o URL da aplicação antes de todos os componentes estão operacionais e em execução e a aplicação está pronta para aceitar pedidos. Para criar estas dependências, personalizar o plano de recuperação para adicionar grupos e, em seguida, selecione uma máquina virtual. Para mover uma máquina virtual entre grupos, altere o grupo da máquina virtual.

![Captura de ecrã de um plano de recuperação de exemplo no Site Recovery](./media/site-recovery-create-recovery-plans/rp.png)

Depois de concluir a personalização, pode visualizar os passos exatos para a recuperação. Segue-se a ordem dos passos que são executados durante a ativação pós-falha de um plano de recuperação:

1. Um passo de encerramento tenta desativar a máquinas virtuais no local. (Excepto numa ativação pós-falha de teste, durante o qual o site primário tem de continuar a ser executada).
2. A tentativa de encerramento aciona uma ativação pós-falha de todas as máquinas virtuais no plano de recuperação em paralelo. O passo de ativação pós-falha prepara discos da máquina virtual utilizando dados replicados.
3. Os grupos de arranque executar, por ordem e iniciar as máquinas virtuais em cada grupo. Em primeiro lugar, grupo 1 executa, em seguida, grupo 2 executa e por fim, grupo 3 executa. Se existir mais do que uma máquina virtual em qualquer grupo (por exemplo, um com balanceamento de carga web front-end), todas as máquinas virtuais iniciada em paralelo.

> [!TIP]
> Sequenciação em grupos de assegura que as dependências entre várias camadas da aplicação são cumpridas. Paralelismo, apropriado a utilizar, melhora o RTO de recuperação de aplicações.

   > [!NOTE]
   > As máquinas que fazem parte de um único grupo com a ativação pós-falha em paralelo. Máquinas que fazem parte de diferentes grupos falharem por ordem de grupos. As máquinas do grupo 2 iniciam a respetiva ativação pós-falha depois de todas as máquinas do grupo 1 tenham a ativação pós-falha e tenha iniciado.

### <a name="automate-most-recovery-tasks-to-reduce-rto"></a>Automatizar a maioria das tarefas de recuperação para reduzir o RTO

Recuperar grande aplicações pode ser uma tarefa complexa. Ter muitos passos manuais para Lembre-se em tempos de chaos é difícil e torna o processo suscetível a erros. Além disso, poderá ser alguém que não tem conhecimento do intricacies a aplicação que é acionado, na verdade, a ativação pós-falha. 

Pode utilizar um plano de recuperação para automatizar as ações necessárias que precisa de tomar em cada passo. Pode configurar as ações necessárias utilizando runbooks de automatização do Azure. Com runbooks, pode automatizar as tarefas de recuperação comuns, como as tarefas nos exemplos seguintes. Para tarefas que não podem ser automatizadas, pode inserir ações manuais os planos de recuperação.

* **Tarefas a pós-falha máquina virtual do Azure**: estas tarefas normalmente têm de conseguir estabelecer ligação à máquina virtual. Exemplos:
    * Crie um endereço IP público na pós-falha da máquina virtual.
    * Atribua um grupo de segurança de rede para o NIC na máquina virtual efetuada a ativação pós-falha.
    * Adicione um balanceador de carga para um conjunto de disponibilidade.
* **Tarefas dentro de pós-falha da máquina virtual**: estas tarefas reconfigurar a aplicação para que este continua a funcionar corretamente no ambiente de novo. Exemplos:
    * Modificar a cadeia de ligação de base de dados dentro da máquina virtual.
    * Altere a configuração de servidor web ou regras.

> [!TIP]
> Alcançar a ativação pós-falha de um clique e otimizar RTO através da criação de um plano de recuperação completa que automatiza as tarefas de pós-recuperação utilizando runbooks de automatização.

### <a name="test-failover-to-be-ready-for-a-disaster"></a>Testar a ativação pós-falha para estar preparado para um desastre

Pode utilizar um plano de recuperação para acionar uma ativação pós-falha ou uma ativação pós-falha de teste. Sempre conclua uma ativação pós-falha de teste da aplicação antes de efetuar uma ativação pós-falha. Ajuda de ativações pós-falha de teste, verifique se a aplicação é apresentada no site de recuperação. Caso não tenham ocorrido algo no seu programa de configuração, pode facilmente acionar a limpeza e, em seguida, repita a ativação pós-falha de teste. Efetue ativação pós-falha de teste várias vezes, até que tem a certeza de que a aplicação recupera facilmente.

![Captura de ecrã de um exemplo de teste plano de recuperação no Site Recovery](./media/site-recovery-create-recovery-plans/rptest.png)

> [!TIP]
> Porque cada aplicação seja exclusiva, terá de criar planos de recuperação que são personalizados para cada aplicação. 
>
> Além disso, num ambiente dinâmico, concentra-se do Centro de dados atuais, aplicações e as respetivas dependências mudam frequentemente. Para garantir que o plano de recuperação atual, ativação pós-falha de teste para as suas aplicações cada trimestre.

## <a name="create-a-recovery-plan"></a>Criar um plano de recuperação

1. Selecione **planos de recuperação** > **criar plano de recuperação**.
   Especifique um nome para o plano de recuperação e uma origem e de destino. A localização de origem tem de ter as máquinas virtuais que estão ativadas para ativação pós-falha e recuperação. Escolha uma origem e de destino com base nas máquinas virtuais que pretende fazer parte do plano de recuperação. 

   |Cenário                   |Origem               |Destino           |
   |---------------------------|---------------------|-----------------|
   |Azure para o Azure             |Região do Azure         |Região do Azure     |
   |VMware para o Azure            |Servidor de configuração |Azure            |
   |O Virtual Machine Manager (VMM) para o Azure               |Nome a apresentar do VMM    |Azure            |
   |Site Hyper-V para o Azure      |Nome de site Hyper-V    |Azure            |
   |Máquinas físicas no Azure |Servidor de configuração |Azure            |
   |VMM para o VMM                 |Nome amigável do VMM    |Nome a apresentar do VMM|

   > [!NOTE]
   > Um plano de recuperação pode conter máquinas virtuais que tenham a mesma origem e destino. Máquinas virtuais VMware e System Center Virtual Machine Manager (VMM) não podem coexistir no mesmo plano de recuperação. No entanto, pode adicionar máquinas virtuais VMware e máquinas físicas para o mesmo plano de recuperação. Neste caso, a origem para máquinas é um servidor de configuração.

2. Em **selecionar máquinas virtuais**, selecione as máquinas virtuais (ou grupo de replicação) que pretende adicionar ao grupo predefinido (grupo 1) no plano de recuperação. Só pode selecionar máquinas virtuais que foram protegidos na origem (como selecionado no plano de recuperação) e que estão protegidos no destino (como selecionado no plano de recuperação).

## <a name="customize-and-extend-recovery-plans"></a>Personalizar e expandir os planos de recuperação

Para personalizar e expandir os planos de recuperação, vá para Painel de recursos do plano de recuperação de Site Recovery. Selecione o **personalizar** separador. Pode personalizar e expandir os planos de recuperação utilizando as seguintes opções:

- **Adicionar novos grupos**: pode adicionar até sete grupos de plano de recuperação adicionais para o grupo predefinido. Em seguida, pode adicionar mais computadores ou grupos de replicação a esses grupos do plano de recuperação. Grupos são numerados pela ordem na qual pode adiciona. Pode incluir uma máquina virtual ou grupo de replicação, no grupo de apenas um plano de recuperação.
- **Adicionar uma ação manual**: pode adicionar ações manuais que são executados antes ou depois de um grupo do plano de recuperação. Quando executa o plano de recuperação, interrompe o momento em que a ação manual que inserido. Uma caixa de diálogo pede-lhe para especificar que a ação manual foi concluída.
- **Adicionar um script**: pode adicionar scripts que são executados antes ou depois de um grupo do plano de recuperação. Quando adiciona um script, adiciona um novo conjunto de ações para o grupo. Por exemplo, é criado um conjunto de pré-passos de para o grupo 1 com o nome *grupo 1: pré-passos*. Todos os pré-passos de estão listados dentro deste conjunto. Pode adicionar um script no site primário, apenas se tiver um servidor VMM implementado. Para obter mais informações, consulte [adicionar um script do VMM para um plano de recuperação](site-recovery-how-to-add-vmmscript.md).
- **Adicionar os runbooks do Azure**: pode expandir a planos de recuperação utilizando os runbooks do Azure. Por exemplo, pode utilizar um runbook para automatizar tarefas ou criar recuperação único passo. Para obter mais informações, consulte [runbooks de automatização do Azure de adicionar a planos de recuperação](site-recovery-runbook-automation.md).


## <a name="add-a-script-runbook-or-manual-action-to-a-plan"></a>Adicionar um script, o runbook ou a ação manual para um plano

Depois de adicionar as máquinas virtuais ou grupos de replicação para um grupo predefinido do plano de recuperação, pode adicionar um script ou a ação manual ao grupo do plano de recuperação.

1. Abra o plano de recuperação.
2. No **passo** lista, selecione um item. Em seguida, selecione **Script** ou **ação Manual**.
3. Especifique se pretende adicionar o script ou ação antes ou depois do item selecionado. Para mover a posição do script ou reduzir verticalmente, selecione o **mover para cima** ou **mover para baixo** botões.
4. Se adicionar um script do VMM, selecione **ativação pós-falha para o script VMM**. No **caminho do Script**, introduza o caminho relativo para a partilha. Por exemplo, **\RPScripts\RPScript.PS1**.
5. Se adicionar um runbook da automatização, especifique a conta de automatização em que o runbook está localizado. Em seguida, selecione o script de runbook do Azure.
6. Para garantir que o script funciona conforme esperado, efetue uma ativação pós-falha do plano de recuperação.

Opções de script ou um runbook estão disponíveis apenas nos seguintes cenários e durante uma ativação pós-falha ou a reativação pós-falha. Uma ação manual está disponível para ativação pós-falha e a reativação pós-falha.


|Cenário               |Ativação pós-falha |Reativação pós-falha |
|-----------------------|---------|---------|
|Azure para o Azure         |Runbook |Runbook  |
|VMware para o Azure        |Runbook |ND       | 
|VMM para o Azure           |Runbook |Script   |
|Site Hyper-V para o Azure  |Runbook |ND       |
|VMM para o VMM             |Script   |Script   |


## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [executar as ativações pós-falha](site-recovery-failover.md).  
* Para ver a recuperação planear em ação, consulte este vídeo:
    
    > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]
