---
title: Definir um conjunto de capacidade para o Azure NetApp Files | Microsoft Docs
description: Descreve como configurar um conjunto de capacidade para que possa criar volumes no mesmo.
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
ms.openlocfilehash: 0e9203f5b4e2a9043e242b804c82017cf6fc3ee1
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010808"
---
# <a name="set-up-a-capacity-pool"></a>Configurar um conjunto de capacidade
Configurar um conjunto de capacidade permite-lhe criar volumes no mesmo.  

## <a name="before-you-begin"></a>Antes de começar 
É necessário que já tenha criado uma conta NetApp.   

[Criar uma conta NetApp](azure-netapp-files-create-netapp-account.md)

## <a name="steps"></a>Passos 

1. Aceda ao painel de gestão da sua conta NetApp e, em seguida, no painel de navegação, selecione **Conjuntos de capacidade**.

2. Clique em **+ Adicionar conjuntos** para criar um novo conjunto de capacidade.   
    É apresentada a janela Novo Conjunto de Capacidade.

3. Forneça as seguintes informações para o novo conjunto de capacidade:  
  * **Nome**  
    Especifique o nome do conjunto de capacidade.  
    O nome de conjunto de capacidade tem de ser exclusivo para cada conta NetApp.

  * **Nível de serviço**   
    Este campo mostra o objetivo de desempenho do conjunto de capacidade.  
    Atualmente, apenas está disponível o nível de serviço Premium. 

  *  **Tamanho**     
      Especifique o tamanho do conjunto de capacidade que está a comprar.        
      O tamanho mínimo do conjunto de capacidade é 4 TiB. Pode criar um conjunto com um tamanho que seja um múltiplo de 4 TiB.   
      
      ![Novo conjunto de capacidade](../media/azure-netapp-files/azure-netapp-files-new-capacity-pool.png)

4. Clique em **OK**.

## <a name="next-steps"></a>Passos seguintes 

1. [Criar um volume para o Azure NetApp Files](azure-netapp-files-create-volumes.md)
2. [Configurar a política de exportação para um volume (opcional)](azure-netapp-files-configure-export-policy.md)

