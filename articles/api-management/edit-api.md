---
title: Editar uma API com o portal do Azure | Microsoft Docs
description: "Este tutorial mostra como utilizar a gestão de API (APIM) para editar uma API."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/08/2017
ms.author: apimpm
ms.openlocfilehash: 362c36181da706e3fe0a27cc5ab262271c2a1e57
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="edit-an-api"></a>Editar uma API

Os passos neste tutorial mostram-lhe como utilizar a gestão de API (APIM) para editar uma API. 

+ Pode fazê-lo através da adição, eliminação, mudar o nome de operações na instância APIM. 
+ Pode editar swagger da sua API.

## <a name="prerequisites"></a>Pré-requisitos

+ [Criar uma instância de API Management do Azure](get-started-create-service-instance.md)
+ [Importar e publicar a sua primeira API](import-and-publish.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="edit-an-api-in-apim"></a>Editar uma API na APIM

![Editar uma api](./media/edit-api/edit-api001.png)

1. Clique em de **APIs** separador.
2. Selecione uma das APIs que importou anteriormente.
3. Selecione o **Design** separador.
4. Selecione uma operação que pretende editar.
5. Para mudar o nome da operação, selecione um **lápis** no **front-end** janela.

## <a name="update-the-swagger"></a>Atualização do swagger

Pode atualizar o seu backbend API do portal do Azure, seguindo estes passos:

1. Selecione **todas as operações**
2. Clique em lápis no **front-end** janela.

    ![Editar uma api](./media/edit-api/edit-api002.png)

    É apresentada a swagger da sua API.

    ![Editar uma api](./media/edit-api/edit-api003.png)

3. Atualize o swagger.
4. Prima **guardar**.

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Exemplos de política APIM](policy-samples.md)
> [transformar e proteger uma API publicada](transform-api.md)