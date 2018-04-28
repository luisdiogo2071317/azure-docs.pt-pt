---
title: Versões do fornecedor da API de recurso suportadas pelos perfis na pilha do Azure | Microsoft Docs
description: Saiba mais sobre a versão do Gestor de recursos do Azure suportada pelos perfis na pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: db01df21c95ee41197344cec719f1c2ab2dfc2ed
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="resource-provider-api-versions-supported-by-profiles-in-azure-stack"></a>Versões do fornecedor da API de recurso suportadas pelos perfis na pilha do Azure

Um fornecedor de recursos do Azure fornece recursos que pode implementar e gerir através do Gestor de recursos do Azure. Cada fornecedor oferece operações para trabalhar com recursos. Alguns fornecedores de recursos comuns incluem a Microsoft. Compute que fornece as máquinas virtuais, a Microsoft, que fornece recursos de conta de armazenamento, e a Microsoft. Web, que fornece recursos relacionados com as web apps. Para obter mais informações, veja [Resource providers and types](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services) (Tipos e fornecedores de recursos).

A tabela seguinte para cada fornecedor de recursos indica a versão suportada da versão de API de pilha do Azure ao utilizar perfis.

### <a name="microsoftauthorization"></a>Microsoft.Authorization

Utilize o controlo de acesso baseado em funções para gerir as ações de utilizadores na sua organização podem efetuar nos recursos. Este conjunto de operações permite-lhe definir funções, atribuir funções a utilizadores ou grupos e obter informações sobre as permissões. Para obter mais informações, consulte [autorização](https://docs.microsoft.com/rest/api/authorization/).

| Tipos de Recurso | Versões de API |
|---------------------|--------------------|
| Bloqueios | 2017-04-01 |
| Operações | 2015-07-01 |
| Permissões | 2015-07-01 |
| Atribuições de Política | 2016-12-01 (2017-06-01-pré-visualização) |
| Definições de Política | 2016-12-01 |
| Operações de fornecedor | 2015-07-01-preview |
| Atribuições de função | 2015-07-01 |
| Definições de função | 2015-07-01 |

### <a name="microsoftcommerce"></a>Microsoft.Commerce

| Tipo de Recurso | Versão de API |
|----------------------------------|----------------------|
| Subscrições de delegado do fornecedor | 2015-06-01 - pré-visualize |
| Utilização de delegados agregados | 2015-06-01 - pré-visualize |
| Recurso de estimativa gastam | 2015-06-01-pré-visualize |
| Operações | 2015-06-01 - pré-visualize |
| Agregados de utilização de subscritor | 2015-06-01 - pré-visualize |
| Agregados de Utilização | 2015-06-01 - pré-visualize |

### <a name="microsoftcompute"></a>Microsoft.Compute

As APIs de computação do Azure dão-lhe acesso programático para máquinas virtuais e os respetivos recursos de suporte. Para obter mais informações, consulte [computação do Azure](https://docs.microsoft.com/rest/api/compute/).

| Tipo de Recurso | Versão de API |
|---------------------------------------------------------------|-------------|
| Conjuntos de Disponibilidade | 2016-03-30 |
| Localizações | 2016-03-30 |
| Localizações/operações | 2016-03-30 |
| Localizações/editores | 2016-03-30 |
| Localizações/utilizações | 2016-03-30 |
| Localizações/vmSizes | 2016-03-30 |
| Operações | 2016-03-30 |
| Virtual Machines | 2016-03-30 |
| Máquinas virtuais/extensões | 2016-03-30 |
| Conjuntos de Dimensionamento de Máquinas Virtuais | 2016-03-30 |
| Conjuntos de dimensionamento da máquina virtual/extensões | 2016-03-30 |
| Interfaces de rede/conjuntos de dimensionamento da máquina virtual | 2016-03-30 |
| Máquinas virtuais/conjuntos de dimensionamento de máquina virtual | 2016-03-30 |
| Define/virtualMachines/networkInterfaces da escala de máquinas virtuais | 2016-03-30 |

### <a name="microsoftgallery"></a>Microsoft.Gallery

| Tipo de Recurso | Versão de API |
|------------------|-------------|
| Organização | 2015-04-01 |
| Conteúdo de Organização | 2015-04-01 |
| Extrato de Organização | 2015-04-01 |
| Itens de galeria | 2015-04-01 |
| Operações | 2015-04-01 |
| Portal | 2015-04-01 |
| Pesquisa | 2015-04-01 |
| Sugerir | 2015-04-01 |

### <a name="microsoftinsights"></a>Microsoft.Insights

| Tipos de Recurso | Versões de API |
|--------------------|--------------------|
| Regras de Alerta | 2016-03-01 |
| Categorias de eventos | 2017-03-01-preview |
| Tipos de Eventos | 2017-03-01-preview |
| Definições de Métricas | 2016-03-01 |
| Operações | 2015-04-01 |

### <a name="microsoftkeyvault"></a>Microsoft.KeyVault

Gerir a sua chave de cofres dos, bem como as chaves, os segredos e certificados dentro da sua cofres de chaves. Para obter mais informações, consulte [referência da API de REST do Cofre de chaves do Azure](https://docs.microsoft.com/rest/api/keyvault/).

| Tipos de Recurso | Versões de API |
|-------------------------|--------------|
| Operações | 2016-10-01 |
| cofres | 2016-10-01 |
| Cofres dos / políticas de acesso | 2016-10-01 |
| Vaults/secrets | 2016-10-01 |

### <a name="microsoftkeyvaultadmin"></a>Microsoft.Keyvault.Admin

Gerir a sua chave de cofres dos, bem como as chaves, os segredos e certificados dentro da sua cofres de chaves. Para obter mais informações, consulte [referência da API de REST do Cofre de chaves do Azure](https://docs.microsoft.com/rest/api/keyvault/).

| Tipos de Recurso | Versões de API |
|------------------|--------------------|
| Localizações | 2017-02-01-preview |
| Locations/quotas | 2017-02-01-preview |

### <a name="microsoftnetwork"></a>Microsoft.Network

Resultado da chamada de operações é uma representação de lista de operações de nuvem de rede disponíveis. Para obter mais informações, consulte [API de REST de operação](https://docs.microsoft.com/rest/api/operation/).

| Tipos de Recurso | Versões de API |
|---------------------------|--------------|
| Ligações | 2015-06-15 |
| Zonas DNS | 2016-04-01 |
| Balanceadores de Carga | 2015-06-15 |
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

O Azure Resource Manager permite-lhe implementar e gerir a infraestrutura para as suas soluções do Azure. Pode organizar os recursos relacionados em grupos de recursos e implementar os recursos com modelos JSON. Para uma introdução ao implementar e gerir recursos com o Resource Manager, consulte [descrição geral do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

| Tipos de Recurso | Versões de API |
|-----------------------------------------|-------------------|
| Registos de aplicações | 2015-01-01 |
| Verificar Nome do Recurso | 2015-012016-09-01 |
| Fornecedores de delegado | 2015-01-01 |
| Fornecedores de delegado/ofertas | 2015-01-01 |
| DelegatedProviders/offers/estimatePrice | 2015-01-01 |
| Implementações | 2016-0209-01 |
| Implementações/operações | 2016-0209-01 |
| Metadados de extensões | 2015-01-01 |
| Ligações | 2015-012016-09-01 |
| Localizações | 2015-01-01 |
| Ofertas | 2015-01-01 |
| Operações | 2015-01-01 |
| Fornecedores | 2015-012017-08-01 |
| Grupos de Recursos | 2015-012016-09-01 |
| Recursos | 2015-012016-09-01 |
| Subscrições | 2015-012016-09-01 |
| Subscrições/localização | 2015-012016-09-01 |
| Resultados de subscrições/operação | 2015-012016-09-01 |
| Subscrições/fornecedores | 2015-012017-08-01 |
| Grupos de recursos/subscrições | 2015-012016-09-01 |
| Subscriptions/resourceGroups/resources | 2015-012016-09-01 |
| Subscrições/recursos | 2015-012016-09-01 |
| Subscrições/tagNames | 2016-0609-01 |
| Subscriptions/tagNames/tagValues | 2016-0609-01 |
| Inquilinos | 2015-012017-08-01 |

### <a name="microsoftstorage"></a>Microsoft.Storage 

O fornecedor de recursos de armazenamento (SRP) permite-lhe gerir a sua conta de armazenamento e as chaves através de programação. Para obter mais informações, consulte [Azure armazenamento recursos fornecedor referência da API REST](https://docs.microsoft.com/rest/api/storagerp/).

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
* [Configurar o Azure pilha ambiente do utilizador do PowerShell](azure-stack-powershell-configure-user.md)  
