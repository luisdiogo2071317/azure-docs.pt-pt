---
title: "Personalizar o estilo de página no portal de Programador de API Management do Azure | Microsoft Docs"
description: "Siga os passos neste guia de introdução para personalizar o estilo dos elementos no portal de Programador de API Management do Azure."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: f427663ba1c437785c8c521925d9f733c45cb40d
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="customize-the-style-of-the-developer-portal-pages"></a>Personalizar o estilo das páginas de portais para programadores

Existem três formas mais comuns para personalizar o portal do programador na API Management do Azure:
 
* [Editar o conteúdo de elementos da página de esquema e páginas estáticas](api-management-modify-content-layout.md)
* Atualizar os estilos utilizados para elementos da página entre o portal do programador (explicado neste guia)
* [Modificar os modelos utilizados para páginas geradas pelo portal](api-management-developer-portal-templates.md) (por exemplo, documentos de API, produtos, a autenticação de utilizador)

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Personalizar o estilo de elementos nas páginas do **programador** portal
> * Ver a alteração

![Personalizar o estilo](./media/modify-developer-portal-style/developer_portal.png)

## <a name="prerequisites"></a>Pré-requisitos

+ Concluir o guia de introdução seguinte: [criar uma instância de API Management do Azure](get-started-create-service-instance.md).
+ Além disso, concluir o tutorial seguinte: [importação e publicar a sua primeira API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="customize-the-developer-portal"></a>Personalizar o portal do Programador

1. Selecione **descrição geral**.
2. Clique em de **portal do programador** botão do **descrição geral** janela. Em alternativa, pode clicar no **portal do programador URL** ligação.
3. No lado superior esquerdo do ecrã, verá um ícone composto por dois brushes de paint. Coloque o cursor sobre este ícone para abrir o menu de personalização do portal.

    ![Personalizar o estilo](./media/modify-developer-portal-style/modify-developer-portal-style01.png)
4. Selecione **estilos** no menu para abrir o painel de personalização de estilo.

    Todos os elementos que pode personalizar utilizando **estilos** aparecer na página
5. Introduza "headings-color" o **alterar valores das variáveis para personalizar o aspeto de portal de programador:** campo.

    O  **@headings-color**  elemento é apresentado na página. Esta variável controla a cor do texto.

    ![Personalizar o estilo](./media/modify-developer-portal-style/modify-developer-portal-style02.png)
    
6. Clique no campo para a  **@headings-color**  variável. 
    
    É aberto o menu pendente do Seletor de cores.
7. Da cor pickers pendente selecione a cor de novo.

    > [!TIP]
    > Pré-visualização em tempo real está disponível para todas as alterações. É apresentado um indicador de progresso na parte superior do painel de personalização. Após alguns segundos o texto do cabeçalho alterações na cor para recentemente selecionado.

8. Selecione **publicar** da parte inferior esquerda no menu do painel de personalização.
9. Selecione **publicar personalizações** que efetue as alterações publicamente disponíveis.

## <a name="view-your-change"></a>Ver a alteração

1. Navegue para o portal do programador.
2. Pode ver a alteração que efetuou.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Personalizar o estilo de elementos nas páginas do **programador** portal
> * Ver a alteração

> [!div class="nextstepaction"]
> [Personalizar o portal do Programador de API Management do Azure através de modelos](api-management-developer-portal-templates.md)