---
title: Moderada de texto com listas de termo personalizado no Moderator conteúdo do Azure | Microsoft Docs
description: Test-Drive listas termo personalizada na consola do conteúdo Moderator API.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: 2542e4590781879408aafe8d072eceef157e02c9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351817"
---
# <a name="moderate-with-custom-term-lists-in-the-api-console"></a>Moderada com listas de termo personalizada na consola de API

Lista global dos termos de licenciamento no Azure conteúdo Moderator predefinido é suficiente para as necessidades de moderação interrupção mais conteúdas. No entanto, poderá ter de o ecrã para os termos que são específicos para a sua organização. Por exemplo, pode querer nomes de competitor etiquetas para revisão adicional. 

Utilize o [API de gestão da lista](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f) criar listas personalizadas dos termos de licenciamento para utilizar com a API de moderação interrupção de texto. O **texto - ecrã** operação analisa o texto para profanity e também compara o texto contra blacklists personalizados e partilhados.

> [!NOTE]
> Há um limite máximo de **apresenta uma lista de 5 termo** com cada lista para **não pode exceder 10 000 termos**.
>

Pode utilizar a API de gestão da lista para efetuar as seguintes tarefas:
- Crie uma lista.
- Adicione termos a uma lista.
- Termos de licenciamento do ecrã contra os termos de uma lista.
- Elimine termos numa lista.
- Elimine uma lista.
- Edite as informações da lista.
- Atualize o índice, para que as alterações à lista de estão incluídas numa nova análise.

## <a name="use-the-api-console"></a>Utilize a consola de API

Antes de pode test-drive API na consola do online, terá da chave de subscrição. Esta chave estiver localizada no **definições** separador o **Ocp-Apim-Subscription-Key** caixa. Para obter mais informações, consulte [descrição geral](overview.md).

## <a name="refresh-search-index"></a>Atualizar o índice de pesquisa

Depois de efetuar alterações a uma lista de prazo, tem de atualizar o respetivo índice para as alterações sejam incluídos em verificações futuras. Este passo é semelhante à forma como um motor de busca no ambiente de trabalho (se estiver ativada) ou um motor de busca web continuamente atualiza respetivo índice para incluir novos ficheiros ou páginas.

1.  No [referência da API de gestão do termo lista](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), no menu à esquerda, selecione **termo lista**e, em seguida, selecione **atualizar o índice de pesquisa**. 

  O **termo lista - atualizar o índice de pesquisa** é aberta a página.

2. Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização. 

  ![Apresenta uma lista de termo - seleção de região de página de índice de pesquisa de atualização](images/test-drive-region.png)

  O **termo lista - atualizar o índice de pesquisa** é aberta a consola de API.

3.  No **listId** box, introduza o ID da lista. Introduza a chave de subscrição e, em seguida, selecione **enviar**.

  ![Apresenta uma lista de termo API - a caixa de conteúdo de resposta do índice de pesquisa de atualização de consola](images/try-terms-list-refresh-1.png)

## <a name="create-a-term-list"></a>Criar uma lista de termo
1.  Vá para o [referência da API de gestão do termo lista](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f). 

  O **termo lista - criar** é aberta a página.

2.  Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização. 

  ![Termo listas - seleção de região de página de criação](images/test-drive-region.png)

  O **termo lista - criar** é aberta a consola de API.
 
3.  No **Ocp-Apim-Subscription-Key** box, introduza a chave de subscrição.

4.  No **corpo do pedido** box, introduza valores para **nome** (por exemplo, MyList) e **Descrição**.

  ![Termo listas - nome de corpo do pedido de consola e uma descrição de criação](images/try-terms-list-create-1.png)

5.  Utilize marcadores de posição de pares chave-valor para atribuir metadados descritivos mais à sua lista.

        {
           "Name": "MyExclusionList",
           "Description": "MyListDescription",
           "Metadata": 
           {
              "Category": "Competitors",
              "Type": "Exclude"
           }
        }

  Adicione metadados de lista como pares chave-valor e termos não reais.
 
6.  Selecione **Enviar**. É criada a sua lista. Tenha em atenção o **ID** valor que está associado a nova lista. Este ID é necessário para outras funções de gestão da lista de prazo.

  ![Termo listas - criar consola resposta conteúdo caixa apresenta o ID de lista](images/try-terms-list-create-2.png)
 
7.  Adicione termos MyList. No menu à esquerda, em **termo**, selecione **adicionar termo**. 

  O **termo - adicionar termo** é aberta a página. 

8.  Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização. 

  ![Termo – adicionar seleção de região de página de termo](images/test-drive-region.png)

  O **termo - adicionar termo** é aberta a consola de API.
 
9.  No **listId** caixa, introduza o ID de lista que gerou e selecione um valor para **idioma**. Introduza a chave de subscrição e, em seguida, selecione **enviar**.

  ![Termo – adicionar parâmetros de consulta da consola de termo](images/try-terms-list-create-3.png)
 
10. Para verificar se o termo foi adicionado à lista, no menu à esquerda, selecione **termo**e, em seguida, selecione **obter todos os termos**. 

  O **termo - obter todos os termos** é aberta a consola de API.

11. No **listId** caixa, introduza o ID de lista e, em seguida, introduza a chave de subscrição. Selecione **Enviar**.

12. No **conteúdo da resposta** caixa, verifique os termos de licenciamento que introduziu.

  ![Termo - Get todos os termos consola resposta conteúdo caixa apresenta uma lista de termos que introduziu](images/try-terms-list-create-4.png)
 
13. Adicione alguns termos mais. Agora que criou uma lista dos termos personalizada, experimente [análise algum texto](try-text-api.md) utilizando a lista de termo personalizado. 

## <a name="delete-terms-and-lists"></a>Eliminar os termos e listas

Eliminar um termo ou uma lista é simples. Utilize a API para efetuar as seguintes tarefas:

- Elimine um termo. (**Termo - eliminar**)
- Elimine todos os termos de licenciamento numa lista sem a eliminação da lista. (**Termo - eliminar todos os termos**)
- Elimine uma lista e todos os respetivos conteúdos. (**Termo listas - eliminar**)

Neste exemplo elimina um termo único.

1.  No [referência da API de gestão do termo lista](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), no menu à esquerda, selecione **termo**e, em seguida, selecione **eliminar**. 

  O **termo - eliminar** abre.

2. Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização. 

  ![Termo - seleção de região de página de eliminação](images/test-drive-region.png)

  O **termo - eliminar** é aberta a consola de API.
  
3.  No **listId** caixa, introduza o ID da lista a que pretende eliminar um termo do. Este ID é o número (no nosso exemplo, **122**) que é devolvido no **termo lista - detalhes do obter** MyList na consola. Introduza o termo e selecione um idioma.
 
  ![Termo - parâmetros de consulta da consola de eliminação](images/try-terms-list-delete-1.png)

4.  Introduza a chave de subscrição e, em seguida, selecione **enviar**.

5.  Para verificar que o termo foi eliminado, utilize o **termo lista - obter todos os** consola.

  ![Termo listas - obter resposta de consola todos os conteúdos caixa mostra que o termo é eliminado](images/try-terms-list-delete-2.png)
 
## <a name="change-list-information"></a>Informações de alteração de lista

Pode editar o nome e descrição de uma lista e adicionar os itens de metadados.

1.  No [referência da API de gestão do termo lista](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), no menu à esquerda, selecione **termo lista**e, em seguida, selecione **detalhes da atualização**. 

  O **termo lista - detalhes da atualização** é aberta a página.

2. Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização. 

  ![Apresenta uma lista de termo - seleção de região de página de detalhes da atualização](images/test-drive-region.png)

  O **termo lista - detalhes da atualização** é aberta a consola de API.

3.  No **listId** caixa, introduza o ID de lista e, em seguida, introduza a chave de subscrição.

4.  No **corpo do pedido** caixa, faça as suas edições e, em seguida, selecione **enviar**.

  ![Apresenta uma lista de termo - detalhes da atualização da consola edições de corpo do pedido](images/try-terms-list-change-1.png)
 

## <a name="next-steps"></a>Passos Seguintes

Utilizar a API REST no seu código ou começar com o [termo apresenta uma lista de início rápido do .NET](term-lists-quickstart-dotnet.md) para integrar com a sua aplicação.
