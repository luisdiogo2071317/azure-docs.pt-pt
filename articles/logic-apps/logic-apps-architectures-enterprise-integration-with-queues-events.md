---
title: Azure arquitetura de referência de integração do serviços de integração empresarial
description: Descreve a arquitetura de referência mostra como implementar um padrão de integração empresarial com o Logic Apps, gestão de API, do Service Bus e Event Grid.
author: mattfarm
manager: jonfan
editor: ''
services: logic-apps api-management
documentationcenter: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/15/2018
ms.author: LADocs; estfan
ms.openlocfilehash: ffa61ebfaa58425cd2bf70d9bf78a2e71b672369
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42918488"
---
# <a name="reference-architecture-enterprise-integration-with-queues-and-events"></a>Arquitetura de referência: integração empresarial com filas e eventos

A arquitetura de referência seguinte mostra um conjunto de práticas comprovadas que pode aplicar a uma aplicação de integração que utiliza serviços de integração do Azure. A arquitetura pode servir como base para muitos padrões de aplicação diferente que necessitam de HTTP APIs, fluxo de trabalho e a orquestração.

![Diagrama da arquitetura - integração empresarial com filas e eventos](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

*Há muitos aplicativos possíveis para a tecnologia de integração. Eles variam de um aplicativo de ponto a ponto simples para uma empresa de completo de aplicação do Azure Service Bus. A série de arquitetura descreve os componentes reutilizáveis que podem ser aplicadas para criar um aplicativo de integração genérico. Arquitetos devem considerar quais componentes que precisam implementar para a sua aplicação e infraestrutura.*
<!-- Should previous line be in Italic or asterisks must be escaped? -->

## <a name="architecture"></a>Arquitetura

A arquitetura *baseia-se nas* a [integração empresarial simples](logic-apps-architectures-simple-enterprise-integration.md) arquitetura. [Recomendações para a arquitetura empresarial simples](logic-apps-architectures-simple-enterprise-integration.md#recommendations) se aplicam aqui. Eles são omitidos dos [recomendações](#recommendations) neste artigo para fins de brevidade. 

A arquitetura é composta pelos seguintes componentes:

- **Grupo de recursos**. Um [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) é um contentor lógico de recursos do Azure.
- **Gestão de API do Azure**. [Gestão de API](https://docs.microsoft.com/azure/api-management/) é uma plataforma totalmente gerida que é utilizada para publicar, proteger e transforme as APIs de HTTP.
- **Portal de programador da gestão de API do Azure**. Cada instância da gestão de API do Azure é fornecido com o acesso para o [portal do programador](https://docs.microsoft.com/azure/api-management/api-management-customize-styles). O portal do programador da gestão de API fornece acesso a exemplos de código e documentação. Pode testar as APIs no portal do programador.
- **Azure Logic Apps**. [O Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) é uma plataforma sem servidor que é utilizada para criar o fluxo de trabalho empresariais e de integração.
- **Conectores**. O Logic Apps utiliza [conectores](https://docs.microsoft.com/azure/connectors/apis-list) ligar ao normalmente utilizou os serviços. O Logic Apps já tenha centenas de conectores diferentes, mas também pode criar um conector personalizado.
- **O Azure Service Bus**. [Do Service Bus](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview) fornece mensagens seguras e fiáveis. Sistema de mensagens pode ser utilizado para desassociar os aplicativos e integrar com outros sistemas baseados em mensagens.
- **Azure Event Grid**. [Grelha de eventos](https://docs.microsoft.com/azure/event-grid/overview) é uma plataforma sem servidor que é utilizada para publicar e entregar eventos de aplicativo.
- **Endereço IP**. O serviço de gestão de API do Azure tem uma pública fixa [endereço IP](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) e um nome de domínio. O nome de domínio predefinido é um subdomínio de azure-api.net, por exemplo, contoso. Azure-api.net, mas [domínios personalizados](https://docs.microsoft.com/azure/api-management/configure-custom-domain) também podem ser configuradas. Logic Apps e do Service Bus também tem um endereço IP público. No entanto, nesta arquitetura, podemos restringir o acesso para chamar pontos finais de Logic Apps para o endereço IP de gestão de API (para segurança). As chamadas para o Service Bus são protegidas por uma assinatura de acesso partilhado (SAS).
- **DNS do Azure**. [O DNS do Azure](https://docs.microsoft.com/azure/dns/) é um serviço de alojamento para domínios DNS. O DNS do Azure oferece resolução de nomes através da infraestrutura do Microsoft Azure. Ao alojar os seus domínios no Azure, pode gerir os registos DNS ao utilizar as mesmas credenciais, APIs, ferramentas e de faturação que utiliza para os outros serviços do Azure. Para utilizar um nome de domínio personalizado (como contoso.com), crie registos DNS que mapeiam o nome de domínio personalizado para o endereço IP. Para obter mais informações, consulte [configurar um nome de domínio personalizado na gestão de API](https://docs.microsoft.com/en-us/azure/api-management/configure-custom-domain).
- **Azure Active Directory (Azure AD)**. Uso [do Azure AD](https://docs.microsoft.com/azure/active-directory/) ou outro fornecedor de identidade para a autenticação. O Azure AD fornece autenticação para aceder a pontos finais da API, passando uma [JSON Web Token para a gestão de API](https://docs.microsoft.com/azure/api-management/policies/authorize-request-based-on-jwt-claims) para validar. O Azure AD pode proteger o acesso ao portal do Programador de gestão de API (apenas escalões Standard e Premium).

A arquitetura tem alguns padrões que são fundamentais para a sua operação:

* HTTP APIs existente back-end são publicadas através do portal do programador da gestão de API. No portal, os desenvolvedores (qualquer um dos interno para sua organização, externas ou ambas) pode integrar a chamadas para essas APIs nas aplicações.
* APIs compostas são criadas ao utilizar o logic apps e ao orquestrar a chamadas para o software como um sistemas de serviço (SaaS), serviços do Azure e qualquer APIs que são publicados para a gestão de API. [Aplicações lógicas também são publicadas](https://docs.microsoft.com/azure/api-management/import-logic-app-as-api) através do portal do programador da gestão de API.
- Aplicações utilizam o Azure AD para [adquirir um token de segurança de OAuth 2.0](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) que é necessário para obter acesso a uma API.
- Gestão de API [valida o token de segurança](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) e, em seguida, transmite o pedido para a aplicação de API ou lógica de back-end.
- Filas do Service Bus estão habituadas [desacoplar](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) atividade das aplicações e ao [suavizar os picos de carga](https://docs.microsoft.com/azure/architecture/patterns/queue-based-load-leveling). As mensagens são adicionadas filas pelo logic apps, aplicações de terceiros, ou (não mostradas) ao publicar a fila como uma API de HTTP através da gestão de API.
- Quando as mensagens são adicionadas a uma fila do Service Bus, um evento é disparado. Uma aplicação lógica é acionada pelo evento. A aplicação lógica, em seguida, processa a mensagem.
- Outros serviços do Azure (por exemplo, o armazenamento de Blobs do Azure e o Event Hubs do Azure) também publicam eventos para o Event Grid. Estes serviços acionam logic apps para receber o evento e, em seguida, executar ações subsequentes.

## <a name="recommendations"></a>Recomendações

Seus requisitos específicos poderão diferir da arquitetura do genérica descrita neste artigo. Utilize as recomendações nesta secção como um ponto de partida.

### <a name="service-bus-tier"></a>Escalão do Service Bus

Utilize o escalão Premium do Service Bus. O Premier escalão suporta notificações do Event Grid. Para obter mais informações, consulte [preços do Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Preços do Event Grid

Grelha de eventos utiliza um modelo sem servidor. A faturação é calculada com base no número de operações (execução do evento). Para obter mais informações, consulte [preços do Event Grid](https://azure.microsoft.com/pricing/details/event-grid/). Atualmente, não existem não existem considerações de camada para o Event Grid.

### <a name="use-peeklock-to-consume-service-bus-messages"></a>Utilizar PeekLock para consumir mensagens do Service Bus

Quando cria uma aplicação lógica para consumir mensagens do Service Bus, utilize [PeekLock](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#queues) na aplicação lógica para aceder a um grupo de mensagens. Quando utiliza PeekLock, a aplicação lógica pode realizar passos para validar cada mensagem antes de a conclusão ou abandono da mensagem. Essa abordagem protege contra a perda acidental de mensagens.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Verificar a existência de vários objetos quando é acionada de um acionador do Event Grid

Quando é acionado um acionador do Event Grid, significa simplesmente que ", pelo menos, uma dessas coisas aconteceu." Por exemplo, quando o Event Grid aciona uma aplicação lógica numa mensagem que aparece numa fila do Service Bus, a aplicação lógica deve sempre assumir que pode haver uma ou mais mensagens disponíveis para processar.

### <a name="region"></a>Região

Aprovisione a gestão de API, o Logic Apps e o Service Bus na mesma região para minimizar a latência de rede. Em geral, selecione a região mais próxima dos seus utilizadores.

O grupo de recursos tem também uma região. A região especifica onde os metadados de implementação são armazenados e onde o modelo de implementação é executado de. Coloca o grupo de recursos e os respetivos recursos na mesma região para melhorar a disponibilidade durante a implementação.

## <a name="scalability"></a>Escalabilidade

O escalão Premium do Service Bus pode aumentar horizontalmente o número de unidades de mensagens para atingir maior escalabilidade. Configurações de escalão Premium podem ter um, dois ou quatro unidades de mensagens. Para obter mais informações sobre o dimensionamento do Service Bus, consulte [melhores práticas para melhorar o desempenho através da utilização de mensagens do Service Bus](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability"></a>Disponibilidade

Atualmente, o contrato de nível de serviço (SLA) para a gestão de API do Azure é de 99,9% para os escalões Basic, Standard e Premium. Configurações de escalão Premium com a implementação de, pelo menos, uma unidade em duas ou mais regiões têm um SLA de 99,95%.

Atualmente, o SLA para o Azure Logic Apps é de 99,9%.

### <a name="disaster-recovery"></a>Recuperação após desastre

Pondere implementar a recuperação após desastre geográfico no Premium do Service Bus para ativar a ativação pós-falha, se ocorrer uma falha grave. Para obter mais informações, consulte [recuperação de desastre geográfico de Azure Service Bus](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability"></a>Capacidade de gestão

Criar grupos de recursos separados para a produção, desenvolvimento e os ambientes de teste. Grupos de recursos separados torna mais fácil gerir as implementações, eliminar as implementações de teste e atribuir direitos de acesso.

Ao atribuir recursos a grupos de recursos, considere os seguintes fatores:

- **Ciclo de vida**. Em geral, coloque os recursos que têm o mesmo ciclo de vida no mesmo grupo de recursos.
- **Acesso**. Pode usar [controlo de acesso baseado em funções](../role-based-access-control/overview.md) (RBAC) para aplicar políticas de acesso a recursos num grupo.
- **Faturação**. Pode ver os custos de rollup para o grupo de recursos.
- **Escalão de preço para a gestão de API**. Recomendamos utilizar o escalão de programador para o desenvolvimento e os ambientes de teste. Para pré-produção, recomendamos a implementação de uma réplica do ambiente de produção, a execução de testes e, em seguida, encerrar para minimizar os custos.

Para obter mais informações, veja [Descrição geral do Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="deployment"></a>Implementação

Recomendamos que utilize [modelos Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) para implementar a gestão de API, o Logic Apps, o Event Grid e o Service Bus. Modelos tornam mais fácil automatizar as implementações através do PowerShell ou a CLI do Azure.

Recomendamos que o serviço de colocação de gestão de API, quaisquer espaços de nomes do Service Bus, tópicos do Event Grid e aplicações lógicas individuais em seus próprios, modelos do Resource Manager separado. Ao utilizar modelos separados, pode armazenar os recursos em sistemas de controle de origem. Em seguida, pode implementar estes modelos em conjunto ou individualmente como parte de um processo de implementação (CI/CD) integração contínua/contínua.

### <a name="diagnostics-and-monitoring"></a>Diagnóstico e monitorização

Tal como gestão de API e aplicações lógicas, pode monitorizar o Service Bus com o Azure Monitor. O Azure Monitor proporciona informações com base nas métricas que estão configuradas para cada serviço. Monitor do Azure está ativado por predefinição.

## <a name="security"></a>Segurança

Proteja o barramento de serviço com SAS. Pode usar [autenticação de SAS](../service-bus-messaging/service-bus-sas.md) para conceder um acesso de utilizador aos recursos do Service Bus com direitos específicos. Para obter mais informações, consulte [do Service Bus autenticação e autorização](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Se precisar de uma fila do Service Bus sejam expostas como um ponto de final HTTP (para o lançamento de novas mensagens), deve utilizar a gestão de API para protegê-lo colocando fronting o ponto final. O ponto final, em seguida, pode ser protegido com certificados ou OAuth conforme apropriado. É a maneira mais fácil de proteger um ponto de extremidade através de uma aplicação lógica com um acionador HTTP de solicitação/resposta como um intermediário.

Grelha de eventos protege a entrega de eventos por meio de um código de validação. Se utilizar o Logic Apps para consumir o evento, a validação é executada automaticamente. Para obter mais informações, consulte [Event Grid segurança e autenticação](../event-grid/security-authentication.md).

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [integração empresarial simples](logic-apps-architectures-simple-enterprise-integration.md).
