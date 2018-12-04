---
title: Armazenamento de dados e de entrada no Azure Time Series Insights (pré-visualização) | Documentos da Microsoft
description: Noções básicas sobre o armazenamento de dados e de entrada no Azure Time Series Insights (pré-visualização)
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: 6635558d1b7cf664084c103e3b3b37b44c022fa6
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856549"
---
# <a name="data-storage-and-ingress-in-the-azure-time-series-insights-preview"></a>Armazenamento de dados e de entrada no Azure Time Series Insights (pré-visualização)

Este artigo descreve as alterações para o armazenamento de dados e de entrada do Azure Time Series Insights (pré-visualização). Ele aborda a estrutura de armazenamento subjacente, o formato de arquivo, e **ID de série de tempo** propriedade. Ele também aborda o processo de entrada subjacente, débito e as limitações.

## <a name="data-storage"></a>Armazenamento de dados

Durante a criação de uma atualização do Time Series Insights (**PAYG Sku**) ambiente, está a criar dois recursos:

1. Um ambiente do Azure TSI.
1. Um fins gerais de armazenamento do Azure, onde serão armazenados os dados de conta de V1.

O TSI (pré-visualização) utiliza o armazenamento de Blobs do Azure com o tipo de ficheiro Parquet. O Azure TSI gerencia todas as operações de dados incluindo a criação de blobs, indexação e criação de partições de dados na conta de armazenamento do Azure. Nestes blobs são criados com uma conta de armazenamento do Azure. Para garantir que todos os eventos podem ser consultados de maneira eficiente. A atualização do TSI irá suportar armazenamento do Azure para fins gerais V1 e V2 "quentes" Opções de configuração.  

Como qualquer outro blob de armazenamento do Azure, pode ler e escrever os blobs criados pelo Azure TSI para oferecer suporte a cenários de integração de diferentes.

> [!TIP]
> É importante lembrar-se de que o desempenho de TSI pode ser afetado negativamente por ler ou escrever para os blobs com demasiada frequência.

Para uma descrição geral do armazenamento de Blobs do Azure funciona, leia os [introdução de blobs de armazenamento](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction).

Para obter mais informações sobre o tipo de ficheiro Parquet, reveja [tipos de ficheiro suportados no armazenamento do Azure](https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs#Parquet-format).

## <a name="parquet-file-format"></a>Formato de ficheiro parquet

Parquet é o formato de arquivo de dados e orientados a coluna que foi projetado para:

* Interoperabilidade
* Eficiência de espaço
* Eficiência de consulta

O Azure TSI escolheu o Parquet, pois ele fornece a compressão de dados eficiente e esquemas de codificação com o avançado desempenho para processar dados complexos em massa.

Para obter uma melhor compreensão do que o ficheiro Parquet formato se trata, vá para o [página oficial de Parquet](https://parquet.apache.org/documentation/latest/).

## <a name="event-structure-in-parquet"></a>Estrutura de eventos no Parquet

Duas cópias de blobs criados pelo Azure TSI serão armazenadas nos seguintes formatos:

1. O primeiro, uma cópia inicial, irá ser particionado por hora da chegada de:

    * `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI internal suffix>.parquet`
    * Hora de criação de BLOBs para os blobs particionados por tempo de chegada.

1. O segundo, uma cópia repartitioned, irá ser particionado por agrupamento dinâmico do ID de série de tempo:

    • `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_< TSI internal suffix >.parquet`
    * Timestamp de evento min no blob blobs particionados por uma série de tempo de ID.

> [!NOTE]
> * `<YYYY>` é mapeado para o ano.
> * `<MM>` é mapeado para o mês.
> * `<YYYYMMDDHHMMSSfff>` é mapeado para timestamp completo em milissegundos.

Eventos do Azure TSI são mapeados para o conteúdo do ficheiro Parquet, da seguinte forma:

* Cada evento é mapeado para uma única linha.
* Incorporado **Timestamp** coluna com um timestamp de evento. O **Timestamp** propriedade nunca é nula.  Assume como predefinição **hora de colocados em fila de origem do evento** se o **Timestamp** propriedade não for especificada na origem de evento.  **Timestamp** está em UTC.  
* Todas as outras propriedades o mapa para colunas terminará com `_string` (cadeia), `_bool` (booleano), `_datetime` (datetime), e `_double` (double), dependendo do tipo de propriedade.
* Essa é a primeira versão do formato de arquivo e fazemos referência a ele como ***V = 1**.  Se esse recurso se desenvolve o nome será incrementado em conformidade para **V = 2**, **V = 3**e assim por diante.

## <a name="how-to-partition"></a>Como a partição

Cada ambiente do Azure TSI (pré-visualização) tem de ter uma **ID de série de tempo** propriedade e um **Timestamp** propriedade, que identifica de forma. Sua **ID de série de tempo** age como uma partição lógica para os seus dados e fornece o ambiente do Azure TSI (pré-visualização) com um limite de natural para a distribuição de dados pelas partições físicas. Gestão de partição física é gerido pelo Azure TSI (pré-visualização) numa conta de armazenamento do Azure.

Azure TSI utiliza o particionamento dinâmico para otimizar o desempenho de consulta e de utilização de armazenamento ao remover e recriar as partições. O algoritmo de particionamento dinâmico do TSI (pré-visualização) se esforça para evitar uma única partição física ter dados de várias partições lógicas diferentes. Ou em outras palavras objetivo do algoritmo de criação de partições é manter todos os dados relacionados com um único **ID de série de tempo** estar exclusivamente presente no ficheiro (s) de Parquet sem a ser intercalado com os outros **IDs de série de tempo**. O algoritmo de particionamento dinâmico também se esforça para preservar a ordem original de eventos num único **ID de série de tempo**.

Inicialmente, ao tempo de entrada, os dados são particionados pela **Timestamp** , para uma única partição lógica dentro de um intervalo de tempo especificado pode estar distribuída por várias partições físicas. Uma única partição física também pode conter muitos ou todos os lógicas partições.  Devido a limitações de tamanho do blob, mesmo com ideal de criação de partições de uma única partição lógica pode ocupar várias partições físicas.

> [!NOTE]
> O **Timestamp** valor é a mensagem **colocados em fila tempo** na origem de evento configurado por predefinição.  

Se estiver a carregar os dados históricos ou mensagens de lote, deve designar a **Timestamp** propriedade nos dados que mapeia para o adequado **Timestamp** valor que pretende armazenar com os seus dados.  O **Timestamp** propriedade diferencia maiúsculas de minúsculas. Para obter mais informações, leia os [artigo de modelo de série de tempo](./time-series-insights-update-tsm.md).

## <a name="physical-partition"></a>Partição física

Uma partição física é um blob de blocos armazenado no armazenamento do Azure. O tamanho real dos blobs irá variar à medida que ele depende da taxa de push, no entanto, Contamos blobs para ser cerca de 20 a 50 MB de tamanho. Devido a essa expectativa, a equipe do TSI selecionado para ser o tamanho para otimizar o desempenho de consulta de 20 MB. Isso pode mudar ao longo do tempo com base no tamanho do ficheiro e a velocidade da entrada de dados.

> [!NOTE]
> * BLOBs são redimensionados em 20MB.
> * Blobs do Azure são ocasionalmente dividida novamente em partições para melhorar o desempenho ao que está a ser removido e recriado.
> * Observe também que os mesmos dados TSI podem constar de vários blobs.

## <a name="logical-partition"></a>Partição lógica

Uma partição lógica é uma partição dentro de uma partição física que armazena todos os dados associados a um valor de chave de partição única. O TSI (pré-visualização) será logicamente de partição cada blob com base em duas propriedades:

1. **ID de série de tempo** -é a chave de partição para todos os dados TSI dentro do fluxo de eventos e o modelo.
1. **Timestamp** - com base na entrada de inicial.

O Azure TSI (pré-visualização) fornece consultas de alto desempenho que se baseiam nessas duas propriedades. Essas duas propriedades também fornecem o método mais eficaz para fornecer dados TSI rapidamente.

É importante selecionar apropriadas **IDs de série de tempo**, como é uma propriedade imutável.  Veja [IDs de série de tempo escolher](./time-series-insights-update-how-to-id.md) para obter mais informações.

## <a name="your-azure-storage-account"></a>Sua conta de armazenamento do Azure

### <a name="storage"></a>Armazenamento

Quando cria um ambiente de TSI PAYG, cria dois recursos – o ambiente do TSI e um conta V1 de onde serão armazenados os dados de armazenamento do Azure para fins gerais. No futuro, as contas de fins gerais V1 do armazenamento do Azure será limitadas para os clientes do Azure, portanto todos os clientes novos para o aprovisionamento de um ambiente de TSI (pré-visualização) do Azure por predefinição criará uma conta de "Acesso frequente" de fins gerais V2 do armazenamento do Azure.  Que escolhemos para fins gerais de armazenamento do Azure V1 de tornar a predefinição devido à sua interoperabilidade, preço e desempenho.  

O Azure TSI publicará até duas cópias de cada evento na sua conta de armazenamento do Azure. A cópia inicial é sempre preservada para garantir que pode consultar performantly com outros serviços. Portanto, Spark, Hadoop ou outras ferramentas familiares podem ser facilmente utilizadas no **IDs de série de tempo** sobre Parquet não processado de arquivos, uma vez que estes mecanismos de suportam básico ficheiros filtragem de nome. Blobs de agrupamento por ano e mês é útil para recolher a lista de BLOBs dentro do intervalo de tempo específico para uma tarefa personalizada.  

Além disso, o TSI irá criar novas partições os ficheiros Parquet para otimizar as APIs do Azure TSI, e também será possível guardar o ficheiro de repartitioned mais recentemente.

Durante a pré-visualização pública, dados serão armazenados indefinidamente na sua conta de armazenamento do Azure. A capacidade de eliminar os dados será adicionada no futuro, que permite um maior grau de controle de gerenciamento de dados antigos.

### <a name="writing-and-editing-time-series-insights-blobs"></a>Escrever e edição de blobs do Time Series Insights

Para garantir o desempenho de consulta e a disponibilidade de dados, não edite ou elimine quaisquer blobs criados por TSI.

### <a name="accessing-and-exporting-data-from-time-series-insights-preview"></a>Aceder e exportar dados do Time Series Insights (pré-visualização)

Poderá aceder aos dados armazenados no Azure TSI (pré-visualização) Explorer para utilizar em conjunto com outros serviços. Por exemplo, pode querer utilizar os seus dados para relatórios no Power BI, para fazer o machine learning com o Azure Machine Learning Studio ou num aplicativo de bloco de notas Jupyter Notebooks, etc.

Existem três caminhos gerais para aceder aos seus dados:

* O Explorador de TSI do Azure (pré-visualização).
* As APIs do Azure TSI (pré-visualização).
* Diretamente a partir de uma conta de armazenamento do Azure.

### <a name="from-the-time-series-insights-preview-explorer"></a>No Explorador do Time Series Insights (pré-visualização)

Pode exportar dados como um ficheiro CSV a partir do TSI (pré-visualização) Explorer. Leia mais sobre o [o TSI (pré-visualização) Explorer](./time-series-insights-update-explorer.md).

### <a name="from-the-time-series-insights-preview-apis"></a>Das APIs do Time Series Insights (pré-visualização)

O ponto final de API pode ser contatado pelo `/getRecorded`. Para saber mais sobre esta API, leia sobre [consultas de séries de tempo](./time-series-insights-update-tsq.md).

### <a name="from-an-azure-storage-account"></a>De uma conta de armazenamento do Azure

1. Tem de ter acesso de leitura concedido a qualquer conta irá utilizar para aceder aos dados do TSI. Para saber mais sobre como conceder acesso de leitura ao armazenamento de Blobs do Azure, leia [gerir o acesso a recursos de armazenamento](https://docs.microsoft.com/azure/storage/blobs/storage-manage-access-to-resources).

1. Para obter mais informações sobre as formas diretas de ler dados a partir do armazenamento de Blobs do Azure, leia [mover dados para e do armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

1. Exportar dados de uma conta de armazenamento do Azure:

    * Primeiro, certifique-se de que a sua conta tem os requisitos necessários cumpridos para exportar os dados. Leia [armazenamento importar e exportar requisitos](https://docs.microsoft.com/azure/storage/common/storage-import-export-requirements) para obter mais informações.
    * Saiba mais sobre outras formas de exportar os dados da sua conta de armazenamento do Azure, visite a página [armazenamento importar e exportar dados de blobs](https://docs.microsoft.com/azure/storage/common/storage-import-export-data-from-blobs)

### <a name="blob-storage-considerations"></a>Considerações de armazenamento de BLOBs

* O armazenamento do Azure têm de leitura e escrita de limites com base no peso é de sua utilização do TSI (pré-visualização).  
* A pré-visualização do Azure TSI privada ainda não forneça qualquer tipo de Parquet meta-arquivo de BLOBs para oferecer suporte a sistemas de processamento de dados externo. No entanto, podemos está a investigar isso e pode adicionar suporte no futuro.  
* Os clientes terão ler os particionados por tempo para ser capaz de processar os dados de blobs do Azure.
* A pré-visualização do Azure privado do TSI executa repartir dinâmica de dados de blob para um melhor desempenho. Isto é conseguido ao remover e recriar os blobs. A maioria dos serviços serão fornecidas melhor usando os arquivos originais.  
* Os dados TSI (pré-visualização) podem ser duplicados em blobs.

### <a name="data-deletion"></a>Eliminação de dados

A pré-visualização do Azure TSI privada não suporta atualmente a eliminação de dados, mas será no futuro. Esperamos que suporte através de disponibilidade geral, mas potencialmente mais cedo. Será notificado utilizadores quando damos suporte a eliminação de dados.

Não elimine os blobs, uma vez que o Time Series Insights (pré-visualização) mantém metadados sobre os blobs dentro de atualização do TSI.

## <a name="ingress"></a>Entrada

### <a name="azure-time-series-insights-ingress-policies"></a>Políticas de entrada do Time Series Insights do Azure

A pré-visualização do Azure TSI privada suporta as mesmas origens de eventos e tipos de ficheiro que o sucede.

Origens de eventos suportados incluem:

* Azure IoT Hub
* Azure Event Hubs
  * Nota: As instâncias do Hub de eventos do Azure suportam Kafka.

Tipos de ficheiro suportados incluem:

* JSON
  * Para mais sobre as formas JSON suportadas podemos manipular, consulte a [consultas de séries de tempo](./time-series-insights-update-tsq.md) documentação.

### <a name="data-availability"></a>Disponibilidade de dados

A pré-visualização do Azure TSI privada indexa dados através de uma estratégia de otimização de tamanho do blob. Isso significa que os dados estarão disponíveis para consultar assim que ele é indexado (que se baseia-se à quantidade de dados está a chegar em e com que velocidade). Quando nos aproximarmos pré-visualização pública, será adicionada lógica para procurar por novos eventos cada alguns segundos (o que irão disponibilizar dados para consultas mais rápidas e confiáveis).

> [!IMPORTANT]
> * O TSI pré-visualização pública irá disponibilizar dados em 60 segundos-lo a atingir uma origem de evento.  
> * Durante a pré-visualização privada, que podemos esperar um período mais longo antes dos dados esteja disponível.  
>   * Se tiver qualquer uma latência significativa, entre em contato conosco.

### <a name="scale"></a>Escala

Esperamos que a pré-visualização do TSI privado do Azure para suportar até 6Mbps por ambiente. Suporte avançado de dimensionamento está planejado para versões futuras.

[!INCLUDE [tsi-update-docs](../../includes/time-series-insights-update-documents.md)]

## <a name="next-steps"></a>Passos Seguintes

Leitura a [Azure TSI (pré-visualização), armazenamento e de entrada](./time-series-insights-update-storage-ingress.md).

Saiba mais sobre a nova [modelo de série de tempo](./time-series-insights-update-tsm.md).

<!-- Images -->
[1]: media/v2-update-storage-ingress/storage-architecture.png
[2]: media/v2-update-storage-ingress/parquet-files.png
[3]: media/v2-update-storage-ingress/blob-storage.png