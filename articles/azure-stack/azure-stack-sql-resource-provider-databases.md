---
title: Utilizar bases de dados fornecidos pelo fornecedor de recursos de adaptador de SQL na pilha do Azure | Microsoft Docs
description: Como criar e gerir bases de dados SQL aprovisionados através da utilização do fornecedor de recursos de adaptador de SQL
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
ms.date: 06/18/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 56d21b76268f94f4254985a6924c4ca2d778a9cd
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36300827"
---
# <a name="create-sql-databases"></a>Criar bases de dados SQL

Pode criar e gerir bases de dados personalizados no portal de utilizador. Um utilizador de pilha do Azure tem uma subscrição com uma oferta, que inclui o serviço de base de dados do SQL Server.

1. Iniciar sessão para o [Azure pilha](azure-stack-poc.md) portal de utilizador.

2. Selecione **+ nova** &gt; **dados + armazenamento** &gt; **base de dados do SQL Server** &gt; **adicionar**.

3. Em **Create Database**, introduza as informações necessárias, tais como **nome de base de dados** e **tamanho máximo em MB**.

   >[!NOTE]
   >O tamanho de base de dados tem de ser, pelo menos, 64 MB, o que pode aumentar depois de implementar a base de dados.

   Configure as outras definições conforme necessário para o seu ambiente.

4. Em **Create Database**, selecione **SKU**. Em **selecionar um SKU**, selecione o SKU da base de dados.

   ![Criar base de dados](./media/azure-stack-sql-rp-deploy/newsqldb.png)

   >[!NOTE]
   >Servidores de alojamento são adicionadas à pilha do Azure, foram atribuídos um SKU. Bases de dados são criados no agrupamento de servidores de um SKU de alojamento.

5. Selecione **início de sessão**.
6. Em **selecionar um início de sessão**, escolha um início de sessão existente ou selecione **+ criar um novo início de sessão**.
7. Em **novo início de sessão**, introduza um nome para **início de sessão da base de dados** e um **palavra-passe**.

   >[!NOTE]
   >Estas definições estão a credencial de autenticação do SQL Server que é criada para o acesso ao apenas esta base de dados. O nome de utilizador de início de sessão tem de ser globalmente exclusivo. Pode reutilizar definições de início de sessão para outras bases de dados que utilizam o SKU do mesmo.

   ![Criar um novo início de sessão de base de dados](./media/azure-stack-sql-rp-deploy/create-new-login.png)

8. Selecione **OK** para concluir a implementação da base de dados.

Em **Essentials**, que é apresentado depois da base de dados é implementado, tome nota do **cadeia de ligação**. Pode utilizar esta cadeia de qualquer aplicação que necessita de aceder à base de dados do SQL Server.

![Obter a cadeia de ligação](./media/azure-stack-sql-rp-deploy/sql-db-settings.png)

## <a name="sql-always-on-databases"></a>SQL Always On bases de dados

Por predefinição, Always On bases de dados são processadas de forma que num ambiente de servidor autónomo. Para obter mais informações, consulte [introdução ao SQL Server Always On grupos de disponibilidade em máquinas virtuais do Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview).

### <a name="verify-sql-always-on-databases"></a>Verificar SQL Always On bases de dados

A seguinte captura de ecrã mostra como pode utilizar o SQL Server Management Studio para observar o estado da base de dados no SQL Always On.

![Estado de base de dados do AlwaysOn](./media/azure-stack-sql-rp-deploy/verifyalwayson.png)

Devem mostrar como sincronizado e disponível em todas as instâncias do SQL Server e são apresentados em grupos de disponibilidade Always On bases de dados. Na captura de ecrã anterior, o exemplo de base de dados é newdb1 e o respetivo estado é **newdb1 (sincronizar)**.

### <a name="delete-an-alwayson-database"></a>Eliminar uma base de dados do AlwaysOn

Quando elimina uma base de dados do AlwaysOn de SQL do fornecedor de recursos, o SQL Server elimina a base de dados da réplica primária e do grupo de disponibilidade.

SQL Server, em seguida, coloca a base de dados no Estado a restaurar nas outras réplicas e não remover a base de dados, a menos que acionado. Se a base de dados não for removida, as réplicas secundárias aceda num Estado não sincronizar.

## <a name="next-steps"></a>Passos Seguintes

[Manter o fornecedor de recursos do SQL Server](azure-stack-sql-resource-provider-maintain.md)
