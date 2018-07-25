---
title: Criar um volume para o Azure NetApp Files | Microsoft Docs
description: Descreve como criar um volume para o Azure NetApp Files.
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
ms.openlocfilehash: 42e475f4da2102bb8b010daec6e6451ba7f13848
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39011097"
---
# <a name="create-a-volume-for-azure-netapp-files"></a>Criar um volume para o Azure NetApp Files

O consumo de capacidade de um volume é contabilizado para a capacidade aprovisionada do seu conjunto.  Pode criar vários volumes num conjunto de capacidade, mas o consumo total de capacidade dos volumes não pode exceder o tamanho do conjunto. 

## <a name="before-you-begin"></a>Antes de começar 
Tem de ter um conjunto de capacidade já configurado.  

[Configurar um conjunto de capacidade](azure-netapp-files-set-up-capacity-pool.md)

## <a name="steps"></a>Passos 
1.  Clique no painel **Volumes**, a partir do painel Gerir Conjuntos de Capacidade. 
2.  Clique em **+ Adicionar volume** para criar um volume.  
    É apresentada a janela Novo Volume.

3.  Na janela Novo Volume, clique em **Criar** e forneça informações para os seguintes campos:   
    * **Nome**      
        Especifique o nome do volume que está a criar.   
        O nome tem de ser exclusivo dentro de um grupo de recursos. Tem de ter pelo menos 3 carateres.  Pode utilizar carateres alfanuméricos.

    * **Caminho do ficheiro**  
        Especifique o caminho de ficheiro que será utilizado para criar o caminho de exportação para o novo volume. O caminho de exportação é usado para montar e aceder ao volume.   
        Um destino de montagem é o ponto final do endereço IP do serviço NFS. É gerado automaticamente.    
        O nome de ficheiro pode conter apenas letras, números e hífenes ("-"). Tem de ter entre 16 e 40 carateres.  

    * **Quota**  
        Especifique a quantidade de armazenamento lógico que está atribuída ao volume.  
        O campo **Quota disponível** mostra a quantidade de espaço não utilizado no conjunto de capacidade escolhido que pode usar para criar um novo volume. O tamanho do novo volume não pode exceder a quota disponível.  

    * **Rede virtual**  
        Especifique a rede virtual do Azure (Vnet) a partir da qual pretende aceder ao volume. A Vnet que especificar tem de ter o Azure NetApp Files configurado. O serviço Azure NetApp Files só pode ser acedido a partir de uma Vnet que esteja na mesma localização do volume.   

    ![Novo volume](../media/azure-netapp-files/azure-netapp-files-new-volume.png)

4.  Clique em **OK**. 
 
Um volume herda a subscrição, grupo de recursos e atributos de localização do seu conjunto de capacidade. Para monitorizar o estado da implementação do volume, pode utilizar o separador Notificações.

## <a name="next-steps"></a>Passos seguintes  
[Configurar a política de exportação para um volume (opcional)](azure-netapp-files-configure-export-policy.md)

