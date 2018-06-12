---
title: Remover o fornecedor de recursos do SQL Server na pilha do Azure | Microsoft Docs
description: Saiba como pode remover o fornecedor de recursos do SQL Server da sua implementação de pilha do Azure.
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
ms.date: 06/11/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 9f90201cad0f74923460c2f25eff4de98dc6690a
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294785"
---
# <a name="removing-the-mysql-resource-provider"></a>Remover o fornecedor de recursos de MySQL  
Antes de remover o fornecedor de recursos do SQL Server, é essencial primeiro de remover as dependências.

## <a name="remove-the-mysql-resource-provider"></a>Remover o fornecedor de recursos de MySQL 

1. Certifique-se de que tenha removido quaisquer dependências de fornecedor de recursos existentes do SQL Server.

  > [!NOTE]
  > Desinstalar o fornecedor de recursos do SQL Server irá continuar, mesmo se os recursos dependentes estão a utilizar atualmente o fornecedor de recursos. 
  
2. Certifique-se de que tem o pacote de implementação original que transferiu para esta versão do adaptador de fornecedor de recursos do SQL Server.
3. Volte a executar o script de implementação utilizando os seguintes parâmetros:
    - Utilize o - desinstalar parâmetro
    - O endereço IP ou nome DNS do ponto final com privilégios.
    - A credencial para o administrador da nuvem, necessária para aceder ao ponto final com privilégios.
    - As credenciais para a conta de administrador do serviço de pilha do Azure. Utilize as mesmas credenciais que utilizou para a implementação de pilha do Azure.

## <a name="next-steps"></a>Passos Seguintes
[Oferecer serviços aplicacionais como PaaS](azure-stack-app-service-overview.md)
