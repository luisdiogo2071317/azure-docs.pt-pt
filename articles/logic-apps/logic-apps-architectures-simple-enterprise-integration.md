---
title: Serviços de integração do Azure - integração empresarial simples
description: Arquitetura de referência que mostra como implementar um padrão de integração do enterprise simples com Azure Logic Apps e a API Management do Azure
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
ms.openlocfilehash: 17f591a470bf65d3c9365bca9c5ae2d5a6c9574f
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232436"
---
# <a name="simple-enterprise-integration---reference-architecture"></a>Simples integração empresarial com - arquitetura de referência

## <a name="overview"></a>Descrição geral

Esta arquitetura de referência mostra um conjunto de práticas comprovados para uma aplicação de integração que utiliza serviços de integração do Azure. Esta arquitetura pode servir como esta base de muitos padrões de diferentes aplicações que necessitam de APIs de HTTP, o fluxo de trabalho e orquestração.

*Existem muitas aplicações possíveis de tecnologia de integração, simples ponto a ponto de uma aplicação de um para um barramento de serviço completo de empresa. Esta série de arquitetura define terminar as partes de componente reutilizáveis para a criação de qualquer aplicação de integração – arquitetos de TI devem considerar os componentes que serão necessário para as respetivas aplicações e infraestruturas.*

   ![Diagrama de arquitetura - integração empresarial simples](./media/logic-apps-architectures-simple-enterprise-integration/simple_arch_diagram.png)

## <a name="architecture"></a>Arquitetura

A arquitetura é composta pelos seguintes componentes:

- Grupo de recursos. Um grupo de recursos é um contentor lógico de recursos do Azure.
- API Management do Azure. Gestão de API do Azure é uma plataforma completamente gerida para publicação, proteger e transformar os APIs de HTTP.
- Portal do Programador de gestão de API do Azure. Cada instância da API Management do Azure inclui um Portal de programador, conceder acesso à documentação, exemplos de código e a capacidade de uma API de teste.
- Aplicações lógicas do Azure. As aplicações lógicas é uma plataforma sem servidor para criação de fluxo de trabalho da empresa e integração.
- Conectores. Os conectores são utilizados pelo Logic Apps para ligar aos serviços comummente utilizados. As Logic Apps já centenas de conectores diferentes, mas também podem ser criadas utilizando um conetor personalizado.
- Endereço IP. O serviço de API Management do Azure tem um endereço IP público fixo e um nome de domínio. O nome de domínio é um subdomínio api.net do azure, tais como contoso. api.net do Azure. As Logic Apps também tem um endereço IP público; No entanto, esta arquitetura é restringir o acesso para chamar pontos finais de aplicações lógicas para apenas o de API gestão de endereços IP (segurança).
- DNS do Azure. O DNS do Azure é um serviço de alojamento de domínios DNS, fornecer a resolução do nome utilizando a infraestrutura do Microsoft Azure. Ao alojar os seus domínios no Azure, pode gerir os recursos DNS com as mesmas credenciais, APIs, ferramentas e faturação dos seus outros serviços do Azure. Para utilizar um nome de domínio personalizado (tal como contoso.com) crie registos DNS que mapeiam o nome de domínio personalizado para o endereço IP. Para obter mais informações, consulte Configurar um nome de domínio personalizado na API Management do Azure.
- Azure Active Directory (Azure AD). Utilize o Azure AD ou outro fornecedor de identidade para a autenticação. Azure AD fornece autenticação para pontos finais da API de acesso (através da transmissão de um Web Token JSON para a gestão de API a validar) e pode proteger o acesso ao Portal de Programador de gestão de API (apenas camadas Standard e Premium).

Esta arquitetura tem alguns padrões fundamentais para a conclusão da operação:

1. Back-end existente HTTP APIs são publicadas através do Portal de programador da gestão de API, permitindo aos programadores (ou interno para a sua organização, externo ou ambos) para integrar chamadas para estas APIs nas aplicações.
2. APIs compostas criadas com Logic Apps; da orquestração chamadas para sistemas SAAS, serviços do Azure e qualquer APIs publicados API Management. As Logic Apps também são publicadas através do Portal de programador da gestão de API.
3. Aplicações adquirir um token de segurança do OAuth 2.0, necessário para obter acesso a uma API utilizando o Azure Active Directory.
4. Gestão de API do Azure valida o token de segurança e transmite o pedido para o aplicação API/lógica de back-end.

## <a name="recommendations"></a>Recomendações

Os requisitos podem ser diferentes da arquitetura descrita aqui. Utilize as recomendações nesta secção como um ponto de partida.

### <a name="azure-api-management-tier"></a>Camada de API Management do Azure

Utilizar o básico, Standard ou Premium camadas pois oferecem uma SLA de produção e suporta Escalamento horizontal na região do Azure (número de unidades varia consoante a camada). Escalão Premium também suporta Escalamento horizontal em várias regiões do Azure. A camada que escolheu no seu nível de débito necessário de base e funcionalidade definida. Para obter mais informações, consulte [preços da API Management](https://azure.microsoft.com/pricing/details/api-management/).

São-lhe cobrados para todas as instâncias de API Management quando estiverem a executar. Se tiver escalado para cima e não precisa de nível de desempenho do tempo, considere tirar partido da gestão de API por hora e faturação escala para baixo.

### <a name="logic-apps-pricing"></a>Logic Apps de preços

As Logic Apps funciona como um [sem servidor](logic-apps-serverless-overview.md) modelo – comprador de faturação é calculada com base na execução da ação e o conector. Consulte [Logic Apps preços](https://azure.microsoft.com/pricing/details/logic-apps/) para obter mais informações. Não existem atualmente não existem considerações sobre a camada Logic Apps.

### <a name="logic-apps-for-asynchronous-api-calls"></a>Logic Apps para chamadas de API assíncronas

As Logic Apps funciona melhor em cenários que não necessitam de latência baixa – por exemplo, assíncrona ou semi longo execução chamadas da API. Se for necessária a latência baixa (por exemplo, uma chamada bloqueia uma interface de utilizador) é recomendável implementar essa API ou a operação utilizando uma tecnologia de diferentes, por exemplo, as funções do Azure ou API Web implementado através do serviço de aplicações. Recomendamos que esta API ser fronted utilizando a gestão de API para os consumidores de API.

### <a name="region"></a>Região

Aprovisione a API Management e Logic Apps na mesma região para minimizar a latência de rede. Por norma, escolha a região mais próxima dos seus utilizadores.

O grupo de recursos tem também uma região que especifica a armazenar metadados de implementação, e onde o modelo de implementação é executado de. Coloque o grupo de recursos e os seus recursos na mesma região. Tal pode melhorar a disponibilidade durante a implementação.

## <a name="scalability-considerations"></a>Considerações de escalabilidade

Devem adicionar os administradores da gestão de API [políticas de colocação em cache](../api-management/api-management-howto-cache.md) , conforme apropriado para aumentar a escalabilidade do serviço e reduzir a carga nos serviços de back-end.

Camadas de API Management básico, Standard e Premium do Azure podem ser ampliadas com uma região do Azure para oferecer maior capacidade. Os administradores podem utilizar a métrica de capacidade no menu de métricas para analisar a utilização do seu serviço e aumentar verticalmente ou reduzir verticalmente, conforme adequados.

Recomendações para dimensionar um serviço de gestão de API:

- Dimensionamento tem de ter a conta de padrões de tráfego – clientes com padrões de tráfego mais volátil terá maior necessidade de capacidade acrescida.
- Consistente capacidade superior a 66% poderá indicam a necessidade de aumentar verticalmente.
- Capacidade consistente abaixo 20% pode indicar uma oportunidade para reduzir verticalmente.
- Recomenda-se sempre ao carregar o seu serviço de API Management com uma carga representativo de teste antes de ativar o em produção.

Serviços do escalão Premium podem ser ampliados em várias regiões do Azure. Os clientes implementar desta forma irão obter um SLA superior (99,95%, por oposição 99,9%) e podem aprovisionar serviços próximo de utilizadores em várias regiões.

Modelo sem servidor as Logic Apps significa que os administradores não é necessário tornar consideração adicional de escalabilidade do serviço; o serviço ajusta automaticamente para satisfazer o pedido.

## <a name="availability-considerations"></a>Considerações de disponibilidade

No momento da escrita, o contrato de nível de serviço (SLA) de API Management do Azure é 99,9% dos escalões básico, Standard e Premium. Configurações de escalão Premium com a implementação pelo menos uma unidade em dois ou mais regiões têm um SLA de 99,95%.

No momento da escrita, o contrato de nível de serviço (SLA) para o Azure Logic Apps é 99,9%.

### <a name="backups"></a>Cópias de segurança

A configuração da API Management do Azure deve ser [regular de cópias de](../api-management/api-management-howto-disaster-recovery-backup-restore.md) (adequadamente baseado no regularity de alteração) e os ficheiros de cópia de segurança armazenados numa localização ou região do Azure diferente onde reside o serviço. Os clientes, em seguida, podem escolher uma das duas opções para a sua estratégia de DR:

1. Um evento de DR, uma nova instância de API Management é aprovisionada, é restaurada a cópia de segurança e registos DNS são repointed.
2. Clientes manter uma cópia passiva do seu serviço em cópias de segurança de região do Azure (incorrer em custos adicionais) outro regularmente são restauradas para a mesma. Um evento de DR, apenas os registos DNS tem de ser repointed para restaurar o serviço.

Como as Logic Apps podem ser recriadas muito rapidamente e são sem servidor, estes são cópias de segurança por guardar uma cópia do modelo Azure Resource Manager associados. Estes podem ser guardados para a origem de controlo/integrados no processo de implementação (CI/CD) de integração contínua/contínua dos clientes.

As Logic Apps que tiverem sido publicadas através da API de gestão terão as respetivas localizações atualizadas se mover para um centro de dados diferentes. Isto pode ser conseguido através de um script do PowerShell simple para atualizar a propriedade de back-end da API.

## <a name="manageability-considerations"></a>Considerações sobre a capacidade de gestão

Criar grupos de recursos separado para produção, desenvolvimento e ambientes de teste. Este procedimento torna mais fácil gerir as implementações, eliminar as implementações de teste e atribuir direitos de acesso.
Ao atribuir recursos aos grupos de recursos, considere o seguinte:

- Ciclo de vida. Em geral, coloque os recursos com o mesmo ciclo de vida no mesmo grupo de recursos.
- Acesso. Pode utilizar [controlo de acesso baseado em funções](../role-based-access-control/overview.md) (RBAC) para aplicar políticas de acesso aos recursos num grupo.
- Faturação. Pode ver os custos agregados ao grupo de recursos.
- Escalão de preço para gestão de API – Recomendamos que utilize a camada de programador para ambientes de desenvolvimento e teste. Para a pré-produção, recomendamos a implementação de uma réplica do seu ambiente de produção, executar testes e, em seguida, a ser encerrado para minimizar os custos.

Para obter mais informações, consulte [grupo de recursos](../azure-resource-manager/resource-group-overview.md) descrição geral.

### <a name="deployment"></a>Implementação

Recomendamos que utilize [modelos Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) para implementar a API Management do Azure e Azure Logic Apps. Modelos de tornam mais fácil automatizar as implementações através do PowerShell ou a interface de linha de comandos do Azure (CLI).

Recomendamos colocando a API Management do Azure e qualquer Logic Apps individuais nos seus próprios modelos do Resource Manager separado. Isto permitirá armazene-os em sistemas de controlo de origem. Estes modelos podem ser implementados em conjunto ou individualmente como parte de um processo de implementação contínua integração/contínua (CI/CD).

### <a name="versions"></a>Versões

Sempre que efetuar uma configuração alterar para uma aplicação lógica (ou implementar uma atualização através de um modelo do Resource Manager), uma cópia do que a versão é mantida para sua comodidade (todas as versões que têm um histórico de execução serão mantidas). Pode utilizar estas versões para controlar as alterações históricas e também promover uma versão seja a configuração atual da aplicação lógica; ao fazer isto, pode ser efetivamente reversão uma aplicação lógica, por exemplo.

API Management tem dois distintos (mas complimentary) [conceitos de controlo de versões](https://blogs.msdn.microsoft.com/apimanagement/2018/01/11/versions-revisions-general-availibility/):

- As versões utilizadas para fornecer os consumidores de API com a API foi consumirem escolher com base nas suas necessidades (por exemplo, v1, v2 ou beta, produção).
- Revisões de permitir que os administradores de API com segurança efetuar alterações a uma API e implementá-las para os utilizadores com interrompa opcional.

No contexto de implementação – gestão de API revisões devem ser consideradas como uma forma de efetuar alterações de forma segura, manter um histórico de alterações e de consumidores de API com suporte para uma destas alterações. Uma revisão pode ser criada num ambiente de desenvolvimento e implementada, entre outros ambientes com modelos do Resource Manager.

Enfrenta revisões podem ser utilizadas para testar uma API antes de que é efetuada 'current' e efetuadas acessíveis aos utilizadores, não recomendamos que utilize este mecanismo de carga ou de teste de integração – teste separada ou ambientes de pré-produção deve ser utilizado em vez disso.

### <a name="configuration"></a>Configuração

Nunca verificar as palavras-passe, chaves de acesso ou cadeias de ligação ao controlo de origem. Se forem necessárias, utilize a técnica apropriada para implementar e proteger estes valores. 

As Logic Apps, quaisquer valores confidenciais necessários na sua aplicação lógica (que não é possível criar o formato de uma ligação) devem ser armazenados no Cofre de chaves do Azure em referido a partir de um modelo do Resource Manager. Podemos também sugerir utilizando os parâmetros do modelo de implementação, juntamente com os ficheiros de parâmetro para cada ambiente. Obter mais orientações sobre a [proteger entradas num fluxo de trabalho e os parâmetros](logic-apps-securing-a-logic-app.md#secure-parameters-and-inputs-within-a-workflow).

Na API Management, os segredos são geridos através de objetos denominados valores/propriedades com o nome. Estes armazenam com segurança os valores que podem ser acedidos nas políticas de gestão de API. Veja como [gerir segredos na API Management](../api-management/api-management-howto-properties.md).

### <a name="diagnostics-and-monitoring"></a>Diagnóstico e monitorização

Ambos [API Management](../api-management/api-management-howto-use-azure-monitor.md) e [Logic Apps](logic-apps-monitor-your-logic-apps.md) suportar a monitorização operacional através de [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md). Monitor do Azure está ativada por predefinição e irá fornecer informações com base nas métricas diferentes configuradas para cada serviço.

Além disso, existem mais opções para cada serviço:

- Os registos de aplicações lógicas podem ser enviados para [Log Analytics](logic-apps-monitor-your-logic-apps-oms.md) para mais aprofundado de análise e dashboarding.
- Gestão de API suporta configurar Application Insights para Dev Ops monitorização.
- Gestão de API suporta a [modelo de solução do Power BI para a análise de API personalizada](http://aka.ms/apimpbi). Este modelo de solução permite aos clientes criar as seus próprios solução de análise personalizada, com os relatórios disponíveis no Power BI para os utilizadores empresariais.

## <a name="security-considerations"></a>Considerações de segurança

Esta secção lista as considerações de segurança que são específicas para os serviços do Azure descritos neste artigo, implementado numa arquitetura, tal como descrito. Não é uma lista completa das melhores práticas de segurança.

- Utilize o controlo de acesso baseado em funções (RBAC) para garantir níveis adequados de acesso de utilizadores.
- Proteger pontos finais públicos da API na API Management utilizando OAuth/abra IDConnect. Fazê-lo ao configurar um fornecedor de identidade e adicionar uma política de validação JWT.
- Se ligam aos serviços de back-end da gestão de API utilizando certificados mútua
- Proteja HTTP baseado no acionador Logic Apps, criando uma lista branca de endereço IP que apontam para o endereço IP de API Management. Isto impede que chamar a aplicação lógica a partir da internet pública sem primeiro passar API Management.

Esta arquitetura de referência mostrada como criar uma plataforma de integração do enterprise simples utilizando os serviços de integração do Azure.

## <a name="next-steps"></a>Passos Seguintes

* [Integração de Enterprise com as filas e eventos](logic-apps-architectures-enterprise-integration-with-queues-events.md)
