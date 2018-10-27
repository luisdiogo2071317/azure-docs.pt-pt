---
title: Gerir um modelo de um laboratório de sala de aula no Azure Lab Services | Documentos da Microsoft
description: Saiba como criar e gerir um modelo de laboratório de sala de aula no Azure Lab Services.
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
ms.date: 10/25/2018
ms.author: spelluru
ms.openlocfilehash: 3ecbef3b3063ceb413b852f8000b44a85d28d08e
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142511"
---
# <a name="create-and-manage-a-classroom-template-in-azure-lab-services"></a>Criar e gerir um modelo de sala de aula no Azure Lab Services
Um modelo num laboratório é a imagem de máquina virtual de base da qual todas as máquinas virtuais dos utilizadores são criadas. Configure a máquina virtual do modelo para que seja configurado com exatamente o que pretende fornecer aos utilizadores do laboratório. Pode fornecer um nome e descrição do modelo que os utilizadores do laboratório vão ver. Em seguida, publicar o modelo para que as instâncias do modelo VM disponíveis aos seus utilizadores de laboratório. Ao publicar um modelo, o Azure Lab Services cria VMs no laboratório através do modelo. O número de VMs criadas neste processo é igual ao número máximo de utilizadores permitidos no laboratório, o que pode configurar na política de utilização do laboratório. Todas as máquinas virtuais têm a mesma configuração do modelo.

Este artigo descreve como criar e gerir uma máquina virtual do modelo num laboratório de sala de aula do Azure Lab Services. 

## <a name="publish-a-template-while-creating-a-classroom-lab"></a>Publicar um modelo ao criar um laboratório de sala de aula
Em primeiro lugar, pode configurar e publicar um modelo ao criar um laboratório de sala de aula.

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
    2. Para publicar mais tarde, selecione **Guardar para utilização posterior**. Pode publicar a VM de modelo depois de concluir o assistente. Para obter detalhes sobre como configurar e publicar depois de concluir o assistente, consulte para obter detalhes sobre como configurar e publicar depois de concluir o assistente, consulte [publicar o modelo](#publish-the-template) secção a [como gerir laboratórios de sala de aula ](how-to-manage-classroom-labs.md) artigo.

        ![Publicar modelo](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Verá o **progresso da publicação** do modelo. Este processo pode demorar até uma hora. 

    ![Publicar modelo – progresso](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Verá a página seguinte, quando o modelo for publicado com êxito. Selecione **Done** (Concluído).

    ![Publicar modelo – êxito](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Verá o **dashboard** para o seu laboratório. 
    
    ![Dashboard de laboratório de sala de aula](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)

## <a name="set-up-a-template-after-creating-a-lab"></a>Configurar um modelo depois de criar um laboratório 
Também pode configurar um modelo depois de criar o laboratório.   

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
Se não o publicar o modelo ao criar o laboratório, pode publicá-lo mais tarde. Antes de publicar, talvez queira ligar ao modelo de VM e atualize-o com qualquer software. Ao publicar um modelo, o Azure Lab Services cria VMs no laboratório através do modelo. O número de VMs criadas neste processo é igual ao número máximo de utilizadores permitidos no laboratório, o que pode configurar na política de utilização do laboratório. Todas as máquinas virtuais têm a mesma configuração do modelo. 

1. Selecione **Publicar** na secção **Modelo**. 

    ![Publicar a VM do modelo](../media/tutorial-setup-classroom-lab/public-access.png)
1. Sobre o **publicar o modelo** caixa de mensagens, reveja a mensagem e selecione **Publish**. Este processo poderá demorar algum tempo consoante quantas VMs estão a ser criadas, sendo o mesmo número de utilizadores permitidos no laboratório.
    
    > [!IMPORTANT]
    > Depois de um modelo ser publicado, não pode ser anulado. Pode voltar a publicar o modelo no entanto. 
4. Mude para a página **Máquinas virtuais** e confirme que vê as máquinas virtuais que estão no estado **Não atribuído**. Estas VMs ainda não estão atribuídas a estudantes. 

    ![Máquinas virtuais](../media/tutorial-setup-classroom-lab/virtual-machines.png)
5. Aguarde até que as VMs sejam criadas. Elas devem estar no estado **Parado**. Pode iniciar uma VM de estudante, ligar à VM, parar a VM e eliminar a VM nesta página. Pode iniciá-las nesta página ou permitir que os seus estudantes iniciem as VMs. 

    ![Máquinas virtuais no estado parado](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="republish-the-template"></a>Voltar a publicar o modelo 
Depois de publicar um modelo, pode continuar a ligar ao modelo de VM, atualizá-lo e, em seguida, publicá-la novamente. Quando voltar a publicar um modelo de VM, todos os utilizadores que VMS obterem desligadas e eles são recriados com base no modelo atualizado. 

1. Na página do dashboard do seu laboratório, selecione **voltar a publicar** na secção de modelo. 

    ![Botão de voltar a publicar](../media/how-to-create-manage-template/republish-button.png)
2. Sobre o **voltar a publicar o modelo** caixa de mensagens, reveja o texto e selecione **voltar a publicar** para continuar. Caso contrário, selecione **Cancelar**. 

    ![Voltar a publicar a mensagem de modelo](../media/how-to-create-manage-template/republish-template-message.png)
3. Verá o estado do volte a publicar no mosaico no **modelo** secção.

    ![Estado de republicação](../media/how-to-create-manage-template/republish-status-publishing.png)
4. Depois do modelo for publicado, o estado é definido como **publicado**. 

    ![Volte a publicar com êxito](../media/how-to-create-manage-template/republish-success.png)

## <a name="next-steps"></a>Passos Seguintes
Introdução à configuração de um laboratório com o Azure Lab Services:

- [Configurar um laboratório de sala de aula](how-to-manage-classroom-labs.md)
- [Configurar um laboratório](../tutorial-create-custom-lab.md)
