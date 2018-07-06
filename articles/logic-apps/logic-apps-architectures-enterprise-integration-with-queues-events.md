---
title: Arquitetura de referência de serviços de integração do Azure
description: Arquitetura de referência, que mostra como implementar um padrão de integração empresarial com o Logic Apps, gestão de API, do Service Bus e Event Grid
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
ms.openlocfilehash: 7d14bbd587b5086348026c41f6551b10809b939a
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37859647"
---
# <a name="enterprise-integration-with-queues-and-events-reference-architecture"></a>Integração empresarial com filas e os eventos: arquitetura de referência

## <a name="overview"></a>Descrição geral

Esta arquitetura de referência mostra um conjunto de práticas comprovadas para uma aplicação de integração que utiliza serviços de integração do Azure. Esta arquitetura pode servir como esta base de muitos padrões de aplicação diferente que requerem HTTP APIs, o fluxo de trabalho e orquestração.

*Há muitos aplicativos possíveis de tecnologia de integração, de um aplicativo de ponto de simples ponto-a para um barramento de serviço completo de enterprise. Esta série de arquitetura define as partes do componente reutilizável que poderão aplicar-se para a criação de uma aplicação de integração genérico – arquitetos devem considerar quais componentes específicos, têm de implementar para seus aplicativos e infra-estrutura.*

![Diagrama da arquitetura - integração empresarial com filas e eventos](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

## <a name="architecture"></a>Arquitetura

Esta arquitetura **baseia-se nas** a [integração empresarial simples](logic-apps-architectures-simple-enterprise-integration.md) arquitetura. O [recomendações de arquitetura empresarial simples](logic-apps-architectures-simple-enterprise-integration.md#recommendations) também se aplicam aqui, mas foi omitido dos [recomendações](#recommendations) neste documento para fins de brevidade. Ele tem os seguintes componentes:

- Grupo de recursos. Um [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) é um contentor lógico de recursos do Azure.
- Gestão de API do Azure. [Gestão de API do Azure](https://docs.microsoft.com/azure/api-management/) é uma plataforma totalmente gerida para publicação, proteger e transformar HTTP APIs.
- Portal de programador da gestão de API do Azure. Cada instância da gestão de API do Azure é fornecido com um [Portal do programador](https://docs.microsoft.com/azure/api-management/api-management-customize-styles), fornecer acesso à documentação, exemplos de código e a capacidade de testar uma API.
- Azure Logic Apps. [O Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) é uma plataforma sem servidor para a criação de fluxo de trabalho empresariais e de integração.
- Conectores. [Conectores](https://docs.microsoft.com/azure/connectors/apis-list) são utilizadas pelo Logic Apps para ligar aos serviços frequentemente utilizados. O Logic Apps já tenha centenas de conectores diferentes, mas também podem ser criadas com um conector personalizado.
- Barramento de serviço do Azure. [Do Service Bus](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-messaging-overview) fornece mensagens seguras e fiáveis. Sistema de mensagens pode ser utilizado para anular acoplar aplicações umas das outras e integrar com outros sistemas baseados em mensagens.
- Grelha de eventos do Azure. [Grelha de eventos](https://docs.microsoft.com/azure/event-grid/overview) é uma plataforma sem servidor para publicação e a entrega de eventos da aplicação.
- Endereço IP. O serviço de gestão de API do Azure tem uma pública fixa [endereço IP](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) e um nome de domínio. O nome de domínio é um subdomínio do azure-api.net, por exemplo, contoso. Azure-api.net. Logic Apps e do Service Bus também tem um endereço IP público; No entanto, nesta arquitetura, restringir o acesso para chamar pontos finais de aplicações de lógica para apenas da API de gestão de endereços IP (para segurança). As chamadas para o Service Bus são protegidas por uma assinatura de acesso partilhado.
- DNS do Azure. [O DNS do Azure](https://docs.microsoft.com/azure/dns/) é um serviço de alojamento para domínios DNS que fornece resolução de nomes através da infraestrutura do Microsoft Azure. Ao alojar os seus domínios no Azure, pode gerir os recursos DNS com as mesmas credenciais, APIs, ferramentas e faturação dos seus outros serviços do Azure. Para utilizar um nome de domínio personalizado (por exemplo, contoso.com) crie registos DNS que mapeiam o nome de domínio personalizado para o endereço IP. Para obter mais informações, consulte Configurar um nome de domínio personalizado na API Management do Azure.
- Azure Active Directory (Azure AD). Uso [do Azure AD](https://docs.microsoft.com/azure/active-directory/) ou outro fornecedor de identidade para a autenticação. O Azure AD fornece autenticação para pontos finais de API de acesso (passando um [JSON Web Token para a gestão de API](https://docs.microsoft.com/azure/api-management/policies/authorize-request-based-on-jwt-claims) para validar) e pode proteger o acesso ao Portal de Programador de gestão de API (apenas escalões Standard e Premium).

Esta arquitetura tem alguns padrões fundamentais para a sua operação:

1. Back-end existente HTTP APIs são publicadas através do Portal de programador da gestão API, que permite aos programadores (qualquer um dos interno para sua organização, externas ou ambas) para integrar a chamadas para essas APIs nas aplicações.
2. APIs compostas baseiam-se utilizar o Logic Apps; orquestrar a chamadas para sistemas SAAS, serviços do Azure e qualquer APIs publicadas para gestão de API. O [Logic Apps também são publicadas](https://docs.microsoft.com/azure/api-management/import-logic-app-as-api) através do Portal de Programador de gestão de API.
3. Aplicativos [adquirir um token de segurança de OAuth 2.0](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) necessário para obter acesso a uma API com o Azure Active Directory.
4. Gestão de API do Azure [valida o token de segurança](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad)e transmite o pedido para o aplicação API/lógica de back-end.
5. Filas do Service Bus estão habituadas [desacoplar](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) atividade das aplicações e [suavizar os picos de carga](https://docs.microsoft.com/azure/architecture/patterns/queue-based-load-leveling). As mensagens são adicionadas filas pelo Logic Apps, 3ª aplicativos de terceiros, ou (não mostradas) ao publicar a fila como uma API de HTTP através da gestão de API.
6. Quando as mensagens são adicionadas a uma fila do Service Bus, um evento é disparado. Uma aplicação lógica é acionada por este evento e processar a mensagem.
7. Da mesma forma, outros serviços do Azure (por exemplo, o armazenamento de BLOBs, o Hub de eventos) também publicam eventos para o Event Grid. Estes acionam o Logic Apps para receber o evento e executar ações subsequentes.

## <a name="recommendations"></a>Recomendações

Seus requisitos específicos poderão diferir da arquitetura genérica descrita aqui. Utilize as recomendações nesta secção como um ponto de partida.

### <a name="service-bus-tier"></a>Escalão do Service Bus

Utilizar o escalão premium do Service Bus como isto suporta atualmente notificações de grelha de eventos (o suporte em todos os escalões é esperado). Ver [preços do Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Preços do Event Grid

Grelha de eventos funciona através de um modelo sem servidor – a faturação é calculada com base no número de operações (execução do evento). Ver [preços do Event Grid](https://azure.microsoft.com/pricing/details/event-grid/) para obter mais informações. Não existem atualmente não existem considerações de camada para o Event Grid.

### <a name="use-peeklock-when-consuming-service-bus-messages"></a>Utilizar PeekLock ao consumir mensagens do Service Bus

Quando criar Logic Apps para consumir mensagens do Service Bus, utilize [PeekLock](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#queues) na sua aplicação lógica para aceder a um grupo de mensagens. A aplicação lógica, em seguida, pode realizar passos para validar cada mensagem antes de a conclusão ou abandono. Essa abordagem protege contra a perda acidental de mensagens.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Verificar a existência de vários objetos quando é acionada de um acionador do Event Grid

Acionadores de grelha do evento disparando significa simplesmente que "Ocorreu, pelo menos, 1 dessas coisas". Por exemplo, quando o Event Grid aciona uma aplicação lógica numa mensagem que aparece numa fila do Service Bus, a aplicação lógica deve sempre assumir pode haver uma ou mais mensagens disponíveis para processar.

### <a name="region"></a>Região

Aprovisione a gestão de API, Logic Apps e do Service Bus na mesma região para minimizar a latência de rede. Por norma, escolha a região mais próxima dos seus utilizadores.

O grupo de recursos tem também uma região, que especifica onde os metadados de implementação são armazenados, e em que o modelo de implementação é executado a partir de. Coloque o grupo de recursos e os seus recursos na mesma região. Tal pode melhorar a disponibilidade durante a implementação.

## <a name="scalability-considerations"></a>Considerações de escalabilidade

O escalão premium do Azure Service Bus pode aumentar horizontalmente o número de unidades de mensagens para atingir maior escalabilidade. Premium podem ter 1, 2 ou 4 unidades de mensagens. Para obter mais orientações sobre o dimensionamento do Azure Service Bus, consulte [melhores práticas para melhoramentos do desempenho através de mensagens do Service Bus](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability-considerations"></a>Considerações de disponibilidade

No momento da escrita, o contrato de nível de serviço (SLA) para a gestão de API do Azure é de 99,9% para os escalões Basic, Standard e Premium. Configurações de escalão Premium com a implementação de, pelo menos, uma unidade em duas ou mais regiões têm um SLA de 99,95%.

No momento da escrita, o contrato de nível de serviço (SLA) do Azure Logic Apps é de 99,9%.

### <a name="disaster-recovery"></a>Recuperação após desastre

Pondere implementar a recuperação após desastre geográfico no premium do Service Bus para ativar a ativação pós-falha em caso de uma falha grave. Leia mais sobre [recuperação de desastre geográfico de Azure Service Bus](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability-considerations"></a>Considerações sobre a capacidade de gestão

Criar grupos de recursos separados para a produção, desenvolvimento e os ambientes de teste. Este procedimento torna mais fácil gerir as implementações, eliminar as implementações de teste e atribuir direitos de acesso.
Ao atribuir recursos aos grupos de recursos, considere o seguinte:

- Ciclo de vida. Em geral, coloque os recursos com o mesmo ciclo de vida no mesmo grupo de recursos.
- Acesso. Pode usar [controlo de acesso baseado em funções](../role-based-access-control/overview.md) (RBAC) para aplicar políticas de acesso a recursos num grupo.
- Faturação. Pode ver os custos agregados ao grupo de recursos.
- Escalão de preço para a gestão de API – Recomendamos que utilize o escalão de programador para ambientes de desenvolvimento e teste. Para a pré-produção, recomendamos a implementação de uma réplica do ambiente de produção, a execução de testes e, em seguida, encerrar para minimizar os custos.

Para obter mais informações, consulte [grupo de recursos](../azure-resource-manager/resource-group-overview.md) descrição geral.

### <a name="deployment"></a>Implementação

Recomendamos que utilize [modelos Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) para implementar a gestão de API do Azure, o Logic Apps, o Event Grid e o Service Bus. Modelos tornam mais fácil automatizar as implementações através do PowerShell ou a interface de linha de comandos (CLI) do Azure.

Recomendamos que o serviço de colocação de API Management do Azure, qualquer individuais Logic Apps, tópicos do Event Grid e espaços de nomes do Service Bus em seus próprios modelos do Resource Manager separado. Isso permitirá que a armazená-los em sistemas de controle de origem. Estes modelos, em seguida, podem ser implementados em conjunto ou individualmente como parte de um processo de implementação contínua integração/contínua (CI/CD).

### <a name="diagnostics-and-monitoring"></a>Diagnóstico e monitorização

Barramento de serviço, como a gestão de API e o Logic Apps podem ser monitorizadas com o Azure Monitor. Monitor do Azure está ativado por predefinição e irá fornecer informações com base em diferentes métricas configuradas para cada serviço.

## <a name="security-considerations"></a>Considerações de segurança

Proteger o Service Bus com uma assinatura de acesso partilhado. [Autenticação de SAS](../service-bus-messaging/service-bus-sas.md) permite-lhe conceder um acesso de utilizador aos recursos do Service Bus, com direitos específicos. Leia mais sobre [do Service Bus autenticação e autorização](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Além disso, uma fila do Service Bus deve ser exposto como um ponto final HTTP (para permitir o lançamento de novas mensagens), gestão de API deve ser usada para protegê-lo colocando fronting o ponto final. Isso pode, em seguida, ser protegido com certificados ou OAuth conforme apropriado. A maneira mais fácil de fazer isso é usar uma aplicação lógica com um acionador de HTTP de solicitação/resposta como um intermediário.

Grelha de eventos protege a entrega de eventos por meio de um código de validação. Se usar LogicApps para consumir o evento, isso é executado automaticamente. Ver mais detalhes [Event Grid segurança e autenticação](../event-grid/security-authentication.md).

## <a name="next-steps"></a>Passos Seguintes

- [Integração empresarial simples](logic-apps-architectures-simple-enterprise-integration.md)