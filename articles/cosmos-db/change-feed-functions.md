---
title: Como utilizar o Azure Cosmos DB alterar feed com as funções do Azure
description: Utilização que do Azure Cosmos DB alterar feed com as funções do Azure
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 41cbb657a4fc83b498c5cc9a6a16397a619aa075
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54034051"
---
# <a name="how-to-use-azure-cosmos-db-change-feed-with-azure-functions"></a>Como utilizar o Azure Cosmos DB alterar feed com as funções do Azure

Se estiver a utilizar as funções do Azure, a forma mais simples para ligar ao feed de alterações é adicionar uma [acionador do Azure Cosmos DB](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger) à sua aplicação de funções do Azure. Quando cria um acionador Cosmos DB numa aplicação de funções do Azure, selecionar o contentor do Cosmos para ligar a e a função é acionada sempre que alterar algo no contentor.

Acionadores podem ser criados no portal de funções do Azure ou no portal do Azure Cosmos DB ou por meio de programação. Para obter mais informações, consulte [computação de base de dados sem servidor com o Azure Cosmos DB e as funções do Azure](serverless-computing-database.md).

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

### <a name="how-can-i-configure-azure-functions-to-read-from-a-particular-region"></a>Como posso configurar as funções do Azure para ler a partir de uma determinada região?

É possível definir o [PreferredLocations](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.preferredlocations?view=azure-dotnet#Microsoft_Azure_Documents_Client_ConnectionPolicy_PreferredLocations) ao utilizar o acionador do Azure Cosmos DB para especificar uma lista de regiões. É mesmo enquanto vai personalizando ConnectionPolicy, para que o acionador ler a partir do seu regiões preferenciais. O ideal é que queira ler a partir da região mais próxima em que as suas funções do Azure é implementada.

### <a name="what-is-the-default-size-of-batches-in-azure-functions"></a>O que é o tamanho predefinido de lotes nas funções do Azure?

O tamanho predefinido é 100 itens para cada invocação das funções do Azure. No entanto, este número pode ser configurado no arquivo Function. Aqui está concluída [lista de opções de configuração](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration). Se estiver a desenvolver localmente, Atualize as definições de aplicação dentro do arquivo Settings.

### <a name="i-am-monitoring-a-container-and-reading-its-change-feed-however-i-dont-get-all-the-inserted-documents-some-items-are-missing"></a>Estou monitorando um contentor e ler o seu feed de alterações, no entanto, não tenho todos os documentos inseridos, alguns itens estão em falta?

Certifique-se de que não existe nenhuma outra função de Azure o mesmo contentor com o mesmo contentor de concessão de leitura. Os documentos em falta são processados por outras funções do Azure que também estão a utilizar a concessão do mesmo.

Por conseguinte, se estiver a criar várias funções do Azure para ler que o mesmo feed de alterações, eles tem de utilizar contentores de concessão diferentes ou utilizar a configuração de "leasePrefix" para partilhar o mesmo contentor. No entanto, quando usa a biblioteca de processador do feed de alterações pode iniciar várias instâncias da sua função do Azure e o SDK irá dividir os documentos entre instâncias diferentes automaticamente por si.

### <a name="azure-cosmos-item-is-updated-every-second-and-i-dont-get-all-the-changes-in-azure-functions-listening-to-change-feed"></a>Item do Cosmos do Azure é atualizado a cada segundo, e eu não concluir todas as alterações nas funções do Azure à escuta para o feed de alterações?

As funções do Azure consulta a alteração do feed de alterações continuamente, com um atraso máximo predefinido de 5 segundos. Se existirem não existem alterações pendentes a ser lido, ou se existem alterações pendentes, depois do acionador é aplicado, a função lê-los será imediatamente. No entanto, se não existirem não existem alterações pendentes, a função irá aguardar 5 segundos e consultar mais alterações.

Se o seu documento receber várias alterações no mesmo intervalo que demorou o acionador para consultar as novas alterações, poderá receber a versão mais recente do documento e não um intermediário.

Se pretender consultar o feed para menos de 5 segundos de alterações, por exemplo, para cada segundo, pode configurar o tempo de consulta "feedPollDelay", consulte [toda a configuração](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.preferredlocations?view=azure-dotnet#Microsoft_Azure_Documents_Client_ConnectionPolicy_PreferredLocations). Ele é definido em milissegundos com padrão de 5000. Consulta para menos de 1 segundo, é possível, mas ele não é aconselhável porque irá começar a utilizar mais de memória de CPU.

### <a name="can-multiple-azure-functions-read-one-containers-change-feed"></a>Várias funções do Azure pode ler a feed de alterações de um contentor?

Sim. Várias funções do Azure pode ler o feed de alterações do mesmo contêiner. No entanto, as funções do Azure tem de ter um separado "leaseCollectionPrefix" definida.

### <a name="if-i-am-processing-change-feed-by-using-azure-functions-in-a-batch-of-10-documents-and-i-get-an-error-at-seventh-document-in-that-case-the-last-three-documents-are-not-processed-how-can-i-start-processing-from-the-failed-document-ie-seventh-document-in-my-next-feed"></a>Se eu estou de processamento de feed de alterações ao utilizar as funções do Azure, num lote de 10 documentos e recebo um erro no sétimo documento. Nesse caso os últimos três documentos não são processados como posso iniciar processamento do documento com falhas (ou seja, o documento sétimo) no meu feed seguinte?

Para lidar com o erro, o padrão recomendado é encapsular seu código com o bloco try-catch e, se estiver fazendo a iteração sobre a lista de documentos, encapsular cada iteração em seu próprio bloco try-catch. Capturar o erro e colocar esse documento numa fila (mensagens não entregues) e, em seguida, definir a lógica para lidar com os documentos que produziu o erro. Com este método se tiver um lote de documento de 200 e apenas um documento falhou, é necessário de jogar fora o lote inteiro.

No caso de erro, não deve de retroceder o ponto de verificação de volta para a partir de outro será pode continuar a receber esses documentos de feed de alterações. Lembre-se de que mantém de feed de alterações a captura de final snap última dos documentos, devido a este poderá perder o instantâneo anterior no documento. feed de alterações mantém apenas uma última versão do documento, e entre os outros processos podem voltar e alterar o documento.

Como manter a corrigir o seu código, descobrirá em breve não existem documentos em fila de mensagens não entregues. As funções do Azure é chamado automaticamente pelo sistema de feed de alterações e o ponto de verificação é mantido internamente pela função do Azure. Se quiser reverter o ponto de verificação e controlar cada aspecto dela, deve considerar usar alteração SDK de processador do feed.

### <a name="are-there-any-extra-costs-for-using-the-azure-cosmos-db-trigger"></a>Existem custos adicionais para utilizar o acionador do Azure Cosmos DB?

O acionador do Azure Cosmos DB tira partido da biblioteca processador do Feed de alterações internamente. Como tal, ele exige uma coleção extra, chamada de coleção de concessões, para manter o estado e pontos de verificação parciais. Esse gerenciamento de estado é necessário para ser capaz de dimensionar e continuar no caso de que pretende parar as suas funções do Azure e continuar o processamento num momento posterior dinamicamente. Para obter mais informações, consulte [biblioteca processador do feed de como trabalhar com alteração](change-feed-processor.md).

## <a name="next-steps"></a>Passos Seguintes

Agora, pode avançar para saber mais sobre a alteração do feed nos seguintes artigos:

* [Descrição geral do feed de alterações](change-feed.md)
* [Feed de alterações de formas para ler](read-change-feed.md)
* [Usando a alteração de biblioteca processador do feed](change-feed-processor.md)
* [Como trabalhar com a alteração de biblioteca processador do feed](change-feed-processor.md)
* [Computação de base de dados sem servidor com o Azure Cosmos DB e as funções do Azure](serverless-computing-database.md)
