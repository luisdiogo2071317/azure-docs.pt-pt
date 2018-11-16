---
title: Configurar um laboratório de sala de aula com o Azure Lab Services | Microsoft Docs
description: Neste tutorial, vai configurar um laboratório para utilizar numa sala de aula.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 11/14/2018
ms.author: spelluru
ms.openlocfilehash: babff55d6684feb1f0414970616260be96b994f4
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706012"
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Tutorial: Configurar um laboratório de sala de aula 
Neste tutorial, vai configurar um laboratório de sala de aula com máquinas virtuais que serão utilizadas pelos alunos na sala de aula.  

Neste tutorial, irá realizar as seguintes ações:

> [!div class="checklist"]
> * Criar um laboratório de sala de aula
> * Configurar o laboratório de sala de aula
> * Enviar a ligação de registo para os alunos

## <a name="prerequisites"></a>Pré-requisitos
Para configurar um laboratório de sala de aula numa conta do laboratório, tem de ser um membro da função **Criador de Laboratório** na conta de laboratório. A conta que utilizou para criar a conta de laboratório é adicionada automaticamente a esta função. Um proprietário de laboratório pode adicionar outros utilizadores à função Criador de Laboratório com os passos do seguinte artigo: [Adicionar um utilizador à função Criador de Laboratório](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).


## <a name="create-a-classroom-lab"></a>Criar um laboratório de sala de aula

1. Navegue para o [site do Azure Lab Services](https://labs.azure.com). 
2. Selecione **Iniciar sessão** e introduza as suas credenciais. O Azure Lab Services suporta contas organizacionais e contas Microsoft. 
3. Na janela **Novo Laboratório**, realize as ações abaixo: 
    1. Especifique um **nome** para o seu laboratório. 
    2. Especifique o **número máximo de utilizadores** permitido no laboratório. 
    6. Selecione **Guardar**.

        ![Criar um laboratório de sala de aula](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. Na página **Selecionar especificações da máquina virtual**, execute os passos seguintes:
    1. Selecione um **tamanho** para máquinas virtuais (VM) criadas no laboratório. 
    2. Selecione a **região** na qual pretende que as VM sejam criadas. 
    3. Selecione a **imagem de VM** a ser utilizado para criar as VM no laboratório. 
    4. Selecione **Seguinte**.

        ![Especificar as especificações de VM](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. Na página **Definir credenciais** , especifique as credenciais predefinidas para todas as VM do laboratório. 
    1. Especifique o **nome de utilizador** de todas as VM do laboratório.
    2. Especifique a **palavra-passe** do utilizador. 

        > [!IMPORTANT]
        > Registe o nome de utilizador e a palavra-passe. Estas não serão apresentadas novamente.
    3. Selecione **Criar**. 

        ![Definir credenciais](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. Na página **Configurar modelo**, verá o estado do processo de criação do laboratório. A criação do modelo no laboratório demora no máximo 20 minutos. 

    ![Configurar modelo](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Depois de concluída a configuração do modelo, verá a página seguinte: 

    ![Configurar a página do modelo após a conclusão](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. Os passos seguintes são opcionais neste tutorial: 
    1. Inicie a VM de modelo através da seleção de **Iniciar**.
    2. Ligue-se à VM de modelo através da seleção de **Ligar**. 
    3. Instalar e configurar o software na sua VM de modelo. 
    4. **Parar** a VM.  
    5. Introduzir uma **descrição** do modelo

        ![Em seguida, na página Configurar modelo](../media/tutorial-setup-classroom-lab/configure-template-next.png)
9. Selecione **Seguinte** na página do modelo. 
10. Na página **Publicar o modelo**, execute as ações seguintes. 
    1. Para publicar o modelo imediatamente, selecione a caixa de verificação *Compreendo que não é possível modificar o modelo após a publicação. Este processo pode ser feito apenas uma vez e pode demorar até uma hora*e selecione **Publicar**.  

        > [!WARNING]
        > Depois de publicar, não é possível anular a publicação. 
    2. Para publicar mais tarde, selecione **Guardar para utilização posterior**. Pode publicar a VM de modelo depois de concluir o assistente. Para obter detalhes sobre como configurar e publicar depois de concluir o assistente, consulte a secção [Publicar o modelo](how-to-create-manage-template.md#publish-the-template-vm)no artigo [Como gerir laboratórios de sala de aula](how-to-manage-classroom-labs.md).

        ![Publicar modelo](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Verá o **progresso da publicação** do modelo. Este processo pode demorar até uma hora. 

    ![Publicar modelo – progresso](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Verá a página seguinte, quando o modelo for publicado com êxito. Selecione **Done** (Concluído).

    ![Publicar modelo – êxito](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Verá o **dashboard** para o seu laboratório. 
    
    ![Dashboard de laboratório de sala de aula](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. Mude para a página **Máquinas virtuais** e confirme que vê as máquinas virtuais que estão no estado **Não atribuído**. Estas VMs ainda não estão atribuídas a estudantes. Elas devem estar no estado **Parado**. Pode iniciar uma VM de estudante, ligar à VM, parar a VM e eliminar a VM nesta página. Pode iniciá-las nesta página ou permitir que os seus estudantes iniciem as VMs. 

    ![Máquinas virtuais no estado parado](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

## <a name="add-users-to-the-lab"></a>Adicionar utilizadores ao laboratório

1. Selecione **utilizadores** no menu da esquerda. Por predefinição, o **restringir o acesso** opção está ativada. Quando esta definição está ativado, um utilizador não é possível registar com o laboratório, mesmo que o utilizador tem a ligação de registo, a menos que o utilizador está na lista de utilizadores. Apenas os utilizadores da lista podem registar com o laboratório, utilizando a ligação de registo que envia. Neste procedimento, pode adicionar utilizadores à lista. Em alternativa, pode desativar **restringir o acesso**, que permite aos utilizadores para se registar com o laboratório, desde que tenham uma ligação de registo. 
2. Selecione **adicionar utilizadores** na barra de ferramentas. 
3. Sobre o **adicionar utilizadores** página, introduza os endereços de e-mail dos utilizadores em linhas separadas ou numa única linha, separados por ponto e vírgula. 

    ![Endereços de e-mail do utilizador](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Selecione **Guardar**. Ver os endereços de e-mail dos utilizadores e os respetivos Estados (registados ou não) na lista. 

    ![Lista de utilizadores](../media/how-to-configure-student-usage/users-list-new.png)


## <a name="send-registration-link-to-students"></a>Enviar a ligação de registo para os alunos

1. Mude para o **utilizadores** ver se ainda não está na página. 
2. Selecione **obter a ligação de registo** mosaico.

    ![Ligação de registo do aluno](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. Na caixa de diálogo **Registo de utilizador**, selecione o botão **Copiar**. A ligação é copiada para a área de transferência. 

    ![Ligação de registo do aluno](../media/tutorial-setup-classroom-lab/registration-link.png)
2. Na caixa de diálogo **Registo de utilizador**, selecione **Fechar**. 
4. Partilhe a ligação de registo com um estudante, para que este possa registar-se na turma. Se tiver o **restringir a opção** definam enabled e tem uma lista de utilizadores na lista, fazer as seguintes ações:
    1. Selecione o **endereço de e-mail** do utilizador na lista. 
    2. Verá uma janela do seu programa de e-mail predefinida com o **TO** endereço preenchidos. 
    3. Colar o **URL de registo** que copiou anteriormente. 
    4. Enviar a **e-mail**.


## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, criou um laboratório de sala de aula e configurou o laboratório. Para saber como um aluno pode aceder a uma VM no laboratório através da ligação de registo, avance para o próximo tutorial:

> [!div class="nextstepaction"]
> [Ligar a uma VM no laboratório de sala de aula](tutorial-connect-virtual-machine-classroom-lab.md)

