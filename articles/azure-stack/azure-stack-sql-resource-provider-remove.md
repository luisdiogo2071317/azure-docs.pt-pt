---
title: Remover o fornecedor de recursos do SQL Server na pilha do Azure | Microsoft Docs
description: Saiba como remover o fornecedor de recursos do SQL Server da sua implementação de pilha do Azure.
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
ms.date: 06/20/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 150d1c40463aa04527bdd6e356a4c24ef68b02ef
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301903"
---
# <a name="remove-the-sql-resource-provider"></a>Remover o fornecedor de recursos SQL

Antes de remover o fornecedor de recursos do SQL Server, tem de remover todas as dependências de fornecedor. Terá também uma cópia do pacote de implementação que foi utilizado para instalar o fornecedor de recursos.

## <a name="to-remove-the-sql-resource-provider"></a>Para remover o fornecedor de recursos SQL

1. Certifique-se de que removeu todas as existentes SQL recursos fornecedor dependências.

   > [!NOTE]
   > Desinstalar o fornecedor de recursos do SQL Server irá continuar, mesmo se os recursos dependentes estão a utilizar atualmente o fornecedor de recursos.
  
2. Obter uma cópia do fornecedor de recursos do SQL Server binário e, em seguida, execute o Self-extractor para extrair os conteúdos num diretório temporário.

3. Abra uma janela da consola do PowerShell new elevada e altere o diretório em que extraiu os ficheiros binários de fornecedor de recursos do SQL Server.

4. Execute o script de DeploySqlProvider.ps1 utilizando os seguintes parâmetros:

    - **Desinstalar**. Remove o fornecedor de recursos e recursos todos os associados.
    - **PrivilegedEndpoint**. O endereço IP ou nome DNS do ponto final com privilégios.
    - **CloudAdminCredential**. A credencial para o administrador da nuvem, necessária para aceder ao ponto final com privilégios.
    - **AzCredential**. A credencial para a conta de administrador do serviço de pilha do Azure. Utilize as mesmas credenciais que utilizou para a implementação de pilha do Azure.

## <a name="next-steps"></a>Passos Seguintes

[Oferecer serviços aplicacionais como PaaS](azure-stack-app-service-overview.md)
