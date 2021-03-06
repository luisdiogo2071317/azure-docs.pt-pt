---
title: Azure notas de versão do fornecedor 1.1.30.0 de recursos do SQL de pilha | Documentos da Microsoft
description: Saiba mais sobre o que há na atualização mais recente do Azure Stack SQL recursos fornecedor, incluindo os problemas conhecidos e onde para baixá-lo.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2019
ms.author: jeffgilb
ms.reviewer: jiahan
ms.lastreviewed: 01/09/2019
ms.openlocfilehash: 4b9b91c17a9199c7890d17a2b721ce0803e6bd6c
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55243339"
---
# <a name="sql-resource-provider-11330-release-notes"></a>Notas de versão do fornecedor 1.1.33.0 de recursos do SQL

*Aplica-se a: Integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Estas notas de versão descrevem as melhorias e problemas conhecidos no SQL 1.1.33.0 de versão do fornecedor de recursos.

## <a name="build-reference"></a>Criar a referência
Transferir o fornecedor de recursos do SQL, binário e, em seguida, execute o Self-extractor para extrair o conteúdo para um diretório temporário. O fornecedor de recursos tem um mínimo correspondente do Azure Stack criar. A versão de lançamento do Azure Stack mínima necessária para instalar esta versão do fornecedor de recursos do SQL está listada abaixo:

> |Versão mínima do Azure Stack|Versão do fornecedor de recursos SQL|
> |-----|-----|
> |Versão 1808 (1.1808.0.97)|[SQL RP versão 1.1.33.0](https://aka.ms/azurestacksqlrp11330)|  
> |     |     |

> [!IMPORTANT]
> Aplicar a atualização com suporte do Azure Stack mínimo ao seu sistema integrado do Azure Stack ou implementar a mais recente do Azure Stack Development Kit (ASDK) antes de implementar a versão mais recente do fornecedor de recursos do SQL.

## <a name="new-features-and-fixes"></a>Novas funcionalidades e correções
Esta versão do fornecedor de recursos de SQL do Azure Stack inclui as seguintes melhorias e correções:

### <a name="fixes"></a>Correções
- **Extensão de portal do fornecedor de recursos do SQL, pode escolher a subscrição errada**. O fornecedor de recursos do SQL utiliza chamadas do Azure Resource Manager para determinar a primeira assinatura de administrador de serviço para utilizar, que podem não ser o *subscrição do fornecedor predefinido*. Se isto acontecer, o fornecedor de recursos do SQL não funciona normalmente. 

- **Do alojamento do SQL server não listar as bases de dados alojadas.** Bases de dados criada por utilizadores não poderão ser listadas ao visualizar os recursos de inquilino para alojar servidores SQL.

- **Implementação de fornecedor (1.1.30.0) de recursos SQL anterior poderá falhar se não estiver ativada TLS 1.2**. Atualizar o fornecedor de recursos do SQL 1.1.33.0 para ativar o TLS 1.2 ao implementar o fornecedor de recursos, a atualizar o fornecedor de recursos ou girando segredos. 

- **Falha de rotação secreta do fornecedor de recursos SQL**. Foi corrigido o problema resulta no seguinte código de erro quando alternar segredos: ` New-AzureRmResourceGroupDeployment - Error: Code=InvalidDeploymentParameterValue; Message=The value of deployment parameter 'StorageAccountBlobUri' is null.`

## <a name="known-issues"></a>Problemas conhecidos 

- **SKUs de SQL pode demorar até uma hora para ser visível no portal**. Pode demorar até uma hora para recentemente SKUs criados para ser visível para utilização durante a criação de novas bases de dados do SQL. 

    **Solução**: Nenhum.

- **Reutilizado inícios de sessão SQL**. Início de sessão com o mesmo nome de utilizador como um início de sessão existente na mesma subscrição tentar criar um novo SQL irá resultar em reutilizar o mesmo início de sessão e a palavra-passe existente. 

    **Solução**: Utilize nomes de utilizador diferente durante a criação de novos inícios de sessão na mesma subscrição ou criar inícios de sessão com o mesmo nome de utilizador em subscrições diferentes.

- **Inícios de sessão SQL partilhados inconsistências nos dados**. Se um início de sessão SQL é partilhado por várias bases de dados do SQL na mesma subscrição, alterar a palavra-passe de início de sessão irá provocar inconsistências nos dados.

    **Solução**: Utilize sempre os inícios de sessão diferentes para bases de dados diferentes na mesma subscrição.

- **Fornecedor de recursos do SQL não consegue adicionar SQL Server Always On de serviço de escuta**. Ao utilizar o endereço IP do serviço de escuta do SQL Server sempre no serviço de escuta, o fornecedor de recursos do SQL VM não é possível resolver o nome de anfitrião do serviço de escuta.

    **Solução**: Certifique-se de que o DNS funciona corretamente para resolver o IP do serviço de escuta para o nome de anfitrião do serviço de escuta.

### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Problemas conhecidos para os administradores de nuvem operacionais do Azure Stack
Consulte a documentação sobre o [notas de versão do Azure Stack](azure-stack-servicing-policy.md).

## <a name="next-steps"></a>Passos Seguintes
[Saiba mais sobre o fornecedor de recursos do SQL](azure-stack-sql-resource-provider.md).

[Preparar para implementar o fornecedor de recursos do SQL](azure-stack-sql-resource-provider-deploy.md#prerequisites).

[Atualize o fornecedor de recursos do SQL de uma versão anterior](azure-stack-sql-resource-provider-update.md). 