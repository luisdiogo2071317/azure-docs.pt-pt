---
title: Otimizar o tempo de consulta na base de dados do Azure para servidor PostgreSQL com a estratégia de armazenamento de tabela de alerta
description: Este artigo descreve como otimizar o tempo de consulta com a estratégia de armazenamento de tabela de alerta numa base de dados do Azure para o servidor PostgreSQL.
author: dianaputnam
ms.author: dianas
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: dee8aaaef4b1998a7234a88d07ad5efbc79d050b
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629302"
---
# <a name="optimizing-query-time-with-toast-table-storage-strategy"></a>Otimizar o tempo de consulta com a estratégia de armazenamento de tabela de alerta 
Este artigo descreve como otimizar os tempos de consultas com a estratégia de armazenamento de tabela do alerta.

## <a name="toast-table-storage-strategies"></a>Estratégias de armazenamento de tabela de alerta
Existem quatro estratégias diferentes para armazenar colunas capaz de alerta no disco que representa a várias combinações entre compactação e o armazenamento de fora de linha. A estratégia pode ser definida no nível de tipo de dados e ao nível da coluna.
- **Sem formatação** impede a compressão ou armazenamento fora de linha; além disso, ele desativa a utilização dos cabeçalhos de byte único para tipos de varlena. **Sem formatação** é a estratégia apenas possível para colunas de tipos de dados não consegue alerta.
- **Expandido** permite o armazenamento de compactação e fora de linha. **Expandido** é a predefinição para a maioria dos tipos de dados capaz de alerta. Compressão será tentado armazenamento primeiro, em seguida, fora de linha se a linha ainda é demasiado grande.
- **Externo** permite o armazenamento fora de linha, mas não a compressão. Usar **externo** fará com que operações de subcadeia no texto grande e bytea colunas com maior rapidez, na penalidade de maior espaço de armazenamento, uma vez que estas operações são otimizadas para obter apenas o necessário partes de fora de linha valor quando ele Não é compactado.
- **Main** permite a compactação, mas não fora de linha de armazenamento. Armazenamento de fora de linha será realizado para este tipo de colunas, mas apenas como último recurso quando não existe nenhuma outra forma de tornar a linha pequena o bastante para caber numa página.

## <a name="using-toast-table-storage-strategies"></a>Utilizar estratégias de armazenamento de tabela de alerta
Se as suas consultas aceder a tipos de dados capaz de alerta, considere a utilização **Main** em vez da predefinição **Extended** opção para reduzir os tempos de consulta. **Main** não impedir o armazenamento fora de linha. Por outro lado, se suas consultas não acedam a tipos de dados capaz de alerta, poderá ser vantajoso manter o **Extended** opção. Portanto, será ajustado uma maior parte das linhas da tabela principal na cache de memória intermédia partilhada, ajudando a desempenho.

Se tiver uma carga de trabalho com um esquema com tabelas largas e contagens de caracteres elevada, considere a utilização de tabelas de alerta do PostgreSQL. Um exemplo de tabela de cliente tinha mais de 350 colunas com várias colunas, que abrange a 255 carateres. Reduziu o seu tempo de consulta de parâmetro de comparação de 4203 segundos para segundos 467, um aprimoramento de 89%, depois de converter a estratégia de alerta do **Main**.

## <a name="next-steps"></a>Passos Seguintes
Reveja a sua carga de trabalho para as características acima. 

Reveja a seguinte documentação de PostgreSQL: [68 do capítulo, armazenamento físico do banco de dados](https://www.postgresql.org/docs/current/storage-toast.html) 