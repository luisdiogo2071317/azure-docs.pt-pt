---
title: Limites do Azure de recursos e o objeto de Analysis Services | Documentos da Microsoft
description: Descreve os limites de recursos e o objeto do Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 09/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 4c2cebe2225e475ccd40460e7b10a6ba3ed428d5
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44724131"
---
# <a name="analysis-services-resource-and-object-limits"></a>Limites de recursos e o objeto de serviços de análise

Este artigo descreve os recursos e o modelo de objeto de limites.

## <a name="tier-limits"></a>Limites do escalão

### <a name="developer-tier"></a>Escalão de programador

Este escalão é recomendado para cenários de avaliação, desenvolvimento e teste. Um único plano inclui as mesmas funcionalidades do escalão standard, mas a capacidade de processamento, as QPUs e o tamanho da memória são limitados. O aumento horizontal de réplicas de consultas *não está disponível* para este escalão. Este escalão não oferece um SLA.

|Planear  |QPUs  |Memória (GB)  |
|---------|---------|---------|
|D1    |    20     |    3     |


### <a name="basic-tier"></a>Escalão Basic

Este escalão é recomendado para soluções de produção com modelos em tabela mais pequenos, simultaneidade de utilizadores limitada e requisitos simples de atualização de dados. O aumento horizontal de réplicas de consultas *não está disponível* para este escalão. As perspetivas, partições múltiplas e funcionalidades de modelos em tabela do DirectQuery *não são suportadas* neste escalão.  

|Planear  |QPUs  |Memória (GB)  |
|---------|---------|---------|
|B1    |    40     |    10     |
|B2    |    80     |    20     |

### <a name="standard-tier"></a>Escalão Standard

Este escalão destina-se a aplicações de produção críticas para o negócio, que requerem simultaneidade de utilizadores elástica e têm modelos de dados em rápido crescimento. Suporta a atualização de dados avançada para atualizações de modelos de dados quase em tempo real, bem como todas as funcionalidades de modelos em tabela.

|Planear  |QPUs  |Memória (GB)  |
|---------|---------|---------|
|S1    |    40     |    10     |
|S2    |    100     |    25     |
|S3    |    200     |    50     |
|S4    |    400     |    100     |
|S8*    |    320     |    200     |
|S9*    |    640    |    400     |

\* Não está disponível em todas as regiões.  

## <a name="object-limits"></a>Limites de objeto

Estes são os limites teóricas. Desempenho irá ser reduzido em números mais baixos.

|Object|Tamanhos de máximos/números|  
|------------|----------------------------|  
|Bases de dados numa instância|16,000|  
|Número combinado de tabelas e colunas numa base de dados|16,000|  
|Linhas numa tabela|Ilimitado<br /><br /> **Aviso:** com a restrição que não existe nenhuma coluna única na tabela pode ter mais de 1 999 999 997 valores distintos.|  
|Hierarquias numa tabela|15,999|  
|Níveis de uma hierarquia|15,999|  
|Relações|8,000|  
|Colunas de chave na tabela de todos os|15,999|  
|Medidas num tabelas|2 ^ 31-1 = 2 147 483 647|  
|Células devolvidas por uma consulta|2 ^ 31-1 = 2 147 483 647|  
|Tamanho de registro da consulta de origem|64K|  
|Comprimento dos nomes de objeto|512 carateres|  


