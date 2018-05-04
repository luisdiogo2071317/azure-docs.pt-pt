---
title: A encriptação transparente de dados no SQL Data Warehouse (Portal) | Microsoft Docs
description: Encriptação de dados transparente (TDE) no SQL Data Warehouse
services: sql-data-warehouse
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 9c4abb0416acc656a4cfae332377c398260191de
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Começar com transparente dados encriptação (TDE) no SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Descrição geral de segurança](sql-data-warehouse-overview-manage-security.md)
> * [Autenticação](sql-data-warehouse-authentication.md)
> * [Encriptação (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Encriptação (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a>Permssions necessária
Para ativar a encriptação de dados transparente (TDE), tem de ser um administrador ou membro da função dbmanager.

## <a name="enabling-encryption"></a>Ativar a encriptação
Para ativar o TDE para um SQL Data Warehouse, siga os passos abaixo:

1. Abrir a base de dados no [portal do Azure](https://portal.azure.com)
2. No painel da base de dados, clique o **definições** botão
3. Selecione o **encriptação transparente de dados** opção ![][1]
4. Selecione o **no** definição ![][2]
5. Selecione **guardar**
   ![][3]  

## <a name="disabling-encryption"></a>A desativação da encriptação
Para desativar o TDE para um SQL Data Warehouse, siga os passos abaixo:

1. Abrir a base de dados no [portal do Azure](https://portal.azure.com)
2. No painel da base de dados, clique o **definições** botão
3. Selecione o **encriptação transparente de dados** opção ![][1]
4. Selecione o **desativar** definição ![][4]
5. Selecione **guardar**
   ![][5]  

## <a name="encryption-dmvs"></a>Encriptação DMVs
Encriptação pode ser confirmada com DMVs os seguintes:

* [sys.databases]
* [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
