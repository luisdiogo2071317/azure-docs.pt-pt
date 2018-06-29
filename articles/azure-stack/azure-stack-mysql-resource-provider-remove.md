---
title: Remover o fornecedor de recursos de MySQL no Azure pilha | Microsoft Docs
description: Saiba como pode remover o fornecedor de recursos de MySQL da implementação do Azure pilha.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: d3a615e3b92a62709a787d0463dfa3148f14d07e
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37085805"
---
# <a name="remove-the-mysql-resource-provider"></a>Remover o fornecedor de recursos de MySQL

Antes de remover o fornecedor de recursos do MySQL, tem de remover todas as dependências de fornecedor. Terá também uma cópia do pacote de implementação que foi utilizado para instalar o fornecedor de recursos.

## <a name="dependency-cleanup"></a>Limpeza de dependência

Existem várias tarefas de limpeza para o fazer antes de executar o script de DeployMySqlProvider.ps1 para remover o fornecedor de recursos.

Os inquilinos são responsáveis pelas seguintes tarefas de limpeza:

* Elimine todas as bases de dados do fornecedor de recursos. (A eliminar as bases de dados do inquilino não elimina os dados.)
* Anular o registo do espaço de nomes do fornecedor.

O administrador é responsável pelas seguintes tarefas de limpeza:

* Elimina os servidores de alojamento da placa de MySQL.
* Elimina quaisquer planos que referenciam o adaptador de MySQL.
* Elimina quaisquer quotas que estão associados a placa de MySQL.

## <a name="to-remove-the-mysql-resource-provider"></a>Para remover o fornecedor de recursos de MySQL

1. Certifique-se de que removeu todas as existentes MySQL recursos fornecedor dependências.

   >[!NOTE]
   >Desinstalar o fornecedor de recursos de MySQL irá continuar, mesmo se os recursos dependentes estão a utilizar atualmente o fornecedor de recursos.
  
2. Obter uma cópia do fornecedor de recursos de MySQL binário e, em seguida, execute o Self-extractor para extrair os conteúdos num diretório temporário.
3. Obter uma cópia do fornecedor de recursos do SQL Server binário e, em seguida, execute o Self-extractor para extrair os conteúdos num diretório temporário.
4. Abra uma janela da consola do PowerShell new elevada e altere o diretório onde extraiu os MySQL recurso binários os ficheiros do fornecedor.
5. Execute o script de DeployMySqlProvider.ps1 utilizando os seguintes parâmetros:
    - **Desinstalar**. Remove o fornecedor de recursos e recursos todos os associados.
    - **PrivilegedEndpoint**. O endereço IP ou nome DNS do ponto final com privilégios.
    - **CloudAdminCredential**. A credencial para o administrador da nuvem, necessária para aceder ao ponto final com privilégios.
    - **DirectoryTenantID**
    - **AzCredential**. A credencial para a conta de administrador do serviço de pilha do Azure. Utilize as mesmas credenciais que utilizou para a implementação de pilha do Azure.

## <a name="next-steps"></a>Passos Seguintes

[Oferecer serviços aplicacionais como PaaS](azure-stack-app-service-overview.md)
