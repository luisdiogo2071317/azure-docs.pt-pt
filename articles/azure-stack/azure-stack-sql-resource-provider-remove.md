---
title: Remover o fornecedor de recursos do SQL no Azure Stack | Documentos da Microsoft
description: Saiba como remover o fornecedor de recursos do SQL da sua implementação do Azure Stack.
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
ms.date: 10/16/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: f5aa67ad0588e3f42e68056c8ffca97767975e8b
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49361486"
---
# <a name="remove-the-sql-resource-provider"></a>Remover o fornecedor de recursos do SQL

Antes de remover o fornecedor de recursos do SQL, tem de remover todas as dependências de fornecedor. Também terá uma cópia do pacote de implementação que foi utilizada para instalar o fornecedor de recursos.

Existem várias tarefas de limpeza para o fazer antes de executar o _DeploySqlProvider.ps1_ script para remover o fornecedor de recursos.
Os inquilinos são responsáveis pelas seguintes tarefas de limpeza:

* Elimine todos os seus bancos de dados do fornecedor de recursos. (A eliminar as bases de dados do inquilino não elimina os dados.)
* Anular o registo do espaço de nomes de fornecedor de recursos.

O administrador é responsável pelas seguintes tarefas de limpeza:

* Elimina os servidores de alojamento do fornecedor de recursos do SQL.
* Elimina os planos que referenciam o fornecedor de recursos do SQL.
* Elimina quaisquer quotas que estão associados com o fornecedor de recursos do SQL.

## <a name="to-remove-the-sql-resource-provider"></a>Para remover o fornecedor de recursos do SQL

1. Certifique-se de que removeu todos as existentes SQL fornecedor dependências de recursos.

   > [!NOTE]
   > Desinstalar o fornecedor de recursos do SQL irá continuar mesmo que os recursos dependentes estiver a utilizar atualmente o fornecedor de recursos.
  
2. Obtenha uma cópia do fornecedor de recursos do SQL de binários e, em seguida, execute o Self-extractor para extrair o conteúdo para um diretório temporário.

3. Abra uma janela de consola novo elevada do PowerShell e altere o diretório onde extraiu arquivos binários de fornecedor de recursos do SQL.

4. Execute o script de DeploySqlProvider.ps1 utilizando os seguintes parâmetros:

    * **Desinstalar**. Remove o fornecedor de recursos e todos os recursos associados.
    * **PrivilegedEndpoint**. O endereço IP ou nome DNS do ponto final com privilégios.
    * **AzureEnvironment**. O ambiente do Azure utilizado para implementar o Azure Stack. Apenas necessário para implementações do Azure AD.
    * **CloudAdminCredential**. A credencial para o administrador da nuvem, necessária para aceder ao ponto final com privilégios.
    * **AzCredential**. A credencial da conta de administrador de serviço do Azure Stack. Utilize as mesmas credenciais que utilizou para a implementação do Azure Stack.

## <a name="next-steps"></a>Passos Seguintes

[Oferecer serviços de aplicações como PaaS](azure-stack-app-service-overview.md)
