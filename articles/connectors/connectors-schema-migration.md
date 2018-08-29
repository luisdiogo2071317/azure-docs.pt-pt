---
title: Migrar aplicações para o esquema mais recente - Azure Logic Apps | Documentos da Microsoft
description: Como migrar as logic apps para a versão mais recente do esquema
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.assetid: 3e177e49-fd69-43e9-9b9b-218abb250c31
ms.topic: article
ms.date: 08/25/2018
ms.openlocfilehash: 8a6925d79b225a34d980472d4fb3241ab9eb1017
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43127416"
---
# <a name="migrate-logic-apps-to-latest-schema-version"></a>Migrar logic apps para a versão mais recente do esquema

Para mover as logic apps existentes para o esquema mais recente, siga estes passos: 

1. Na [portal do Azure](https://portal.azure.com), abra a aplicação lógica no Estruturador da aplicação lógica.

2. No menu da sua aplicação lógica, escolha **descrição geral**. Na barra de ferramentas, escolha **atualizar esquema**.

   > [!NOTE]
   > Quando selecionar **atualizar esquema**, Azure Logic Apps automaticamente executa os passos de migração e fornece o código de saída para. Pode utilizar esta saída para atualizar a definição da aplicação lógica. No entanto, certifique-se de seguir as melhores práticas, tal como descrito a seguir **melhores práticas** secção.

   ![Atualizar Esquema](./media/connectors-schema-migration/update-schema.png)

   A página atualizar esquema aparece e apresenta uma ligação para um documento que descreve os aperfeiçoamentos no novo esquema.

## <a name="best-practices"></a>Melhores práticas

Aqui estão algumas das melhores práticas para migrar as logic apps para a versão mais recente do esquema:

* Copie o script migrado para uma nova aplicação lógica. Não substitua a versão antiga até concluir o teste e confirme que a sua aplicação migrada funciona conforme esperado.

* Teste a aplicação lógica **antes de** colocar em produção.

* Depois de concluir a migração, comece a atualizar as logic apps para utilizar o [APIs geridas](../connectors/apis-list.md) sempre que possível. Por exemplo, começar a utilizar o Dropbox v2 em todos os lugares que usar o DropBox v1.

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [migrar manualmente as Logic apps](../logic-apps/logic-apps-schema-2015-08-01.md)
