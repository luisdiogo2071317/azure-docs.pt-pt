---
title: Moderada imagens utilizando listas personalizadas no Moderator conteúdo do Azure | Microsoft Docs
description: Test-Drive listas de imagem personalizada na consola do conteúdo Moderator API.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: 2d714f017be16d978ffbb877a2b7e78e1caf9169
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351824"
---
# <a name="moderate-with-custom-image-lists-in-the-api-console"></a>Moderada com listas de imagem personalizada na consola de API

Utilizar o [API de gestão da lista](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672) no Moderator conteúdo do Azure para criar listas personalizadas de imagens. Utilize as listas de imagens personalizadas com a API de moderação interrupção de imagem. A operação de moderação interrupção imagem avalia a imagem. Se criar listas personalizadas, a operação também compara-os para as imagens no seu listas personalizadas. Pode utilizar listas personalizadas para bloquear ou permitir que a imagem.

> [!NOTE]
> Há um limite máximo de **apresenta uma lista de imagem de 5** com cada lista para **não pode exceder 10 000 imagens**.
>

Utilize a API de gestão da lista para efetuar as seguintes tarefas:

- Crie uma lista.
- Adicione imagens a uma lista.
- Imagens de ecrã contra as imagens numa lista.
- Elimine imagens a partir de uma lista.
- Elimine uma lista.
- Edite as informações da lista.
- Atualize o índice, para que as alterações à lista de estão incluídas numa nova análise.

## <a name="use-the-api-console"></a>Utilize a consola de API
Antes de pode test-drive API na consola do online, terá da chave de subscrição. Isto está localizado no **definições** separador o **Ocp-Apim-Subscription-Key** caixa. Para obter mais informações, consulte [descrição geral](overview.md).

## <a name="refresh-search-index"></a>Atualizar o índice de pesquisa

Depois de efetuar alterações a uma lista de imagem, tem de atualizar o respetivo índice para as alterações sejam incluídos em verificações futuras. Este passo é semelhante à forma como um motor de busca no ambiente de trabalho (se estiver ativada) ou um motor de busca web continuamente atualiza respetivo índice para incluir novos ficheiros ou páginas.

1.  No [referência da API de gestão de lista de imagem](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), no menu à esquerda, selecione **apresenta uma lista de imagem**e, em seguida, selecione **atualizar o índice de pesquisa**.

  O **imagem lista - atualizar o índice de pesquisa** é aberta a página.

2. Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização. 
 
    ![Apresenta uma lista de imagem - seleção de região de página de índice de pesquisa de atualização](images/test-drive-region.png)

    O **imagem lista - atualizar o índice de pesquisa** é aberta a consola de API.

3.  No **listId** box, introduza o ID da lista. Introduza a chave de subscrição e, em seguida, selecione **enviar**.

  ![Apresenta uma lista de imagem - caixa de conteúdo de resposta do índice de pesquisa de atualização de consola](images/try-image-list-refresh-1.png)


## <a name="create-an-image-list"></a>Criar uma lista de imagens

1.  Vá para o [referência da API de gestão de lista de imagem](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672).

  O **apresenta uma lista de imagem - criar** é aberta a página. 

3.  Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização.

    ![Apresenta uma lista de imagem - seleção de região de página de criação](images/test-drive-region.png)

    O **apresenta uma lista de imagem - criar** é aberta a consola de API.
 
4.  No **Ocp-Apim-Subscription-Key** box, introduza a chave de subscrição.

5.  No **corpo do pedido** box, introduza valores para **nome** (por exemplo, MyList) e **Descrição**.

  ![Apresenta uma lista de imagem - nome de corpo do pedido de consola e uma descrição de criação](images/try-terms-list-create-1.png)

6.  Utilize marcadores de posição de pares chave-valor para atribuir metadados descritivos mais à sua lista.

        {
           "Name": "MyExclusionList",
           "Description": "MyListDescription",
           "Metadata": 
           {
             "Category": "Competitors",
             "Type": "Exclude"
           }
        }

  Adicione metadados de lista como pares chave-valor e não as imagens reais.
 
7.  Selecione **Enviar**. É criada a sua lista. Tenha em atenção o **ID** valor que está associado a nova lista. Este ID é necessário para outras funções de gestão da lista de imagem.

  ![Apresenta uma lista de imagem – criar consola resposta conteúdo caixa apresenta o ID de lista](images/try-terms-list-create-2.png)
 
8.  Em seguida, adicione imagens ao MyList. No menu à esquerda, selecione **imagem**e, em seguida, selecione **adicionar imagem de**.

  O **imagem - adicionar imagem** é aberta a página. 

9. Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização.

  ![Imagem – adicionar seleção de região da página de imagem](images/test-drive-region.png)

  O **imagem - adicionar imagem** é aberta a consola de API.
 
10. No **listId** caixa, introduza o ID de lista que é gerado e, em seguida, introduza o URL da imagem que pretende adicionar. Introduza a chave de subscrição e, em seguida, selecione **enviar**.

11. Para verificar se a imagem foi adicionada à lista, no menu à esquerda, selecione **imagem**e, em seguida, selecione **obter todos os Ids de imagem**.

  O **imagem - obter todos os Ids de imagem** é aberta a consola de API.
  
12. No **listId** caixa, introduza o ID de lista e, em seguida, introduza a chave de subscrição. Selecione **Enviar**.

  ![Imagem - Get todos os Ids de imagem consola resposta conteúda caixa apresenta uma lista de imagens que introduziu](images/try-image-list-create-11.png)
 
10. Adicione alguns mais imagens. Agora que criou uma lista de imagens personalizada, experimente [avaliar imagens](try-image-api.md) utilizando a lista de imagem personalizada. 

## <a name="delete-images-and-lists"></a>Eliminar imagens e listas

Eliminar uma imagem ou uma lista é simples. Pode utilizar a API para efetuar as seguintes tarefas:

- Elimine uma imagem. (**Imagem - eliminar**)
- Elimine todas as imagens numa lista sem a eliminação da lista. (**Imagem - eliminar todas as imagens**)
- Elimine uma lista e todos os respetivos conteúdos. (**Apresenta uma lista de imagem - eliminar**)

Neste exemplo elimina uma única imagem:

1. No [referência da API de gestão de lista de imagem](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), no menu à esquerda, selecione **imagem**e, em seguida, selecione **eliminar**. 

  O **imagem - eliminar** é aberta a página.

2. Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização. 

  ![Imagem - seleção de região de página de eliminação](images/test-drive-region.png)
 
  O **imagem - eliminar** é aberta a consola de API.
 
3.  No **listId** box, introduza o ID de eliminar uma imagem a partir da lista.  Este é o número devolvido a **imagem - obter todos os Ids de imagem** MyList na consola. Em seguida, introduza o **ImageId** da imagem para eliminar. 

No nosso exemplo, o ID de lista é **58953**, o valor para **ContentSource**. A imagem ID é **59021**, o valor para **ContentIds**.

4.  Introduza a chave de subscrição e, em seguida, selecione **enviar**.

5.  Para verificar que a imagem foi eliminada, utilize o **imagem - obter todos os Ids de imagem** consola.
 
## <a name="change-list-information"></a>Informações de alteração de lista

Pode editar o nome e descrição de uma lista e adicionar os itens de metadados.

1.  No [referência da API de gestão de lista de imagem](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), no menu à esquerda, selecione **apresenta uma lista de imagem**e, em seguida, selecione **detalhes da atualização**. 

  O **imagem lista - detalhes da atualização** é aberta a página.

2. Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização.  

    ![Apresenta uma lista de imagem - seleção de região de página de detalhes da atualização](images/test-drive-region.png)

    O **imagem lista - detalhes da atualização** é aberta a consola de API.
 
3.  No **listId** caixa, introduza o ID de lista e, em seguida, introduza a chave de subscrição.

4.  No **corpo do pedido** caixa, faça as suas edições e, em seguida, selecione o **enviar** botão na página.

  ![Apresenta uma lista de imagem - detalhes da atualização da consola edições de corpo do pedido](images/try-terms-list-change-1.png)
 

## <a name="next-steps"></a>Passos Seguintes

Utilizar a API REST no seu código ou começar com o [imagem apresenta uma lista de início rápido do .NET](image-lists-quickstart-dotnet.md) para integrar com a sua aplicação.
