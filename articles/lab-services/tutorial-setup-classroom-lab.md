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
ms.date: 04/19/2018
ms.author: spelluru
ms.openlocfilehash: 3e9f8d118c4413ec93b7dffcfa41b6ad4381b052
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-set-up-a-classroom-lab"></a>Tutorial: Configurar um laboratório de sala de aula 
Neste tutorial, vai configurar um laboratório de sala de aula com um conjunto de máquinas virtuais que serão utilizadas pelos alunos na sala de aulas.  

Neste tutorial, vai realizar as seguintes ações:

> [!div class="checklist"]
> * Criar um laboratório de sala de aula
> * Configurar o laboratório de sala de aula
> * Enviar a ligação de registo para os alunos

## <a name="create-a-classroom-lab"></a>Criar um laboratório de sala de aula

1. Navegue para o [site do Azure Lab Services](https://labs.azure.com).
2. Na janela **Novo Laboratório**, realize as ações abaixo: 
    1. Especifique um **nome** para o laboratório de sala de aula. 
    2. Selecione o **tamanho** da máquina virtual que planeia utilizar na sala de aula.
    3. Selecione a **imagem** a utilizar para criar a máquina virtual.
    4. Especifique as **credenciais predefinidas** para iniciar sessão nas máquinas virtuais no laboratório. 
    7. Selecione **Guardar**.

        ![Criar um laboratório de sala de aula](./media/tutorial-setup-classroom-lab/new-lab-window.png)
1. Verá a **home page** do laboratório. 
    
    ![Home page do laboratório da sala de aula](./media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Configurar a política de utilização

1. Selecione **Política de utilização**. 
2. Nas definições da **Política de utilização**, introduza o **número de utilizadores** que têm permissão para utilizar o laboratório.
3. Selecione **Guardar**. 

    ![Política de utilização](./media/tutorial-setup-classroom-lab/usage-policy-settings.png)

## <a name="set-up-the-template"></a>Configurar o modelo 
Um modelo num laboratório é de onde criamos máquinas virtuais para todos os utilizadores. Configure a máquina virtual do modelo para que seja configurado com exatamente o que pretende fornecer aos utilizadores do laboratório. Pode indicar um nome e uma descrição do modelo que os utilizadores do laboratório veem e definir a visibilidade como “Pública” para disponibilizar as instâncias do modelo de VM para os utilizadores do laboratório. 

### <a name="set-title-and-description"></a>Definir o título e a descrição
1. Na secção **Modelo**, selecione **Editar** (ícone de lápis) para o modelo. 
2. Na janela **Vista do utilizador**, introduza um **título** para o modelo.
3. Introduza a **descrição** do modelo.
4. Selecione **Guardar**.

    ![Descrição do laboratório da sala de aula](./media/tutorial-setup-classroom-lab/lab-description.png)

### <a name="make-instances-of-the-template-public"></a>Tornar públicas as instâncias do modelo
Depois de definir a visibilidade de um modelo como **Pública**, o Azure Lab Services cria VMs no laboratório através do modelo. O número de VMs criadas neste processo é igual ao número máximo de utilizadores permitidos no laboratório, o que pode configurar na política de utilização do laboratório. Todas as máquinas virtuais têm a mesma configuração do modelo. 

1. Selecione **Visibilidade** na secção **Modelo**. 
2. Na página **Disponibilidade**, selecione **Pública**.
    
    > [!IMPORTANT]
    > Depois de um modelo estar disponível publicamente, não é possível alterar o acesso para privado. 
3. Selecione **Guardar**.

    ![Disponibilidade](./media/tutorial-setup-classroom-lab/public-access.png)

## <a name="send-registration-link-to-students"></a>Enviar a ligação de registo para os alunos

1. Selecione o mosaico **Registo de utilizador**.
2. Na caixa de diálogo **Registo de utilizador**, selecione o botão **Copiar**. A ligação é copiada para a área de transferência. Cole-a num editor de e-mail e envie um e-mail para o aluno. 

    ![Ligação de registo do aluno](./media/tutorial-setup-classroom-lab/registration-link.png)

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, criou um laboratório de sala de aula e configurou o laboratório. Para saber como um aluno pode aceder a uma VM no laboratório através da ligação de registo, avance para o próximo tutorial:

> [!div class="nextstepaction"]
> [Ligar a uma VM no laboratório de sala de aula](tutorial-connect-virtual-machine-classroom-lab.md)

