---
title: Acompanhar a utilização de um laboratório no Azure Lab Services | Microsoft Docs
description: Neste tutorial, enquanto criador/proprietário do laboratório, vai acompanhar a utilização do mesmo.
services: lab-services
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
ms.date: 07/23/2018
ms.author: spelluru
ms.openlocfilehash: 710d157dcf4c6d060e59bcfbb69455e2ddc91bdd
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39450135"
---
# <a name="tutorial-track-usage-of-a-lab-in-azure-lab-service"></a>Tutorial: Acompanhar a utilização de um laboratório no Azure Lab Services
Este tutorial mostra-lhe como é que os criadores/proprietários de laboratórios podem acompanhar a utilização dos laboratórios.

Neste tutorial, irá realizar as seguintes ações:

> [!div class="checklist"]
> * Ver utilizadores registados no laboratório
> * Ver a utilização das VMs no laboratório
> * Gerir VMs de alunos 


## <a name="view-users-registered-with-the-lab"></a>Ver utilizadores registados no laboratório

1. Navegue para o [site do Azure Lab Services](https://labs.azure.com). 
2. Selecione **Iniciar sessão** e introduza as suas credenciais. O Azure Lab Services suporta contas organizacionais e contas Microsoft.
3. Na página **Os meus laboratórios**, selecione o laboratório cuja utilização quer acompanhar. 
4. Selecione o separador **Users** (Utilizadores). Verá os alunos que se registaram no laboratório. Selecione **Registration link** (Ligação de registo), copie a ligação e envie-a para alunos novos que ainda não se tenham registado no mesmo. 

    ![Utilizadores registados](../media/tutorial-track-usage/registered-users.png)

## <a name="view-the-usage-of-vms-in-the-lab"></a>Ver a utilização das VMs no laboratório 

1. Selecione **Máquinas virtuais**, no menu do lado esquerdo. 
2. Confirme que vê o estado das VMs e o número de horas que estiveram em execução. O tempo que despender na VM de um aluno não é contabilizado no tempo de utilização que aparece na última coluna. 

    ![Utilização de VM](../media/tutorial-track-usage/vm-usage.png)

## <a name="manage-student-vms"></a>Gerir VMs de alunos 
Quando passa o cursor sobre uma linha na lista de máquinas virtuais, verá controlos para executar as tarefas abaixo: 

- Ligar a uma VM
- Iniciar uma VM
- Parar uma VM
- Eliminar uma VM

![Controlos de VMs](../media/tutorial-track-usage/vm-controls.png) 



## <a name="next-steps"></a>Passos seguintes
Neste tutorial, aprendeu a encontrar utilizadores que se registaram no laboratório, bem como a acompanhar a utilização e a gerir as VMs no mesmo.

Para saber mais sobre os laboratórios de sala de aula, veja os tópicos nos [Guias de procedimentos](how-to-manage-lab-accounts.md).
