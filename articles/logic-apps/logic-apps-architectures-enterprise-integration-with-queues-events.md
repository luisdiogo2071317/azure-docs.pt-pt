---
title: Padrão de arquitetura de integração do Enterprise - serviços de integração do Azure
description: Esta referência de arquitetura mostra como pode implementar um padrão de integração empresarial com o Azure Logic Apps, API Management do Azure, Azure Service Bus e Azure Event Grid
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: mattfarm
ms.author: mattfarm
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/15/2018
ms.openlocfilehash: 5b58f4b71d8d9f3ca91d8cefc6215073fd836765
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093671"
---
# <a name="enterprise-integration-architecture-with-queues-and-events"></a>Arquitetura de integração empresarial com filas e eventos

Este artigo descreve uma arquitetura de integração do enterprise que usa práticas comprovadas que pode aplicar a uma aplicação de integração ao utilizar os serviços de integração do Azure. Pode utilizar esta arquitetura como base para muitos padrões de aplicação diferente que necessitam de HTTP APIs, fluxo de trabalho e a orquestração.

![Diagrama da arquitetura - integração empresarial com filas e eventos](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

Esta série descreve os componentes reutilizáveis que podem ser aplicadas para criar um aplicativo de integração genérico. Porque a tecnologia de integração tem muitos aplicativos possíveis, desde aplicações ponto a ponto simples para aplicações empresariais completa do Service bus do Azure, considere os componentes que precisa implementar para as aplicações e infraestrutura.

## <a name="architecture-components"></a>Componentes de arquitetura

Esta arquitetura baseia-se a arquitetura descrita no artigo [referência de arquitetura: integração empresarial simples](/azure/architecture/reference-architectures/enterprise-integration/simple-enterprise-integration). Essa arquitetura [recomendações](/azure/architecture/reference-architectures/enterprise-integration/simple-enterprise-integration#recommendations) também se aplicam aqui, mas para fins de brevidade, este artigo omite essas recomendações da [recomendações](#recommendations) secção. Esta arquitetura de integração do enterprise inclui estes componentes:

- **Grupo de recursos**: A [grupo de recursos](../azure-resource-manager/resource-group-overview.md) é um contentor lógico de recursos do Azure.

- **Gestão de API do Azure**: A [gestão de API](https://docs.microsoft.com/azure/api-management/) service é uma plataforma totalmente gerida para publicação, proteger e transformar HTTP APIs.

- **Portal de programador da gestão de API do Azure**: cada instância da gestão de API do Azure fornece acesso para o [portal do programador](../api-management/api-management-customize-styles.md). Esse portal fornece acesso a exemplos de código e documentação. Também pode testar as APIs no portal do programador.

- **O Azure Logic Apps**: [Logic Apps](../logic-apps/logic-apps-overview.md) é uma plataforma sem servidor para a criação de fluxos de trabalho empresariais e integrações.

- **Conectores**: utiliza o Logic Apps [conectores](../connectors/apis-list.md) para ligar ao frequentemente utilizadas serviços. O Logic Apps oferece centenas de conectores, mas também pode criar um conector personalizado.

- **O Azure Service Bus**: [do Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) fornece mensagens seguras e fiáveis. Pode utilizar para a dissociação de aplicativos e integração com outros sistemas baseados em mensagens de mensagens.

- **O Azure Event Grid**: [Event Grid](../event-grid/overview.md) é uma plataforma sem servidor para publicação e a entrega de eventos da aplicação.

- **Endereço IP**: o serviço de gestão de API do Azure a tem uma pública fixa [endereço IP](../virtual-network/virtual-network-ip-addresses-overview-arm.md) e um nome de domínio. O nome de domínio predefinido é um subdomínio de azure-api.net, por exemplo, contoso. Azure-api.net, mas também pode configurar [domínios personalizados](../api-management/configure-custom-domain.md). Logic Apps e do Service Bus também tem um endereço IP público. No entanto, para segurança, esta arquitetura restringe o acesso para chamar pontos finais de Logic Apps para o endereço IP de gestão de API. As chamadas para o Service Bus são protegidas por uma assinatura de acesso partilhado (SAS).

- **O DNS do Azure**: [DNS do Azure](https://docs.microsoft.com/azure/dns/) é um serviço de alojamento para domínios DNS. O DNS do Azure oferece resolução de nomes através da infraestrutura do Microsoft Azure. Ao alojar os seus domínios no Azure, pode gerir os registos DNS ao utilizar as mesmas credenciais, APIs, ferramentas e de faturação que utiliza para os outros serviços do Azure. Para utilizar um nome de domínio personalizado, como contoso.com, crie registos DNS que mapeiam o nome de domínio personalizado para o endereço IP. Para obter mais informações, consulte [configurar um nome de domínio personalizado na gestão de API](../api-management/configure-custom-domain.md).

- **Azure Active Directory (Azure AD)**: pode utilizar [do Azure AD](https://docs.microsoft.com/azure/active-directory/) ou outro fornecedor de identidade para a autenticação. O Azure AD fornece autenticação para aceder a pontos finais da API, passando uma [JSON Web Token para a gestão de API](../api-management/policies/authorize-request-based-on-jwt-claims.md) para validar. Para os escalões Standard e Premium, do Azure AD pode proteger o acesso ao portal do programador da gestão de API.

## <a name="patterns"></a>Padrões 

Esta arquitetura utiliza alguns padrões que são fundamentais para operação:

* HTTP APIs existente back-end são publicadas através do portal do programador da gestão de API. No portal, os desenvolvedores, que são internos para sua organização, externas ou ambas,  
Pode integrar a chamadas para essas APIs nas aplicações.

* APIs compostas baseiam-se com o logic apps, o que orquestrar chamadas para o software como um sistemas de serviço (SaaS), serviços do Azure e qualquer APIs que são publicados para a gestão de API. Aplicações lógicas são também [publicados através do portal do programador da gestão de API](../api-management/import-logic-app-as-api.md).

* Aplicações utilizam o Azure AD para [obter um token de segurança de OAuth 2.0](../api-management/api-management-howto-protect-backend-with-aad.md) que é necessário para obter acesso a uma API.

* Gestão de API do Azure [valida o token de segurança](../api-management/api-management-howto-protect-backend-with-aad.md) e, em seguida, transmite o pedido para a aplicação de API ou lógica de back-end.

* As filas do Service Bus do Azure são utilizadas para [desacoplamento](../service-bus-messaging/service-bus-messaging-overview.md) atividade de aplicativo e para [suavização picos de carga](https://docs.microsoft.com/azure/architecture/patterns/queue-based-load-leveling). As mensagens são adicionadas filas pelo logic apps, aplicações de terceiros, ou (não mostradas) ao publicar a fila como uma API de HTTP através da gestão de API.

* Quando as mensagens são adicionadas a uma fila do Service Bus, um evento é disparado. O evento é acionado uma aplicação lógica, que, em seguida, processa a mensagem.

* Outros serviços do Azure, como o armazenamento de Blobs do Azure e Hubs de eventos do Azure, também publicam eventos para o Event Grid. Estes serviços acionam logic apps para receber o evento e, em seguida, executar ações subsequentes.

## <a name="recommendations"></a>Recomendações

Seus requisitos específicos poderão diferir da arquitetura do genérica descrita neste artigo. Utilize as recomendações nesta secção como um ponto de partida.

### <a name="service-bus-tier"></a>Escalão do Service Bus

Utilize o escalão Premium do Service Bus, que suporta notificações do Event Grid. Para obter mais informações, consulte [preços do Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Preços do Event Grid

Grelha de eventos utiliza um modelo sem servidor. A faturação é calculada com base no número de operações (execuções de evento). Para obter mais informações, consulte [preços do Event Grid](https://azure.microsoft.com/pricing/details/event-grid/). Atualmente, não existem não existem considerações de camada para o Event Grid.

### <a name="use-peeklock-to-consume-service-bus-messages"></a>Utilizar PeekLock para consumir mensagens do Service Bus

Ao criar uma aplicação lógica para consumir mensagens do Service Bus, tem a aplicação lógica, utilize [PeekLock](../service-bus-messaging/service-bus-messaging-overview.md#queues) para aceder a um grupo de mensagens. Quando utiliza PeekLock, a aplicação lógica pode realizar passos para validar cada mensagem antes de a conclusão ou abandono da mensagem. Essa abordagem protege contra a perda acidental de mensagens.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Verificar a existência de vários objetos quando é acionada de um acionador do Event Grid

Quando for acionado um acionador do Event Grid, esta ação significa simplesmente que ", pelo menos, uma dessas coisas aconteceu." Por exemplo, quando o Event Grid aciona uma aplicação lógica numa mensagem que aparece numa fila do Service Bus, a aplicação lógica deve sempre assumir que pode haver uma ou mais mensagens disponíveis para processar.

### <a name="region"></a>Região

Para minimizar a latência de rede, escolha a mesma região para a gestão de API, o Logic Apps e o Service Bus. Em geral, selecione a região mais próxima dos seus utilizadores.

O grupo de recursos tem também uma região. Esta região especifica onde pretende armazenar os metadados de implementação e a localização para executar o modelo de implementação. Para melhorar a disponibilidade durante a implementação, coloque o grupo de recursos e os recursos na mesma região.

## <a name="scalability"></a>Escalabilidade

Para atingir maior escalabilidade, o escalão Premium do Service Bus pode aumentar horizontalmente o número de unidades de mensagens. Configurações de escalão Premium podem ter um, dois ou quatro unidades de mensagens. Para obter mais informações sobre o dimensionamento do Service Bus, consulte [melhores práticas para melhorar o desempenho através da utilização de mensagens do Service Bus](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability"></a>Disponibilidade

* Para os escalões Basic, Standard e Premium, o contrato de nível de serviço (SLA) para a gestão de API do Azure está atualmente 99,9%. Para configurações de escalão premium com uma implementação que tem, pelo menos, uma unidade em duas ou mais regiões, o SLA é de 99,95%.

* O SLA para o Azure Logic Apps atualmente é de 99,9%.

### <a name="disaster-recovery"></a>Recuperação após desastre

Para ativar a ativação pós-falha se ocorrer uma falha grave, considere a implementação de recuperação após desastre geográfico no Premium do Service Bus. Para obter mais informações, consulte [recuperação de desastre geográfico de Azure Service Bus](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability"></a>Capacidade de gestão

Criar grupos de recursos separados para a produção, desenvolvimento e os ambientes de teste. Grupos de recursos separados facilitam a gerir implementações, eliminar as implementações de teste e atribuir direitos de acesso.

Ao atribuir recursos a grupos de recursos, considere estes fatores:

* **Ciclo de vida**: em geral, coloque os recursos que têm o mesmo ciclo de vida no mesmo grupo de recursos.

* **Acesso**: para aplicar políticas de acesso a recursos num grupo, pode usar [o controlo de acesso baseado em funções (RBAC)](../role-based-access-control/overview.md).

* **Faturação**: pode ver os custos de rollup para o grupo de recursos.

* **Escalão de preço para a gestão de API**: utilizar o escalão de programador para os seus ambientes de desenvolvimento e teste. Para minimizar os custos durante pré-produção, implementação de uma réplica do ambiente de produção, em seguida, executar seus testes e, em seguida, encerrado.

Para obter mais informações, veja [Descrição geral do Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="deployment"></a>Implementação

* Para implementar a gestão de API, o Logic Apps, o Event Grid e o Service Bus, utilize o [modelos Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md). Modelos facilitam Implantações de automatização com o PowerShell ou a CLI do Azure.

* Colocar a gestão de API, quaisquer espaços de nomes do Service Bus, tópicos do Event Grid e aplicações lógicas individuais em seus próprios modelos do Resource Manager separado. Ao utilizar modelos separados, pode armazenar os recursos em sistemas de controle de origem. Em seguida, pode implementar estes modelos em conjunto ou individualmente como parte de um processo de implementação (CI/CD) integração contínua/contínua.

## <a name="diagnostics-and-monitoring"></a>Diagnóstico e monitorização

Tal como gestão de API e aplicações lógicas, pode monitorizar do Service Bus com o Azure Monitor, que está ativada por predefinição. O Azure Monitor proporciona informações com base nas métricas que estão configuradas para cada serviço. 

## <a name="security"></a>Segurança

Para proteger o Service Bus, utilize a assinatura de acesso partilhado (SAS). Por exemplo, pode conceder um acesso de utilizador aos recursos do Service Bus com direitos específicos usando [autenticação de SAS](../service-bus-messaging/service-bus-sas.md). Para obter mais informações, consulte [do Service Bus autenticação e autorização](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Se tiver de expor uma fila do Service Bus como um ponto de final HTTP, por exemplo, para publicar novas mensagens, utilize a gestão de API para proteger a fila por fronting o ponto final. Em seguida, pode proteger o ponto final com certificados ou de autenticação OAuth conforme apropriado. A maneira mais fácil, pode proteger um ponto final está a utilizar uma aplicação lógica com um acionador de pedido/resposta HTTP como um intermediário.

O serviço do Event Grid protege a entrega de eventos por meio de um código de validação. Se utilizar o Logic Apps para consumir o evento, validação é executada automaticamente. Para obter mais informações, consulte [Event Grid segurança e autenticação](../event-grid/security-authentication.md).

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [integração empresarial simples](/azure/architecture/reference-architectures/enterprise-integration/simple-enterprise-integration)
