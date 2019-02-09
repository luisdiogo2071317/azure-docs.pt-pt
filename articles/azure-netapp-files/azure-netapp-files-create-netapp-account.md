---
title: Criar uma conta NetApp para aceder ao Azure NetApp Files | Microsoft Docs
description: Descreve como aceder ao Azure NetApp Files e criar uma conta NetApp, para poder configurar um conjunto de capacidade e criar um volume.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: 47b9d25f8db2241bb578528780e28f43d56371e5
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2019
ms.locfileid: "55963085"
---
# <a name="create-a-netapp-account"></a>Criar uma conta NetApp
Criar uma conta NetApp permite-lhe configurar um conjunto de capacidade e, em seguida, criar um volume. Pode utilizar o painel do Azure NetApp Files para criar uma nova conta NetApp.

## <a name="before-you-begin"></a>Antes de começar
Tem de ter registado a sua subscrição para utilizar o fornecedor de recursos de NetApp e a funcionalidade de pré-visualização pública.

[Registre-se para os ficheiros do Azure NetApp](azure-netapp-files-register.md)

## <a name="steps"></a>Passos 

1. Inicie sessão no Portal do Azure. 
2. Aceda ao painel do Azure NetApp Files através de um dos seguintes métodos:  
  * Procure **Azure NetApp Files** na caixa de pesquisa do portal do Azure.  
  * Clique em **Todos os serviços** na navegação e, em seguida, filtre para Azure NetApp Files.  

  Pode adicionar o painel do Azure NetApp Files aos favoritos ao clicar no ícone de estrela junto ao mesmo. 

3. Clique em **+ Adicionar** para criar uma nova conta NetApp.  
  É apresentada a janela da nova conta NetApp.  

4. Forneça as seguintes informações para a conta NetApp: 
  * **Nome da conta**  
    Especifique um nome exclusivo para a subscrição.
  *  **Subscrição**  
    Selecione uma subscrição a partir das subscrições existentes.
  * **Grupo de recursos**   
    Utilize um Grupo de Recursos existente ou crie um novo.
  * **Localização**  
    Selecione a região onde quer ter a conta e os respetivos recursos subordinados a localizar.  

    ![Nova conta NetApp](../media/azure-netapp-files/azure-netapp-files-new-netapp-account.png)


5. Clique em **Criar**.     
  A conta NetApp que criou aparece agora no painel do Azure NetApp Files. 

## <a name="next-steps"></a>Passos Seguintes  

[Configurar um conjunto de capacidade](azure-netapp-files-set-up-capacity-pool.md)

