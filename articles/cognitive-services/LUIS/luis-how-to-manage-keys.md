---
title: Gerir as suas chaves em LUIS | Documentos da Microsoft
description: Utilize a compreensão de idiomas (LUIS) para gerir a sua API programática, o ponto final e a chaves externas.
titleSuffix: Azure
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/21/2018
ms.author: v-geberr
ms.openlocfilehash: 80064584468c109d0bad49f1a22c485fa9cce846
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344523"
---
# <a name="manage-your-luis-keys"></a>Gerir as chaves de LUIS
Uma chave permite-lhe criar e publicar a sua aplicação do LUIS ou consultar o ponto final. 

<a name="programmatic-key" ></a>
<a name="authoring-key" ></a>
<a name="endpoint-key" ></a>
<a name="use-endpoint-key-in-query" ></a>
<a name="api-usage-of-ocp-apim-subscription-key" ></a>
<a name="key-limits" ></a>
<a name="key-limit-errors" ></a>
## <a name="key-concepts"></a>Conceitos-chave
Ver [chaves de LUIS](luis-concept-keys.md) para compreender conceitos-chave de criação e o ponto final LUIS.

<a name="create-and-use-an-endpoint-key"></a>
## <a name="assign-endpoint-key"></a>Atribuir a chave de ponto final
Sobre o **publicar aplicação** página, já existe uma chave no **recursos e as chaves** tabela. Esta é a chave de criação (iniciante). 

1. Criar uma chave de LUIS sobre o [portal do Azure](https://portal.azure.com). Para obter mais instruções, consulte [criar uma chave de ponto final a utilizar o Azure](luis-how-to-azure-subscription.md).
 
2. Para adicionar a chave de LUIS criada no passo anterior, clique nas **Adicionar chave** botão para abrir o **atribuir uma chave à sua aplicação** caixa de diálogo. 

    ![Atribuir uma chave à sua aplicação](./media/luis-manage-keys/assign-key.png)
3. Na caixa de diálogo, selecione um inquilino. 
 
    > [!Note]
    > No Azure, um inquilino representa a ID do Azure Active Directory do cliente ou da organização associada um serviço. Se já tiver uma subscrição do Azure com a sua Account individual da Microsoft, já tem um inquilino! Quando iniciar sessão portal do Azure, é automaticamente sessão iniciada no [inquilino predefinido](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant). Tem liberdade para utilizar este inquilino, mas pode querer criar uma conta de administrador organizacional.

4. Escolha a subscrição do Azure associada à chave do Azure LUIS que pretende adicionar.

5. Selecione a conta do Azure LUIS. A região da conta é apresentada em parênteses. 

    ![Escolha a chave](./media/luis-manage-keys/assign-key-filled-out.png)

6. Depois de atribuir esta chave de ponto de extremidade, utilizá-lo em todas as consultas de ponto final. 

<!-- content moved to luis-reference-regions.md, need replacement links-->
<a name="regions-and-keys"></a>
<a name="publishing-to-europe"></a>
<a name="publishing-to-australia"></a>

## <a name="publishing-regions"></a>Regiões de publicação
Saiba mais sobre a publicação [regiões](luis-reference-regions.md) incluindo publicação na [Europa](luis-reference-regions.md#publishing-to-europe), e [Austrália](luis-reference-regions.md#publishing-to-australia). Regiões de publicação são diferentes das regiões de criação. Certifique-se de que criar uma aplicação na região criação correspondente para a região de publicação que pretende.

## <a name="unassign-key"></a>Anular a atribuição de chave

* Na **lista de recursos e as chaves**, clique no ícone de reciclagem de caixote do lixo junto a entidade que pretende anular a atribuição. Em seguida, clique em **OK** na mensagem de confirmação para confirmar a eliminação.
 
    ![Anular a atribuição de entidade](./media/luis-manage-keys/unassign-key.png)

> [!NOTE]
> Unassigning a chave de LUIS não o elimina da sua subscrição do Azure.

## <a name="next-steps"></a>Passos Seguintes

Utilize a chave para publicar a sua aplicação no **publicar aplicação** página. Para obter instruções sobre a publicação, consulte [publicar aplicação](luis-how-to-publish-app.md).

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website