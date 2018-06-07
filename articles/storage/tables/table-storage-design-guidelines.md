---
title: Estrutura de tabela diretrizes para armazenamento do Azure | Microsoft Docs
description: Estruture o seu serviço de tabela do Azure para suportar operações de leitura de forma eficiente.
services: storage
documentationcenter: na
author: SnehaGunda
manager: kfile
ms.assetid: 8e228b0c-2998-4462-8101-9f16517393ca
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/23/2018
ms.author: sngun
ms.openlocfilehash: 5329d33aee1bb1a55e9982b1ba9e3e8329246980
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34661044"
---
# <a name="guidelines-for-table-design"></a>Diretrizes para estruturar tabela

Conceber a tabelas para utilização com o serviço de tabela de armazenamento do Azure é muito diferente de considerações de design para uma base de dados relacional. Este artigo descreve diretrizes para conceber a sua solução de serviço tabela para ser lidos eficiente e escrever eficiente.

## <a name="design-your-table-service-solution-to-be-read-efficient"></a>Estruturar a solução de serviço tabela para ser eficiente de leitura

* ***Estrutura de consulta em aplicações pesado de leitura.*** Ao conceber a sua tabelas, tenha em consideração as consultas (especialmente a latência aqueles confidenciais) que será executado antes de pensar sobre como atualizar as entidades. Normalmente, isto resulta numa solução eficiente e performant.  
* ***Especifique PartitionKey e RowKey nas suas consultas.*** *Ponto de consultas* como estas são as consultas de serviço tabela mais eficientes.  
* ***Considere armazenar cópias duplicadas de entidades.*** O Table storage é cheap, por isso, considere armazenar a mesma entidade várias vezes (com chaves diferentes) para ativar as consultas mais eficientes.  
* ***Considere denormalizing os seus dados.*** O Table storage é cheap por isso considere denormalizing os seus dados. Por exemplo, armazene entidades de resumidas para que as consultas para agregadas dados só precisam de aceder a uma única entidade.  
* ***Utilize os valores da chave compostos.*** As chaves só tem são **PartitionKey** e **RowKey**. Por exemplo, utilize os valores da chave compostos Ativar caminhos de acesso codificada alternativo para entidades.  
* ***Utilize projeção da consulta.*** Pode reduzir a quantidade de dados que são transferidos através da rede através de consultas que selecionar apenas os campos que precisa.  

## <a name="design-your-table-service-solution-to-be-write-efficient"></a>Estruturar a solução de serviço tabela para ser eficiente de escrita  

* ***Não crie partições frequente.*** Escolha as chaves que lhe permite distribuídos os pedidos por várias partições em qualquer ponto de tempo.  
* ***Evite picos de tráfego.*** Smooth o tráfego durante um período de tempo razoável e evitar picos de tráfego.
* ***Necessariamente não crie uma tabela separada para cada tipo de entidade.*** Quando necessitar de transações atómicas em tipos de entidade, pode armazenar estes vários tipos de entidade na mesma partição na mesma tabela.
* ***Considere o débito máximo que deve realizar.*** Tem de ter em consideração os destinos de escalabilidade para o serviço de tabela e certifique-se de que a estrutura não fará com que excedesse-los.  

Como ler este guia, irá ver exemplos que colocar todos estes princípios em prática. 

## <a name="next-steps"></a>Passos Seguintes

- [Padrões de conceção da tabela](table-storage-design-patterns.md)
- [Conceção para consultas](table-storage-design-for-query.md)
- [Encriptar dados da tabela](table-storage-design-encrypt-data.md)
- [Estrutura de modificação de dados](table-storage-design-for-modification.md)