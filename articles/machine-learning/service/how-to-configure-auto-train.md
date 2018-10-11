---
title: Configurar seu automatizada experimentação do machine learning - Azure Machine Learning
description: Aprendizagem automática escolhe um algoritmo para e gera um modelo pronto para implantação. Saiba as opções que pode utilizar para configurar automatizada experimentações de machine learning.
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 33126c094a55bc57edd49a54fbc4f5acd7401998
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079009"
---
# <a name="configure-your-automated-machine-learning-experiment"></a>Configurar seu automatizada experimentação do machine learning

Aprendizagem automática (ML automatizada) escolhe um algoritmo e hiperparâmetros para e gera um modelo pronto para implantação. O modelo pode ser transferido para ser personalizadas também. Existem várias opções que pode utilizar para configurar experiências de ML automatizada. Neste guia, aprenderá a configurar várias definições de configuração.

Para ver exemplos de um ML automatizada, consulte [Tutorial: preparar um modelo de classificação automatizada de machine Learning](tutorial-auto-train-models.md) ou [formar modelos com aprendizagem automática na cloud](how-to-auto-train-remote.md).

Opções de configuração disponíveis no automatizada de machine learning:

* Selecione o tipo de experimentação, por exemplo, classificação, regressão 
* Origem de dados, formatos e obtenção de dados
* Escolher o destino de computação (local ou remoto)
* Automatizada as definições de experimentação do ML
* Executar uma experimentação do ML automatizada
* Explorar métricas de modelo
* Registe e implemente o modelo

## <a name="select-your-experiment-type"></a>Selecione o tipo de experimentação
Antes de começar a sua experimentação, deve determinar o tipo de problema de aprendizado de máquina que está a resolver. ML automatizada suporta duas categorias de aprendizagem supervisionada: classificação e regressão. Automatizado ML oferece suporte aos seguintes algoritmos durante o processo de Otimização e automatização. Como um utilizador, não é necessário para especificar o algoritmo.
Classificação | Regressão
--|--
sklearn.linear_model. LogisticRegression | sklearn.linear_model. ElasticNet
sklearn.linear_model. SGDClassifier  | sklearn.ensemble.GradientBoostingRegressor
sklearn.naive_bayes. BernoulliNB | sklearn.tree.DecisionTreeRegressor
sklearn.naive_bayes. MultinomialNB | sklearn.neighbors.KNeighborsRegressor
sklearn.svm.SVC | sklearn.linear_model. LassoLars
sklearn.svm.LinearSVC | sklearn.linear_model. SGDRegressor
sklearn.calibration.CalibratedClassifierCV |    sklearn.ensemble.RandomForestRegressor
sklearn.neighbors.KNeighborsClassifier |    sklearn.ensemble.ExtraTreesRegressor
sklearn.tree.DecisionTreeClassifier |   lightgbm. LGBMRegressor
sklearn.ensemble.RandomForestClassifier |
sklearn.ensemble.ExtraTreesClassifier   |
sklearn.ensemble.GradientBoostingClassifier |
lightgbm. LGBMClassifier |


## <a name="data-source-and-format"></a>Origem de dados e de formato
ML automatizada suporta os dados que residem no ambiente de trabalho local ou na cloud no armazenamento de Blobs do Azure. Os dados podem ser lidos em scikit-saiba formatos de dados suportados. É possível ler os dados em matrizes de 1) Numpy X (recursos) e y (variável de destino ou também conhecido como etiqueta) ou 2) Pandas dataframe. 

Exemplos:

1.  Matrizes de Numpy

    ```python
    digits = datasets.load_digits()
    X_digits = digits.data 
    y_digits = digits.target
    ```

2.  Pandas dataframe

    ```python
    Import pandas as pd
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"') 
    # get integer labels 
    le = LabelEncoder() 
    le.fit(df["Label"].values) 
    y = le.transform(df["Label"].values) 
    df = df.drop(["Label"], axis=1) 
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Obter dados para executar a experimentação em computação remota

Se estiver a utilizar uma computação remota para executar a sua experimentação, a obtenção de dados deverá ser colada num script de python separado `get_data()`. Este script é executado na computação remota onde a experimentação do ML automatizada é executada. `get_data` Elimina a necessidade de obter os dados durante a transmissão para cada iteração. Sem `get_data`, sua experimentação irão falhar quando é executado no cálculo remoto.

Eis um exemplo de `get_data`:

```python
%%writefile $project_folder/get_data.py 
import pandas as pd 
from sklearn.model_selection 
import train_test_split 
from sklearn.preprocessing import LabelEncoder 
def get_data(): # Burning man 2016 data 
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"') 
    # get integer labels 
    le = LabelEncoder() 
    le.fit(df["Label"].values) 
    y = le.transform(df["Label"].values) 
    df = df.drop(["Label"], axis=1) 
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42) 
    return { "X" : df, "y" : y }
```

No seu `AutoMLConfig` objeto, especifique o `data_script` parâmetro e forneça o caminho para o `get_data` ficheiro de script semelhante à abaixo:

```python
automl_config = AutoMLConfig(****, data_script=project_folder + "/get_data.py", **** )
```

`get_data` script pode retornar o seguinte:
Chave | Tipo |    Mutuamente exclusivo com | Descrição
---|---|---|---
X | Pandas Dataframe ou matriz de Numpy | data_train, etiqueta, colunas |  Todas as funcionalidades para treinar com
Y | Pandas Dataframe ou matriz de Numpy |   label   | Etiqueta de dados para preparar com. Para classificação, devem ser uma matriz de inteiros.
X_valid | Pandas Dataframe ou matriz de Numpy   | data_train, etiqueta | _Opcional_ todas as funcionalidades para validar com. Se não for especificado, X é dividida entre train e validar
y_valid |   Pandas Dataframe ou matriz de Numpy | data_train, etiqueta | _Opcional_ os dados da etiqueta para validar com. Se não for especificado, y é dividida entre train e validar
sample_weight | Pandas Dataframe ou matriz de Numpy |   data_train, etiqueta, colunas| _Opcional_ um valor de peso para cada exemplo. Utilize quando quiser atribuir diferentes pesos para os seus pontos de dados 
sample_weight_valid | Pandas Dataframe ou matriz de Numpy | data_train, etiqueta, colunas |    _Opcional_ um valor de peso para cada exemplo de validação. Se não for especificado, sample_weight é dividida entre train e validar
data_train |    Pandas Dataframe |  X, y, X_valid, y_valid |    Todos os dados (funcionalidades + etiqueta) para treinar com
label | cadeia  | X, y, X_valid, y_valid |  A coluna data_train representa a etiqueta
Colunas | matriz de cadeias de caracteres  ||  _Opcional_ lista branca de colunas a utilizar para funcionalidades
cv_splits_indices   | Matriz de números inteiros ||  _Opcional_ lista de índices para dividir os dados para cruzada validação

## <a name="train-and-validation-data"></a>Educar e a validação de dados

Pode especificar train separado e validação definida por meio de get_data() ou diretamente no `AutoMLConfig` método.

## <a name="cross-validation-split-options"></a>Opções de divisão de validação cruzada

### <a name="k-folds-cross-validation"></a>K subconjuntos de validação cruzada

Utilize `n_cross_validations` definição para especificar o número de cruzada validações. O conjunto de dados de treinamento será dividido aleatoriamente em `n_cross_validations` subconjuntos de tamanho igual. Durante cada validação cruzada redondo, um dos subconjuntos será utilizado para a validação do modelo com base em com os subconjuntos restantes. Este processo repete-se para `n_cross_validations` Arredonda por excesso até que cada subconjuntos de validação é utilizado uma vez como conjunto de validação. Por fim, a média pontua em todos os `n_cross_validations` rodadas serão reportadas e o modelo correspondente será reestruturar os em todo o conjunto de dados de preparação.

### <a name="monte-carlo-cross-validation-aka-repeated-random-sub-sampling"></a>Monte Carlo (também conhecido como validação cruzada Amostragem de secundárias aleatória repetida)

Utilizar `validation_size` para especificar a percentagem do conjunto de dados de treinamento que deve ser utilizado para validação e use `n_cross_validations` para especificar o número de cruzada validações. Durante cada uma validação round, um subconjunto de tamanho de cruzada `validation_size` serão selecionados aleatoriamente para a validação do modelo com base em com os dados restantes. Por fim, a média pontua em todos os `n_cross_validations` rodadas serão reportadas e o modelo correspondente será reestruturar os em todo o conjunto de dados de preparação.

### <a name="custom-validation-dataset"></a>Conjunto de dados de validação personalizada

Utilize o conjunto de dados de validação personalizada se dividir aleatório não é aceitável (normalmente de tempo os dados de série ou desequilibrado). Com isso, pode especificar o seu próprio conjunto de dados de validação. O modelo será avaliado contra o conjunto de dados de validação especificado, em vez de um conjunto de dados aleatório.

## <a name="compute-to-run-experiment"></a>Computação para executar a experimentação

Em seguida, determine onde será preparado o modelo. Uma experimentação de preparação de ML automatizada é executado num destino de computação que o proprietário e gerir. 

Opções de computação suportadas são:
1.  Seu computador local, como uma área de trabalho local ou computador portátil – em geral, quando tem pequeno conjunto de dados e estiver na fase de exploração.
2.  Um computador remoto na cloud – [máquina de Virtual de ciência de dados do Azure](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) que executem o Linux – tem um conjunto de dados grandes e quer aumentar verticalmente para uma máquina de grandes dimensões que está disponível na Cloud do Azure. 
3.  Cluster do Azure Batch AI – A geridos cluster que pode configurar para aumentar horizontalmente e executar iterações de ML automatizada em paralelo. 


## <a name="configure-your-experiment-settings"></a>Configurar as definições de experimentação

Existem vários botões que pode utilizar para configurar a sua experimentação do ML automatizada. Esses parâmetros são definidos pela Instanciação de um `AutoMLConfig` objeto.

Alguns exemplos incluem:

1.  Experimentação de classificação usando AUC ponderada como a métrica primária com tempo máximo de 12 000 segundos por iteração, com a experimentação para terminar depois de 50 iterações e 2 subconjuntos de validação cruzada.

    ```python
    automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        max_time_sec=12000,
        iterations=50,
        X=X, 
        y=y,
        n_cross_validations=2)
    ```
2.  Segue-se um exemplo de um conjunto de experimentação de regressão para terminar após 100 iterações, com cada iteração duração até 600 segundos, com 5 validação cruzada subconjuntos.

    ````python
    automl_regressor = AutoMLConfig(
        task='regression',
        max_time_sec=600,
        iterations=100,
        primary_metric='r2_score',
        X=X, 
        y=y,
        n_cross_validations=5)
    ````

Esta tabela lista das definições dos parâmetros disponíveis para a sua experimentação e seus valores padrão.

Propriedade |  Descrição | Valor Predefinido
--|--|--
`task`  |Especifique o tipo de problema do machine learning. Valores permitidos são <li>classification</li><li>Regressão</li>    | Nenhuma |
`primary_metric` |Métrica que pretende otimizar na criação de seu modelo. Por exemplo, se especificar a precisão como a primary_metric, ML automatizada procura para localizar um modelo com precisão máxima. Só pode especificar um primary_metric por experimentação. Valores permitidos são <br/>**Classificação**:<br/><li> accuracy  </li><li> AUC_weighted</li><li> precision_score_weighted </li><li> balanced_accuracy </li><li> average_precision_score_weighted </li><br/>**Regressão**: <br/><li> normalized_mean_absolute_error </li><li> spearman_correlation </li><li> normalized_root_mean_squared_error </li><li> normalized_root_mean_squared_log_error</li><li> R2_score    </li> | Para classificação: precisão <br/>Para regressão: spearman_correlation <br/> |
`exit_score` |  Pode definir um valor de destino para sua primary_metric. Depois de um modelo for encontrado que cumpra o destino de primary_metric, ML automatizada irá parar de iteração e encerra a experimentação. Se este valor não for definido (predefinição), experimentação do ML automatizada irá continuar a executar o número de iterações especificado em iterações. Cria um valor duplo. Se nunca alcance o destino, em seguida, ML automatizada continuará até atingir o número de iterações especificado em iterações.|   Nenhuma
`iterations` |Número máximo de iterações. Cada iteração é igual a uma tarefa de preparação que resulta num pipeline. Os pipelines são o pré-processamento de dados e o modelo. Para obter uma utilização do modelo de alta qualidade 250 ou mais | 100
`Concurrent_iterations`|    Número máximo de iterações para ser executadas em paralelo. Esta definição funciona apenas para computação remota.|    1
`max_cores_per_iteration`   | Indica o número de núcleos no destino de computação seriam usados para preparar um único pipeline. Se o algoritmo pode tirar partido de vários núcleos, isso aumenta o desempenho numa máquina com vários núcleo. Pode configurá-lo como -1 para utilizar todos os núcleos disponíveis na máquina.|  1
`max_time_sec` |    Limita a quantidade de tempo (segundos) demora de uma iteração específica. Se uma iteração exceder o período especificado, obtém cancelada iteração. Se não for definida, em seguida, a iteração continua a ser executado até ser concluído. |   Nenhuma
`n_cross_validations`   |Número de divisões de validação cruzada| Nenhuma
`validation_size`   |Tamanho da validação definida como percentagem de todos os exemplo de treinamento.|  Nenhuma
`preprocess` | Verdadeiro/Falso <br/>Experimente verdadeiro ativa para realizar o processamento prévio de entrada. Segue-se um subconjunto de pré-processamento<li>Dados em falta: Imputes falta dados numéricos com média, texto com a maioria dos occurance </li><li>Valores categóricos: Se for de tipo de dados numéricos e de número exclusivo de valores é menos de 5 por cento, converte num-hot codificação </li><li>Etc. para verificação da lista completa [o repositório do GitHub](https://aka.ms/aml-notebooks)</li><br/>Nota: se os dados estão dispersos não é possível utilizar pré-processar = true |  Falso | 
`blacklist_algos`   | Experimentação do ML automatizada tem muitos algoritmos diferentes que tentar. Configure ML automatizada para excluir determinados algoritmos da experimentação. Útil se estiver ciente de que algorithm(s) não funcionam bem para o conjunto de dados. Excluir algoritmos pode lhe poupar recursos de computação e o tempo de treinamento.<br/>Valores permitidos para classificação<br/><li>Regressão logística</li><li>Classificador SGD</li><li>MultinomialNB</li><li>BernoulliNB</li><li>SVM</li><li>LinearSVM</li><li>kNN</li><li>DT</li><li>RF</li><li>árvores Extras</li><li>aumentam a gradação</li><li>lgbm_classifier</li><br/>Valores permitidos para regressão<br/><li>Elástico net</li><li>Gradação regressor de foi adaptativo</li><li>Regressor foi de DT</li><li>regressor foi de kNN</li><li>Lasso lars</li><li>Regressor foi SGD</li><li>Regressor foi de RF</li><li>regressor foi de árvores extra</li>|   Nenhuma
`verbosity` |Controla o nível de registo com informações de que está a ser o mais verboso e crítico a ser menos.<br/>Valores permitidos são:<br/><li>logging.INFO</li><li>o registo. AVISO</li><li>o registo. ERRO</li><li>o registo. CRÍTICO</li>  | logging.INFO</li> 
`X` | Todas as funcionalidades para treinar com |  Nenhuma
`y` |   Etiqueta de dados para preparar com. Para classificação, devem ser uma matriz de inteiros.|  Nenhuma
`X_valid`|_Opcional_ todas as funcionalidades para validar com. Se não for especificado, X é dividida entre train e validar |   Nenhuma
`y_valid`   |_Opcional_ os dados da etiqueta para validar com. Se não for especificado, y é dividida entre train e validar    | Nenhuma
`sample_weight` |   _Opcional_ um valor de peso para cada exemplo. Utilize quando quiser atribuir diferentes pesos para os seus pontos de dados |   Nenhuma
`sample_weight_valid`   |   _Opcional_ um valor de peso para cada exemplo de validação. Se não for especificado, sample_weight é dividida entre train e validar   | Nenhuma
`run_configuration` |   Objeto de RunConfiguration.  Utilizada para execuções de remotas. |Nenhuma
`data_script`  |    Caminho para um ficheiro que contém o método get_data.  É necessário para execução remota.   |Nenhuma


## <a name="run-experiment"></a>Execute experimentação

Em seguida, podemos pode iniciar a experimentação para executar e gerar um modelo para nós. Passar o `AutoMLConfig` para o `submit` método para gerar o modelo.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>As dependências são instaladas pela primeira vez numa DSVM do novo.  Pode demorar até 10 minutos antes de saída é mostrada.
>Definição `show_output` como verdadeira resulta na saída a ser apresentado na consola.


## <a name="explore-model-metrics"></a>Explorar métricas de modelo
Pode ver os resultados num widget ou inline, se estiver num bloco de notas. Consulte os detalhes para "Controlar e avaliar modelos". (Certifique-se de conteúdo AML contém informações relevantes para ML automatizado)

As métricas seguintes são salvas em cada iteração
* AUC_macro
* AUC_micro
* AUC_weighted
* accuracy
* average_precision_score_macro
* average_precision_score_micro
* average_precision_score_weighted
* balanced_accuracy
* f1_score_macro
* f1_score_micro
* f1_score_weighted
* log_loss
* norm_macro_recall
* precision_score_macro
* precision_score_micro
* precision_score_weighted
* recall_score_macro
* recall_score_micro
* recall_score_weighted
* weighted_accuracy

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre [como e onde implementar um modelo](how-to-deploy-and-where.md).

Saiba mais sobre [como preparar um modelo de classificação com ML automatizada](tutorial-auto-train-models.md) ou [como dar formação através de ML de automatizada num recurso remoto](how-to-auto-train-remote.md). 