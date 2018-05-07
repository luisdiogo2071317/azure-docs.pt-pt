---
title: Bases de dados SQL a utilizar na pilha do Azure | Microsoft Docs
description: Saiba como pode implementar bases de dados do SQL Server como um serviço na pilha do Azure e os passos rápidos para implementar o adaptador de fornecedor de recursos do SQL Server.
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
ms.date: 05/01/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: c2686a2d5241af46e70263d1827028aa7e9b2138
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="remove-the-sql-resource-provider"></a>Remover o fornecedor de recursos SQL

Para remover o fornecedor de recursos do SQL Server, é essencial primeiro de remover quaisquer dependências:

1. Certifique-se de que tem o pacote de implementação original que transferiu para esta versão do adaptador de fornecedor de recursos do SQL Server.

2. Devem ser apagadas todas as bases de dados de utilizador do fornecedor de recursos. (A eliminar as bases de dados do utilizador não elimina os dados.) Esta tarefa deve ser efetuada pelos utilizadores pessoalmente.

3. O administrador terá de eliminar os servidores de alojamento do adaptador de fornecedor de recursos do SQL Server.

4. O administrador tem de eliminar quaisquer planos que referenciam o adaptador de fornecedor de recursos do SQL Server.

5. O administrador tem de eliminar todos os SKUs e quotas que estão associadas a placa de fornecedor de recursos do SQL Server.

6. Volte a executar o script de implementação com os seguintes elementos:
    - -Desinstalar parâmetro
    - Os pontos finais do Azure Resource Manager
    - O DirectoryTenantID
    - As credenciais da conta de administrador de serviço

