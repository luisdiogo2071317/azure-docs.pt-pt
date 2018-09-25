---
title: Padrão de arquitetura de integração do enterprise simples - serviços de integração do Azure
description: Esta referência de arquitetura mostra como pode implementar um padrão de integração empresarial simples com o Azure Logic Apps e gestão de API do Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: mattfarm
ms.author: mattfarm
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/15/2018
ms.openlocfilehash: 5ed6fa9f514bae3ea651edba6702714e2680091f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46955950"
---
# <a name="simple-enterprise-integration-architecture"></a>Arquitetura de integração empresarial simples

Este artigo descreve uma arquitetura de integração do enterprise que usa práticas comprovadas que pode aplicar a uma aplicação de integração ao utilizar os serviços de integração do Azure. Pode utilizar esta arquitetura como base para muitos padrões de aplicação diferente que necessitam de HTTP APIs, fluxo de trabalho e a orquestração.

![Diagrama da arquitetura - integração empresarial simples](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

Esta série descreve os componentes reutilizáveis que podem ser aplicadas para criar um aplicativo de integração genérico. Porque a tecnologia de integração tem muitos aplicativos possíveis, desde aplicações ponto a ponto simples para aplicações empresariais completa do Service bus do Azure, considere os componentes que precisa implementar para as aplicações e infraestrutura.

## <a name="architecture-components"></a>Componentes de arquitetura

Esta arquitetura de integração do enterprise inclui estes componentes:

- **Grupo de recursos**: A [grupo de recursos](../azure-resource-manager/resource-group-overview.md) é um contentor lógico de recursos do Azure.

- **Gestão de API do Azure**: A [gestão de API](https://docs.microsoft.com/azure/api-management/) service é uma plataforma totalmente gerida para publicação, proteger e transformar HTTP APIs.

- **Portal de programador da gestão de API do Azure**: cada instância da gestão de API do Azure fornece acesso para o [portal do programador](../api-management/api-management-customize-styles.md). Esse portal fornece acesso a exemplos de código e documentação. Também pode testar as APIs no portal do programador.

- **O Azure Logic Apps**: [Logic Apps](../logic-apps/logic-apps-overview.md) é uma plataforma sem servidor para a criação de fluxos de trabalho empresariais e integrações.

- **Conectores**: utiliza o Logic Apps [conectores](../connectors/apis-list.md) para ligar ao frequentemente utilizadas serviços. O Logic Apps oferece centenas de conectores, mas também pode criar um conector personalizado.

- **Endereço IP**: o serviço de gestão de API do Azure a tem uma pública fixa [endereço IP](../virtual-network/virtual-network-ip-addresses-overview-arm.md) e um nome de domínio. O nome de domínio predefinido é um subdomínio de azure-api.net, por exemplo, contoso. Azure-api.net, mas também pode configurar [domínios personalizados](../api-management/configure-custom-domain.md). Logic Apps e do Service Bus também tem um endereço IP público. No entanto, para segurança, esta arquitetura restringe o acesso para chamar pontos finais de Logic Apps para o endereço IP de gestão de API. As chamadas para o Service Bus são protegidas por uma assinatura de acesso partilhado (SAS).

- **O DNS do Azure**: [DNS do Azure](https://docs.microsoft.com/azure/dns/) é um serviço de alojamento para domínios DNS. O DNS do Azure oferece resolução de nomes através da infraestrutura do Microsoft Azure. Ao alojar os seus domínios no Azure, pode gerir os registos DNS ao utilizar as mesmas credenciais, APIs, ferramentas e de faturação que utiliza para os outros serviços do Azure. Para utilizar um nome de domínio personalizado, como contoso.com, crie registos DNS que mapeiam o nome de domínio personalizado para o endereço IP. Para obter mais informações, consulte [configurar um nome de domínio personalizado na gestão de API](../api-management/configure-custom-domain.md).

- **Azure Active Directory (Azure AD)**: pode utilizar [do Azure AD](https://docs.microsoft.com/azure/active-directory/) ou outro fornecedor de identidade para a autenticação. O Azure AD fornece autenticação para aceder a pontos finais da API, passando uma [JSON Web Token para a gestão de API](../api-management/policies/authorize-request-based-on-jwt-claims.md) para validar. Para os escalões Standard e Premium, do Azure AD pode proteger o acesso ao portal do programador da gestão de API.

## <a name="patterns"></a>Padrões 

Esta arquitetura utiliza alguns padrões que são fundamentais para operação:

* APIs compostas baseiam-se com o logic apps, o que orquestrar chamadas para o software como um sistemas de serviço (SaaS), serviços do Azure e qualquer APIs que são publicados para a gestão de API. Aplicações lógicas são também [publicados através do portal do programador da gestão de API](../api-management/import-logic-app-as-api.md).

* Aplicações utilizam o Azure AD para [obter um token de segurança de OAuth 2.0](../api-management/api-management-howto-protect-backend-with-aad.md) que é necessário para obter acesso a uma API.

* Gestão de API do Azure [valida o token de segurança](../api-management/api-management-howto-protect-backend-with-aad.md) e, em seguida, transmite o pedido para a aplicação de API ou lógica de back-end.

## <a name="recommendations"></a>Recomendações

Seus requisitos específicos poderão diferir da arquitetura do genérica descrita neste artigo. Utilize as recomendações nesta secção como um ponto de partida.

### <a name="azure-api-management-tier"></a>Escalão de gestão de API do Azure

Utilize as camadas de gestão de API básica, Standard ou Premium. Estas camadas de oferecem um contrato de nível de serviço de produção (SLA) e suportam o aumento horizontal na região do Azure. O número de unidades varia consoante o escalão. O escalão Premium também suporta o aumento horizontal em várias regiões do Azure. Escolha o escalão de com base no seu conjunto de recursos e o nível de taxa de transferência necessária. Para obter mais informações, consulte [preços da API Management](https://azure.microsoft.com/pricing/details/api-management/).

É-lhe cobrada todas as instâncias de gestão de API que são executados. Se tem aumentado verticalmente e não precisar desse nível de desempenho o tempo todo, considere a tirar partido da gestão de API de faturação por hora e reduzir verticalmente.

### <a name="logic-apps-pricing"></a>Preços de aplicações lógicas

O Logic Apps utiliza um [sem servidor](../logic-apps/logic-apps-serverless-overview.md) modelo. A faturação é calculada com base na execução de ações e conectores. Para obter mais informações, consulte [preços de aplicações lógicas](https://azure.microsoft.com/pricing/details/logic-apps/). Atualmente, não existem não existem considerações de camada para o Logic Apps.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Logic Apps para chamadas assíncronas de API

Aplicações lógicas também funcionam melhor em cenários que não necessitam de latência baixa. Por exemplo, aplicações lógicas funciona melhor para assíncrona ou chamadas de API de execução longa semiestruturados. Se for necessária a baixa latência, por exemplo, uma chamada que bloqueia uma interface do usuário, implementar sua API ou a operação com uma tecnologia diferente. Por exemplo, utilize as funções do Azure ou uma API Web que implantar usando o App Service do Azure. Utilize a gestão de API para a API para os consumidores de API do front.

### <a name="region"></a>Região

Para minimizar a latência de rede, escolha a mesma região para a gestão de API, o Logic Apps e o Service Bus. Em geral, selecione a região mais próxima dos seus utilizadores.

O grupo de recursos tem também uma região. Esta região especifica onde pretende armazenar os metadados de implementação e a localização para executar o modelo de implementação. Para melhorar a disponibilidade durante a implementação, coloque o grupo de recursos e os recursos na mesma região.

## <a name="scalability"></a>Escalabilidade

Para aumentar a escalabilidade ao administrar um serviço de gestão de API, adicione [políticas de colocação em cache](../api-management/api-management-howto-cache.md) quando apropriado. Colocação em cache também ajuda a reduzir a carga nos serviços de back-end.

Para oferecer uma capacidade maior, pode aumentar horizontalmente camadas de gestão de API do Azure básico, Standard e Premium na região do Azure. Para analisar a utilização para o seu serviço, no **métricas** menu, selecione a **métricas de capacidade** opção e, em seguida, aumentar ou reduzir verticalmente, conforme adequado.

Recomendações para dimensionar um serviço de gestão de API:

- Quando o dimensionamento, considere padrões de tráfego. Os clientes com os padrões de tráfego mais volátil precisam de mais capacidade.

- Capacidade consistente que é superior a 66% pode indicar a necessidade de aumentar verticalmente.

- Capacidade consistente que está em 20% pode indicar uma oportunidade para reduzir verticalmente.

- Antes de ativar a carga na produção, sempre teste de carga seu serviço de gestão de API com uma carga representativa.

Pode dimensionar os serviços de escalão Premium em várias regiões do Azure. Se implementar o dimensionamento de serviços em várias regiões do Azure, pode obter um SLA mais elevado (99,95% em comparação com 99,9%) e serviços de aprovisionamento aos usuários em várias regiões.

O Logic Apps sem servidor modelo significa que os administradores não precisa de planear a escalabilidade do serviço. O serviço pode ser dimensionada automaticamente para satisfazer a procura.

## <a name="availability"></a>Disponibilidade

* Para os escalões Basic, Standard e Premium, o contrato de nível de serviço (SLA) para a gestão de API do Azure está atualmente 99,9%. Para configurações de escalão premium com uma implementação que tem, pelo menos, uma unidade em duas ou mais regiões, o SLA é de 99,95%.

* O SLA para o Azure Logic Apps atualmente é de 99,9%.

### <a name="backups"></a>Cópias de segurança

Com base em regularidade de alteração, [fazer regularmente cópias de segurança](../api-management/api-management-howto-disaster-recovery-backup-restore.md) sua configuração de gestão de API do Azure. Store os ficheiros de cópia de segurança de uma localização ou a região do Azure que é diferente de onde reside o seu serviço. Em seguida, pode escolher qualquer uma das opções como estratégia de recuperação de desastres:

* Num evento de recuperação após desastre, Aprovisiona uma nova instância de gestão de API, em seguida, restaurar a cópia de segurança para a nova instância e reencaminhar os registos DNS.

* Mantenha uma cópia passiva do seu serviço noutra região do Azure, que incorre em custos adicionais. Restaure regularmente cópias de segurança para a cópia. Para restaurar o serviço durante um evento de recuperação após desastre, só precisa de reencaminhar os registos DNS.

Uma vez que rapidamente pode recriar logic apps, o que são sem servidor, o backup ao guardar uma cópia do modelo do Azure Resource Manager associados. Pode salvar os modelos no controle de origem e, pode integrar modelos com o seu processo de implementação (CI/CD) integração contínua/contínua.

Se publicou uma aplicação de lógica através da gestão de API do Azure, e essa aplicação lógica se move para outro datacenter, atualize a localização da aplicação. É possível atualizar a sua API **back-end** propriedade utilizando um script de PowerShell básico.

## <a name="manageability"></a>Capacidade de gestão

Criar grupos de recursos separados para a produção, desenvolvimento e os ambientes de teste. Grupos de recursos separados facilitam a gerir implementações, eliminar as implementações de teste e atribuir direitos de acesso.

Ao atribuir recursos a grupos de recursos, considere estes fatores:

* **Ciclo de vida**: em geral, coloque os recursos que têm o mesmo ciclo de vida no mesmo grupo de recursos.

* **Acesso**: para aplicar políticas de acesso a recursos num grupo, pode usar [o controlo de acesso baseado em funções (RBAC)](../role-based-access-control/overview.md).

* **Faturação**: pode ver os custos de rollup para o grupo de recursos.

* **Escalão de preço para a gestão de API**: utilizar o escalão de programador para os seus ambientes de desenvolvimento e teste. Para minimizar os custos durante pré-produção, implementação de uma réplica do ambiente de produção, em seguida, executar seus testes e, em seguida, encerrado.

Para obter mais informações, veja [Descrição geral do Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="deployment"></a>Implementação

* Para implementar a gestão de API e o Logic Apps, utilize o [modelos Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md). Modelos facilitam Implantações de automatização com o PowerShell ou a CLI do Azure.

* Colocar a gestão de API e todas as aplicações lógicas individuais em seus próprios modelos do Resource Manager separado. Ao utilizar modelos separados, pode armazenar os recursos em sistemas de controle de origem. Em seguida, pode implementar estes modelos em conjunto ou individualmente como parte de um processo de implementação (CI/CD) integração contínua/contínua.

### <a name="versions"></a>Versões

Sempre que alterar a configuração de uma aplicação lógica ou implementar uma atualização através de um modelo do Resource Manager, o Azure mantém uma cópia do que a versão para sua comodidade e mantenha todas as versões que têm um histórico de execuções. Pode utilizar estas versões para o controle de alterações históricas ou de o promover uma versão que a configuração atual da aplicação lógica. Por exemplo, pode efetivamente reverter uma aplicação lógica.

Gestão de API do Azure tem estes distintas, mas complementares [conceitos de controlo de versões](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/):

* Versões que fornecem os consumidores de API, a capacidade de escolher uma versão de API com base nas suas necessidades, por exemplo, v1, v2, beta ou de produção

* Revisões que permitem que os administradores de API com segurança fazer alterações numa API e, em seguida, implementar essas alterações para os utilizadores com comentários opcionais

Para a implementação, considere revisões de gestão de API, como uma forma de faça alterações seguras, manter um histórico de alterações e comunicar essas alterações aos consumidores da sua API. Pode fazer uma revisão num ambiente de desenvolvimento e implementar essa alteração em outros ambientes com modelos do Resource Manager.

Apesar de poder utilizar revisões para uma API de teste antes de fazer essas alterações "current" e acessíveis aos utilizadores, este método não é recomendado para a carga ou teste de integração. Em alternativa, utilize o teste separada ou ambientes de pré-produção.

### <a name="configuration-and-sensitive-information"></a>Configuração e informações confidenciais

Nunca selecione palavras-passe, chaves de acesso ou cadeias de ligação no controlo de origem. Se estes valores são necessários, proteger e implementar estes valores com as técnicas apropriadas. 

No Logic Apps, se uma aplicação lógica requer os valores de confidenciais que não é possível criar dentro de uma ligação, armazenar esses valores no Azure Key Vault e referenciá-los a partir de um modelo do Resource Manager. Utilize ficheiros de parâmetros e parâmetros de modelo de implementação para cada ambiente. Para obter mais informações, consulte [protegendo parâmetros e de entradas num fluxo de trabalho](../logic-apps/logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

Gestão de API gere segredos com objetos de chamada *valores nomeados* ou *propriedades*. Esses objetos armazenam em segurança os valores que pode aceder através de políticas de gestão de API. Para obter mais informações, consulte [gerir segredos na gestão de API](../api-management/api-management-howto-properties.md).

## <a name="diagnostics-and-monitoring"></a>Diagnóstico e monitorização

Pode usar [do Azure Monitor](../azure-monitor/overview.md) para a monitorização operacional em ambos [gestão de API](../api-management/api-management-howto-use-azure-monitor.md) e [Logic Apps](../logic-apps/logic-apps-monitor-your-logic-apps.md). O Azure Monitor fornece informações com base em métricas configuradas para cada serviço e está ativado por predefinição.

Cada serviço também tem estas opções:

* Para obter mais aprofundadas análises e vistas, pode enviar registos de aplicações lógicas para [do Azure Log Analytics](logic-apps-monitor-your-logic-apps-oms.md).

* Para a monitorização de DevOps, pode configurar o Azure Application Insights para a gestão de API.

* Gestão de API suporta a [modelo de solução do Power BI para análise de API personalizada](http://aka.ms/apimpbi). Pode utilizar este modelo de solução para criar sua própria solução de análise. Para usuários empresariais, Power BI disponibiliza relatórios.

## <a name="security"></a>Segurança

Embora esta lista não completamente descreve todas as melhores práticas de segurança, aqui estão algumas considerações de segurança que se aplicam especificamente para os serviços do Azure implementados na arquitetura descrita neste artigo:

* Para certificar-se de que os utilizadores têm níveis de acesso adequados, utilize o controlo de acesso baseado em funções (RBAC).

* Proteger pontos finais públicos do API na gestão de API com OAuth ou OpenID Connect. Para proteger pontos finais da API públicos, configure um fornecedor de identidade e adicionar uma política de validação do JSON Web Token (JWT).

* Ligar a serviços de back-end da gestão de API, através da utilização de certificados mútuos.

* Proteja as aplicações de lógica baseada no acionador HTTP através da criação de uma lista de permissões de endereço IP que aponta para o endereço IP de gestão de API. Impede que um endereço IP na lista de permissões a chamar a aplicação lógica a partir da internet pública sem entrar primeiro através da gestão de API.

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [integração empresarial com filas e eventos](../logic-apps/logic-apps-architectures-enterprise-integration-with-queues-events.md)
