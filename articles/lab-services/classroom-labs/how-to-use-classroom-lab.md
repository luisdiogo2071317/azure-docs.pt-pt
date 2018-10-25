---
title: Como aceder a um laboratório de sala de aula no Azure Lab Services | Microsoft Docs
description: Neste tutorial, irá aceder a máquinas virtuais num laboratório de sala de aula configurado por um professor.
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
ms.date: 10/17/2018
ms.author: spelluru
ms.openlocfilehash: 4b137396dd6a8ff924c9380aeb87a81b95f91414
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49466227"
---
# <a name="how-to-access-a-classroom-lab-in-azure-lab-services"></a>Como aceder a um laboratório de sala de aula no Azure Lab Services
Este artigo descreve como aceder a um laboratório de sala de aula, ligar à VM no laboratório e parar a VM. 

## <a name="view-all-the-classroom-labs"></a>Ver todos os laboratórios de sala de aula

1. Navegue para o **URL de registo** que recebeu do professor/educador. 
2. Inicie sessão no serviço com a sua conta escolar para concluir o registo. 
3. Depois de se registar, confirme se vê as máquinas virtuais dos laboratórios a que tem acesso. 

    ![Ver todos os laboratórios](../media/how-to-use-classroom-lab/all-labs.png)

## <a name="connect-to-the-virtual-machine-in-a-classroom-lab"></a>Ligar à máquina virtual num laboratório de sala de aula

1. Selecione **Ligar** no mosaico que representa a máquina virtual do laboratório a que pretende aceder.

    ![Ver todos os laboratórios](../media/how-to-use-classroom-lab/connect-button.png)
2. A máquina virtual demora alguns minutos a ficar pronta.

    ![Preparar a máquina virtual](../media/how-to-use-classroom-lab/getting-virtual-machine-ready.png)
3. Guarde o ficheiro RDP no disco rígido e abra-o. 
    
    ![Guardar o ficheiro RDP](../media/how-to-use-classroom-lab/save-rdp-file.png)
4. Utilize o **nome de utilizador** e a **palavra-passe** que recebeu do seu professor/educador para iniciar sessão na máquina. 

## <a name="stop-the-virtual-machine-in-a-classroom-lab"></a>Parar a máquina virtual num laboratório de sala de aula

Selecione **Parar** no mosaico que representa o laboratório de sala de aula. Quando a VM está parada, o botão **Iniciar** no mosaico é ativado. 

## <a name="next-steps"></a>Passos seguintes
Introdução à configuração de um laboratório com o Azure Lab Services:

- [Configurar um laboratório de sala de aula](how-to-manage-classroom-labs.md)
- [Configurar um laboratório](../tutorial-create-custom-lab.md)

 