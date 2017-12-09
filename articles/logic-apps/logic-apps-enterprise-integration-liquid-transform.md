---
title: "Converter os dados JSON com transformações Liquid - Azure Logic Apps | Microsoft Docs"
description: "Crie as transformações ou maps para transformações de JSON avançadas com Logic Apps e Liquid modelo."
services: logic-apps
documentationcenter: 
author: divyaswarnkar
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: LADocs; divswa
ms.openlocfilehash: cd177b1ebcb5d236ce265dc153ee6a02125a69df
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="advanced-json-transformations-using-liquid-template"></a>Avançadas transformações de JSON utilizando o modelo Liquid
Aplicações lógicas do Azure suporta básicas transformações de JSON através de ações de operação de dados nativas como Compose ou analisar JSON. As Logic Apps agora também suporta avançadas transformações de JSON com modelos Liquid. [Liquid](https://shopify.github.io/liquid/) é uma linguagem de modelo de open source para aplicações web flexível.
 
Neste artigo, saiba como utilizar um mapa Liquid ou um modelo, que pode suportar transformações de JSON mais complexas, tais como iterações, fluxos de controlo, variáveis e assim sucessivamente. Tem de definir o JSON para o mapeamento de JSON com este mapa Liquid e armazenar que o mapa na sua conta de integração antes de poder executar uma transformação Liquid na sua aplicação lógica.

## <a name="prerequisites"></a>Pré-requisitos
Seguem-se os pré-requisitos para utilizar a ação liquid:

* Uma subscrição do Azure. Se não tiver uma subscrição, pode [começar com uma conta do Azure gratuita](https://azure.microsoft.com/free/). Caso contrário, pode [inscrever-se numa subscrição Pay As You Go](https://azure.microsoft.com/pricing/purchase-options/).

* Conhecimento básico sobre [como criar aplicações lógicas](../logic-apps/logic-apps-create-a-logic-app.md).

* Num nível básico [Integration Account](logic-apps-enterprise-integration-create-integration-account.md).


## <a name="create-and-add-liquid-template-or-map-to-integration-account"></a>Criar e adicionar o modelo Liquid ou mapear a conta de integração

1. Crie o modelo de Liquid de exemplo para este exemplo. O modelo Liquid define como transformar entrada JSON, conforme descrito aqui:

   ```
   {%- assign deviceList = content.devices | Split: ', ' -%}
    {
        "fullName": "{{content.firstName | Append: ' ' | Append: content.lastName}}",
        "firstNameUpperCase": "{{content.firstName | Upcase}}",
        "phoneAreaCode": "{{content.phone | Slice: 1, 3}}",
        "devices" : [
        {%- for device in deviceList -%}
            {%- if forloop.Last == true -%}
            "{{device}}"
            {%- else -%}
            "{{device}}",
            {%- endif -%}
        {%- endfor -%}
        ]
    }
    ```
    > [!NOTE]
    > Se o seu modelo Liquid utiliza qualquer [filtros](https://shopify.github.io/liquid/basics/introduction/#filters), esses filtros tem de começar com letras maiúsculas. 

2. Inicie sessão no [Portal do Azure](https://portal.azure.com).

3. No menu principal do Azure, selecione **todos os recursos**. 

4. Na caixa de pesquisa, forneça a sua conta de integração. Selecione a sua conta.

   ![Selecione a conta de integração](./media/logic-apps-enterprise-integration-liquid-transform/select-integration-account.png)

5.  O mosaico da conta de integração, selecione **Maps**.

   ![Selecione o maps](./media/logic-apps-enterprise-integration-liquid-transform/add-maps.png)

6. Escolha **adicionar** e forneça estes detalhes para o mapa:
  * **Nome**: O nome para o mapa, que é "JsontoJsonTemplate" neste exemplo.
  * **Mapear tipo**: O tipo para o mapa. Para o JSON de transformação de JSON, tem de selecionar **liquid**.
  * **Mapa**: um Liquid modelo ou do mapa ficheiro existente a utilizar para a transformação, que é "SimpleJsonToJsonTemplate.liquid" neste exemplo. Pode utilizar o Seletor de ficheiros para localizar este ficheiro.

    ![Adicionar modelo liquid](./media/logic-apps-enterprise-integration-liquid-transform/add-liquid-template.png)

    
## <a name="add-the-liquid-action-to-transform-json-in-your-logic-app"></a>Adicionar a ação Liquid para transformar o JSON na sua aplicação lógica

1. [Criar uma aplicação lógica](logic-apps-create-a-logic-app.md).

2. Adicionar o [acionador pedido](../connectors/connectors-native-reqres.md#use-the-http-request-trigger) à sua aplicação lógica.

3. Escolha **+ novo passo > Adicionar uma ação**. Procurar *liquid* na caixa de pesquisa. Selecione **Liquid - transformação JSON JSON**.

  ![Liquid de ação de pesquisa](./media/logic-apps-enterprise-integration-liquid-transform/search-action-liquid.png)

4. No **conteúdo** caixa, selecione **corpo** na lista de conteúdo dinâmica ou lista de parâmetros, que é apresentada. 
  
  ![Selecione corpo](./media/logic-apps-enterprise-integration-liquid-transform/select-body.png)
 
5. Do **mapa** pendente lista, selecione o seu modelo Liquid, o que é JsonToJsonTemplate neste exemplo.

  ![Selecione mapa](./media/logic-apps-enterprise-integration-liquid-transform/select-map.png)

   Se a lista estiver vazia, a sua aplicação lógica provavelmente não está ligada à sua conta de integração. Para ligar a sua aplicação lógica para a conta de integração que tenha Liquid modelo ou do mapa, siga estes passos:

   1. No menu aplicação lógica, selecione **definições de fluxo de trabalho**. 
   2. Do **selecionar uma conta de integração** lista, selecione a sua conta de integração e escolha **guardar**.

     ![Aplicação de lógica de ligação para a conta de integração](./media/logic-apps-enterprise-integration-liquid-transform/link-integration-account.png)


## <a name="test-your-logic-app"></a>Testar a sua aplicação lógica

   Após a entrada JSON para a sua aplicação de lógica de [Postman](https://www.getpostman.com/postman) ou uma ferramenta semelhante. A saída JSON transformada da sua aplicação lógica aspeto neste exemplo:
  
   ![Exemplo de saída](./media/logic-apps-enterprise-integration-liquid-transform/example-output.png)


## <a name="next-steps"></a>Passos seguintes
* [Saiba mais sobre o Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Saiba mais sobre o pacote de integração do Enterprise")  
* [Saiba mais sobre o maps](../logic-apps/logic-apps-enterprise-integration-maps.md "Saiba mais sobre a maps de integração do enterprise")  

