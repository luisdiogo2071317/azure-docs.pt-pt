---
title: "Resolução de problemas para eventos de entrada com formato incorreto no Azure Stream Analytics | Microsoft Docs"
description: "Como saber qual o evento na minha entrada dados está a causar o problema de uma tarefa de Stream Analytics"
keywords: 
documentationcenter: 
services: stream-analytics
author: SnehaGunda
manager: Kfile
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/05/2018
ms.author: sngun
ms.openlocfilehash: 6b6c154568fe97b7495ae70dc162dc475169afea
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="common-issues-in-stream-analytics-and-steps-to-troubleshoot"></a>Problemas comuns do Stream Analytics e passos para resolver problemas

## <a name="troubleshoot-for-malformed-input-events"></a>Resolução de problemas para eventos de entrada com formato incorreto

Quando o fluxo de entrada da tarefa do Stream Analytics contém mensagens mal formadas, causar problemas de serialização. Mensagens mal formadas incluem serialização incorreto, tais como parênteses em falta num objeto JSON ou formato de carimbo de data / hora incorretos. Quando uma tarefa de Stream Analytics recebe uma mensagem incorretamente formada, ignora a mensagem e notifica o utilizador com um aviso. Um símbolo de aviso é apresentado no **entradas** mosaico da tarefa do Stream Analytics (esta sessão de aviso existe, desde que a tarefa está em estado de execução):

![Entradas do mosaico](media/stream-analytics-malformed-events/inputs_tile.png)

Pode ativar os registos de diagnóstico para ver os detalhes sobre o aviso. Para eventos de entrada malformatted, os registos de execução contém uma entrada com a mensagem que se assemelha: "mensagem: não foi possível anular a serialização do evento de entrada do recurso <blob URI> como json)". 

### <a name="troubleshooting-steps"></a>Passos de resolução de problemas

1. Navegue para o mosaico de entrada e clique para ver os avisos.
2. O mosaico de entrada de detalhes apresenta um conjunto de avisos com detalhes sobre o problema. Segue-se uma mensagem de aviso de exemplo, a mensagem de aviso mostra a partição, o desvio e números de sequência em que contém dados JSON com formato incorreto. 

   ![Mensagem de aviso com desvio igual a](media/stream-analytics-malformed-events/warning_message_with_offset.png)

3. Para obter os dados JSON tem um formato incorreto, execute o código de CheckMalformedEvents.cs, pode obter o it do [repositório do GitHub exemplos](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH). Este código lê a Id de partição de deslocamento e imprime os dados que estão localizados em que o desvio. 

4. Depois de ler os dados, pode analisar e corrija o formato de serialização. 

## <a name="handling-duplicate-records-when-using-azure-sql-database-as-output-for-a-stream-analytics-job"></a>Processamento de registos duplicados quando utilizar a SQL Database do Azure como saída para uma tarefa de Stream Analytics

Quando configura a base de dados SQL do Azure como saída para uma tarefa de Stream Analytics, mesmo em massa insere registos na tabela de destino. Em geral, do Azure stream analytics garante que [entrega, pelo menos, uma vez]( https://msdn.microsoft.com/azure/stream-analytics/reference/event-delivery-guarantees-azure-stream-analytics) para o sink de saída, um pode ainda [alcançar exatamente-uma vez entrega]( https://blogs.msdn.microsoft.com/streamanalytics/2017/01/13/how-to-achieve-exactly-once-delivery-for-sql-output/) para SQL Server de saída quando a tabela SQL tem uma restrição exclusiva definida. 

Depois de restrições de chave exclusivas são configuração na tabela de SQL e existem registos duplicados, que está a ser inseridos na tabela SQL, o Azure Stream Analytics remove o registo duplicado dividindo os dados em lotes e recursivamente inserir os lotes até um único registo duplicado foi encontrado. Se existirem considerável número de linhas duplicadas no seu pipeline, dividir e recursivamente a inserir dados, a ignorar duplicados um de cada é um processo moroso. Se vir várias mensagens de aviso de violação da chave no registo de atividades dentro da hora anterior, é provável que a saída do SQL Server é abrandamento a tarefa de toda. Para resolver este problema, deve [configurar o índice]( https://docs.microsoft.com/sql/t-sql/statements/create-index-transact-sql) que está a causar a violação de chave ao ativar a opção IGNORE_DUP_KEY. A ativação desta opção permite especificar valores duplicados e ignorados pelo SQL durante inserções em massa e SQL Azure simplesmente produz uma mensagem de aviso em vez de um erro. O Azure Stream Analytics não produz já erros de violação da chave primária.

Quando configurar IGNORE_DUP_KEY para vários tipos de índices, tenha em atenção as de observações seguintes:

* Não é possível definir IGNORE_DUP_KEY numa chave primária ou uma restrição exclusiva que utilize ALTER INDEX, terá de remover e recriar o índice.  
* Pode definir a opção IGNORE_DUP_KEY utilizando ALTER INDEX para um índice exclusivo, que é diferente da restrição de chave primária/exclusivos e criado com a definição de índice ou CREATE INDEX.  
* IGNORE_DUP_KEY não se aplicar aos índices de arquivo de colunas não é possível impor a exclusividade nessas índices.  

## <a name="next-steps"></a>Passos Seguintes

* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

