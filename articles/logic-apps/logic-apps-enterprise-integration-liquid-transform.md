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
ms.openlocfilehash: c1a1a5530c19d39a8e37d122235c8340caa88570
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/20/2018
---
# <a name="perform-advanced-json-transformations-with-a-liquid-template"></a>Efetuar transformações de JSON avançadas com um modelo Liquid

Aplicações lógicas do Azure suporta básicas transformações de JSON através de ações de operação de dados nativos como **Compose** ou **analisar JSON**. Para transformações de JSON avançadas, pode utilizar modelos Liquid com as logic apps. 
[Liquid](https://shopify.github.io/liquid/) é uma linguagem de modelo de open source para aplicações web flexível.
 
Neste artigo, saiba como utilizar um mapa Liquid ou um modelo, que suporta transformações de JSON mais complexas, tais como iterações, fluxos de controlo, variáveis e assim sucessivamente. Antes de poder executar uma transformação Liquid na sua aplicação lógica, tem de definir o mapeamento de JSON para JSON com um mapa Liquid e arquivo que mapeie na sua conta de integração.

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. Se não tiver uma subscrição, pode [começar com uma conta do Azure gratuita](https://azure.microsoft.com/free/). Caso contrário, pode [inscrever-se numa subscrição Pay As You Go](https://azure.microsoft.com/pricing/purchase-options/).

* Conhecimento básico sobre [como criar aplicações lógicas](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Num nível básico [Integration Account](logic-apps-enterprise-integration-create-integration-account.md)


## <a name="create-a-liquid-template-or-map-for-your-integration-account"></a>Criar um modelo Liquid ou mapa para a sua conta de integração

1. Crie o modelo de Liquid de exemplo para este exemplo. O modelo Liquid define como transformar entrada JSON, conforme descrito aqui:

   ``` json
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

2. Inicie sessão no [portal do Azure](https://portal.azure.com).

3. No menu principal do Azure, escolha **todos os recursos**. 

4. Na caixa de pesquisa, localize e selecione a sua conta de integração.

   ![Selecione a conta de integração](./media/logic-apps-enterprise-integration-liquid-transform/select-integration-account.png)

5.  O mosaico da conta de integração, selecione **Maps**.

   ![Selecione o maps](./media/logic-apps-enterprise-integration-liquid-transform/add-maps.png)

6. Escolha **adicionar** e forneça estes detalhes para o mapa:

   * **Nome**: O nome para o mapa, que é "JsontoJsonTemplate" neste exemplo
   * **Mapear tipo**: O tipo para o mapa. Para o JSON de transformação de JSON, tem de selecionar **liquid**.
   * **Mapa**: um Liquid modelo ou do mapa ficheiro existente a utilizar para a transformação, que é "SimpleJsonToJsonTemplate.liquid" neste exemplo. Para localizar este ficheiro, pode utilizar o Seletor de ficheiros.

   ![Adicionar modelo liquid](./media/logic-apps-enterprise-integration-liquid-transform/add-liquid-template.png)
    
## <a name="add-the-liquid-action-for-json-transformation"></a>Adicionar a ação Liquid de transformação de JSON

1. [Criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

2. Adicionar o [acionador pedido](../connectors/connectors-native-reqres.md#use-the-http-request-trigger) à sua aplicação lógica.

3. Escolha **+ novo passo > Adicionar uma ação**. Na caixa de pesquisa, introduza *liquid*e selecione **Liquid - transformar JSON JSON**.

   ![Localize e selecione a ação Liquid](./media/logic-apps-enterprise-integration-liquid-transform/search-action-liquid.png)

4. No **conteúdo** caixa, selecione **corpo** na lista de conteúdo dinâmica ou lista de parâmetros, que é apresentada.
  
   ![Selecione o corpo](./media/logic-apps-enterprise-integration-liquid-transform/select-body.png)
 
5. Do **mapa** lista, selecione o seu modelo Liquid, o que é "JsonToJsonTemplate" neste exemplo.

   ![Selecione o mapa](./media/logic-apps-enterprise-integration-liquid-transform/select-map.png)

   Se a lista estiver vazia, a aplicação lógica, provavelmente, não está ligada à sua conta de integração. 
   Para ligar a sua aplicação lógica para a conta de integração que tenha Liquid modelo ou do mapa, siga estes passos:

   1. No menu aplicação lógica, selecione **definições de fluxo de trabalho**.
   2. Do **selecionar uma conta de integração** lista, selecione a sua conta de integração e escolha **guardar**.

   ![Aplicação de lógica de ligação para a conta de integração](./media/logic-apps-enterprise-integration-liquid-transform/link-integration-account.png)

## <a name="test-your-logic-app"></a>Testar a sua aplicação lógica

Após a entrada JSON para a sua aplicação de lógica de [Postman](https://www.getpostman.com/postman) ou uma ferramenta semelhante. A saída JSON transformada da sua aplicação lógica aspeto neste exemplo:
  
![Exemplo de saída](./media/logic-apps-enterprise-integration-liquid-transform/example-output.png)

## <a name="next-steps"></a>Passos Seguintes

* [Saiba mais sobre o Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Saiba mais sobre o pacote de integração do Enterprise")  
* [Saiba mais sobre o maps](../logic-apps/logic-apps-enterprise-integration-maps.md "Saiba mais sobre a maps de integração do enterprise")  

