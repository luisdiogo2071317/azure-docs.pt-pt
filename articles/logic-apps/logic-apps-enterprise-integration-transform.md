---
title: Converter os dados XML com transformações - Azure Logic Apps | Microsoft Docs
description: Criar as transformações ou mapps converter dados XML entre formatos nas logic apps, utilizando o SDK de integração do Enterprise
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: jeconnoc
editor: cgronlun
ms.assetid: add01429-21bc-4bab-8b23-bc76ba7d0bde
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 1621843d58954aa05a572f43fdab0fb16097332e
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35299458"
---
# <a name="enterprise-integration-with-xml-transforms"></a>Integração empresarial com transformações XML
## <a name="overview"></a>Descrição geral
O conector de transformação de integração do Enterprise converte dados de um formato para outro formato. Por exemplo, pode ter uma mensagem a receber que contém a data atual no formato YearMonthDay. Pode utilizar uma transformação para reformatar a data para estar no formato MonthDayYear.

## <a name="what-does-a-transform-do"></a>O que faz uma transformação?
Uma transformação, que também é conhecido como um mapa, é composta por um esquema de XML de origem (entrada) e um esquema de XML de destino (saída). Pode utilizar diferentes funções incorporadas para ajudar a manipular ou controlar os dados, incluindo manipulações de cadeia, atribuições condicionais, expressões aritméticas, ao mesmo tempo data hora e o mesmo ciclo construções.

## <a name="how-to-create-a-transform"></a>Como criar uma transformação?
Pode criar um mapa/transformação utilizando o Visual Studio [Enterprise integração SDK](https://aka.ms/vsmapsandschemas). Quando tiver terminado de criar e testar a transformação, carregue a transformação na sua conta de integração. 

## <a name="how-to-use-a-transform"></a>Como utilizar uma transformação
Depois de carregar o mapa/transformação na sua conta de integração, pode utilizá-lo para criar uma aplicação lógica. A aplicação lógica executa as transformações sempre que a aplicação lógica é acionada (e conteúdo de entrada que tem de ser transformados).

**Eis os passos a utilizar uma transformação**:

### <a name="prerequisites"></a>Pré-requisitos

* Criar uma conta de integração e adicione um mapeamento para o mesmo  

Agora que já Tratado dos pré-requisitos, está na altura de criar a sua aplicação lógica:  

1. Criar uma aplicação lógica e [ligá-lo à sua conta de integração](../logic-apps/logic-apps-enterprise-integration-accounts.md "saiba associar uma conta de integração para uma aplicação lógica") que contém o mapa.
2. Adicionar um **pedido** acionador à sua aplicação lógica  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-1.png)    
3. Adicionar o **transformação XML** ação selecionando primeiro **adicionar uma ação**   
   ![](./media/logic-apps-enterprise-integration-transforms/transform-2.png)   
4. Introduza a palavra *transformar* na caixa de pesquisa para filtrar todas as ações de que pretende utilizar  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-3.png)  
5. Selecione o **transformação XML** ação   
6. Adicionar o XML **conteúdo** que lhe transformar. Pode utilizar os dados XML recebe o pedido de HTTP como o **conteúdo**. Neste exemplo, selecione o corpo do pedido HTTP que acionou a aplicação lógica.

   > [!NOTE]
   > Certifique-se de que o conteúdo para o **transformação XML** XML. Se o conteúdo não em XML ou com codificação base64, tem de especificar uma expressão que processa o conteúdo. Por exemplo, pode utilizar [funções](logic-apps-workflow-definition-language.md#functions), como ```@base64ToBinary``` para descodificação conteúdo ou ```@xml``` para processar o conteúdo como XML.
 

7. Selecione o nome do **mapa** que pretende utilizar para efetuar a transformação. O mapa já deve estar na sua conta de integração. No passo anterior, já deu ao acesso de aplicação lógica à sua conta de integração que contém o mapa.      
   ![](./media/logic-apps-enterprise-integration-transforms/transform-4.png) 
8. Guarde o trabalho  
    ![](./media/logic-apps-enterprise-integration-transforms/transform-5.png) 

Neste momento, tiver terminado de configurar o mapa. Uma aplicação do mundo real, poderá armazenar os dados transformados numa aplicação LOB, tais como o SalesForce. Pode facilmente como uma ação para enviar o resultado da transformação Salesforce. 

Pode agora testar a transformação ao efetuar um pedido para o ponto final de HTTP.  


## <a name="features-and-use-cases"></a>Funcionalidades e casos de utilização
* A transformação criada num mapa pode ser simple, tais como copiar um nome e endereço de um documento para outra. Em alternativa, pode criar transformações mais complexas utilizando as operações de out of box mapa.  
* Várias operações de mapa ou as funções estejam prontamente disponíveis, incluindo cadeias, as funções de tempo de data e assim sucessivamente.  
* Para fazer uma cópia de dados diretas entre os esquemas. O mapeador de incluído no SDK, este é tão simple como desenhar uma linha que liga os elementos do esquema de origem com os seus homólogos do esquema de destino.  
* Ao criar um mapa, ver uma representação gráfica do mapa que mostra todas as relações e ligações que cria.
* Utilize a funcionalidade de mapa de teste para adicionar uma mensagem XML de exemplo. Com um clique simple, pode testar o mapa que criou e ver o resultado gerado.  
* Carregar o maps existentes  
* Inclui suporte para o formato XML.

## <a name="advanced-features"></a>Funcionalidades avançadas

### <a name="reference-assembly-or-custom-code-from-maps"></a>Assemblagem de referência ou de código personalizado do maps 
A ação de transformação também suporta maps ou transforma com referência ao assemblagem externo. Esta capacidade permite chamadas a código personalizado do .NET diretamente a partir do XSLT maps. Seguem-se os pré-requisitos para utilizar assemblagem Maps.

* O mapa e a assemblagem referenciada a partir do mapa tem de ser [carregada para a conta de integração](./logic-apps-enterprise-integration-maps.md). 

  > [!NOTE]
  > São necessários para ser carregado por uma ordem específica mapa e assemblagem. Tem de carregar a assemblagem antes de carregar o mapa que referencia a assemblagem.

* O mapa também tem de ter estes atributos e uma secção CDATA que contém a chamada para o código de assemblagem:

    * **nome** é o nome de assemblagem personalizada.
    * **espaço de nomes** é o espaço de nomes no seu assemblagem que inclui o código personalizado.

  Este exemplo mostra um mapa que referencia uma assemblagem com o nome "XslUtilitiesLib" e as chamadas a `circumreference` método da assemblagem.

  ````xml
  <?xml version="1.0" encoding="UTF-8"?>
  <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:msxsl="urn:schemas-microsoft-com:xslt" xmlns:user="urn:my-scripts">
  <msxsl:script language="C#" implements-prefix="user">
    <msxsl:assembly name="XsltHelperLib"/>
    <msxsl:using namespace="XsltHelpers"/>
    <![CDATA[public double circumference(int radius){ XsltHelper helper = new XsltHelper(); return helper.circumference(radius); }]]>
  </msxsl:script>
  <xsl:template match="data">
     <circles>
        <xsl:for-each select="circle">
            <circle>
                <xsl:copy-of select="node()"/>
                    <circumference>
                        <xsl:value-of select="user:circumference(radius)"/>
                    </circumference>
            </circle>
        </xsl:for-each>
     </circles>
    </xsl:template>
    </xsl:stylesheet>
  ````


### <a name="byte-order-mark"></a>Marca de ordem de bytes
Por predefinição, a resposta da transformação é iniciado o com marca de ordem de bytes (LM). Pode aceder a esta funcionalidade só ao trabalhar no editor de vista de código. Para desativar esta funcionalidade, especifique `disableByteOrderMark` para o `transformOptions` propriedade:

````json
"Transform_XML": {
    "inputs": {
        "content": "@{triggerBody()}",
        "integrationAccount": {
            "map": {
                "name": "TestMap"
            }
        },
        "transformOptions": "disableByteOrderMark"
    },
    "runAfter": {},
    "type": "Xslt"
}
````





## <a name="learn-more"></a>Saiba mais
* [Saiba mais sobre o Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Saiba mais sobre o pacote de integração do Enterprise")  
* [Saiba mais sobre o maps](../logic-apps/logic-apps-enterprise-integration-maps.md "Saiba mais sobre a maps de integração do enterprise")  

