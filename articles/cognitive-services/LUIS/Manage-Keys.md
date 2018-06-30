---
title: Gerir as chaves de LUIS | Microsoft Docs
description: Utilize compreensão de idiomas (LUIS) para gerir a sua API programática, ponto final e chaves externas.
titleSuffix: Azure
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/21/2018
ms.author: v-geberr
ms.openlocfilehash: 1fdf0f21697e48fd5cb9eca4212596059a9519cb
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127122"
---
# <a name="manage-your-luis-keys"></a>Gerir as chaves de LUIS
Uma chave permite-lhe criar e publicar a aplicação de LUIS ou consultar o ponto final. 

<a name="programmatic-key" ></a>
<a name="authoring-key" ></a>
<a name="endpoint-key" ></a>
<a name="use-endpoint-key-in-query" ></a>
<a name="api-usage-of-ocp-apim-subscription-key" ></a>
<a name="key-limits" ></a>
<a name="key-limit-errors" ></a>
## <a name="key-concepts"></a>Conceitos-chave
Consulte [chaves na LUIS](luis-concept-keys.md) compreender conceitos chave LUIS criação e o ponto final.

<a name="create-and-use-an-endpoint-key"></a>
## <a name="assign-endpoint-key"></a>Atribuir a chave de ponto final
No **publicar aplicação** página, se já houver uma chave a **recursos e as chaves** tabela. Esta é a chave de criação (arranque). 

1. Criar uma chave de LUIS no [portal do Azure](https://portal.azure.com). Para obter instruções, consulte [criar uma chave de ponto final a utilizar o Azure](luis-how-to-azure-subscription.md).
 
2. Para poder adicionar a chave de LUIS criada no passo anterior, clique em de **Adicionar chave** botão para abrir o **atribuir uma chave para a sua aplicação** caixa de diálogo. 

    ![Atribuir uma chave para a sua aplicação](./media/luis-manage-keys/assign-key.png)
3. Selecione um inquilino na caixa de diálogo. 
 
    > [!Note]
    > No Azure, um inquilino representa o ID do Active Directory do Azure da organização associada um serviço ou cliente. Se tiver anteriormente sessão para uma subscrição do Azure com o seu Account individual da Microsoft, já possui um inquilino! Quando inicia sessão portal do Azure, é automaticamente registados [o inquilino predefinido](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant). Tem de utilizar este inquilino, mas pode querer criar uma conta de administrador organizacional.

4. Selecione a subscrição do Azure associada à chave do Azure LUIS que pretende adicionar.

5. Selecione a conta do Azure LUIS. A região da conta é apresentada no parênteses. 

    ![Escolha a chave](./media/luis-manage-keys/assign-key-filled-out.png)

6. Depois de atribuir esta chave de ponto final, utilizá-lo em todas as consultas de ponto final. 

<!-- content moved to luis-reference-regions.md, need replacement links-->
<a name="regions-and-keys"></a>
<a name="publishing-to-europe"></a>
<a name="publishing-to-australia"></a>

## <a name="publishing-regions"></a>Publicação de regiões
Saiba mais sobre a publicação [regiões](luis-reference-regions.md) , incluindo a publicação no [Europa](luis-reference-regions.md#publishing-to-europe), e [Austrália](luis-reference-regions.md#publishing-to-australia). Publicação de regiões forem diferentes das regiões de criação. Certifique-se que criar uma aplicação na região criação correspondente à região de publicação que pretende.

## <a name="unassign-key"></a>Anule a atribuição chave

* No **lista de recursos e as chaves**, clique no ícone de reciclagem de lixo junto a entidade que pretende anule atribuição. Em seguida, clique em **OK** na mensagem de confirmação para confirmar a eliminação.
 
    ![Anule a atribuição entidade](./media/luis-manage-keys/unassign-key.png)

> [!NOTE]
> Unassigning a chave de LUIS não elimine-o da sua subscrição do Azure.

## <a name="next-steps"></a>Passos Seguintes

Utilizar a sua chave para publicar a aplicação no **publicar aplicação** página. Para obter instruções sobre a publicação, consulte [publicar aplicação](PublishApp.md).

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website