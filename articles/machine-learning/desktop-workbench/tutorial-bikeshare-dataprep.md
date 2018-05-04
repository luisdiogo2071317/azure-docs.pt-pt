---
title: Tutorial de partilha de bicicletas - Preparação de dados avançada com o Azure Machine Learning Workbench
description: Neste tutorial, vai realizar uma tarefa de preparação de dados completa com o Azure Machine Learning Workbench
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 09/21/2017
ms.openlocfilehash: 51ed7dbead7f9085de563d2fb20c4e4a08f90bb2
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-use-azure-machine-learning-workbench-for-advanced-data-preparation-bike-share-data"></a>Tutorial: Utilizar o Azure Machine Learning Workbench para preparação de dados avançada dados de partilha de bicicletas)
O Azure Machine Learning (pré-visualização) é uma solução completa e integrada de análise avançada e de ciência de dados para os cientistas de dados profissionais prepararem dados, desenvolverem experimentações e implementarem modelos à escala da cloud.

Neste tutorial, vai utilizar o Machine Learning (pré-visualização) para aprender a:
> [!div class="checklist"]
> * Preparar dados de forma interativa com a ferramenta de preparação de dados do Machine Learning.
> * Importar, transformar e criar um conjunto de dados de teste.
> * Gerar um pacote de preparação de dados.
> * Executar o pacote de preparação de dados com o Python.
> * Gerar um conjunto de dados de formação ao reutilizar o pacote de preparação de dados para ficheiros de entrada adicionais.
> * Executar scripts numa janela local da CLI do Azure.
> * Executar scripts num ambiente do Azure HDInsight na cloud.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* Uma instalação local do Azure Machine Learning Workbench. Para obter mais informações, siga o [início rápido da instalação](../service/quickstart-installation.md).
* Se não tiver a CLI do Azure instalada, siga as instruções para [instalar a versão mais recente da CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* Um [cluster do HDInsights Spark](how-to-create-dsvm-hdi.md#create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal) criado no Azure.
* Uma conta de armazenamento do Azure.
* Conhecimentos sobre a criação de projetos novos no Workbench.
* Embora não seja necessário, é útil ter o [Explorador de Armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/) instalado, para que possa carregar, transferir e ver os blobs na sua conta de armazenamento.

## <a name="data-acquisition"></a>Aquisição de dados
Este tutorial utiliza o [conjunto de dados da Boston Hubway](https://s3.amazonaws.com/hubway-data/index.html) e os dados meteorológicos de Boston da [NOAA](http://www.noaa.gov/) (National Oceanic and Atmospheric Administration dos E.U.A.).

1. Transfira os ficheiros de dados a partir das ligações abaixo para o seu ambiente de desenvolvimento local:

   * [Boston weather data](https://azuremluxcdnprod001.blob.core.windows.net/docs/azureml/bikeshare/BostonWeather.csv) (Dados meteorológicos de Boston)

   * Dados de viagens da Hubway a partir do site da Hubway:

      - [201501-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201501-hubway-tripdata.zip)
      - [201504-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201504-hubway-tripdata.zip)
      - [201510-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201510-hubway-tripdata.zip)
      - [201601-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201601-hubway-tripdata.zip)
      - [201604-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201604-hubway-tripdata.zip)
      - [201610-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201610-hubway-tripdata.zip)
      - [201701-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201701-hubway-tripdata.zip)

2. Deszipe cada ficheiro após a transferência.

## <a name="upload-data-files-to-azure-blob-storage"></a>Carregar ficheiros de dados para o armazenamento de Blobs do Azure
Pode utilizar o armazenamento de Blobs do Azure para alojar os ficheiros de dados.

1. Utilize a mesma conta de armazenamento que é utilizada para o cluster do HDInsight que utiliza.

    ![Conta de armazenamento do cluster do HDInsight](media/tutorial-bikeshare-dataprep/hdinsightstorageaccount.png)

2. Crie um contentor novo com o nome **data-files** para armazenar os ficheiros de dados **BikeShare**.

3. Carregue os ficheiros de dados. Carregue `BostonWeather.csv` para uma pasta com o nome `weather`. Carregue os ficheiros de dados de viagens para uma pasta com o nome `tripdata`.

    ![Carregar os ficheiros de dados](media/tutorial-bikeshare-dataprep/azurestoragedatafile.png)

> [!TIP]
> Também pode utilizar o Explorador de Armazenamento para carregar blobs. Utilize esta ferramenta se também quiser ver os conteúdos dos ficheiros gerados no tutorial.

## <a name="learn-about-the-datasets"></a>Saiba mais sobre os conjuntos de dados
1. O ficheiro __Boston weather__ contém os campos relacionados com meteorologia abaixo, comunicados hora a hora:

   * **DATE**

   * **REPORTTPYE**

   * **HOURLYDRYBULBTEMPF**

   * **HOURLYRelativeHumidity**

   * **HOURLYWindSpeed**

2. Os dados da __Hubway__ estão organizados em ficheiros por ano e mês. Por exemplo, o ficheiro com o nome `201501-hubway-tripdata.zip` contém um ficheiro .csv que inclui os dados de janeiro de 2015. Os dados contêm os seguintes campos, sendo que cada linha representa uma viagem de bicicleta:

   * **Trip Duration (in seconds)** (Duração da Viagem [em segundos])

   * **Start Time and Date** (Data e Hora de Início)

   * **Stop Time and Date** (Data e Hora de Fim)

   * **Start Station Name & ID** (Nome e ID da Estação de Partida)

   * **End Station Name & ID** (Nome e ID da Estação de Chegada)

   * **Bike ID** (ID da Bicicleta)

   * **User Type (Casual = 24-Hour or 72-Hour Pass user; Member = Annual or Monthly Member)** [Tipo de Utilizador (Casual = utilizador com passe de 24 ou 72 horas; sócio = sócio anual ou mensal)]

   * **ZIP Code (if user is a member)** (Código Postal [se o utilizador for sócio)]

   * **Gender (self-reported by member)** (Sexo [comunicado pelo próprio sócio)]

## <a name="create-a-new-project"></a>Criar um novo projeto
1. Inicie o **Machine Learning Workbench** no menu Iniciar ou no iniciador.

2. Crie um projeto do Machine Learning novo. Selecione o botão **+**, na página **Projetos**, ou selecione **Ficheiro** > **Novo**.

   * Utilize o modelo **Bike Share**.

   * Dê ao projeto o nome **BikeShare**. 

## <a id="newdatasource"></a>Criar uma origem de dados nova

1. Crie uma origem de dados nova. Selecione o botão **Dados** (ícone de cilindro), na barra de ferramentas do lado esquerdo, para ver a vista **Dados**.

   ![Separador da vista de dados](media/tutorial-bikeshare-dataprep/navigatetodatatab.png)

2. Adicione uma origem de dados. Selecione o ícone **+** e, em seguida, selecione **Adicionar Origem de Dados**.

   ![Opção Adicionar origem de dados](media/tutorial-bikeshare-dataprep/newdatasource.png)

## <a name="add-weather-data"></a>Adicionar dados meteorológicos

1. **Arquivo de Dados**: selecione o arquivo de dados que contém os dados. Uma vez que está a utilizar ficheiros, selecione **File(s)/Directory** (Ficheiro(s)/Diretório). Selecione **Seguinte** para continuar.

   ![Entrada de File(s)/Directory](media/tutorial-bikeshare-dataprep/datasources.png)

2. **Seleção de Ficheiros**: adicione os dados meteorológicos. Procure e selecione o ficheiro `BostonWeather.csv` que carregou para o armazenamento de blobs anteriormente. Selecione **Seguinte**.

   ![Seleção de ficheiro com BostonWeather.csv selecionado](media/tutorial-bikeshare-dataprep/azureblobpickweatherdatafile.png)

3. **Detalhes do Ficheiro**: verifique que esquema de ficheiro é detetado. O Machine Learning Workbench analisa os dados no ficheiro e infere qual o esquema que vai ser utilizado.

   ![Verificar os detalhes dos ficheiros](media/tutorial-bikeshare-dataprep/fileparameters.png)

   > [!IMPORTANT]
   > O Workbench pode não detetar o esquema correto em alguns casos. Confirme sempre que os parâmetros estão certos para o seu conjunto de dados. Relativamente aos dados meteorológicos, verifique se estão definidos como os valores abaixo:
   >
   > * __Tipo de Ficheiro__: ficheiro delimitado (csv, tsv, txt, etc.)
   > * __Separador__: vírgula [,]
   > * __Caráter da Linha de Comentário__: nenhum valor definido.
   > * __Modo para Ignorar Linhas__: não ignorar
   > * __Codificação do Ficheiro__: utf-8
   > * __Modo de Promoção de Cabeçalhos__: utilizar os cabeçalhos do Primeiro Ficheiro

   A pré-visualização dos dados deverá apresentar as seguintes colunas:

   * **Caminho**

   * **DATE**

   * **REPORTTYPE**

   * **HOURLYDRYBULBTEMPF**
   
   * **HOURLYRelativeHumidity**

   * **HOURLYWindSpeed**

   Para continuar, selecione **Seguinte**.

4. **Tipos de Dados**: reveja os tipos de dados que são detetados automaticamente. O Machine Learning Workbench analisa os dados no ficheiro e infere os tipos de dados que vão ser utilizados.

   a. Para estes dados, altere **DATA TYPE** em todas as colunas para **String**.

   > [!NOTE]
   > String é utilizado para destacar as capacidades do Workbench mais à frente no tutorial. 

   ![Rever os tipos de dados](media/tutorial-bikeshare-dataprep/datatypedetection.png)

   b. Para continuar, selecione __Seguinte__. 

5. **Amostragem**: para criar um esquema de amostragem, selecione **Editar**. Selecione a linha __Primeiros 10 000__ que é adicionado e selecione __Editar__. Defina __Estratégia de Amostragem__ como **Ficheiro Completo** e selecione **Aplicar**.

   ![Adicionar uma estratégia de amostragem nova](media/tutorial-bikeshare-dataprep/weatherdatasamplingfullfile.png)

   Para utilizar a estratégia __Ficheiro Completo__, selecione a entrada __Ficheiro Completo__ e selecione __Definir como Ativo__. É apresentada uma estrela junto a __Ficheiro Completo__, para indicar que esta é a estratégia ativa.

   ![Ficheiro Completo como estratégia ativa](media/tutorial-bikeshare-dataprep/fullfileactive.png)

   Para continuar, selecione **Seguinte**.

6. **Coluna de Caminho**: utilize a secção __Coluna de Caminho__ para incluir o caminho do ficheiro completo como coluna nos dados importados. Selecione __Não Incluir Coluna de Caminho__.

   > [!TIP]
   > A inclusão do caminho como coluna é útil se estiver a importar uma pasta que tenha muitos ficheiros com diferentes nomes. Também é útil se os nomes dos ficheiros contiverem informações que pretenda extrair mais tarde.

   ![Coluna de Caminho definida para não ser incluída](media/tutorial-bikeshare-dataprep/pathcolumn.png)

7. **Concluir**: para concluir a criação da origem de dados, selecione o botão **Concluir**.

    É aberto um separador de origem de dados novo com o nome __BostonWeather__. É apresentado um exemplo dos dados numa vista de grelha. O exemplo baseia-se no esquema de amostragem ativo especificado anteriormente.

    Repare que o painel **Passos**, no lado direito do ecrã, apresenta as ações individuais realizadas durante a criação desta origem de dados.

   ![Apresentar a origem de dados, o exemplo e os passos](media/tutorial-bikeshare-dataprep/weatherdataloaded.png)

### <a name="view-data-source-metrics"></a>Ver as métricas da origem de dados

Selecione __Métricas__, no canto superior esquerdo da vista de grelha do separador. Esta vista mostra a distribuição e outras estatísticas agregadas dos dados de amostragem.

![Apresentação de métricas](media/tutorial-bikeshare-dataprep/weathermetrics.png)

> [!NOTE]
> Para configurar a visibilidade das estatísticas, utilize a lista pendente **Escolher Métrica**. Para alterar a vista de grelha, selecione ou anule a seleção de métricas aí.

Para regressar à vista __Dados__, selecione __Dados__, no canto superior esquerdo da página.

## <a name="add-a-data-source-to-the-data-preparation-package"></a>Adicionar uma origem de dados ao pacote de preparação de dados

1. Selecione __Preparar__ para iniciar a preparação dos dados. 

2. Quando lhe for pedido, introduza um nome para o pacote de preparação de dados, como, por exemplo, **BikeShare Data Prep**. 

3. Selecione __OK__ para continuar.

   ![Caixa de diálogo Preparar](media/tutorial-bikeshare-dataprep/dataprepdialog.png)

4. Aparece um pacote novo com o nome **BikeShare Data Prep**, na secção __Preparação de Dados__ do separador __Dados__. 

   Para apresentar o pacote, selecione esta entrada. 

5. Selecione o botão **>>** para expandir __Fluxos de Dados__ e mostrar os fluxos de dados presentes no pacote. Neste exemplo, o único fluxo de dados é __BostonWeather__.

   > [!IMPORTANT]
   > Os pacotes podem conter vários fluxos de dados.

   ![Fluxos de dados no pacote](media/tutorial-bikeshare-dataprep/weatherdataloadedingrid.png)

## <a name="filter-data-by-value"></a>Filtrar dados por valor
1. Para filtrar os dados, faça duplo clique numa célula com um determinado valor e selecione __Filtro__. Em seguida, selecione o tipo de filtro.

2. Neste tutorial, selecione uma célula que contenha o valor `FM-15`. Em seguida, defina o filtro **é igual a**.  Agora, os dados estão filtrados para devolverem apenas linhas em que __REPORTTYPE__ é `FM-15`.

   ![Caixa de diálogo Filtro](media/tutorial-bikeshare-dataprep/weatherfilterinfm15.png)

   > [!NOTE]
   > FM-15 é um tipo de Boletim Meteorológico METAR (Meteorological Terminal Aviation Routine Weather Report). Os boletins FM-15 são considerados empiricamente como sendo os mais completos, com poucos dados em falta.

## <a name="remove-a-column"></a>Remover uma coluna

Já não precisa da coluna __REPORTTYPE__. Clique com o botão direito do rato no cabeçalho da coluna e selecione **Remover Coluna**.

   ![Opção Remover Coluna](media/tutorial-bikeshare-dataprep/weatherremovereporttype.png)

## <a name="change-datatypes-and-remove-errors"></a>Alterar os tipos de dados e remover erros
1. Selecione Ctrl (Command ⌘ em Mac) enquanto seleciona os cabeçalhos das colunas para selecionar várias colunas ao mesmo tempo. Utilize esta técnica para selecionar os cabeçalhos seguintes:

   * **HOURLYDRYBULBTEMPF**

   * **HOURLYRelativeHumidity**

   * **HOURLYWindSpeed**

2. Clique com botão direito do rato num dos cabeçalhos de colunas selecionado e escolha **Converter Tipo de Campo em Numérico**. Esta opção converte o tipo de dados das colunas em numérico.

   ![Converter várias colunas num valor numérico](media/tutorial-bikeshare-dataprep/weatherconverttonumeric.png)

3. Filtre os valores de erro. Algumas colunas têm problemas de conversão de tipo de dados. Este problema é indicado pela cor vermelha na __Barra de Qualidade dos Dados__ das colunas em causa.

   Para remover as linhas com erros, clique com o botão direito do rato no cabeçalho de coluna **HOURLYDRYBULBTEMPF**. Selecione **Filtrar Coluna**. Utilize a opção predefinida **Pretendo** como **Manter Linhas**. Altere a lista pendente **Condições** para selecionar **não é erro**. Selecione **OK** para aplicar o filtro.

   ![Filtrar valores de erro](media/tutorial-bikeshare-dataprep/filtererrorvalues.png)

4. Para eliminar as restantes linhas de erros nas outras colunas, repita este processo de filtragem nas colunas **HOURLYRelativeHumidity** e **HOURLYWindSpeed**.

## <a name="use-by-example-transformations"></a>Utilizar transformações por exemplo

Para utilizar os dados de uma predição de blocos de tempo de duas horas, tem de calcular as condições meteorológicas médias para períodos de duas horas. Utilize as ações seguintes:

* Divida a coluna **DATE** em duas colunas separadas, **Date** e **Time**. Veja os passo detalhados na secção seguinte.

* Derive uma coluna **Hour_Range** a partir da coluna **Time**. Veja os passo detalhados na secção seguinte.

* Derive uma coluna **Date\_Hour\_Range** a partir das colunas **DATE** e **Hour_Range** . Veja os passo detalhados na secção seguinte.

### <a name="split-column-by-example"></a>Dividir coluna por exemplo

1. Divida a coluna **DATE** em duas colunas separadas, **Date** e **Time**. Clique com o botão direito do rato no cabeçalho de coluna **DATE** e selecione **Dividir Coluna por Exemplo**.

   ![Entrada Dividir Coluna por Exemplo](media/tutorial-bikeshare-dataprep/weathersplitcolumnbyexample.png)

2. O Machine Learning Workbench identifica automaticamente um delimitador relevante e cria duas colunas mediante a divisão dos dados em valores de data e hora. 

3. Selecione __OK__ para aceitar os resultados da operação de divisão.

   ![Dividir colunas DATE_1 e DATE_2](media/tutorial-bikeshare-dataprep/weatherdatesplitted.png)

### <a name="derive-column-by-example"></a>Derivar coluna por exemplo

1. Para derivar um intervalo de duas horas, clique com o botão direito do rato no cabeçalho de coluna __DATE\_2__ e selecione **Derivar Coluna por Exemplo**.

   ![Entrada Derivar Coluna por Exemplo](media/tutorial-bikeshare-dataprep/weatherdate2range.png)

   É adicionada uma coluna vazia nova com valores nulos.

2. Selecione a primeira célula vazia da coluna nova. Para indicar um exemplo do intervalo de tempo pretendido, escreva **12AM-2AM** na coluna nova e prima Enter.

   ![Coluna nova com o valor 12AM-2AM](media/tutorial-bikeshare-dataprep/weathertimerangeexample.png)

   > [!NOTE]
   > O Machine Learning Workbench sintetiza um programa com base nos exemplos que os utilizadores fornecem e aplica o mesmo programa às linhas restantes. Todas as outras linhas são preenchidas automaticamente com base no exemplo que tiver fornecido. Além disso, o Workbench também analisa os dados e tenta identificar casos extremos. 

   > [!IMPORTANT]
   > A identificação de casos extremos poderá não funcionar em Mac na versão atual do Workbench. Ignore os passos 3 e 4 seguintes em Mac. Em alternativa, selecione __OK__ depois de todas as linhas terem sido preenchidas com os valores derivados.
   
3. O texto **A Analisar Dados**, acima da grelha, indica que o Workbench está a tentar detetar casos extremos. Quando terminar, o estado muda para **Rever próxima linha sugerida** ou **Sem sugestões**. Neste exemplo, é devolvido **Rever próxima linha sugerida**.

4. Para rever as alterações sugeridas, selecione **Rever próxima linha sugerida**. A célula que deve rever e corrigir, se necessário, é realçada no ecrã.

   ![Rever próxima linha sugerida](media/tutorial-bikeshare-dataprep/weatherreviewnextsuggested.png)

    Selecione __OK__ para aceitar a transformação.
 
5. É reencaminhado para a vista de grelha dos dados de __BostonWeather__. A grelha contém agora as três colunas adicionadas anteriormente.

   ![Vista de grelha com linhas adicionadas](media/tutorial-bikeshare-dataprep/timerangecomputed.png)

   > [!TIP]
   > Todas as alterações que tiver feito são preservadas no painel **Passos**. Vá para o passo que criou no painel **Passos**, selecione a seta para baixo e selecione **Editar**. É apresentada a janela avançada para **Derivar coluna por Exemplo**. Todos os seus exemplos são preservados aqui. Também pode fazer duplo clique numa linha na grelha abaixo para adicionar exemplos manualmente. Selecione **Cancelar** para regressar à grelha principal sem aplicar as alterações. Pode aceder a esta vista igualmente ao selecionar **Modo avançado** enquanto faz uma transformação **Derivar Coluna por Exemplo**.

6. Para mudar o nome da coluna, faça duplo clique no cabeçalho da coluna e escreva **Hour Range**. Selecione Enter para guardar a alteração.

   ![Mudar o nome da coluna](media/tutorial-bikeshare-dataprep/weatherhourrangecolumnrename.png)

7. Para derivar o intervalo de datas e horas, selecione ao mesmo tempo as colunas **Date\_1** e **Hour Range**, clique com o botão direito do rato e selecione **Derivar Coluna por Exemplo**.

   ![Derivar Coluna por Exemplo](media/tutorial-bikeshare-dataprep/weatherderivedatehourrange.png)

   Escreva **Jan 01, 2015 12AM-2AM** como o exemplo na primeira linha e selecione Enter.

   O Workbench determina a transformação com base no exemplo que indicar. Neste exemplo, o resultado é que o formato de data é alterado e concatenado com o período de duas horas.

   ![Exemplo Jan 01, 2015 12AM-2AM](media/tutorial-bikeshare-dataprep/wetherdatehourrangeexample.png)

   > [!IMPORTANT]
   > Num Mac, siga o passo abaixo em vez do 8:
   >
   > * Aceda à primeira célula que contém **Feb 01, 2015 12AM-2AM**. Deve ser a linha 15. Corrija o valor para **Jan 02, 2015 12AM-2AM** e selecione Enter. 
   

8. Aguarde até que o estado passe de **A Analisar Dados** para **Rever próxima linha sugerida**. Esta alteração poderá demorar vários segundos. Selecione a ligação de estado para ir para a linha sugerida. 

   ![Linha sugerida a ser revista](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguate.png)

   A linha tem um valor nulo porque o valor de data de origem pode ser tanto dd/mm/aaaa, como mm/dd/aaaa. Escreva o valor correto, **Jan 13, 2015 2AM-4AM**, e selecione Enter. O Workbench utiliza os dois exemplos para melhorar a derivação para as linhas restantes.

   ![Dados formatados corretamente](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguated.png)

9. Selecione **OK** para aceitar a transformação.

   ![Grelha de transformação concluída](media/tutorial-bikeshare-dataprep/weatherdatehourrangecomputed.png)

   > [!TIP]
   > Para utilizar o **Modo avançado** de **Derivar Coluna por Exemplo** neste passo, selecione a seta para baixo no painel **Passos**. Na grelha de dados, existem caixas de verificação junto às colunas **DATE\_1** e **Hour Range**. Desmarque a caixa de verificação junto à coluna **Hour Range** para ver de que forma é que o resultado é alterado. Se a coluna **Hour Range** não estiver presente como entrada, **12AM-2AM** é tratado como uma constante e acrescentado aos valores derivados. Selecione **Cancelar** para regressar à grelha principal sem aplicar as alterações.
   ![Modo avançado](media/tutorial-bikeshare-dataprep/derivedcolumnadvancededitdeselectcolumn.png)

10. Para mudar o nome da coluna, faça duplo clique no cabeçalho. Mude o nome para **Date Hour Range** e selecione Enter.

11. Selecione ao mesmo tempo as colunas **DATE**, **DATE\_1**, **DATE\_2** e **Hour Range**. Clique com o botão direito do rato e selecione **Remover coluna**.

## <a name="summarize-data-mean"></a>Resumir os dados (média)

O passo seguinte é calcular a média dos valores, agrupados por intervalo de horas, para resumir as condições atmosféricas.

1. Selecione a coluna **Date Hour Range** e, no menu **Transformações**, selecione **Resumir**.

    ![Menu Transformações](media/tutorial-bikeshare-dataprep/weathersummarizemenu.png)

2. Para resumir os dados, arraste as colunas da grelha, na parte inferior da página, para os painéis do lado esquerdo e direito da parte superior. O painel do lado esquerdo contém o texto **Arrastar colunas para aqui para agrupar os dados**. O do lado direito contém o texto **Arrastar colunas para aqui para resumir os dados**. 

    a. Arraste a coluna **Date Hour Range** da grelha na parte inferior para o painel do lado esquerdo. Arraste **HOURLYDRYBULBTEMPF**, **HOURLYRelativeHumidity** e **HOURLYWindSpeed** para o painel do lado direito. 

    b. Neste painel, selecione **Média** como a medida **Agregada** para cada coluna. Selecione **OK** para concluir o resumo.

    ![Ecrã de dados resumidos](media/tutorial-bikeshare-dataprep/weathersummarize.png)

## <a name="transform-dataflow-by-using-script"></a>Transformar fluxos de dados com scripts

Alguns modelos podem convergir rapidamente se os dados nas colunas numéricas forem alterados para um intervalo de 0 a 1. Atualmente, não existe nenhuma transformação incorporada para fazer esta transformação genericamente. Utilize um script de Python para realizar esta operação.

1. No menu **Transformação**, selecione **Transformar Fluxo de Dados (Script)**.

2. Introduza o código seguinte na caixa de texto que aparece. Se tiver utilizado os nomes das colunas, o código deverá funcionar sem modificações. Vai escrever uma lógica de normalização mínima-máxima simples no Python.

    > [!WARNING]
    > O script espera os nomes das coluna utilizados anteriormente neste tutorial. Se tiver outros nomes de colunas, tem alterá-los no script.

   ```python
   maxVal = max(df["HOURLYDRYBULBTEMPF_Mean"])
   minVal = min(df["HOURLYDRYBULBTEMPF_Mean"])
   df["HOURLYDRYBULBTEMPF_Mean"] = (df["HOURLYDRYBULBTEMPF_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYDRYBULBTEMPF_Mean":"N_DryBulbTemp"},inplace=True)

   maxVal = max(df["HOURLYRelativeHumidity_Mean"])
   minVal = min(df["HOURLYRelativeHumidity_Mean"])
   df["HOURLYRelativeHumidity_Mean"] = (df["HOURLYRelativeHumidity_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYRelativeHumidity_Mean":"N_RelativeHumidity"},inplace=True)

   maxVal = max(df["HOURLYWindSpeed_Mean"])
   minVal = min(df["HOURLYWindSpeed_Mean"])
   df["HOURLYWindSpeed_Mean"] = (df["HOURLYWindSpeed_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYWindSpeed_Mean":"N_WindSpeed"},inplace=True)

   df
   ```

    > [!TIP]
    > O script de Python tem de devolver `df` no final. Este valor é utilizado para preencher a grelha.
    
   ![Caixa de diálogo de transformação de fluxo de dados (Script)](media/tutorial-bikeshare-dataprep/transformdataflowscript.png)

3. Selecione __OK__ para utilizar o script. As colunas numéricas na grelha contêm agora valores no intervalo 0 a 1.

    ![Grelha que contém valores entre 0 e 1](media/tutorial-bikeshare-dataprep/datagridwithdecimals.png)

Concluiu a preparação dos dados meteorológicos. Em seguida, prepare os dados da viagem.

## <a name="load-trip-data"></a>Carregar dados da viagem

1. Para importar o ficheiro `201701-hubway-tripdata.csv`, siga os passos na secção [Criar nova Origem de Dados](#newdatasource). Utilize as opções abaixo durante o processo de importação:

    * __Seleção de Ficheiro__: selecione **Blob do Azure** quando navegar para selecionar o ficheiro.

    * __Esquema de Amostragem__: selecione o esquema de amostragem **Ficheiro Completo** e ative a amostra.

    * __Tipo de Dados__: aceite as predefinições.

2. Depois de importar os dados, selecione __Preparar__ para iniciar a preparação dos mesmos. Selecione o pacote **BikeShare Data Prep.dprep** já existente e selecione __OK__.

    Este processo adiciona um **Fluxo de Dados** ao ficheiro **Data Preparation** já existente em vez de criar um novo.

    ![Selecione o pacote existente](media/tutorial-bikeshare-dataprep/addjandatatodprep.png)

3. Após a grelha ter sido carregada, expanda __DATAFLOWS__. Existem agora dois fluxos de dados: **BostonWeather** e **201701-hubway-tripdata**. Selecione a entrada **201701-hubway-tripdata**.

    ![201701-hubway-tripdata entry](media/tutorial-bikeshare-dataprep/twodfsindprep.png)

## <a name="use-the-map-inspector"></a>Utilizar o inspetor de mapas

Para a preparação de dados, estão disponíveis visualizações úteis, denominadas inspetores, para dados de cadeia, numéricos e geográficos. Podem ajudá-lo a compreender melhor os dados e a identificar valores atípicos. Siga os passos abaixo para utilizar o inspetor de mapas.

1. Selecione ao mesmo tempo as colunas **start station latitude** e **start station longitude**. Clique com o botão direito do rato numa das colunas e selecione **Mapa**.

    > [!TIP]
    > Para permitir a seleção ao mesmo tempo, mantenha premida a tecla Ctrl (comando ⌘ em Mac) e selecione o cabeçalho de cada coluna.

    ![Visualização do mapa](media/tutorial-bikeshare-dataprep/launchMapInspector.png)

2. Para maximizar a visualização dos mapas, selecione o ícone **Maximizar**. Para que o mapa se ajuste à janela, selecione o ícone **E**, no canto superior esquerdo da visualização.

    ![Imagem maximizada](media/tutorial-bikeshare-dataprep/maximizedmap.png)

3. Selecione botão **Minimizar** para regressar à vista de grelha.

## <a name="use-the-column-statistics-inspector"></a>Utilizar o inspetor de estatísticas de coluna

Para utilizar o inspetor de estatísticas de coluna, clique com o botão direito do rato na coluna __tripduration__ e selecione __Estatísticas de Coluna__.

![Entrada de estatísticas de coluna](media/tutorial-bikeshare-dataprep/tripdurationcolstats.png)

Este processo adiciona uma visualização nova com o nome __Estatísticas de tripduration__, no painel __INSPETORES__.

![O inspetor de estatísticas de tripduration](media/tutorial-bikeshare-dataprep/tripdurationcolstatsinwell.png)

> [!IMPORTANT]
> O valor máximo da duração da viagem são 961 814 minutos, o que equivale a, aproximadamente, dois anos. Parece que existem alguns valores atípicos no conjunto de dados.

## <a name="use-the-histogram-inspector"></a>Utilizar o inspetor de histogramas

Para tentar identificar valores atípicos, clique com botão direito do rato na coluna __tripduration__ e selecione __Histograma__.

![Inspetor de Histogramas](media/tutorial-bikeshare-dataprep/tripdurationhistogram.png)

O histograma não é útil porque os valores atípicos distorcem o gráfico.

## <a name="add-a-column-by-using-script"></a>Utilizar um script para adicionar uma coluna

1. Clique com o botão direito do rato na coluna **tripduration** e selecione **Adicionar Coluna (Script)**.

    ![Menu Adicionar Coluna (Script)](media/tutorial-bikeshare-dataprep/computecolscript.png)

2. Na caixa de diálogo __Adicionar Coluna (Script)__, utilize os valores seguintes:

    * __Nome da Coluna Nova__: logtripduration

    * __Inserir esta Coluna Nova a Seguir a__: tripduration

    * __Código da Coluna Nova__: `math.log(row.tripduration)`

    * __Tipo de Bloco de Código__: expressão

   ![Caixa de diálogo Adicionar Coluna (Script)](media/tutorial-bikeshare-dataprep/computecolscriptdialog.png)

3. Selecione __OK__ para adicionar a coluna **logtripduration**.

4. Clique com o botão direito do rato na coluna e selecione **Histograma**.

    ![Histograma da coluna logtripduration](media/tutorial-bikeshare-dataprep/logtriphistogram.png)

    Visualmente, parece que este histograma é uma distribuição normal com uma cauda anormal.

## <a name="use-an-advanced-filter"></a>Utilizar um filtro avançado

A utilização de um filtro nos dados atualiza os inspetores com a distribuição nova. 

1. Clique com o botão direito do rato na coluna **logtripduration** e selecione **Filtrar Coluna**. 

2. Na caixa de diálogo __Editar__, utilize os valores seguintes:

    * __Filtrar esta Coluna Numérica__: logtripduration

    * __Pretendo__: Manter as Linhas

    * __Quando__: qualquer uma das Condições abaixo for Verdadeira (OU lógico)

    * __Se esta coluna__: inferior a

    * __O valor__: 9

    ![Opções de filtragem](media/tutorial-bikeshare-dataprep/loftripfilter.png)

3. Selecione __OK__ para aplicar o filtro.

    ![Histogramas atualizados depois de o filtro ser aplicado](media/tutorial-bikeshare-dataprep/loftripfilteredinspector.png)

### <a name="the-halo-effect"></a>O efeito de halo

1. Maximize o histograma **logtripduration**. Existe um histograma azul sobreposto num cinzento. Este ecrã é denominado o **Efeito Halo**:

    * O histograma cinzento representa a distribuição antes da operação (neste caso, a operação de filtragem).

    * O histograma azul representa o histograma após a operação. 

   O efeito de halo ajuda a visualizar o efeito de uma operação nos dados.

   ![O efeito de halo](media/tutorial-bikeshare-dataprep/loftripfilteredinspectormaximized.png)

    > [!NOTE]
    > O histograma azul aparece mais curto comparado com o anterior. Essa diferença deve-se à recriação automática de registos dos dados no intervalo novo.

2. Para remover o halo, selecione __Editar__ e limpe __Mostrar halo__.

    ![Opções para o histograma](media/tutorial-bikeshare-dataprep/uncheckhalo.png)

3. Selecione **OK** para desativar o efeito de halo. Em seguida, minimize o histograma.

### <a name="remove-columns"></a>Remover colunas

Nos dados de viagens, cada linha representa um evento de recolha de bicicletas. Neste tutorial, só precisa das colunas **starttime** e **start station id**. Para remover as outras colunas, selecione ao mesmo tempo estas duas colunas, clique com o botão direito do rato no cabeçalho da coluna e selecione **Manter Coluna**. As outras colunas são removidas.

![Opção Manter Coluna](media/tutorial-bikeshare-dataprep/tripdatakeepcolumn.png)

### <a name="summarize-data-count"></a>Resumir os dados (contagem)

Para resumir a procura por bicicletas durante um período de duas horas, utilize colunas derivadas.

1. Clique com o botão direito do rato na coluna **starttime** e selecione **Derivar Coluna por Exemplo**.

    ![Opção Derivar Coluna por Exemplo](media/tutorial-bikeshare-dataprep/tripdataderivebyexample.png)

2. No exemplo, introduza o valor **Jan 01, 2017 12AM-2AM** na primeira linha.

    > [!IMPORTANT]
    > No exemplo anterior de colunas derivadas, utilizou vários passos para derivar uma coluna que continha o período de data e hora. Neste exemplo, é possível ver que esta operação pode ser executada como um único passo ao indicar um exemplo do resultado final.

    > [!NOTE]
    > Pode dar um exemplo em relação a qualquer uma das linhas. Neste exemplo, o valor **Jan 01, 2017 12AM-2AM** é válido na primeira linha de dados.

    ![Dados de exemplo](media/tutorial-bikeshare-dataprep/tripdataderivebyexamplefirstexample.png)

   > [!IMPORTANT]
   > Num Mac, siga este passo em vez do 3:
   >
   > * Aceda à primeira célula que contém **Jan 01, 2017 1AM-2AM**. Deve ser a linha 14. Corrija o valor para **Jan 01, 2017 12AM-2AM** e selecione Enter. 

3. Aguarde até que a aplicação calcule os valores de todas as linhas. O processo poderá demorar vários segundos. Depois de concluída a análise, utilize a ligação __Rever próxima linha sugerida__ para rever os dados.

   ![Análise concluída com ligação de revisão](media/tutorial-bikeshare-dataprep/tripdatabyexanalysiscomplete.png)

    Confirme que os valores calculados estão corretos. Se não estiverem, atualize-os com os valores esperados e selecione Enter. Depois, aguarde até que a análise seja concluída. Conclua o processo **Rever próxima linha sugerida** até ver **Nenhuma sugestão**. **Nenhuma sugestão**, significa que a aplicação analisou os casos extremos e está satisfeita com o programa sintetizado. Antes de aceitar a transformação, é melhor prática fazer uma inspeção visual dos dados transformados. 

4. Selecione **OK** para aceitar a transformação. Mude o nome da coluna criada recentemente para **Date Hour Range**.

    ![Coluna com Nome Mudado](media/tutorial-bikeshare-dataprep/tripdatasummarize.png)

5. Clique com o botão direito do rato no cabeçalho da coluna **starttime** e selecione **Remover coluna**.

6. Para resumir os dados, no menu __Transformar__, selecione __Resumir__. Para criar a transformação, utilize os passos abaixo:

    * Arraste __Date Hour Range__ e __start station id__ para o painel **Agrupar Por** do lado esquerdo.

    * Arraste __start station id__ para o painel **resumir dados** do lado direito.

   ![Opções de resumo](media/tutorial-bikeshare-dataprep/tripdatacount.png)

7. Selecione **OK** para aceitar o resultado do resumo.

## <a name="join-dataflows"></a>Associar fluxos de dados

Para associar os dados meteorológicos aos dados de viagens, utilize os seguintes passos:

1. No menu __Transformar__, selecione __Associar__.

2. __Tabelas__: selecione **BostonWeather** como o fluxo de dados do lado **Esquerdo** e **201701-hubway-tripdata** como o do lado **Direito**. Para continuar, selecione **Seguinte**.

    ![Seleções de tabelas](media/tutorial-bikeshare-dataprep/jointableselection.png)

3. __Colunas de Chaves__: selecione a coluna **Date Hour Range** em ambas as tabelas e selecione __Seguinte__.

    ![Seleções de colunas de chave](media/tutorial-bikeshare-dataprep/joinkeyselection.png)

4. __Tipo de Associação__: selecione __Linhas correspondentes__ como o tipo de associação e selecione __Concluir__.

    ![Tipo de associação de linhas correspondentes](media/tutorial-bikeshare-dataprep/joinscreen.png)

    Este processo cria fluxo de dados novo com o nome __Associar Resultado__.

## <a name="create-additional-features"></a>Criar características adicionais

1. Para criar uma coluna que contenha o dia da semana, clique com o botão direito do rato na coluna **Date Hour Range** e selecione **Derivar Coluna por Exemplo**. Utilize um valor como __Sun__ (Dom.) para uma data que tenha ocorrido num domingo. Por exemplo, **Jan 01, 2017 12AM-2AM**. Selecione Enter e selecione **OK**. Mude o nome desta coluna para __Weekday__.

    ![Criar coluna nova para dia da semana](media/tutorial-bikeshare-dataprep/featureweekday.png)

2. Para criar uma coluna que contenha o período de tempo de uma linha, clique com o botão direito do rato na coluna **Date Hour Range** e selecione **Derivar Coluna por Exemplo**. Utilize o valor **12AM-2AM** na linha que contém **Jan 01, 2017 12AM-2AM**. Selecione Enter e selecione **OK**. Mude o nome desta coluna para **Period**.

    ![Coluna de período](media/tutorial-bikeshare-dataprep/featurehourrange.png)

3. Para remover as colunas **Date Hour Range** e **r_Date Hour Range**, selecione Ctrl (comando ⌘ em Mac) e selecione cada cabeçalho de coluna. Clique com o botão direito do rato e selecione **Remover Coluna**.

## <a name="read-data-from-python"></a>Ler dados a partir do Python

Pode executar um pacote de preparação de dados a partir de Python ou PySpark e obtenha os resultados como um **Data Frame**.

Para gerar um script de Python de exemplo, clique com o botão direito do rato em __BikeShare Data Prep__ e selecione __Gerar Ficheiro de Código de Acesso a Dados__. O ficheiro de Python de exemplo é criado na sua **Pasta do Projeto** e também é carregado num separador do Workbench. O script de Python seguinte é um exemplo do código que é gerado:

```python
# Use the Azure Machine Learning data preparation package
from azureml.dataprep import package

# Use the Azure Machine Learning data collector to log various metrics
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()

# This call will load the referenced package and return a DataFrame.
# If run in a PySpark environment, this call returns a
# Spark DataFrame. If not, it will return a Pandas DataFrame.
df = package.run('BikeShare Data Prep.dprep', dataflow_idx=0)

# Remove this line and add code that uses the DataFrame
df.head(10)
```

Neste tutorial, o nome do ficheiro é `BikeShare Data Prep.py`. Este ficheiro é utilizado mais adiante no tutorial.

## <a name="save-test-data-as-a-csv-file"></a>Guardar os dados de teste como ficheiro CSV

Para guardar o fluxo de dados **Associar Resultado** num ficheiro .csv, tem de alterar o script `BikeShare Data Prep.py`. 

1. Abra o projeto para edição no Visual Studio Code.

    ![Abra o projeto no Visual Studio Code](media/tutorial-bikeshare-dataprep/openprojectinvscode.png)

2. Utilize o código abaixo para atualizar o script de Python no ficheiro `BikeShare Data Prep.py`:

    ```python
    import pyspark

    from azureml.dataprep.package import run
    from pyspark.sql.functions import *

    # start Spark session
    spark = pyspark.sql.SparkSession.builder.appName('BikeShare').getOrCreate()

    # dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
    df = run('BikeShare Data Prep.dprep', dataflow_idx=2)
    df.show(n=10)
    row_count_first = df.count()

    # Example file name: 'wasb://data-files@bikesharestorage.blob.core.windows.net/testata'
    # 'wasb://<your container name>@<your azure storage name>.blob.core.windows.net/<csv folder name>
    blobfolder = 'Your Azure Storage blob path'

    df.write.csv(blobfolder, mode='overwrite') 

    # retrieve csv file parts into one data frame
    csvfiles = "<Your Azure Storage blob path>/*.csv"
    df = spark.read.option("header", "false").csv(csvfiles)
    row_count_result = df.count()
    print(row_count_result)
    if (row_count_first == row_count_result):
        print('counts match')
    else:
        print('counts do not match')
    print('done')
    ```

3. Substitua `Your Azure Storage blob path` pelo caminho para o ficheiro de saída que vai ser criado. Substitua nas variáveis `blobfolder` e `csvfiles`.

## <a name="create-an-hdinsight-run-configuration"></a>Criar uma configuração de execução de HDInsight

1. No Machine Learning Workbench, abra a janela da linha de comandos, selecione o menu **Ficheiro** e selecione **Abrir Linha de Comandos**. A linha de comandos inicia-se na pasta de projeto com o comando `C:\Projects\BikeShare>`.

    ![Abra uma linha de comandos](media/tutorial-bikeshare-dataprep/opencommandprompt.png)

   >[!IMPORTANT]
   >Tem de utilizar a janela da linha de comandos (aberta no Workbench) para realizar os passos seguintes.

2. Utilize a linha de comandos para iniciar sessão no Azure. 

   A aplicação Workbench e a CLI utilizam caches de credenciais independentes quando se autentica em recursos do Azure. Só tem de o fazer uma vez até que o token em cache expire. O comando `az account list` devolve a lista de subscrições disponíveis para o seu início de sessão. Se houver mais de uma, utilize o valor do ID da subscrição pretendida. Defina essa subscrição como a conta predefinida para utilização com o comando `az account set -s` e indique o valor do ID da mesma. Em seguida, utilize o comando `show` de conta para confirmar a definição.

   ```azurecli
   REM login by using the aka.ms/devicelogin site
   az login
   
   REM lists all Azure subscriptions you have access to 
   az account list -o table
   
   REM sets the current Azure subscription to the one you want to use
   az account set -s <subscriptionId>
   
   REM verifies that your current subscription is set correctly
   az account show
   ```

3. Crie a configuração da execução do HDInsight. Precisa do nome do cluster e da palavra-passe `sshuser`.

    ```azurecli
    az ml computetarget attach cluster --name hdinsight --address <yourclustername>.azurehdinsight.net --username sshuser --password <your password>
    az ml experiment prepare -c hdinsight
    ```
> [!NOTE]
> Quando é criado um projeto em branco, as configurações de execução predefinidas são **local** e **docker**. Este passo cria uma configuração de execução nova que está disponível no Workbench quando executa os seus scripts. 

## <a name="run-in-an-hdinsight-cluster"></a>Executar num cluster do HDInsight

Regresse à aplicação Machine Learning Workbench para executar o script no cluster do HDInsight.

1. Volte para o ecrã principal do projeto ao selecionar o ícone **Página Inicial**, no lado esquerdo.

2. Selecione **hdinsight** na lista pendente para executar o script no cluster do HDInsight.

3. Selecione **Executar**. O script é submetido como um trabalho. O estado do trabalho muda para __Concluído__ após o ficheiro ter sido escrito na localização especificada no seu contentor de armazenamento.

    ![Executar o script do HDInsight](media/tutorial-bikeshare-dataprep/hdinsightrunscript.png)


## <a name="substitute-data-sources"></a>Substitui as origens de dados

Nos passos anteriores, utilizou as origens de dados `201701-hubway-tripdata.csv` e `BostonWeather.csv` para preparar os dados de teste. Para utilizar o pacote com os outros ficheiros de dados de viagens, utilize os passos seguintes:

1. Utilize os passos anteriores para criar uma origem de dados nova com as alterações seguintes ao processo:

    * __Seleção de Ficheiros__: quando seleciona um ficheiro, selecione ao mesmo tempo os seis ficheiros .csv de dados de viagens restantes.

    ![Carregue os seis ficheiros restantes](media/tutorial-bikeshare-dataprep/browseazurestoragefortripdatafiles.png)

    > [!NOTE]
     > A entrada __+5__ indica que existem cinco ficheiros adicionais para além do que está listado.

    * __Detalhes do Ficheiro__: defina __Modo de Promoção de Cabeçalhos__ como **Todos os Ficheiros Têm os Mesmos Cabeçalhos.**. Este valor indica que cada um dos ficheiros contém o mesmo cabeçalho.

    ![Seleção de detalhes do ficheiro](media/tutorial-bikeshare-dataprep/headerfromeachfile.png) 

   Guarde o nome desta origem de dados, uma vez que vai ser utilizada em passos posteriores.

2. Selecione o ícone de pasta para ver os ficheiros no seu projeto. Expanda o diretório __aml\_config__ e selecione o ficheiro `hdinsight.runconfig`.

    ![Localização do hdinsight.runconfig](media/tutorial-bikeshare-dataprep/hdinsightsubstitutedatasources.png) 

3. Selecione o botão **Editar** para abrir o ficheiro no Visual Studio Code.

4. Adicione as linhas seguintes à parte final do ficheiro `hdinsight.runconfig` e selecione o ícone de disco para guardá-lo.

    ```yaml
    DataSourceSubstitutions:
      201701-hubway-tripdata.dsource: 201501-hubway-tripdata.dsource
    ```

    Esta alteração substitui a origem de dados original pela que contém os seis ficheiros de dados de viagens.

## <a name="save-training-data-as-a-csv-file"></a>Guardar os dados de aprendizagem como ficheiro CSV

1. Procure o ficheiro de Python `BikeShare Data Prep.py` que editou anteriormente. Indique um caminho de ficheiro diferente para guardar os dados de formação.

    ```python
    import pyspark

    from azureml.dataprep.package import run
    from pyspark.sql.functions import *

    # start Spark session
    spark = pyspark.sql.SparkSession.builder.appName('BikeShare').getOrCreate()

    # dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
    df = run('BikeShare Data Prep.dprep', dataflow_idx=2)
    df.show(n=10)
    row_count_first = df.count()

    # Example file name: 'wasb://data-files@bikesharestorage.blob.core.windows.net/traindata'
    # 'wasb://<your container name>@<your azure storage name>.blob.core.windows.net/<csv folder name>
    blobfolder = 'Your Azure Storage blob path'

    df.write.csv(blobfolder, mode='overwrite') 

    # retrieve csv file parts into one data frame
    csvfiles = "<Your Azure Storage blob path>/*.csv"
    df = spark.read.option("header", "false").csv(csvfiles)
    row_count_result = df.count()
    print(row_count_result)
    if (row_count_first == row_count_result):
        print('counts match')
    else:
        print('counts do not match')
    print('done')
    ```

2. Utilize a pasta com o nome `traindata` para a saída dos dados de formação.

3. Para submeter um trabalho novo, selecione **Executar**. Confirme que **hdinsight** está selecionado. É submetido um trabalho com a configuração nova. O resultado deste trabalho são os dados de formação. Estes dados são criados com os mesmos passos de preparação de dados que seguiu anteriormente. O trabalho pode demorar alguns minutos a ser concluído.


## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Passos seguintes
Concluiu o tutorial de preparação de dados de partilha de bicicletas. Neste tutorial, utilizou o Machine Learning (pré-visualização) para aprender a:
> [!div class="checklist"]
> * Preparar dados de forma interativa com a ferramenta de preparação de dados do Machine Learning.
> * Importar, transformar e criar um conjunto de dados de teste.
> * Gerar um pacote de preparação de dados.
> * Executar o pacote de preparação de dados com o Python.
> * Gerar um conjunto de dados de formação ao reutilizar o pacote de preparação de dados para ficheiros de entrada adicionais.

De seguida, aprenda mais sobre a preparação de dados:
> [!div class="nextstepaction"]
> [Data preparation user guide](data-prep-user-guide.md) (Guia de utilizador da preparação de dados)
