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
ms.date: 09/13/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: cd0195796189158650c9c2655062950b71130ad7
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45578485"
---
# <a name="remove-the-mysql-resource-provider"></a>Remover o fornecedor de recursos do MySQL

Antes de remover o fornecedor de recursos do MySQL, tem de remover todas as dependências de fornecedor. Também terá uma cópia do pacote de implementação que foi utilizada para instalar o fornecedor de recursos.

## <a name="dependency-cleanup"></a>Limpeza de dependência

Existem várias tarefas de limpeza para o fazer antes de executar o script de DeployMySqlProvider.ps1 para remover o fornecedor de recursos.

Os inquilinos são responsáveis pelas seguintes tarefas de limpeza:

* Elimine todos os seus bancos de dados do fornecedor de recursos. (A eliminar as bases de dados do inquilino não elimina os dados.)
* Anular o registo do espaço de nomes do fornecedor.

O administrador é responsável pelas seguintes tarefas de limpeza:

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
