---
title: Serviços de integração Azure - integração empresarial simples
description: Arquitetura de referência, que mostra como implementar um padrão de integração empresarial simples com o Azure Logic Apps e gestão de API do Azure
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
ms.openlocfilehash: df86ca5aed405ab5eda05c1dd207f0b6656bfd96
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37860202"
---
# <a name="simple-enterprise-integration---reference-architecture"></a>Integração empresarial simples - arquitetura de referência

## <a name="overview"></a>Descrição geral

Esta arquitetura de referência mostra um conjunto de práticas comprovadas para uma aplicação de integração que utiliza serviços de integração do Azure. Esta arquitetura pode servir como esta base de muitos padrões de aplicação diferente que requerem HTTP APIs, o fluxo de trabalho e orquestração.

*Há muitos aplicativos possíveis de tecnologia de integração, de um aplicativo de ponto de simples ponto-a para um barramento de serviço completo de enterprise. Esta série de arquitetura define as partes do componente reutilizável que poderão aplicar-se para a criação de uma aplicação de integração genérico – arquitetos devem considerar quais componentes específicos, têm de implementar para seus aplicativos e infra-estrutura.*

   ![Diagrama da arquitetura - integração empresarial simples](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

## <a name="architecture"></a>Arquitetura

A arquitetura é composta pelos seguintes componentes:

- Grupo de recursos. Um [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) é um contentor lógico de recursos do Azure.
- Gestão de API do Azure. [Gestão de API do Azure](https://docs.microsoft.com/azure/api-management/) é uma plataforma totalmente gerida para publicação, proteger e transformar HTTP APIs.
- Portal de programador da gestão de API do Azure. Cada instância da gestão de API do Azure é fornecido com um [Portal do programador](https://docs.microsoft.com/azure/api-management/api-management-customize-styles), fornecer acesso à documentação, exemplos de código e a capacidade de testar uma API.
- Azure Logic Apps. [O Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview) é uma plataforma sem servidor para a criação de fluxo de trabalho empresariais e de integração.
- Conectores. [Conectores](https://docs.microsoft.com/azure/connectors/apis-list) são utilizadas pelo Logic Apps para ligar aos serviços frequentemente utilizados. O Logic Apps já tenha centenas de conectores diferentes, mas também podem ser criadas com um conector personalizado.
- Endereço IP. O serviço de gestão de API do Azure tem uma pública fixa [endereço IP](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) e um nome de domínio. O nome de domínio é um subdomínio do azure-api.net, por exemplo, contoso. Azure-api.net. Logic Apps e do Service Bus também tem um endereço IP público; No entanto, nesta arquitetura, restringir o acesso para chamar pontos finais de aplicações de lógica para apenas da API de gestão de endereços IP (para segurança). As chamadas para o Service Bus são protegidas por uma assinatura de acesso partilhado.
- DNS do Azure. [O DNS do Azure](https://docs.microsoft.com/azure/dns/) é um serviço de alojamento para domínios DNS que fornece resolução de nomes através da infraestrutura do Microsoft Azure. Ao alojar os seus domínios no Azure, pode gerir os recursos DNS com as mesmas credenciais, APIs, ferramentas e faturação dos seus outros serviços do Azure. Para utilizar um nome de domínio personalizado (por exemplo, contoso.com) crie registos DNS que mapeiam o nome de domínio personalizado para o endereço IP. Para obter mais informações, consulte Configurar um nome de domínio personalizado na API Management do Azure.
- Azure Active Directory (Azure AD). Uso [do Azure AD](https://docs.microsoft.com/azure/active-directory/) ou outro fornecedor de identidade para a autenticação. O Azure AD fornece autenticação para pontos finais de API de acesso (passando um [JSON Web Token para a gestão de API](https://docs.microsoft.com/azure/api-management/policies/authorize-request-based-on-jwt-claims) para validar) e pode proteger o acesso ao Portal de Programador de gestão de API (apenas escalões Standard e Premium).

Esta arquitetura tem alguns padrões fundamentais para a sua operação:

2. APIs compostas baseiam-se utilizar o Logic Apps; orquestrar a chamadas para sistemas SAAS, serviços do Azure e qualquer APIs publicadas para gestão de API. O [Logic Apps também são publicadas](https://docs.microsoft.com/azure/api-management/import-logic-app-as-api) através do Portal de Programador de gestão de API.
3. Aplicativos [adquirir um token de segurança de OAuth 2.0](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad) necessário para obter acesso a uma API com o Azure Active Directory.
4. Gestão de API do Azure [valida o token de segurança](https://docs.microsoft.com/azure/api-management/api-management-howto-protect-backend-with-aad)e transmite o pedido para o aplicação API/lógica de back-end.

## <a name="recommendations"></a>Recomendações

Seus requisitos específicos poderão diferir da arquitetura genérica descrita aqui. Utilize as recomendações nesta secção como um ponto de partida.

### <a name="azure-api-management-tier"></a>Escalão de gestão de API do Azure

Utilizar o básico, Standard ou Premium camadas porque eles oferecem um SLA de produção e suportam de escalamento horizontal na região do Azure (número de unidades varia consoante o escalão). O escalão Premium também suporta a aumentar horizontalmente em várias regiões do Azure. O escalão que escolheu em seu nível de débito necessário de base e conjunto de recursos. Para obter mais informações, consulte [preços da API Management](https://azure.microsoft.com/pricing/details/api-management/).

É-lhe cobrada todas as instâncias de gestão de API que são executados. Se tem aumentado verticalmente e não precisar desse nível de desempenho o tempo todo, considere a tirar partido da faturação por hora da gestão de API e dimensionamento para baixo.

### <a name="logic-apps-pricing"></a>Preços de aplicações lógicas

Aplicações lógicas também funcionam como um [sem servidor](logic-apps-serverless-overview.md) modelo – a faturação é calculada com base na execução de ações e conectores. Ver [preços de aplicações lógicas](https://azure.microsoft.com/pricing/details/logic-apps/) para obter mais informações. Não existem atualmente não existem considerações de camada para o Logic Apps.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Logic Apps para chamadas assíncronas de API

Aplicações lógicas também funcionam melhor em cenários que não necessitam de latência baixa – por exemplo, assíncrona ou chamadas de API semi longo execução. Se for necessária a baixa latência (por exemplo, uma chamada que bloqueia uma interface do usuário) é recomendável implementar essa API ou operação usando uma tecnologia diferente, por exemplo, as funções do Azure ou API Web implementadas com o serviço de aplicações. Ainda é recomendável que esta API ser apoiado com a gestão de API para os consumidores de APIS.

### <a name="region"></a>Região

Gestão de API de aprovisionamento e o Logic Apps na mesma região para minimizar a latência de rede. Por norma, escolha a região mais próxima dos seus utilizadores.

O grupo de recursos tem também uma região, que especifica onde os metadados de implementação são armazenados, e em que o modelo de implementação é executado a partir de. Coloque o grupo de recursos e os seus recursos na mesma região. Tal pode melhorar a disponibilidade durante a implementação.

## <a name="scalability-considerations"></a>Considerações de escalabilidade

Devem adicionar os administradores de gestão de API [políticas de colocação em cache](../api-management/api-management-howto-cache.md) quando apropriado para aumentar a escalabilidade do serviço e reduzir a carga de seus serviços de back-end.

Escalões de gestão de API básica, Standard e Premium do Azure podem ser ampliadas com uma região do Azure para oferecer uma capacidade maior. Os administradores podem usar a métrica de capacidade no menu de métricas para analisar a utilização do seu serviço e aumentar ou reduzir verticalmente, conforme adequados.

Recomendações para dimensionar um serviço de gestão de API:

- Dimensionamento precisa levar em conta os padrões de tráfego – os clientes com os padrões de tráfego mais volátil terá maior necessidade de maior capacidade.
- Consistente com capacidade superior a 66% pode indicar a necessidade de aumentar verticalmente.
- Capacidade consistente inferior a 20% pode indicar uma oportunidade para reduzir verticalmente.
- Recomenda-se sempre ao carregar o seu serviço de gestão de API com uma carga representativa de teste antes de ativar na produção.

Serviços de escalão Premium podem ser aumentados horizontalmente em várias regiões do Azure. Os clientes que implementam desta forma irão obter um SLA mais elevado (99,95%, em vez de 99,9%) e podem aprovisionar serviços próximo utilizadores em várias regiões.

Modelo de Logic Apps sem servidor significa que os administradores não é necessário tornar uma maior consideração de escalabilidade do serviço; o serviço pode ser dimensionada automaticamente para satisfazer a procura.

## <a name="availability-considerations"></a>Considerações de disponibilidade

No momento da escrita, o contrato de nível de serviço (SLA) para a gestão de API do Azure é de 99,9% para os escalões Basic, Standard e Premium. Configurações de escalão Premium com a implementação de, pelo menos, uma unidade em duas ou mais regiões têm um SLA de 99,95%.

No momento da escrita, o contrato de nível de serviço (SLA) do Azure Logic Apps é de 99,9%.

### <a name="backups"></a>Cópias de segurança

A configuração de gestão de API do Azure deve estar [backup regularmente](../api-management/api-management-howto-disaster-recovery-backup-restore.md) (adequadamente, com base no regularidade de alteração) e os ficheiros de cópia de segurança armazenados numa localização ou região do Azure diferente para onde reside o serviço. Os clientes podem, em seguida, escolher uma das duas opções para a sua estratégia de DR:

1. Num evento de DR, uma nova instância de gestão de API está aprovisionada, é restaurar a cópia de segurança ao mesmo e registos DNS são repointed.
2. Os clientes mantenha uma cópia passiva do seu serviço de cópias de segurança de outra região do Azure (incorrer em custos adicionais) regularmente são restauradas para ele. Num evento de DR, apenas os registos DNS tem de ser repointed para restaurar o serviço.

Como Logic Apps pode ser recriada muito rapidamente e são sem servidor, eles são uma cópia de segurança ao guardar uma cópia do modelo do Azure Resource Manager associados. Estes podem ser guardados a origem de controlo/integrados no processo de implementação (CI/CD) integração contínua/contínua de uma parte dos clientes.

O Logic Apps que foram publicadas através da gestão de API tem seus locais atualizados mover para um centro de dados diferentes. Isso pode ser realizado por meio de um simple script do PowerShell para atualizar a propriedade de back-end da API.

## <a name="manageability-considerations"></a>Considerações sobre a capacidade de gestão

Criar grupos de recursos separados para a produção, desenvolvimento e os ambientes de teste. Este procedimento torna mais fácil gerir as implementações, eliminar as implementações de teste e atribuir direitos de acesso.
Ao atribuir recursos aos grupos de recursos, considere o seguinte:

- Ciclo de vida. Em geral, coloque os recursos com o mesmo ciclo de vida no mesmo grupo de recursos.
- Acesso. Pode usar [controlo de acesso baseado em funções](../role-based-access-control/overview.md) (RBAC) para aplicar políticas de acesso a recursos num grupo.
- Faturação. Pode ver os custos agregados ao grupo de recursos.
- Escalão de preço para a gestão de API – Recomendamos que utilize o escalão de programador para ambientes de desenvolvimento e teste. Para a pré-produção, recomendamos a implementação de uma réplica do ambiente de produção, a execução de testes e, em seguida, encerrar para minimizar os custos.

Para obter mais informações, consulte [grupo de recursos](../azure-resource-manager/resource-group-overview.md) descrição geral.

### <a name="deployment"></a>Implementação

Recomendamos que utilize [modelos Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) para implementar a gestão de API do Azure e Azure Logic Apps. Modelos tornam mais fácil automatizar as implementações através do PowerShell ou a interface de linha de comandos (CLI) do Azure.

Recomendamos que o serviço de colocação de API Management do Azure e qualquer Logic Apps individuais em seus próprios modelos do Resource Manager separado. Isso permitirá que a armazená-los em sistemas de controle de origem. Estes modelos, em seguida, podem ser implementados em conjunto ou individualmente como parte de um processo de implementação contínua integração/contínua (CI/CD).

### <a name="versions"></a>Versões

Sempre que fizer uma configuração alterar para uma aplicação lógica (ou implementar uma atualização através de um modelo do Resource Manager), uma cópia do que a versão é mantida para sua comodidade (todas as versões que têm um histórico de execuções serão mantidas). Pode utilizar estas versões para controlar alterações históricas e também promover uma versão seja a configuração atual da aplicação lógica; ao fazer isto, pode efetivamente reverter uma aplicação lógica, por exemplo.

Gestão de API tem dois distintos (mas complementares) [conceitos de controlo de versões](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/):

- As versões utilizadas para fornecer os consumidores de API com uma opção da API que era possível consumir com base nas suas necessidades (por exemplo, v1, v2 ou beta, produção).
- Revisões permitindo que os administradores de API efetuar alterações a uma API e implementá-las para os utilizadores com comentários opcionais em segurança.

No contexto de implementação – revisões de gestão de API devem ser considerados como uma forma de efetuar alterações de forma segura, manter um histórico de alterações e fazer com que os consumidores de APIS com reconhecimento de uma destas alterações. Uma revisão pode ser criada num ambiente de desenvolvimento e implementada, entre outros ambientes com modelos do Resource Manager.

Embora revisões podem ser utilizados para testar uma API antes que ela for feita 'current' e disponibilizados a utilizadores, recomendamos que não utilize esse mecanismo para a carga ou teste de integração – teste separada ou ambientes de pré-produção devem ser utilizados em vez disso.

### <a name="configuration"></a>Configuração

Nunca verificar as palavras-passe, chaves de acesso ou cadeias de ligação no controle de origem. Se forem necessárias, use a técnica apropriada para implantar e proteger esses valores. 

No Logic Apps, quaisquer valores confidenciais necessários na sua aplicação lógica (que não é possível criar na forma de uma ligação) devem ser armazenados no Azure Key Vault e chamados a partir de um modelo do Resource Manager. Podemos também sugiro o uso de parâmetros de modelo de implementação juntamente com os ficheiros de parâmetro para cada ambiente. Obter mais orientações sobre [protegendo parâmetros e de entradas num fluxo de trabalho](logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

Na gestão de API, os segredos são geridos através do chamado valores com o nome/as propriedades de objetos. Estes armazenam em segurança os valores que podem ser acessados nas políticas de gestão de API. Veja como [gerir segredos na gestão de API](../api-management/api-management-howto-properties.md).

### <a name="diagnostics-and-monitoring"></a>Diagnóstico e monitorização

Ambos [gestão de API](../api-management/api-management-howto-use-azure-monitor.md) e [Logic Apps](logic-apps-monitor-your-logic-apps.md) monitorização operacional por meio de suporte [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Monitor do Azure está ativado por predefinição e irá fornecer informações com base em diferentes métricas configuradas para cada serviço.

Além disso, existem mais opções para cada serviço:

- Registos de aplicações lógicas podem ser enviados para [do Log Analytics](logic-apps-monitor-your-logic-apps-oms.md) para mais aprofundado de análise e vistas.
- Gestão de API oferece suporte a configurar o Application Insights para monitorização de operações de programação.
- Gestão de API suporta a [modelo de solução do Power BI para análise de API personalizada](http://aka.ms/apimpbi). Este modelo de solução permite aos clientes criar sua própria solução de análise personalizada, com relatórios disponíveis no Power BI para utilizadores empresariais.

## <a name="security-considerations"></a>Considerações de segurança

Esta secção lista as considerações de segurança que são específicas para os serviços do Azure descritos neste artigo, implantado na arquitetura, tal como descrito. Não é uma lista completa das melhores práticas de segurança.

- Utilize o controlo de acesso baseado em funções (RBAC) para garantir níveis apropriados de acesso dos utilizadores.
- Proteger pontos finais públicos do API na gestão de API com OAuth/Open IDConnect. Fazer isso, deve configurar um fornecedor de identidade e adicionar uma política de validação do JWT.
- Ligar a serviços de back-end da gestão de API de utilização de certificados mútuos
- Proteja as aplicações de lógica de baseado no acionador HTTP através da criação de uma lista de permissões do endereço IP que aponta para o endereço IP de API Management. Isto impede que chamar a aplicação lógica a partir da internet pública sem entrar primeiro através da gestão de API.

Esta arquitetura de referência mostrou como criar uma plataforma de integração empresarial simples usando os serviços de integração do Azure.

## <a name="next-steps"></a>Passos Seguintes

- [Integração empresarial com filas e eventos](logic-apps-architectures-enterprise-integration-with-queues-events.md)
