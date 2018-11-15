---
title: Remover o fornecedor de recursos do MySQL no Azure Stack | Documentos da Microsoft
description: Saiba como pode remover o fornecedor de recursos do MySQL da sua implementação do Azure Stack.
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
ms.date: 11/14/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: 7d3b0e179972464a1ed857c576ca8a7c8fc2e162
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51686811"
---
# <a name="remove-the-mysql-resource-provider"></a>Remover o fornecedor de recursos do MySQL

Antes de remover o fornecedor de recursos do MySQL, tem de remover todas as dependências de fornecedor. Também terá uma cópia do pacote de implementação que foi utilizada para instalar o fornecedor de recursos.

  |Versão mínima do Azure Stack|Versão de MySQL RP|
  |-----|-----|
  |Versão 1808 (1.1808.0.97)|[MySQL RP versão 1.1.30.0](https://aka.ms/azurestacksqlrp11300)|
  |Versão 1804 (1.0.180513.1)|[MySQL RP versão 1.1.24.0](https://aka.ms/azurestackmysqlrp11240)
  |     |     |

## <a name="dependency-cleanup"></a>Limpeza de dependência

Existem várias tarefas de limpeza para o fazer antes de executar o script de DeployMySqlProvider.ps1 para remover o fornecedor de recursos.

Os utilizadores de inquilino do Azure Stack são responsáveis pelas seguintes tarefas de limpeza:

* Elimine todos os seus bancos de dados do fornecedor de recursos. (A eliminar as bases de dados do inquilino não elimina os dados.)
* Anular o registo do espaço de nomes do fornecedor.

O operador de pilha do Azure é responsável pelas seguintes tarefas de limpeza:

* Elimina os servidores de hospedagem do adaptador de MySQL.
* Elimina os planos que referenciam o adaptador do MySQL.
* Elimina quaisquer quotas que estão associados com o adaptador do MySQL.

## <a name="to-remove-the-mysql-resource-provider"></a>Para remover o fornecedor de recursos do MySQL

1. Certifique-se de que removeu todos as existentes MySQL fornecedor dependências de recursos.

   >[!NOTE]
   >Desinstalar o fornecedor de recursos do MySQL irá continuar mesmo que os recursos dependentes estiver a utilizar atualmente o fornecedor de recursos.
  
2. Obtenha uma cópia do fornecedor de recursos MySQL binária e, em seguida, execute o Self-extractor para extrair o conteúdo para um diretório temporário.
3. Obtenha uma cópia do fornecedor de recursos do SQL de binários e, em seguida, execute o Self-extractor para extrair o conteúdo para um diretório temporário.
4. Abra uma janela de consola novo elevada do PowerShell e altere o diretório onde extraiu arquivos binários de fornecedor de recursos do MySQL.
5. Execute o script de DeployMySqlProvider.ps1 utilizando os seguintes parâmetros:
    - **Desinstalar**. Remove o fornecedor de recursos e todos os recursos associados.
    - **PrivilegedEndpoint**. O endereço IP ou nome DNS do ponto final com privilégios.
    - **AzureEnvironment**. O ambiente do Azure utilizado para implementar o Azure Stack. Apenas necessário para implementações do Azure AD.
    - **CloudAdminCredential**. A credencial para o administrador da nuvem, necessária para aceder ao ponto final com privilégios.
    - **DirectoryTenantID**
    - **AzCredential**. A credencial da conta de administrador de serviço do Azure Stack. Utilize as mesmas credenciais que utilizou para a implementação do Azure Stack.

## <a name="next-steps"></a>Passos Seguintes

[Oferecer serviços de aplicações como PaaS](azure-stack-app-service-overview.md)
