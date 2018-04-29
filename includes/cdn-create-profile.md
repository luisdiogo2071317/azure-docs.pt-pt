---
title: incluir ficheiro
description: incluir ficheiro
services: cdn
author: SyntaxC4
ms.service: cdn
ms.topic: include
ms.date: 04/13/2018
ms.author: cfowler
ms.custom: include file
ms.openlocfilehash: 0db5f571324694f0518ffc4e92af985e5125d755
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
## <a name="create-a-new-cdn-profile"></a>Criar um novo perfil da CDN

Um perfil da CDN é um contentor de pontos finais da CDN e especifica um escalão de preço.

1. No portal do Azure portal, na parte superior esquerda, selecione **Criar um recurso**.
    
    O painel **Novo** é apresentado.
   
2. Selecione **Web + Móvel** e, em seguida, selecione **CDN**.
   
    ![Selecione o recurso CDN](./media/cdn-create-profile/cdn-new-resource.png)

    É apresentado o painel do **perfil da CDN**.

    Utilize as definições especificadas na tabela abaixo da imagem.
   
    ![Novo perfil da CDN](./media/cdn-create-profile/cdn-new-profile.png)

    | Definição  | Valor |
    | -------- | ----- |
    | **Nome** | Introduza *my-cdn-perfil-123* como nome do perfil. Este nome deve ser globalmente exclusivo; se já se encontrar em utilização, pode introduzir um diferente. |
    | **Subscrição** | Selecione uma subscrição do Azure na lista pendente.|
    | **Grupo de recursos** | Selecione **Criar novo** e introduza *my-resource-group-123* como nome do grupo de recursos. Este nome deve ser globalmente exclusivo; se já se encontrar em utilização, pode introduzir um diferente. | 
    | **Localização do grupo de recursos** | Selecione **EUA Central** na lista pendente. |
    | **Escalão de preço** | Selecione **Standard da Verizon** na lista pendente. |
    | **Agora, crie um novo ponto final da CDN** | Deixe por selecionar. |  
   
3. Selecione **Afixar ao dashboard** para guardar o perfil ao seu dashboard depois de criado.
    
4. Selecione **Criar** para criar o perfil. 

