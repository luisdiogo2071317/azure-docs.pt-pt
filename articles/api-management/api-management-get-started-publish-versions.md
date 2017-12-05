---
title: "Publicar versões da sua API utilizando a API Management do Azure | Microsoft Docs"
description: "Siga os passos deste tutorial para saber como publicar as várias versões na API Management."
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
ms.openlocfilehash: d63bdd3110f5c5db3e7bfec424644fdbc8d8d90c
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="publish-multiple-versions-of-your-api"></a>Publicar várias versões da sua API 

Existem vezes quando é impractical os todos os chamadores à sua utilização de API de ter exatamente a mesma versão. Por vezes, que pretende publicar novas ou diferentes funcionalidades de API a alguns utilizadores, enquanto outros pretendem stick com a API atualmente funciona para os mesmos. Quando os chamadores pretendem atualizar para uma versão posterior, pretende efetuar esta utilizando uma fácil de compreender a abordagem.  Podemos fazer isto utilizando **versões** na API Management do Azure. Para obter mais informações, consulte [versões & revisões](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/).

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Adicionar uma nova versão a uma API existente
> * Escolha um esquema de versão
> * Adicionar a versão a um produto
> * Navegue até ao portal de programador para ver a versão

![Versão apresentada no portal do Programador](media/api-management-getstarted-publish-versions/azure_portal.PNG)

## <a name="prerequisites"></a>Pré-requisitos

+ Concluir o guia de introdução seguinte: [criar uma instância de API Management do Azure](get-started-create-service-instance.md).
+ Além disso, concluir o tutorial seguinte: [importação e publicar a sua primeira API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="add-a-new-version"></a>Adicionar uma nova versão

![Menu de contexto de API - adicionar versão](media/api-management-getstarted-publish-versions/AddVersionMenu.png)

1. Selecione **conferência API** na lista de API.
2. Selecione o menu de contexto (**...** ) junto ao mesmo.
3. Selecione **+ adicionar versão**.

    > [!TIP]
    > Também podem ser ativadas a versões ao primeiro criar uma nova API - selecione **versão esta API?** no **adicionar API** ecrã.

## <a name="choose-a-versioning-scheme"></a>Escolha um esquema de controlo de versões

Gestão de API do Azure permite-lhe escolher a forma nos quais permite os chamadores especificar que versão de API pretendem. Fazê-lo ao escolher um **esquema de controlo de versões**. Este esquema pode ser **cadeia de caminho, consulta ou cabeçalho**. No nosso exemplo, utilizamos o caminho.

![Adicionar o ecrã de versão](media/api-management-getstarted-publish-versions/AddVersion.PNG)

1. Deixe **caminho** selecionado como o **esquema de controlo de versões**.
2. Adicionar **v1** como seu **identificador da versão**.

    > [!TIP]
    > Se selecionar **cabeçalho** ou **cadeia de consulta** como um esquema de controlo de versões, terá de fornecer um valor adicional - o nome do cabeçalho ou parâmetro de cadeia de consulta.

3. Forneça uma descrição se desejar.
4. Selecione **criar** para configurar a nova versão.
5. Underneath **grande conferência API** na lista de API, agora, ver APIs distintas dois - **Original**, e **v1**.

    ![Versões listadas sob uma API no portal do Azure](media/api-management-getstarted-publish-versions/VersionList.PNG)

    > [!Note]
    > Se adicionar uma versão de uma API não com a versão, criamos sempre um **Original** para si - a responder no URL predefinido. Isto garante que qualquer os chamadores existentes não são divididos pelo processo de adição de uma versão. Se criar uma nova API com versões ativadas no início, não é criado um Original.

6. Agora pode editar e configurar **v1** como uma API que é separada para **Original**. As alterações a uma versão não afetam a outra.

## <a name="add-the-version-to-a-product"></a>Adicionar a versão a um produto

Para os chamadores ver a nova versão, tem de ser adicionado a um **produto** (produtos não são herdados de versões de principal).

1. Selecione **produtos** da página de gestão de serviço.
2. Selecione **ilimitados**.
3. Selecione **APIs**.
4. Selecione **Adicionar**.
5. Selecione **conferência API, versão v1**.
6. Regressar à página de gestão de serviço e selecionar **APIs**.

## <a name="browse-the-developer-portal-to-see-the-version"></a>Navegue até ao portal de programador para ver a versão

1. Selecione **Portal do programador** no menu superior.
2. Selecione **APIs**, repare que **conferência API** mostra **Original** e **v1** versões.
3. Selecione **v1**.
4. Tenha em atenção o **URL pedido** da operação primeiro na lista. Indica que o caminho de URL de API inclui **v1**.

    ![Menu de contexto de API - adicionar versão](media/api-management-getstarted-publish-versions/developer_portal.png)

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Adicionar uma nova versão a uma API existente
> * Escolha um esquema de versão 
> * Adicionar a versão a um produto
> * Navegue até ao portal de programador para ver a versão

Avançar para o próximo tutorial:

> [!div class="nextstepaction"]
> [Atualizar e dimensionar](upgrade-and-scale.md)