---
title: Armazenar cópias de segurança de SQL Database do Azure para até 10 anos | Microsoft Docs
description: Saiba como o SQL Database do Azure suporta armazenar cópias de segurança completa da base de dados até 10 anos.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 05/17/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: b2f3c454ba84c7b892096cc42dcbe2706ab6159f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34648297"
---
# <a name="store-azure-sql-database-backups-for-up-to-10-years"></a>Armazenar cópias de segurança de SQL Database do Azure para até 10 anos

Muitas aplicações terem regulamentação, conformidade ou de outras empresas fins que necessitam que sejam manter cópias de segurança da base de dados para além dos dias 7-35 fornecidas pelo SQL Database do Azure [cópias de segurança automáticas](sql-database-automated-backups.md). Ao utilizar a funcionalidade de retenção (imediatamente disponíveis) de longo prazo, pode armazenar especificadas SQL da base de dados cópias de segurança completas em [RA-GRS](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) armazenamento de BLOBs para até 10 anos. Em seguida, pode restaurar qualquer cópia de segurança como uma nova base de dados.

> [!IMPORTANT]
> Retenção de longa duração está atualmente em pré-visualização. Cópias de segurança existentes armazenadas no cofre do serviço de recuperação de serviços do Azure como parte da pré-visualização anterior desta funcionalidade são migradas para o armazenamento do SQL Azure.<!-- and available in the following regions: Australia East, Australia Southeast, Brazil South, Central US, East Asia, East US, East US 2, India Central, India South, Japan East, Japan West, North Central US, North Europe, South Central US, Southeast Asia, West Europe, and West US.-->
>

## <a name="how-sql-database-long-term-retention-works"></a>Como funciona a retenção de longo prazo de base de dados SQL

Retenção de cópias de segurança de longa duração tira partido do [cópias de segurança de base de dados SQL automáticas](sql-database-automated-backups.md) criado demora para que o restauro de momento (PITR). Pode configurar uma política de retenção de longo prazo para cada base de dados SQL e especifique a frequência tem de copiar as cópias de segurança para o armazenamento de longa duração. Para ativar essa flexibilidade, pode definir a política utilizando uma combinação de quatro parâmetros: retenção de cópias de segurança semanal (M), retenção de cópias de segurança mensal (M), a retenção de cópias de segurança anual (Y) e a semana do ano (WeekOfYear). Se especificar W, uma cópia de segurança todas as semanas será copiado para o armazenamento de longa duração. Se especificar M, uma cópia de segurança durante a primeira semana de cada mês será copiada para o armazenamento de longa duração. Se especificar Y, uma cópia de segurança durante a semana especificada pelo WeekOfYear será copiada para o armazenamento de longa duração. Cada cópia de segurança será mantida no armazenamento de longa duração para o período especificado por estes parâmetros. 

Exemplos:

-  W=0, M=0, Y=5, WeekOfYear=3

   A cópia de segurança completa 3rd de cada ano será mantida durante 5 anos.

- W=0, M=3, Y=0

   A primeira cópia de segurança completa de cada mês será mantida durante 3 meses.

- W=12, M=0, Y=0

   Cada cópia de segurança completa semanal será mantida para 12 semanas.

- W=6, M=12, Y=10, WeekOfYear=16

   Cada cópia de segurança completa semanal será mantida durante 6 semanas. Exceto a primeira cópia de segurança completa de cada mês, que serão mantidos durante 12 meses. Exceto a cópia de segurança completa tomada 16th semana do ano, que serão mantidos para 10 anos. 

A tabela seguinte ilustra a cadência e expiração de cópias de segurança de longo prazo para a seguinte política:

W = 12 semanas (84 dias), M = 12 meses (365 dias), Y = 10 anos (3650 dias), WeekOfYear = 15 (após 15 de Abril de semana)

   ![exemplo de imediatamente disponíveis](./media/sql-database-long-term-retention/ltr-example.png)


 
Se tiver que modifique a política acima e W = 0 (nenhum cópias de segurança semanais), a cadência de conjunto de cópias de segurança alteraria como mostrado na tabela acima pelas datas realçadas. A quantidade de armazenamento necessária para manter estas cópias de segurança seria reduzir em conformidade. 

> [!NOTE]
1. As cópias de imediatamente disponíveis são criadas pelo serviço de armazenamento do Azure, para que o processo de cópia não tenha nenhum impacto no desempenho na base de dados existente.
2. A política se aplica as cópias de segurança futuras. Por exemplo, Se o WeekOfYear especificado está no passado quando a política estiver configurada, será criada a primeira cópia de segurança imediatamente disponíveis ano seguinte. 
3. Para restaurar uma base de dados do armazenamento imediatamente disponíveis, pode selecionar uma cópia de segurança específica com base no respetivo carimbo.   É possível restaurar a base de dados para qualquer servidor existente na mesma subscrição, como a base de dados original. 
> 

## <a name="configure-long-term-backup-retention"></a>Configurar a retenção de cópias de segurança de longa duração

Para saber como configurar a retenção de longo prazo com o portal do Azure ou o PowerShell, consulte [configurar a retenção de cópias de segurança de longa duração](sql-database-long-term-backup-retention-configure.md).

## <a name="next-steps"></a>Passos Seguintes

Porque as cópias de segurança da base de dados proteger os dados de danos acidentais ou eliminação, se estiver a uma parte essencial dos quaisquer continuidade do negócio e a estratégia de recuperação após desastre. Para saber mais sobre as outras soluções de continuidade do negócio da base de dados do SQL Server, consulte [descrição geral da continuidade do negócio](sql-database-business-continuity.md).
