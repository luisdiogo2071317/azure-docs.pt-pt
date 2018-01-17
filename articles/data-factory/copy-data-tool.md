---
title: "Dados de cópia da ferramenta do Azure Data Factory | Microsoft Docs"
description: "Fornece informações sobre a ferramenta de copiar os dados na IU de fábrica de dados do Azure"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: 2fb25dcc0de4ebb1d025101670a9edfe3fe2bea9
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2018
---
# <a name="copy-data-tool-in-azure-data-factory"></a>Ferramenta de cópia de dados no Azure Data Factory
A ferramenta do Azure Data Factory copiar dados facilita e otimiza o processo de ingestão relacionados dados no data lake, que é normalmente um primeiro passo para um cenário de integração de dados ponto-a-ponto.  -Poupa tempo, especialmente quando utiliza a fábrica de dados do Azure para ingestão de dados de uma origem de dados pela primeira vez. Algumas das vantagens de utilizar esta ferramenta são:

- Quando utilizar a ferramenta de dados de cópia de fábrica de dados do Azure, não precisa de compreender as definições de fábrica de dados para os serviços ligados, conjuntos de dados, pipelines, atividades e acionadores. 
- O fluxo da ferramenta de dados de cópia é intuitivo para carregar dados para o data lake. A ferramenta cria automaticamente todos os recursos de fábrica de dados necessários para copiar dados do arquivo de dados de origem selecionado para o arquivo de dados de destino selecionada/sink. 
- A ferramenta de dados de cópia ajuda-o a validar os dados que estão a ser ingeridos no momento da criação, que ajuda a evitar quaisquer potenciais erros no início em si.
- Se precisar de implementar a lógica de negócio complexas para carregar dados para o data lake, ainda pode editar os recursos de fábrica de dados criados pela ferramenta de copiar os dados utilizando a por atividade criação de conteúdos na IU de fábrica de dados. 

A tabela seguinte fornece orientações sobre quando utilizar a ferramenta de cópia de dados vs por atividade criação de conteúdos na IU de fábrica de dados: 

| Ferramenta de cópia de dados | Por atividade (atividade de cópia) de criação |
| -------------- | -------------------------------------- |
| Pretende criar facilmente um dados ao carregar a tarefa sem saber mais sobre entidades do Azure Data Factory (serviços ligados, conjuntos de dados, pipelines, etc.) | Pretende implementar a lógica complexa e flexível para carregar dados para lake. |
| Pretende carregar rapidamente um grande número de artefactos de dados para o data lake. | Pretende encadeiam atividade de cópia com as atividades subsequentes de limpeza ou processamento de dados. |

Para iniciar a ferramenta de dados de cópia, clique em de **copiar dados** mosaico na home page da fábrica de dados.

![Página de introdução - ligação à ferramenta de dados de cópia](./media/copy-data-tool/get-started-page.png)


## <a name="intuitive-flow-for-loading-data-into-a-data-lake"></a>Fluxo intuitivo para carregar dados para o data lake
Esta ferramenta permite-lhe facilmente mover dados de uma ampla variedade de origens para destinos em minutos com um fluxo intuitivo:  

1. Configurar definições para o **origem**.
2. Configurar definições para o **destino**. 
3. Configurar **definições avançadas** para a operação de cópia como mapeamento de colunas, definições de desempenho e as definições de tolerância a falhas. 
4. Especifique um **agenda** para os dados ao carregar a tarefa. 
5. Reveja **resumo** de entidades do Data Factory a ser criado. 
6. **Editar** pipeline ao atualizar as definições para a atividade de cópia, conforme necessário. 

 A ferramenta é concebida com macrodados em mente desde o início, com suporte para diversos dados e os tipos de objeto. Pode utilizá-lo para mover centenas de pastas, ficheiros ou tabelas. A ferramenta suporta pré-visualização de dados automática, captura de esquema e mapeamento automático e dados, bem como a filtragem.

![Ferramenta de cópia de dados](./media/copy-data-tool/copy-data-tool.png)

## <a name="automatic-data-preview"></a>Pré-visualização de dados automática
Pode pré-visualizar a parte dos dados do arquivo de dados origem selecionada, que lhe permite validar os dados que estão a ser copiados. Além disso, se a origem de dados num ficheiro de texto, a ferramenta de dados de cópia analisa o ficheiro de texto para detetar automaticamente os delimitadores de linha e coluna e o esquema.

![Definições de ficheiros](./media/copy-data-tool/file-format-settings.png)

Após a deteção:

![Pré-visualização e definições de ficheiro detetado](./media/copy-data-tool/after-detection.png)

## <a name="schema-capture-and-automatic-mapping"></a>Captura de esquema e mapeamento automático
O esquema da origem de dados não pode ser igual ao esquema de destino de dados em muitos casos. Neste cenário, é necessário mapear colunas de esquema de origem para colunas de esquema de destino.

A ferramenta de dados de cópia monitoriza e aprende o seu comportamento quando o mapeamento de colunas entre os arquivos de origem e destino. Depois de escolher uma ou algumas colunas do arquivo de dados de origem e mapeá-los para o esquema de destino, a ferramenta de dados de cópia começa a analisar o padrão de pares de coluna selecionado de ambos os lados. Em seguida, aplica-se o mesmo padrão para o resto das colunas. Por conseguinte, pode ver todas as colunas tenham foi mapeadas para o destino de uma forma que útil logo a seguir alguns cliques.  Se não estiver satisfeito com a opção de mapeamento de colunas fornecida pela ferramenta de copiar os dados, pode ignorá-lo e continuar com o mapeamento manualmente as colunas. Entretanto, a ferramenta de dados de cópia constantemente aprende o padrão de atualizações e, em última análise atinge o padrão de direita para o mapeamento de colunas que pretende alcançar. 

> [!NOTE]
> Quando copiar dados do SQL Server ou SQL Database do Azure para o Azure SQL Data Warehouse, se a tabela não existe no arquivo de destino, a ferramenta de dados de cópia suporta a criação da tabela automaticamente utilizando o esquema de origem. 

## <a name="filter-data"></a>Dados de filtro
Pode filtrar os dados de origem para selecionar apenas os dados que tem de ser copiados para o arquivo de dados do sink. Filtragem reduz o volume dos dados a ser copiado para o arquivo de dados do sink e, por conseguinte, melhora o débito da operação de cópia. Ferramenta de cópia de dados fornece uma forma flexível de dados de filtro na base de dados relacional utilizando a linguagem de consulta SQL ou ficheiros numa pasta BLOBs do Azure. 

### <a name="filter-data-in-a-database"></a>Dados de filtro numa base de dados
A seguinte captura de ecrã mostra uma consulta SQL para filtrar os dados.

![Dados de filtro numa base de dados](./media/copy-data-tool/filter-data-in-database.png)

### <a name="filter-data-in-an-azure-blob-folder"></a>Dados de filtro numa pasta BLOBs do Azure
Pode utilizar variáveis no caminho da pasta para copiar dados a partir de uma pasta. As variáveis suportadas são: **{year}**, **{month}**, **{day}**, **{hora}**, e **{minuto}**. Por exemplo: inputfolder / {year} / {month} / {day}. 

Suponha que tem de entrada pastas no seguinte formato: 

```
2016/03/01/01
2016/03/01/02
2016/03/01/03
...
```

Clique em de **procurar** botão para **ficheiro ou pasta**, navegue para uma destas pastas (por exemplo, 2016->-03 -> 01 -> 02) e clique em **escolha**. Deverá ver 2016/03/01/02 na caixa de texto. 

Em seguida, substitua **2016** com **{year}**, **03** com **{month}**, **01** com **{day}** , e **02** com **{hora}**e prima a **separador** chave. Deverá ver na lista pendente para selecionar o formato para estas quatro variáveis:

![Filtro de ficheiro ou pasta](./media/copy-data-tool/filter-file-or-folder.png)

A ferramenta de cópia de dados gera os parâmetros com expressões, funções e variáveis do sistema que podem ser utilizadas para representar {year}, {month}, {day}, {hora} e {minuto} ao criar o pipeline. Para obter mais informações, consulte o [como de leitura ou escrita particionada dados](how-to-read-write-partitioned-data.md) artigo.

## <a name="scheduling-options"></a>Opções de agendamento
Pode executar a operação de cópia uma vez ou com base numa agenda (hora a hora, diariamente, e assim sucessivamente). Estas opções podem ser utilizadas para os conectores entre ambientes diferentes, incluindo no local, de nuvem e de ambiente de trabalho local. 

Uma operação de cópia de uso individual permite que o movimento de dados de uma origem para um destino apenas uma vez. Aplica-se aos dados de qualquer dimensão e qualquer formato suportado. A cópia agendada permite-lhe copiar dados de uma periodicidade que especificar. Pode utilizar as definições avançadas (como tentativas, o tempo limite e alertas) para configurar a cópia agendada.

![Opções de agendamento](./media/copy-data-tool/scheduling-options.png)


## <a name="next-steps"></a>Passos Seguintes
Experimente estes tutoriais que utilizam a ferramenta de cópia de dados:

- [Início rápido: criar uma fábrica de dados utilizando a ferramenta de dados de cópia](quickstart-create-data-factory-copy-data-tool.md)
- [Tutorial: copiar dados no Azure utilizando a ferramenta de dados de cópia](tutorial-copy-data-tool.md) 
- [Tutorial: copiar dados no local para o Azure utilizando a ferramenta de dados de cópia](tutorial-hybrid-copy-data-tool.md)
