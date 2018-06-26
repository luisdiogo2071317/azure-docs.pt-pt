---
title: Fazer uma cópia de segurança de ficheiros no Azure pilha das VMs
description: Utilize a cópia de segurança do Azure para criar cópias de segurança e recuperar ficheiros de pilha do Azure e aplicações para o seu ambiente de pilha do Azure.
services: backup
author: adiganmsft
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 6/5/2018
ms.author: adigan
ms.openlocfilehash: 2fb3bad56de781dd81d4c5f82b734c9420c75dee
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751709"
---
# <a name="back-up-files-on-azure-stack"></a>Fazer uma cópia de segurança de ficheiros na pilha do Azure
Pode utilizar o Backup do Azure para proteger (ou cópia de segurança) ficheiros e aplicações na pilha do Azure. Para fazer uma cópia de segurança de ficheiros e aplicações, instale o servidor de cópia de segurança do Microsoft Azure como uma máquina virtual em execução na pilha do Azure. Pode proteger os ficheiros em qualquer servidor de pilha do Azure na mesma rede virtual. Uma vez que instalou o servidor de cópia de segurança do Azure, adicione discos do Azure para aumentar o armazenamento local disponível para dados de cópia de segurança de curta duração. Servidor de cópia de segurança do Azure utiliza o armazenamento do Azure para a retenção de longo prazo.

> [!NOTE]
> Apesar do servidor de cópia de segurança do Azure e o System Center Data Protection Manager (DPM) são semelhantes, o DPM não é suportado para utilização com a pilha do Azure.
>

Este artigo não abrange a instalar o servidor de cópia de segurança do Azure no ambiente de pilha do Azure. Para instalar o servidor de cópia de segurança do Azure na pilha do Azure, consulte o artigo [instalar o servidor de cópia de segurança do Azure](backup-mabs-install-azure-stack.md).


## <a name="back-up-files-and-folders-in-azure-stack-vms-to-azure"></a>Fazer cópias de segurança de ficheiros e pastas em VMs de pilha do Azure para o Azure

Para configurar o servidor de cópia de segurança do Azure para proteger ficheiros em máquinas virtuais de VMs de pilha do Azure, abra a consola do servidor de cópia de segurança do Azure. Irá utilizar a consola para configurar grupos de proteção e para proteger os dados nas suas máquinas virtuais.

1. Na consola do servidor de cópia de segurança do Azure, clique em **proteção** e na barra de ferramentas, clique em **novo** para abrir o **criar novo grupo de proteção** assistente.

   ![Configurar a proteção na consola do servidor de cópia de segurança do Azure](./media/backup-mabs-files-applications-azure-stack/1-mabs-menu-create-protection-group.png)

    Pode demorar alguns segundos para que o assistente abrir. Uma vez que abre o assistente, clique em **seguinte** para avançar para o **selecionar tipo de grupo de proteção** ecrã.

   ![Abre o Assistente de novo grupo de proteção](./media/backup-mabs-files-applications-azure-stack/2-create-new-protection-group-wiz.png)

2. No **selecionar tipo de grupo de proteção** ecrã, escolha **servidores** e clique em **seguinte**.

    ![Abre o Assistente de novo grupo de proteção](./media/backup-mabs-files-applications-azure-stack/3-select-protection-group-type.png)

    O **selecionar membros do grupo** ecrã é aberto. 

    ![Abre o Assistente de novo grupo de proteção](./media/backup-mabs-files-applications-azure-stack/4-opening-screen-choose-servers.png)

3. No **selecionar membros do grupo** ecrã, clique em **+** para expandir a lista de subitems. Para todos os itens que pretende proteger, selecione a caixa de verificação. Depois de todos os itens selecionados, clique em **seguinte**.

    ![Abre o Assistente de novo grupo de proteção](./media/backup-mabs-files-applications-azure-stack/5-select-group-members.png)

    A Microsoft recomenda a colocar todos os dados que irão partilhar uma política de proteção, no grupo de proteção de um. Para obter informações completas sobre planear e implementar grupos de proteção, consulte o artigo do System Center DPM [implementar grupos de proteção](https://docs.microsoft.com/en-us/system-center/dpm/create-dpm-protection-groups?view=sc-dpm-1801).

4. No **selecionar método de proteção de dados** ecrã, escreva um nome para o grupo de proteção. Selecione a caixa de verificação **pretendo proteção curto prazo utilizando:** e **pretendo proteção online**. Clique em **Seguinte**.

    ![Abre o Assistente de novo grupo de proteção](./media/backup-mabs-files-applications-azure-stack/6-select-data-protection-method.png)

    Para selecionar **pretendo proteção online**, primeiro tem de selecionar **pretendo proteção curto prazo utilizando:** disco. Servidor de cópia de segurança do Azure não protege em banda, para que o disco é a escolha apenas para proteção de curta duração.

5. No **especificar objetivos a curto prazo** ecrã, selecione o período de tempo manter os pontos de recuperação guardados em disco e guardar as cópias de segurança incrementais. Clique em **Seguinte**.

    > [!IMPORTANT]
    > Deve **não** manter os dados de recuperação operacional (cópia de segurança) nos discos ligados por servidor de cópia de segurança do Azure mais de cinco dias.
    >

    ![Abre o Assistente de novo grupo de proteção](./media/backup-mabs-files-applications-azure-stack/7-select-short-term-goals.png) 

    Em vez de selecionar um intervalo para cópias de segurança incrementais, para executar uma rápida cópia de segurança completa imediatamente antes de cada ponto de recuperação agendado, clique em **apenas antes de um ponto de recuperação**. Se estiver a proteger cargas de trabalho da aplicação, o servidor de cópia de segurança do Azure cria pontos de recuperação pela agenda da frequência de sincronização (desde que a aplicação suportar cópias de segurança incrementais). Se a aplicação não suporta cópias de segurança incrementais, servidor de cópia de segurança do Azure é executada uma rápida cópia de segurança completa.

    Para **pontos de recuperação de ficheiros**, especifique quando criar pontos de recuperação. Clique em **modificar** para definir as horas e os dias da semana quando são criados os pontos de recuperação.

6. No **rever alocação do disco** ecrã, reveja o espaço de disco de agrupamento de armazenamento atribuído para o grupo de proteção.

    **Tamanho de dados total** é o tamanho dos dados que pretende criar cópias de segurança e **disco espaço a ser aprovisionado** no servidor de cópia de segurança do Azure é o espaço recomendado para o grupo de proteção. Servidor do Backup do Azure escolhe o volume de cópia de segurança ideal, com base nas definições. No entanto, pode editar as opções de cópia de segurança de volume nos detalhes de alocação de disco. Para cargas de trabalho, selecione o armazenamento preferencial no menu pendente. As edições que efectuou alterar os valores de armazenamento Total e de armazenamento livre no painel de armazenamento de disco disponível. Espaço underprovisioned é a quantidade de armazenamento de que servidor de cópia de segurança do Azure sugere que adiciona ao volume, para continuar com cópias de segurança facilmente no futuro.

7. No **escolher método de criação de réplica**, selecione a forma como pretende lidar com a replicação inicial de dados completa. Se optar por replicar através da rede, o Azure recomenda que escolher uma hora de ponta. Para grandes quantidades de dados ou condições de rede ideais, considere replicar os dados utilizando o suporte de dados amovível.

8. No **escolher opções de verificação de consistência**, selecione a forma como pretende automatizar as verificações de consistência. Ative verificações de consistência ser executado apenas quando a replicação de dados se tornar inconsistente ou, de acordo com uma agenda. Se não pretender configurar a verificação de consistência automática, execute uma verificação manual em qualquer altura:
    * No **proteção** área da consola do servidor de cópia de segurança do Azure, com o botão direito do grupo de proteção e selecione **efetuar verificação de consistência**.

9. Se optar por criar cópias de segurança para o Azure, no **especificar dados da proteção online** página Certifique-se as cargas de trabalho que pretende fazer cópias de segurança do Azure estão selecionadas.

10. No **Especificar agenda de cópia de segurança online**, especifique quando devem ocorrer cópias de segurança incrementais para o Azure. 

    Pode agendar cópias de segurança para executar cada dia/semana/mês/ano e a data/hora em que deve executar. As cópias de segurança podem ocorrer até duas vezes por dia. Sempre que é executada uma tarefa de cópia de segurança, um ponto de recuperação de dados é criado no Azure a partir de cópia dos dados de cópia de segurança armazenados no disco do servidor de cópia de segurança do Azure.

11. No **especificar política de retenção online**, especifique a forma como os pontos de recuperação criados a partir de cópias de segurança diárias/semanal/mensal/anual são mantidos no Azure.

12. No **escolher replicação online**, especifique a forma como ocorre a replicação inicial completa dos dados. 

13. No **resumo**, reveja as definições. Ao clicar em **criar grupo**, ocorre a replicação inicial de dados. Quando a replicação de dados estiver concluído, no **estado** página, é apresentado o estado do grupo de proteção como **OK**. A tarefa de cópia de segurança inicial ocorre de acordo com a proteção definições do grupo.

Perguntas que necessitam de atender: como a expandir o armazenamento de disco para armazenamento de disco de curta duração pilha do Azure. Quais são as diretrizes que precisam de ser realçadas explicar o armazenamento de disco de curta duração?

## <a name="recover-file-data"></a>Recuperar dados de ficheiro

Utilize a consola do servidor de cópia de segurança do Azure para recuperar dados para a máquina virtual.

1. Na consola do servidor de cópia de segurança do Azure, na barra de navegação, clique em **recuperação** e navegue para os dados que pretende recuperar. No painel de resultados, selecione os dados.

2. No calendário na secção de pontos de recuperação, datas a negrito indicam os pontos de recuperação estão disponíveis. Selecione a data para recuperar um ponto de recuperação.

3. No **item recuperável** painel, selecione o item que pretende recuperar.

4. No **ações** painel, clique em **recuperar** para abrir o Assistente de recuperação.

5. Pode recuperar os dados da seguinte forma:

    * **Recuperar para a localização original** -se o computador cliente estiver ligado através de VPN, esta opção não funcionar. Em vez disso, utilize uma localização alternativa e, em seguida, copiar dados a partir dessa localização.
    * **Recuperar para uma localização alternativa**

6. Especifique as opções de recuperação:

    * Para **comportamento de recuperação da versão existente**, selecione **criar cópia**, **ignorar**, ou **substituir**. Substituir só está disponível quando recuperar para a localização original.
    * Para **restaurar segurança**, escolha **aplicar as definições do computador de destino** ou **aplicar as definições de segurança da versão do ponto de recuperação de**.
    * Para **limitação da utilização de largura de banda de rede**, clique em **modificar** para ativar a limitação da utilização de largura de banda de rede.
    * **Notificação** clique **enviar um e-mail ao concluir a recuperação**, e especifique os destinatários que irão receber a notificação. Separe os endereços de correio eletrónico por vírgulas.
    * Depois de efetuar as seleções, clique em **seguinte**

7. Reveja as definições de recuperação e clique em **recuperar**. 

    > [!Note] 
    > Enquanto a tarefa de recuperação está em curso, são canceladas todas as tarefas de sincronização para os itens de recuperação selecionado.
    >

Se estiver a utilizar o armazenamento de cópia de segurança moderna (MB), não é suportada a recuperação de utilizador final (EUR) do servidor de ficheiros. EUR do servidor de ficheiros tem uma dependência no Volume de cópia sombra Service (VSS), que não utilizam o armazenamento de cópia de segurança moderna. Se estiver ativada EUR, utilize os seguintes passos para recuperar dados:

1. Navegue para os ficheiros protegidos e o nome de ficheiro com o botão direito e selecione **propriedades**.

2. No **propriedades** menu, clique em **versões anteriores** e escolha a versão que pretende recuperar.

## <a name="view-azure-backup-server-with-a-vault"></a>Servidor de cópia de segurança do Azure vista com o Cofre de
Para ver as entidades de servidor de cópia de segurança do Azure no Portal do Azure, pode seguir os seguintes passos:
1. Abra o Cofre dos serviços de recuperação.
2. Clique em infraestrutura de cópia de segurança.
3. Servidores de gestão de cópia de segurança de vista.

## <a name="see-also"></a>Consulte também
Para obter informações sobre como utilizar o servidor de cópia de segurança do Azure para proteger outras cargas de trabalho, consulte um dos seguintes artigos:
- [Fazer cópias de segurança do farm do SharePoint](https://docs.microsoft.com/en-us/azure/backup/backup-mabs-sharepoint-azure-stack)
- [Criar cópias de segurança do SQL Server](https://docs.microsoft.com/en-us/azure/backup/backup-mabs-sql-azure-stack)
