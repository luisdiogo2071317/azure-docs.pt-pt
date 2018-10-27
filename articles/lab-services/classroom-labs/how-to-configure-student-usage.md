---
title: Configurar definições de utilização nos laboratórios de sala de aula do Azure Lab Services | Documentos da Microsoft
description: Saiba como configurar o número de utilizadores para o laboratório, obtê-los registado com o laboratório, controlar o número de horas que podem utilizar a VM e muito mais.
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
ms.openlocfilehash: a91d5826f52b2beb05f2d9a08f3646bd776b294e
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50142480"
---
# <a name="configure-usage-settings-and-policies"></a>Configurar as políticas e definições de utilização
Este artigo descreve como configurar o número de utilizadores para o laboratório, obtê-los registado com o laboratório, controlar o número de horas que podem utilizar a VM e muito mais. 


## <a name="specify-the-number-of-users-allowed-into-the-lab"></a>Especifique o número de utilizadores com permissão para o laboratório

1. Selecione **Política de utilização**. 
2. Nas definições da **Política de utilização**, introduza o **número de utilizadores** que têm permissão para utilizar o laboratório.
3. Selecione **Guardar**. 

    ![Política de utilização](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="send-registration-link-to-users"></a>Enviar ligação de registo para os utilizadores

1. Mude para a vista **Dashboard**. 
2. Selecione o mosaico **Registo de utilizador**.

    ![Ligação de registo do aluno](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. Na caixa de diálogo **Registo de utilizador**, selecione o botão **Copiar**. A ligação é copiada para a área de transferência. Cole-a num editor de e-mail e envie um e-mail para o aluno. Os estudantes utilizam a ligação para navegar para a página que ajuda a com o registo com o laboratório. 

    ![Ligação de registo do aluno](../media/tutorial-setup-classroom-lab/registration-link.png)
2. Na caixa de diálogo **Registo de utilizador**, selecione **Fechar**. 

## <a name="view-users-registered-with-the-lab"></a>Ver utilizadores registados no laboratório

Selecione **utilizadores** no menu da esquerda para ver a lista de utilizadores registados com o laboratório. 

![Lista de utilizadores registados com o laboratório](../media/how-to-configure-student-usage/users-list.png)

## <a name="set-quotas-per-user"></a>Definir quotas por utilizador

1. Selecione **definições de utilizador** no menu da esquerda.
2. Selecione **Quta por utilizador** mosaico. 
3. Selecione **limitar o número de horas, um utilizador pode utilizar uma VM**. 
4. Para **quantas horas que pretende dar a cada utilizador?**, introduza o número de horas e selecione **guardar**. 

    ![Número de horas por utilizador](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
5. Consulte o número de horas sobre a **Quota por utilizador** mosaico agora. 

    ![Quota de por utilizador](../media/how-to-configure-student-usage/quota-per-user.png)

## <a name="manage-user-vms"></a>Gerir VMs do utilizador
Assim que os estudantes registar com o Azure Lab Services usando o registro link fornecido aos mesmos, verá as VMs atribuídas aos estudantes na **máquinas virtuais** separador. 

![Máquinas de virtuais atribuídas para estudantes](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

Pode efetuar as seguintes tarefas numa VM de estudante: 

- Pare uma VM se a VM está em execução. 
- Inicie uma VM se a VM é parada. 
- Ligue à VM. 
- Elimine a VM. 
- Ver o número de horas que os utilizadores utilizaram a máquina virtual. 


## <a name="next-steps"></a>Passos Seguintes
Introdução à configuração de um laboratório com o Azure Lab Services:

- [Configurar um laboratório de sala de aula](how-to-manage-classroom-labs.md)
- [Configurar um laboratório](../tutorial-create-custom-lab.md)
