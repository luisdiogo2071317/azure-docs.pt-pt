---
title: Resolução de problemas de sugestões de pesquisa cognitivos na Azure Search | Microsoft Docs
description: Para configurar cognitivos de resolução de problemas e sugestões de pesquisa pipelines na Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 15fc879958bfd886210a90239e0247c60fe231f9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="troubleshooting-tips-for-cognitive-search"></a>Sugestões de resolução de problemas para pesquisa cognitivos

Este artigo contém uma lista de sugestões e truques para mantê-lo a mover como começar com capacidades de pesquisa cognitivos na Azure Search. 

Se ainda não o tiver o feito, siga os passos a [Tutorial: Saiba como chamar a APIs de pesquisa cognitivos](cognitive-search-quickstart-blob.md) com recomendado na aplicação enrichments pesquisa cognitivos a uma origem de dados de Blobs.

## <a name="tip-1-start-with-a-small-dataset"></a>Sugestão 1: Iniciar com um pequeno conjunto de dados
É a melhor forma de determinar os problemas rapidamente para aumentar a velocidade a que pode corrigir problemas. É a melhor forma de reduzir o tempo de indexação, reduzindo o número de documentos ser indexados. 

Comece por criar uma origem de dados com apenas alguns a documentos/registos. O exemplo de documento deve ser uma boa representação de variedade de documentos que irá ser indexados. 

Execute o seu exemplo de documento através do pipeline de ponto a ponto e verifique se os resultados as suas necessidades. Quando estiver satisfeito com os resultados, pode adicionar mais ficheiros para a origem de dados.

## <a name="tip-2-make-sure-your-data-source-credentials-are-correct"></a>Sugestão 2: Certifique-se de que as credenciais da origem de dados estão corretas
A ligação da origem de dados não é validada até definir um indexador que o utiliza. Se vir algum erro mentioning que o indexador não é possível obter os dados, certifique-se de que:
- A cadeia de ligação está correta. Especialmente quando estiver a criar SAS tokens, certifique-se de que utiliza o formato esperado pela pesquisa do Azure. Consulte [como especificar credenciais secção](
https://docs.microsoft.com/en-us/azure/search/search-howto-indexing-azure-blob-storage#how-to-specify-credentials) para saber mais sobre os formatos diferentes suportados.
- O nome do contentor no indexador está correta.

## <a name="tip-3-see-what-works-even-if-there-are-some-failures"></a>Sugestão 3: Ver o que funciona mesmo se existirem algumas falhas
Por vezes, uma pequena falha deixa um indexador no respetivo controla. Que é adequado se planear corrigir problemas de um de cada. No entanto, pode querer ignorar um determinado tipo de erro, permitindo que o indexador continuar para que possa ver que fluxos estão realmente a funcionar.

Nesse caso, poderá querer saber o indexador para ignorar erros. Fazê-lo definindo *maxFailedItems* e *maxFailedItemsPerBatch* como -1 como parte da definição de indexador.

```
{
  "// rest of your indexer definition
   "parameters":
   {
      "maxFailedItems":-1,
      "maxFailedItemsPerBatch":-1
   }
}
```
## <a name="tip-4-looking-at-enriched-documents-under-the-hood"></a>Sugestão 4: Observar documentos avançados os bastidores 
Documentos avançados são estruturas temporárias criados durante sem causa e, em seguida, eliminados quando o processamento estar concluído.

Para capturar um instantâneo do documento avançado criado durante a indexação, adicione um campo chamado ```enriched``` ao seu índice. O indexador automaticamente informações de estado para o campo de uma representação de cadeia de todos os enrichments para esse documento.

O ```enriched``` campo irá conter uma cadeia que é uma representação lógica do documento em memória avançada no JSON.  O valor do campo é um documento JSON válido, no entanto. As aspas são escape, pelo que terá de substituir `\"` com `"` para ver o documento como formatadas JSON. 

O campo avançado destina-se para fins de depuração só, para ajudar a compreender de forma lógica do conteúdo que estão a ser avaliadas expressões. Não deve depender este campo para fins de indexação.

Adicionar um ```enriched``` campo como parte da sua definição de índice para fins de depuração:

#### <a name="request-body-syntax"></a>Sintaxe de corpo do pedido
```json
{
  "fields": [
    // other fields go here.
    {
      "name": "enriched",
      "type": "Edm.String",
      "searchable": false,
      "sortable": false,
      "filterable": false,
      "facetable": false
    }
  ]
}
```

## <a name="tip-5-expected-content-fails-to-appear"></a>Sugestão 5: Era esperado conteúdo falhar a aparecer

Conteúdo em falta pode ser a consequência de documentos obter ignorados durante a indexação. Escalões gratuitos e básicos têm limites baixos no tamanho do documento. Qualquer ficheiro exceder o limite é removido durante a indexação. Pode verificar a existência de documentos processados no portal do Azure. No dashboard de serviço de pesquisa, faça duplo clique no mosaico de indexadores. Reveja o rácio de documentos com êxito indexada. Se não for 100%, pode clicar em rácio para obter mais detalhes. 

Se o problema está relacionado com o tamanho de ficheiro, poderá ver um erro semelhante isto: "Blob < nome de ficheiro >" tem o tamanho de bytes de < tamanho de ficheiro >, o que excede o tamanho máximo de extração de documentos para o escalão de serviço atual. " Para obter mais informações sobre limites de indexador, consulte [os limites de serviço](search-limits-quotas-capacity.md).

Um segundo motivo para o conteúdo que não são apresentadas poderão erros relacionados mapeamento de entrada/saída. Por exemplo, um nome de destino de saída é "Pessoas", mas o nome do campo de índice é minúsculas "pessoas". O sistema foi devolver 201 êxito mensagens para o pipeline de completa para refletir a indexação foi concluída com êxito, quando na realidade um campo está vazio. 

## <a name="tip-6-extend-processing-beyond-maximum-run-time-24-hour-window"></a>Sugestão 6: Expandir o processamento além do tempo (janela de 24 horas) de execução máximo

Análise de imagem é viáveis intensivas simples, mesmo nos casos, por isso, quando as imagens são especialmente grande ou complexa, tempos de processamento podem exceder o tempo máximo permitido. 

Tempo de execução máximo varia consoante a camada: camada de alguns minutos no gratuito, 24 horas indexação em camadas sujeito a faturação. Se o processamento de mensagens em fila não conseguir concluir durante um período de 24 horas para processar a pedido, mude para uma agenda para que o indexador processará processamento onde parou. 

Para indexadores agendadas, indexação retoma numa agenda na última documento boa conhecida. Ao utilizar uma agenda periódica, o indexador pode trabalhar a caminho através do registo de segurança de imagem através de uma série de horas ou dias, até que todas as imagens não processadas são processadas. Para obter mais informações sobre a sintaxe de agendamento, consulte [passo 3: criar um indexador](search-howto-indexing-azure-blob-storage.md#step-3-create-an-indexer).

Para baseado no portal de indexação (conforme descrito no início rápido), escolher o indexador "executar uma vez" opção limites de processamento para 1 hora (`"maxRunTime": "PT1H"`). Poderá pretender expandir a janela de processamento para algo mais.

## <a name="tip-7-increase-indexing-throughput"></a>Sugestão 7: Aumentar o débito de indexação

Para [indexação paralelas](search-howto-reindex.md#parallel-indexing), coloque os dados no vários contentores ou várias pastas virtuais no interior do contentor do mesmo. Em seguida, crie vários pares de origem de dados e o indexador. Todos os indexadores podem utilizar o mesmo skillset e de escrita para o mesmo índice de pesquisa de destino, pelo que a aplicação de pesquisa não têm de ter em consideração a criação de partições.
Para obter mais informações, consulte [indexação grandes conjuntos de dados](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets).

## <a name="see-also"></a>Consulte também
+ [Início rápido: Criar um pipeline de pesquisa cognitivos no portal](cognitive-search-quickstart-blob.md)
+ [Tutorial: Saber pesquisa cognitivos REST APIs](cognitive-search-tutorial-blob.md)
+ [Especificar as credenciais da origem de dados](search-howto-indexing-azure-blob-storage.md#how-to-specify-credentials)
+ [A indexação de grandes conjuntos de dados](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets)
+ [Como definir um skillset](cognitive-search-defining-skillset.md)
+ [Como mapear campos avançados para um índice](cognitive-search-output-field-mapping.md)