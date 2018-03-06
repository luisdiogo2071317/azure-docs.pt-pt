---
title: "Preparar dados para classificar o tutorial Iris em serviços do Azure Machine Learning (pré-visualização) | Microsoft Docs"
description: "Este tutorial completo mostra como utilizar os serviços do Azure Machine Learning (pré-visualização) ponto a ponto. Esta é a parte um e fala sobre a preparação dos dados."
services: machine-learning
author: hning86
ms.author: haining, j-martens
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 02/28/2018
ms.openlocfilehash: 0bef557ee1394e3c786fd2c54e821b5dea28fabf
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="tutorial-classify-iris-part-1---preparing-the-data"></a>Tutorial: Classificar Íris, parte 1 – preparar os dados

Os serviços do Azure Machine Learning (pré-visualização) são uma solução de análise avançada e ciência de dados ponto a ponto integrada para os cientistas de dados profissionais prepararem dados, desenvolverem experimentações e implementarem modelos à escala da cloud.

Este tutorial é a primeira parte de uma série de três partes. Neste tutorial, vamos percorrer as noções básicas dos serviços do Azure Machine Learning (pré-visualização). Saiba como:

> [!div class="checklist"]
> * Criar um projeto no Azure Machine Learning Workbench
> * Criar um pacote de preparação de dados
> * Gerar código Python/PySpark para invocar um pacote de preparação de dados

Este tutorial utiliza o [conjunto de dados flor de Iris](https://en.wikipedia.org/wiki/Iris_flower_data_set) intemporal. As capturas de ecrã são específicas do Windows, mas a experiência de macOS é praticamente idêntica.

## <a name="prerequisites"></a>Pré-requisitos

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

Para concluir este tutorial, tem de ter:
- Uma conta de Experimentação do Azure Machine Learning
- O Azure Machine Learning Workbench instalado

Se ainda não tiver estes pré-requisitos, siga os passos do artigo [Início Rápido: instalar e iniciar](quickstart-installation.md) para configurar esta conta e instalar a aplicação Azure Machine Learning Workbench. 

## <a name="create-a-new-project-in-workbench"></a>Criar um projeto novo no Workbench

Se seguiu os passos do artigo [Início Rápido: instalar e iniciar](quickstart-installation.md), já deve ter este projeto e pode avançar para a secção seguinte.

1. Abra a aplicação Azure Machine Learning Workbench e inicie sessão, se necessário. 
   
   + No Windows, utilize o atalho do ambiente de trabalho **Machine Learning Workbench** para iniciá-lo. 
   + No macOS, selecione **Azure ML Workbench** no Launchpad.

1. Selecione o sinal de adição (+) no painel **PROJETOS** e escolha **Novo Projeto**.  

   ![Nova área de trabalho](media/tutorial-classifying-iris/new_ws.png)

1. Preencha os campos do formulário e selecione o botão **Criar** para criar um novo projeto no Workbench.

   Campo|Valor sugerido para o tutorial|Descrição
   ---|---|---
   Nome do projeto | myIris |Introduza um nome exclusivo que identifique a sua conta. Pode utilizar o seu nome ou o nome de um departamento ou projeto, aquele que melhor identifique a experimentação. O nome deve ter entre 2 e 32 carateres. Deve incluir apenas carateres alfanuméricos e o caráter de travessão (–). 
   Diretório do projeto | c:\Temp\ | Especifique o diretório no qual é criado o projeto.
   Descrição do projeto | _deixar em branco_ | Campo opcional útil para descrever os projetos.
   Visualstudio.com |_deixar em branco_ | Campo opcional. Um projeto pode, opcionalmente, ser associado a um repositório do Git no Visual Studio Team Services para controlo de código fonte e colaboração. [Saiba como configurar essa opção.](https://docs.microsoft.com/en-us/azure/machine-learning/preview/using-git-ml-project#step-3-set-up-a-machine-learning-project-and-git-repo) 
   Área de trabalho | IrisGarden (se existir) | Escolha uma área de trabalho que tenha criado para a sua conta de Experimentação no portal do Azure. <br/>Se seguiu o Início Rápido, deve ter uma área de trabalho com o nome IrisGarden. Caso contrário, selecione a que criou quando criou a conta de Experimentação ou qualquer outra que queira utilizar.
   Modelo de projeto | Classifying Iris | Os modelos contêm scripts e dados que pode utilizar para explorar o produto. Este modelo contém os scripts e dados de que precisa para este início rápido e para outros tutoriais neste site de documentação. 

   ![Novo projeto](media/tutorial-classifying-iris/new_project.png)
 
 É criado um novo projeto e o dashboard do projeto abre com esse projeto. Neste momento, pode explorar a home page, as origens de dados, os blocos de notas e os ficheiros de código de origem do projeto. 

## <a name="create-a-data-preparation-package"></a>Criar um pacote de preparação de dados

Nesta parte do tutorial, vai explorar os dados e iniciar o processo de preparação dos dados. Quando preparar os dados no Azure Machine Learning Workbench, uma representação JSON das transformações que executar no Workbench é armazenada num pacote de preparação de dados local (ficheiro *.dprep). Este pacote de preparação de dados é o contentor principal para o trabalho de preparação de dados no Workbench.

Este pacote de preparação de dados pode ser entregue para execução a um runtime, como, por exemplo, local-C#/CoreCLR, Scala/Spark ou Scala/HDI, onde o código é gerado para o runtime adequado para execução. 

1. Selecione o ícone de pasta para abrir a vista de ficheiro e, em seguida, selecione **iris.csv** para abrir esse ficheiro.  

   O ficheiro é uma tabela com cinco colunas e 150 linhas. Tem quatro colunas de funcionalidades numéricas e uma coluna de destino de cadeia. Não tem cabeçalhos de coluna.

   ![iris.csv](media/tutorial-classifying-iris/show_iris_csv.png)

   >[!NOTE]
   > Não inclua ficheiros de dados na pasta do projeto, especialmente quando o tamanho do ficheiro é grande. Incluímos o ficheiro **iris.csv** neste modelo para fins de demonstração, porque é muito pequeno. Para obter mais informações, veja [How to read and write large data files](how-to-read-write-files.md) (Como ler e escrever ficheiros de dados grandes).

2. Na **Vista de Dados**, selecione o sinal de mais (**+**) para adicionar uma origem de dados nova. É aberta a página **Adicionar Origem de Dados**. 

   ![Vista de dados](media/tutorial-classifying-iris/data_view.png)

3. Selecione **Ficheiros de Texto (*.csv, .json, .txt.,...)**  e clique em **Seguinte**.
   ![Origem de Dados](media/tutorial-classifying-iris/data-source.png)
   

4. Procure o ficheiro **iris.csv**e clique em **Seguinte**.  
 
   ![Selecionar iris](media/tutorial-classifying-iris/select_iris_csv.png)

   >[!IMPORTANT]
   >Certifique-se de que seleciona o ficheiro **iris.csv** no diretório do projeto atual para este exercício. Caso contrário, os passos posteriores poderão falhar.
   
5. Deixe os valores predefinidos e clique em **Concluir**.

6. É criado um ficheiro novo com o nome **iris-1.dsource**. É atribuído um nome exclusivo ao ficheiro com "-1", uma vez que o projeto de exemplo já vem com um ficheiro **iris.dsource** não numerado.  

   O ficheiro abre-se e os dados são apresentados. É automaticamente adicionada uma série de cabeçalhos de coluna, de **Column1** a **Column5**, a este conjunto de dados. Desloque o cursor até à parte inferior e repare que a última linha do conjunto de dados está vazia. A linha está vazia porque há uma quebra de linha adicional no ficheiro CSV.

   ![Vista de dados de iris](media/tutorial-classifying-iris/iris_data_view.png)

7. Selecione o botão **Métricas**. Observe os histogramas. Foi calculado para cada coluna um conjunto completo de estatísticas. Também pode selecionar o botão **Dados** para ver os dados novamente. 

   ![Vista de dados de iris](media/tutorial-classifying-iris/iris_metrics_view.png)

8. Selecione o botão **Preparar**. É aberta a caixa de diálogo **Preparar**. 

   O projeto de exemplo inclui um ficheiro **iris.dprep**. Por predefinição, pede-lhe para criar um novo fluxo de dados no pacote de preparação de dados **iris.dprep** que já existe. 

   Selecione **+ Novo Pacote de Preparação de Dados** no menu pendente, introduza um valor novo para o nome do pacote, utilize **iris-1** e, em seguida, selecione **OK**.

   ![Vista de dados de iris](media/tutorial-classifying-iris/new_dprep.png)

   É criado e aberto no editor de preparação de dados um pacote de preparação de dados novo com o nome **iris-1.dprep**.

9. Agora, vamos fazer uma preparação de dados simples. Selecione cada cabeçalho de coluna para tornar o texto do cabeçalho editável e mude o nome de cada coluna da seguinte forma: 

   Por ordem, introduza **Sepal Length**, **Sepal Width**, **Petal Length**, **Petal Width** e **Species** nas cinco colunas, respetivamente.

   ![Mudar o nome das colunas](media/tutorial-classifying-iris/rename_column.png)

10. Para contar valores distintos, selecione a coluna **Species** e clique com o botão direito do rato para selecioná-la. Selecione **Contagens de Valores** no menu pendente. 

   Esta ação abre o painel **Inspetores** abaixo dos dados. É apresentado um histograma com quatro barras. A coluna de destino tem três valores distintos: **Iris_virginica**, **Iris_versicolor**, **Iris-setosa** e um valor **(null)** (nulo).

   ![Selecione Contagens de Valores](media/tutorial-classifying-iris/value_count.png)

11. Para filtrar os valores nulos, selecione a etiqueta "Null" e selecione o sinal de subtração (**-**). Em seguida, a linha Null fica cinzenta para indicar que foi filtrada. 

   ![Histograma da contagem de valores](media/tutorial-classifying-iris/filter_out.png)

12. Tenha em conta os passos individuais detalhados no painel **PASSOS**. Quando mudou o nome das colunas e filtrou as linhas de valores nulos, cada ação foi registada como um passo de preparação de dados. Pode editar os passos individuais para ajustar as definições, reordenar os passos e remover os passos.

   ![Passos](media/tutorial-classifying-iris/steps.png)

13. Feche o editor de preparação de dados. Selecione **Fechar** (x) no separador **iris-1** com o ícone de gráfico. O seu trabalho é guardado automaticamente no ficheiro **iris-1.dprep** que é apresentado no cabeçalho **Preparações de Dados**.

## <a name="generate-pythonpyspark-code-to-invoke-a-data-preparation-package"></a>Gerar código Python/PySpark para invocar um pacote de preparação de dados

<!-- The output/results of a Package can be explored in Python or via a Jupyter Notebook. A Package can be executed across multiple runtimes including local Python, Spark (including in Docker), and HDInsight. A Package contains one or more Dataflows that are the steps and transforms applied to the data. A Package may use another Package as a Data Source (referred to as a Reference Data Flow). -->

1. Localize o ficheiro **iris-1.dprep** no separador Preparações de Dados.

1. Clique com o botão direito do rato no ficheiro **iris-1.dprep** e selecione **Gerar Ficheiro de Código de Acesso a Dados** no menu de contexto. 

   ![Gerar código](media/tutorial-classifying-iris/generate_code.png)

   Um novo ficheiro com o nome **iris-1.py** abre com as seguintes linhas de código para invocar a lógica que criou como um pacote de preparação de dados:

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

   Consoante o contexto em que este código é executado, `df` representa os vários tipos de pacotes de dados. É utilizado um [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html) quando é executado num runtime de Python ou um [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html) quando é executado num contexto do Spark. 
   
   Para saber como preparar dados no Azure Machine Learning Workbench, veja o guia [Get started with data preparation](data-prep-getting-started.md) (Introdução à preparação de dados).

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
