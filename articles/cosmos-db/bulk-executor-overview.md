---
title: Azure Cosmos DB em massa executor descrição geral da biblioteca | Microsoft Docs
description: Saiba mais sobre a base de dados do Azure Cosmos em massa executor biblioteca vantagens de utilizar a biblioteca e respetiva arquitetura.
keywords: Executor do Java em massa
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: ramkris
ms.openlocfilehash: 7c490aa958cf9e78c260dd0fbcf7952b55d8d88c
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096181"
---
# <a name="azure-cosmos-db-bulk-executor-library-overview"></a>Azure Cosmos DB em massa executor descrição geral da biblioteca
 
BD do Cosmos do Azure é um serviço de base de dados rápido, flexível e globalmente distribuída que foi concebido para aprovisionadas de escalamento horizontal para suportar: 

* Grande de leitura e escrita throughputs (milhões de operações por segundo).  
* Armazenamento de volumes elevados de (centenas de terabytes ou até mesmo mais) transacionais e operacionais dados com uma latência de milissegundo previsível.  

A biblioteca de executor em massa ajuda-o a tirar partido do débito intenso e o armazenamento, a biblioteca de executor em massa permite-lhe efetuar em massa operações do BD Azure Cosmos através da importação em volume e em massa atualizar APIs. Pode ler mais sobre as funcionalidades da biblioteca de executor em massa nas secções seguintes. 

> [!NOTE] 
> Atualmente, a biblioteca de executor de volume suporta a importação e operações update e esta biblioteca é suportado pelo apenas contas do Azure da API do Cosmos BD SQL. Consulte [.NET](sql-api-sdk-bulk-executor-dot-net.md) e [Java](sql-api-sdk-bulk-executor-java.md) notas de versão do quaisquer atualizações para a biblioteca.
 
## <a name="key-features-of-the-bulk-executor-library"></a>Principais funcionalidades da biblioteca de executor em massa  
 
* Esta opção reduz significativamente os recursos de computação do lado do cliente necessários para sob saturar ao débito atribuído a um contentor. Uma única aplicação threading escreve dados utilizando que a API de importação em massa distribui 10 vezes o débito de escrita superior quando comparado com uma aplicação multi-thread que escreve dados em paralelo, ao saturating o cliente de CPU da máquina.  

* Deduz, ausente, as tarefas tedious de escrever a lógica de aplicação para processar a limitação de taxa de pedidos, tempos limite de pedido e outras exceções transitórias pelo processamento-los de forma eficiente na biblioteca.  

* Fornece um mecanismo simplificado para aplicações efetuar operações em massa aumentar horizontalmente. Uma instância de executor em massa única em execução numa VM do Azure pode consumir maior do que 500 mil RU/s e pode conseguir uma taxa de débito superior, adicionando instâncias adicionais no cliente individuais VMs.  
 
* -Pode efetuar em massa importar mais do que um terabyte de dados dentro de uma hora, utilizando uma arquitetura de escalamento horizontal.  

* -Pode efetuar em massa de atualização de dados existente na base de dados do Azure Cosmos contentores como patches. 
 
## <a name="how-does-the-bulk-executor-operate"></a>Como operar o Executor em volume? 

Quando é acionada uma operação em massa para importar ou atualizar documentos com um lote de entidades, estas são inicialmente fora de posição para registos correspondente ao respetivo intervalo de chave de partição de BD do Cosmos do Azure. Dentro de cada registo corresponde a um intervalo de chave de partição, estes são divididos em lotes de mini e cada ato de mini-batch como um payload de é consolidado no lado do servidor. A biblioteca de executor em massa tem incorporada otimizações para execução em simultâneo lotes estes mini-dentro e entre os intervalos de chaves de partição. Imagem seguinte ilustra como o executor em massa lotes dados para as chaves de partição diferentes:  

![Arquitetura de executor em massa](./media/bulk-executor-overview/bulk-executor-architecture.png)

A biblioteca de Executor em massa certifica-se utilizar maximally o débito atribuído a uma coleção. Utiliza um [mecanismo de controlo de congestionamento do estilo AIMD](https://tools.ietf.org/html/rfc5681) para cada base de dados do Azure Cosmos intervalo de chaves para processar de forma eficiente limitação de taxa e tempos limite de partição. 

## <a name="next-steps"></a>Próximos Passos 
  
* Saiba mais por experimentar as aplicações de exemplo consumir a biblioteca de Executor em massa do [.NET](bulk-executor-dot-net.md) e [Java](bulk-executor-java.md).  
* Veja as em massa executor SDK informações e versão notas no [.NET](sql-api-sdk-bulk-executor-dot-net.md) e [Java](sql-api-sdk-bulk-executor-java.md).
* A biblioteca de Executor em massa está integrada o conector do Cosmos DB Spark, para obter mais informações, consulte [conector Azure Cosmos DB Spark](spark-connector.md) artigo.  
* A biblioteca de executor em massa também está integrada de uma nova versão do [conector de base de dados do Azure Cosmos](https://aka.ms/bulkexecutor-adf-v2) para o Azure Data Factory para copiar dados.
