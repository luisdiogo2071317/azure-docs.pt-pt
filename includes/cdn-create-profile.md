---
title: incluir ficheiro
description: incluir ficheiro
services: cdn
author: dksimpson
ms.service: cdn
ms.topic: include
ms.date: 04/04/2018
ms.author: rli; v-deasim
ms.custom: include file
ms.openlocfilehash: 692364e9b2e78b3bd1f63137148dfbc680364737
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
## <a name="create-a-new-cdn-profile"></a>Criar um novo perfil da CDN

Um perfil da CDN é um contentor de pontos finais da CDN e especifica um escalão de preço.

1. No portal do Azure, no canto superior esquerdo, selecione **crie um recurso**.
    
    O **novo** painel aparece.
   
2. Selecione **Web + móvel**, em seguida, **CDN**.
   
    ![Selecione o recurso CDN](./media/cdn-create-profile/cdn-new-resource.png)

    O **perfil da CDN** painel aparece.

    Utilize as definições especificadas na tabela a seguir a imagem.
   
    ![Novo perfil da CDN](./media/cdn-create-profile/cdn-new-profile.png)

    | Definição  | Valor |
    | -------- | ----- |
    | **Nome** | Introduza *my-cdn-perfil-123* para o nome do perfil. Este nome deve ser globalmente exclusivo; Se já se encontra em utilização, pode introduzir uma diferente. |
    | **Subscrição** | Selecione uma subscrição do Azure a partir da lista pendente.|
    | **Grupo de recursos** | Selecione **criar nova** e introduza *my-resource-grupo-123* para o nome do grupo de recursos. Este nome deve ser globalmente exclusivo; Se já se encontra em utilização, pode introduzir uma diferente. | 
    | **Localização do grupo de recursos** | Selecione **EUA Central** na lista pendente. |
    | **Escalão de preço** | Selecione **Standard da Verizon** na lista pendente. |
    | **Criar um novo ponto final da CDN agora** | Deixe não seleccionado. |  
   
3. Selecione **afixar ao dashboard** para guardar o perfil ao seu dashboard depois de criado.
    
4. Selecione **criar** para criar o perfil. 

