---
title: Controle experimentações e métricas de treinamento - Azure Machine Learning | Documentos da Microsoft
description: Com o serviço Azure Machine Learning, pode controlar suas experimentações e monitorizar as métricas para melhorar o processo de criação de modelo. Saiba como adicionar registos ao seu script de treinamento, como submeter a experimentação, como verificar o progresso de uma tarefa em execução e como visualizar os resultados de uma execução.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: 3256c8815b19f9b070cce3cd422f92c296e3e5c3
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/11/2018
ms.locfileid: "49115187"
---
# <a name="track-experiments-and-training-metrics-in-azure-machine-learning"></a>Controle experimentações e métricas de formação no Azure Machine Learning

No serviço do Azure Machine Learning, pode controlar suas experimentações e monitorizar as métricas para melhorar o processo de criação de modelo. Neste artigo, irá aprender sobre as diferentes formas de adicionar registos ao seu script de treinamento, como submeter a experimentação com **start_logging** e **ScriptRunConfig**, como verificar o progresso de um tarefa de execução e como visualizar os resultados de uma execução. 

## <a name="list-of-training-metrics"></a>Lista de métricas de treinamento 

As métricas seguintes podem ser adicionadas a uma execução enquanto uma experimentação de preparação. Para ver uma lista mais detalhada das quais podem ser acompanhadas numa execução, consulte a [documentação de referência do SDK](https://docs.microsoft.com/python/api/overview/azure/azure-ml-sdk-overview?view=azure-ml-py).

|Tipo| Função de Python | Notas|
|----|:----:|:----:|
|Valores escalares | `run.log(name, value, description='')`| Inicie a sessão de um valor de métrica para a execução com o nome fornecido. Registo de uma métrica para uma execução faz com que essa métrica sejam armazenadas no registo de execução na experimentação.  Pode se conectar a mesma métrica várias vezes dentro de uma execução, o resultado a ser considerado um vetor dessa métrica.|
|Listas| `run.log_list(name, value, description='')`|Inicie a sessão de um valor de métrica de lista para a execução com o nome fornecido.|
|Linha| `run.log_row(name, description=None, **kwargs)`|Usando *log_row* cria uma métrica de tabela com colunas, conforme descrito em kwargs. Cada parâmetro nomeado gera uma coluna com o valor especificado.  *log_row* pode ser chamado uma vez para iniciar sessão uma tupla arbitrária, ou várias vezes num loop para gerar uma tabela completa.|
|Tabela| `run.log_table(name, value, description='')`| Inicie a sessão de uma métrica de tabela para a execução com o nome fornecido. |
|Imagens| `run.log_image(name, path=None, plot=None)`|Inicie a sessão de uma métrica de imagem para o registo de execução. Utilizar log_image para registar um ficheiro de imagem ou um matplotlib gráfico para a execução.  Estas imagens serão visíveis e comparável no registo de execução.|
|Marcar uma execução| `run.tag(key, value=None)`|Marca a execução com uma chave de cadeia de caracteres e o valor de cadeia de caracteres opcional.|
|Carregar o ficheiro ou diretório|`run.upload_file(name, path_or_stream)`|Carregar um ficheiro para o registo de execução. Execuções de recolher automaticamente o ficheiro no diretório de saída especificado, o que está predefinida para ". / saídas" para a mais tipos de execução.  Utilize upload_file apenas quando os ficheiros adicionais devem ser carregados ou não for especificado um diretório de saída. Sugerimos que adicionar `outputs` para o nome, de modo que ele é carregado para o diretório de saídas. Pode listar todos os ficheiros que estão associados com esta opção executar o registo por chamada `run.get_file_names()`|

> [!NOTE]
> As métricas de escalares, listas, linhas e tabelas podem ter o tipo: número de vírgula flutuante, número inteiro ou uma cadeia.

## <a name="log-metrics-for-experiments"></a>Métricas de registo de experimentações

Se pretender controlar ou monitorizar a sua experimentação, tem de adicionar o código para iniciar o log ao submeter a execução. Seguem-se formas para acionar o envio de execução:
* __Run.start_logging__ - adicionar funções de registo para o script de treinamento e iniciar uma sessão de registo interativo na experimentação do especificado. **start_logging** cria uma execução interativa para utilização em cenários como blocos de notas. Qualquer métricas que são registadas durante a sessão são adicionadas ao registo de execução na experimentação.
* __ScriptRunConfig__ - adicionar funções de registo para o script de formação e a pasta de script inteiro com a execução de carga.  **ScriptRunConfig** é uma classe para a configuração de configurações para o script é executado. Com esta opção, pode adicionar o código de monitorização para ser notificado da conclusão de ou para obter um widget visual para monitorizar.

## <a name="set-up-the-workspace-and-experiment"></a>Configurar a área de trabalho e experimentação
Antes de adicionar o registo e submeter uma experimentação, tem de configurar a área de trabalho e a experimentação.

1. Carregar a área de trabalho. Para saber mais sobre como definir a configuração de área de trabalho, siga os [guia de introdução](https://docs.microsoft.com/azure/machine-learning/service/quickstart-get-started).

  ```python
  from azureml.core import Workspace, Run
  import azureml.core
  
  ws = Workspace(workspace_name = <<workspace_name>>,
               subscription_id = <<subscription_id>>,
               resource_group = <<resource_group>>)
   ```

2. Crie a experimentação.

  ```python
  from azureml.core import Experiment

  # make up an arbitrary name
  experiment_name = 'train-in-notebook'
  exp = Experiment(workspace_object = ws, name = experiment_name)
  ```
  
## <a name="option-1-use-startlogging"></a>Opção 1: Utilizar start_logging

**start_logging** cria uma execução interativa para utilização em cenários como blocos de notas. Qualquer métricas que são registadas durante a sessão são adicionadas ao registo de execução na experimentação.

O exemplo seguinte prepara um modelo simples de sklearn Ridge localmente num bloco de notas Jupyter local. Para saber mais sobre como enviar experimentações em ambientes diferentes, veja [configurar destinos de computação de preparação de modelos com o serviço Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/how-to-set-up-training-targets).

1. Crie um script de treinamento num bloco de notas Jupyter local. 

  ``` python
  # load diabetes dataset, a well-known small dataset that comes with scikit-learn
  from sklearn.datasets import load_diabetes
  from sklearn.linear_model import Ridge
  from sklearn.metrics import mean_squared_error
  from sklearn.model_selection import train_test_split
  from sklearn.externals import joblib

  X, y = load_diabetes(return_X_y = True)
  columns = ['age', 'gender', 'bmi', 'bp', 's1', 's2', 's3', 's4', 's5', 's6']
  X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
  data = {
      "train":{"X": X_train, "y": y_train},        
      "test":{"X": X_test, "y": y_test}
  }
  reg = Ridge(alpha = 0.03)
  reg.fit(data['train']['X'], data['train']['y'])
  preds = reg.predict(data['test']['X'])
  print('Mean Squared Error is', mean_squared_error(preds, data['test']['y']))
  joblib.dump(value = reg, filename = 'model.pkl');
  ```

2. Adicionar controlo de experimentação com o SDK do serviço do Azure Machine Learning e carregar um modelo persistente para a experimentação executar o registo. O código a seguir adiciona etiquetas, os registos e carrega um ficheiro de modelo para a experimentação executar.

  ```python
  run = Run.start_logging(experiment = exp)
  run.tag("Description","My first run!")
  run.log('alpha', 0.03)
  reg = Ridge(alpha = 0.03)
  reg.fit(data['train']['X'], data['train']['y'])
  preds = reg.predict(data['test']['X'])
  run.log('mse', mean_squared_error(preds, data['test']['y']))
  joblib.dump(value = reg, filename = 'model.pkl')
  # Upload file directly to the outputs folder
  run.upload_file(name = 'outputs/model.pkl', path_or_stream = './model.pkl')

  run.complete()
  ```

O script termina com ```run.complete()```, que marca a execução como concluída.  Isso é normalmente utilizado em cenários de bloco de notas interativo.

## <a name="option-2-use-scriptrunconfig"></a>Opção 2: Utilizar ScriptRunConfig

**ScriptRunConfig** é uma classe para a configuração de configurações para o script é executado. Com esta opção, pode adicionar o código de monitorização para ser notificado da conclusão de ou para obter um widget visual para monitorizar.

Este exemplo Expande o modelo básico de sklearn Ridge acima. Ele faz um parâmetro simple paramétrico para paramétrico sobre os valores alfabéticos do modelo para capturar métricas com modelos de formação em é executado sob a experimentação. O exemplo é executado localmente em relação a um ambiente gerenciado por utilizador. 

1. Crie um script de treinamento. Esta opção utiliza ```%%writefile%%``` escrever o código de treinamento para a pasta de script como ```train.py```.

  ```python
  %%writefile $project_folder/train.py

  import os
  from sklearn.datasets import load_diabetes
  from sklearn.linear_model import Ridge
  from sklearn.metrics import mean_squared_error
  from sklearn.model_selection import train_test_split
  from azureml.core.run import Run
  from sklearn.externals import joblib

  import numpy as np

  #os.makedirs('./outputs', exist_ok = True)

  X, y = load_diabetes(return_X_y = True)

  run = Run.get_submitted_run()

  X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
  data = {"train": {"X": X_train, "y": y_train},
          "test": {"X": X_test, "y": y_test}}

  # list of numbers from 0.0 to 1.0 with a 0.05 interval
  alphas = mylib.get_alphas()

  for alpha in alphas:
      # Use Ridge algorithm to create a regression model
      reg = Ridge(alpha = alpha)
      reg.fit(data["train"]["X"], data["train"]["y"])

      preds = reg.predict(data["test"]["X"])
      mse = mean_squared_error(preds, data["test"]["y"])
      # log the alpha and mse values
      run.log('alpha', alpha)
      run.log('mse', mse)

      model_file_name = 'ridge_{0:.2f}.pkl'.format(alpha)
      # save model in the outputs folder so it automatically get uploaded
      with open(model_file_name, "wb") as file:
          joblib.dump(value = reg, filename = model_file_name)

      # upload the model file explicitly into artifacts 
      run.upload_file(name = model_file_name, path_or_stream = model_file_name)

      # register the model
      #run.register_model(file_name = model_file_name)

      print('alpha is {0:.2f}, and mse is {1:0.2f}'.format(alpha, mse))
  
  ```

2. O ```train.py``` referências de script ```mylib.py```. Este ficheiro permite-lhe obter a lista de valores alfa para utilizar no modelo ridge.

  ```python
  %%writefile $script_folder/mylib.py
  import numpy as np

  def get_alphas():
      # list of numbers from 0.0 to 1.0 with a 0.05 interval
      return np.arange(0.0, 1.0, 0.05)
  ```

3. Configure um ambiente local do utilizador gerido.

  ```python
  from azureml.core.runconfig import RunConfiguration

  # Editing a run configuration property on-fly.
  run_config_user_managed = RunConfiguration()

  run_config_user_managed.environment.python.user_managed_dependencies = True

  # You can choose a specific Python environment by pointing to a Python path 
  #run_config.environment.python.interpreter_path = '/home/user/miniconda3/envs/sdk2/bin/python'
  ```

4. Submeter o ```train.py``` script seja executado no ambiente do utilizador gerido. Esta pasta de script inteiro é submetida para treinamento, incluindo o ```mylib.py``` ficheiro.

  ```python
  from azureml.core import ScriptRunConfig

  src = ScriptRunConfig(source_directory = script_folder, script = 'train.py', run_config = run_config_user_managed)
  run = exp.submit(src)
  ```
  
## <a name="view-run-details"></a>Vista de detalhes da execução

### <a name="monitor-run-with-jupyter-notebook-widgets"></a>Monitor de executar com widgets de bloco de notas do Jupyter
Quando utiliza a **ScriptRunConfig** executa o método para submeter, pode ver o progresso da execução com um widget de bloco de notas do Jupyter. Tal como a submissão da execução, o widget é assíncrono e disponibiliza atualizações dinâmicas a cada 10 a 15 segundos até à conclusão do trabalho.

1. Ver o widget de Jupyter enquanto aguarda a execução concluir.

  ```python
  from azureml.train.widgets import RunDetails
  RunDetails(run).show()
  ```

  ![Widget de bloco de notas de captura de ecrã do Jupyter](./media/how-to-track-experiments/widgets.PNG)

### <a name="get-log-results-upon-completion"></a>Obter resultados do registo após a conclusão

Modelo de formação e monitorização ocorre em segundo plano para que possam executar outras tarefas enquanto espera. Também pode aguardar até que o modelo de conclusão da formação antes de executar mais de código. Quando utiliza **ScriptRunConfig**, pode utilizar ```run.wait_for_completion(show_output = True)``` para mostrar quando a preparação de modelos está concluída. O ```show_output``` sinalizador dá-lhe uma saída verbosa ideal. 
  
### <a name="query-run-metrics"></a>Métricas de execução da consulta

Pode ver as métricas de usar um modelo preparado ```run.get_metrics()```. Agora pode obter todas as métricas que foram registadas no exemplo acima para determinar o melhor modelo.

## <a name="view-the-experiment-in-the-azure-portal"></a>Ver a experimentação no portal do Azure

Quando uma experimentação estiver concluída em execução, pode navegar para a experimentação gravada executar o registo. Pode fazê-lo de duas formas:

* Obter o URL para a execução diretamente ```print(run.get_portal_url())```
* Ver os detalhes da execução, enviando o nome da execução (neste caso, ```run```). Isso aponta para o nome de experimentação, ID, tipo, estado, página de detalhes, uma ligação para o portal do Azure e uma ligação para a documentação.

A ligação para a execução traz até diretamente para a página de detalhes da execução no portal do Azure. Aqui pode ver quaisquer propriedades, métricas registadas, imagens e gráficos que são registados na experimentação. Neste caso, fizemos logon MSE e os valores alfa.

  ![Captura de ecrã de detalhes da execução no portal do Azure](./media/how-to-track-experiments/run-details-page-web.PNG)

Também pode ver qualquer saídas ou registos para a execução, ou transferir o instantâneo da experimentação submetida para que possa partilhar a pasta de experimentação com outras pessoas.

## <a name="example-notebooks"></a>Blocos de notas de exemplo
Os seguintes blocos de notas demonstram os conceitos deste artigo:
* [01.Getting-Started/01.Train-within-Notebook/01.Train-within-Notebook.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/01.train-within-notebook)
* [01.Getting-Started/02.Train-on-local/02.Train-on-local.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local)

Obtenha estes blocos de notas: [!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Passos Seguintes

Experimente os passos seguintes para aprender a utilizar o Azure Machine Learning SDK para Python:

* Veja um exemplo de como registar o melhor modelo e implementá-la no tutorial [preparar um modelo de classificação de imagem com o Azure Machine Learning](tutorial-train-models-with-aml.md).

* Saiba como [formar modelos PyTorch com o Azure Machine Learning](how-to-train-pytorch.md).
