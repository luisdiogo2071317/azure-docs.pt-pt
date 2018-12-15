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
ms.openlocfilehash: 55a1d16ce1617ecf7bc28c7c62de8557ceeea311
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53412916"
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

## <a name="next-steps"></a>Passos Seguintes 

[Delegar uma sub-rede para ficheiros do Azure NetApp](azure-netapp-files-delegate-subnet.md)


