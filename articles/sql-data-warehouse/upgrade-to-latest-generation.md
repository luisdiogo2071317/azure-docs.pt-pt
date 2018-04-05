---
title: Atualizar para a geração de mais recente do Azure SQL Data Warehouse | Microsoft Docs
description: Passos para atualizar o armazém de dados SQL do Azure para a geração mais recente da arquitetura de hardware e de armazenamento do Azure.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.services: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/02/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 42b716274e655bf91f72c1b3ab207b8a5f1ccee0
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="upgrade-to-latest-generation-of-azure-sql-data-warehouse-in-the-azure-portal"></a>Atualizar para a geração de mais recente do Azure SQL Data Warehouse no portal do Azure

Utilize o portal do Azure para atualizar o armazém de dados SQL do Azure para utilizar a geração da arquitetura de hardware e de armazenamento do Azure mais recente. Ao atualizar, pode tirar partido de um desempenho mais rápido, maior escalabilidade e armazenamento ilimitado para índices columnstore.  

## <a name="applies-to"></a>Aplica-se a
Esta atualização aplica-se para armazéns de dados no Optimized para a camada de desempenho de elasticidade.  As instruções atualizar um armazém de dados de Optimized para a camada de desempenho de elasticidade para Optimized para a camada de desempenho de computação. 

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="before-you-begin"></a>Antes de começar

1. Se o Optimized elasticidade do armazém de dados a ser atualizado está em pausa, [retomar o armazém de dados](pause-and-resume-compute-portal.md).
2. Estar preparadas para alguns minutos de indisponibilidade. 
3. O processo de atualização inutilizam todas as sessões e remove todas as ligações. Antes de atualizar, certifique-se de que concluíram as suas consultas. Se iniciar uma atualização com transações em curso, o tempo de reversão pode ser mais extenso. 

## <a name="start-the-upgrade"></a>Iniciar a atualização

1. Abra o seu armazém de dados no portal do Azure e clique em **atualização Optimized da computação**.
2. Tenha em atenção o Optimized para opções de camada de desempenho de computação. A seleção predefinida é comparável para o nível atual antes da atualização.
3. Escolha um escalão de desempenho. O preço de Optimized para a camada de desempenho de computação está atualmente half-desativada durante o período de pré-visualização.
4. Clique em **atualizar**.
5. Verifique o estado no portal do Azure.
6. Aguarde que o armazém de dados alterar para Online.

## <a name="rebuild-columnstore-indexes"></a>Reconstruir índices columnstore

Assim que o armazém de dados estiver online, pode carregar os dados e executar consultas. No entanto, o desempenho pode ser lenta em primeiro lugar, porque um processo em segundo plano está a migrar os dados para o novo hardware. 

Para forçar os dados para migrar como rapidamente quanto possível, recomendamos que reconstruir os índices columnstore. Para tal, consulte as orientações para [reconstruir índices columnstore para melhorar a qualidade de segmento](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality). 

## <a name="next-steps"></a>Passos Seguintes
O armazém de dados está online. Para utilizar as novas funcionalidades de desempenho, consulte [classes de recursos para a gestão de carga de trabalho](resource-classes-for-workload-management.md).
 