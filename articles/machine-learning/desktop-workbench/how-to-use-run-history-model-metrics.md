---
title: Como utilizar o Run histórico e métricas de modelo no Azure do Machine Learning Workbench | Documentos da Microsoft
description: Guia para a utilizar os recursos de histórico de execuções e métricas de modelo do Azure Machine Learning Workbench
services: machine-learning
author: rastala
ms.author: roastala
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/07/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 52b02460d444464211fc74c8982379424abebb5c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46965524"
---
# <a name="how-to-use-run-history-and-model-metrics-in-azure-machine-learning-workbench"></a>Como o histórico de execuções de utilização e métricas de modelo no Azure Machine Learning Workbench

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



O Azure Machine Learning Workbench oferece suporte a experimentação de ciência de dados por meio de seus **histórico de execuções** e **métricas de modelo** funcionalidades.
**Histórico de execuções** fornece um meio para controlar as saídas das suas experimentações de machine learning e, em seguida, permite a filtragem e comparação dos seus resultados.
**Métricas de modelo** pode ter sessão iniciada a partir de qualquer ponto dos seus scripts, de valores que são mais importantes em suas experiências de ciência de dados de controlo.
Este artigo descreve como fazer uso eficiente desses recursos para aumentar a taxa de e a qualidade da sua experimentação de ciência de dados.

## <a name="prerequisites"></a>Pré-requisitos
Para seguir este guia de procedimentos, terá de:
* [Criar e instalar o Azure Machine Learning](quickstart-installation.md)
- [Criar um projeto](quickstart-installation.md)


## <a name="azure-ml-logging-api-overview"></a>Descrição geral da API de registo do Azure ML
O [API de registo do Azure ML](reference-logging-api.md) está disponível através do **azureml.logging** módulo em Python (que é instalado com o Azure ML Workbench.) Depois de importar este módulo, pode utilizar o **get_azureml_logger** método para instanciar um **logger** objeto.
Em seguida, pode usar o logger **log** método para armazenar pares chave/valor produzidos pelos seus scripts de Python.
Atualmente, as métricas de modelo do registo de escalar e tipos de lista são suportados como mostrado.

```Python
# create a logger instance in already set up environment 
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()

# log scalar (any integer or floating point type is fine)
logger.log("simple value", 7)


# log list
logger.log("all values", [5, 6, 7])
```
É fácil de usar o agente de log nos seus projetos de Azure ML Workbench e este artigo mostra-lhe como fazer isso.

## <a name="create-a-project-in-azure-ml-workbench"></a>Criar um projeto no Azure ML Workbench
Se ainda não tiver um projeto, pode criar um a [criar e instalar o início rápido](quickstart-installation.md) da **Dashboard do projeto**, pode abrir o **iris_sklearn. PY** (do script conforme mostrado).

![aceder a um script do separador de ficheiros](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-01b.png)

Pode utilizar este script como um guia de implementação esperada de métrica de modelo, iniciar sessão no Azure ML.

## <a name="parameterize-and-log-model-metrics-from-script"></a>Parametrizar e métricas de modelo de registo de Script
Na **iris_sklearn. PY** do script, o padrão esperado para importação e a construção, o agente de log em Python que pode ser reduzido para as seguintes linhas de código.

```Python
from azureml.logging import get_azureml_logger
run_logger = get_azureml_logger()
```

Depois de criado, é possível invocar o **log** método com qualquer nome/valor par.

Quando o desenvolvimento é concluído, muitas vezes é útil para parametrizar os scripts, de modo a que os valores podem ser transmitidos através da linha de comandos.
O exemplo abaixo mostra como aceitam parâmetros da linha de comandos (quando presente) usando bibliotecas de Python padrão.
Este script usa um único parâmetro para a taxa de regularização (*reg*) usado de acordo com um modelo de classificação num esforço para aumentar *precisão* sem overfitting.
Estas variáveis, em seguida, são registadas como *taxa de regularização* e *precisão* para que o modelo com melhores resultados pode ser facilmente identificado.

```Python
# change regularization rate and you will likely get a different accuracy.
reg = 0.01
# load regularization rate from argument if present
if len(sys.argv) > 1:
    reg = float(sys.argv[1])

print("Regularization rate is {}".format(reg))

# log the regularization rate
run_logger.log("Regularization Rate", reg)

# train a logistic regression model on the training set
clf1 = LogisticRegression(C=1/reg).fit(X_train, Y_train)
print (clf1)

# evaluate the test set
accuracy = clf1.score(X_test, Y_test)
print ("Accuracy is {}".format(accuracy))

# log accuracy
run_logger.log("Accuracy", accuracy)
```

Seguir estes passos nos scripts de ativá-las tornar a utilização ideal **histórico de execuções**.

## <a name="launch-runs-from-project-dashboard"></a>Lançamento é executado a partir do Dashboard do projeto
Retornar para o **Dashboard do projeto**, pode iniciar um **controlada da execução** ao selecionar o **iris_sklearn. PY** script e introduzir a **taxa de regularização**  parâmetro na **argumentos** caixa de edição.

![introdução de parâmetros e iniciando execuções](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-05.png)

Uma vez que iniciar execuções controladas não bloqueia o Azure ML Workbench, vários podem ser iniciados em paralelo.
O estado de cada execução controlada está visível no **painel de tarefas** conforme mostrado.

![controlo é executado no painel de tarefas](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-06.png)

Isto permite que uma melhor utilização dos recursos, sem a necessidade de cada tarefa para ser executada em série.

## <a name="view-results-in-run-history"></a>Ver resultados no histórico de execuções
Progresso e os resultados das execuções controladas estão disponíveis para análise no Azure ML Workbench **histórico de execuções**.
**Histórico de execuções** fornece três exibições distintas:
- Dashboard
- Detalhes
- Comparação

O **Dashboard** vista apresenta dados em todas as execuções de um determinado script, compostas em formulários de gráficos tanto em tabela.
O **detalhes** vista apresenta todos os dados gerados a partir de uma execução específica de um script específico, incluindo métricas com sessão iniciada e os ficheiros de saída (tais como processado desenha.) O **comparação** vista permite que os resultados de duas ou três execuções para ser visualizada lado a lado, também a incluir as métricas registadas e ficheiros de saída.

Em oito controlados execuções do **iris_sklearn. PY**, os valores para o **taxa de regularização** parâmetro e **precisão** resultado foram registados para ilustrar como usar a execução Vistas de histórico.

### <a name="run-history-dashboard"></a>Dashboard do histórico de execuções
Os resultados de todas as execuções de oito são visíveis no **Dashboard do histórico de execução**.
Como **iris_sklearn. PY** registos *taxa de regularização* e *precisão*, o **Dashboard do histórico de execução** apresenta gráficos para esses valores por padrão.

![dashboard do histórico de execuções](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-07.png)

O **Dashboard do histórico de execução** podem ser personalizados para que os valores com sessão iniciada também aparecem na grade.  Ao clicar o **personalizar** ícone é apresentado o **personalização de vista de lista** diálogo conforme mostrado.

![personalizando a grade de dashboard do histórico de execuções](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-08.png)

Quaisquer valores que tem sessão iniciadas durante execuções controladas estão disponíveis para exibição e selecionar **taxa de regularização** e **precisão** adiciona-os à grade.

![valores com sessão iniciada na grelha personalizada](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-09.png)

É fácil encontrar execuções interessantes ao pairar o rato sobre os pontos nos gráficos.  Neste caso, executar 7 gerou uma precisão de boa juntamente com uma duração de baixa.

![encontrar um interessante executar](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-10.png)

Clicar num ponto associado 7 execute em qualquer gráfico ou na ligação para executar 7 em telas de grade a **detalhes do histórico de execução**.

### <a name="run-history-details"></a>Detalhes do histórico de execução
A partir desta vista, os resultados completos do 7 executar, juntamente com quaisquer artefactos produzidos por executar 7 são apresentados.

![Detalhes do histórico de execução](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-11.png)

O **detalhes da execução da histórico** vista também fornece a capacidade de **transferir** todos os ficheiros escritos para o **. / produz** pasta (esses arquivos são apoiados pelo Azure ML Workbench armazenamento na cloud para o histórico de execuções, que é o assunto do outro artigo.)

Por fim, **detalhes do histórico de execução** fornece um meio para restaurar o seu projeto de seu estado no momento desta execução.
Ao clicar o **restaurar** botão exibe uma caixa de diálogo de confirmação, conforme mostrado.

![Confirmar restauro executar](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-13.png)

Se confirmar, os ficheiros poderão ser substituídos ou então removido, a utilizar esta funcionalidade com cuidado.

### <a name="run-history-comparison"></a>Executar a comparação de histórico
Selecionar dois ou três execuções **Dashboard histórico da execução** e clicando em **comparar** traz até o **comparação do histórico de execução** vista.
Em alternativa, clicando em **Compare** e selecionando uma execução dentro a **detalhes do histórico de execução** vista também traz até o **comparação do histórico de execução** vista.
Em ambos os casos, o **comparação de histórico de execução** vista fornece um meio para ver os resultados com sessão iniciada e artefatos das duas ou três execuções lado a lado.

![executar a comparação de histórico](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-12.png)

Esta vista é especialmente útil para a comparação de gráficos, mas em geral, todas as propriedades das execuções possam ser comparadas aqui.

### <a name="command-line-interface"></a>Interface de Linha de Comandos
O Azure Machine Learning Workbench fornece também acesso ao histórico de execuções por meio de seus **Interface de linha de comandos**.
Para aceder a **Interface de linha de comandos**, clique no **linha de comandos aberta** menu, conforme mostrado.

![Abra a linha de comandos](media/how-to-use-run-history-model-metrics/how-to-use-run-history-model-metrics-14.png)

Os comandos disponíveis para o histórico de execuções são acedidos através de `az ml history`, com a ajuda online disponíveis, adicionando o `-h` sinalizador.
```
$ az ml history -h

Group
    az ml history: View run history of machine learning experiments.

Commands:
    compare : Compare two runs.
    download: Download all the artifacts from a run into the destination path.
    info    : Details about one run.
    last    : Get detail about most recent run.
    list    : List runs.
    promote : Promote Artifacts.
```
Estes comandos fornecem as mesmas funcionalidades e devolver os mesmos dados mostrados a **vistas de histórico de execução**.
Por exemplo, os resultados da última execução podem ser apresentados como um objeto JSON.
```
$ az ml history last
{
  "Accuracy": 0.6792452830188679,
  "Regularization Rate": 0.078125,
  "attachments": "control_log, control_log.txt, driver_log, driver_log.txt, pid.txt, dataprep/backgroundProcess.log, dataprep/backgroundProcess_Engine.log, dataprep/backgroundProcess_EngineHost.log, dataprep/backgroundProcess_ProjectProvider.log, dataprep/backgroundProcess_Sampling.log, dataprep/backgroundProcess_Telemetry.log, outputs/cm.png, outputs/model.pkl, outputs/sdk_debug.txt, outputs/roc.png",
  "created_utc": "2017-09-08T00:58:01.611105+00:00",
  "description": null,
  "duration": "0:00:04.892389",
  "end_time_utc": "2017-09-08T00:58:07.951403+00:00",
  "experiment_id": "ce92d0a9-3e2c-4d51-85de-93ef22249ce1",
  "heartbeat_enabled": true,
  "hidden": false,
  "name": null,
  "parent_run_id": null,
  "properties": {
    "CommitId": "e77a5f0df2af1a482bbe39b70bfbb16b62228cb3"
  },
  "run_id": "IrisDemo_1504832281116",
  "run_number": 8,
  "script_name": "iris_sklearn.py",
  "start_time_utc": "2017-09-08T00:58:03.059014+00:00",
  "status": "Completed",
  "target": "local",
  "user_id": "e9fafe06-b0e4-4154-8374-aae34f9977b2"
}
```
Além disso, pode ser apresentada a lista de todas as execuções num formato tabular.
```
$ az ml history list -o table
  Accuracy    Regularization Rate  Duration        Run_id                  Script_name      Start_time_utc                    Status
----------  ---------------------  --------------  ----------------------  ---------------  --------------------------------  ---------
  0.679245               0.078125  0:00:04.892389  IrisDemo_1504832281116  iris_sklearn.py  2017-09-08T00:58:03.059014+00:00  Completed
  0.679245               0.15625   0:00:04.734956  IrisDemo_1504832255198  iris_sklearn.py  2017-09-08T00:57:38.507196+00:00  Completed
  0.660377               0.3125    0:00:04.913762  IrisDemo_1504832234904  iris_sklearn.py  2017-09-08T00:57:16.849881+00:00  Completed
  0.660377               0.625     0:00:06.107764  IrisDemo_1504832210767  iris_sklearn.py  2017-09-08T00:56:54.602813+00:00  Completed
  0.641509               1.25      0:00:04.742727  IrisDemo_1504832171373  iris_sklearn.py  2017-09-08T00:56:13.879280+00:00  Completed
  0.660377               2.5       0:00:04.915401  IrisDemo_1504832148526  iris_sklearn.py  2017-09-08T00:55:52.937083+00:00  Completed
  0.641509               5         0:00:04.730627  IrisDemo_1504832127172  iris_sklearn.py  2017-09-08T00:55:29.612382+00:00  Completed
  0.641509              10         0:00:06.059082  IrisDemo_1504832109906  iris_sklearn.py  2017-09-08T00:55:14.739806+00:00  Completed

```
O **Interface de linha de comandos** é um caminho alternativo para acessar o poder do Azure Machine Learning Workbench.

## <a name="next-steps"></a>Próximos Passos
Esses recursos estão disponíveis para ajudá-lo com o processo de experimentação de ciência de dados.
Esperamos que encontrá-los para serem úteis e gostaria muito de receber seus comentários.
Esta é apenas a nossa implementação inicial, e temos uma grande quantidade de aprimoramentos planejados.
Estamos ansiosos por continuamente fornecê-los para o Azure Machine Learning Workbench. 
