---
title: Série de tempo de procura de energia de previsão | Documentos da Microsoft
description: Como aplicar o machine learning para previsão de série temporal de procura de energia no Azure Machine Learning Workbench.
services: machine-learning
documentationcenter: ''
author: anta
manager: ireiter
editor: anta
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: anta
ROBOTS: NOINDEX
ms.openlocfilehash: 934d37783165c2e57dcabd0ff764747e1ab4b65e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946869"
---
# <a name="energy-demand-time-series-forecasting"></a>Série de tempo de procura de energia de previsão

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 




Previsão de séries de tempo é a tarefa de prever futuros valores numa sequência cronológica de observações. Ele é um problema comum e possui aplicativos em várias indústrias. Por exemplo, as empresas de varejo tem de realizar a previsão de vendas de produtos futuros para que elas com eficiência podem organizar suas cadeias de fornecimento para satisfazer a procura. Da mesma forma, as empresas de entrega de pacote precisam de estimar a demanda por seus serviços, portanto, também pode planear requisitos de força de trabalho e rotas de distribuição antes do tempo. Em muitos casos, os riscos financeiros de previsões incorretas podem ser significativos. Portanto, a previsão, muitas vezes, é uma atividade crítica de negócios.

Este exemplo mostra como a previsão de séries de tempo pode ser executada por meio de aplicação de técnicas de aprendizado de máquina. É guiado pelos cada etapa da modelagem de processo incluindo:
- Preparação de dados para limpar e formatar os dados;
- Criar recursos para os modelos de machine learning a partir de dados de séries de tempo não processados;
- Treinamento de vários modelos de aprendizagem automática;
- Os modelos de avaliação ao comparar seu desempenho num conjunto de dados de teste mantidos-out; e,
- Operacionalizar o melhor modelo, tornando-a disponível através de um serviço web para gerar as previsões a pedido.

O Azure Machine Learning Workbench auxilia o processo de modelagem em cada etapa: 
- O exemplo mostra como o ambiente de bloco de notas do Jupyter - disponível diretamente a partir por meio da bancada de trabalho - pode tornar o código do Python é mais fácil de desenvolver. O processo de desenvolvimento do modelo pode ser explicado aos outros utilizadores mais claramente anotações de markdown e gráficos. Estes blocos de notas podem ser visualizados, editados e executados diretamente do Workbench.
- Modelos de formação podem ser persistente e carregados para o armazenamento de Blobs. Isto ajuda a cientista de dados para manter o controle de objetos de modelo preparado e certifique-se de que são mantidos e quando necessário.
- As métricas podem ser registadas ao executar um script de Python, permitindo que os cientistas de dados e manter um registro das pontuações de desempenho do modelo.
- A Bancada de trabalho produz tabelas personalizáveis de métricas com sessão iniciada, permitindo que o cientista de dados para comparar facilmente as métricas de desempenho do modelo. Os gráficos são apresentados automaticamente para que o melhor desempenho de modelo pode ser identificado prontamente.
- Por fim, o exemplo mostra como pode ser operacionalizou um modelo preparado, implementando-os num serviço da web em tempo real.

## <a name="link-to-the-gallery-github-repository"></a>Ligação para o repositório do GitHub de galeria
O repositório do GitHub público para este cenário do mundo real contém todos os materiais, incluindo exemplos de código, necessários para este exemplo:

[https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting](https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting)


## <a name="use-case-overview"></a>Descrição geral de caso de utilização

Este cenário se concentra em que o objetivo é prever a carga futura numa rede energética de previsão de procura de energia. É uma operação comercial crítico para empresas no setor energético como operadores tem de manter o equilíbrio certo entre a energia consumida numa grade e a energia fornecida ao mesmo. Oferece muito poder fornecido à grade pode resultar em desperdício de energia ou falhas técnicas. No entanto, se for fornecida muito pouca energia pode levar a blackouts, deixando os clientes sem energia. Normalmente, operadores de grade podem levar a decisões de curto prazo para gerir o abastecimento de energia para a grade e manter a carga em saldo. Uma previsão preciso de curta duração de procura de energia, portanto, é essencial para o operador tomar essas decisões com confiança.

Este cenário mostra em detalhe a construção de uma solução de previsão de procura de energia de aprendizagem automática. A solução é preparada num conjunto de dados público a [Nova Iorque independentes sistema operador (NYISO)](http://www3.dps.ny.gov/W/PSCWeb.nsf/All/298372E2CE4764E885257687006F39DF?OpenDocument), que opera a grade de energia para o estado de nova York. O conjunto de dados por hora inclui dados de procura de energia para a cidade de nova York durante um período de cinco anos. Um conjunto de dados adicional que contém uma vez por hora condições meteorológicas na cidade de nova York durante o mesmo período de tempo em que foi feito [darksky.net](https://darksky.net).

## <a name="prerequisites"></a>Pré-requisitos

- Uma [conta do Azure](https://azure.microsoft.com/free/) (avaliações gratuitas estão disponíveis).
- Uma cópia instalada do [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) seguintes a [guia de início rápido de instalação](quickstart-installation.md) para instalar o programa e criar uma área de trabalho.
- Este exemplo pressupõe que está a executar o Azure ML Workbench no Windows 10 com [motor do Docker](https://www.docker.com/) instalado localmente. Se estiver a utilizar o macOS, as instruções são praticamente os mesmos.
- Operacionalização de Aprendizado de máquina do Azure instalado com um ambiente de implantação local, configurar e uma conta de gestão de modelos criados conforme descrito neste [guia](./model-management-configuration.md).
- Este exemplo requer que Atualize a instalação de Pandas para versão 0.20.3 ou superior e instalar matplotlib. Clique em *linha de comandos aberta* partir do *ficheiro* menu na bancada de trabalho e execute os seguintes comandos para instalar estas dependências:

    ```
    conda install "pandas>=0.21.1"
    ```
    
## <a name="create-a-new-workbench-project"></a>Criar um novo projeto do Workbench

Crie um novo projeto com este exemplo como um modelo:
1.  Abrir o Azure Machine Learning Workbench
2.  Sobre o **projetos** página, clique no **+** iniciar sessão e selecionar **novo projeto**
3.  Na **criar novo projeto** painel, preencha as informações para o novo projeto
4.  Na **modelos de projeto de pesquisa** caixa de pesquisa, escreva "Energia tempo série de previsão de procura" e selecione o modelo
5.  Clique em **Criar**.


## <a name="data-description"></a>Descrição de dados

Dois conjuntos de dados são fornecidos com este exemplo e são transferidos utilizando o `1-data-preparation.ipynb` bloco de notas: `nyc_demand.csv` e `nyc_weather.csv`.

**nyc_demand.csv** à hora contém valores de pedido de energia para a cidade de nova York anos 2012 2017. Os dados têm a seguinte estrutura simple:

| timeStamp | pedido |
| --- | --- |
| 2012-01-01 00:00:00 | 4937.5 |
| 2012-01-01 01:00:00 | 4752.1 |
| 2012-01-01 02:00:00 | 4542.6 |
| 2012-01-01 03:00:00 | 4357.7 |

Os valores de pedido são no megawatts-hora (MWh). Segue-se um gráfico de procura de energia num período de 7 dias, em Julho de 2017:

![Procura energética](./media/scenario-time-series-forecasting/energy_demand.png  "procura energética")

**nyc_weather.csv** contém valores de tempo para a cidade de nova York de hora a hora ao longo dos anos 2012 2017:

| timeStamp | precip | Temp
| --- | --- | --- |
| 2012-01-01 00:00:00 | 0.0 | 46.13 |
| 2012-01-01 01:00:00 | 0.01 | 45.89 |
| 2012-01-01 02:00:00 | 0.05 | 45.04 |
| 2012-01-01 03:00:00 | 0.02 | 45.03 |

*precip* é uma medida percentual do nível de precipitação. *Temp* valores (temperatura) tem sido redimensionados, de modo a que todos os valores que se encontra no intervalo [0, 100].

## <a name="scenario-structure"></a>Estrutura do cenário

Quando abrir este projeto pela primeira vez no Azure Machine Learning Workbench, navegue para o *ficheiros* painel no lado esquerdo. Os arquivos de código seguintes são fornecidos com este exemplo:
- `1-data-preparation.ipynb` -Este bloco de notas do Jupyter transfere e processa os dados para o preparar para modelagem. Este é o primeiro bloco de notas que irá executar.
- `2-linear-regression.ipynb` -Este bloco de notas prepara um modelo de regressão linear em dados de treinamento.
- `3-ridge.ipynb` -prepara um modelo de regressão ridge.
- `4-ridge-poly2.ipynb` -prepara um modelo de regressão ridge polynomial recursos do nível 2.
- `5-mlp.ipynb` -prepara uma rede neural perceptron multi camada.
- `6-dtree.ipynb` -prepara um modelo de árvore de decisão.
- `7-gbm.ipynb` -prepara um modelo de máquina de elevada gradação.
- `8-evaluate-model.py` -Este script carrega um modelo preparado e utiliza-o para fazer previsões sobre um conjunto de dados de teste mantidos-out. Produz métricas de avaliação do modelo para que o desempenho de modelos diferentes possam ser comparado.
- `9-forecast-output-exploration.ipynb` -Este bloco de notas produz as visualizações das previsões geradas pelos modelos de machine learning.
- `10-deploy-model.ipynb` -Este bloco de notas, demonstra como pode ser operacionalizou um modelo de previsão preparado num serviço da web em tempo real.
- `evaluate-all-models.py` -Este script avalia os modelos de formação tudo. Ele fornece uma alternativa à execução `8-evaluate-model.py` para cada treinados modelar individualmente.

### <a name="1-data-preparation-and-cleaning"></a>1. Preparação de dados e a limpeza

Para iniciar a execução do exemplo, primeiro clique em `1-data-preparation.ipynb` para abrir o bloco de notas no modo de pré-visualização. Clique em ***iniciar o servidor de bloco de notas*** para iniciar um servidor de bloco de notas do Jupyter e o kernel de Python no seu computador. Pode optar por executar os blocos de notas a partir do Workbench ou pode utilizar o seu browser ao navegar até [ http://localhost:8888 ](http://localhost:8888). Tenha em atenção que tem de alterar o kernel *PROJECT_NAME local*. Pode fazê-lo a partir da *Kernel* menu no bloco de notas em *kernel de alteração*. Prima ***shift + Enter*** para executar o código em células individuais do bloco de notas ou clique em *executar todos* no *célula* menu para executar o bloco de notas completo.

O bloco de notas transfere primeiro os dados a partir de um contentor de armazenamento de BLOBs alojado no Azure. Os dados são armazenados, em seguida, no computador no `AZUREML_NATIVE_SHARE_DIRECTORY`. Esta localização é acessível a partir de qualquer blocos de notas ou scripts que executar a partir do Workbench é por isso, um bom local para armazenar dados e modelos de formação.

Assim que tem sido baixados os dados, o bloco de notas limpa os dados, preenchendo as lacunas na série de tempo e preencher valores em falta por interpolação. Limpar os dados de séries de tempo dessa forma, maximiza a quantidade de dados disponíveis para os modelos de formação.

Vários recursos de modelo são criados a partir de dados não processados limpos. Estas funcionalidades podem ser categorizadas em dois grupos:

- **Controlado por funcionalidades de tempo** derivam da *timestamp* variável por exemplo, *mês*, *dayofweek* e *hora*.
- **Paras funcionalidades** são valores de tempo mudou de valores a pedido ou temperatura reais. Esses recursos têm como objetivo para capturar as dependências condicionais entre os períodos de tempo consecutivos no modelo.

O conjunto de dados resultante do recurso, em seguida, pode ser utilizado durante o desenvolvimento de modelos de previsão.

### <a name="2-model-development"></a>2. Desenvolvimento de modelos

Uma abordagem de modelagem primeiro pode ser um modelo de regressão linear simples. O `2-linear-regression.ipynb` bloco de notas mostra como preparar um modelo de previsão de regressão linear de procura energética futuras. Em particular, será preparado o modelo para prever a procura de energia uma hora (*t + 1*) à frente o atual período de tempo (*t*). No entanto, é possível aplicar esta recursivamente de modelo "um passo" em tempo de teste para gerar as previsões para períodos de tempo futuros, *t + n*.

Para preparar um modelo, um pipeline preditivo é criado que envolve três etapas distintas:

- **Uma transformação de dados**: formatos necessários dados de entrada podem variar, dependendo do algoritmo de machine learning. Neste caso, o modelo de regressão linear requer categóricas variáveis para ser uma-hot codificados.
- **A cruzada rotina de validação**:, muitas vezes, um modelo de machine learning terão um ou mais hiperparâmetros que têm de ser ajustada por meio de experimentação. Validação cruzada pode ser usada para encontrar o conjunto ótimo de valores de parâmetros. O modelo é repetidamente treinado e avaliado em diferentes subconjuntos do conjunto de dados. Os melhores parâmetros são aqueles que obter o melhor desempenho de modelo quando média em todos os subconjuntos de validação cruzada. Esse processo é explicado detalhadamente no `2-linear-regression.ipynb`.
- **Preparar o modelo final**: é preparado o modelo no conjunto de dados inteiro, usando o melhor conjunto de hiperparâmetros.

Incluímos uma série de 6 modelos diferentes blocos de notas numerado 2 a 7. Cada bloco de anotações prepara um modelo diferente e armazena-na `AZUREML_NATIVE_SHARE_DIRECTORY` localização. Depois de ter preparado todos os modelos que desenvolveu para este cenário, podemos avaliar e compará-los como decribed na próxima seção.

### <a name="3-model-evaluation-and-comparison"></a>3. Avaliação de modelo e comparação

Podemos usar um modelo preparado para fazer previsões para períodos de tempo futuros. É melhor avaliar esses modelos num conjunto de dados mantidos-out de teste. Ao avaliar os modelos diferentes no mesmo conjunto de dados que não foram visto, podemos bastante comparar seu desempenho e identifique o melhor modelo. Neste cenário, vamos aplicar recursivamente o modelo preparado para prever as diversas etapas de tempo no futuro. Em particular, geramos previsões até seis horas, *t + 6* um passo à frente a hora atual, *t*. Estas predições são avaliadas em relação a procura real observada para cada período de tempo.

Para avaliar um modelo, abra o `8-evaluate-model.py` partir do script a *arquivos* painel na bancada de trabalho. Verifique se *configuração de Run* está definida como **local** e, em seguida, escreva o nome de modelo para o *argumentos* campo. O nome do modelo tem de corresponder exatamente a *nome_modelo* variável definida no início do bloco de notas em que é preparado o modelo. Por exemplo, introduza "linear_regression" para avaliar o modelo de regressão linear preparado. Em alternativa, uma vez que todos os modelos treinados, pode avaliá-los tudo com um comando ao executar `evaluate-all-models.py`. Para executar este script, abra um prompt de comando da bancada de trabalho e execute o seguinte comando:

```
python evaluate-all-models.py
```
O `8-evaluate-model.py` script realiza as seguintes operações:

- Carrega um pipeline de modelo treinado do disco
- Faz predições no conjunto de dados de teste
- Calcule métricas de desempenho do modelo e regista-os
- Guarda o teste de previsões de conjunto de dados para `AZUREML_NATIVE_SHARE_DIRECTORY` para posterior inspeção e análise
- Guarda o pipeline de modelo preparado para o *produz* pasta.

> [!Note]
> A guardar o modelo para o *produz* pasta copia automaticamente o objeto de modelo para a conta de armazenamento de Blobs do Azure associada à conta de experimentação. Isso significa que sempre poderá recuperar o objeto de modelo guardado do blob, mesmo que altere a máquina ou contexto de cálculo.

Navegue para o *histórico de execuções* painel e clique em `8-evaluate-model.py`. Verá gráficos exibindo a várias métricas de desempenho do modelo:

- **-ME**: significa que o erro de previsão. Isso pode ser interpretado como a tendência de média de previsão.
- **MPE**: [significa que a percentagem de erro](https://en.wikipedia.org/wiki/Mean_percentage_error) (eu expressa em percentagem da intimação real)
- **MSE**: [significa erros ao quadrado](https://en.wikipedia.org/wiki/Mean_squared_error)
- **RMSE**: média de raiz ao quadrado erro (a raiz quadrada do MSE)
- **MAPE**: [significa absoluto de percentagem de erro](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
- **sMAPE**: [erro simétrica percentagem absoluto mean](https://en.wikipedia.org/wiki/Symmetric_mean_absolute_percentage_error)
- **MAPE_base**: MAPE de uma linha de base de previsão na qual a predição é igual ao último valor conhecido de pedido.
- **MdRAE**: erro relativo absoluto mediana. Erro de previsão a proporção mediana do erro de previsão para a linha de base. Um valor inferior a 1 significa que a previsão desempenho melhor do que a linha de base.

Acima de todas as métricas, consulte a *t + 1* prever. Além das métricas acima, também verá um conjunto de métricas correspondentes com o *_horizon* sufixo. Esta é a métrica de média de todos os períodos no intervalo de previsão do período *t + 1* ao período *t + 6*.

Se as métricas não são apresentados na área de gráfico, clique no símbolo de engrenagem no canto superior direito. Certifique-se de que as métricas de desempenho do modelo que está interessado são verificadas. As métricas também serão exibidas na tabela abaixo os gráficos. Novamente, esta tabela é personalizável clicando no símbolo de engrenagem. Ordena a tabela por uma métrica de desempenho para identificar o melhor modelo. Se estiver interessado em ver como o desempenho de previsão varia de período *t + 1* ao *t + 6*, clique na entrada para o modelo da tabela. Gráficos exibindo o MAPE, métricas MPE e MdRAE entre o período de previsão são apresentadas sob *visualizações*.

Ao avaliar os modelos de previsão, pode ser muito útil visualizar as previsões de saída. Isto ajuda as cientistas de dados e para determinar se a previsão produzida aparece realista. Também pode ajudar a identificar problemas na previsão, se, por exemplo, a previsão ruim em determinados períodos de tempo. O `9-forecast-output-exploration.ipynb` bloco de notas produzirá as visualizações das previsões geradas para o conjunto de dados de teste.

### <a name="4-deployment"></a>4. Implementação

O melhor modelo pode serem operacionalizado implementando-o como um serviço da web em tempo real. Este serviço web, em seguida, pode ser invocado para gerar as previsões a pedido à medida que novos dados torna-se disponível. Neste cenário, uma previsão nova tem de ser gerados a cada hora para prever a procura de energia na hora subsequente. Para executar essa tarefa, um serviço web pode ser implementado que assume uma matriz de funcionalidades para uma hora de determinado período de tempo como entrada e retorna a procura prevista como saída.

Neste exemplo, um serviço web é implementado para uma máquina com o Windows 10. Certifique-se de que concluiu os passos de pré-requisitos para a implementação local propus desta [guia de introdução](https://github.com/Azure/Machine-Learning-Operationalization/blob/master/documentation/getting-started.md) para a CLI de Operacionalização. Assim que tiver configurado a seu ambiente local e a conta de gestão de modelos, execute o `10-deploy-model.ipynb` bloco de notas para implementar o serviço web.

## <a name="conclusion"></a>Conclusão

Este exemplo demonstra como criar uma série de tempo de ponto a ponto da solução de previsão para fins de previsão de procura energética. Muitos dos princípios explorados neste exemplo podem ser estendidos a outros setores e cenários de previsão.

Este cenário mostra como o Azure Machine Learning Workbench pode ajudar um cientista de dados no desenvolvimento de soluções do mundo real com recursos úteis, como o ambiente de bloco de notas do Jupyter e capacidades de registo de métrica. O exemplo também orienta o leitor na forma como um modelo pode ser operacionalizado e implementado através da CLI do Azure Machine Learning Operacionalização. Depois de implementada, a API de serviço da web permite que os desenvolvedores ou engenheiros de dados integrar o modelo de previsão de um pipeline de dados mais amplo.
