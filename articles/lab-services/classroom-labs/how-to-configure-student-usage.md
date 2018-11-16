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
ms.date: 11/14/2018
ms.author: spelluru
ms.openlocfilehash: 30c033b487fe58d017080b02c257502f82338164
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51710045"
---
# <a name="configure-usage-settings-and-policies"></a>Configurar as políticas e definições de utilização
Este artigo descreve como configurar o número de utilizadores para o laboratório, obtê-los registado com o laboratório, controlar o número de horas que podem utilizar a VM e muito mais. 


## <a name="specify-the-number-of-users-allowed-into-the-lab"></a>Especifique o número de utilizadores com permissão para o laboratório

1. Selecione **Política de utilização**. 
2. Nas definições da **Política de utilização**, introduza o **número de utilizadores** que têm permissão para utilizar o laboratório.
3. Selecione **Guardar**. 

    ![Política de utilização](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="send-registration-link-to-students"></a>Enviar a ligação de registo para os alunos

1. Mude para o **usuários** vista ao selecionar **utilizadores** no menu da esquerda. 
2. Selecione **obter a ligação de registo** mosaico.

    ![Ligação de registo do aluno](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. Na caixa de diálogo **Registo de utilizador**, selecione o botão **Copiar**. A ligação é copiada para a área de transferência. Cole-a num editor de e-mail e envie um e-mail para o aluno. 

    ![Ligação de registo do aluno](../media/tutorial-setup-classroom-lab/registration-link.png)
2. Na caixa de diálogo **Registo de utilizador**, selecione **Fechar**. 
4. Partilhe a ligação de registo com um estudante, para que este possa registar-se na turma. Se tiver o **restringir a opção** definam enabled e tem uma lista de utilizadores na lista, fazer as seguintes ações:
    1. Selecione o **endereço de e-mail** do utilizador na lista. 
    2. Verá uma janela do seu programa de e-mail predefinida com o **TO** endereço preenchidos. 
    3. Colar o **URL de registo** que copiou anteriormente. 
    4. Enviar a **e-mail**. 

## <a name="view-users-registered-with-the-lab"></a>Ver utilizadores registados no laboratório

Selecione **utilizadores** no menu da esquerda para ver a lista de utilizadores registados com o laboratório. 

![Lista de utilizadores registados com o laboratório](../media/how-to-configure-student-usage/users-list.png)

## <a name="set-quotas-per-user"></a>Definir quotas por utilizador

1. Selecione **utilizadores** no menu da esquerda.
2. Selecione **Quota por utilizador: ilimitado** na barra de ferramentas. 
3. Sobre o **Quota por usuário** página, selecione **limitar o número de horas, um utilizador pode utilizar uma VM**. 
4. Para **quantas horas que pretende dar a cada usuário**, introduza o número de horas e selecione **guardar**. 

    ![Número de horas por utilizador](../media/how-to-configure-student-usage/number-of-hours-per-user.png)
5. Ver o número de horas na barra de ferramentas agora: **Quota por utilizador: &lt;número de horas&gt;**. 

    ![Quota de por utilizador](../media/how-to-configure-student-usage/quota-per-user.png)

## <a name="add-users-to-the-lab"></a>Adicionar utilizadores ao laboratório
Se tiver o **restringir o acesso** ativado, adicione utilizadores (endereços de e-mail) à lista.

1. Selecione **utilizadores** no menu da esquerda.
2. Selecione **adicionar utilizadores** na barra de ferramentas. 
3. Sobre o **adicionar utilizadores** página, introduza os endereços de e-mail dos utilizadores em linhas separadas ou numa única linha, separados por ponto e vírgula. 

    ![Endereços de e-mail do utilizador](../media/how-to-configure-student-usage/add-users-email-addresses.png)
4. Selecione **Guardar**. Ver os endereços de e-mail dos utilizadores e os respetivos Estados (registados ou não) na lista. 

    ![Lista de utilizadores](../media/how-to-configure-student-usage/users-list-new.png)

### <a name="add-users-by-uploading-a-csv-file"></a>Adicionar utilizadores ao carregar um ficheiro CSV
Também pode adicionar utilizadores ao carregar um ficheiro CSV com endereços de e-mail dos utilizadores.

1. Selecione **carregar CSV** na barra de ferramentas.
2. Selecione o ficheiro CSV com endereços de e-mail do utilizador. Todos os endereços de e-mail devem ser uma coluna, quando abri-lo com o Excel. 

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
Consulte os seguintes artigos:

- [Como administrador, criar e gerir contas de laboratório](how-to-manage-lab-accounts.md)
- [Como proprietário de um laboratório, criar e gerir laboratórios](how-to-manage-classroom-labs.md)
- [Como proprietário de um laboratório, configurar e publicar modelos](how-to-create-manage-template.md)
- [Como um utilizador de laboratório, acessar os laboratórios de sala de aula](how-to-use-classroom-lab.md)
