---
title: Gerir laboratórios de sala de aulas nos serviços de laboratório do Azure | Microsoft Docs
description: Saiba como criar e configurar um laboratório de sala de aula, ver todos os os sala de aulas laboratórios, shre o registo de ligação com um utilizador de laboratório ou eliminar um laboratório.
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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 311e58f01fac6d7786992b3c11e4b1b7c02ca838
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36304522"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Gerir laboratórios de sala de aulas nos serviços de laboratório do Azure 
Este artigo descreve como criar e configurar um laboratório de sala de aula, ver todos os laboratórios de sala de aula ou eliminar um laboratório de sala de aula.

## <a name="prerequisites"></a>Pré-requisitos
Para configurar um laboratório de sala de aulas numa conta do laboratório, tem de ser um membro do **criador de laboratório** função na conta de laboratório. Um proprietário de laboratório, pode adicionar um utilizador à função de criador de laboratório, utilizando os passos no seguinte artigo: [adicionar um utilizador à função de criador de laboratório](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Criar um laboratório de sala de aula

1. Navegue para o [site do Azure Lab Services](https://labs.azure.com).
2. Na janela **Novo Laboratório**, realize as ações abaixo: 
    1. Especifique um **nome** para o laboratório de sala de aula. 
    2. Selecione o **tamanho** da máquina virtual que planeia utilizar na sala de aula.
    3. Selecione a **imagem** a utilizar para criar a máquina virtual.
    4. Especifique o **as credenciais predefinidas** a utilizar para iniciar sessão nas máquinas virtuais no laboratório.
    7. Selecione **Guardar**.

        ![Criar um laboratório de sala de aula](../media/how-to-manage-classroom-labs/new-lab-window.png)
1. Pode ver o **dashboard** para o laboratório. 
    
    ![Dashboard de laboratório de sala de aulas](../media/how-to-manage-classroom-labs/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Configurar a política de utilização

1. Selecione **Política de utilização**. 
2. Nas definições da **Política de utilização**, introduza o **número de utilizadores** que têm permissão para utilizar o laboratório.
3. Selecione **Guardar**. 

    ![Política de utilização](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="set-up-the-template"></a>Configurar o modelo
Um modelo num laboratório é a imagem de máquina virtual de base da qual todas as máquinas virtuais dos utilizadores são criadas. Configure a máquina virtual do modelo para que seja configurado com exatamente o que pretende fornecer aos utilizadores do laboratório. Pode fornecer um nome e descrição do modelo que os utilizadores do laboratório vão ver. Publica o modelo para disponibilizar as instâncias do modelo de VM para os utilizadores de laboratório.  

### <a name="set-template-title-and-description"></a>Título de modelo do conjunto e a descrição
1. Na secção **Modelo**, selecione **Editar** (ícone de lápis) para o modelo. 
2. Na janela **Vista do utilizador**, introduza um **título** para o modelo.
3. Introduza a **descrição** do modelo.
4. Selecione **Guardar**.

    ![Descrição do laboratório da sala de aula](../media/how-to-manage-classroom-labs/lab-description.png)

### <a name="set-up-the-template-vm"></a>Configurar o modelo de VM
 Ligar ao modelo de VM e instalar obrigatoriamente qualquer software necessário no mesmo antes de o tornar disponível para a sua estudantes. 

1. Aguarde até que a máquina virtual do modelo está pronta. Assim que estiver pronto, o **iniciar** botão deve ser ativado. Para iniciar a VM, selecione **iniciar**.

    ![Iniciar o modelo de VM](../media/tutorial-setup-classroom-lab/start-template-vm.png)
1. Para ligar à VM, selecione **Connect**e siga as instruções. 

    ![Ligar ao modelo de VM](../media/tutorial-setup-classroom-lab/connect-template-vm.png)
1. Instale obrigatoriamente qualquer software necessária para estudantes fazer o laboratório (por exemplo, Visual Studio, o Explorador de armazenamento do Azure, etc.). 
2. Desligar (fechar a sessão de ambiente de trabalho remota) do modelo de VM. 
3. **Parar** o modelo de VM selecionando **parar**. 

    ![Parar o modelo de VM](../media/tutorial-setup-classroom-lab/stop-template-vm.png)


### <a name="publish-the-template"></a>Publicar o modelo 
Quando publicar um modelo, serviços de laboratório do Azure cria VMs no laboratório utilizando o modelo. O número de VMs criadas neste processo é igual ao número máximo de utilizadores permitidos no laboratório, o que pode configurar na política de utilização do laboratório. Todas as máquinas virtuais têm a mesma configuração do modelo. 

1. Selecione **publicar** no **modelo** secção. 

    ![Publicar o modelo de VM](../media/tutorial-setup-classroom-lab/public-access.png)
1. No **publicar** janela, selecione o **publicada** opção. 
2. Agora, selecione o **publicar** botão. Este processo poderá demorar algum tempo consoante quantas VMs está a ser criado, que é igual ao número de utilizadores permitido para o laboratório.
    
    > [!IMPORTANT]
    > Depois de um modelo estar disponível publicamente, não é possível alterar o acesso para privado. 
4. Mudar para o **máquinas virtuais** e confirme que vê cinco máquinas virtuais que estão na página **não atribuídos** estado. Estas VMs não estão atribuídos a estudantes ainda. 

    ![Máquinas virtuais](../media/tutorial-setup-classroom-lab/virtual-machines.png)
5. Aguarde até que as VMs que são criadas. Devem ser na **parado** estado. Pode iniciar um VM do estudante, ligar à VM, pare a VM e elimine a VM nesta página. Pode iniciá-los nesta página ou permitir que o seu estudantes iniciar as VMs. 

    ![Máquinas virtuais no estado parado](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="send-registration-link-to-students"></a>Enviar a ligação de registo para os alunos

1. Mudar para o **Dashboard** vista. 
2. Selecione o mosaico **Registo de utilizador**.

    ![Ligação de registo do aluno](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. Na caixa de diálogo **Registo de utilizador**, selecione o botão **Copiar**. A ligação é copiada para a área de transferência. Cole-a num editor de e-mail e envie um e-mail para o aluno. 

    ![Ligação de registo do aluno](../media/tutorial-setup-classroom-lab/registration-link.png)
2. No **registo de utilizador** caixa de diálogo, selecione **fechar**. 

## <a name="view-all-classroom-labs"></a>Ver todos os laboratórios de sala de aulas
1. Navegue para [portal dos serviços do Azure laboratório](https://labs.azure.com).
2. Confirme que vê todos os laboratórios na conta de laboratório selecionado. 

    ![Todos os laboratórios](../media/how-to-manage-classroom-labs/all-labs.png)
3. Utilize na lista pendente na parte superior para selecionar uma conta diferente de laboratório. Consulte laboratórios na conta de laboratório selecionado. 

## <a name="delete-a-classroom-lab"></a>Eliminar um laboratório de sala de aulas
1. No mosaico para o laboratório, selecione as reticências (…) no canto. 

    ![Selecionar o laboratório](../media/how-to-manage-classroom-labs/select-three-dots.png)
2. Selecione **Eliminar**. 

    ![Eliminar botão](../media/how-to-manage-classroom-labs/delete-button.png)
3. No **eliminar laboratório** caixa de diálogo, selecione **eliminar**. 

    ![Eliminar a caixa de diálogo](../media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)
 
## <a name="manage-student-vms"></a>Gerir estudante VMs
Depois de registar os estudantes que estejam com os serviços de laboratório do Azure utilizando o registo de ligação fornecida aos mesmos, verá as VMs atribuídas para os estudantes que estejam no **máquinas virtuais** separador. 

![Máquinas virtuais atribuídas ao estudantes](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

Pode efetuar as seguintes tarefas num estudante VM: 

- Pare uma VM se a VM está em execução. 
- Inicie uma VM se a VM está parada. 
- Ligue à VM. 
- Elimine a VM. 

## <a name="next-steps"></a>Passos Seguintes
Introdução à configuração de um laboratório com o Azure Lab Services:

- [Configurar um laboratório de sala de aula](how-to-manage-classroom-labs.md)
- [Configurar um laboratório](../tutorial-create-custom-lab.md)
