---
title: "Como definir ou editar as políticas de API Management do Azure | Microsoft Docs"
description: "Este tópico mostra como definir ou editar as políticas de API Management do Azure."
services: api-management
documentationcenter: 
author: Juliako
manager: cflower
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: 409069cbc382610a48139df75f0f64b1682d8ee6
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="how-to-set-or-edit-azure-api-management-policies"></a>Como definir ou editar as políticas de API Management do Azure

A definição de política é um documento XML que descreve uma sequência de declarações de entrada e saídas. O XML pode ser editado diretamente na janela de definição. Também pode selecionar uma política predefinida da lista que é fornecida para a direita da janela de política. As declarações aplicáveis ao âmbito atual estiverem ativadas e realçadas. Clicar numa instrução ativada adiciona o XML adequado na localização de cursor na vista de definição. 

Para obter informações detalhadas sobre as políticas, consulte [políticas na API Management do Azure](api-management-howto-policies.md).

## <a name="set-or-edit-a-policy"></a>Definir ou editar uma política

Para definir ou editar uma política, siga os passos seguintes:

1. Inicie sessão no portal do Azure em [https://portal.azure.com](https://portal.azure.com).
2. Navegue até à sua instância APIM.
3. Clique em de **APIs** separador.
4. Selecione uma das APIs que importou anteriormente.
5. Selecione o **Design** separador.
6. Selecione uma operação para os quais pretende aplicar a política. Se pretender aplicar a política a todas as operações, selecione **todas as operações**.
7. Clique o triângulo junto a **entrada** ou **saída** pencils.
8. Selecione o **editor de código** item.

    ![Editar política](./media/set-edit-policies/set-edit-policies01.png)

9. Cole o código de política pretendidas para um dos blocos de adequado.
         
        <policies>
             <inbound>
                 <base />
             </inbound>
             <backend>
                 <base />
             </backend>
             <outbound>
                 <base />
             </outbound>
             <on-error>
                 <base />
             </on-error>
         </policies>
 
## <a name="configure-scope"></a>Configurar o âmbito

As políticas podem ser configuradas globalmente ou no âmbito de um produto, a API ou a operação. Para começar a configurar uma política, primeiro tem de selecionar o âmbito no qual a política deve ser aplicada.

Âmbitos de política são avaliados pela seguinte ordem:

1. Âmbito global
2. Âmbito do produto
3. Âmbito de API
4. Âmbito de operação

As declarações de políticas são avaliadas de acordo com a colocação do `base` elemento, se estiver presente. Política global não tem um principal de política e utilizar o `<base>` elemento no mesmo não tem qualquer efeito.

Para ver as políticas no âmbito atual no editor de política, clique em **recalcular política eficaz para âmbito selecionado**.

### <a name="global-scope"></a>Âmbito global

Âmbito global está configurado para **todas as APIs** na sua instância APIM.

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com/) e navegue para a instância APIM.
2. Clique em **todas as APIs**.

    ![Âmbito global](./media/api-management-howto-policies/global-scope.png)

3. Clique no ícone de triângulo.
4. Selecione **editor de código**.
5. Adicionar ou editar as políticas.
6. Prima **guardar**. 

    As alterações sejam propagadas do gateway de gestão de API imediatamente.

### <a name="product-scope"></a>Âmbito do produto

Âmbito do produto está configurado para o produto selecionado.

1. Clique em **produtos**.

    ![Âmbito do produto](./media/api-management-howto-policies/product-scope.png)

2. Selecione o produto para os quais pretende aplicar políticas.
3. Clique em **políticas**.
4. Adicionar ou editar as políticas.
5. Prima **guardar**. 

### <a name="api-scope"></a>Âmbito de API

Âmbito de API está configurado para **todas as operações** da API selecionada.

1. Selecione o **API** que pretende aplicar políticas.

    ![Âmbito de API](./media/api-management-howto-policies/api-scope.png)

2. Selecione **todas as operações**
3. Clique no ícone de triângulo.
4. Selecione **editor de código**.
5. Adicionar ou editar as políticas.
6. Prima **guardar**. 

### <a name="operation-scope"></a>Âmbito de operação 

Âmbito da operação está configurado para a operação selecionada.

1. Selecione um **API**.
2. Selecione a operação que pretende aplicar políticas.

    ![Âmbito de operação](./media/api-management-howto-policies/operation-scope.png)

3. Clique no ícone de triângulo.
4. Selecione **editor de código**.
5. Adicionar ou editar as políticas.
6. Prima **guardar**. 

## <a name="next-steps"></a>Passos seguintes

Consulte os seguintes tópicos relacionados:

+ [APIs de transformação](transform-api.md)
+ [Referência de política](api-management-policy-reference.md) para uma lista completa das declarações de política e as respetivas definições
+ [Exemplos de política](policy-samples.md)