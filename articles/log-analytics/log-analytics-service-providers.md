---
title: Funcionalidades de análise de registo para fornecedores de serviços | Microsoft Docs
description: Análise de registos pode ajudar geridos fornecedores de serviços (MSPs), as grandes empresas, os fabricantes Sofware independentes (ISVs) e fornecedores de serviços de alojamento gerirem e monitorizar os servidores de infraestrutura de nuvem ou no local do cliente.
services: log-analytics
documentationcenter: ''
author: richrundmsft
manager: jochan
editor: ''
ms.assetid: c07f0b9f-ec37-480d-91ec-d9bcf6786464
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: meirm
ms.component: na
ms.openlocfilehash: 73f199b83fc5fc56259a2fbbc9dce4bbd8b23998
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37129734"
---
# <a name="log-analytics-features-for-service-providers"></a>Funcionalidades de análise do registo para fornecedores de serviços
Análise de registos pode ajudar os fornecedores de serviços geridos (MSPs), as grandes empresas, os fabricantes independentes de software (ISV) e fornecedores de serviços de alojamento gerir e monitorizar os servidores de infraestrutura de nuvem ou no local do cliente. 

As grandes empresas partilham diversas semelhanças com fornecedores de serviços, especialmente quando existe uma equipa de TI centralizada que é responsável pela gestão IT para muitas unidades empresariais diferentes. Simplicidade, este documento utiliza o termo *fornecedor de serviços* , mas a mesma funcionalidade também está disponível para as empresas e outros clientes.

Parceiros e fornecedores de serviço que fazem parte do [fornecedor de solução em nuvem (CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) programa, análise de registos é um dos serviços do Azure disponíveis no [subscrição Azure CSP](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview). 

## <a name="architectures-for-service-providers"></a>Arquiteturas de fornecedores de serviços

Áreas de trabalho de análise de registo fornecem um método para o administrador pode controlar o fluxo e o isolamento dos registos e criar uma arquitetura de registo que abrange as necessidades comerciais específicas. [Este artigo](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-manage-access) explica as considerações gerais em torno da gestão de área de trabalho. Fornecedores de serviços tem considerações adicionais.

Existem três arquiteturas possíveis para fornecedores de serviços sobre áreas de trabalho de análise de registos:

### <a name="1-distributed---logs-are-stored-in-workspaces-located-in-the-customers-tenant"></a>1. Distribuída - os registos são armazenados em áreas de trabalho localizadas no inquilino do cliente 

Nesta arquitetura, a área de trabalho é implementada no inquilino do cliente que é utilizado para todos os registos desse cliente. Os administradores do fornecedor de serviço são concedidos o acesso a esta área de trabalho utilizando [utilizadores convidados do Azure Active Directory (B2B)](https://docs.microsoft.com/en-us/azure/active-directory/b2b/what-is-b2b). O administrador do fornecedor de serviço tem de mudar no portal do Azure para o diretório do seu cliente para poder aceder a estas áreas de trabalho.

As vantagens colateral desta arquitetura são:
* O cliente pode gerir o acesso aos registos utilizando os seus próprios [acesso baseado em funções](https://docs.microsoft.com/en-us/azure/role-based-access-control/overview).
* Cada cliente pode ter definições diferentes para a sua área de trabalho, tais como a retenção e capping de dados.
* Isolamento entre os clientes de regulamentação e conformidade era.
* A taxa cada área de trabalho será revertida para a subscrição do cliente.
* Os registos podem ser recolhidos de todos os tipos de recursos, não apenas agente baseado em. Por exemplo, a auditoria do Azure.

As desvantagens colateral desta arquitetura são:
* É mais difícil para o fornecedor de serviço gerir o elevado número de inquilinos do cliente em simultâneo.
* Os administradores do fornecedor de serviço tem de ser aprovisionado no diretório de cliente.
* O fornecedor de serviços não é possível analisar os dados em que os seus clientes.

### <a name="2-central---logs-are-stored-in-workspace-located-in-the-service-provider-tenant"></a>2. Central - registos são armazenados na área de trabalho localizada no inquilino do fornecedor de serviço

Nesta arquitetura, os registos não são armazenados em inquilinos do cliente, mas apenas numa localização central dentro de uma das subscrições do fornecedor de serviços. Os agentes que estão instalados em VMs do cliente estão configurados para enviar os registos para esta área de trabalho com o ID da área de trabalho e a chave secreta.

As vantagens colateral desta arquitetura são:
* É fácil de gerir elevado número de clientes e integrá-los para vários sistemas de back-end.
* O fornecedor de serviços tem propriedade completa sobre os registos e os vários artefactos, tais como as funções e guardar consultas.
* O fornecedor de serviços pode efetuar análises em todos os clientes.

As desvantagens colateral desta arquitetura são:
* Será difícil de separar os dados entre os clientes. O método única para o fazer consiste em utilizar o nome de domínio do computador.
* Todos os dados de todos os clientes serão armazenados na mesma região com uma única fatura e as mesmas definições de retenção e a configuração.
* Recursos de infraestrutura do Azure e PaaS serviços, tais como diagnóstico do Azure e a auditoria do Azure requer a área de trabalho estar no mesmo inquilino como o recurso, por conseguinte, não poderem enviar os registos para a área de trabalho central.

### <a name="3-hybrid---logs-are-stored-in-workspace-located-in-the-customers-tenant-and-some-of-them-are-pulled-to-a-central-location"></a>3. Híbrida - registos são armazenados na área de trabalho localizada no inquilino do cliente e alguns deles são solicitados para uma localização central.

Combinar a arquitetura terceira entre as duas opções. Baseia-se na arquitetura distribuída primeiro onde os registos são locais para cada cliente, mas utilizar algum mecanismo para criar um repositório central de registos. Uma parte dos registos é solicitada para uma localização central para análises e relatórios. Esta parte pode ser pequeno número de tipos de dados ou um resumo da atividade como estatística diária.

Existem duas opções para implementar a localização central na análise de registos:

1. Área de trabalho central: O fornecedor de serviço pode criar uma área de trabalho no seu inquilino e utilizar um script que utiliza o [API de consulta](https://dev.loganalytics.io/) com o [API de recolha de dados](log-analytics-data-collector-api.md) para colocar os dados de áreas de trabalho vários Isto localização central. Outra opção que não seja o script está a utilizar [aplicação de lógica de Azure](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-overview).

2. Power BI como uma localização central: Power BI pode atuar como a localização central, quando as várias áreas de trabalho exportar dados para-la utilizando a integração entre o análise de registos e [Power BI](log-analytics-powerbi.md). 


## <a name="next-steps"></a>Próximos Passos
* Automatizar a criação e a configuração de áreas de trabalho utilizando [modelos do Resource Manager](log-analytics-template-workspace-configuration.md)
* Automatizar a criação de áreas de trabalho utilizando [PowerShell](log-analytics-powershell-workspace-configuration.md) 
* Utilize [alertas](log-analytics-alerts.md) para integrar com sistemas existentes
* Gerar relatórios de resumo utilizando [Power BI](log-analytics-powerbi.md)

