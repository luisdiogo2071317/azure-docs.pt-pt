---
title: Arquitetura de referência de serviços de integração do Azure
description: Arquitetura de referência que mostra como implementar um padrão de integração empresarial com Logic Apps, API Management, o Service Bus e grelha de eventos
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
ms.openlocfilehash: db3350b3c70f6e6f35949e8423334061849b7b37
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232492"
---
# <a name="enterprise-integration-with-queues-and-events-reference-architecture"></a>Integração de Enterprise com as filas e eventos: arquitetura de referência

## <a name="overview"></a>Descrição geral

Esta arquitetura de referência mostra um conjunto de práticas comprovados para uma aplicação de integração que utiliza serviços de integração do Azure. Esta arquitetura pode servir como esta base de muitos padrões de diferentes aplicações que necessitam de APIs de HTTP, o fluxo de trabalho e orquestração.

*Existem muitas aplicações possíveis de tecnologia de integração, simples ponto a ponto de uma aplicação de um para um barramento de serviço completo de empresa. Esta série de arquitetura define terminar as partes de componente reutilizáveis para a criação de qualquer aplicação de integração – arquitetos de TI devem considerar os componentes que serão necessário para as respetivas aplicações e infraestruturas.*

![Diagrama de arquitetura - integração empresarial com as filas & de eventos](media/logic-apps-architectures-enterprise-integration-with-queues-events/integr_queues_events_arch_diagram.png)

## <a name="architecture"></a>Arquitetura

Esta arquitetura baseia-se mostradas no [integração empresarial simples com](logic-apps-architectures-simple-enterprise-integration.md). Inclui os seguintes componentes:

- Grupo de recursos. Um grupo de recursos é um contentor lógico de recursos do Azure.
- API Management do Azure. Gestão de API do Azure é uma plataforma completamente gerida para publicação, proteger e transformar os APIs de HTTP.
- Portal do Programador de gestão de API do Azure. Cada instância da API Management do Azure inclui um Portal de programador, conceder acesso à documentação, exemplos de código e a capacidade de uma API de teste.
- Aplicações lógicas do Azure. As aplicações lógicas é uma plataforma sem servidor para criação de fluxo de trabalho da empresa e integração.
- Conectores. Os conectores são utilizados pelo Logic Apps para ligar aos serviços comummente utilizados. As Logic Apps já centenas de conectores diferentes, mas também podem ser criadas utilizando um conetor personalizado.
- Service Bus do Azure. O Service Bus fornece mensagens seguras e fiáveis. Mensagens podem ser utilizada para anular couple aplicações entre si e integrar com outros sistemas baseada em mensagens.
- Grelha de eventos do Azure. Grelha de eventos é uma plataforma para a publicar e entrega de eventos da aplicação sem servidor.
- Endereço IP. O serviço de API Management do Azure tem um endereço IP público fixo e um nome de domínio. O nome de domínio é um subdomínio api.net do azure, tais como contoso.azure api.net. As Logic Apps e o Service Bus também tem um endereço IP público; No entanto, esta arquitetura é restringir o acesso para chamar pontos finais de aplicações lógicas para apenas o de API gestão de endereços IP (segurança). Chamadas para o Service Bus são protegidas por uma assinatura de acesso partilhado.
- DNS do Azure. O DNS do Azure é um serviço de alojamento de domínios DNS, fornecer a resolução do nome utilizando a infraestrutura do Microsoft Azure. Ao alojar os seus domínios no Azure, pode gerir os recursos DNS com as mesmas credenciais, APIs, ferramentas e faturação dos seus outros serviços do Azure. Para utilizar um nome de domínio personalizado (tal como contoso.com) crie registos DNS que mapeiam o nome de domínio personalizado para o endereço IP. Para obter mais informações, consulte Configurar um nome de domínio personalizado na API Management do Azure.
- Azure Active Directory (Azure AD). Utilize o Azure AD ou outro fornecedor de identidade para a autenticação. Azure AD fornece autenticação para pontos finais da API de acesso (através da transmissão de um Web Token JSON para a gestão de API a validar) e pode proteger o acesso ao Portal de Programador de gestão de API (apenas camadas Standard e Premium).

Esta arquitetura tem alguns padrões fundamentais para a conclusão da operação:

1. Back-end existente HTTP APIs são publicadas através do Portal de programador da gestão de API, permitindo aos programadores (ou interno para a sua organização, externo ou ambos) para integrar chamadas para estas APIs nas aplicações.
2. APIs compostas criadas com Logic Apps; da orquestração chamadas para sistemas SAAS, serviços do Azure e qualquer APIs publicados API Management. As Logic Apps também são publicadas através do Portal de programador da gestão de API.
3. Aplicações adquirir um token de segurança do OAuth 2.0, necessário para obter acesso a uma API utilizando o Azure Active Directory.
4. Gestão de API do Azure valida o token de segurança e transmite o pedido para o aplicação API/lógica de back-end.
5. Filas do Service Bus são utilizadas para desassociar a atividade das aplicações e picos uniforme em carga. As mensagens que são adicionadas filas por Logic Apps, aplicações de terceiros 3rd, ou (não ilustradas) através da publicação de fila como uma API de HTTP através da API Management.
6. Quando as mensagens são adicionadas a uma fila de barramento de serviço, um evento é acionado. Uma aplicação lógica é acionada por este evento e processa a mensagem.
7. Da mesma forma, os outros serviços do Azure (por exemplo, o Blob Storage, o Hub de eventos) também publicar eventos, à grelha de eventos. Estes acionam Logic Apps para receber o evento e efetuar ações posteriores.

## <a name="recommendations"></a>Recomendações

Os requisitos podem ser diferentes da arquitetura descrita aqui. Utilize as recomendações nesta secção como um ponto de partida.

### <a name="service-bus-tier"></a>Camada de Service Bus

Utilizar o escalão de premium do Service Bus que isto suporta notificações de grelha de eventos atualmente (suporte em todos os escalões é esperado). Consulte [preços do Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="event-grid-pricing"></a>Preços de grelha de eventos

Grelha de evento funciona através de um modelo sem servidor – faturação é calculada com base no número de operações (execução do evento). Consulte [preços da grelha de evento](https://azure.microsoft.com/pricing/details/event-grid/) para obter mais informações. Não existem atualmente não existem considerações sobre a camada grelha de eventos.

### <a name="use-peeklock-when-consuming-service-bus-messages"></a>Utilizar PeekLock ao consumir mensagens do Service Bus

Quando criar Logic Apps para consumir mensagens do Service Bus, utilize [PeekLock](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#queues) na sua aplicação lógica para aceder a um grupo de mensagens em fila. A aplicação lógica, em seguida, pode efetuar os passos para validar cada mensagem antes de concluir ou abandonar. Esta abordagem protege contra a perda acidental de mensagem.

### <a name="check-for-multiple-objects-when-an-event-grid-trigger-fires"></a>Verifique a existência de vários objetos quando um acionador de grelha de evento é acionado

Acionadores de grelha de evento que despoletou significa simplesmente ocorrida ", pelo menos, 1 de tudo". Por exemplo, quando a grelha de evento aciona uma aplicação lógica de uma mensagem numa fila do Service Bus, a aplicação lógica deve sempre partem do princípio poderá haver uma ou mais mensagens disponíveis para processar.

### <a name="region"></a>Região

Aprovisione a API Management, as Logic Apps e o Service Bus na mesma região para minimizar a latência de rede. Por norma, escolha a região mais próxima dos seus utilizadores.

O grupo de recursos tem também uma região que especifica a armazenar metadados de implementação, e onde o modelo de implementação é executado de. Coloque o grupo de recursos e os seus recursos na mesma região. Tal pode melhorar a disponibilidade durante a implementação.

## <a name="scalability-considerations"></a>Considerações de escalabilidade

Camada de premium do Service Bus do Azure pode aumentar horizontalmente o número de unidades de mensagens para alcançar a escalabilidade superior. Premium pode ter 1, 2 ou 4 unidades de mensagens. Para obter orientações adicionais sobre como aumentar o Service Bus do Azure, consulte [melhores práticas para melhorias de desempenho através de mensagens do Service Bus](../service-bus-messaging/service-bus-performance-improvements.md).

## <a name="availability-considerations"></a>Considerações de disponibilidade

No momento da escrita, o contrato de nível de serviço (SLA) de API Management do Azure é 99,9% dos escalões básico, Standard e Premium. Configurações de escalão Premium com a implementação pelo menos uma unidade em dois ou mais regiões têm um SLA de 99,95%.

No momento da escrita, o contrato de nível de serviço (SLA) para o Azure Logic Apps é 99,9%.

### <a name="disaster-recovery"></a>Recuperação após desastre

Pondere implementar a recuperação de desastres Georreplicação nas premium do Service Bus para ativar a ativação pós-falha em caso de uma falha grave. Leia mais sobre [recuperação Georreplicação-desastre do Service Bus do Azure](../service-bus-messaging/service-bus-geo-dr.md).

## <a name="manageability-considerations"></a>Considerações sobre a capacidade de gestão

Criar grupos de recursos separado para produção, desenvolvimento e ambientes de teste. Este procedimento torna mais fácil gerir as implementações, eliminar as implementações de teste e atribuir direitos de acesso.
Ao atribuir recursos aos grupos de recursos, considere o seguinte:

- Ciclo de vida. Em geral, coloque os recursos com o mesmo ciclo de vida no mesmo grupo de recursos.
- Acesso. Pode utilizar [controlo de acesso baseado em funções](../role-based-access-control/overview.md) (RBAC) para aplicar políticas de acesso aos recursos num grupo.
- Faturação. Pode ver os custos agregados ao grupo de recursos.
- Escalão de preço para gestão de API – Recomendamos que utilize a camada de programador para ambientes de desenvolvimento e teste. Para a pré-produção, recomendamos a implementação de uma réplica do seu ambiente de produção, executar testes e, em seguida, a ser encerrado para minimizar os custos.

Para obter mais informações, consulte [grupo de recursos](../azure-resource-manager/resource-group-overview.md) descrição geral.

### <a name="deployment"></a>Implementação

Recomendamos que utilize [modelos Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) para implementar a API Management do Azure, as Logic Apps, grelha de evento e o Service Bus. Modelos de tornam mais fácil automatizar as implementações através do PowerShell ou a interface de linha de comandos do Azure (CLI).

Recomendamos colocando a API Management do Azure, qualquer individuais Logic Apps, tópicos de grelha de eventos e espaços de nomes de barramento de serviço na suas própria separadas modelos do Resource Manager. Isto permitirá armazene-os em sistemas de controlo de origem para. Estes modelos podem ser implementados em conjunto ou individualmente como parte de um processo de implementação contínua integração/contínua (CI/CD).

### <a name="diagnostics-and-monitoring"></a>Diagnóstico e monitorização

Barramento de serviço, como a API Management e Logic Apps podem ser monitorizadas com o Monitor do Azure. Monitor do Azure está ativada por predefinição e irá fornecer informações com base nas métricas diferentes configuradas para cada serviço.

## <a name="security-considerations"></a>Considerações de segurança

Proteger os utilizando uma assinatura de acesso partilhado do Service Bus. [Autenticação de SAS](../service-bus-messaging/service-bus-sas.md) permite-lhe conceder acesso de utilizadores aos recursos do Service Bus, com direitos específicos. Leia mais sobre [barramento de serviço de autenticação e autorização](../service-bus-messaging/service-bus-authentication-and-authorization.md).

Além disso, deve uma fila de barramento de serviço tem de ser exposta como um ponto final de HTTP (para permitir a publicação de novas mensagens), gestão de API devem ser utilizada para proteger a por fronting o ponto final. Isto pode, em seguida, ser protegido com certificados ou OAuth conforme apropriado. A forma mais fácil para efetuar este procedimento está a utilizar uma aplicação lógica com um acionador de HTTP de pedido/resposta como um intermediário.

Grelha de evento protege a entrega de eventos através de um código de validação. Se utilizar LogicApps para consumir o evento, este procedimento é efetuado automaticamente. Ver mais detalhes [grelha de eventos de segurança e autenticação](../event-grid/security-authentication.md).

## <a name="next-steps"></a>Passos Seguintes

* [Integração do Enterprise simples](logic-apps-architectures-simple-enterprise-integration.md)
