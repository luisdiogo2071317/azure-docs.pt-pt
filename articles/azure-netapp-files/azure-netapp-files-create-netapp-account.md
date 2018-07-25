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
ms.openlocfilehash: ad8cc550ce69e4dc4c19a569718fa873a65b3620
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010349"
---
# <a name="create-a-netapp-account"></a>Criar uma conta NetApp
Criar uma conta NetApp permite-lhe configurar um conjunto de capacidade e, em seguida, criar um volume. Pode utilizar o painel do Azure NetApp Files para criar uma nova conta NetApp.

## <a name="before-you-begin"></a>Antes de começar
Tem de estar na lista de permissões para aceder ao Fornecedor de Recursos do Azure, Microsoft.NetApp, e configurado para utilizar o serviço Azure NetApp Files.  

[Página de inscrição na Pré-visualização Pública do Azure NetApp Files](https://aka.ms/nfspublicpreview). 

## <a name="steps"></a>Passos 

1. Localize o URL do portal do Azure de pré-visualização no convite de pré-visualização e inicie sessão no portal. 
2.  Aceda ao painel do Azure NetApp Files através de um dos seguintes métodos:  
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
    Atualmente, o serviço Azure NetApp Files é suportado apenas na região E.U.A Leste.  

    ![Nova conta NetApp](../media/azure-netapp-files/azure-netapp-files-new-netapp-account.png)


5. Clique em **Criar**.     
  A conta NetApp que criou aparece agora no painel do Azure NetApp Files. 

## <a name="next-steps"></a>Passos seguintes  

1. [Configurar um conjunto de capacidade](azure-netapp-files-set-up-capacity-pool.md)
2. [Criar um volume para o Azure NetApp Files](azure-netapp-files-create-volumes.md)
3. [Configurar a política de exportação para um volume (opcional)](azure-netapp-files-configure-export-policy.md)
