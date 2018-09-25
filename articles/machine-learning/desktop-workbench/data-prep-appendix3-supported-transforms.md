---
title: Usar transformações de dados para preparação de dados no Azure Machine Learning | Documentos da Microsoft
description: Este artigo fornece uma lista completa das transformações disponíveis para a preparação de dados do Azure Machine Learning.
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 26674648217e01b66fbe722cd013d51d867e9ba9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46984395"
---
# <a name="use-data-transforms-for-data-preparation-in-azure-machine-learning"></a>Usar transformações de dados para preparação de dados no Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

R *transformar* no Azure Machine Learning consome dados num determinado formato, executa uma operação nos dados (por exemplo, alterando o tipo de dados) e, em seguida, produz dados no novo formato. Cada transformação tem sua própria interface e o comportamento. Vinculando várias transformações em conjunto através de passos no fluxo de dados, pode efetuar transformações complexas e passível de repetição nos seus dados. Este é o núcleo da funcionalidade de preparação de dados.

Seguem-se as transformações disponíveis no Azure Machine Learning. 

## <a name="column-selection"></a>Seleção de coluna 
Muitas das transformações nesta lista funcionam numa única coluna ou muitos. Para selecionar várias colunas, utilize a tecla Ctrl. Selecionar um intervalo de colunas, utilize a tecla Shift.

## <a name="transforms-from-the-main-menu-or-the-grid-header"></a>Transformações a partir do menu principal ou o cabeçalho da grelha 
Acesso transforma-se de que a opção de transformações no menu principal. Também pode selecionar transformações clicando com o nome da coluna na grelha de dados. Se forem selecionadas várias colunas, clicar em qualquer um deles oferece um menu de transformações.

Menu de atalho mostra apenas as transformações válidas para o tipo de dados selecionada. Menu principal oferece todas as transformações, mas que Desabilita os não é relevante para as colunas selecionadas.

Um pequeno subconjunto de transformações contextuais está disponível clicando numa célula. Essas transformações de cópia, Replace e filtram. Estes são dados tipo com suporte, para que as opções para uma coluna de número são diferentes para uma coluna de cadeia de caracteres.

## <a name="derive-column-by-example"></a>Derivar Coluna por Exemplo
Utilize essa transformação para criar uma nova coluna como um derivativo de um ou mais colunas existentes. A transformação analisa as colunas de entrada (selecionadas) e o exemplo fornecido e, em seguida, determina a saída desejada na nova coluna. 

Para usar essa transformação, selecione uma ou mais colunas. Adicione uma nova coluna derivada (em branco), por exemplo. Escreva um exemplo de que pretende ver na coluna derivada (supondo que ele é derivado de outras colunas) e o "por exemplo" tecnologia tenta preencher todas as outras células na coluna. 

Para obter exemplos complicados, poderá ter de fornecer mais do que um exemplo. Para tal, selecione outra célula e escreva outro exemplo.

A tecnologia de "Por exemplo" utiliza colunas selecionadas para tentar determinar o significado de um exemplo. Se não foram seleccionadas colunas quando essa transformação é invocada, todas as células da linha atual são utilizadas. Selecionar apenas as colunas necessárias leva aos resultados mais precisos.

Pode selecionar colunas antes de invocar a transformação. Quando é aberto o editor de transformação, caixas de verificação na parte superior de cada coluna indicam quais colunas são selecionadas como entradas. Pode adicionar ou remover colunas da seleção com as caixas de verificação nos cabeçalhos de coluna.

Para obter uma explicação mais detalhada do **coluna derivada por exemplo** transformação, juntamente com mais exemplos, veja [derivar coluna por referência de exemplo](data-prep-derive-column-by-example.md).  

## <a name="split-column-by-example"></a>Dividir coluna por exemplo
Essa transformação leva uma coluna existente e, por meio do mecanismo de "Por exemplo", tenta dividir essa coluna na *n* outras colunas. Pode executar a divisão de automática nas colunas geradas subsequentes.

Para obter uma explicação mais detalhada do **dividir coluna por exemplo** transformação, juntamente com mais exemplos, veja [dividir coluna por referência de exemplo](data-prep-split-column-by-example.md).

## <a name="expand-json"></a>Expandir o JSON

Essa transformação permite-lhe adicionar várias colunas ao expandir uma coluna com o texto JSON válido.

Para obter uma explicação mais detalhada do **expanda JSON** transformação, juntamente com mais exemplos, veja [referência expanda JSON](data-prep-expand-json.md).


## <a name="combine-columns-by-example"></a>Combinar colunas por exemplo

Essa transformação adiciona uma nova coluna ao combinar valores de várias colunas. 

Para obter uma explicação mais detalhada do **combinar colunas por exemplo** transformação, juntamente com mais exemplos, veja [combinar colunas por referência de exemplo](data-prep-combine-columns-by-example.md).


## <a name="duplicate-column"></a>Duplicar coluna
Essa transformação faz uma cópia exata de um ou mais colunas selecionadas e dá a cada um novo nome derivado do nome da coluna original.

## <a name="text-clustering"></a>Clustering de texto 
Essa transformação foi concebida para tirar inconsistentes valores que devem ser os mesmos e agrupá-los em conjunto.  

Com essa transformação, os valores numa única coluna são analisados de semelhanças e agrupados em clusters. Para cada cluster, existe um valor canônico, que é o valor que substitui todas as instâncias do cluster e todos os valores de instância. Clusters completas podem ser removidos e o valor canônico editá-lo. Instâncias podem ser removidas de um determinado cluster. O filtro de limiar de pontuação de semelhanças que utilizou para instâncias de grupo num cluster pode ser alterado.

Por padrão, essa transformação substitui todos os valores de instância de cluster com o valor canônico para esse cluster, criando uma nova coluna que contém os novos valores. Também pode adicionar a classificação de semelhança para cada instância para uma nova coluna (que pode ser nomeada) para utilização posterior no fluxo de dados.

## <a name="replace-values"></a>Substituir valores
Utilize essa transformação para substituir uma cadeia de caracteres com outra. A cadeia de caracteres de origem pode ser uma cadeia de caracteres parcial ou completa célula e a substituição pode aplicar-se a uma única coluna ou muitos. A cadeia de procura suporta carateres especiais, bem como para caracteres regulares a procurar. 

## <a name="replace-na-values"></a>Substitua os valores NA
Usar essa transformação para substituir as diversas formas de ND (n/d, NA, null, NaN, etc.), ou para substituir as cadeias de caracteres vazias com um valor único para torná-las consistentes. Essa transformação suporta uma ou várias colunas e apenas é apresentada quando é selecionada uma coluna. Não estiver presente no menu de transformação principal quando não foram seleccionadas colunas.

## <a name="replace-missing-values"></a>Substituir valores em falta
Essa transformação substitui dados em falta com um único valor, e suporta uma ou várias colunas. Essa transformação só é apresentada quando é selecionada uma coluna. Não estiver presente no menu de transformação principal quando não foram seleccionadas colunas.

## <a name="replace-error-values"></a>Substitua os valores de erro
Essa transformação substitui erros com um único valor, e ele oferece suporte a uma ou várias colunas. Essa transformação só é apresentada quando é selecionada uma coluna. Não estiver presente no menu de transformação principal quando não foram seleccionadas colunas.

## <a name="trim-string"></a>Cortar a cadeia de caracteres
Essa transformação remove e à direita "espaços em branco" carateres (incluindo espaços, tabulações, etc.) de uma ou mais colunas.

## <a name="adjust-precision"></a>Ajustar a precisão
Essa transformação define o número de casas decimais para uma coluna numérica.

## <a name="rename-column"></a>Mudar o nome de coluna
Essa transformação altera o nome da coluna selecionada. Pode também invocá-lo inline no cabeçalho da coluna ao clicar no nome da coluna.

## <a name="remove-column"></a>Remover coluna
Essa transformação remove as colunas selecionadas e funciona numa única coluna ou muitos. 

## <a name="keep-column"></a>Manter coluna
Essa transformação mantém apenas as colunas selecionadas e funciona numa única coluna ou muitos.

## <a name="handle-path-column"></a>Lidar com a coluna de caminho
Durante a importação de um ficheiro, é automaticamente adicionada uma coluna de caminho para o conjunto de dados pela **Adicionar origem de dados** funcionalidade. Ela contém o nome de ficheiro completamente qualificado que forma o caminho para o conjunto de dados. Essa transformação adiciona ou remove essa coluna extra do conjunto de dados.

## <a name="convert-field-type-to-numeric"></a>Converter o tipo de campo em numérico
Essa transformação altera o tipo de coluna em numérico. Pode especificar o separador para dados não inteiros. Por padrão, essa transformação não pedem para o separador, por isso, utilize o **editar** item de menu para invocar o editor. Essa transformação funciona numa única coluna ou muitos.

## <a name="convert-field-type-to-date"></a>Converter o tipo de campo de data
Essa transformação altera o tipo de coluna até à data. Um formato de data/hora padrão é usado, mas pode ser substituído utilizando `strftime` diretivas. Também pode preceder os valores time com uma data fixa.

Por padrão, essa transformação não pedem para o formato, por isso, utilize o **editar** item de menu no passo resultante para invocar o editor. Essa transformação funciona numa única coluna ou muitos.

Apenas as datas entre 9-22-1677 e 4-11-2262 pode ser convertido para o tipo de data.

## <a name="convert-field-type-to-boolean"></a>Converter o tipo de campo em booleano
Essa transformação altera o tipo de coluna para verdadeiro/falso. Ele oferece suporte a vários valores de mapeamento para true ou false, e pode editar estes mapeamentos. Também suporta uma constante para o qual pode mapear os valores que não existem nas tabelas de mapeamento de verdadeiro/falso. Essa transformação funciona numa única coluna ou muitos.

## <a name="convert-field-type-to-string"></a>Converter o tipo de campo de cadeia de caracteres
Essa transformação altera o tipo de coluna para a cadeia de caracteres, e ele funciona numa única coluna ou muitos.

## <a name="convert-unix-timestamp-to-datetime"></a>Converter Unix Timestamp para DateTime
Essa transformação compreende o formato de timestamp de Unix, mesmo que este é representado como uma cadeia de dados. Ele converte o carimbo de hora corretamente para um tipo de data na preparação de dados.

## <a name="filter"></a>Filtro
Essa transformação à oferece suporte à filtragem de linhas com base nos valores de uma ou várias colunas. As condições do filtro dependem do tipo de dados de cada coluna, para que pode filtrar colunas de cadeia de caracteres por "contém" ou "não contém". Colunas numéricas possam ser filtradas por "superior" ou "menor que" condições.

Quando o **filtro** transformação é invocada a partir do menu principal ou de clicar no cabeçalho de uma coluna, a opção para bifurcar as linhas com falhas em outro fluxo de dados está disponível. O fluxo de dados principal continua com filtrado **no** linhas e um novo fluxo de dados é criada que contém todas as linhas que foram filtradas **horizontalmente**.

## <a name="use-first-row-as-headers"></a>Utilizar primeira linha como cabeçalhos
Essa transformação promove a primeira linha do conjunto de dados para ser os nomes das colunas. Se algumas colunas não tiverem dados na primeira linha, é um nome gerado automaticamente. Usar essa transformação significa que a importação de dados é iniciado na linha 2 o mais brevemente possível. Consoante a **Ignorar linhas** definição, a importação pode começar mais longe baixo no conjunto de dados. Também pode utilizá-lo para remover a promoção e usar nomes gerado automaticamente.

## <a name="join"></a>Associar
Essa transformação é utilizada para associar o dois fluxos de dados em conjunto. Pode selecionar qual saída da associação para ser o resultado: as linhas com êxito da junção, o "left" linhas de associação ou o "direito" linhas da junção a falhar a falhar.

O **associação** transformação começa a partir de um fluxo de dados individual e seleciona esse fluxo de dados como um lado da associação. Em seguida, pede-lhe para selecionar outro fluxo de dados para o outro lado da associação. Depois de selecionar os dois fluxos, a transformação necessita de uma única coluna de cada lado da associação para ser selecionado a participar. Se a associação precisar de mais de uma coluna, crie uma coluna derivada antes de iniciar a transformação para criar uma coluna nova, concatenada para ser utilizada apenas para o associar. A transformação tenta sugerir chaves de associação e, se possível, gerar a coluna derivada automaticamente.

Após a associação estiver concluída, é apresentada uma vista de diagrama Sankey da associação. A largura das linhas em relação ao outro representa o número de linhas movendo em meio a essa parte do fluxo de associação. Com o painel à direita, pode selecionar as linhas com êxito, à esquerda de linhas ou à direita exclusivamente a falhar de linhas a falhar. Também pode escolher uma única ramificação.

## <a name="append-rows"></a>Acrescentar linhas
Essa transformação acrescenta dados a partir de outro fluxo de dados para o atual. Ele mapeia as colunas pela posição para adicionar novas linhas no final.

## <a name="append-columns"></a>Acrescentar colunas
Essa transformação acrescenta novas colunas a partir de outro fluxo de dados para atual. Ele adiciona novas colunas à direita. Não tenta linha dos dados em linhas.

## <a name="summarize"></a>resumir
Essa transformação calcula as agregações para a combinação de valores exclusivos nas colunas selecionadas uma ou mais. 

As agregações suportadas são: CONTAGEM, soma, MIN, Máx, média, VARIÂNCIA e desvio padrão. A lista de agregados para uma determinada coluna é filtrada para apenas os que se aplicam ao tipo de dados agregados. Os agregados não se aplicam estão desativados. Por exemplo, não é possível calcular a média de uma coluna de cadeia de caracteres, mas é possível calcular o mínimo e o máximo.

Quando o editor estiver disponível, arraste do cabeçalho da coluna cópia de segurança para o painel no canto superior esquerdo, onde são apresentadas as colunas a agregar. Este painel é hierárquica, então pode fazer agregados aninhados. O painel de editor na parte superior direita é utilizado para selecionar qual agregado para aplicar a uma coluna. Uma única coluna pode ser agregada de uma ou mais vezes. Depois de, pelo menos, um agregado tiver sido escolhido, a grade na parte inferior direita pré-visualizações os dados em sua forma agregada. 

Essa transformação é semelhante a um `ANSI-SQL GROUP BY`.

## <a name="remove-duplicates"></a>Remover duplicados
Essa transformação remove a linha inteira quando existem valores duplicados nas colunas selecionadas uma ou mais. Se não existem colunas são escolhidas, as somente as linhas removidas são aqueles em que todos os valores de coluna são os mesmos.

## <a name="sort"></a>Ordenar
Essa transformação classifica os dados. A classificação pode ser feita por uma única coluna ou muitos e cada coluna pode ser ordenada (predefinição) de ascendente ou descendente (o que pode ser alterada do editor).

Essa transformação é semelhante a um `ANSI-SQL ORDER BY`.

## <a name="output-transforms"></a>Transformações de saída
As seguintes transformações dados de saída. Pode ter vários blocos de escrita num único fluxo para escrever os dados em pontos diferentes.

### <a name="write-to-csv"></a>Escrever para CSV
Essa transformação escreve os dados em formato CSV do ponto atual no fluxo de dados. Ele controla a localização (local ou remota) e várias definições em todo o ficheiro.

### <a name="write-to-parquet"></a>Escrever no Parquet
Essa transformação escreve os dados em formato Parquet do ponto atual no fluxo de dados. Ele controla a localização (local ou remota) e várias definições em todo o ficheiro.

## <a name="script-based-transforms"></a>Transformações de baseada em script
As seguintes transformações usar script (Python) para executar a funcionalidade que está em falta no produto principal. 

>[!NOTE]
>Antes de utilizar qualquer um dessas transformações, leia [extensibilidade através de Python](data-prep-python-extensibility-overview.md).

### <a name="add-column-script"></a>Adicionar coluna (script)
Essa transformação adiciona uma coluna para os dados usando uma expressão de Python.
Para obter mais informações, consulte [código de Python de exemplo para efetuar a derivação de novas colunas](data-prep-appendix10-sample-custom-column-transforms-python.md).

### <a name="advanced-filter-script"></a>Filtro avançado (script)
Utilize essa transformação para escrever um filtro de nível de linha de Python.
Para obter mais informações, consulte [expressões de filtro de exemplo](data-prep-appendix6-sample-filter-expressions-python.md).

### <a name="transform-dataflow-script"></a>Transformar fluxos de dados (script)
Essa transformação aplica-se a Python para todo o conjunto de dados.
Para obter mais informações, consulte [transformações de fluxo de dados de transformação de exemplo](data-prep-appendix7-sample-transform-data-flow-python.md).

Essa transformação também pode aplicar Python a uma partição de dados inteiro.
Para obter mais informações, consulte [transformações de fluxo de dados de transformação de exemplo](data-prep-appendix7-sample-transform-data-flow-python.md).

### <a name="write-dataflow-script"></a>Escrever o fluxo de dados (script)
Essa transformação utiliza o Python para gravar todo um conjunto de dados.
Para obter mais informações, consulte [Python de exemplo para efetuar a derivação de novas colunas](data-prep-appendix9-sample-destination-connections-python.md).



