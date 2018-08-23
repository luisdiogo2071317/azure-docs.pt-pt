---
title: Versões de API do fornecedor de recursos suportadas pelo perfis no Azure Stack | Documentos da Microsoft
description: Saiba mais sobre a versão do Azure Resource Manager suportada pelo perfis no Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 9ba954db7003e062444e57bd56f87ff5d279b07f
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "42059926"
---
# <a name="resource-provider-api-versions-supported-by-profiles-in-azure-stack"></a>Versões de API do fornecedor de recursos suportadas pelo perfis no Azure Stack

Pode encontrar o fornecedor de recursos e os números de versão para cada perfil de API utilizada pelo Azure Stack neste artigo. As tabelas neste artigo listam as versões suportadas para cada fornecedor de recursos e as versões de API dos perfis. Cada fornecedor de recursos contém um conjunto de tipos de recursos e os números de versão específica.

O perfil de API utiliza três convenções de nomenclatura:
 - mais recente
 - Aaaa-mm-dd-híbrida
 - aaaa-mm-dd-perfil

Para obter uma explicação de perfis de API e cadência de lançamento de versão para o Azure Stack, veja [perfis de versão de API de gerir no Azure Stack](azure-stack-version-profiles.md).

> [!Note]  
> O **mais recente** perfil de API contém a versão mais recente da versão de API do fornecedor de recursos e não está listado neste artigo.

## <a name="overview-of-2018--03-01-hybrid"></a>Descrição geral de 2018 – 03-01-híbrida

| Fornecedor de recursos | versão de API |
|-----------------------------------------------|-----------------------------------------------------|
| Microsoft.Compute | 2017-03-30 |
| Microsoft.Network | 2017-10-01<br>Gateway de VPN será 2017-03-01 |
| Microsoft. Storage (plano de dados) | 2017-04-17 |
| Microsoft. Storage (plano de controlo) | 2016-01-01 |
| Microsoft. Web | 2016-08-01<br>qual é a versão mais recente (a partir de agora) no Azure |
| Microsoft.KeyVault | 01 de 10 de 2016 (não alterar) |
| Microsoft.Resources (Azure Resource Manager em si) | 2016-02-01 |
| Microsoft.Authorization (operações de política) | 2015-11-01 |
| Microsoft.Insights | 2015-11-01 |
| Microsoft. keyvault | 2016-10-01 |
| Política | 2016-10-01 |
| Recursos | 2016-10-01 |
| Resources_Links | 2016-10-01 |
| Resources_Locks | 2016-10-01 |
| Subscrições | 2016-10-01 |

Para mais uma lista das versões para cada tipo de recurso para os fornecedores no perfil de api, consulte [detalhes para a definição de 2018-03-01-híbrida](#details-for-the-2018-03-01-hybrid) perfil.

## <a name="overview-of-2017-03-09-profile"></a>Descrição geral do perfil-de-03-09-2017

| Fornecedor de recursos | versão de API |
|------------------------------------------------|------------------------------|
| Microsoft.Compute | 2016-03-30 |
| Microsoft.Network | 2015-06-15 |
| Microsoft. Storage (plano de dados) | 2015-04-05  |
| Microsoft. Storage (plano de controlo) | 2016-01-01   |
| Microsoft.Websites | 2016-01-01 |
| Microsoft.KeyVault | 2016-10-01<br>(Não alterar) |
| Microsoft.Resources<br>(O azure Resource Manager em si) | 2016-02-01 |
| Microsoft.Authorization<Br>(operações de política) | 2015-11-01 |
| Microsoft.Insights | 2015-11-01 |
| Microsoft. keyvault | 2016-10-01 |
| Política | 2015-10-01-pré-visualização |
| Recursos | 2016-02-01 |
| Resources_Links | 2016-09-01 |
| Resources_Locks | 2016-09-01 |
| Subscrições | 2016-06-1 |

Para mais uma lista das versões para cada tipo de recurso para os fornecedores no perfil de api, consulte [detalhes para o 2017-03-09-perfil](#details-for-the-2017-03-09-profile)

## <a name="details-for-the-2018-03-01-hybrid"></a>Detalhes para a definição de 2018-03-01-híbrida

### <a name="microsoftauthorization"></a>Microsoft.Authorization

Utilize o controlo de acesso baseado em funções para gerir as ações que os utilizadores na sua organização podem executar nos recursos. Este conjunto de operações permite-lhe definir funções, atribuir funções a utilizadores ou grupos e obter informações sobre permissões. Para obter mais informações, consulte [autorização](https://docs.microsoft.com/rest/api/authorization/).

| Tipos de Recurso | Versões de API |
|---------------------|--------------------|
| Bloqueios | 2017-04-01 |
| Operações | 2015-07-01 |
| Permissões | 2015-07-01 |
| Atribuições de Política | 2016-12-01 (2017-06-01-pré-visualização) |
| Definições de Política | 2016-12-01 |
| Operações de fornecedor | 2015-07-01-pré-visualização |
| Atribuições de Funções | 2015-07-01 |
| Definições de funções | 2015-07-01 |

### <a name="microsoftcommerce"></a>Microsoft.Commerce

| Tipo de Recurso | Versão da API |
|----------------------------------|----------------------|
| Subscrições do fornecedor delegado | 2015-06-01 - pré-visualização |
| Agregados de utilização de delegados | 2015-06-01 - pré-visualização |
| Recursos de estimativa de gastos | 2015-06-01 – pré-visualização |
| Operações | 2015-06-01 - pré-visualização |
| Agregados de utilização do subscritor | 2015-06-01 - pré-visualização |
| Agregados de Utilização | 2015-06-01 - pré-visualização |

### <a name="microsoftcompute"></a>Microsoft.Compute

As APIs de computação do Azure dão-lhe acesso programático a máquinas virtuais e seus recursos de suporte. Para obter mais informações, consulte [computação do Azure](https://docs.microsoft.com/rest/api/compute/).

| Tipo de Recurso | Versão da API |
|---------------------------------------------------------------|-------------|
| Conjuntos de Disponibilidade | 2016-03-30 |
| Localizações | 2016-03-30 |
| Localizações/operações | 2016-03-30 |
| Localizações/editores | 2016-03-30 |
| Localizações/utilizações | 2016-03-30 |
| Localizações/vmSizes | 2016-03-30 |
| Operações | 2016-03-30 |
| Virtual Machines | 2016-03-30 |
| Extensões de máquinas virtuais / | 2016-03-30 |
| Conjuntos de Dimensionamento de Máquinas Virtuais | 2016-03-30 |
| Conjuntos de dimensionamento de máquinas virtuais/extensões | 2016-03-30 |
| Interfaces de rede/conjuntos de dimensionamento de máquinas virtuais | 2016-03-30 |
| Máquinas virtuais/conjuntos de dimensionamento de máquina virtual | 2016-03-30 |
| Conjuntos de dimensionamento de máquinas virtuais/virtualMachines/networkInterfaces | 2016-03-30 |

### <a name="microsoftgallery"></a>Microsoft.Gallery

| Tipo de Recurso | Versão da API |
|------------------|-------------|
| Organização | 2015-04-01 |
| Conteúdo de Organização | 2015-04-01 |
| Extrato de Organização | 2015-04-01 |
| Itens da Galeria | 2015-04-01 |
| Operações | 2015-04-01 |
| Portal | 2015-04-01 |
| Pesquisa | 2015-04-01 |
| Sugerir | 2015-04-01 |

### <a name="microsoftinsights"></a>Microsoft.Insights

| Tipos de Recurso | Versões de API |
|--------------------|--------------------|
| Operações | 2015-04-01 |
| Tipos de Eventos | 2015-04-01 |
| Categorias de eventos | 2015-04-01 |
| Definições de Métricas | 2018-01-01 |
| Métricas | 2018-01-01 |
| Definições de diagnóstico | 2017-05-01-pré-visualização |
| Categorias de definições de diagnóstico | 2017-05-01-pré-visualização |


### <a name="microsoftkeyvault"></a>Microsoft.KeyVault

Gestão da chave de cofres dos, bem como as chaves, segredos e certificados em seus cofres de chaves. Para obter mais informações, consulte [referência de API de REST do Cofre de chaves do Azure](https://docs.microsoft.com/rest/api/keyvault/).

| Tipos de Recurso | Versões de API |
|-------------------------|--------------|
| Operações | 2016-10-01 |
| Cofres | 2016-10-01 |
| Cofres dos / políticas de acesso | 2016-10-01 |
| Vaults/secrets | 2016-10-01 |

### <a name="microsoftnetwork"></a>Microsoft.Network

Resultado da chamada de operações é uma representação da lista de operações de cloud de rede disponíveis. Para obter mais informações, consulte [operação REST API](https://docs.microsoft.com/rest/api/operation/).

| Tipos de Recurso | Versões de API |
|---------------------------|--------------|
| Ligações | 2015-06-15 |
| Zonas DNS | 2016-04-01 |
| Balanceador de Carga | 2015-06-15 |
| Gateway de Rede Local | 2015-06-15 |
| Localizações | 2016-04-01 |
| Location/operationResults | 2016-04-01 |
| Localizações/operações | 2016-04-01 |
| Localizações/utilizações | 2016-04-01 |
| Interfaces de Rede | 2015-06-15 |
| Grupos de Segurança de Rede | 2015-06-15 |
| Operações | 2015-06-15 |
| Endereço IP Público | 2015-06-15 |
| Tabelas de Rota | 2015-06-15 |
| Gateway de Rede Virtual | 2015-06-15 |
| Redes Virtuais | 2015-06-15 |

### <a name="microsoftresources"></a>Microsoft.Resources

O Azure Resource Manager permite-lhe implementar e gerir a infraestrutura para as suas soluções do Azure. Organizar os recursos relacionados em grupos de recursos e implementar os recursos com modelos JSON. Para obter uma introdução para implementar e gerir recursos com o Resource Manager, consulte [descrição geral do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

| Tipos de Recurso | Versões de API |
|-----------------------------------------|-------------------|
| Registos de aplicação | 2015-01-01 |
| Verificar Nome do Recurso | 2016-09-01 |
| Fornecedores delegados | 2015-01-01 |
| Fornecedores/ofertas delegadas | 2015-01-01 |
| DelegatedProviders/offers/estimatePrice | 2015-01-01 |
| Implementações | 2016-0209-01 |
| Implementações/operações | 2016-0209-01 |
| Metadados de extensões | 2015-01-01 |
| Ligações | 2016-09-01 |
| Localizações | 2015-01-01 |
| Ofertas | 2015-01-01 |
| Operações | 2015-01-01 |
| Fornecedores | 2015-012017-08-01 |
| Grupos de Recursos | 2016-09-01 |
| Recursos | 2016-09-01 |
| Subscrições | 2016-09-01 |
| Subscrições/localização | 2016-09-01 |
| Resultados de subscrições/operação | 2016-09-01 |
| Subscrições/fornecedores | 2015-012017-08-01 |
| Grupos de subscrições/recursos | 2016-09-01 |
| Subscriptions/resourceGroups/resources | 2016-09-01 |
| Subscrições/recursos | 2016-09-01 |
| Subscrições/tagNames | 2016-0609-01 |
| Subscriptions/tagNames/tagValues | 2016-0609-01 |
| Inquilinos | 2015-012017-08-01 |

### <a name="microsoftstorage"></a>Microsoft.Storage 

O fornecedor de recursos de armazenamento (SRP) permite-lhe gerir a sua conta de armazenamento e as chaves através de programação. Para obter mais informações, consulte [os referência do API de REST do fornecedor de recursos do Azure Storage](https://docs.microsoft.com/rest/api/storagerp/).

| Tipos de Recurso | Versões de API |
|-------------------------|--------------|
| Verificar Disponibilidade do Nome | 2016-01-01 |
| Localizações | 2016-01-01 |
| Locations/quotas | 2016-01-01 |
| Operações | 2016-01-01 |
| StorageAccounts | 2016-01-01 |
| Utilizações | 2016-01-01 |

## <a name="details-for-the-2017-03-09-profile"></a>Detalhes para o 2017-03-09-perfil

### <a name="microsoft-authorization"></a>Autorização da Microsoft

| Tipos de Recurso | Versões de API |
|---------------------|---------------------------------|
| Bloqueios | 2017-04-01 |
| Operações | 2015-07-01 |
| Permissões | 2015-07-01 |
| Atribuições de Política | 2016-12-01 (2017-06-01-pré-visualização) |
| Definições de Política | 2016-12-01 |
| Operações de fornecedor | 2015-07-01-pré-visualização |
| Atribuições de Funções | 2015-07-01 |
| Definições de funções | 2015-07-01 |

### <a name="microsoftcompute"></a>Microsoft.Compute

| Tipo de Recurso | Versão da API |
|---------------------------------------------------------------|-------------|
| Conjuntos de Disponibilidade | 2016-03-30 |
| Localizações | 2016-03-30 |
| Localizações/operações | 2016-03-30 |
| Localizações/editores | 2016-03-30 |
| Localizações/utilizações | 2016-03-30 |
| Localizações/vmSizes | 2016-03-30 |
| Operações | 2016-03-30 |
| Virtual Machines | 2016-03-30 |
| Extensões de máquinas virtuais / | 2016-03-30 |
| Conjuntos de Dimensionamento de Máquinas Virtuais | 2016-03-30 |
| Conjuntos de dimensionamento de máquinas virtuais/extensões | 2016-03-30 |
| Interfaces de rede/conjuntos de dimensionamento de máquinas virtuais | 2016-03-30 |
| Máquinas virtuais/conjuntos de dimensionamento de máquina virtual | 2016-03-30 |
| Conjuntos de dimensionamento de máquinas virtuais/virtualMachines/networkInterfaces | 2016-03-30 |

### <a name="microsoftnetwork"></a>Microsoft.Network

| Tipos de Recurso | Versões de API |
|---------------------------|--------------|
| Ligações | 2015-06-15 |
| Zonas DNS | 2016-04-01 |
| Balanceador de Carga | 2015-06-15 |
| Gateway de Rede Local | 2015-06-15 |
| Localizações | 2016-04-01 |
| Location/operationResults | 2016-04-01 |
| Localizações/operações | 2016-04-01 |
| Localizações/utilizações | 2016-04-01 |
| Interfaces de Rede | 2015-06-15 |
| Grupos de Segurança de Rede | 2015-06-15 |
| Operações | 2015-06-15 |
| Endereço IP Público | 2015-06-15 |
| Tabelas de Rota | 2015-06-15 |
| Gateway de Rede Virtual | 2015-06-15 |
| Redes Virtuais | 2015-06-15 |

### <a name="microsoftresources"></a>Microsoft.Resources

| Tipos de Recurso | Versões de API |
|-----------------------------------------|--------------|
| Registos de aplicação | 2015-01-01 |
| Verificar Nome do Recurso | 2016-09-01 |
| Fornecedores delegados | 2015-01-01 |
| Fornecedores/ofertas delegadas | 2015-01-01 |
| DelegatedProviders/offers/estimatePrice | 2015-01-01 |
| Implementações | 2016-09-01 |
| Implementações/operações | 2016-09-01 |
| Metadados de extensões | 2015-01-01 |
| Ligações | 2016-09-01 |
| Localizações | 2015-01-01 |
| Ofertas | 2015-01-01 |
| Operações | 2015-01-01 |
| Fornecedores | 2017-08-01 |
| Grupos de Recursos | 2016-09-01 |
| Recursos | 2016-09-01 |
| Subscrições | 2016-09-01 |
| Subscrições/localização | 2016-09-01 |
| Resultados de subscrições/operação | 2016-09-01 |
| Subscrições/fornecedores | 2017-08-01 |
| Grupos de subscrições/recursos | 2016-09-01 |
| Subscriptions/resourceGroups/resources | 2016-09-01 |
| Subscrições/recursos | 2016-09-01 |
| Subscriptiosn/tagNames | 2016-09-01 |
| Subscriptions/tagNames/tagValues | 2016-09-01 |
| Inquilinos | 2017-08-01 |

### <a name="microsoftstorage"></a>Microsoft.Storage

| Tipos de Recurso | Versões de API |
|-------------------------|--------------|
| Verificar Disponibilidade do Nome | 2016-01-01 |
| Localizações | 2016-01-01 |
| Locations/quotas | 2016-01-01 |
| Operações | 2016-01-01 |
| StorageAccounts | 2016-01-01 |
| Utilizações | 2016-01-01 |

## <a name="next-steps"></a>Passos Seguintes

* [Instalar o PowerShell para o Azure Stack](azure-stack-powershell-install.md)
* [Configurar o ambiente do PowerShell do utilizador do Azure Stack](azure-stack-powershell-configure-user.md)  
