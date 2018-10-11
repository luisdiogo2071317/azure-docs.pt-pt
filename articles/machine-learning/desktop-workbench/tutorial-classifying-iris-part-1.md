---
title: Preparar dados para classificar o tutorial Iris no serviço Azure Machine Learning (pré-visualização) | Microsoft Docs
description: Este tutorial completo mostra como utilizar o serviço Azure Machine Learning (pré-visualização) ponto a ponto. Esta é a parte um e fala sobre a preparação dos dados.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs, gcampanella
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 3/7/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 272b8250a80fee42780311dec92f6d47c221c160
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990166"
---
# <a name="tutorial-1-classify-iris---preparing-the-data"></a>Tutorial 1: Classificar Íris – Preparar os dados

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

O serviço Azure Machine Learning (pré-visualização) é uma solução de análise avançada e de ciência de dados ponto a ponto integrada orientada para cientistas de dados profissionais tendo em vista a preparação de dados, o desenvolvimento de experimentações e a implementação de modelos à escala da cloud.

Este tutorial é a **primeira parte de uma série composta por três partes**. Neste tutorial, irá percorrer as noções básicas do serviço Azure Machine Learning (pré-visualização) e aprender a:

> [!div class="checklist"]
> * Criar um projeto no Azure Machine Learning Workbench
> * Criar um pacote de preparação de dados
> * Gerar código Python/PySpark para invocar um pacote de preparação de dados

Este tutorial utiliza o [conjunto de dados flor de Iris](https://en.wikipedia.org/wiki/Iris_flower_data_set) intemporal. 

[!INCLUDE [aml-preview-note](../../../includes/aml-preview-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

Para concluir este tutorial, tem de ter:
- Uma conta de Experimentação do Azure Machine Learning
- O Azure Machine Learning Workbench instalado

Se ainda não tem estes pré-requisitos, siga os passos do artigo [Início Rápido: instalar e iniciar](quickstart-installation.md) para configurar as suas contas e instalar a aplicação Azure Machine Learning Workbench. 

## <a name="create-a-new-project-in-workbench"></a>Criar um projeto novo no Workbench

Se seguiu os passos do artigo [Início Rápido: instalar e iniciar](quickstart-installation.md), já deve ter este projeto e pode avançar para a secção seguinte.

1. Abra a aplicação Azure Machine Learning Workbench e inicie sessão, se necessário. 
   
   + No Windows, utilize o atalho do ambiente de trabalho **Machine Learning Workbench** para iniciá-lo. 
   + No macOS, selecione **Azure ML Workbench** no Launchpad.

1. Selecione o sinal de adição (+) no painel **PROJETOS** e escolha **Novo Projeto**.  

   ![Nova área de trabalho](./media/tutorial-classifying-iris/new_ws.png)

1. Preencha os campos do formulário e selecione o botão **Criar** para criar um novo projeto no Workbench.

   Campo|Valor sugerido para o tutorial|Descrição
   ---|---|---
   Nome do projeto | myIris |Introduza um nome exclusivo que identifique a sua conta. Pode utilizar o seu nome ou o nome de um departamento ou projeto, aquele que melhor identifique a experimentação. O nome deve ter entre 2 e 32 carateres. Deve incluir apenas carateres alfanuméricos e o caráter de travessão (–). 
   Diretório do projeto | c:\Temp\ | Especifique o diretório no qual é criado o projeto.
   Descrição do projeto | _deixar em branco_ | Campo opcional útil para descrever os projetos.
   URL do Repositório GIT Visualstudio.com |_deixar em branco_ | Campo opcional. Pode associar um projeto a um repositório do Git no Azure DevOps para fins de controlo de código fonte e colaboração. [Saiba como configurar essa opção](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/using-git-ml-project#step-3-set-up-a-machine-learning-project-and-git-repo). 
   Área de trabalho selecionada | IrisGarden (se existir) | Escolha uma área de trabalho que tenha criado para a sua conta de Experimentação no portal do Azure. <br/>Se seguiu o Início Rápido, deve ter uma área de trabalho com o nome IrisGarden. Caso contrário, selecione a que criou quando criou a conta de Experimentação ou qualquer outra que queira utilizar.
   Modelo de projeto | Classifying Iris | Os modelos contêm scripts e dados que pode utilizar para explorar o produto. Este modelo contém os scripts e dados de que precisa para este início rápido e para outros tutoriais neste site de documentação. 

   ![Novo projeto](media/tutorial-classifying-iris/new_project.png)
 
 É criado um novo projeto e o dashboard do projeto abre com esse projeto. Neste momento, pode explorar a home page, as origens de dados, os blocos de notas e os ficheiros de código de origem do projeto. 

   ![Abrir projeto](media/tutorial-classifying-iris/project-open.png)
 

## <a name="create-a-data-preparation-package"></a>Criar um pacote de preparação de dados

Em seguida, pode explorar e começar a preparar os dados no Azure Machine Learning Workbench. Cada transformação que efetuar no Workbench é armazenada em formato JSON num pacote de preparação de dados local (ficheiro *.dprep). Este pacote de preparação de dados é o contentor principal para o trabalho de preparação de dados no Workbench.

Este pacote de preparação de dados pode ser posteriormente entregue a um runtime como, por exemplo, C#/CoreCLR local, Scala/Spark ou Scala/HDI. 

1. Selecione o ícone de pasta para abrir a vista Ficheiros e, em seguida, selecione **iris.csv** para abrir esse ficheiro.

   Este ficheiro contém uma tabela com 5 colunas e 50 linhas. Quatro das colunas são colunas de funcionalidades numéricas. A quinta coluna é uma coluna de destino de cadeias. Nenhuma das colunas tem nomes de cabeçalho.

   ![iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   > Não inclua ficheiros de dados na pasta do projeto, especialmente quando o tamanho do ficheiro é grande. Uma vez que o ficheiro de dados **iris.csv** é muito pequeno, foi incluído neste modelo para fins de demonstração. Para obter mais informações, veja [How to read and write large data files](how-to-read-write-files.md) (Como ler e escrever ficheiros de dados grandes).

2. Na **Vista de dados**, selecione o sinal de adição (**+**) para adicionar uma origem de dados nova. É aberta a página **Adicionar Origem de Dados**. 

   ![Vista de dados no Azure Machine Learning Workbench](media/tutorial-classifying-iris/data_view.png)

3. Selecione **Ficheiros de Texto (\*.csv, \*.json, \*.txt., ...)** e clique em **Seguinte**.
   ![Origem de Dados no Azure Machine Learning Workbench](media/tutorial-classifying-iris/data-source.png)

4. Procure o ficheiro **iris.csv** e clique em **Concluir**. Este procedimento irá utilizar os valores predefinidos para os parâmetros, como o separador e os tipos de dados.

   >[!IMPORTANT]
   >Certifique-se de que seleciona o ficheiro **iris.csv** no diretório do projeto atual para este exercício. Caso contrário, os passos posteriores poderão falhar.
 
   ![Selecionar iris](media/tutorial-classifying-iris/select_iris_csv.png)
   
5. É criado um ficheiro novo com o nome **iris-1.dsource**. É atribuído um nome exclusivo ao ficheiro com "-1", uma vez que o projeto de exemplo já vem com um ficheiro **iris.dsource** não numerado.  

   O ficheiro abre-se e os dados são apresentados. É automaticamente adicionada uma série de cabeçalhos de coluna, de **Column1** a **Column5**, a este conjunto de dados. Desloque o cursor até à parte inferior e repare que a última linha do conjunto de dados está vazia. A linha está vazia porque há uma quebra de linha adicional no ficheiro CSV.

   ![Vista de dados de iris](media/tutorial-classifying-iris/iris_data_view.png)

1. Selecione o botão **Métricas**. Os histogramas são gerados e apresentados.

   Pode voltar à vista de dados ao selecionar o botão **Dados**.
   
   ![Vista de dados de iris](media/tutorial-classifying-iris/iris_data_view_metrics.png)

1. Observe os histogramas. Foi calculado para cada coluna um conjunto completo de estatísticas. 

   ![Vista de dados de iris](media/tutorial-classifying-iris/iris_metrics_view.png)

8. Comece a criar um pacote de preparação de dados ao selecionar o botão **Preparar**. É aberta a caixa de diálogo **Preparar**. 

   O projeto de exemplo contém um ficheiro de preparação de dados **iris.dprep** que está selecionado por predefinição. 

   ![Vista de dados de iris](media/tutorial-classifying-iris/prepare.png)

1. Crie um novo pacote de preparação de dados ao selecionar **+ Novo Pacote de Preparação de Dados** no menu pendente.

   ![Vista de dados de iris](media/tutorial-classifying-iris/prepare_new.png)

1. Introduza um novo valor para o nome do pacote (utilize **iris-1**) e, em seguida, selecione **OK**.

   É criado e aberto no editor de preparação de dados um pacote de preparação de dados novo com o nome **iris-1.dprep**.

   ![Vista de dados de iris](media/tutorial-classifying-iris/prepare_iris-1.png)

   Agora, vamos fazer uma preparação de dados simples. 

1. Selecione cada cabeçalho de coluna para tornar o texto do cabeçalho editável. Em seguida, mude o nome de cada coluna da seguinte forma: 

   Por ordem, introduza **Sepal Length**, **Sepal Width**, **Petal Length**, **Petal Width** e **Species** nas cinco colunas, respetivamente.

   ![Mudar o nome das colunas](media/tutorial-classifying-iris/rename_column.png)

1. Faça a contagem dos valores distintos:
   1. Selecione a coluna **Species**.
   1. Clique com o botão direito do rato para a selecionar. 
   1. Selecione **Contagens de Valores** no menu pendente. 

   O painel **Inspectors** é aberto por baixo dos dados. É apresentado um histograma com quatro barras. A coluna de destino tem quatro valores distintos: **Iris_virginica**, **Iris_versicolor**, **Iris-setosa** e um valor **(null)** (nulo).

   ![Selecione Contagens de Valores](media/tutorial-classifying-iris/value_count.png)

   ![Histograma da contagem de valores](media/tutorial-classifying-iris/filter_out.png)

1. Para filtrar os valores nulos, selecione a barra "(null)" e, em seguida, selecione o sinal de subtração (**-**). 

   Em seguida, a linha (null) aparece a cinzento para indicar que foi filtrada. 

   ![Filtrar valores nulos](media/tutorial-classifying-iris/filter_out2.png)

1. Repare nos passos de preparação de dados individuais descritos no painel **STEPS**. Quando mudou o nome das colunas e filtrou as linhas de valores nulos, cada ação foi registada como um passo de preparação de dados. Pode editar os passos individuais para ajustar as definições, reordenar os passos e remover os passos.

   ![Passos](media/tutorial-classifying-iris/steps.png)

1. Feche o editor de preparação de dados. Selecione o ícone **x** no separador **iris-1** com o ícone de gráfico para fechar o separador. O seu trabalho é guardado automaticamente no ficheiro **iris-1.dprep** que é apresentado no cabeçalho **Preparações de Dados**.

   ![Fechar](media/tutorial-classifying-iris/close.png)

## <a name="generate-pythonpyspark-code-to-invoke-a-data-preparation-package"></a>Gerar código Python/PySpark para invocar um pacote de preparação de dados

 O resultado de um pacote de preparação de dados pode ser explorado diretamente em Python ou num Bloco de Notas do Jupyter. Os pacotes podem ser executados em vários runtimes, incluindo o Python local, o Spark (inclusive no Docker) e o HDInsight. 

1. Localize o ficheiro **iris-1.dprep** no separador Preparações de Dados.

1. Clique com o botão direito do rato no ficheiro **iris-1.dprep** e selecione **Gerar Ficheiro de Código de Acesso a Dados** no menu de contexto. 

   ![Gerar código](media/tutorial-classifying-iris/generate_code.png)

   É aberto um novo ficheiro denominado **iris-1.py** com as seguintes linhas de código para invocar a lógica que criou como um pacote de preparação de dados:

   ```python
   # Use the Azure Machine Learning data preparation package
   from azureml.dataprep import package

   # Use the Azure Machine Learning data collector to log various metrics
   from azureml.logging import get_azureml_logger
   logger = get_azureml_logger()

   # This call will load the referenced package and return a DataFrame.
   # If run in a PySpark environment, this call returns a
   # Spark DataFrame. If not, it will return a Pandas DataFrame.
   df = package.run('iris-1.dprep', dataflow_idx=0)

   # Remove this line and add code that uses the DataFrame
   df.head(10)
   ```

   Consoante o contexto em que este código é executado, `df` representa um tipo de DataFrame diferente:
   + Quando a execução é realizada num runtime de Python, é utilizado um [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html).
   + Quando a execução é realizada num contexto de Spark, é utilizado um [DataFrame de Spark](https://spark.apache.org/docs/latest/sql-programming-guide.html). 
   
   Para saber mais sobre como preparar dados no Azure Machine Learning Workbench, veja o guia [Introdução à preparação de dados](data-prep-getting-started.md).

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, utilizou o Azure Machine Learning Workbench para:
> [!div class="checklist"]
> * Criar um novo projeto
> * Criar um pacote de preparação de dados
> * Gerar código Python/PySpark para invocar um pacote de preparação de dados

Está pronto para avançar para a próxima parte da série de tutoriais, na qual vai aprender a criar um modelo do Azure Machine Learning:
> [!div class="nextstepaction"]
> [Tutorial 2 - Criar modelos](tutorial-classifying-iris-part-2.md)
