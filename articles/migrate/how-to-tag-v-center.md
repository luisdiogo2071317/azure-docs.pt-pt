---
title: Grupo de VMs com o VMware vCenter etiquetagem | Microsoft Docs
description: "Descreve como criar um grupo antes de executar uma avaliação com o serviço Azure migrar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: 967c2a1fc0e5144c6c7d5c8c4a81cec3d77ffdb5
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2017
---
# <a name="group-vms-with-vcenter-tagging"></a>Grupo de VMs com a marcação vCenter

Este artigo descreve como criar um grupo de computadores para [Azure migrar](migrate-overview.md) avaliação utilizar a marcação no VMware vCenter. Especifique a categoria de etiqueta que pretende utilizar quando criar um grupo. 

## <a name="set-up-tagging"></a>Configurar a etiquetagem

Durante a implementação do Azure migrar, uma VM de migração no local do Azure Deteta máquinas em execução em anfitriões ESXi geridos por um servidor vCenter. Terá de configurar o vCenter etiquetagem antes do processo de deteção.

1. No VMware vSphere cliente Web, navegue para o servidor vCenter.
2. Clique em **etiquetas**, para rever as etiquetas atuais.
3. Para marcar uma VM, selecione **objetos relacionados** > **máquinas virtuais**e, em seguida, selecione a VM que pretende tag.
4. No **resumo** > **etiquetas**, clique em **atribuir**. 
5. Clique em **nova etiqueta**e especifique um nome de etiqueta e descrição.
6. Para crate uma categoria para a tag, selecione **nova categoria** na lista pendente.
7. Especifique um nome de categoria e a descrição e a cardinalidade. Em seguida, clique em **OK**.

    ![Etiquetas VM](./media/how-to-tag-v-center/vm-tag.png)

## <a name="use-tagging-to-create-groups"></a>Utilizar a marcação para criar grupos

1. Configurar deteção de máquinas no local, conforme descrito no [tutorial de avaliação do VMware](tutorial-assessment-vmware.md#run-the-collector-to-discover-vms).
2. No **categoria de Tag para agrupamento**, selecione a categoria de tag vCenter no qual o grupo de avaliação deve basear-se. Migrar do Azure cria automaticamente um grupo para a categoria selecionada.

    

## <a name="next-steps"></a>Passos seguintes

[Configurar a avaliação de VM de VMware](tutorial-assessment-vmware.md).
