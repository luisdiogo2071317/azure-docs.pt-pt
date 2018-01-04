---
title: "Tutorial de partilha de bicicletas - Preparação de dados avançada com o Azure Machine Learning Workbench"
description: "Tutorial completo de preparação de dados com o Azure Machine Learning Workbench"
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial, azure
ms.topic: tutorial
ms.date: 09/21/2017
ms.openlocfilehash: 69f6911a95be382b06313d984f09c7e85aec10df
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2018
---
# <a name="bike-share-tutorial-advanced-data-preparation-with-azure-machine-learning-workbench"></a>Tutorial de partilha de bicicletas: preparação de dados avançada com o Azure Machine Learning Workbench
Os serviços do Azure Machine Learning (pré-visualização) são uma solução de análise avançada e ciência de dados ponto a ponto integrada para os cientistas de dados profissionais prepararem dados, desenvolverem experimentações e implementarem modelos à escala da cloud.

Neste tutorial, vai utilizar os serviços do Azure Machine Learning (pré-visualização) para aprender a:
> [!div class="checklist"]
> * Preparar os dados de forma interativa com a ferramenta Preparação de Dados do Azure Machine Learning
> * Importar, transformar e criar um conjunto de dados de teste
> * Gerar um pacote de Preparação de Dados
> * Executar o pacote de Preparação de Dados com o Python
> * Gerar um conjunto de dados de formação ao reutilizar o pacote de Preparação de Dados para ficheiros de entrada adicionais

> [!IMPORTANT]
> Este tutorial só prepara os dados, não cria o modelo de predição.
>
> Pode utilizar os dados preparados para formar os seus próprios modelos de predição. Por exemplo, pode criar um modelo para prever a procura por bicicletas durante um período de duas horas.

## <a name="prerequisites"></a>Pré-requisitos
* O Azure Machine Learning Workbench tem de ser instalado localmente. Para obter mais informações, siga o [Início Rápido da instalação](quickstart-installation.md).
* Conhecimentos sobre a criação de projetos novos no Workbench.

## <a name="data-acquisition"></a>Aquisição de dados
Este tutorial utiliza o [conjunto de dados da Boston Hubway](https://s3.amazonaws.com/hubway-data/index.html) e os dados meteorológicos de Boston da [NOAA](http://www.noaa.gov/) (National Oceanic and Atmospheric Administration dos E.U.A.)

1. Transfira os ficheiros de dados a partir das ligações abaixo para o seu ambiente de desenvolvimento local. 
   * [Boston weather data](https://azuremluxcdnprod001.blob.core.windows.net/docs/azureml/bikeshare/BostonWeather.csv) (Dados meteorológicos de Boston) 
   * Dados de viagens da Hubway a partir do site da Hubway.

      - [201501-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201501-hubway-tripdata.zip)
      - [201504-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201504-hubway-tripdata.zip)
      - [201510-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201510-hubway-tripdata.zip)
      - [201601-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201601-hubway-tripdata.zip)
      - [201604-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201604-hubway-tripdata.zip)
      - [201610-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201610-hubway-tripdata.zip)
      - [201701-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201701-hubway-tripdata.zip)

2. Deszipe cada ficheiro após a transferência.

## <a name="learn-about-the-datasets"></a>Saiba mais sobre os conjuntos de dados
1. O ficheiro __Boston weather__ contém os campos relacionados com meteorologia abaixo, comunicados hora a hora:
   * DATA
   * REPORTTPYE
   * HOURLYDRYBULBTEMPF
   * HOURLYRelativeHumidity
   * HOURLYWindSpeed

2. Os dados da __Hubway__ estão organizados em ficheiros por ano e mês. Por exemplo, o ficheiro denominado `201501-hubway-tripdata.zip` contém um ficheiro .csv que inclui os dados de janeiro de 2015. Os dados contêm os seguintes campos, sendo que cada linha representa uma viagem de bicicleta:

   * Duração da Viagem (em segundos)
   * Data e Hora de Início
   * Data e Hora de Fim
   * Nome e ID da Estação de Partida
   * Nome e ID da Estação de Chegada
   * ID da Bicicleta
   * Tipo de Utilizador (Casual = utilizador com passe de 24 ou 72 horas; sócio = sócio anual ou mensal)
   * Código Postal (se o utilizador for sócio)
   * Sexo (comunicado automaticamente pelo sócio)

## <a name="create-a-new-project"></a>Criar um novo projeto
1. Inicie o **Azure Machine Learning Workbench** no menu Iniciar ou no iniciador.

2. Crie um projeto do Azure Machine Learning novo.  Clique no botão **+**, na página **Projetos**, ou em **Ficheiro** > **Novo**.
   - Utilize o modelo **Projeto em Branco**.
   - Dê ao projeto o nome **BikeShare**. 

## <a id="newdatasource"></a>Criar uma origem de dados nova

1. Crie uma origem de dados nova. Clique no botão **Dados** (ícone de cilindro), na barra de ferramentas do lado esquerdo. É apresentada a **Vista de Dados**.

   ![Imagem do separador da vista de dados](media/tutorial-bikeshare-dataprep/navigatetodatatab.png)

2. Adicione uma origem de dados. Selecione o ícone **+** e, em seguida, selecione **Adicionar Origem de Dados**.

   ![Imagem da entrada Adicionar Origem de Dados](media/tutorial-bikeshare-dataprep/newdatasource.png)

## <a name="add-weather-data"></a>Adicionar dados meteorológicos

1. **Arquivo de Dados**: selecione o arquivo de dados que contém os dados. Uma vez que está a utilizar ficheiros, selecione **Ficheiro(s)/Diretório**. Selecione **Seguinte** para continuar.

   ![Imagem da entrada Ficheiro(s)/Diretório](media/tutorial-bikeshare-dataprep/datasources.png)

2. **Seleção de Ficheiros**: adicione os dados meteorológicos. Procure e selecione o ficheiro `BostonWeather.csv` que transferiu anteriormente. Clique em **Seguinte**.

   ![Imagem da seleção de ficheiro com BostonWeater.csv selecionado](media/tutorial-bikeshare-dataprep/pickweatherdatafile.png)

3. **Detalhes do Ficheiro**: verifique que esquema de ficheiro é detetado. O Azure Machine Learning Workbench analisa os dados no ficheiro e infere qual o esquema que vai ser utilizado.

   ![Imagem dos Detalhes do Ficheiro](media/tutorial-bikeshare-dataprep/fileparameters.png)

   > [!IMPORTANT]
   > O Workbench pode não detetar o esquema correto em alguns casos. Deve sempre confirmar que os parâmetros estão certos para o seu conjunto de dados. Relativamente aos dados meteorológicos, verifique se estão definidos como os valores abaixo:
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

4. **Tipos de Dados**: reveja os tipos de dados que são detetados automaticamente. O Azure Machine Learning Workbench analisa os dados no ficheiro e infere os tipos de dados que vão ser utilizados.

   Relativamente a estes dados, altere `DATA TYPE` de todas as colunas para `String`.

   > [!NOTE]
   > `String` é utilizado para destacar as capacidades do Workbench mais à frente no tutorial. 

   ![Imagem dos tipos de dados](media/tutorial-bikeshare-dataprep/datatypedetection.png)

   Para continuar, selecione __Seguinte__. 

5. **Amostragem**: para criar um esquema de amostragem, selecione o botão **+ Nova**. Selecione a linha __Primeiros 10 000__ que é adicionado e selecione __Editar__. Defina __Estratégia de Amostragem__ como **Ficheiro Completo** e selecione **Aplicar**.

   ![Imagem da adição de uma nova estratégia de amostragem](media/tutorial-bikeshare-dataprep/weatherdatasampling.png)

   Para utilizar a estratégia __Ficheiro Completo__, selecione a entrada __Ficheiro Completo__ e selecione __Definir como Ativo__. É apresentada uma estrela junto a __Ficheiro Completo__, para indicar que esta é a estratégia ativa.

   ![Imagem de Ficheiro Completo como a estratégia ativa.](media/tutorial-bikeshare-dataprep/fullfileactive.png)

   Para continuar, selecione **Seguinte**.

6. **Coluna de Caminho**: a secção __Coluna de Caminho__ permite-lhe incluir o caminho do ficheiro completo como coluna nos dados importados. Selecione __Não Incluir Coluna de Caminho__.

   > [!TIP]
   > A inclusão do caminho como coluna é útil se estiver a importar uma pasta que tenha muitos ficheiros com diferentes nomes. Também é útil se os nomes dos ficheiros contiverem informações que pretenda extrair mais tarde.

   ![Imagem da Coluna de Caminho definida para não ser incluída](media/tutorial-bikeshare-dataprep/pathcolumn.png)

7. **Concluir**: para concluir a criação da origem de dados, selecione o botão **Concluir**.

    É aberto um separador de origem de dados novo com o nome __BostonWeather__. É apresentado um exemplo dos dados numa vista de grelha. O exemplo baseia-se no esquema de amostragem ativo especificado anteriormente.

    Repare que o painel **Passos**, no lado direito do ecrã, apresenta as ações individuais realizadas durante a criação desta origem de dados.

   ![Imagem que apresenta a origem de dados, o exemplo e os passos](media/tutorial-bikeshare-dataprep/weatherdataloaded.png)

### <a name="view-data-source-metrics"></a>Ver as métricas da origem de dados

Selecione o botão __Métricas__, no canto superior esquerdo da vista de grelha do separador. Esta vista mostra a distribuição e outras estatísticas agregadas dos dados de amostragem.

![Imagem das Métricas apresentadas](media/tutorial-bikeshare-dataprep/weathermetrics.png)

> [!NOTE]
> Para configurar a visibilidade das estatísticas, utilize o menu pendente **Escolher Métrica**. Para alterar a vista de grelha, selecione ou anule a seleção de métricas aí.

Para regressar à __Vista de Dados__, selecione __Dados__, no canto superior esquerdo da página.

## <a name="add-data-source-to-data-preparation-package"></a>Adicionar origem de dados para o pacote de preparação de dados

1. Selecione __Preparar__ para iniciar a preparação dos dados. 

2. Quando lhe for pedido, introduza um nome para o pacote de Preparação de Dados, como, por exemplo, `BikeShare Data Prep`. 

3. Selecione __OK__ para continuar.

   ![Imagem da caixa de diálogo Preparar](media/tutorial-bikeshare-dataprep/dataprepdialog.png)

4. Aparece um pacote novo com o nome **BikeShare Data Prep**, na secção __Preparação de Dados__ do separador __Dados__. 

   Para apresentar o pacote, selecione esta entrada. 

5. Selecione o botão **>>** para expandir e mostrar os __Fluxos de Dados__ presentes no pacote. Neste exemplo, o único fluxo de dados é __BostonWeather__.

   > [!IMPORTANT]
   > Os pacotes podem conter vários Fluxos de Dados.

   ![Imagem dos fluxos de dados presentes no pacote](media/tutorial-bikeshare-dataprep/weatherdataloadedingrid.png)

## <a name="filter-data-by-value"></a>Filtrar dados por valor
1. Para filtrar os dados, faça duplo clique numa célula com um determinado valor, selecione __Filtro__ e, em seguida, o tipo de filtro.

2. Neste tutorial, selecione uma célula que contenha o valor `FM-15` e defina o filtro como um filtro de **É Igual A**.  Agora, os dados estão filtrados para devolverem apenas linhas em que __REPORTTYPE__ é `FM-15`.

   ![Imagem da caixa de diálogo Filtrar](media/tutorial-bikeshare-dataprep/weatherfilterinfm15.png)

   > [!NOTE]
   > FM-15 é um tipo de Boletim METAR (Meteorological Terminal Aviation Routine Weather Report). Os boletins FM-15 são considerados empiricamente como sendo os mais completos, com poucos dados em falta.

## <a name="remove-a-column"></a>Remover uma coluna

Já não precisa da coluna __REPORTTYPE__. Clique com o botão direito do rato no cabeçalho da coluna e selecione **Remover Coluna**.

   ![Imagem da opção de remoção de coluna](media/tutorial-bikeshare-dataprep/weatherremovereporttype.png)

## <a name="change-datatypes-and-remove-errors"></a>Alterar os tipos de dados e remover erros
1. Premir __Ctrl (Comando ⌘ em Mac)__ enquanto seleciona os cabeçalhos das colunas permite-lhe selecionar várias colunas de uma vez. Utilize esta ação para selecionar os cabeçalhos seguintes:
   * **HOURLYDRYBULBTEMPF**
   * **HOURLYRelativeHumidity**
   * **HOURLYWindSpeed**

2. **Clique com botão direito do rato** num dos cabeçalhos de colunas selecionado e selecione **Converter Tipo de Campo em Numérico**. Desta forma, o tipo de dados das colunas é convertido em numérico.

   ![Converter várias colunas num valor numérico](media/tutorial-bikeshare-dataprep/weatherconverttonumeric.png)

3. Filtre os valores de erro. Algumas colunas têm problemas de conversão de tipo de dados. Este problema é indicado pela cor vermelha na __Barra de Qualidade dos Dados__ das colunas em causa.

   Para remover as linhas com erros, clique com o botão direito do rato no cabeçalho de coluna **HOURLYDRYBULBTEMPF**. Selecione **Filtrar Coluna**. Utilize a opção predefinida **Pretendo** como **Manter Linhas**. Altere o menu pendente **Condições** para selecionar **não é erro**. Selecione **OK** para aplicar o filtro.

4. Para eliminar as restantes linhas de erros nas outras colunas, repita este processo de filtragem nas colunas **HOURLYRelativeHumidity** e **HOURLYWindSpeed**.

## <a name="use-by-example-transformations"></a>Utilizar transformações por exemplo

Para utilizar os dados de uma predição de blocos de tempo de duas horas, tem de calcular as condições meteorológicas médias para períodos de duas horas. Para tal, pode utilizar as ações seguintes:

* Divida a coluna **DATE** em duas colunas separadas, **Date** e **Time**. Veja os passo detalhados na secção seguinte.

* Derive uma coluna **Hour_Range** a partir da coluna **Time**. Veja os passo detalhados na secção seguinte.

* Derive uma coluna **Date\_Hour\_Range** a partir das colunas **DATE** e **Hour_Range** . Veja os passo detalhados na secção seguinte.

### <a name="split-column-by-example"></a>Dividir coluna por exemplo

1. Divida a coluna **DATE** em duas colunas separadas, Date e Time. Clique com o botão direito do rato no cabeçalho de coluna **DATE** e selecione **Dividir Coluna por Exemplo**.

   ![Imagem da entrada da coluna dividida por exemplo](media/tutorial-bikeshare-dataprep/weathersplitcolumnbyexample.png)

2. O Azure Machine Learning Workbench identifica automaticamente um delimitador relevante e cria duas colunas mediante a divisão dos dados em valores de data e hora. 

3. Selecione __OK__ para aceitar os resultados da operação de divisão.

   ![Imagem das colunas divididas DATE_1 e DATE_2](media/tutorial-bikeshare-dataprep/weatherdatesplitted.png)

### <a name="derive-column-by-example"></a>Derivar coluna por exemplo

1. Para derivar um intervalo de duas horas, clique com o botão direito do rato no cabeçalho de coluna __DATE\_2__ e selecione **Derivar Coluna por Exemplo**.

   ![Imagem da entrada de coluna derivada por exemplo](media/tutorial-bikeshare-dataprep/weatherdate2range.png)

   É adicionada uma coluna vazia nova com valores nulos.

2. Clique na primeira célula vazia da coluna nova. Escreva `12AM-2AM` na coluna nova e prima Enter para indicar um exemplo do intervalo de tempo pretendido.

   ![Imagem da coluna nova com o valor 12AM-2AM](media/tutorial-bikeshare-dataprep/weathertimerangeexample.png)

   > [!NOTE]
   > O Azure ML Workbench sintetiza um programa com base nos exemplos que os utilizadores fornecem e aplica o mesmo programa às linhas restantes. Todas as outras linhas são preenchidas automaticamente com base no exemplo que tiver fornecido. Além disso, o Workbench também analisa os dados e tenta identificar casos extremos. 
  
3. O texto **A Analisar Dados**, acima da grelha, indica que o Workbench está a tentar detetar casos extremos. Quando terminar, o estado muda para **Rever próxima linha sugerida** ou **Sem sugestões**. Neste exemplo, é devolvido **Rever próxima linha sugerida**.

4. Para rever as alterações sugeridas, selecione **Rever próxima linha sugerida**. A célula que deve rever e corrigir (se necessário) é realçada no ecrã.

   ![Imagem da linha a ser revista](media/tutorial-bikeshare-dataprep/weatherreviewnextsuggested.png)

    Selecione __OK__ para aceitar a transformação.
 
5. É reencaminhado para a vista de grelha dos dados de __BostonWeather__. A grelha contém agora as três colunas adicionadas anteriormente.

   ![Imagem da vista de grelha com linhas adicionadas](media/tutorial-bikeshare-dataprep/timerangecomputed.png)

   > [!TIP]
   >  Todas as alterações que tiver feito são mantidas no painel **Passos**. Vá para o passo que criou no painel **Passos**, clique na seta para baixo e selecione **Editar**. É apresentada a janela avançada para **Derivar coluna por Exemplo**. Todos os seus exemplos são preservados aqui. Também pode fazer duplo clique numa linha na grelha abaixo e adicionar exemplos manualmente. Selecione **Cancelar** para regressar à grelha principal sem aplicar as alterações. Também pode aceder a esta vista ao selecionar **Modo Avançado** enquanto faz uma transformação **Derivar Coluna por Exemplo**.

6. Para mudar o nome da coluna, faça duplo clique no cabeçalho da coluna e escreva **Hour Range**. Prima **Enter** para guardar a alteração.

   ![Mudar o nome das colunas](media/tutorial-bikeshare-dataprep/weatherhourrangecolumnrename.png)

7. Para derivar o intervalo de datas e horas, selecione ao mesmo tempo as colunas **Date\_1** e **Hour Range**, clique com o botão direito do rato e selecione **Derivar Coluna por Exemplo**.

   ![Dividir coluna por exemplo](media/tutorial-bikeshare-dataprep/weatherderivedatehourrange.png)

   Escreva `Jan 01, 2015 12AM-2AM` como o exemplo na primeira linha e prima **Enter**.

   O Workbench determina a transformação com base no exemplo que fornecer. Neste exemplo, o resultado é que o formato de data é alterado e concatenado com o período de duas horas.

   ![Imagem do exemplo “Jan 01, 2015 12AM-2AM”](media/tutorial-bikeshare-dataprep/wetherdatehourrangeexample.png)


8. Aguarde até que o estado passe de **A Analisar Dados** para **Rever próxima linha sugerida**. Esta alteração pode demorar vários segundos. Selecione a ligação de estado para ir para a linha sugerida. 

   ![Imagem da linha sugerida a ser revista](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguate.png)

   A linha tem um valor nulo porque o valor de data de origem podia ser tanto dd/mm/aaaa, como mm/dd/aaaa. Escreva o valor correto, `Jan 13, 2015 2AM-4AM`, e prima **Enter**. O Workbench utiliza os dois exemplos para melhorar a derivação para as linhas restantes.

   ![Imagem dos dados formatados corretamente](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguated.png)

9. Selecione **OK** para aceitar a transformação.

   ![Imagem da grelha de transformação concluída](media/tutorial-bikeshare-dataprep/weatherdatehourrangecomputed.png)

   > [!TIP]
   > Pode clicar na seta para baixo, no painel **Passos**, para utilizar o modo avançado de **Derivar Coluna por Exemplo** neste passo. Na grelha de dados, existem caixas de verificação junto aos nomes das colunas **DATE\_1** e **Hour Range**. Desmarque a caixa de verificação junto à coluna **Hour Range** para ver de que forma é que o resultado é alterado. Se a coluna **Hour Range** não estiver presente como entrada, **12AM-2AM** é tratado como uma constante e acrescentado aos valores derivados. Selecione **Cancelar** para regressar à grelha principal sem aplicar as alterações.

10. Para mudar o nome da coluna, faça duplo clique no cabeçalho. Mude o nome para **Date Hour Range** e prima **Enter**.

11. Selecione ao mesmo tempo as colunas **DATE**, **DATE\_1**, **DATE\_2** e **Hour Range**. Clique com o botão direito do rato e selecione **Remover coluna**.

## <a name="summarize-data-mean"></a>Resumir os dados (média)

O passo seguinte é calcular a média dos valores, agrupados por intervalo de horas, para resumir as condições atmosféricas.

1. Selecione a coluna **Date Hour Range** e selecione **Resumir**, a partir do menu **Transformações**.

    ![Menu Transformações](media/tutorial-bikeshare-dataprep/weathersummarizemenu.png)

2. Para resumir os dados, arraste as colunas da grelha, na parte inferior da página, para os painéis do lado esquerdo e direito da parte superior. O painel do lado esquerdo contém o texto **Arrastar colunas para aqui para agrupar os dados**. O do lado direito contém o texto **Arrastar colunas para aqui para resumir os dados**. 

    Arraste a coluna **Date Hour Range** da grelha na parte inferior para o painel do lado esquerdo. Arraste **HOURLYDRYBULBTEMPF**, **HOURLYRelativeHumidity** e **HOURLYWindSpeed** para o painel do lado direito. 

    Neste painel, selecione **Média** como a medida **Agregada** para cada coluna. Clique em **OK** para concluir o resumo.

   ![Imagem do ecrã de dados resumidos](media/tutorial-bikeshare-dataprep/weathersummarize.png)

## <a name="transform-dataflow-using-script"></a>Transformar fluxos de dados com scripts

Alguns modelos podem convergir rapidamente se os dados nas colunas numéricas forem alterados para um intervalo de 0-1. Atualmente não existe nenhuma transformação incorporada para realizar esta transformação genericamente, mas pode ser utilizado um script de Python para a realizar.

1. No menu **Transformação**, selecione **Transformar Fluxo de Dados**.

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
    
    ![Caixa de diálogo de script de transformação de fluxo de dados](media/tutorial-bikeshare-dataprep/transformdataflowscript.png)

3. Selecione __OK__ para utilizar o script. As colunas numéricas na grelha contêm agora valores no intervalo 0-1.

    ![Grelha que contém os valores entre 0 e 1](media/tutorial-bikeshare-dataprep/datagridwithdecimals.png)

Concluiu a preparação dos dados meteorológicos. Em seguida, prepare os dados da viagem.

## <a name="load-trip-data"></a>Carregar dados da viagem

1. Para importar o ficheiro `201701-hubway-tripdata.csv`, siga os passos na secção [Criar nova Origem de Dados](#newdatasource). Utilize as opções abaixo durante o processo de importação:

    * __Esquema de amostragem__: esquema de amostragem **Ficheiro Completo**, torne a amostragem ativa e 
    * __Tipo de Dados__: aceite as predefinições.

2. Depois de importar os dados, selecione o botão __Preparar__ para começar a preparar os dados. Selecione o pacote **BikeShare Data Prep.dprep** já existente e selecione __OK__.

    Este processo adiciona um **Fluxo de Dados** ao ficheiro **Data Preparation** já existente em vez de criar um novo.

    ![Imagem da seleção do pacote já existente](media/tutorial-bikeshare-dataprep/addjandatatodprep.png)

3. Após a grelha ter sido carregada, expanda __DATAFLOWS__. Existem agora dois fluxos de dados: **BostonWeather** e **201701-hubway-tripdata**. Selecione a entrada **201701-hubway-tripdata**.

    ![Imagem da entrada 201701-hubway-tripdata](media/tutorial-bikeshare-dataprep/twodfsindprep.png)

## <a name="use-map-inspector"></a>Utilizar o inspetor de mapas

Para a preparação de dados, existem várias visualizações úteis, denominadas **Inspetores** para dados de Cadeia, Numéricos e Geográficos, os quais ajudam a compreender melhor os dados e a identificar valores atípicos. Para utilizar o Inspetor de mapas, siga os passos abaixo:

1. Selecione ao mesmo tempo as colunas **start station latitude** e **start station longitude**. Clique com o botão direito do rato numa das colunas e selecione **Mapa**.

    > [!TIP]
    > Para permitir a seleção ao mesmo tempo, manteha premido __Ctrl (comando ⌘ em Mac)__ e selecione o cabeçalho de cada coluna.

    ![Imagem da visualização dos mapas](media/tutorial-bikeshare-dataprep/launchMapInspector.png)

2. Para maximizar a visualização dos mapas, selecione o ícone **Maximizar**. Para que o mapa se ajuste à janela, selecione o ícone **E**, no canto superior esquerdo da visualização.

    ![Imagem maximizada](media/tutorial-bikeshare-dataprep/maximizedmap.png)

3. Clique no botão **Minimizar** para regressar à vista de grelha.

## <a name="use-column-statistics-inspector"></a>Utilizar o Inspetor Estatísticas de Coluna

Para utilizar o inspetor de estatísticas de coluna, clique com o botão direito do rato na coluna __tripduration__ e selecione __Estatísticas de Coluna__.

![Entrada de estatísticas de coluna](media/tutorial-bikeshare-dataprep/tripdurationcolstats.png)

Este processo adiciona uma visualização nova com o nome __Estatísticas de tripduration__, no painel __INSPETORES__.

![Imagem de inspetor de estatísticas de tripduration](media/tutorial-bikeshare-dataprep/tripdurationcolstatsinwell.png)

> [!IMPORTANT]
> O valor máximo da duração da viagem são **961 814 minutos**, o que equivale a, aproximadamente, dois anos. Parece que existem alguns valores atípicos no conjunto de dados.

## <a name="use-histogram-inspector"></a>Utilizar o inspetor de histogramas

Para tentar identificar os valores atípicos, clique com botão direito do rato na coluna __tripduration__ e selecione __Histograma__.

![Inspetor de Histogramas](media/tutorial-bikeshare-dataprep/tripdurationhistogram.png)

O histograma não é útil, uma vez que os valores atípicos estão a distorcer o gráfico.

## <a name="add-column-using-script"></a>Utilizar script para adicionar colunas

1. Clique com o botão direito do rato na coluna **tripduration** e selecione **Adicionar Coluna (Script)**.

    ![Imagem do menu Adicionar Coluna (script)](media/tutorial-bikeshare-dataprep/computecolscript.png)

2. Na caixa de diálogo __Adicionar Coluna (Script)__, utilize os valores seguintes:

    * __Nome da Coluna Nova__: logtripduration
    * __Inserir esta Coluna Nova a Seguir a__: tripduration
    * __Código da Coluna Nova__: `math.log(row.tripduration)`
    * __Tipo de Bloco de Código__: expressão

   ![A caixa de diálogo Adicionar Coluna (Script)](media/tutorial-bikeshare-dataprep/computecolscriptdialog.png)

3. Selecione __OK__ para adicionar a coluna **logtripduration**.

4. Clique com o botão direito do rato na coluna e selecione **Histograma**.

    ![Histograma da coluna logtripduration](media/tutorial-bikeshare-dataprep/logtriphistogram.png)

  Visualmente, parece que este histograma, é uma Distribuição Normal com uma cauda anormal.

## <a name="use-advanced-filter"></a>Utilizar o Filtro Avançado

A utilização de um filtro nos dados atualiza os inspetores com a distribuição nova. Clique com o botão direito do rato na coluna **logtripduration** e selecione **Filtrar Coluna**. Na caixa de diálogo __Editar__, utilize os valores seguintes:

* __Filtrar esta Coluna Numérica__: logtripduration
* __Pretendo__: Manter as Linhas
* __Quando__: qualquer uma das Condições abaixo for Verdadeira (OU lógico)
* __Se esta coluna__: inferior a
* __O valor__: 9

![Opções de filtragem](media/tutorial-bikeshare-dataprep/loftripfilter.png)

Selecione __OK__ para aplicar o filtro.

![Histogramas atualizados depois de o filtro ser aplicado](media/tutorial-bikeshare-dataprep/loftripfilteredinspector.png)

### <a name="the-halo-effect"></a>O efeito de halo

1. Maximize o histograma **logtripduration**. Existe um histograma azul sobreposto num cinzento. Este ecrã é denominado o **Efeito Halo**:

    * O **histograma cinzento** representa a distribuição antes da operação (neste caso, a operação de filtragem).
    * O **histograma azul** representa o histograma após a operação. 

   O efeito de halo ajuda a visualizar o efeito de uma operação nos dados.

   ![Imagem do efeito de halo](media/tutorial-bikeshare-dataprep/loftripfilteredinspectormaximized.png)

    > [!NOTE]
    > Repare o que o histograma azul aparece mais curto comparado com anterior. Isto deve-se à recriação automática de registos dos dados no intervalo novo.

2. Para remover o halo, selecione __Editar__ e desbloqueie __Mostrar halo__.

    ![Opções para o histograma](media/tutorial-bikeshare-dataprep/uncheckhalo.png)

3. Selecione **OK** para desativar o efeito de halo e minimizar a histograma.

### <a name="remove-columns"></a>Remover colunas

Nos dados de viagens, cada linha representa um evento de recolha de bicicletas. Neste tutorial, só precisa das colunas **starttime** e **start station id**. Remova as outras colunas ao selecionar ao mesmo tempo estas duas colunas, clicar com o botão direito do rato no cabeçalho da coluna e selecionar **Manter Coluna**. As outras colunas são removidas.

![Imagem da opção para manter a coluna](media/tutorial-bikeshare-dataprep/tripdatakeepcolumn.png)

### <a name="summarize-data-count"></a>Resumir os dados (Contagem)

Para resumir a procura por bicicletas durante um período de duas horas, utilize colunas derivadas.

1. Clique com o botão direito do rato na coluna **starttime** e selecione **Derivar Coluna por Exemplo**.

    ![Imagem da opção para derivar por exemplo](media/tutorial-bikeshare-dataprep/tripdataderivebyexample.png)

2. Para o exemplo, introduza o valor `Jan 01, 2017 12AM-2AM` na primeira linha.

    > [!IMPORTANT]
    > No exemplo anterior de colunas derivadas, utilizou vários passos para derivar uma coluna que continha o período de data e hora. Neste exemplo, é possível ver que esta operação pode ser executada como um único passo ao indicar um exemplo do resultado final.

    > [!NOTE]
    > Pode dar um exemplo em relação a qualquer uma das linhas. Neste exemplo, o valor `Jan 01, 2017 12AM-2AM` é válido para a primeira linha de dados.

    ![Imagem dos dados de exemplo](media/tutorial-bikeshare-dataprep/tripdataderivebyexamplefirstexample.png)
   
3. Aguarde até que a aplicação calcule os valores de todas as linhas. Esta operação pode demorar vários segundos. Depois de concluída a análise, utilize a ligação __Rever próxima linha sugerida__ para rever os dados.

   ![Imagem da análise concluída com a ligação de revisão](media/tutorial-bikeshare-dataprep/tripdatabyexanalysiscomplete.png)

    Confirme que os valores calculados estão corretos. Se não estiverem, atualize-os com os valores esperados e prima Enter. Depois, aguarde até que a análise seja concluída. Conclua o processo **Rever próxima linha sugerida** até ver **Nenhuma sugestão**. Quando vir **Nenhuma sugestão**, a aplicação analisou os casos extremos e está satisfeita com o programa sintetizado. Antes de aceitar a transformação, é melhor prática fazer uma inspeção visual dos dados transformados. 

4. Selecione **OK** para aceitar a transformação. Mude o nome da coluna criada recentemente para **Date Hour Range**.

    ![Imagem da coluna com o nome alterado](media/tutorial-bikeshare-dataprep/tripdatasummarize.png)

5. Clique com o botão direito do rato no cabeçalho da coluna **starttime** e selecione **Remover coluna**.

6. Para resumir os dados, selecione __Resumir__, no menu __Transformar__. Para criar a transformação, execute as ações abaixo:

    * Arraste __Date Hour Range__ e __start station id__ para o painel do lado esquerdo (agrupar por).
    * Arraste __start station id__ para o painel do lado direito (resumir dados).

   ![Imagem das opções de resumo](media/tutorial-bikeshare-dataprep/tripdatacount.png)

7. Selecione **OK** para aceitar o resultado do resumo.

## <a name="join-dataflows"></a>Associar fluxos de dados

Para associar os dados meteorológicos aos dados de viagens, utilize os seguintes passos:

1. Selecione __Associar__, no menu __Transformações__.

2. __Tabelas__: selecione **BostonWeather** como o fluxo de dados do lado esquerdo e **201701-hubway-tripdata** como o do lado direito. Para continuar, selecione **Seguinte**.

    ![Imagem das seleções de tabelas](media/tutorial-bikeshare-dataprep/jointableselection.png)

3. __Colunas de Chaves__: selecione a coluna **Date Hour Range** em ambas as tabelas e selecione __Seguinte__.

    ![Imagem da associação nas colunas de chaves](media/tutorial-bikeshare-dataprep/joinkeyselection.png)

4. __Tipo de Associação__: selecione __Linhas correspondentes__ como o tipo de associação e selecione __Concluir__.

    ![Tipo de associação de linhas correspondentes](media/tutorial-bikeshare-dataprep/joinscreen.png)

    Este processo cria fluxo de dados novo com o nome __Associar Resultado__.

## <a name="create-additional-features"></a>Criar características adicionais

1. Para criar uma coluna que contenha o dia da semana, clique com o botão direito do rato na coluna **Date Hour Range** e selecione **Derivar Coluna por Exemplo**. Utilize um valor como __Sun__ (Dom.) para uma data que tenha ocorrido num domingo. Por exemplo, **1 de Jan. de 2017, 24:00 - 02:00**. Prima **Enter** e selecione **OK**. Mude o nome desta coluna para __Weekday__.

    ![Imagem da criação de uma nova coluna que contém o dia da semana](media/tutorial-bikeshare-dataprep/featureweekday.png)

2. Para criar uma coluna que contenha o período de tempo de uma linha, clique com o botão direito do rato na coluna **Date Hour Range** e selecione **Derivar Coluna por Exemplo**. Utilize um valor como **24:00 - 02:00** na linha que contém **01 de Jan. de 2017, 24:00 - 02:00)**. Prima **Enter** e selecione **OK**. Mude o nome desta coluna para **Period**.

    ![Imagem da coluna de período](media/tutorial-bikeshare-dataprep/featurehourrange.png)

3. Para remover as colunas **Date Hour Range** e **rDate Hour Range**, prima **Ctrl (comando ⌘ em Mac)** e selecione cada cabeçalho de coluna. Clique com o botão direito do rato e selecione **Remover Coluna**.

## <a name="read-data-from-python"></a>Ler dados a partir do Python

Pode executar um pacote de preparação de dados a partir de Python ou PySpark e obtenha os resultados como um **Data Frame**.

Para gerar um script de Python de exemplo, clique com o botão direito do rato em __BikeShare Data Prep__ e selecione __Gerar Ficheiro de Código de Acesso a Dados__. O ficheiro de Python do exemplo é criado na sua **Pasta do Projeto** e também é carregado num separador do Workbench. O script de Python seguinte é um exemplo do código que é gerado:

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

Par guardar o Fluxo de Dados **Associar Resultado** num ficheiro .CSV, tem de alterar o script `BikeShare Data Prep.py`. Utilize o código abaixo para atualizar o script de Python:

```python
from azureml.dataprep.package import run

# dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
df = run('BikeShare Data Prep.dprep', dataflow_idx=2)

# Example file path: C:\\Users\\Jayaram\\BikeDataOut\\BikeShareTest.csv
df.to_csv('Your Test Data File Path here')
```

Selecione **Executar** a partir da parte superior do ecrã. O script é submetido como um **Trabalho** no computador local. Quando o estado do trabalho passar para __Concluído__, significa que o ficheiro foi escrito para a localização especificada.

## <a name="substitute-data-sources"></a>Substitui as origens de dados

Nos passos anteriores, utilizou as origens de dados `201701-hubway-tripdata.csv` e `BostonWeather.csv` para preparar os dados de Teste. Para utilizar o pacote com os outros ficheiros de dados de viagens, utilize os passos seguintes:

1. Crie uma **Origem de Dados** nova com os passos anteriores, com as alterações seguintes ao processo:

    * __Seleção de Ficheiros__: ao selecionar um ficheiro, selecione ao mesmo tempo os seis ficheiros .CSV de dados de viagens restantes.

        ![Carregar os seis ficheiros restantes](media/tutorial-bikeshare-dataprep/selectsixfiles.png)

        > [!NOTE]
        > A entrada __+5__ indica que existem cinco ficheiros adicionais para além do que está listado.

    * __Detalhes do Ficheiro__: defina __Modo de Promoção de Cabeçalhos__ como **Todos os Ficheiros Têm os Mesmos Cabeçalhos.** Este valor indica que cada um dos ficheiros contém o mesmo cabeçalho.

        ![Seleção de detalhes do ficheiro](media/tutorial-bikeshare-dataprep/headerfromeachfile.png) 

   Guarde o nome desta origem de dados, uma vez que vai ser utilizada em passos posteriores.

2. Selecione o ícone de pasta para ver os ficheiros no seu projeto. Expanda o diretório __aml\_config__ e selecione o ficheiro `local.runconfig`.

    ![Imagem da localização de local.runconfig](media/tutorial-bikeshare-dataprep/localrunconfig.png) 

3. Adicione as linhas seguintes à parte final do ficheiro `local.runconfig` e selecione o ícone de disco para guardá-lo.

    ```yaml
    DataSourceSubstitutions:
      201701-hubway-tripdata.dsource: 201501-hubway-tripdata.dsource
    ```

    Esta alteração substitui a origem de dados original pela que contém os seis ficheiros de dados de viagens.

## <a name="save-training-data-as-a-csv-file"></a>Guardar os dados de aprendizagem como ficheiro CSV

Navegue para o ficheiro Python `BikeShare Data Prep.py` que editou anteriormente e indique um Caminho de Ficheiro diferente para guardar os Dados de Aprendizagem.

```python
from azureml.dataprep.package import run
# dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
df = run('BikeShare Data Prep.dprep', dataflow_idx=2)

# Example file path: C:\\Users\\Jayaram\\BikeDataOut\\BikeShareTrain.csv
df.to_csv('Your Training Data File Path here')
```

Para submeter um trabalho novo, utilize o ícone **Executar**, na parte superior da página. É submetido um **Trabalho** com a configuração nova. O resultado deste trabalho são os Dados de Aprendizagem. Estes dados são criados com os mesmos passos de Preparação de Dados que criou anteriormente. O trabalho pode demorar alguns minutos até ser concluído.

## <a name="next-steps"></a>Passos Seguintes
Concluiu o tutorial de Preparação de Dados de Partilha de Bicicletas. Neste tutorial, utilizou os serviços do Azure Machine Learning (pré-visualização) para aprender a:
> [!div class="checklist"]
> * Preparar os dados de forma interativa com a ferramenta Preparação de Dados do Azure Machine Learning
> * Importar, transformar e criar um conjunto de dados de teste
> * Gerar um pacote de Preparação de Dados
> * Executar o pacote de Preparação de Dados com o Python
> * Gerar um conjunto de dados de formação ao reutilizar o pacote de Preparação de Dados para ficheiros de entrada adicionais

De seguida, aprenda mais sobre a preparação de dados:
> [!div class="nextstepaction"]
> [Data preparation user guide](data-prep-user-guide.md) (Guia de utilizador da preparação de dados)
