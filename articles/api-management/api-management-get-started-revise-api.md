---
title: "Utilize as revisões para efetuar alterações de sem quebra em segurança na API Management do Azure | Microsoft Docs"
description: "Siga os passos deste tutorial para saber como tornar a alterações sem quebra utilizando revisões na API Management."
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
ms.openlocfilehash: 50d7ac17faebb34f1a1f9a3259aa0196950391d9
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="use-revisions-to-make-non-breaking-changes-safely"></a>Utilize revisões para efetuar alterações de sem quebra de forma segura
Quando a API está pronta para começar e começa a ser utilizado pelos programadores, normalmente terá asseguramos na realização de alterações para essa API e ao mesmo tempo para não prejudicam os chamadores da sua API. Também é útil permitir que os programadores de saber sobre as alterações que efetuou. Podemos fazer isto na API Management do Azure utilizando **revisões**. Para obter mais informações, consulte [versões & revisões](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/) e [controlo de versões de API com a API Management do Azure](https://blogs.msdn.microsoft.com/apimanagement/2017/09/13/api-versioning-with-azure-api-management/).

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Adicionar uma nova revisão
> * Altera sem quebra a revisão
> * Efetue a revisão atual e adicionar uma entrada de registo de alteração
> * Procurar o portal do programador para ver as alterações e alterar o registo

![Registo de alterações no Portal de programador](media/api-management-getstarted-revise-api/azure_portal.PNG)

## <a name="prerequisites"></a>Pré-requisitos

+ Concluir o guia de introdução seguinte: [criar uma instância de API Management do Azure](get-started-create-service-instance.md).
+ Além disso, concluir o tutorial seguinte: [importação e publicar a sua primeira API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="add-a-new-revision"></a>Adicionar uma nova revisão

1. Selecione **APIs** página.
2. Selecione **conferência API** da lista de API (ou outro API para o qual pretende adicionar revisões).
3. Clique em de **revisões** separador no menu de perto da parte superior da página.
4. Selecione **+ adicionar revisão**

    > [!TIP]
    > Também pode optar por **adicionar revisão** no menu de contexto (**...** ) da API.
    
    ![Menu de revisões perto da parte superior do ecrã](media/api-management-getstarted-revise-api/TopMenu.PNG)

5. Indique uma descrição para a nova revisão, para ajudar a lembrar-se de que será utilizado para.
6. Selecione **criar**
7. Agora é criada a sua nova revisão.

    > [!NOTE]
    > A API original permanece no **revisão 1**. Esta é a revisão que os utilizadores continuam a chamar, até que optar por fazer uma revisão diferentes atual.

## <a name="make-non-breaking-changes-to-your-revision"></a>Altera sem quebra a revisão

1. Selecione **conferência API** na lista de API.
2. Selecione o **Design** separador perto da parte superior do ecrã.
3. Tenha em atenção que o **Seletor de revisão** (diretamente acima separador estrutura) mostra a revisão atual como **revisão 2**.

    > [!TIP]
    > Utilize o Seletor de revisão para alternar entre revisões de que pretende trabalhar.

4. Selecione **+ adicionar operação**.
5. Definir a operação de novo a ser **POST**e o nome & o nome a apresentar da operação como **testar**
6. **Guardar** a operação de novo.
7. Iremos agora efetuou uma alteração ao **revisão 2**. Utilize o **Seletor de revisão** perto da parte superior da página para voltar ao painel **revisão 1**.
8. Tenha em atenção que a operação de nova não aparece na **revisão 1**. 

## <a name="make-your-revision-current-and-add-a-change-log-entry"></a>Efetue a revisão atual e adicionar uma entrada de registo de alteração
1. Selecione o **revisões** separador no menu de perto da parte superior da página.

    ![O menu de revisão no ecrã de revisão.](media/api-management-getstarted-revise-api/RevisionsMenu.PNG)
1. Abra o menu de contexto (**...** ) para **revisão 2**.
2. Selecione **tornar atual**. Verifique **publique registo alterar públicos para esta API**, se pretender publicar notas sobre esta alteração.
3. Selecione **Post para o registo de alterações públicos para esta API**
4. Indique uma descrição para a sua alteração que os programadores vê, por exemplo **revisões de teste. Foram adicionadas novas "teste" a operação.**
5. **Revisão 2** está agora atual.

## <a name="browse-the-developer-portal-to-see-changes-and-change-log"></a>Procurar o portal do programador para ver as alterações e alterar o registo
1. No portal do Azure, selecione **APIs**
2. Selecione **Portal do programador** no menu superior.
3. Selecione **APIs**e, em seguida, selecione **conferência API**.
4. Tenha em atenção a nova **testar** está agora disponível a operação.
5. Selecione **histórico de alterações de API** partir abaixo o nome da API.
6. Tenha em atenção que a entrada de registo de alterações for apresentada nesta lista.

    ![Portal do programador](media/api-management-getstarted-revise-api/developer_portal.PNG)

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Adicionar uma nova revisão
> * Altera sem quebra a revisão
> * Efetue a revisão atual e adicionar uma entrada de registo de alteração
> * Procurar o portal do programador para ver as alterações e alterar o registo

Avançar para o próximo tutorial:

> [!div class="nextstepaction"]
> [Publicar várias versões da sua API](api-management-get-started-publish-versions.md)