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
ms.date: 11/20/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: b79d64cc063105cb8ecce537a09a7f39a78eef4c
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52275032"
---
# <a name="remove-the-sql-resource-provider"></a>Remover o fornecedor de recursos do SQL

Antes de remover o fornecedor de recursos do SQL, tem de remover todas as dependências de fornecedor. Também terá uma cópia do pacote de implementação que foi utilizada para instalar o fornecedor de recursos.

> [!NOTE]
> Pode encontrar as ligações de transferência para o recurso de programas de instalação do fornecedor na [implementar os pré-requisitos do fornecedor de recursos](.\azure-stack-sql-resource-provider-deploy.md#prerequisites).

Remover o fornecedor de recursos do SQL não eliminar bases de dados do inquilino de alojar servidores.

## <a name="dependency-cleanup"></a>Limpeza de dependência

Existem várias tarefas de limpeza para o fazer antes de executar o script de DeploySqlProvider.ps1 para remover o fornecedor de recursos.

O operador de pilha do Azure é responsável pelas seguintes tarefas de limpeza:

* Elimine os planos que referenciam o adaptador de SQL.
* Elimine quaisquer quotas que estão associados com o adaptador de SQL.

## <a name="to-remove-the-sql-resource-provider"></a>Para remover o fornecedor de recursos do SQL

1. Certifique-se de que removeu todos as existentes SQL fornecedor dependências de recursos.

   > [!NOTE]
   > Desinstalar o fornecedor de recursos do SQL irá continuar mesmo que os recursos dependentes estiver a utilizar atualmente o fornecedor de recursos.
  
2. Obtenha uma cópia do pacote de instalação de fornecedor de recursos do SQL e, em seguida, execute o Self-extractor para extrair o conteúdo para um diretório temporário.

3. Abra uma janela de consola novo elevada do PowerShell e altere o diretório onde extraiu os ficheiros de instalação do fornecedor de recursos SQL.

4. Execute o script de DeploySqlProvider.ps1 utilizando os seguintes parâmetros:

    * **Desinstalar**. Remove o fornecedor de recursos e todos os recursos associados.
    * **PrivilegedEndpoint**. O endereço IP ou nome DNS do ponto final com privilégios.
    * **AzureEnvironment**. O ambiente do Azure utilizado para implementar o Azure Stack. Apenas necessário para implementações do Azure AD.
    * **CloudAdminCredential**. A credencial para o administrador da nuvem, necessária para aceder ao ponto final com privilégios.
    * **AzCredential**. A credencial da conta de administrador de serviço do Azure Stack. Utilize as mesmas credenciais que utilizou para a implementação do Azure Stack.

## <a name="next-steps"></a>Passos Seguintes

[Oferecer serviços de aplicações como PaaS](azure-stack-app-service-overview.md)
