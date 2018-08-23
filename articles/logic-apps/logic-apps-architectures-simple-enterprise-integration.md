---
title: Azure arquitetura de referência de integração do serviços de integração empresarial simples
description: Descreve a arquitetura de referência mostra como implementar um padrão de integração empresarial simples com o Azure Logic Apps e gestão de API do Azure.
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
ms.openlocfilehash: f73a9e59c0add664128b506172182afe566ca670
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444515"
---
# <a name="reference-architecture-simple-enterprise-integration"></a>Arquitetura de referência: integração empresarial simples

A arquitetura de referência seguinte mostra um conjunto de práticas comprovadas que pode aplicar a uma aplicação de integração que utiliza serviços de integração do Azure. A arquitetura pode servir como base para muitos padrões de aplicação diferente que necessitam de HTTP APIs, fluxo de trabalho e a orquestração.

![Diagrama da arquitetura - integração empresarial simples](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

*Há muitos aplicativos possíveis para a tecnologia de integração. Eles variam de um aplicativo de ponto a ponto simples para uma empresa de completo de aplicação do Azure Service Bus. A série de arquitetura descreve os componentes reutilizáveis que podem ser aplicadas para criar um aplicativo de integração genérico. Arquitetos devem considerar quais componentes que precisam implementar para a sua aplicação e infraestrutura.*

## <a name="architecture"></a>Arquitetura

A arquitetura é composta pelos seguintes componentes:

- **Grupo de recursos**. Um [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) é um contentor lógico de recursos do Azure.
- **Gestão de API do Azure**. [Gestão de API](https://docs.microsoft.com/azure/api-management/) é uma plataforma totalmente gerida que é utilizada para publicar, proteger e transforme as APIs de HTTP.
- **Portal de programador da gestão de API do Azure**. Cada instância da gestão de API do Azure é fornecido com o acesso para o [portal do programador](https://docs.microsoft.com/azure/api-management/api-management-customize-styles). O portal do programador da gestão de API fornece acesso a exemplos de código e documentação. Pode testar as APIs no portal do programador.
- **Azure Logic Apps**. [O Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) é uma plataforma sem servidor que é utilizada para criar o fluxo de trabalho empresariais e de integração.
- **Conectores**. O Logic Apps utiliza [conectores](https://docs.microsoft.com/azure/connectors/apis-list) ligar ao normalmente utilizou os serviços. O Logic Apps já tenha centenas de conectores diferentes, mas também pode criar um conector personalizado.
- **Endereço IP**. O serviço de gestão de API do Azure tem uma pública fixa [endereço IP](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) e um nome de domínio. O nome de domínio predefinido é um subdomínio de azure-api.net, por exemplo, contoso. Azure-api.net, mas [domínios personalizados](https://docs.microsoft.com/azure/api-management/configure-custom-domain) também podem ser configuradas. Logic Apps e do Service Bus também tem um endereço IP público. No entanto, nesta arquitetura, podemos restringir o acesso para chamar pontos finais de Logic Apps para o endereço IP de gestão de API (para segurança). As chamadas para o Service Bus são protegidas por uma assinatura de acesso partilhado (SAS).
- **DNS do Azure**. [O DNS do Azure](https://docs.microsoft.com/azure/dns/) é um serviço de alojamento para domínios DNS. O DNS do Azure oferece resolução de nomes através da infraestrutura do Microsoft Azure. Ao alojar os seus domínios no Azure, pode gerir os registos DNS ao utilizar as mesmas credenciais, APIs, ferramentas e de faturação que utiliza para os outros serviços do Azure. Para utilizar um nome de domínio personalizado, como contoso.com, crie registos DNS que mapeiam o nome de domínio personalizado para o endereço IP. Para obter mais informações, consulte [configurar um nome de domínio personalizado na gestão de API](https://docs.microsoft.com/en-us/azure/api-management/configure-custom-domain).
- **Azure Active Directory (Azure AD)**. Uso [do Azure AD](https://docs.microsoft.com/azure/active-directory/) ou outro fornecedor de identidade para a autenticação. O Azure AD fornece autenticação para aceder a pontos finais da API, passando uma [JSON Web Token para a gestão de API](https://docs.microsoft.com/azure/api-management/policies/authorize-request-based-on-jwt-claims) para validar. O Azure AD pode proteger o acesso ao portal do Programador de gestão de API (apenas escalões Standard e Premium).

A arquitetura tem alguns padrões que são fundamentais para a sua operação:

- APIs compostas são criadas ao utilizar o logic apps. Orquestrar a chamadas para o software como um sistemas de serviço (SaaS), aos serviços do Azure e a quaisquer APIs que são publicados para a gestão de API. [Aplicações lógicas também são publicadas](https://docs.microsoft.com/azure/api-management/import-logic-app-as-api) através do portal do programador da gestão de API.
- Aplicações utilizam o Azure AD para [adquirir um token de segurança de OAuth 2.0](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) que é necessário para obter acesso a uma API.
- Gestão de API do Azure [valida o token de segurança](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) e, em seguida, transmite o pedido para a aplicação de API ou lógica de back-end.

## <a name="recommendations"></a>Recomendações

Seus requisitos específicos poderão diferir da arquitetura genérica descrita neste artigo. Utilize as recomendações nesta secção como um ponto de partida.

### <a name="azure-api-management-tier"></a>Escalão de gestão de API do Azure

Utilize as camadas de gestão de API básica, Standard ou Premium. Os escalões de oferecem um contrato de nível de serviço de produção (SLA) e suportam de escalamento horizontal na região do Azure (o número de unidades varia consoante o escalão). O escalão Premium também oferece suporte a aumentar horizontalmente em várias regiões do Azure. O escalão que escolhe o nível de débito necessário e seu conjunto de funcionalidades de base. Para obter mais informações, consulte [preços da API Management](https://azure.microsoft.com/pricing/details/api-management/).

É-lhe cobrada todas as instâncias de gestão de API que são executados. Se tem aumentado verticalmente e não precisar desse nível de desempenho o tempo todo, considere a tirar partido da gestão de API de faturação por hora e reduzir verticalmente.

### <a name="logic-apps-pricing"></a>Preços de aplicações lógicas

O Logic Apps utiliza um [sem servidor](logic-apps-serverless-overview.md) modelo. A faturação é calculada com base na execução de ações e conectores. Para obter mais informações, consulte [preços de aplicações lógicas](https://azure.microsoft.com/pricing/details/logic-apps/). Atualmente, não existem não existem considerações de camada para o Logic Apps.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Logic Apps para chamadas assíncronas de API

Aplicações lógicas também funcionam melhor em cenários que não necessitam de latência baixa. Por exemplo, é melhor para assíncrona ou chamadas de API de execução longa semiestruturados. Se a latência baixa é necessária (por exemplo, uma chamada que bloqueia uma interface do usuário), é recomendável implementar essa API ou a operação com uma tecnologia diferente. Por exemplo, utilize as funções do Azure ou uma API Web que implantar usando o App Service do Azure. Ainda é recomendável que de front-os consumidores de API para API utilizando a gestão de API.

### <a name="region"></a>Região

Gestão de API de aprovisionamento e o Logic Apps na mesma região para minimizar a latência de rede. Em geral, selecione a região mais próxima dos seus utilizadores.

O grupo de recursos tem também uma região. A região especifica onde os metadados de implementação são armazenados e onde o modelo de implementação é executado de. Coloca o grupo de recursos e os respetivos recursos na mesma região para melhorar a disponibilidade durante a implementação.

## <a name="scalability"></a>Escalabilidade

Devem adicionar os administradores de gestão de API [políticas de colocação em cache](../api-management/api-management-howto-cache.md) quando apropriado para aumentar a escalabilidade do serviço. Colocação em cache também ajuda a reduzir a carga nos serviços de back-end.

Numa região do Azure para oferecer uma capacidade maior, podem ser ampliadas escalões básicas de gestão de API do Azure, Standard e Premium. Os administradores podem utilizar o **métrica de capacidade** opção a **métricas** menu para analisar a utilização do seu serviço e, em seguida, aumentar ou reduzir verticalmente, conforme adequado.

Recomendações para dimensionar um serviço de gestão de API:

- Necessidades de dimensionamento para levar em conta os padrões de tráfego. Os clientes com os padrões de tráfego mais volátil têm maior necessidade de maior capacidade.
- Consistente com capacidade superior a 66% pode indicar a necessidade de aumentar verticalmente.
- Capacidade consistente inferior a 20% pode indicar uma oportunidade para reduzir verticalmente.
- É sempre recomendado para teste de carga seu serviço de gestão de API com uma carga representativa antes de ativar a carga na produção.

Serviços de escalão Premium podem ser aumentados horizontalmente em várias regiões do Azure. Os clientes que implementem ao dimensionar serviços em várias regiões do Azure obtêm um SLA mais elevado (99,95% em comparação com 99,9%) e podem aprovisionar serviços aos usuários em várias regiões.

O Logic Apps sem servidor modelo significa que os administradores não precisa de planear a escalabilidade do serviço. O serviço pode ser dimensionada automaticamente para satisfazer a procura.

## <a name="availability"></a>Disponibilidade

Atualmente, o SLA para a gestão de API do Azure é de 99,9% para os escalões Basic, Standard e Premium. Configurações de escalão Premium com a implementação de, pelo menos, uma unidade em duas ou mais regiões têm um SLA de 99,95%.

Atualmente, o SLA para o Azure Logic Apps é de 99,9%.

### <a name="backups"></a>Cópias de segurança

A configuração de gestão de API do Azure deve estar [backup regularmente](../api-management/api-management-howto-disaster-recovery-backup-restore.md) (com base no regularidade de alteração). Ficheiros de cópia de segurança devem ser armazenados numa localização ou região do Azure que é diferente de onde reside o serviço. Os clientes podem, em seguida, escolher uma das duas opções para a sua estratégia de recuperação após desastre:

- Num evento de recuperação após desastre, uma nova instância de gestão de API está aprovisionada, é restaurar a cópia de segurança para a nova instância e registos DNS são repointed.
- Os clientes manter uma cópia passiva do seu serviço noutra região do Azure (incorrer em custos adicionais). As cópias de segurança regularmente são restauradas para a cópia. Num evento de recuperação após desastre, apenas os registos DNS tem de ser repointed para restaurar o serviço.

Porque a aplicações lógicas podem voltar a criar rapidamente e sem servidor, eles são uma cópia de segurança ao guardar uma cópia do modelo do Azure Resource Manager associados. Modelos podem ser guardados para controlo de origem e que possam ser integradas com o processo de implementação (CI/CD) de integração contínua/contínua de um cliente.

Se uma aplicação lógica que foi publicada através da gestão de API for movido para outro datacenter, atualize a localização da aplicação. Para atualizar a localização da aplicação, utilize um script de PowerShell básico para atualizar o **back-end** propriedade da API.

## <a name="manageability"></a>Capacidade de gestão

Criar grupos de recursos separados para a produção, desenvolvimento e os ambientes de teste. Utilizar grupos de recursos separados torna mais fácil gerir as implementações, eliminar as implementações de teste e atribuir direitos de acesso.

Ao atribuir recursos a grupos de recursos, considere os seguintes fatores:

- **Ciclo de vida**. Em geral, coloque os recursos que têm o mesmo ciclo de vida no mesmo grupo de recursos.
- **Acesso**. Pode usar [controlo de acesso baseado em funções](../role-based-access-control/overview.md) (RBAC) para aplicar políticas de acesso a recursos num grupo.
- **Faturação**. Pode ver os custos de rollup para o grupo de recursos.
- **Escalão de preço para a gestão de API**. Recomendamos utilizar o escalão de programador para o desenvolvimento e os ambientes de teste. Para pré-produção, recomendamos a implementação de uma réplica do ambiente de produção, a execução de testes e, em seguida, encerrar para minimizar os custos.

Para obter mais informações, veja [Descrição geral do Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="deployment"></a>Implementação

Recomendamos que utilize [modelos Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) para implementar a gestão de API e o Logic Apps. Modelos tornam mais fácil automatizar as implementações através do PowerShell ou a CLI do Azure.

Recomendamos que o serviço de colocação de API Management do Azure e todas as aplicações lógicas individuais em seus próprios, modelos do Resource Manager separado. Ao utilizar modelos separados, pode armazenar os recursos em sistemas de controle de origem. Também pode implementar os recursos em conjunto ou individualmente como parte de um processo de implementação de CI/CD.

### <a name="versions"></a>Versões

Sempre que fizer uma configuração alterar para uma aplicação lógica (ou implementar uma atualização através de um modelo do Resource Manager), uma cópia do que a versão é mantida para sua comodidade (todas as versões que têm um histórico de execuções são mantidas). Pode utilizar estas versões para controlar alterações históricas e para promover uma versão seja a configuração atual da aplicação lógica. Por exemplo, pode efetivamente reverter uma aplicação lógica.

Gestão de API tem dois distintos (mas complementares) [conceitos de controlo de versões](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/):

- Versões que são utilizados para fornecer os consumidores de API com uma opção de API, elas poderão consumir com base nas suas necessidades (por exemplo, v1, v2 ou beta, produção).
- Revisões que permitem aos administradores de API com segurança de fazer alterações a uma API e, em seguida, implementar as alterações aos utilizadores com comentários opcionais.

No contexto da implementação, é uma boa idéia considerar revisões de gestão de API como uma forma de efetuar alterações de forma segura, manter um histórico de alterações e fazer com que os consumidores de APIS com reconhecimento de uma destas alterações. Uma revisão pode ser criada num ambiente de desenvolvimento e ser implementada, entre outros ambientes, utilizando modelos do Resource Manager.

Apesar de poder utilizar revisões para testar uma API antes de realmente "current" e acessíveis aos utilizadores, não é recomendado utilizar este mecanismo para a carga ou teste de integração. Em alternativa, utilize o teste separada ou ambientes de pré-produção.

### <a name="configuration"></a>Configuração

Nunca verificação em palavras-passe, chaves de acesso ou cadeias de ligação ao controlo de origem. Se estes valores são necessários, utilize a técnica apropriada para implantar e proteger esses valores. 

No Logic Apps, quaisquer valores de confidenciais necessários numa aplicação lógica (que não é possível criar na forma de uma ligação) devem ser armazenados no Azure Key Vault e chamados a partir de um modelo do Resource Manager. Também sugerimos que utilize ficheiros de parâmetros e parâmetros de modelo de implementação para cada ambiente. Para obter mais informações, consulte [protegendo parâmetros e de entradas num fluxo de trabalho](logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

Na gestão de API, segredos são geridos com objetos chamados *valores nomeados* ou *propriedades*. Os objetos de armazenam em segurança os valores que podem ser acessados nas políticas de gestão de API. Para obter mais informações, consulte [gerir segredos na gestão de API](../api-management/api-management-howto-properties.md).

### <a name="diagnostics-and-monitoring"></a>Diagnóstico e monitorização

[Gestão de API](../api-management/api-management-howto-use-azure-monitor.md) e [Logic Apps](logic-apps-monitor-your-logic-apps.md) oferecem suporte a monitorização operacional por meio de [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). O Azure Monitor proporciona informações com base nas métricas que estão configuradas para cada serviço. Monitor do Azure está ativado por predefinição.

Estas opções também estão disponíveis para cada serviço:

- Registos de aplicações lógicas podem ser enviados para [do Azure Log Analytics](logic-apps-monitor-your-logic-apps-oms.md) para mais aprofundado de análise e vistas.
- Gestão de API suporta a configuração do Azure Application Insights para monitorização de DevOps.
- Gestão de API suporta a [modelo de solução do Power BI para análise de API personalizada](http://aka.ms/apimpbi). Os clientes podem utilizar o modelo de solução para criar sua própria solução de análise personalizada. Relatórios estão disponíveis no Power BI para utilizadores empresariais.

## <a name="security"></a>Segurança

Esta secção lista as considerações de segurança que são específicas para os serviços do Azure que são descritos neste artigo, e que é implementado na arquitetura, tal como descrito. Não é uma lista completa das melhores práticas de segurança.

- Utilize o controlo de acesso baseado em funções (RBAC) para garantir níveis apropriados de acesso dos utilizadores.
- Proteger pontos finais públicos do API na gestão de API com OAuth/OpenID Connect. Para proteger pontos finais da API públicos, configure um fornecedor de identidade e adicionar uma política de validação do JSON Web Token (JWT).
- Ligar a serviços de back-end da gestão de API, através da utilização de certificados mútuos.
- Proteja as aplicações de lógica baseada no acionador HTTP através da criação de uma lista de permissões de endereço IP que aponta para o endereço IP de gestão de API. Impede que um endereço IP na lista de permissões a chamar a aplicação lógica a partir da internet pública sem entrar primeiro através da gestão de API.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [integração empresarial com filas e os eventos](logic-apps-architectures-enterprise-integration-with-queues-events.md).
