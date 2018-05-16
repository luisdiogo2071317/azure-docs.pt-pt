---
title: Personalizar o estilo de página no portal de programador da Gestão de API do Azure | Microsoft Docs
description: Siga os passos neste início rápido para personalizar o estilo dos elementos no portal de programador da Gestão de API do Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: d1f638c9825ea5eedf6eaee0e0ca2ccfd5a491bc
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="customize-the-style-of-the-developer-portal-pages"></a>Personalizar o estilo das páginas de portal do Programador

Existem três formas mais comuns de personalizar o portal do Programador na Gestão de API do Azure:
 
* [Editar o conteúdo de páginas estáticas e elementos de esquema da página](api-management-modify-content-layout.md)
* Atualizar os estilos utilizados para elementos de página em todo o portal de programador (explicado neste guia)
* [Modificar os modelos utilizados para páginas geradas pelo portal](api-management-developer-portal-templates.md)(por exemplo, documentos de API, produtos, autenticação de utilizador)

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Personalizar o estilo dos elementos em páginas do portal do **Programador**
> * Ver a sua alteração

![personalizar estilo](./media/modify-developer-portal-style/developer_portal.png)

## <a name="prerequisites"></a>Pré-requisitos

+ Conclua o guia de início rápido seguinte: [Criar uma instância da Gestão de API do Azure](get-started-create-service-instance.md).
+ Conclua também o tutorial seguinte: [Importar e publicar a sua primeira API](import-and-publish.md).

## <a name="customize-the-developer-portal"></a>Personalizar o Portal do programador

1. Selecione **Descrição geral**.
2. Clique no botão **Portal do programador** na parte superior da janela **Descrição geral**. Em alternativa, pode clicar na ligação **URL do portal do programador**.
3. No canto superior esquerdo do ecrã, verá um ícone com dois pincéis. Coloque o cursor sobre este ícone para abrir o menu de personalização do portal.

    ![personalizar estilo](./media/modify-developer-portal-style/modify-developer-portal-style01.png)
4. Selecione **Estilos** no menu para abrir o painel de personalização de estilos.

    Todos os elementos que pode personalizar com **Estilos** aparecem na página
5. Introduza "headings-color" no campo **Alterar valores de variáveis para personalizar o aspeto do portal do programador:**.

    O elemento **@headings-color** é apresentado na página. Esta variável controla a cor do texto.

    ![personalizar estilo](./media/modify-developer-portal-style/modify-developer-portal-style02.png)
    
6. Clique no campo para a variável **@headings-color**. 
    
    É aberto o menu pendente do selecionador de cores.
7. No menu pendente do selecionador de cores, selecione uma nova cor.

    > [!TIP]
    > A pré-visualização em tempo real está disponível para todas as alterações. Aparece um indicador de progresso na parte superior do painel de personalização. Após alguns segundos, o texto do cabeçalho muda de cor para a nova selecionada.

8. Selecione **Publicar** na parte superior esquerda do menu do painel de personalização.
9. Selecione **Publicar personalizações** para tornar as alterações publicamente disponíveis.

## <a name="view-your-change"></a>Ver a sua alteração

1. Navegue para o portal do Programador.
2. Pode ver a alteração que efetuou.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Personalizar o estilo dos elementos em páginas do portal do **Programador**
> * Ver a sua alteração

> [!div class="nextstepaction"]
> [Personalizar o portal do programador da Gestão de API do Azure através de modelos](api-management-developer-portal-templates.md)