---
title: Importar dados para Machine Learning Studio titleSuffix: Descrição do Azure Machine Learning Studio: Como importar os dados para o Azure Machine Learning Studio a partir de várias origens de dados. Saiba quais tipos de dados e formatos de dados são suportados.
services: machine-learning ms.service: machine-learning ms.subservice: studio ms.topic: article

author: ericlicoding ms.author: amlstudiodocs ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro ms.date: 11/29/2017
---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a>Importar os seus dados de preparação para o Azure Machine Learning Studio a partir de várias origens de dados

Para utilizar os seus dados no Machine Learning Studio para desenvolver e formar uma solução de Análise Preditiva, pode utilizar dados a partir de: 

* R [ **ficheiro local** ](import-data-from-local-file.md) -carregar os dados locais antes do tempo do disco rígido para criar um módulo de conjunto de dados na sua área de trabalho
* [**Origens de dados online** ](import-data-from-online-sources.md) -utilize o [importar dados] [ import-data] módulo para aceder a dados a partir de uma das várias origens online durante a execução de sua experiência
* [**Experimentação do Studio de aprendizagem da máquina** ](import-data-from-an-experiment.md) -usar dados que foi guardados como um conjunto de dados no Machine Learning Studio
* [**Base de dados do SQL Server no local** ](use-data-from-an-on-premises-sql-server.md) -utilizar dados a partir de uma base de dados do SQL Server no local sem ter de copiar manualmente os dados

> [!NOTE]
> Um número de conjuntos de dados de exemplo estão disponíveis no Machine Learning Studio, que pode utilizar para os dados de treinamento. Para obter informações sobre elas, consulte [utilizar os conjuntos de dados de exemplo no Azure Machine Learning Studio](use-sample-datasets.md)).
> 
> 

Este artigo introdutório também aborda como obter dados pronto para utilização no Machine Learning Studio e descreve quais formatos de dados e os tipos de dados são compatíveis.

## <a name="get-data-ready-for-use-in-azure-machine-learning-studio"></a>Obter dados prontos a utilizar no Azure Machine Learning Studio
O Machine Learning Studio foi projetado para trabalhar com dados retangulares ou em tabela, como dados de texto delimitados ou estruturado de dados a partir de uma base de dados, embora em algumas circunstâncias os dados não retangulares podem ser usados.

É melhor se seus dados forem relativamente limpos. Ou seja, desejará se encarregar de problemas, como cadeias de caracteres unquoted antes de carregar os dados na sua experimentação.

No entanto, existem módulos disponíveis no Machine Learning Studio, que permitem alguma manipulação de dados dentro de sua experiência. Consoante os algoritmos de machine learning que irá utilizar, poderá ter de decidir como irá lidar com problemas de estrutural de dados, tais como valores em falta e dados dispersos, e existem módulos que podem ajudar com isso. Consulte a **transformação de dados** secção da paleta do módulo para módulos que executar estas funções.

Em qualquer ponto na sua experimentação pode ver ou transferir os dados que são produzidos por um módulo ao clicar na porta de saída. Dependendo do módulo, pode haver opções diferentes de download disponível ou poderá visualizar os dados dentro de seu navegador da web no Machine Learning Studio.

## <a name="data-formats-and-data-types-supported"></a>Tipos de dados e formatos de dados suportados
Pode importar um número de tipos de dados na sua experimentação, dependendo de qual mecanismo é utilizado para importar dados e de onde vem de:

* Texto sem formatação (. txt)
* Valores separados por vírgulas (CSV) com um cabeçalho (. csv) ou sem (. nh.csv)
* Separador valores separados (TSV) com um cabeçalho. (tsv) ou sem (. nh.tsv)
* Ficheiro do Excel
* Tabela do Azure
* Tabela do Hive
* Tabela de base de dados SQL
* Valores de OData
* Dados de SVMLight (.svmlight) (consulte a [SVMLight definição](http://svmlight.joachims.org/) para informações de formato)
* Atributo de dados do formato de ficheiro de relação (ARFF) (.arff) (consulte a [definição ARFF](http://weka.wikispaces.com/ARFF) para informações de formato)
* Ficheiro zip (. zip)
* Ficheiro de objeto ou área de trabalho de R (. RData)

Se importar dados num formato como ARFF que inclui metadados, o Machine Learning Studio usa esses metadados para definir o cabeçalho e o tipo de dados de cada coluna.

Se importar dados, tais como o formato TSV ou CSV, que não inclua estes metadados, o Machine Learning Studio infere o tipo de dados para cada coluna, os dados de amostragem. Se os dados também não tem cabeçalhos de coluna, o Machine Learning Studio dispõe nomes predefinidos.

É possível especificar explicitamente ou alterar os cabeçalhos e tipos de dados para colunas com o [Editar metadados][edit-metadata].

O seguinte procedimento **tipos de dados** são reconhecidos pelo Machine Learning Studio:

* String
* Número inteiro
* Double
* Booleano
* DateTime
* TimeSpan

O Machine Learning Studio utiliza um tipo de dados interno chamado ***tabela de dados*** para passar dados entre os módulos. Explicitamente pode converter os dados em formato de tabela de dados com o [converter para o conjunto de dados] [ convert-to-dataset] módulo.

Qualquer módulo que formatos que não seja a tabela de dados irá converter os dados à tabela de dados silenciosamente antes de passá-lo ao módulo seguinte.

Se necessário, pode converter o formato de tabela de dados no CSV, TSV, ARFF ou formato de SVMLight com outros módulos de conversão.
Consulte a **conversões de formato de dados** secção da paleta do módulo para módulos que executar estas funções.

<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
