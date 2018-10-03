---
title: Gerir laboratórios de sala de aula no Azure Lab Services | Documentos da Microsoft
description: Saiba como criar e configurar um laboratório de sala de aula, veja todos os os sala de aula laboratórios, shre o registo estar vinculados a um utilizador de laboratório ou eliminar um laboratório.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2018
ms.author: spelluru
ms.openlocfilehash: eebdc98db5ecdf518d3b0b58e6757a2b7ecd5dd7
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043809"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Gerir laboratórios de sala de aula no Azure Lab Services 
Este artigo descreve como criar e configurar um laboratório de sala de aula, ver todos os laboratórios de sala de aula ou eliminar um laboratório de sala de aula.

## <a name="prerequisites"></a>Pré-requisitos
Para configurar um laboratório de sala de aula numa conta do laboratório, tem de ser um membro da função **Criador de Laboratório** na conta de laboratório. A conta que utilizou para criar a conta de laboratório é adicionada automaticamente a esta função. Um proprietário de laboratório pode adicionar outros utilizadores à função Criador de Laboratório com os passos do seguinte artigo: [Adicionar um utilizador à função Criador de Laboratório](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Criar um laboratório de sala de aula

1. Navegue para o [site do Azure Lab Services](https://labs.azure.com). 
2. Selecione **Iniciar sessão** e introduza as suas credenciais. O Azure Lab Services suporta contas organizacionais e contas Microsoft. 
3. Na janela **Novo Laboratório**, realize as ações abaixo: 
    1. Especifique um **nome** para seu laboratório. 
    2. Especifique o número máximo **número de utilizadores** permitido para o laboratório. 
    6. Selecione **Guardar**.

        ![Criar um laboratório de sala de aula](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. Sobre o **Selecione as especificações de máquina virtual** página, efetue os seguintes passos:
    1. Selecione um **tamanho** para máquinas virtuais (VMs) criadas no laboratório. 
    2. Selecione o **região** em quais serão as VMs a ser criada. 
    3. Selecione o **imagem de VM** a ser utilizado para criar VMs no laboratório. 
    4. Selecione **Seguinte**.

        ![Especificar as especificações de VM](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. Sobre o **definir credenciais** , especifique as credenciais predefinidas para todas as VMs no laboratório. 
    1. Especifique a **nome do utilizador** para todas as VMs no laboratório.
    2. Especifique a **palavra-passe** para o utilizador. 

        > [!IMPORTANT]
        > Tome nota do nome de utilizador e palavra-passe. Estas não serão apresentadas novamente.
    3. Selecione **Criar**. 

        ![Definir credenciais](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. Sobre o **configurar modelo** página, verá o estado do processo de criação de laboratório. A criação do modelo no laboratório demora até 20 minutos. 

    ![Configurar o modelo](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Depois de concluída a configuração do modelo, verá a página seguinte: 

    ![Configurar a página do modelo depois de ter sido](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. Os passos seguintes são passos opcionais neste tutorial: 
    1. Inicie o modelo de VM selecionando **iniciar**.
    2. Ligue-se ao modelo de VM selecionando **Connect**. 
    3. Instalar e configurar o software no seu modelo VM. 
    4. **Parar** a VM.  
    5. Introduza um **Descrição** para o modelo

        ![Em seguida, na página do modelo de configuração](../media/tutorial-setup-classroom-lab/configure-template-next.png)
9. Selecione **seguinte** na página do modelo. 
10. No **publicar o modelo** página, efetue as seguintes ações. 
    1. Para publicar o modelo imediatamente, selecione a caixa de verificação *compreendo que não é possível modificar o modelo após a publicação. Este processo pode ser feito apenas uma vez e pode demorar até uma hora*e selecione **publicar**.  

        > [!WARNING]
        > Depois de publicar, não é possível anular a publicação. 
    2. Para publicar mais tarde, selecione **guardar para utilizar mais tarde**. Pode publicar o modelo de VM depois de concluir o assistente. Para obter detalhes sobre como configurar e publicar depois de concluir o assistente, consulte para obter detalhes sobre como configurar e publicar depois de concluir o assistente, consulte [publicar o modelo](#publish-the-template) secção a [como gerir laboratórios de sala de aula ](how-to-manage-classroom-labs.md) artigo.

        ![Publicar o modelo](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Verá o **progresso da publicação** o modelo. Este processo pode demorar até uma hora. 

    ![Publicar o modelo - progresso](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Verá a página seguinte, quando o modelo for publicado com êxito. Selecione **Done** (Concluído).

    ![Publicar o modelo - êxito](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Verá o **dashboard** para o seu laboratório. 
    
    ![Dashboard de laboratório de sala de aula](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. Mude para a página **Máquinas virtuais** e confirme que vê as máquinas virtuais que estão no estado **Não atribuído**. Estas VMs ainda não estão atribuídas a estudantes. Elas devem estar no estado **Parado**. Pode iniciar uma VM de estudante, ligar à VM, parar a VM e eliminar a VM nesta página. Pode iniciá-las nesta página ou permitir que os seus estudantes iniciem as VMs. 

    ![Máquinas virtuais no estado parado](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

## <a name="configure-usage-policy"></a>Configurar a política de utilização

1. Selecione **Política de utilização**. 
2. Nas definições da **Política de utilização**, introduza o **número de utilizadores** que têm permissão para utilizar o laboratório.
3. Selecione **Guardar**. 

    ![Política de utilização](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="set-up-the-template"></a>Configurar o modelo
Um modelo num laboratório é a imagem de máquina virtual de base da qual todas as máquinas virtuais dos utilizadores são criadas. Configure a máquina virtual do modelo para que seja configurado com exatamente o que pretende fornecer aos utilizadores do laboratório. Pode fornecer um nome e descrição do modelo que os utilizadores do laboratório vão ver. Publique o modelo para tornar as instâncias da VM de modelo disponíveis para os seus utilizadores de laboratório.  

### <a name="set-template-title-and-description"></a>Título do modelo de conjunto e uma descrição
1. Na secção **Modelo**, selecione **Editar** (ícone de lápis) para o modelo. 
2. Na janela **Vista do utilizador**, introduza um **título** para o modelo.
3. Introduza a **descrição** do modelo.
4. Selecione **Guardar**.

    ![Descrição do laboratório da sala de aula](../media/how-to-manage-classroom-labs/lab-description.png)

### <a name="set-up-the-template-vm"></a>Configurar a VM do modelo
 Ligue à VM do modelo e instale o software obrigatório na mesma antes de a disponibilizar aos seus estudantes. 

1. Aguarde até que a máquina virtual do modelo esteja pronta. Assim que estiver pronta, o botão **Iniciar** deve ser ativado. Para iniciar a VM, selecione **Iniciar**.

    ![Iniciar a VM do modelo](../media/tutorial-setup-classroom-lab/start-template-vm.png)
1. Para ligar à VM, selecione **Ligar** e siga as instruções. 

    ![Ligar à VM do modelo](../media/tutorial-setup-classroom-lab/connect-template-vm.png)
1. Instale o software obrigatório para os estudantes fazerem o laboratório (por exemplo, o Visual Studio, o Explorador de Armazenamento do Azure, etc.). 
2. Desligue (feche a sessão em curso remota) a partir do modelo da VM. 
3. **Pare** a VM do modelo ao selecionar **Parar**. 

    ![Parar a VM do modelo](../media/tutorial-setup-classroom-lab/stop-template-vm.png)


## <a name="publish-the-template"></a>Publicar o modelo 
Ao publicar um modelo, o Azure Lab Services cria VMs no laboratório através do modelo. O número de VMs criadas neste processo é igual ao número máximo de utilizadores permitidos no laboratório, o que pode configurar na política de utilização do laboratório. Todas as máquinas virtuais têm a mesma configuração do modelo. 

1. Selecione **Publicar** na secção **Modelo**. 

    ![Publicar a VM do modelo](../media/tutorial-setup-classroom-lab/public-access.png)
1. Na janela **Publicar**, selecione a opção **Publicado**. 
2. Agora, selecione o botão **Publicar**. Este processo poderá demorar algum tempo consoante quantas VMs estão a ser criadas, sendo o mesmo número de utilizadores permitidos no laboratório.
    
    > [!IMPORTANT]
    > Depois de um modelo ser publicado, não pode ser anulado. 
4. Mude para a página **Máquinas virtuais** e confirme que vê as máquinas virtuais que estão no estado **Não atribuído**. Estas VMs ainda não estão atribuídas a estudantes. 

    ![Máquinas virtuais](../media/tutorial-setup-classroom-lab/virtual-machines.png)
5. Aguarde até que as VMs sejam criadas. Elas devem estar no estado **Parado**. Pode iniciar uma VM de estudante, ligar à VM, parar a VM e eliminar a VM nesta página. Pode iniciá-las nesta página ou permitir que os seus estudantes iniciem as VMs. 

    ![Máquinas virtuais no estado parado](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="send-registration-link-to-students"></a>Enviar a ligação de registo para os alunos

1. Mude para a vista **Dashboard**. 
2. Selecione o mosaico **Registo de utilizador**.

    ![Ligação de registo do aluno](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. Na caixa de diálogo **Registo de utilizador**, selecione o botão **Copiar**. A ligação é copiada para a área de transferência. Cole-a num editor de e-mail e envie um e-mail para o aluno. 

    ![Ligação de registo do aluno](../media/tutorial-setup-classroom-lab/registration-link.png)
2. Na caixa de diálogo **Registo de utilizador**, selecione **Fechar**. 

## <a name="view-all-classroom-labs"></a>Ver todos os laboratórios de sala de aula
1. Navegue para [portal do Azure Lab Services](https://labs.azure.com).
2. Confirme que vê todos os laboratórios na conta de laboratório selecionado. 

    ![Todos os laboratórios](../media/how-to-manage-classroom-labs/all-labs.png)
3. Utilize a lista pendente na parte superior para selecionar uma conta de laboratório diferentes. Veja laboratórios na conta de laboratório selecionado. 

## <a name="delete-a-classroom-lab"></a>Eliminar um laboratório de sala de aula
1. No mosaico para o laboratório, selecione as reticências (...) no canto. 

    ![Selecionar o laboratório](../media/how-to-manage-classroom-labs/select-three-dots.png)
2. Selecione **Eliminar**. 

    ![Botão de eliminar](../media/how-to-manage-classroom-labs/delete-button.png)
3. Sobre o **laboratório de eliminação** caixa de diálogo, selecione **eliminar**. 

    ![Eliminar a caixa de diálogo](../media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)
 
## <a name="manage-student-vms"></a>Gerir VMs de alunos
Assim que os estudantes registar com o Azure Lab Services usando o registro link fornecido aos mesmos, verá as VMs atribuídas aos estudantes na **máquinas virtuais** separador. 

![Máquinas de virtuais atribuídas para estudantes](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

Pode efetuar as seguintes tarefas numa VM de estudante: 

- Pare uma VM se a VM está em execução. 
- Inicie uma VM se a VM é parada. 
- Ligue à VM. 
- Elimine a VM. 

## <a name="next-steps"></a>Passos Seguintes
Introdução à configuração de um laboratório com o Azure Lab Services:

- [Configurar um laboratório de sala de aula](how-to-manage-classroom-labs.md)
- [Configurar um laboratório](../tutorial-create-custom-lab.md)
