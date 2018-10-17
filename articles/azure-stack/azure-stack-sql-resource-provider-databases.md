---
title: Utilizar bases de dados fornecidos pelo fornecedor de recursos de adaptador de SQL no Azure Stack | Documentos da Microsoft
description: Como criar e gerir bases de dados SQL aprovisionadas utilizando o fornecedor de recursos de adaptador de SQL
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
ms.openlocfilehash: 604e96d03d66cfde049316a1e6e99f07fee5032a
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49362382"
---
# <a name="create-sql-databases"></a>Criar bases de dados SQL

Pode criar e gerir bases de dados Self-serviços no portal de utilizador. Um utilizador do Azure Stack tem uma subscrição com uma oferta que inclui o serviço de base de dados SQL.

1. Inicie sessão para o [do Azure Stack](azure-stack-poc.md) portal de utilizador.

2. Selecione **+ nova** &gt; **dados + armazenamento** &gt; **base de dados do SQL Server** &gt; **adicionar**.

3. Sob **Create Database**, introduza as informações necessárias, tal como **nome da base de dados** e **tamanho máximo em MB**.

   >[!NOTE]
   >O tamanho da base de dados tem de ser, pelo menos, 64 MB, o que pode aumentar depois de implementar a base de dados.

   Configure as outras definições conforme necessário para o seu ambiente.

4. Sob **Create Database**, selecione **SKU**. Sob **selecionar um SKU**, selecione o SKU da base de dados.

   ![Criar base de dados](./media/azure-stack-sql-rp-deploy/newsqldb.png)

   >[!NOTE]
   >Servidores de alojamento são adicionadas ao Azure Stack, que lhes foram atribuídas um SKU. Bases de dados são criadas no conjunto de servidores num SKU de hospedagem.

5. Selecione **início de sessão**.
6. Sob **selecionar um início de sessão**, escolha um início de sessão existente ou selecione **+ criar um novo início de sessão**.
7. Sob **novo início de sessão**, introduza um nome para **início de sessão da base de dados** e um **palavra-passe**.

   >[!NOTE]
   >Estas definições estão a credencial de autenticação do SQL criada para o seu acesso a apenas esta base de dados. O nome de utilizador de início de sessão tem de ser globalmente exclusivo. Pode reutilizar definições de início de sessão para outros bancos de dados que utilizam o mesmo SKU.

   ![Criar um novo início de sessão de banco de dados](./media/azure-stack-sql-rp-deploy/create-new-login.png)

8. Selecione **OK** para concluir a implementação da base de dados.

Sob **Essentials**, que é apresentado depois da base de dados é implementado, anote o **cadeia de ligação**. Pode usar essa cadeia de caracteres em qualquer aplicativo que precise acessar o banco de dados do SQL Server.

![Obter a cadeia de ligação](./media/azure-stack-sql-rp-deploy/sql-db-settings.png)

## <a name="sql-always-on-databases"></a>SQL Always On bases de dados

Por design, sempre em bases de dados são processadas forma diferente do que num ambiente de servidor autónomo. Para obter mais informações, consulte [apresentando o SQL Server Always On grupos de disponibilidade em máquinas virtuais do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview).

### <a name="verify-sql-always-on-databases"></a>Verifique se o SQL Always On bases de dados

Captura de ecrã seguinte mostra como pode utilizar o SQL Server Management Studio para ver estado da base de dados no SQL Always On.

![Estado da base de dados AlwaysOn](./media/azure-stack-sql-rp-deploy/verifyalwayson.png)

Always On bases de dados devem mostrar como Synchronized e disponível em todas as instâncias SQL e aparecem nos grupos de disponibilidade. Na captura de ecrã anterior, o exemplo de base de dados é newdb1 e o respetivo estado é **newdb1 (sincronizados)**.

### <a name="delete-an-alwayson-database"></a>Eliminar uma base de dados do AlwaysOn

Ao eliminar uma base de dados do AlwaysOn de SQL do fornecedor de recursos, o SQL elimina da base de dados da réplica primária e do grupo de disponibilidade.

SQL, em seguida, coloca a base de dados no Estado a restaurar nas outras réplicas e não remover a base de dados, a menos que acionada. Se a base de dados não for removida, as réplicas secundárias entram num Estado não a sincronizar.

## <a name="next-steps"></a>Passos Seguintes

[Manter o fornecedor de recursos do SQL Server](azure-stack-sql-resource-provider-maintain.md)
