---
title: Descrição geral da biblioteca do Cosmos BulkExecutor de BD do Azure | Microsoft Docs
description: Saiba mais sobre a biblioteca do Azure Cosmos DB BulkExecutor vantagens de utilizar a biblioteca e respetiva arquitetura.
keywords: Executor do Java em massa
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: ramkris
ms.openlocfilehash: 5f1987009d2277590e32136336340aa1b3be07fd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34610347"
---
# <a name="azure-cosmos-db-bulkexecutor-library-overview"></a>Descrição geral da biblioteca do Cosmos BulkExecutor de BD do Azure
 
BD do Cosmos do Azure é um serviço de base de dados rápido, flexível e globalmente distribuída que foi concebido para aprovisionadas de escalamento horizontal para suportar: 

* Grande de leitura e escrita throughputs (milhões de operações por segundo).  
* Armazenamento de volumes elevados de (centenas de terabytes ou até mesmo mais) transacionais e operacionais dados com uma latência de milissegundo previsível.  

A biblioteca de BulkExecutor ajuda-o a tirar partido do débito intenso e o armazenamento, o BulkExecutor biblioteca permite-lhe efetuar em massa operações do BD Azure Cosmos através da importação em volume e em massa atualizar APIs. Pode ler mais sobre as funcionalidades da biblioteca de BulkExecutor nas secções seguintes. 

> [!NOTE] 
> Atualmente, Bulkxecutor biblioteca suporta a importação e operações de atualização e esta biblioteca é suportado pela apenas contas do Azure Cosmos BD SQL API. Consulte [.NET](sql-api-sdk-bulk-executor-dot-net.md) e [Java](sql-api-sdk-bulk-executor-java.md) notas de versão do quaisquer atualizações para a biblioteca.
 
## <a name="key-features-of-the-bulkexecutor-library"></a>Principais funcionalidades da biblioteca BulkExecutor  
 
* Esta opção reduz significativamente os recursos de computação do lado do cliente necessários para sob saturar ao débito atribuído a um contentor. Uma única aplicação threading escreve dados utilizando que a API de importação em massa distribui 10 vezes o débito de escrita superior quando comparado com uma aplicação multi-thread que escreve dados em paralelo, ao saturating o cliente de CPU da máquina.  

* Deduz, ausente, as tarefas tedious de escrever a lógica de aplicação para processar o pedido de limitação, tempos limite de pedido e outras exceções transitórias pelo processamento-los de forma eficiente na biblioteca.  

* Fornece um mecanismo simplificado para aplicações efetuar operações em massa aumentar horizontalmente. Uma única instância de BulkExecutor em execução numa VM do Azure pode consumir maior do que 500 mil RU/s e pode conseguir uma taxa de débito superior, adicionando instâncias adicionais no cliente individuais VMs.  
 
* -Pode efetuar em massa importar mais do que um terabyte de dados dentro de uma hora, utilizando uma arquitetura de escalamento horizontal.  

* -Pode efetuar em massa de atualização de dados existente na base de dados do Azure Cosmos contentores como patches. 
 
## <a name="how-does-the-bulk-executor-operate"></a>Como operar o Executor em volume? 

Quando é acionada uma operação em massa para importar ou atualizar documentos com um lote de entidades, estas são inicialmente fora de posição para registos correspondente ao respetivo intervalo de chave de partição de BD do Cosmos do Azure. Dentro de cada registo corresponde a um intervalo de chave de partição, estes são divididos em lotes de mini e cada ato de mini-batch como um payload de é consolidado no lado do servidor. A biblioteca de BulkExecutor tem incorporada otimizações para execução em simultâneo lotes estes mini-dentro e entre os intervalos de chaves de partição. Imagem seguinte ilustra como BulkExecutory lotes dados para as chaves de partição diferentes:  

![Arquitetura de executor em massa](./media/bulk-executor-overview/bulk-executor-architecture.png)

A biblioteca de Executor em massa certifica-se utilizar maximally o débito atribuído a uma coleção. Utiliza um [mecanismo de controlo de congestionamento do estilo AIMD](https://tools.ietf.org/html/rfc5681) para cada base de dados do Azure Cosmos particionar o intervalo de chaves de forma eficiente processar limitação e tempos limite. 

## <a name="next-steps"></a>Próximos Passos 
  
* Saiba mais por experimentar as aplicações de exemplo consumir a biblioteca de Executor em massa do [.NET](bulk-executor-dot-net.md) e [Java](bulk-executor-java.md).  
* Consulte as notas de versão e informações de BulkExecutor SDK no [.NET](sql-api-sdk-bulk-executor-dot-net.md) e [Java](sql-api-sdk-bulk-executor-java.md).
* A biblioteca de Executor em massa está integrada o conector do Cosmos DB Spark, para obter mais informações, consulte [conector Azure Cosmos DB Spark](spark-connector.md) artigo.  
* A biblioteca de BulkExecutor também está integrada de uma nova versão do [conector de base de dados do Azure Cosmos](https://aka.ms/bulkexecutor-adf-v2) para o Azure Data Factory para copiar dados.
