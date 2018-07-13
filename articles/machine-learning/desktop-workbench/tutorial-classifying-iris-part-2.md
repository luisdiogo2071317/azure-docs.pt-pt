---
title: Tutorial para criar um modelo para os serviços do Azure Machine Learning (pré-visualização) | Microsoft Docs
description: Este tutorial completo mostra como utilizar os serviços do Azure Machine Learning (pré-visualização) ponto a ponto. Esta é a parte dois e aborda a experimentação.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: jmartens
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 3/15/2018
ms.openlocfilehash: 77dcad0f3e49b601110f8700245aaf479bde1c4e
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38722784"
---
# <a name="tutorial-2-classify-iris---build-a-model"></a>Tutorial: Classificar Íris, parte 2 – Criar um modelo
Os serviços do Azure Machine Learning (pré-visualização) são uma solução de análise avançada e ciência de dados integrada para os cientistas de dados profissionais prepararem dados, desenvolverem experimentações e implementarem modelos à escala da cloud.

Este tutorial é a **segunda parte de uma série composta por três partes**. Nesta parte do tutorial, vai utilizar os serviços do Azure Machine Learning para:

> [!div class="checklist"]
> * Abrir scripts e rever código
> * Executar scripts num ambiente local
> * Rever históricos de execuções
> * Executar scripts numa janela local da CLI do Azure
> * Executar scripts num ambiente Docker local
> * Executar scripts num ambiente Docker remoto
> * Executar scripts num ambiente do Azure HDInsight na cloud

Este tutorial utiliza o [conjunto de dados flor de Iris](https://en.wikipedia.org/wiki/Iris_flower_data_set) intemporal. 

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa de:
- Uma subscrição do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar. 
- Uma conta de experimentação e o Azure Machine Learning Workbench instalado, conforme descrito neste [início rápido](../service/quickstart-installation.md)
- Projeto e dados de Íris preparados do [Tutorial, parte 1](tutorial-classifying-iris-part-1.md)
- Um motor de Docker instalado e em execução localmente. A edição Community Edition do Docker é suficiente. Saiba como instalar o Docker aqui: https://docs.docker.com/engine/installation/.

## <a name="review-irissklearnpy-and-the-configuration-files"></a>Reveja os ficheiros de configuração e iris_sklearn.py

1. Abra a aplicação Azure Machine Learning Workbench.

1. Abra o projeto **myIris** que criou na [Parte 1 da série de tutoriais](tutorial-classifying-iris-part-1.md).

2. No projeto aberto, selecione o botão **Ficheiros** (o ícone de pasta), no painel mais à esquerda, para abrir a lista de ficheiros na pasta do projeto.

   ![Abrir o projeto do Azure Machine Learning Workbench](media/tutorial-classifying-iris/2-project-open.png)

3. Selecione o ficheiro de script de Python **iris_sklearn.py**. 

   ![Escolher um script](media/tutorial-classifying-iris/2-choose-iris_sklearn.png)

   O código é aberto num novo separador do editor de texto dentro do Workbench. Este é o script que vai utilizar ao longo desta parte do tutorial. 

   >[!NOTE]
   >O código que vir pode não ser exatamente igual ao código anterior, pois este projeto de exemplo é atualizado frequentemente.
   
   ![Abrir um ficheiro](media/tutorial-classifying-iris/open_iris_sklearn.png)

4. Examine o código do script de Python para se familiarizar com o estilo de programação. 

   O script **iris_sklearn.py** executa as seguintes tarefas:

   * Carrega o pacote de preparação de dados predefinido chamado **iris.dprep** para criar um [pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html). 

   * Adiciona funcionalidades aleatórias para tornar o problema mais difícil de resolver. A aleatoriedade é necessária porque o Íris é um conjunto de dados pequeno que pode ser facilmente classificado com uma precisão de quase 100%.

   * Utiliza a biblioteca de machine learning [scikit-learn](http://scikit-learn.org/stable/index.html) para criar um modelo de regressão logística simples.  Esta biblioteca é fornecida com o Azure Machine Learning Workbench por predefinição.

   * Serializa o modelo ao utilizar a biblioteca [pickle](https://docs.python.org/3/library/pickle.html) num ficheiro na pasta `outputs`. 
   
   * Carrega o modelo serializado e, em seguida, anula a serialização do mesmo, carregando-o novamente para a memória.

   * Utiliza o modelo com a serialização anulada para fazer uma predição num registo novo. 

   * Desenha dois gráficos, uma matriz de confusão e uma curva ROC (receiver operating characteristic) multiclasse mediante a utilização da biblioteca [matplotlib](https://matplotlib.org/) e guarda-os na pasta `outputs`. Pode instalar esta biblioteca no seu ambiente, se ainda não estiver aí presente.

   * Desenha a taxa de regularização e a precisão do modelo no histórico de execuções automaticamente. O objeto `run_logger` é utilizado transversalmente para registar a taxa de regularização e a exatidão do modelo nos registos. 


## <a name="run-irissklearnpy-in-your-local-environment"></a>Executar o script iris_sklearn.py no seu ambiente local

1. Abra a interface de linha de comandos (CLI) do Azure Machine Learning:
   1. Inicie o Azure Machine Learning Workbench.

   1. No menu Workbench, selecione **Ficheiro** > **Abrir Linha de Comandos**. 
   
   A janela da interface de linha de comandos (CLI) do Azure Machine Learning é aberta na pasta de projeto `C:\Temp\myIris\>` no Windows. Este projeto é o mesmo que criou na Parte 1 do tutorial.

   >[!IMPORTANT]
   >Tem de utilizar esta janela da CLI para realizar os passos seguintes.

1. Na janela da CLI, instale a biblioteca de desenho de Python, **matplotlib**, caso ainda não a tenha.

   O script **iris_sklearn.py** tem dependências em dois pacotes de Python: **scikit-learn** e **matplotlib**.  O pacote **scikit-learn** é instalado pelo Azure Machine Learning Workbench para sua comodidade. No entanto, tem de instalar o pacote **matplotlib** caso este ainda não esteja instalado.

   Se avançar sem ter o **matplotlib** instalado, isso não impede a execução bem-sucedida do código deste tutorial. Contudo, o código não conseguirá produzir os gráficos de saída da matriz de confusão nem da curva ROC multiclasse, conforme mostrado nas visualizações do histórico.

   ```azurecli
   pip install matplotlib
   ```

   Esta instalação demora cerca de um minuto.

1. Regresse à aplicação Workbench. 

1. Localize o separador chamado **iris_sklearn.py**. 

   ![Localizar separador com o script](media/tutorial-classifying-iris/2-iris_sklearn-tab.png)

1. Na barra de ferramentas desse separador, selecione **local** como o ambiente de execução e `iris_sklearn.py` como o script a executar. Estes podem já estar selecionados.

   ![Escolha de local e script](media/tutorial-classifying-iris/2-local-script.png)

1. Vá para o lado direito da barra de ferramentas e introduza `0.01` no campo **Argumentos**. 

   Este valor corresponde à taxa de regularização do modelo de regressão logística.

   ![Escolha de local e script](media/tutorial-classifying-iris/2-local-script-arguments.png)

1. Selecionar o botão **Executar**. É agendado imediatamente um trabalho. O trabalho é listado no painel **Trabalhos**, no lado direito da janela do Workbench. 

   ![Escolha de local e script](media/tutorial-classifying-iris/2-local-script-arguments-run.png)

   Ao fim de alguns instantes, o estado do trabalho passa de **A submeter** para **Em execução** e, por fim, para **Concluído**.

1. Selecione **Concluído** no texto do estado do trabalho, no painel **Trabalhos**. 

   ![Executar sklearn](media/tutorial-classifying-iris/2-completed.png)

   Abre-se uma janela de pop-up, que apresenta o texto de saída padrão (stdout) para a execução. Para fechar o texto stdout, selecione o botão **Fechar** (**x**), no canto superior direito da janela do pop-up.

   ![Saída padrão](media/tutorial-classifying-iris/2-standard-output.png)

9. No mesmo estado de trabalho no painel **Trabalhos**, selecione o texto azul **iris_sklearn.py [n]** (_n_ é o número de execuções) imediatamente acima do estado **Concluído** e da hora de início. É aberta a janela **Propriedades da Execução**, que mostra as informações abaixo relativas a essa execução:
   - Informações das **Propriedades da Execução**
   - **Saídas**
   - **Métricas**
   - **Visualizações**, se existentes
   - **Registos** 

   Quando a execução estiver concluída, a janela de pop-up mostra os resultados abaixo:

   >[!NOTE]
   >Uma vez que o tutorial introduziu alguma aleatoriedade no conjunto de preparação anteriormente, os resultados exatos podem ser ligeiramente diferentes dos apresentados aqui.

   ```text
   Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  5 2016, 11:41:13) [MSC v.1900 64 bit (AMD64)]
   
   Iris dataset shape: (150, 5)
   Regularization rate is 0.01
   LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
          intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
          penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
          verbose=0, warm_start=False)
   Accuracy is 0.6792452830188679
   
   ==========================================
   Serialize and deserialize using the outputs folder.
   
   Export the model to model.pkl
   Import the model from model.pkl
   New sample: [[3.0, 3.6, 1.3, 0.25]]
   Predicted class is ['Iris-setosa']
   Plotting confusion matrix...
   Confusion matrix in text:
   [[50  0  0]
    [ 1 37 12]
    [ 0  4 46]]
   Confusion matrix plotted.
   Plotting ROC curve....
   ROC curve plotted.
   Confusion matrix and ROC curve plotted. See them in Run History details pane.
   ```
    
10. Feche o separador **Propriedades da Execução** e regresse ao separador **iris_sklearn.py**. 

11. Repita para execuções adicionais. 

    Introduza uma série de valores no campo **Argumentos**, entre `0.001` e `10`. Selecione **Executar** para executar o código mais algumas vezes. O valor de argumento que alterar em cada execução é fornecido ao modelo de regressão logística no código, o que resulta sempre em resultados diferentes.

## <a name="review-the-run-history-in-detail"></a>Rever o histórico de execuções em detalhe
No Azure Machine Learning Workbench, cada execução de script é capturada como um registo de histórico de execuções. Se abrir a vista **Execuções**, pode ver o histórico de execuções de scripts específicos.

1. Para abrir a lista de **Execuções**, selecione o botão **Execuções** (ícone de relógio), na barra de ferramentas do lado esquerdo. Em seguida, selecione **iris_sklearn.py** para mostrar o **Dashboard da Execução** de `iris_sklearn.py`.

   ![Vista de execução](media/tutorial-classifying-iris/run_view.png)

1. O separador **Dashboard da Execução** abre-se. 

   Reveja as estatísticas capturadas nas várias execuções. Os gráficos são compostos na parte superior do separador. Cada execução tem um número consecutivo e os detalhes da execução estão listados na tabela na parte inferior do ecrã.

   ![Dashboard da execução](media/tutorial-classifying-iris/run_dashboard.png)

1. Filtre a tabela e selecione um dos gráficos para ver o estado, a duração, a precisão e a taxa de regularização de cada execução. 

1. Selecione as caixas de verificação junto de duas ou mais execuções na tabela **Execuções**. Selecione o botão **Comparar** para abrir um painel de comparação detalhado. Reveja a comparação lado a lado. 

1. Para regressar a **Dashboard da Execução**, selecione o botão de retrocesso **Lista de Execuções** no canto superior esquerdo do painel **Comparação**.

   ![Regressar a Lista de execuções](media/tutorial-classifying-iris/2-compare-back.png)

1. Selecione uma execução individual para ver a vista de detalhes da mesma. Repare que as estatísticas da execução selecionada são listadas na secção **Propriedades da Execução**. Os ficheiros escritos na pasta de saída são listados na secção **Saídas** e podem ser transferidos a partir daí.

   ![Detalhes da Execução](media/tutorial-classifying-iris/run_details.png)

   Os dois gráficos -- matriz de confusão e curva COR multiclasse -- são compostos na secção **Visualizações**. Também estão disponíveis na secção **Registos** todos os ficheiros de registos.


## <a name="run-scripts-in-local-docker-environments"></a>Executar scripts em ambientes do Docker locais

Opcionalmente, pode experimentar a execução de scripts em relação a um contentor de Docker local. Pode configurar ambientes de execução adicionais, como o Docker, e executar o script nos mesmos. 

>[!NOTE]
>Para experimentar a emissão de scripts para execução num contentor do Docker numa VM do Azure remota ou num cluster do Spark do HDInsight, pode seguir as [ instruções para criar uma Máquina Virtual de Ciência de Dados do Azure baseada em Ubuntu ou um cluster do HDInsight](how-to-create-dsvm-hdi.md).

1. Se ainda não o tiver feito, instale e inicie o Docker localmente no seu computador Windows ou MacOS. Para obter mais informações, veja as instruções de instalação do Docker em https://docs.docker.com/install/. A edição Community Edition é suficiente.

1. No painel do lado esquerdo, selecione o ícone de **Pasta** para abrir a lista **Ficheiros** do seu projeto. Expanda a pasta `aml_config`. 

2. Existem vários ambientes pré-configurados: **docker-python**, **docker-spark** e **local**. 

   Cada ambiente tem dois ficheiros, tais como `docker.compute` (para **docker-python** e **docker-spark**) e `docker-python.runconfig`. Abra cada um dos ficheiros e verá que algumas opções podem ser configuradas no editor de texto.  

   Para limpar, selecione **Fechar** (**x**) nos separadores de qualquer editor aberto.

3. Execute o script **iris_sklearn.py** com o ambiente **docker-python**: 

   - Na barra de ferramentas do lado esquerdo, selecione o ícone de **Relógio** para abrir o painel **Execuções**. Selecione **Todas as Execuções**. 

   - Na parte superior do separador **Todas as Execuções**, selecione **docker-python** como o ambiente de destino em vez do ambiente **local** predefinido. 

   - Em seguida, vá para o lado direito e selecione **iris_sklearn.py** como o script a executar. 

   - Deixe o campo **Argumentos** em branco, pois o script especifica um valor predefinido. 

   - Selecionar o botão **Executar**.

4. Observe que é iniciado um trabalho novo. Aparece no painel **Trabalhos**, no lado direito da janela do Workbench.

   Quando fizer a execução no Docker pela primeira vez, são necessários mais alguns minutos para concluir o trabalho. 

   Nos bastidores, o Azure Machine Learning Workbench compila um novo ficheiro do Docker. 
   O ficheiro novo referencia a imagem base do Docker especificada no ficheiro `docker.compute` e os pacotes de Python de dependência especificados no ficheiro `conda_dependencies.yml`. 
   
   O motor do Docker realiza as seguintes tarefas:

    - Transfere a imagem base a partir do Azure.
    - Instala os pacotes de Python especificados no ficheiro `conda_dependencies.yml`.
    - Inicia um contentor do Docker.
    - Copia ou referencia, dependendo da configuração da execução, a cópia local da pasta do projeto.      
    - Executa o script `iris_sklearn.py`.

   No final, deverá ver o mesmo resultado de quando **local** é o destino.

5. Agora, vamos experimentar o Spark. A imagem base do Docker contém uma instância do Spark previamente instalada e configurada que pode utilizar para executar um script PySpark. A utilização desta imagem base permite desenvolver e testar o seu programa Spark de uma forma fácil, sem que tenha de dedicar tempo a instalar e configurar o Spark. 

   Abra o ficheiro `iris_spark.py`. Este script carrega o ficheiro de dados `iris.csv` e utiliza o algoritmo de regressão logística da biblioteca de Spark do Machine Learning para classificar o conjunto de dados Íris. Agora, altere o ambiente de execução para **docker-spark** e o script para **iris_spark.py** e volte a executá-lo. Este processo demora um pouco mais, uma vez que tem de ser criada e iniciada uma sessão do Spark dentro do contentor do Docker. Também pode ver que o stdout é diferente do stdout de `iris_spark.py`.

6. Inicie mais algumas execuções e experimente diferentes argumentos. 

7. Abra o ficheiro `iris_spark.py` para ver o modelo de regressão logística criado com a biblioteca de Spark do Machine Learning. 

8. Interaja com o painel **Trabalhos**, execute uma vista de lista do histórico de execuções e execute uma vista de detalhes das suas execuções em diferentes ambientes.

## <a name="run-scripts-in-the-cli-window"></a>Executar scripts na janela da CLI

1. Abra a interface de linha de comandos (CLI) do Azure Machine Learning:
   1. Inicie o Azure Machine Learning Workbench.

   1. No menu Workbench, selecione **Ficheiro** > **Abrir Linha de Comandos**. 
   
   A linha de comandos da CLI é aberta na pasta do projeto `C:\Temp\myIris\>` no Windows. Este é o projeto que criou na Parte 1 do tutorial.

   >[!IMPORTANT]
   >Tem de utilizar esta janela da CLI para realizar os passos seguintes.

1. Na janela da CLI, inicie sessão no Azure. [Obter mais informações sobre o comando az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

   Pode ser que já tenha a sessão iniciada. Nesse caso, pode ignorar este passo.

   1. Na linha de comandos, escreva:
      ```azurecli
      az login
      ```

      Este comando devolve um código para utilizar no seu browser em https://aka.ms/devicelogin.

   1. Aceda a https://aka.ms/devicelogin no seu browser.

   1. Quando lhe for pedido, introduza o código, aquele que recebeu na CLI, no seu browser.

   A aplicação Workbench e a CLI utilizam caches de credenciais independentes quando efetuam a autenticação nos recursos do Azure. Depois de iniciar sessão, não terá de se autenticar novamente até o token em cache expirar. 

1. Se a sua organização tiver várias subscrições do Azure (ambiente empresarial), tem de definir a subscrição a ser utilizada. Localize a sua subscrição, defina-a através da utilização do ID de subscrição e, em seguida, teste-a.

   1. Liste todas as subscrições do Azure a que tem acesso com este comando:
   
      ```azurecli
      az account list -o table
      ```

      O comando **az account list** devolve a lista de subscrições disponíveis para o seu início de sessão. 
      Se existir mais do que uma, identifique o valor do ID da subscrição que diz respeito à subscrição que pretende utilizar.

   1. Defina a subscrição do Azure que pretende utilizar como conta predefinida:
   
      ```azurecli
      az account set -s <your-subscription-id>
      ```
      em que \<ID-da-sua-subscrição\> é o valor do ID da subscrição que pretende utilizar. Não inclua os parêntesis angulares.

   1. Confirme a nova definição de subscrição ao pedir os detalhes da subscrição atual. 

      ```azurecli
      az account show
      ```    

1. Na janela da CLI, instale a biblioteca de desenho de Python, **matplotlib**, caso ainda não a tenha.

   ```azurecli
   pip install matplotlib
   ```

1. Na janela da CLI, submeta o script **iris_sklearn.py** como uma experimentação.

   A execução de iris_sklearn.py é feita no contexto da computação local.

   + No Windows:
     ```azurecli
     az ml experiment submit -c local .\iris_sklearn.py
     ```

   + Em MacOS:
     ```azurecli
     az ml experiment submit -c local iris_sklearn.py
     ```
   
   A saída obtida deve ser semelhante a:
    ```text
    RunId: myIris_1521077190506
    
    Executing user inputs .....
    ===========================
    
    Python version: 3.5.2 |Continuum Analytics, Inc.| (default, Jul  2 2016, 17:52:12) 
    [GCC 4.2.1 Compatible Apple LLVM 4.2 (clang-425.0.28)]
    
    Iris dataset shape: (150, 5)
    Regularization rate is 0.01
    LogisticRegression(C=100.0, class_weight=None, dual=False, fit_intercept=True,
              intercept_scaling=1, max_iter=100, multi_class='ovr', n_jobs=1,
              penalty='l2', random_state=None, solver='liblinear', tol=0.0001,
              verbose=0, warm_start=False)
    Accuracy is 0.6792452830188679
        
    ==========================================
    Serialize and deserialize using the outputs folder.
    
    Export the model to model.pkl
    Import the model from model.pkl
    New sample: [[3.0, 3.6, 1.3, 0.25]]
    Predicted class is ['Iris-setosa']
    Plotting confusion matrix...
    Confusion matrix in text:
    [[50  0  0]
     [ 1 37 12]
     [ 0  4 46]]
    Confusion matrix plotted.
    Plotting ROC curve....
    ROC curve plotted.
    Confusion matrix and ROC curve plotted. See them in Run History details page.
    
    Execution Details
    =================
    RunId: myIris_1521077190506
    ```

1. Reveja a saída. A saída e os resultados são iguais aos que obteve quando utilizou o Workbench para executar o script. 

1. Na janela da CLI, execute o script de Python, **iris_sklearn.py**, novamente com um ambiente de execução do Docker (se tiver o Docker instalado no seu computador).

   + Se o contentor estiver no Windows: 
     |Execução<br/>environment|Comando no Windows|
     |---------------------|------------------|
     |Python|`az ml experiment submit -c docker-python .\iris_sklearn.py 0.01`|
     |Spark|`az ml experiment submit -c docker-spark .\iris_spark.py 0.1`|

   + Se o contentor estiver em MacOS: 
     |Execução<br/>environment|Comando no Windows|
     |---------------------|------------------|
     |Python|`az ml experiment submit -c docker-python iris_sklearn.py 0.01`|
     |Spark|`az ml experiment submit -c docker-spark iris_spark.py 0.1`|

1. Volte ao Workbench e:
   1. Selecione o ícone de pasta no painel da esquerda para listar os ficheiros do projeto.
   
   1. Abra o script de Python com o nome **run.py**. Este script é útil para realizar um ciclo através de vários taxas de regularização. 

   ![Regressar a Lista de execuções](media/tutorial-classifying-iris/2-runpy.png)

1. Execute a experimentação múltiplas vezes com estas taxas. 

   Este script inicia um trabalho `iris_sklearn.py` com uma taxa de regularização de `10.0` (um número extremamente grande). Depois, o script diminui a taxa para metade na execução seguinte, e assim sucessivamente, até que a taxa não seja inferior a `0.005`. 

   O script contém o seguinte código:

   ![Regressar a Lista de execuções](media/tutorial-classifying-iris/2-runpy-code.png)

1. Execute o script **run.py** a partir da linha de comandos, do seguinte modo:

   ```cmd
   python run.py
   ```

   Este comando submete o script iris_sklearn.py várias vezes com diferentes taxas de regularização

   Após a conclusão de `run.py`, pode ver gráficos de diferentes métricas na vista de lista do histórico de execuções no Workbench.

## <a name="run-scripts-in-a-remote-docker-container"></a>Executar scripts num contentor do Docker remoto
Para executar o script num contentor do Docker num computador Linux remoto, tem de ter acesso SSH (nome de utilizador e palavra-passe) a esse computador remoto. Além disso, o computador tem de ter um motor do Docker instalado e em execução. A forma mais fácil de obter um computador Linux destes é criar uma Máquina Virtual de Ciência de Dados (DSVM) baseada em Ubuntu no Azure. Saiba [como criar um DSVM em Ubuntu para utilizar no Azure ML Workbench](how-to-create-dsvm-hdi.md#create-an-ubuntu-dsvm-in-azure-portal).

>[!NOTE] 
>Tenha em conta que a DSVM baseada em CentOS *não* é suportada.

1. Quando a VM estiver criada, pode ligá-la como ambiente de execução ao gerar um par de ficheiros `.runconfig` e `.compute`. Utilize o comando abaixo para criá-los. 

 Vamos atribuir ao novo destino de computação o nome `myvm`.
 
   ```azurecli
   az ml computetarget attach remotedocker --name myvm --address <your-IP> --username <your-username> --password <your-password>
   ```
   
   >[!NOTE]
   >O endereço IP também pode ser um nome de domínio completamente qualificado (FQDN) endereçável publicamente, como `vm-name.southcentralus.cloudapp.azure.com`. É boa prática adicionar um FQDN à sua DSVM e utilizá-lo em vez de um endereço IP. Esta prática é útil porque poderá desativar a VM a determinada altura para poupar custos. Além disso, da próxima vez que iniciar a VM, o endereço IP pode ter sido alterado.

   >[!NOTE]
   >Além da autenticação com o nome de utilizador e a palavra-passe, pode especificar uma chave privada e a frase de acesso correspondente (se aplicável) através das opções `--private-key-file` e `--private-key-passphrase` (opcionalmente).

   Em seguida, prepare o destino de computação **myvm** com a execução deste comando.
   
   ```azurecli
   az ml experiment prepare -c myvm
   ```
   
   O comando anterior cria a imagem do Docker na VM de modo a prepará-la para a execução dos scripts.
   
   >[!NOTE]
   >Também pode alterar o valor de `PrepareEnvironment` em `myvm.runconfig` do valor predefinido `false` para `true`. Esta alteração prepara automaticamente o contentor do Docker como parte da primeira execução.

2. Edite o ficheiro `myvm.runconfig` gerado em `aml_config` e altere a estrutura do valor predefinido `PySpark` para `Python`:

   ```yaml
   Framework: Python
   ```
   >[!NOTE]
   >Embora o PySpark também deva funcionar, o Python é mais eficiente se não precisar de uma sessão do Spark para executar o script Python.

3. Emita o mesmo comando como fez anteriormente na janela da CLI, mas desta vez utilize o destino _myvm_ para executar o script iris_sklearn.py num contentor do Docker remoto:
   ```azurecli
   az ml experiment submit -c myvm iris_sklearn.py
   ```
   O comando é executado como se estivesse a utilizar um ambiente `docker-python`, com a diferença de que a execução tem lugar na VM do Linux remota. A janela da CLI apresenta as mesmas informações de saída.

4. Vamos experimentar com o Spark no contentor. Abra o Explorador de Ficheiros. Faça uma cópia do ficheiro `myvm.runconfig` e dê-lhe o nome `myvm-spark.runconfig`. Edite o ficheiro novo para alterar a definição `Framework` de `Python` para `PySpark`:
   ```yaml
   Framework: PySpark
   ```
   Não faça nenhuma alteração ao ficheiro `myvm.compute`. Para a execução no Spark, é utilizada a mesma imagem do Docker na mesma VM. No `myvm-spark.runconfig` novo, o campo `Target` aponta para o mesmo ficheiro `myvm.compute` através do respetivo nome `myvm`.

5. Escreva o comando seguinte para executar o script **iris_spark.py** na instância do Spark em execução no contentor do Docker remoto:
   ```azureli
   az ml experiment submit -c myvm-spark .\iris_spark.py
   ```

## <a name="run-scripts-in-hdinsight-clusters"></a>Executar scripts em clusters do HDInsight
Também pode executar este script num cluster do HDInsight Spark. Saiba [como criar um Cluster do Spark do HDInsight para utilizar no Azure ML Workbench](how-to-create-dsvm-hdi.md#create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal).

>[!NOTE] 
>O cluster do HDInsight tem de utilizar o Blob do Azure como armazenamento principal. A utilização do armazenamento do Azure Data Lake ainda não é suportada.

1. Se tiver acesso a um cluster do Spark para o Azure HDInsight, gere um comando de configuração de execução do HDInsight, conforme mostrado aqui. Indique como parâmetros o nome do cluster do HDInsight e o seu nome de utilizador e a sua palavra-passe do HDInsight. 

   Utilize o seguinte comando para criar um destino de computação que aponte para um cluster do HDInsight:

   ```azurecli
   az ml computetarget attach cluster --name myhdi --address <cluster head node FQDN> --username <your-username> --password <your-password>
   ```

   Para preparar o cluster do HDInsight, execute este comando:

   ```
   az ml experiment prepare -c myhdi
   ```

   Tipicamente, o FQDN do nó principal do cluster é `<your_cluster_name>-ssh.azurehdinsight.net`.

   >[!NOTE]
   >O `username` é o nome de utilizador SSH do cluster definido durante a configuração do HDInsight. Por predefinição, o valor é `sshuser`. Não é `admin`, que é o outro utilizador criado durante a configuração e que permite o acesso ao Web site de administração do cluster. 

2. Execute o script **iris_spark.py** no cluster do HDInsight com este comando:

   ```azurecli
   az ml experiment submit -c myhdi .\iris_spark.py
   ```

   >[!NOTE]
   >Quando executa num cluster do HDInsight remoto, também pode ver os detalhes da execução do trabalho do Yet Another Resource Negotiator (YARN), em `https://<your_cluster_name>.azurehdinsight.net/yarnui`, com a conta de utilizador `admin`.

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Passos seguintes
Nesta segunda parte da série de tutoriais composta por três partes, aprendeu a:
> [!div class="checklist"]
> * Abrir scripts e analisar o código no Workbench
> * Executar scripts num ambiente local
> * Rever o histórico de execuções
> * Executar scripts num ambiente Docker local

Agora, pode experimentar a terceira parte desta série de tutoriais, na qual pode implementar o modelo de regressão logística criado como um serviço Web em tempo real.

> [!div class="nextstepaction"]
> [Tutorial, parte 3 – Implementar modelos](tutorial-classifying-iris-part-3.md)
