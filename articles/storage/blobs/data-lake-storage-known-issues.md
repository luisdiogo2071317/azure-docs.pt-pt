---
title: Problemas conhecidos com geração 2 de armazenamento do Azure Data Lake | Documentos da Microsoft
description: Saiba mais sobre as limitações e problemas conhecidos relacionados com a geração 2 de armazenamento do Azure Data Lake
services: storage
author: normesta
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: normesta
ms.openlocfilehash: 83e9dfbe18dd79e8547e6b48daef39a5aed2cced
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52975285"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Problemas conhecidos relacionados com a geração 2 de armazenamento do Azure Data Lake

Este artigo contém problemas conhecidos e limitações temporárias com geração 2 de armazenamento do Azure Data Lake.

## <a name="api-interoperability"></a>Interoperabilidade de API

APIs de armazenamento de BLOBs e APIs de geração 2 do Azure Data Lake não são interoperáveis entre si.

Se precisar de utilizar a mesma ferramenta para trabalhar com todo o conteúdo que carregar para a sua conta, em seguida, não a ativar espaços de nomes hierárquicos na sua conta de armazenamento de BLOBs até que estas APIs tornam-se interoperável entre si. Com uma conta de armazenamento sem o espaço de nomes hierárquico significa que, em seguida, não tem acesso a recursos específicos de geração 2 de armazenamento do Data Lake, como o sistema de ficheiros e diretórios acessar listas de controlo.

## <a name="blob-storage-apis"></a>APIs de armazenamento de BLOBs

APIs de armazenamento de blob ainda não estão disponíveis para contas de armazenamento Gen 2 do Azure Data Lake.

Essas APIs estão desativadas para evitar problemas de acesso de dados acidentais que podem surgir como APIs de armazenamento de BLOBs não ainda são interoperáveis com APIs de geração 2 do Azure Data Lake.

Discos não geridos da Máquina Virtual (VM) dependem essas APIs, portanto, se pretender ativar espaços de nomes hierárquicos numa conta de armazenamento, considere colocar os discos VM não geridos para uma conta de armazenamento que não tem espaços de nomes hierárquicos ativados.

## <a name="azure-storage-explorer"></a>Explorador do Storage do Azure

Algumas funcionalidades do Explorador de armazenamento ainda não funcionam com os sistemas de ficheiros de geração 2 de armazenamento do Azure Data Lake. Essas limitações aplicam-se tanto a [versão autônoma](https://azure.microsoft.com/features/storage-explorer/) do Explorador de armazenamento do Azure, bem como a versão que aparece no portal do Azure.

## <a name="blob-viewing-tool"></a>Ferramenta de visualização de blob

Blob de visualização de ferramenta no portal do Azure tem apenas suporte limitado para geração 2 de armazenamento do Azure Data Lake.

## <a name="third-party-applications"></a>Aplicações de terceiros

Aplicações de terceiros podem não suportar a geração 2 de armazenamento do Azure Data Lake.

O suporte é ao critério de cada fornecedor de aplicações de terceiros. Atualmente, APIs de armazenamento de BLOBs e APIs de geração 2 de armazenamento do Azure Data Lake não pode ser utilizadas para gerir o mesmo conteúdo. Pois estamos a trabalhar para permitir que a interoperabilidade, é possível que muitas ferramentas de terceiros serão automaticamente suporta a geração 2 de armazenamento do Azure Data Lake.

## <a name="azcopy-support"></a>Suporte do AzCopy

AzCopy versão 8 não suporta a geração 2 de armazenamento do Azure Data Lake.

Em alternativa, utilize a versão de pré-visualização mais recente do AzCopy ( [AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json) ) por oferecer suporte a pontos finais de geração 2 de armazenamento do Azure Data Lake.

## <a name="oauth-authentication"></a>Autenticação OAuth

Serviços como o Azure Databricks, HDInsight e Azure Data Factory ainda não integram com a autenticação de token de portador do OAuth do Azure Active Directory (Azure AD).

## <a name="access-control-lists-acl"></a>Listas de controlo de acesso (ACL)

Diretório e listas de controle de acesso de nível de arquivo (ACL) são difíceis de gerir. Não há nenhuma ferramenta baseada na interface do Usuário que pode usar para obter e definir essas listas de controlo de acesso.

## <a name="azure-event-grid"></a>Azure Event Grid

[O Azure Event Grid](https://azure.microsoft.com/services/event-grid/) não receber eventos de contas de geração 2 do Azure Data Lake, porque essas contas ainda não gerá-los.  

## <a name="role-based-access-control"></a>Controlo de acesso baseado em funções

Não é possível aplicar o controlo de acesso baseado em funções aos objetos de sistema de ficheiros numa conta de geração 2 de armazenamento do Azure Data Lake.

## <a name="sql-data-warehouse-polybase"></a>PolyBase do SQL Data Warehouse

Quando os Firewalls de armazenamento estão ativadas numa conta de armazenamento do Azure, o SQL Data Warehouse [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide?view=sql-server-2017) não é possível aceder essas contas.

## <a name="soft-delete-and-snapshots"></a>Eliminação de forma recuperável e instantâneos

Eliminação de forma recuperável e instantâneos não estão disponíveis para contas de geração 2 de armazenamento do Azure Data Lake.

Todas as funcionalidades de controlo de versões incluindo [instantâneos](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) e [eliminação de forma recuperável](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) não ainda estão disponíveis para contas de armazenamento que têm espaços de nomes hierárquicos ativados.

## <a name="object-level-storage-tiers"></a>Camadas de armazenamento ao nível do objeto

Camadas de armazenamento ao nível do objeto (frequente, esporádico e arquivo) ainda não estão disponíveis para contas do Azure Data Lake Storage Gen 2, mas estão disponíveis para contas de armazenamento que não tenham espaços hierárquicos ativados.

## <a name="azure-blob-storage-lifecycle-management-preview-policies"></a>Políticas de gestão (pré-visualização) de ciclo de vida de armazenamento de Blobs do Azure

Políticas de gestão (pré-visualização) de ciclo de vida de armazenamento de Blobs do Azure ainda não estão disponíveis para contas de geração 2 de armazenamento do Azure Data Lake.

Estas políticas estão disponíveis para contas de armazenamento que não tenham espaços hierárquicos ativados.

## <a name="diagnostic-logs"></a>Registos de diagnósticos

Os registos de diagnóstico não estão disponíveis para contas de geração 2 de armazenamento do Azure Data Lake.

Para pedir os registos de diagnóstico, contacte o suporte do Azure. Disponibilize o seu nome de conta e o período de tempo para que os utilizadores precisam registos.
