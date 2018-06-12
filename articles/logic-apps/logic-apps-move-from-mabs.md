---
title: Mover as aplicações dos BizTalk Services para aplicações lógicas do Azure | Microsoft Docs
description: Mover ou migrar os serviços de BizTalk do Azure (MABS) para o Azure Logic Apps
services: logic-apps
documentationcenter: ''
author: jonfancey
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: jonfan; LADocs
ms.openlocfilehash: 846386172c0221c217430e62c8560484f799fa7f
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35299550"
---
# <a name="move-from-biztalk-services-to-azure-logic-apps"></a>Mover dos BizTalk Services para aplicações lógicas do Azure

Extinguir o serviços de BizTalk do Azure (MABS) da Microsoft. Para mover as soluções de integração MABS para [Azure Logic Apps](../logic-apps/logic-apps-overview.md), siga as orientações neste artigo. 

## <a name="introduction"></a>Introdução

BizTalk Services consiste em dois subservices:

* Microsoft BizTalk Services ligações híbridas
* Integração com base em bridge EAI e EDI

[Ligações híbridas do serviço de aplicação do Azure](../app-service/app-service-hybrid-connections.md) substitui as ligações do BizTalk Services híbridas. Ligações de híbrida do Azure está disponível com o App Service do Azure através do portal do Azure. Este serviço fornece um Gestor de ligações híbridas para que possa gerir as ligações híbridas do BizTalk Services existentes bem como novas ligações híbridas que criar no portal. 

[As Logic Apps](../logic-apps/logic-apps-overview.md) substitui a integração com base em bridge EAI e EDI com todas as mesmas capacidades no BizTalk Services e muito mais. Este serviço fornece funcionalidades com base no consumo de fluxo de trabalho e orquestração para que possa criar, rápida e facilmente soluções de integração complexos através de um browser ou com o Visual Studio de escala da nuvem.

Esta tabela mapeia as capacidades do BizTalk Services para aplicações lógicas.

| Serviços BizTalk   | Aplicações Lógicas            | Objetivo                      |
| ------------------ | --------------------- | ---------------------------- |
| Conector          | Conector             | Enviar e receber dados   |
| Bridge             | Aplicação Lógica             | Processador de pipeline           |
| Validar fase     | Ação de validação de XML | Validar um documento XML com um esquema | 
| Enriqueça a fase       | Tokens de dados           | Promova propriedades em mensagens de ou para tomar decisões de encaminhamento |
| Fase de transformação    | Ação de transformação      | Converter mensagens XML de um formato para outro |
| Descodificar fase       | Ação de descodificar o ficheiro simples | Converter a partir do ficheiro simples em XML |
| Fase de codificar       | Ação de codificar o ficheiro simples | Converter a partir de XML para ficheiros simples |
| Mensagem Inspector  | As funções do Azure ou das API Apps | Executar código personalizado no seu integrações |
| Ação de rota       | Condição ou comutador | Encaminhar mensagens para um dos conectores especificados |
|||| 

## <a name="biztalk-services-artifacts"></a>Artefactos de serviços BizTalk

BizTalk Services tem vários tipos de artefactos. 

## <a name="connectors"></a>Conectores

Conectores do BizTalk Services ajudam pontes enviar e receber dados, incluindo pontes bidirecionais, que permitem interações de pedido/resposta baseado em HTTP. As Logic Apps utiliza a mesma terminologia e tem 180 + conectores que fornecer a mesma finalidade, ligando-se para uma vasta gama de tecnologias e serviços. Por exemplo, os conectores estão disponíveis para a nuvem SaaS e serviços, tais como o OneDrive, Office 365, Dynamics CRM e muito mais, PaaS plus sistemas através do Gateway de dados no local, que substitui o BizTalk Adapter Service dos BizTalk Services no local. Origens nos BizTalk Services estão limitadas a subscrição de FTP, SFTP e fila do Service Bus ou tópico.

![](media/logic-apps-move-from-mabs/sources.png)

Por predefinição, cada bridge tem um ponto final de HTTP, que está configurado com o endereço de tempo de execução e as propriedades de endereço relativo para a bridge. Para alcançar os mesmos resultados com as Logic Apps, utilize o [pedido e resposta](../connectors/connectors-native-reqres.md) ações.

## <a name="xml-processing-and-bridges"></a>Processamento de XML e bridges

BizTalk Services, uma ponte é semelhante a um pipeline de processamento. Uma ponte pode demorar recebidos a partir de um conector de dados, entram algumas trabalhar com os dados e enviar os resultados para outro sistema. As Logic Apps tem a mesma funcionalidade que suporta os padrões de interação com base no pipeline mesmo como os BizTalk Services e também fornecem outros padrões de integração. O [Bridge de pedido-resposta XML](https://msdn.microsoft.com/library/azure/hh689781.aspx) nos BizTalk Services é conhecido como um pipeline VETER, que consiste em fases que efetuar estas tarefas:

* (V) validar
* (E) enriquecer a
* (T) transformação
* (E) enriquecer a
* (R) rota

Esta imagem mostra como processamento é dividido entre pedido e resposta, que fornece controlo sobre o pedido e os caminhos de resposta em separado, por exemplo, utilizando diferentes mapeia para cada caminho:

![](media/logic-apps-move-from-mabs/xml-request-reply.png)

Além disso, um XML bridge unidirecional adiciona fases Decode e codificar no início e no fim do processamento. A bridge de pass-through contém uma fase de Enrich único.

### <a name="message-processing-decoding-and-encoding"></a>Processamento de mensagens, descodificar e codificação

BizTalk Services, pode receber diferentes tipos de mensagens XML e determinar o esquema correspondente para a mensagem recebida. Este trabalho é executado no *tipos de mensagens* fase de pipeline de processamento de receção. A fase de Decode, em seguida, utiliza o tipo de mensagem detetado descodificar a mensagem utilizando o esquema fornecido. Se o esquema é um esquema de ficheiro simples, nesta fase converte o ficheiro de entrada simples em XML. 

As Logic Apps fornece capacidades semelhantes. Recebe um ficheiro simples através de protocolos diferentes, utilizando os acionadores de conector diferente (sistema de ficheiros, FTP, HTTP e assim sucessivamente) e, utilize o [descodificar o ficheiro simples](../logic-apps/logic-apps-enterprise-integration-flatfile.md) ação para converter os dados de entrada em XML. Pode mover as esquemas de ficheiro simples existente diretamente para Logic Apps sem quaisquer alterações e, em seguida, carregue esquemas para a sua conta de integração.

### <a name="validation"></a>Validação

Depois dos dados de entrada são convertidos para XML (ou se o XML foi o formato da mensagem recebido), a validação é executado para determinar se a mensagem respeita o esquema XSD. Para executar esta tarefa no Logic Apps, utilize o [validação XML](../logic-apps/logic-apps-enterprise-integration-xml-validation.md) ação. Pode utilizar os mesmos esquemas do BizTalk Services sem quaisquer alterações.

### <a name="transform-messages"></a>Mensagens de transformação

Nos serviços do BizTalk, a fase de transformação converte um formato de mensagem baseado em XML para outro. Este trabalho é feito aplicando um mapa, utilizando o mapeador de TRFM. As Logic Apps, o processo é semelhante. A ação de transformação executa um mapa da sua conta de integração. A principal diferença é que o maps em Logic Apps estão no formato XSLT. XSLT inclui a capacidade de reutilizar XSLT existente já tiver, incluindo o maps criados para BizTalk Server, que contêm functoids. 

### <a name="routing-rules"></a>Regras de encaminhamento

BizTalk Services faz uma decisão de encaminhamento no ponto final ou o conector para enviar mensagens a receber ou dados. A capacidade de selecionar a partir de pontos finais previamente configurados é possível utilizar a opção de filtro de encaminhamento:

![](media/logic-apps-move-from-mabs/route-filter.png)

BizTalk Services, se existem apenas duas opções, a utilizar um *condição* é a melhor forma para converter os filtros de encaminhamento nos BizTalk Services. Se existir mais do que dois, em seguida, utilize um **comutador**.

As Logic Apps proporciona capacidades sofisticadas lógica mais avançados de controlo de fluxo e o encaminhamento com [instruções condicionais](../logic-apps/logic-apps-control-flow-conditional-statement.md) e [comutador instruções](../logic-apps/logic-apps-control-flow-switch-statement.md).

### <a name="enrich"></a>Enriqueça

No processamento dos BizTalk Services, a fase de Enrich Adiciona propriedades para o contexto de mensagem associado com os dados recebidos. Por exemplo, ao promover uma propriedade a utilizar para o encaminhamento de uma pesquisa de base de dados, ou pelo extrair um valor utilizando uma expressão de XPath. As Logic Apps fornece acesso a todas as saídas de dados contextuais de precedente ações, tornando-o simples replicar o mesmo comportamento. Por exemplo, utilizando o `Get Row` ação de ligação do SQL Server, devolver dados a partir de uma base de dados do SQL Server e utilizar os dados numa ação decisões de encaminhamento. Da mesma forma, propriedades no barramento de serviço receber mensagens em fila por um acionador são endereçável, bem como XPath utilizando a expressão de linguagem de definição de fluxo de trabalho xpath.

### <a name="run-custom-code"></a>Executar código personalizado

BizTalk Services permite-lhe [executar código personalizado](https://msdn.microsoft.com/library/azure/dn232389.aspx) que é carregado na suas própria assemblagens. Esta funcionalidade é implementada pelo [IMessageInspector](https://msdn.microsoft.com/library/microsoft.biztalk.services.imessageinspector) interface. Cada fase a bridge inclui duas propriedades (no Inspector introduza e no Inspector de saída) que fornecem o tipo de .NET que criou que implemente esta interface. Código personalizado permite-lhe efetuar o processamento de mais complexo nos dados e permite-lhe reutilizar o código existente em assemblagens que executam a lógica de negócio comuns. 

As Logic Apps fornece duas formas de principais para executar código personalizado: das funções do Azure e API Apps. As funções do Azure podem ser criadas e chamadas a partir das logic apps. Consulte [adicionar e executar código personalizado para aplicações lógicas através das funções do Azure](../logic-apps/logic-apps-azure-functions.md). Utilize a API Apps, parte do App Service do Azure, para criar as suas próprias acionadores e ações. Saiba mais sobre [criar uma API personalizada a utilizar com as Logic Apps](../logic-apps/logic-apps-create-api-app.md). 

Se tiver o código personalizado em assemblagens que chamar a partir dos BizTalk Services, pode mover este código para as funções do Azure, ou criar APIs personalizadas com API Apps, consoante o que estiver a implementar. Por exemplo, se tiver o código que encapsula num wrapper para os quais as Logic Apps não tem um conector de outro serviço, em seguida, criar uma aplicação API e utilizar as ações que a aplicação API fornece na sua aplicação lógica. Se tiver de funções de programa auxiliar ou bibliotecas, em seguida, das funções do Azure é, provavelmente, a melhor opção.

### <a name="edi-processing-and-trading-partner-management"></a>EDI processar e gestão de parceiros comerciais

BizTalk Services e as Logic Apps incluem processamento EDI e B2B com suporte para AS2 (declaração de aplicabilidade 2), X12 e EDIFACT. Nos serviços do BizTalk, a criar bridges EDI e criar ou gerir parceiros comerciais e de contratos no portal de gestão e o controlo dedicado.
As Logic Apps, receberá esta funcionalidade do [Enterprise Integration Pack (EIP)](../logic-apps/logic-apps-enterprise-integration-overview.md). Fornece o EIP [Integration Account](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) e ações do B2B para processamento EDI e B2B. Também é utilizar uma conta de integração para criar e gerir [comerciais parceiros](../logic-apps/logic-apps-enterprise-integration-partners.md) e [contratos](../logic-apps/logic-apps-enterprise-integration-agreements.md). Depois de criar uma conta de integração, pode ligar um ou mais das logic apps para a conta. Em seguida, pode utilizar ações de B2B para aceder a informações de parceiros comerciais da sua aplicação lógica. São fornecidas as seguintes ações:

* Codificar AS2
* Descodificar AS2
* Codificar X12
* Descodificar o X12
* EDIFACT codificar
* EDIFACT descodificar

Ao contrário dos BizTalk Services, estas ações são dissociadas dos protocolos de transporte. Quando cria as logic apps, terá uma maior flexibilidade na quais os conectores que utilize para enviar e receber dados. Por exemplo, pode receber X12 ficheiros como anexos de e-mail e, em seguida, processo, estes ficheiros de uma aplicação lógica. 

## <a name="manage-and-monitor"></a>Gerir e monitorizar

Nos BizTalk Services, um portal dedicado fornecidas capacidades de controlo para monitorizar e resolver problemas. As Logic Apps fornece capacidades através de monitorização e controlo mais rico de [portal do Azure](../logic-apps/logic-apps-monitor-your-logic-apps.md)e inclui uma aplicação móvel para manter a par no coisas quando estiver em viagem.

## <a name="high-availability"></a>Elevada disponibilidade

Para elevada disponibilidade (HA) nos BizTalk Services, pode partilhar a carga de processamento através da utilização de mais do que uma instância numa região específica. As Logic Apps fornece HA na região sem custos adicionais. 

BizTalk Services, recuperação de desastres do limite da região para processamento de B2B necessita de um processo de cópia de segurança e restauro. Para a continuidade do negócio, as Logic Apps fornecem por várias regiões ativo/passivo [capacidade de DR](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md), que lhe permite sincronizar dados B2B em contas de automatização em diferentes regiões.

## <a name="next-steps"></a>Passos Seguintes

* [O que é Logic Apps?](../logic-apps/logic-apps-overview.md)
* [Criar a sua primeira aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md) ou começar rapidamente com um [modelo compilado previamente](../logic-apps/logic-apps-create-logic-apps-from-templates.md)  
* [Ver todos os conectores disponíveis](../connectors/apis-list.md) que pode utilizar as logic apps