---
title: Diferenças fundamentais do Machine Learning Services (com R) na descrição geral da base de dados do Azure SQL (pré-visualização)
description: Este tópico descreve as principais diferenças entre serviços do Azure SQL Database Machine Learning (com R) e serviços de Machine Learning do SQL Server.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning-services
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 01/31/2019
ms.openlocfilehash: adc303e9b375aaa6f37e9e79ea5434675c75523d
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824140"
---
# <a name="key-differences-between-machine-learning-services-in-azure-sql-database-and-sql-server"></a>Principais diferenças entre os serviços do Machine Learning na base de dados do Azure SQL e SQL Server

A funcionalidade do Machine Learning Services (com R) na Base de Dados SQL do Azure é semelhante ao [Machine Learning Services para SQL Server](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning). Seguem-se algumas diferenças fundamentais entre eles.

## <a name="language-support"></a>Suporte de idiomas

O SQL Server tem suporte para R e Python através da [estrutura de extensibilidade](https://docs.microsoft.com/en-us/sql/advanced-analytics/concepts/extensibility-framework). Base de dados SQL não suporta as duas linguagens. As principais diferenças são:

- R é o único idioma suportado na base de dados SQL. Ainda não há suporte para Python neste momento.
- A versão de R é 3.4.4.
- Não é necessário para configurar `external scripts enabled` via `sp_configure`. Assim que estiver [inscreveu](sql-database-machine-learning-services-overview.md#signup), de machine learning está ativada para a base de dados SQL.

## <a name="package-management"></a>Gestão de pacotes

Gestão de pacotes de R e instalação de trabalho diferentes entre a base de dados SQL e o SQL Server. Essas diferenças são:

- Pacotes de R são instalados por meio [sqlmlutils](https://github.com/Microsoft/sqlmlutils) ou [CREATE EXTERNAL LIBRARY](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql).
- Pacotes não é possível efetuar chamadas de rede de saída. Esta limitação é semelhante para o [predefinido de regras de firewall do Machine Learning Services](https://docs.microsoft.com//sql/advanced-analytics/security/firewall-configuration) no SQL Server, mas não pode ser alterado na base de dados do SQL.
- Não há suporte para pacotes que dependem de tempos de execução externos (como Java) ou precisam de acesso a APIs de sistema operacional para instalação ou utilização.

## <a name="resource-governance"></a>Governação de recursos

Não é possível limitar os recursos de R através de [Resource Governor](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) e agrupamentos de recursos externos. Recursos de R são uma percentagem de recursos da base de dados SQL e dependem em que camada de serviço que escolher. Para obter mais informações, consulte [modelos de compra do Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers).

## <a name="security-isolation"></a>Isolamento de segurança

Na base de dados SQL do Azure, a camada de Abstração de plataforma de SQL (SQLPAL) fornece isolamento de processos externos. Esse isolamento proporciona uma camada extra de segurança para executar R scripts.

## <a name="next-steps"></a>Passos Seguintes

- Consulte a [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics) documentação para obter informações gerais
- Para saber como utilizar o Machine Learning Services (com R) na base de dados do Azure SQL, veja [guia de início rápido](sql-database-connect-query-r.md).
- Saiba mais com [tutoriais da linguagem R do SQL Server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sql-server-r-tutorials)