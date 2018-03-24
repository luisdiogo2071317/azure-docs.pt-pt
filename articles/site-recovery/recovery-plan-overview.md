---
title: Planos de recuperação a utilizar no Azure Site Recovery | Microsoft Docs
description: Saiba mais sobre os planos de recuperação no Azure Site Recovery.
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 03/21/2018
ms.author: raynew
ms.openlocfilehash: 871c9e8404438f966cab2fc5ab782e254295569e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="about-recovery-plans"></a>Sobre os planos de recuperação

Este artigo descreve os planos de recuperação no [do Azure Site Recovery](site-recovery-overview.md).

Um plano de recuperação reúne máquinas em grupos de recuperação. Pode personalizar um plano adicionando ordem, instruções e tarefas ao mesmo. Depois de um plano estiver definido, pode executar uma ativação pós-falha na mesma.





## <a name="why-use-a-recovery-plan"></a>Porquê utilizar um plano de recuperação?

Um plano de recuperação ajuda a definir um processo de recuperação systematic, através da criação de pequenas unidades independentes que pode efetuar a ativação pós-falha. Uma unidade normalmente representa uma aplicação no seu ambiente. Um plano de recuperação define a forma como máquinas ativação pós-falha e a sequência na qual iniciar após a ativação pós-falha. Utilize planos de recuperação para:

* Modelar uma aplicação em torno as respetivas dependências.
* Automatizar tarefas de recuperação para reduzir o RTO.
- Certifique-se de que está preparado para a recuperação de migração ou um desastre, garantindo que as aplicações façam parte de um plano de recuperação.
* Executar a ativação pós-falha de teste em planos de recuperação garantir a recuperação após desastre ou migração está a funcionar conforme esperado.


## <a name="model-apps"></a>Aplicações do modelo

Pode planear e criar um grupo de recuperação para capturar as propriedades de específico da aplicação. Por exemplo, vamos considere uma aplicação de três camadas típica com um servidor SQL back-end, middleware e web front-end. Normalmente, personalizar o plano de recuperação para que as máquinas em cada camada iniciam na ordem correta após a ativação pós-falha.

    - Back-end do SQL Server deve começar em primeiro lugar, o middleware seguinte e, finalmente, o front-end da web.
    - Esta ordem de início garante que a aplicação está a funcionar dentro do tempo inicia a última máquina.
    - Esta ordem garante que quando o middleware é iniciado e tenta ligar para o escalão de SQL Server, a camada de SQL Server já está em execução. 
    - Esta ordem também ajuda a garantir que o servidor de front-end inicia última, para que os utilizadores finais não ligar para o URL da aplicação antes de todos os componentes estão operacionais e em execução e a aplicação está pronto para aceitar pedidos.

Para criar esta ordem, adicionar grupos ao grupo de recuperação e adicionar computadores para os grupos. 
    - Em que ordem for especificada, é utilizada a sequenciação. As ações executam em paralelo apropriado melhorar a recuperação de aplicações RTO.
    - Máquinas de um único grupo com a ativação pós-falha em paralelo.
    - As máquinas em diferentes grupos falharem por ordem de grupo, para que as máquinas do grupo 2 iniciam a respetiva ativação pós-falha depois de todas as máquinas em 1 grupo tenham a ativação pós-falha e tenha iniciado.

    ![Plano de recuperação de exemplo](./media/recovery-plan-overview/rp.png)

Com esta personalização no local, eis o que acontece quando executar uma ativação pós-falha no plano de recuperação: 

1. Um passo de encerramento tenta desative as máquinas no local. A exceção é se executar uma ativação pós-falha de teste, caso em que o site primário continua a ser executado. 
2. O encerramento aciona uma ativação pós-falha paralela de todas as máquinas no plano de recuperação.
3. A ativação pós-falha prepara discos da máquina virtual utilizando dados replicados.
4. Os grupos de arranque executados na ordem e iniciar as máquinas em cada grupo. Em primeiro lugar, grupo 1 é executada, em seguida, grupo 2 e, por fim, grupo 3. Se existir mais de uma máquina em qualquer grupo, em seguida, iniciam todas as máquinas em paralelo.


## <a name="automate-tasks"></a>Automatizar tarefas

Recuperar grande aplicações pode ser uma tarefa complexa. Passos manuais tornar o processo suscetível a erros e a pessoa que está a executar a ativação pós-falha possa não ter conhecimento de todas as intricacies de aplicação. Pode utilizar um plano de recuperação para impor a ordem e automatizar as ações necessárias em cada passo, através de runbooks de automatização do Azure para ativação pós-falha para o Azure ou scripts. Para tarefas que não podem ser automatizadas, pode inserir coloca em pausa para ações manuais planos de recuperação. Existem alguns dos tipos de tarefas que pode configurar:

* **Tarefas na VM do Azure após a ativação pós-falha**: quando estiver a efetuar a ativação pós-falha para o Azure, normalmente, tem de efetuar ações para que possam ligar à VM após a ativação pós-falha. Por exemplo: 
    * Crie um endereço IP público na VM do Azure.
    * Atribua um grupo de segurança de rede para o adaptador de rede da VM do Azure.
    * Adicione um balanceador de carga para um conjunto de disponibilidade.
* **Tarefas no interior da VM após a ativação pós-falha**: estas tarefas normalmente reconfigurar a aplicação em execução no computador, para que este continua a funcionar corretamente no ambiente de novo. Por exemplo:
    * Modificar a cadeia de ligação de base de dados no interior da máquina.
    * Altere a configuração de servidor web ou regras.


## <a name="test-failover"></a>Ativação pós-falha de teste

Pode utilizar um plano de recuperação para acionar uma ativação pós-falha de teste. Utilize as seguintes melhores práticas:

- Sempre conclua uma ativação pós-falha de teste numa aplicação, antes de executar uma ativação pós-falha completa. As ativações pós-falha de teste ajudá-lo a verificar se a aplicação é apresentada no site de recuperação.
- Se achar que tiver estado em falta algo, acionar um limpa cópias de segurança e, em seguida, execute novamente a ativação pós-falha de teste. 
- Execute uma ativação pós-falha de teste várias vezes, até que tenha a certeza de que a aplicação recupera facilmente.
- Porque cada aplicação seja exclusiva, terá de criar planos de recuperação que são personalizados para cada aplicação e execute uma ativação pós-falha de teste em cada um.
- As aplicações e as respetivas dependências alterado frequentemente. Para garantir que os planos de recuperação estão atualizados, execute uma ativação pós-falha de teste para cada aplicação cada trimestre.

    ![Captura de ecrã de um exemplo de teste plano de recuperação no Site Recovery](./media/recovery-plan-overview/rptest.png)

## <a name="watch-the-video"></a>Ver o vídeo

Ver um vídeo de exemplo rápido que mostra uma ativação pós-falha de no clique para uma aplicação WordPress de duas camadas.
    
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]



## <a name="next-steps"></a>Passos Seguintes

- [Criar](site-recovery-create-recovery-plans.md) um plano de recuperação.
* Saiba mais sobre [executar as ativações pós-falha](site-recovery-failover.md).  
