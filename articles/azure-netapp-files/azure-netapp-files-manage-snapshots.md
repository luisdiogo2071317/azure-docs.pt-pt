---
title: Gerir instantâneos com ficheiros de NetApp do Azure | Documentos da Microsoft
description: Descreve como criar um instantâneo de sob demanda para um volume ou o restauro a partir de um instantâneo para um novo volume através de ficheiros do Azure NetApp.
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
ms.topic: how-to-article
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: 48cb88b9815ba723d93c18caf63f33b50eea850c
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009197"
---
# <a name="manage-snapshots-by-using-azure-netapp-files"></a>Gerir instantâneos com ficheiros de NetApp do Azure
Pode utilizar ficheiros de NetApp do Azure para criar um instantâneo de sob demanda para um volume ou restaurar a partir de um instantâneo para um novo volume.

## <a name="create-an-on-demand-snapshot-for-a-volume"></a>Criar um instantâneo de sob demanda para um volume
Pode criar instantâneos apenas sob demanda.  Políticas de instantâneos não são atualmente suportadas.  
1.  No painel do Volume de gerir, clique em **instantâneos**, em seguida, clique em **+ adicionar instantâneo** para criar um instantâneo de sob demanda para um volume.

2.  Na janela novo instantâneo, forneça um nome para o novo instantâneo que está a criar.   

3. Clique em **OK**. 


## <a name="restore-a-snapshot-to-a-new-volume"></a>Restaurar um instantâneo para um novo volume
Atualmente, pode restaurar um instantâneo apenas para um novo volume. 
1. Vá para o **gerir instantâneos** painel a partir do painel para apresentar a lista de instantâneo do Volume. 
2. Selecione um instantâneo para restaurar.  
3. O nome de instantâneo com o botão direito e selecione **restaurar para novo volume** da opção de menu.  

    ![Restaurar o instantâneo para o novo volume](../media/azure-netapp-files/azure-netapp-files-snapshot-restore-to-new-volume.png)

4. Na janela do novo Volume, fornecem informações para o novo volume:  
    * **Nome**   
        Especifique o nome para o volume que está a criar.  
        
        O nome tem de ser exclusivo dentro de um grupo de recursos. Tem de ser, pelo menos, 3 carateres de comprimento.  Pode utilizar quaisquer carateres alfanuméricos.

    * **Caminho do ficheiro**     
        Especifique o caminho de ficheiro que será utilizado para criar o caminho de exportação para o novo volume. O caminho de exportação é utilizado para montar e aceder ao volume.   
        
        Um destino de montagem é o ponto final do endereço IP do serviço NFS. É gerado automaticamente.   
        
        O nome de caminho de ficheiro pode conter letras, números e hífenes ("-") apenas. Tem de estar entre 16 e 40 carateres de comprimento. 

    * **Quota**  
        Especifique a quantidade de armazenamento lógico, que é atribuído ao volume.  

        O **quota disponível** campo mostra a quantidade de espaço não utilizado no agrupamento de escolhido de capacidade que pode utilizar para a criação de um novo volume. O tamanho do novo volume não pode exceder a quota disponível.

    *   **Rede virtual**  
        Especifique a rede virtual do Azure (Vnet) do qual pretende aceder ao volume. 
        
        A Vnet que especificar tem de ter ficheiros de NetApp do Azure configurado. O serviço de ficheiros do Azure NetApp pode ser acedido apenas a partir de uma Vnet que está na mesma localização que o volume.  

    ![Restaurado novo volume](../media/azure-netapp-files/azure-netapp-files-snapshot-new-volume.png) 
    
5. Clique em **OK**.   
    O novo volume para que o instantâneo é restaurado aparece no painel do Volumes.

