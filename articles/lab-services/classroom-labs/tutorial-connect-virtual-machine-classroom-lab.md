---
title: Aceder a um laboratório de sala de aula no Azure Lab Services | Microsoft Docs
description: Neste tutorial, vai aceder a máquinas virtuais num laboratório de sala de aula configurado por um professor.
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
ms.openlocfilehash: b9d140eedf69ed730985412ba1dcdca2d626b191
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706709"
---
# <a name="tutorial-access-a-classroom-lab-in-azure-lab-services"></a>Tutorial: Aceder a um laboratório de sala de aula no Azure Lab Services
Neste tutorial, como estudante vai ligar-se a uma máquina virtual (VM) num laboratório de sala de aula. 

Neste tutorial, vai realizar as seguintes ações:

> [!div class="checklist"]
> * Utilizar a ligação de registo 
> * Ligar à máquina virtual

## <a name="use-the-registration-link"></a>Utilizar a ligação de registo

1. Navegue para o **URL de registo** que recebeu do professor/educador. 
2. Inicie sessão no serviço com a sua conta escolar para concluir o registo. 
3. Depois de se registar, confirme se vê a máquina virtual do laboratório a que tem acesso. 
2. Aguarde até que a máquina virtual esteja pronta e, depois, **inicie-a**. Este processo demora algum tempo.  

    ![Iniciar a VM](../media/tutorial-connect-vm-in-classroom-lab/start-vm.png)

## <a name="connect-to-the-virtual-machine"></a>Ligar à máquina virtual

1. Selecione **Connect** no mosaico para a máquina virtual do laboratório que pretende aceder. 

    ![Ligar à VM](../media/tutorial-connect-vm-in-classroom-lab/connect-vm.png)
2. Guarde o ficheiro RDP para o disco rígido e abri-lo (partindo do princípio de que é uma VM do Windows)
3. Utilize o **nome de utilizador** e a **palavra-passe** que recebeu do seu professor/educador para iniciar sessão na máquina. 

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, acedeu a um laboratório de sala de aulas através da ligação de registo que recebeu do seu professor/educador.

Como proprietário de um laboratório, queira ver quem tiver registrado com o seu laboratório e controlar a utilização de VMs. Avance para o próximo tutorial para saber como controlar a utilização do laboratório:

> [!div class="nextstepaction"]
> [Track usage of a lab](tutorial-track-usage.md) (Acompanhar a utilização de um laboratório) 