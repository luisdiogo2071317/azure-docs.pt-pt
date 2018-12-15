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
ms.openlocfilehash: e3ae11adf84e858429cba4643802300f7915a166
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53412943"
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
        Especifique o nome do volume que está a criar.  
        
        O nome tem de ser exclusivo dentro de um grupo de recursos. Tem de ser, pelo menos, três carateres de comprimento.  Pode utilizar carateres alfanuméricos.

    * **Caminho do ficheiro**     
        Especifique o caminho de ficheiro que será utilizado para criar o caminho de exportação para o novo volume. O caminho de exportação é usado para montar e aceder ao volume.   
        
        Um destino de montagem é o ponto final do endereço IP do serviço NFS. É gerado automaticamente.   
        
        O nome de ficheiro pode conter apenas letras, números e hífenes ("-"). Tem de ter entre 16 e 40 carateres. 

    * **Quota**  
        Especifique a quantidade de armazenamento lógico que está atribuída ao volume.  

        O campo **Quota disponível** mostra a quantidade de espaço não utilizado no conjunto de capacidade escolhido que pode usar para criar um novo volume. O tamanho do novo volume não pode exceder a quota disponível.

    *   **Rede virtual**  
        Especifique a rede virtual do Azure (Vnet) a partir da qual pretende aceder ao volume.  
        A Vnet que especificar tem de ter uma sub-rede de delegado ao serviço ficheiros do Azure NetApp. O serviço de ficheiros do Azure NetApp pode ser acedido apenas a partir da mesma Vnet ou de uma Vnet que está na mesma região que o volume através de Vnet peering. Também pode acessar o volume a partir da sua rede no local através de Express Route. 

    * **Sub-rede**  
        Especifique a sub-rede que pretende utilizar para o volume.  
        A sub-rede que especificar deve ser delegada para o serviço de ficheiros do Azure NetApp. Pode criar uma nova sub-rede, selecionando **criar novo** sob o campo de sub-rede.  
<!--
    ![Restored new volume](../media/azure-netapp-files/azure-netapp-files-snapshot-new-volume.png) 
-->

5. Clique em **OK**.   
    O novo volume para que o instantâneo é restaurado aparece no painel do Volumes.

