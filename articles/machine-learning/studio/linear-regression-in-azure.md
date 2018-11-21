---
title: Utilizar regressão Linear no Machine Learning | Documentos da Microsoft
description: Uma comparação dos modelos de regressão linear no Excel e no Azure Machine Learning Studio
metakeywords: ''
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 417ae6ab-de4f-4bdd-957a-d96133234656
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: b96143ff587cbf5828f4a1d2d1dc11f5bcc33412
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52261399"
---
# <a name="using-linear-regression-in-azure-machine-learning"></a>Utilizar a regressão linear no Azure Machine Learning
> *Kate Baroni* e *Ben Boatman* é enterprise arquitetos de soluções em Data Insights Center of Excellence do Microsoft. Neste artigo, eles descrevem sua experiência de migrar um pacote de análise de regressão existente para uma solução baseada na cloud com o Azure Machine Learning. 
> 
> 

&nbsp; 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="goal"></a>Objetivo
Nosso projeto de introdução em mente dois objetivos: 

1. Utilize análises preditivas para melhorar a precisão de projeções de receita mensal de nossa organização 
2. Utilizar o Azure Machine Learning para confirmar, otimizar, aumentar a velocidade e o dimensionamento de nossos resultados. 

Como muitos negócios, nossa organização executa uma processo de previsão de receita mensal. Nossa pequena equipa de analistas de negócios foi tarefa com o Azure Machine Learning para suportar o processo e melhorar a precisão da previsão. A equipe passou vários meses, a recolha de dados de várias origens e executar os atributos de dados por meio de identificar os principais atributos relevantes para a previsão de vendas de serviços de análise estatística. A próxima etapa era começar a modelos de regressão estatística de criação de protótipos nos dados no Excel. Em poucas semanas, tínhamos um modelo de regressão do Excel que foi a ultrapassar o campo atual e a previsão de processos de finanças. Isso se tornou o resultado de predição de linha de base. 

Em seguida, tivemos a próxima etapa para mover nossa análise preditiva através do Azure Machine Learning para descobrir como pode melhorar o Machine Learning no desempenho de previsão.

## <a name="achieving-predictive-performance-parity"></a>Obtenção de paridade de desempenho de previsão
Nossa primeira prioridade era alcançar paridade entre modelos de regressão de Machine Learning e o Excel. Tendo em conta os mesmos dados e a divisão mesmo para formação e testar dados, gostaríamos de alcançar a paridade de desempenho de previsão entre o Excel e o Machine Learning. Inicialmente não foi possível. O modelo do Excel superava o modelo de Machine Learning. A falha foi devido à falta de compreensão da definição de base de ferramenta no Machine Learning. Depois de uma sincronização com a equipe de produto do Machine Learning, estamos ganhou uma melhor compreensão da base de configuração necessários para nossos conjuntos de dados e, atingido paridade entre os dois modelos. 

### <a name="create-regression-model-in-excel"></a>Criar modelo de regressão no Excel
Nosso regressão de Excel utilizado o modelo de regressão linear padrão encontrado nas de análise de Excel. 

Calculamos *Mean % absoluto erro* e utilizado como a medida de desempenho para o modelo. Demorou 3 meses para chegar um modelo funcional com o Excel. Nós trouxemos grande parte o aprendizado para a experimentação de Machine Learning Studio, que, por fim, era benéfico em entendendo os requisitos.

### <a name="create-comparable-experiment-in-azure-machine-learning"></a>Criar a experiência comparável no Azure Machine Learning
Seguimos as etapas abaixo para criar nosso experimentação no Machine Learning Studio: 

1. Carregar o conjunto de dados como um ficheiro csv para Machine Learning Studio (ficheiro muito pequeno)
2. Criou uma nova experimentação e utilizou a [Select Columns in Dataset] [ select-columns] módulo para selecionar os mesmos recursos de dados utilizados no Excel 
3. Utilizado o [Split Data] [ split] módulo (com *expressão relativo* modo) para dividir os dados em conjuntos de dados de treinamento mesmo como fiz no Excel 
4. As minhas experiências com o [regressão Linear] [ linear-regression] módulo (apenas opções predefinidas), documentada e, em comparação com os resultados para o nosso modelo de regressão de Excel

### <a name="review-initial-results"></a>Rever os resultados iniciais
Em primeiro lugar, o modelo do Excel claramente superava o modelo de Machine Learning Studio: 

|  | Excel | Studio |
| --- |:---:|:---:|
| Desempenho | | |
| <ul style="list-style-type: none;"><li>Ajustado quadrado de R</li></ul> |0.96 |N/A |
| <ul style="list-style-type: none;"><li>Coeficiente de <br />Determinação</li></ul> |N/A |0.78<br />(precisão baixa) |
| Significa que o erro absoluto |US $9. 5M |US $ 19.4 M |
| Significa que o erro absoluto (%) |6.03% |12.2% |

Quando executamos nosso processo e os resultados pela programadores e cientistas de dados na equipe do Machine Learning, rapidamente fornecidas algumas dicas úteis. 

* Quando utiliza a [regressão Linear] [ linear-regression] módulo no Machine Learning Studio, dois métodos são fornecidos:
  * Online gradiente descendente: Pode ser mais adequado para problemas de maior escala
  * Comum de menos de quadrados: Este é o método a que maioria das pessoas pensam quando elas ouvem a regressão linear. Para conjuntos de dados pequenas, comum de menos de quadrados pode ser uma escolha mais ideal.
* Considere a ajustar o parâmetro de peso de regularização de L2 para melhorar o desempenho. Ele é definido como 0,001 por predefinição, mas para nosso pequeno conjunto de dados, defina-o como 0.005 para melhorar o desempenho. 

### <a name="mystery-solved"></a>Mistério resolvido!
Quando Aplicamos as recomendações, obtivemos o mesmo desempenho de linha de base em Machine Learning Studio como com o Excel: 

|  | Excel | Studio (inicial) | Studio com menos quadrados |
| --- |:---:|:---:|:---:|
| Valor com nome |Actuals (numérico) |Mesmo |Mesmo |
| Aprendiz |Excel-dados > análise -> regressão |Regressão linear. |Regressão linear |
| Opções de aprendiz |N/A |Predefinições |quadrados do menos comuns<br />ERROS DE L2 = 0.005 |
| Conjunto de dados |26 linhas, funcionalidades de 3, 1 etiqueta. Todos os numérica. |Mesmo |Mesmo |
| Split: Train |Excel com base em com as linhas primeiro 18, testadas nas 8 pela última vez linhas. |Mesmo |Mesmo |
| Split: teste |Fórmula de regressão de Excel aplicada às 8 pela última vez linhas |Mesmo |Mesmo |
| **Performance** (Desempenho) | | | |
| Ajustado quadrado de R |0.96 |N/A | |
| Coeficiente de determinação |N/A |0.78 |0.952049 |
| Significa que o erro absoluto |US $9. 5M |US $ 19.4 M |US $9. 5M |
| Significa que o erro absoluto (%) |<span style="background-color: 00FF00;"> 6.03%</span> |12.2% |<span style="background-color: 00FF00;"> 6.03%</span> |

Além disso, os coeficientes do Excel em comparação com bem os pesos de funcionalidade no modelo treinado do Azure:

|  | Coeficientes do Excel | Pesos de funcionalidade do Azure |
| --- |:---:|:---:|
| Intercepção/tendência |19470209.88 |19328500 |
| Funcionalidade A |0.832653063 |0.834156 |
| Funcionalidade B |11071967.08 |11007300 |
| Funcionalidade C |25383318.09 |25140800 |

## <a name="next-steps"></a>Próximos Passos
Gostaríamos de consumir o serviço web do Machine Learning no Excel. Nosso analistas de negócios que se baseiam no Excel e precisávamos de uma forma de chamar o serviço web do Machine Learning com uma linha de dados do Excel e retornará o valor previsto para o Excel. 

Também gostaríamos de otimizar nosso modelo, utilizando as opções e algoritmos disponíveis no Machine Learning Studio.

### <a name="integration-with-excel"></a>Integração com o Excel
Nossa solução era operacionalizar o nosso modelo de regressão de Machine Learning através da criação de um serviço web do modelo treinado. Em poucos minutos, o serviço web foi criado e vamos chamá-lo diretamente a partir do Excel para retornar um valor de receita prevista. 

O *Dashboard de serviços da Web* secção inclui um livro do Excel que pode ser baixado. A pasta de trabalho vem previamente formatada com as web service API informações de esquema e incorporadas. Quando clica em *transferir o livro do Excel*, abre o livro e pode guardá-lo para o computador local. 

![][1]

Com o livro aberto, copie os parâmetros predefinidos para a secção de parâmetro azul conforme mostrado abaixo. Assim que os parâmetros forem inseridos, o Excel chama o serviço da web de Machine Learning e as etiquetas com a pontuação previstas serão exibida na secção de valores previstos verde. O livro irá continuar a criar previsões de indisponibilidade para os parâmetros com base no seu modelo preparado para todos os itens de linha introduzidos em parâmetros. Para obter mais informações sobre como utilizar esta funcionalidade, consulte [consumindo um serviço de Web do Azure Machine Learning a partir do Excel](consuming-from-excel.md). 

![][2]

### <a name="optimization-and-further-experiments"></a>Otimização e ainda mais experimentações
Agora que tivéssemos uma linha de base com o nosso modelo do Excel, passamos em frente otimizar o nosso modelo de regressão Linear do Machine Learning. Usamos o módulo [seleção de funcionalidades com base no filtro] [ filter-based-feature-selection] para melhorar a nossa seleção de dados iniciais elementos e ele ajudaram-na alcançar uma melhoria de desempenho de 4.6% significa erro absoluto. Para projetos futuros utilizaremos esta funcionalidade que foi possível guardar nos semanas de iterar por meio de atributos de dados para localizar o conjunto certo de recursos para utilizar para a modelação de chegar. 

Em seguida, planejamos incluir algoritmos adicionais, como [Bayesianos] [ bayesian-linear-regression] ou [árvores de decisões elevada] [ boosted-decision-tree-regression] na nossa experiência para comparar desempenho. 

Se quiser experimentar regressão, um bom conjunto de dados para tentar é o dataset de exemplo de regressão de eficiência de energia, com vários atributos numéricos. O conjunto de dados é fornecido como parte dos conjuntos de dados de exemplo no Machine Learning Studio. Pode usar uma variedade de módulos de aprendizagem para prever a carga aquecer ou carga de resfriamento. O gráfico abaixo é que uma comparação de desempenho de diferente regressão aprende contra a previsão de conjunto de dados de eficiência energética para a variável de destino Cooling carga: 

| Modelo | Significa que o erro absoluto | Erro ao quadrado de média de raiz | Erro relativo absoluto | Erro ao quadrado de caminho relativo | Coeficiente de determinação |
| --- | --- | --- | --- | --- | --- |
| Árvore de decisões elevada |0.930113 |1.4239 |0.106647 |0.021662 |0.978338 |
| Regressão linear (gradiente descendente) |2.035693 |2.98006 |0.233414 |0.094881 |0.905119 |
| Regressão de rede neural |1.548195 |2.114617 |0.177517 |0.047774 |0.952226 |
| Regressão linear (quadrados de menos comum) |1.428273 |1.984461 |0.163767 |0.042074 |0.957926 |

## <a name="key-takeaways"></a>Principais pedidas
Aprendemos muito por de regressão de Excel em execução e o Azure Machine Learning experimentações em paralelo. Criando o modelo de linha de base no Excel e compará-los aos modelos com o Machine Learning [regressão Linear] [ linear-regression] ajudou-nos saber que o Azure Machine Learning e Descobrimos oportunidades para melhorar a dados desempenho de seleção e modelo. 

Encontrámos também é aconselhável usar [seleção de funcionalidades com base no filtro] [ filter-based-feature-selection] para acelerar os projetos de predição futuras. Aplicando a seleção de funcionalidades aos seus dados, pode criar um modelo aprimorado no Machine Learning com melhor desempenho geral. 

A capacidade de transferir o preditiva analíticos de previsão do Machine Learning para o Excel sistematicamente permite que um aumento significativo na capacidade de fornecer os resultados com êxito para um público de usuários de negócios ampla. 

## <a name="resources"></a>Recursos
Aqui estão alguns recursos para ajudá-lo a trabalhar com a regressão: 

* Regressão no Excel. Se nunca tenha tentado regressão no Excel, este tutorial torna mais fácil: [http://www.excel-easy.com/examples/regression.html](http://www.excel-easy.com/examples/regression.html)
* Vs de regressão de previsão. Tyler Chessman escreveu um artigo de blogue explicando como no Excel, que contém descrição um principiante boa de regressão linear de previsão de série de tempo. [http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts](http://sqlmag.com/sql-server-analysis-services/understanding-time-series-forecasting-concepts) 
* Regressão Linear de quadrados de menos comum: Falhas, problemas e armadilhas. Para uma introdução e a discussão de regressão: [http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/ ](http://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/)

[1]: ./media/linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png
[2]: ./media/linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png


<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

