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
ms.date: 11/20/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: c3dbecfcaf40a85c57b9f795d7f2d9b76d27c195
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52274075"
---
# <a name="remove-the-mysql-resource-provider"></a>Remover o fornecedor de recursos do MySQL

Antes de remover o fornecedor de recursos do MySQL, tem de remover todas as dependências de fornecedor. Também terá uma cópia do pacote de implementação que foi utilizada para instalar o fornecedor de recursos.

> [!NOTE]
> Pode encontrar as ligações de transferência para o recurso de programas de instalação do fornecedor na [implementar os pré-requisitos do fornecedor de recursos](.\azure-stack-mysql-resource-provider-deploy.md#prerequisites).

Remover o fornecedor de recursos do MySQL não eliminar bases de dados do inquilino de alojar servidores.

## <a name="dependency-cleanup"></a>Limpeza de dependência

Existem várias tarefas de limpeza para o fazer antes de executar o script de DeployMySqlProvider.ps1 para remover o fornecedor de recursos.

O operador de pilha do Azure é responsável pelas seguintes tarefas de limpeza:

* Elimine os planos que referenciam o adaptador do MySQL.
* Elimine quaisquer quotas que estão associados com o adaptador do MySQL.

## <a name="to-remove-the-mysql-resource-provider"></a>Para remover o fornecedor de recursos do MySQL

1. Certifique-se de que removeu todos as existentes MySQL fornecedor dependências de recursos.

   > [!NOTE]
   > Desinstalar o fornecedor de recursos do MySQL irá continuar mesmo que os recursos dependentes estiver a utilizar atualmente o fornecedor de recursos.
  
2. Obtenha uma cópia do pacote de instalação do fornecedor de recursos MySQL e, em seguida, execute o Self-extractor para extrair o conteúdo para um diretório temporário.
3. Abra uma janela de consola novo elevada do PowerShell e altere o diretório onde extraiu os ficheiros de instalação do fornecedor de recursos MySQL.
4. Execute o script de DeployMySqlProvider.ps1 utilizando os seguintes parâmetros:
    - **Desinstalar**. Remove o fornecedor de recursos e todos os recursos associados.
    - **PrivilegedEndpoint**. O endereço IP ou nome DNS do ponto final com privilégios.
    - **AzureEnvironment**. O ambiente do Azure utilizado para implementar o Azure Stack. Apenas necessário para implementações do Azure AD.
    - **CloudAdminCredential**. A credencial para o administrador da nuvem, necessária para aceder ao ponto final com privilégios.
    - **DirectoryTenantID**
    - **AzCredential**. A credencial da conta de administrador de serviço do Azure Stack. Utilize as mesmas credenciais que utilizou para a implementação do Azure Stack.

## <a name="next-steps"></a>Passos Seguintes

[Oferecer serviços de aplicações como PaaS](azure-stack-app-service-overview.md)
