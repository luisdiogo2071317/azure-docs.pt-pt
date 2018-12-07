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
ms.date: 12/05/2018
ms.openlocfilehash: e2440f6aa32710730e8b015bef1e7c583f7063e2
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "53001868"
---
# <a name="data-storage-and-ingress-in-the-azure-time-series-insights-preview"></a>Armazenamento de dados e de entrada no Azure Time Series Insights (pré-visualização)

Este artigo descreve as alterações para armazenamento de dados e de entrada a partir da pré-visualização do Azure Time Series Insights (TSI). Ele aborda a estrutura de armazenamento subjacente, o formato de arquivo, e **ID de série de tempo** propriedade. Ele também aborda o processo de entrada subjacente, débito e as limitações.

## <a name="data-storage"></a>Armazenamento de dados

Ao criar uma pré-visualização do Azure TSI (**PAYG SKU**) ambiente, está a criar dois recursos:

* Um ambiente do Azure TSI.
* Uma armazenamento do Azure para fins gerais V1 conta onde serão armazenados os dados.

O TSI (pré-visualização) utiliza o armazenamento de Blobs do Azure com o tipo de ficheiro Parquet. O Azure TSI gerencia todas as operações de dados incluindo a criação de blobs, indexação e criação de partições de dados na conta de armazenamento do Azure. Nestes blobs são criados com uma conta de armazenamento do Azure.

Como qualquer outro blob de armazenamento do Azure, pode ler e escrever os blobs criados pelo Azure TSI para oferecer suporte a cenários de integração de diferentes.

> [!TIP]
> Desempenho do TSI pode ser afetado negativamente por ler ou escrever para os blobs com demasiada frequência.

Para uma descrição geral do armazenamento de Blobs do Azure, leia os [introdução de blobs de armazenamento](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction).

Para mais informações sobre o tipo de ficheiro Parquet, consulte [tipos de ficheiro suportados no armazenamento do Azure](https://docs.microsoft.com/azure/data-factory/supported-file-formats-and-compression-codecs#Parquet-format).

## <a name="parquet-file-format"></a>Formato de ficheiro parquet

Parquet é orientado por colunas, dados, o formato de ficheiro que foi projetado para:

* Interoperabilidade
* Eficiência de espaço
* Eficiência de consulta

O Azure TSI escolheu o Parquet, pois ele fornece a compressão de dados eficiente e esquemas de codificação com o avançado desempenho para processar dados complexos em massa.

Para obter uma melhor compreensão sobre o formato de arquivo é tudo uma questão de Parquet, leia os [documentação oficial do Parquet](https://parquet.apache.org/documentation/latest/).

## <a name="event-structure-in-parquet"></a>Estrutura de eventos no Parquet

Duas cópias de blobs criados pelo Azure TSI serão armazenadas nos seguintes formatos:

1. O primeiro, uma cópia inicial, irá ser particionado por hora da chegada de:

    * `V=1/PT=Time/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`
    * Hora de criação de BLOBs para os blobs particionados por tempo de chegada.

1. O segundo, uma cópia repartitioned, irá ser particionado por agrupamento dinâmico de **ID de série de tempo**:

    * `V=1/PT=TsId/Y=<YYYY>/M=<MM>/<YYYYMMDDHHMMSSfff>_<TSI_INTERNAL_SUFFIX>.parquet`
    * Timestamp de mínima do evento num blob para blobs particionados por **ID de série de tempo**.

> [!NOTE]
> * `<YYYY>` é mapeado para uma representação do ano de 4 dígitos.
> * `<MM>` é mapeado para uma representação do mês de 2 dígitos.
> * `<YYYYMMDDHHMMSSfff>` mapeia para uma representação de timestamp com o ano de 4 dígitos (`YYYY`), 2 dígitos mês (`MM`), dígitos de 2 dias (`DD`), hora de 2 dígitos (`HH`), dígitos de 2 minutos (`MM`), segundo de 2 dígitos (`SS`) e 3 dígitos milissegundo (`fff`).

Eventos do Azure TSI são mapeados para o conteúdo do ficheiro Parquet, da seguinte forma:

* Cada evento é mapeado para uma única linha.
* Incorporado **Timestamp** coluna com um timestamp de evento. O **Timestamp** propriedade nunca é nula.  Assume como predefinição **hora de colocados em fila de origem do evento** se o **Timestamp** propriedade não for especificada na origem de evento.  **Timestamp** está em UTC.  
* Todas as outras propriedades o mapa para colunas terminará com `_string` (cadeia), `_bool` (booleano), `_datetime` (datetime), e `_double` (double), dependendo do tipo de propriedade.
* Que é a primeira versão do formato de arquivo e fazemos referência a ele como **V = 1**.  Se esse recurso se desenvolve o nome será incrementado em conformidade para **V = 2**, **V = 3**e assim por diante.

## <a name="partitions"></a>Partições

Cada ambiente do Azure TSI (pré-visualização) tem de ter uma **ID de série de tempo** propriedade e um **Timestamp** propriedade, que identifica de forma. Sua **ID de série de tempo** age como uma partição lógica para os seus dados e fornece o ambiente do Azure TSI (pré-visualização) com um limite de natural para a distribuição de dados pelas partições físicas. Gestão de partição física é gerido pelo Azure TSI (pré-visualização) numa conta de armazenamento do Azure.

Azure TSI utiliza o particionamento dinâmico para otimizar o desempenho de consulta e de utilização de armazenamento ao remover e recriar as partições. O Azure TSI (pré-visualização) tenta de algoritmo de particionamento dinâmico para impedir que uma única partição física ter dados de várias partições distintas, lógicas. Em outras palavras, o algoritmo de criação de partições mantém todos os dados específicos para uma única **ID de série de tempo** exclusivamente presentes no ficheiros Parquet sem a ser intercalado com os outros **IDs de série de tempo**. O algoritmo de particionamento dinâmico também tenta preservar a ordem original dos eventos num único **ID de série de tempo**.

Inicialmente, ao tempo de entrada, os dados são particionados pela **Timestamp** , para uma única partição lógica dentro de um intervalo de tempo especificado pode estar distribuída por várias partições físicas. Uma única partição física também pode conter muitos ou todos os lógicas partições.  Devido a limitações de tamanho do blob, mesmo com ideal de criação de partições de uma única partição lógica pode ocupar várias partições físicas.

> [!NOTE]
> O **Timestamp** valor é a mensagem **colocados em fila tempo** na origem de evento configurado por predefinição.  

Se estiver a carregar os dados históricos ou mensagens de lote, deve designar a **Timestamp** propriedade nos dados que mapeia para o adequado **Timestamp** valor que pretende armazenar com os seus dados.  O **Timestamp** propriedade diferencia maiúsculas de minúsculas. Para obter mais informações, leia os [artigo de modelo de série de tempo](./time-series-insights-update-tsm.md).

## <a name="physical-partitions"></a>Partições físicas

Uma partição física é um blob de blocos armazenado no armazenamento do Azure. O tamanho real dos blobs irá variar à medida que ele depende da taxa de push, no entanto, Contamos blobs para ser cerca de 20 a 50 MB de tamanho. Devido a essa expectativa, a equipe do TSI selecionado para ser o tamanho para otimizar o desempenho de consulta de 20 MB. Isso pode mudar ao longo do tempo com base no tamanho do ficheiro e a velocidade da entrada de dados.

> [!NOTE]
> * BLOBs são redimensionados em 20MB.
> * Blobs do Azure são ocasionalmente dividida novamente em partições para melhorar o desempenho ao que está a ser removido e recriado.
> * Observe também que os mesmos dados TSI podem constar de vários blobs.

## <a name="logical-partitions"></a>Partições lógicas

Uma partição lógica é uma partição dentro de uma partição física que armazena todos os dados associados a um valor de chave de partição única. O TSI (pré-visualização) será logicamente de partição cada blob com base em duas propriedades:

1. **ID de série de tempo** -é a chave de partição para todos os dados TSI dentro do fluxo de eventos e o modelo.
1. **Timestamp** - com base na entrada de inicial.

O Azure TSI (pré-visualização) fornece consultas de alto desempenho que se baseiam nessas duas propriedades. Essas duas propriedades também fornecem o método mais eficaz para fornecer dados TSI rapidamente.

É importante selecionar apropriadas **ID de série de tempo**, como é uma propriedade imutável.  Ver [IDs de série de tempo escolher](./time-series-insights-update-how-to-id.md) para obter mais informações.

## <a name="your-azure-storage-account"></a>Sua conta de armazenamento do Azure

### <a name="storage"></a>Armazenamento

Quando cria um TSI **PAYG** ambiente, criar dois recursos – o ambiente de TSI e uma conta de fins gerais de armazenamento do Azure da V1 onde serão armazenados os dados. Que escolhemos tornar a predefinição devido à sua interoperabilidade, preço e desempenho de armazenamento do Azure para fins gerais V1.  

O Azure TSI publicará até duas cópias de cada evento na sua conta de armazenamento do Azure. A cópia inicial é sempre preservada para garantir que pode consultar performantly com outros serviços. Portanto, Spark, Hadoop ou outras ferramentas familiares podem ser facilmente utilizadas no **IDs de série de tempo** sobre Parquet não processado de arquivos, uma vez que estes mecanismos de suportam básico ficheiros filtragem de nome. Blobs de agrupamento por ano e mês é útil para recolher a lista de BLOBs dentro do intervalo de tempo específico para uma tarefa personalizada.  

Além disso, o TSI irá criar novas partições os ficheiros Parquet para otimizar as APIs do Azure TSI, e também será possível guardar o ficheiro de repartitioned mais recentemente.

Durante a pré-visualização pública, dados serão armazenados indefinidamente na sua conta de armazenamento do Azure.

### <a name="writing-and-editing-time-series-insights-blobs"></a>Escrever e edição de blobs do Time Series Insights

Para garantir o desempenho de consulta e a disponibilidade de dados, não edite ou elimine quaisquer blobs criados por TSI.

### <a name="accessing-and-exporting-data-from-time-series-insights-preview"></a>Aceder e exportar dados do Time Series Insights (pré-visualização)

Pode querer aceder a dados armazenados no Explorador de TSI do Azure (pré-visualização) para utilizar em conjunto com outros serviços. Por exemplo, pode querer utilizar os seus dados para relatórios no Power BI, para fazer o machine learning com o Azure Machine Learning Studio ou num aplicativo de bloco de notas Jupyter Notebooks, etc.

Existem três caminhos gerais para aceder aos seus dados:

* O Explorador de TSI do Azure (pré-visualização).
* As APIs do Azure TSI (pré-visualização).
* Diretamente a partir de uma conta de armazenamento do Azure.

### <a name="from-the-time-series-insights-preview-explorer"></a>No Explorador do Time Series Insights (pré-visualização)

Pode exportar dados como um ficheiro CSV a partir do Explorador de TSI (pré-visualização). Leia mais sobre o [Explorador de TSI (pré-visualização)](./time-series-insights-update-explorer.md).

### <a name="from-the-time-series-insights-preview-apis"></a>Das APIs de Insights (pré-visualização) de série de tempo

O ponto final de API pode ser contatado pelo `/getRecorded`. Para saber mais sobre esta API, leia sobre [consultas de séries de tempo](./time-series-insights-update-tsq.md).

### <a name="from-an-azure-storage-account"></a>De uma conta de armazenamento do Azure

1. Tem de ter acesso de leitura concedido a qualquer conta irá utilizar para aceder aos dados do TSI. Para saber mais sobre como conceder acesso de leitura ao armazenamento de Blobs do Azure, leia [gerir o acesso a recursos de armazenamento](https://docs.microsoft.com/azure/storage/blobs/storage-manage-access-to-resources).

1. Para obter mais informações sobre as formas diretas de ler dados a partir do armazenamento de Blobs do Azure, leia [mover dados para e do armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

1. Exportar dados de uma conta de armazenamento do Azure:

    * Primeiro, certifique-se de que a sua conta tem os requisitos necessários cumpridos para exportar os dados. Leia [armazenamento importar e exportar requisitos](https://docs.microsoft.com/azure/storage/common/storage-import-export-requirements) para obter mais informações.
    * Saiba mais sobre outras formas de exportar os dados da sua conta de armazenamento do Azure, visite a página [armazenamento importar e exportar dados de blobs](https://docs.microsoft.com/azure/storage/common/storage-import-export-data-from-blobs)

### <a name="data-deletion"></a>Eliminação de dados

Não elimine os blobs, uma vez que o Azure TSI (pré-visualização) mantém metadados sobre os blobs dentro de atualização do TSI.

## <a name="ingress"></a>Entrada

### <a name="azure-time-series-insights-ingress-policies"></a>Políticas de entrada do Time Series Insights do Azure

O Azure TSI (pré-visualização) suporta as mesmas origens de eventos e tipos de ficheiro que o sucede.

Origens de eventos suportados incluem:

- Azure IoT Hub
- Azure Event Hubs
  
  > [!NOTE]
  > Instâncias de Hub de eventos do Azure suportam Kafka.

Tipos de ficheiro suportados incluem:

* JSON: Para mais sobre as formas JSON suportadas podemos manipular, consulte a [como a forma JSON](./time-series-insights-send-events.md#json) documentação.

### <a name="data-availability"></a>Disponibilidade de dados

Em pré-visualização pública, o Azure TSI (pré-visualização) indexa dados através de uma estratégia de otimização de tamanho do blob. Isso significa que os dados estarão disponíveis para consultar assim que ele é indexado (que se baseia-se à quantidade de dados está a chegar em e com que velocidade).

> [!IMPORTANT]
> * GA TSI irá disponibilizar dados em 60 segundos-lo a atingir uma origem de evento.  
> * Durante a pré-visualização, esperamos ver um período mais longo antes dos dados ficam disponíveis.  
>   * Se tiver qualquer uma latência significativa, entre em contato conosco.

### <a name="scale"></a>Escala

Azure TSI (pré-visualização) oferece suporte a uma escala de entrada inicial de até 6 Mbps por ambiente. Suporte avançado de dimensionamento está em curso. Documentação será atualizada para refletir esses avanços.

## <a name="next-steps"></a>Passos Seguintes

Leitura a [armazenamento do Azure TSI (pré-visualização) e de entrada](./time-series-insights-update-storage-ingress.md).

Saiba mais sobre a nova [modelação de dados](./time-series-insights-update-tsm.md).

<!-- Images -->
[1]: media/v2-update-storage-ingress/storage-architecture.png
[2]: media/v2-update-storage-ingress/parquet-files.png
[3]: media/v2-update-storage-ingress/blob-storage.png
