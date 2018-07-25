---
title: Adicionar recursos em aplicativos de LUIS | Documentos da Microsoft
description: Utilizar a compreensão de idiomas (LUIS) para adicionar funcionalidades de aplicação que podem melhorar a deteção ou a predição de objetivos e entidades que categorias e padrões
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/30/2018
ms.author: diberry
ms.openlocfilehash: 5ec75436c7df5c08f5507794229bec1f9adb2804
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39222958"
---
# <a name="use-features-to-improve-your-luis-apps-performance"></a>Utilizar as funcionalidades para melhorar o desempenho da sua aplicação LUIS  

Pode adicionar funcionalidades à sua aplicação LUIS para melhorar a precisão. Recursos ajudam o LUIS, fornecendo as sugestões que determinadas palavras e frases fazem parte de uma categoria. Se LUIS aprende a reconhecer um membro da categoria, ele pode tratar outros da mesma forma.

## <a name="add-phrase-list"></a>Adicionar a lista de frase

1. Abra a sua aplicação ao clicar em seu nome no **as minhas aplicações** página e, em seguida, clique em **crie**, em seguida, clique em **frase listas** no painel esquerdo da sua aplicação. 

    ![Navegação de lista de frase](./media/luis-add-features/phrase-list-nav.png)

2. Sobre o **frase listas** página, clique em **criar nova lista de frase**. 
 
    ![Criar nova lista de frase](./media/luis-add-features/create-new-phrase-list.png)
    
3. Na **lista de frase adicionar** caixa de diálogo, escreva "Cidades" como o nome da lista frase. Na **valor** , escreva os valores da lista frase. Escreva um valor numa hora ou um conjunto de valores separados por vírgulas e, em seguida, prima **Enter**.

    ![Adicionar frase lista cidades](./media/luis-add-features/add-phrase-list-cities.png)

4. LUIS pode propor valores relacionados para adicionar à sua lista de frase. Clique em **Recomendamos** para obter um grupo de valores propostos semanticamente relacionados com o added value(s). Pode clicar em qualquer um dos valores propostos ou clique em **adicionar todos os** para adicioná-los todos.

    ![Lista de frase proposto valores](./media/luis-add-features/related-values.png)

5. Clique em **estes valores são intercambiáveis** se os valores de lista de frase adicionados são alternativas que podem ser utilizadas alternadamente.

    ![Lista de frase proposto valores](./media/luis-add-features/interchangeable.png)

6. Clique em **Guardar**. A lista de frase "Cidades" é adicionada para o **frase listas** página.

    ![Lista de frase adicionada](./media/luis-add-features/phrase-list-cities.png)

## <a name="edit-phrase-list"></a>Editar lista de frase

Clique no nome da lista frase no **frase listas** página. Na **Editar lista de frase** caixa de diálogo que é aberta, efetuar qualquer necessário editar as alterações e clique em **guardar**.

 ![Lista de frase adicionada](./media/luis-add-features/edit-phrase-list.png)

## <a name="delete-phrase-list"></a>Eliminar expressão lista 

Clique nas reticências (***...*** ) no final da linha e selecione **eliminar**.

 ![Eliminar lista adicionada](./media/luis-add-features/delete-phrase-list.png)

## <a name="deactivate-phrase-list"></a>Desativar a lista de frase 

Clique nas reticências (***...*** ) no final da linha e selecione **desativar**.

 ![Desativar lista adicionada](./media/luis-add-features/deactivate-phrase-list.png)

## <a name="pattern-regular-expression-feature"></a>Funcionalidade do padrão (expressão regular) 
**Esta funcionalidade foi preterida**. Não não possível adicionar novos recursos de padrão LUIS. Todos os recursos padrão existentes são suportados até Maio de 2018. Contribuir para a expressão regular padrão do LUIS, correspondência com um pedido Pull para o [repositório do Github de texto reconhecedores](https://github.com/Microsoft/Recognizers-Text). 

## <a name="next-steps"></a>Passos Seguintes

Após adicionar, editar, eliminar ou desativar uma lista de frase [treinar e testar a aplicação](luis-interactive-test.md) novamente para ver se melhora o desempenho.
