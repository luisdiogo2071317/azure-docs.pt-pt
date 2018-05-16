---
title: Publicar versões da sua API com a Gestão de API do Azure | Microsoft Docs
description: Siga os passos deste tutorial para aprender a publicar várias versões na Gestão de API.
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
ms.openlocfilehash: 1cbe63184578f7d1e72992577a11c58b9b83a002
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="publish-multiple-versions-of-your-api"></a>Publicar várias versões da sua API 

Quando não for prático ter todos os autores de chamadas direcionados para a sua API, utilize exatamente a mesma versão. Quando os autores de chamadas quiserem atualizar para uma versão posterior, querem fazê-lo com uma abordagem de fácil compreensão. É possível fazê-lo com **versões** na Gestão de API do Azure. Para obter mais informações, veja [Versões e revisões](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/).

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Adicionar uma nova versão a uma API existente
> * Escolher um esquema de versão
> * Adicionar a versão a um produto
> * Navegar até ao portal do programador para ver a versão

![A versão é apresentada no portal do programador](media/api-management-getstarted-publish-versions/azure_portal.PNG)

## <a name="prerequisites"></a>Pré-requisitos

* Conclua o guia de início rápido seguinte: [Criar uma instância da Gestão de API do Azure](get-started-create-service-instance.md).
* Conclua também o tutorial seguinte: [Importar e publicar a sua primeira API](import-and-publish.md).

## <a name="add-a-new-version"></a>Adicionar uma nova versão

![Menu de contexto da API - adicionar versão](media/api-management-getstarted-publish-versions/AddVersionMenu.png)

1. Selecione **Conference API** na lista de APIs.
2. Selecione o menu de contexto (**...**) junto à mesma.
3. Selecione **+ Adicionar Versão**.

    > [!TIP]
    > As versões também podem ser ativadas quando criar uma nova API. Selecione **Controlar a versão desta API?** no ecrã **Adicionar API**.

## <a name="choose-a-versioning-scheme"></a>Escolher um esquema de versões

A Gestão de API do Azure permite-lhe escolher a forma como permite aos autores de chamadas especificar a versão da API que querem. Especifica a versão da API a utilizar ao selecionar um **esquema de versões**. Este esquema pode ser um **caminho, um cabeçalho ou uma cadeira de consulta**. No exemplo seguinte, é utilizado o caminho para selecionar o esquema de versões.

![Adicionar ecrã de versão](media/api-management-getstarted-publish-versions/AddVersion.PNG)

1. Mantenha **caminho** selecionado como **esquema de versões**.
2. Adicione **v1** como **identificador da versão**.

    > [!TIP]
    > Se selecionar **cabeçalho** ou **cadeia de consulta** como esquema de versões, tem de fornecer um valor adicional, o nome do cabeçalho ou o parâmetro da cadeia de consulta.

3. Forneça uma descrição, se desejar.
4. Selecione **Criar** para configurar a nova versão.
5. Abaixo de **Big Conference API** na lista de APIs, pode ver agora duas APIs distintas: **Original** e **v1**.

    ![Versões listadas numa API no portal do Azure](media/api-management-getstarted-publish-versions/VersionList.PNG)

    > [!Note]
    > Se adicionar uma versão a uma API sem versões, será criada automaticamente uma **Original**, que responde no URL predefinido. Isto garante que todos os autores de chamadas existentes não são divididos pelo processo de adição de uma versão. Se criar uma nova API com versões ativadas no início, não é criada uma Original.

6. Agora, pode editar e configurar **v1** como uma API separada da **Original**. As alterações a uma versão não afetam a outra.

## <a name="add-the-version-to-a-product"></a>Adicionar a versão a um produto

Para os autores de chamadas verem a nova versão, esta tem de ser adicionada a um **produto**.

1. Selecione **Produtos** na página do modelo de implementação clássica.
2. Selecione **Ilimitados**.
3. Selecione **APIs**.
4. Selecione **Adicionar**.
5. Selecione **Conference API, Versão v1**.
6. Navegue até à página de gestão de serviço e selecione **APIs**.

## <a name="browse-the-developer-portal-to-see-the-version"></a>Navegar até ao portal do programador para ver a versão

1. Selecione **Portal do Programador** no menu superior.
2. Selecione **APIs**, repare que **Conference API** mostra as versões **Original** e **v1**.
3. Selecione **v1**.
4. Repare no **URL do pedido** da primeira operação na lista. Indica que o caminho do URL da API inclui **v1**.

    ![Menu de contexto da API - adicionar versão](media/api-management-getstarted-publish-versions/developer_portal.png)

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Adicionar uma nova versão a uma API existente
> * Escolher um esquema de versão 
> * Adicionar a versão a um produto
> * Navegar até ao portal do programador para ver a versão

Avance para o tutorial seguinte:

> [!div class="nextstepaction"]
> [Atualizar e dimensionar](upgrade-and-scale.md)