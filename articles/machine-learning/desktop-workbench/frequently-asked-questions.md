---
title: Do Azure Machine Learning 2017 Preview perguntas frequentes | Documentos da Microsoft
description: Este artigo contém perguntas freqüentes e respostas para as funcionalidades de pré-visualização do Azure Machine Learning
services: machine-learning
author: serinakaye
ms.author: serinak
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 08/30/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 45cf987d9af7b7dd0e8f05056b49ba56835603e7
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/26/2018
ms.locfileid: "52313950"
---
# <a name="azure-machine-learning-frequently-asked-questions"></a>O Azure Machine Learning, perguntas mais frequentes

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

O Azure Machine Learning é um serviço completamente gerido do Azure que permite-lhe criar, testar, gerenciar e implantar o machine learning e modelos de IA. Nossos serviços e uma aplicação transferível e oferecem uma abordagem de código em primeiro lugar, que tira partido da cloud, no local e edge para fornecer o comboio, implementar, gerir e monitorizar os modelos com energia, a velocidade e flexibilidade. Em alternativa, o Azure Machine Learning Studio oferece um visual, baseados em navegador arrastar-e-soltar ambiente de criação onde não é necessária nenhuma codificação. 

## <a name="general-product-questions"></a>Perguntas gerais do produto

**Que outros serviços do Azure são necessários?**

Azure Container Registry e armazenamento de Blobs do Azure são utilizados pelo Azure Machine Learning. Além disso, terá de aprovisionar recursos de computação, como um cluster de VM de ciência de dados ou o HDInsight. Computação e alojamento também são necessários quando implementar os seus serviços da web, tal como [Azure Container Service](https://docs.microsoft.com/azure/aks).

**Como o Azure Machine Learning se relaciona com a Microsoft Machine Learning Services no SQL Server 2017?**   

Machine Learning Services no SQL Server 2017 é uma plataforma extensível e escalável para a integração de tarefas de machine learning em fluxos de trabalho de base de dados. É um ajuste perfeito para cenários em que é necessária, por exemplo, onde o movimento de dados é dispendioso ou untenable uma solução no local. Por outro lado, as cargas de trabalho na nuvem ou híbrida são uma excelente escolha para os nossos novos serviços do Azure. 

**Como o Azure Machine Learning se relaciona com o Microsoft Machine Learning para o Spark?**

MMLSpark fornece a aprendizagem profunda e ferramentas de ciência de dados para o Apache Spark, com ênfase na produtividade, facilitam de experimentação e algoritmos de topo de gama. MMLSpark oferece integração de pipelines do Spark do Machine Learning com o Microsoft Cognitive Toolkit e OpenCV. Pode criar poderosos e altamente dimensionáveis, modelos preditivos e analíticos para dados de imagem e texto. MMLSpark está disponível sob uma licença de código-fonte aberto e está incluído na bancada de trabalho de AML como um conjunto de algoritmos e modelos de consumíveis. Para obter mais informações sobre o MMLSpark, visite a nossa documentação do produto. 

**Quais versões do Spark são suportadas pelas novas ferramentas e serviços?**

Atualmente, o Workbench inclui e suporta o MMLSpark versão 0,8, que é compatível com Apache Spark 2.1. Tem também uma opção para utilizar a imagem do Docker ativadas para GPU de MMLSpark 0,8 em máquinas virtuais do Linux.

## <a name="experimentation-service"></a>Serviço de experimentação

**O que é o serviço de experimentação do Azure Machine Learning?**

O serviço de experimentação é um serviço gerido do Azure que leva a experimentação do machine learning para o próximo nível. Experimentações podem ser criadas localmente ou na cloud. Protótipo num ambiente de trabalho, em seguida, dimensione rapidamente para máquinas virtuais ou clusters do Spark. VMs do Azure com a mais recente tecnologia GPU permitem que participar de aprendizagem profunda de maneira rápida e eficaz. Também incluímos uma integração profunda com o Git para que pode se conectar facilmente fluxos de trabalho existentes para controlo de código, configuração e colaboração. 

**Como vou ser cobrado para o serviço de experimentação?**

Os dois primeiros utilizadores associados com o serviço de experimentação do Azure Machine Learning são gratuitos. Os utilizadores adicionais são cobrados à tarifa de pré-visualização pública de US $50/mês. Para obter mais informações sobre preços e faturação, visite a nossa página de preços.

**Vou ser cobrado com base no número de experimentações que executo?**

Não, o serviço de experimentação permite experimentações tantos à medida que necessidade e os encargos apenas com base no número de utilizadores. Os recursos de computação de Experimentação são cobrados em separado. Aconselhamo-lo a fazer múltiplas experimentações para que possa encontrar o melhor que se ajusta de modelo para a sua solução.   

**Que tipos específicos de recursos de computação e armazenamento posso utilizar?**

O serviço de experimentação pode executar as suas experimentações em máquinas locais (diretas ou baseadas no Docker), [máquinas virtuais do Azure](https://azure.microsoft.com/services/virtual-machines/), e [HDInsight](https://azure.microsoft.com/services/hdinsight/). O serviço também acessa uma [armazenamento do Azure](https://azure.microsoft.com/services/storage/) conta para armazenar as saídas de execução e pode tirar partido de um [Visual Studio Team Service](https://azure.microsoft.com/services/visual-studio-team-services/) de contas de controle de versão e o armazenamento de Git. Tenha em atenção de que será faturado independentemente de qualquer computação consumidos e recursos de armazenamento, com base nos respetivos preços individuais.  


## <a name="model-management"></a>Gestão de Modelos

**O que é a gestão de modelos do Azure Machine Learning?**

Gestão de modelo do Azure Machine Learning é um serviço gerido do Azure que permite que as equipes de operações de programação e os cientistas de dados implementar modelos preditivos de forma fiável numa grande variedade de ambientes. Repositórios do Git e contentores do Docker fornecem a capacidade de rastreamento e capacidade de repetição. Modelos podem ser implementados de forma fiável na cloud, no local ou do edge. Uma vez na produção, pode gerir o desempenho do modelo e depois reenviar proativamente se degrada o desempenho. Pode implementar modelos de máquinas locais, a [as VMs do Azure](https://azure.microsoft.com/services/virtual-machines/), Spark nos [HDInsight](https://azure.microsoft.com/services/hdinsight/) ou Kubernetes orquestradas [Azure Container Service](https://azure.microsoft.com/services/container-service/) clusters.  

**O que é um "modelo"?**

Um modelo é foi promovida a saída de uma experimentação executá-lo para gestão de modelos. Um modelo que está registado na conta de alojamento é contabilizado relativamente ao seu plano, incluindo modelos atualizados por meio de iteração reparametrização ou versão.

**O que é um "modelo gerido"?**

Um modelo é o resultado de um processo de formação e a aplicação de um algoritmo de machine learning para dados de formação. Gestão de modelos permite-lhe implementar modelos como serviços da web, gerir várias versões dos seus modelos e monitorizar o desempenho e as métricas. Modelos de "Gerido" foram registrados com uma conta de gestão de modelos do Azure Machine Learning. Como exemplo, considere um cenário em que está a tentar prever vendas. Durante a fase de experimentação, é possível gerar muitos modelos com diferentes conjuntos de dados ou algoritmos. Ter gerado quatro modelos com precisões variadas, mas optar por registar apenas o modelo com a precisão mais elevada. O modelo que está registado torna-se o seu primeiro modelo gerenciado.
 
**O que é uma "implementação?"**

Gestão de modelos permite implementar modelos como contentores do serviço web empacotados no Azure. Esses serviços da web podem ser invocados utilizando REST APIs. Cada serviço web é contabilizado como uma única implementação e o número total de implementações ativas é contabilizado relativamente de seu plano. Utilizar a previsão de exemplo, quando implementar o seu melhor modelo de desempenho de vendas, seu plano é incrementado numa implementação. Se, em seguida, voltar a preparar e implementar outra versão, terá duas implementações. Se determinar que o modelo mais recente é melhor e eliminar o original, o número de implementações é diminuída por um.  

**Quais recursos de computação específicos estão disponíveis para as minhas implementações?** 

Gestão de modelos pode executar as suas implementações como contentores do Docker registados [Azure Container Service](https://azure.microsoft.com/services/container-service/), como [máquinas virtuais do Azure](https://azure.microsoft.com/services/virtual-machines/), ou em máquinas locais. Destinos de implementação adicionais serão adicionados em breve. Tenha em atenção de que será faturado independentemente de quaisquer recursos de computação consumidos, com base nos respetivos preços individuais.     

**Pode utilizar a gestão de modelos do Azure Machine Learning para implementar modelos criados com ferramentas além do serviço experimentação?**

Obter a melhor experiência quando implementar modelos criados com o serviço de experimentação. No entanto, pode implementar modelos criados com outras estruturas e ferramentas. Suportamos uma grande variedade de modelos, incluindo o MMLSpark, TensorFlow, Microsoft Cognitive Toolkit, scikit-learn, Keras, etc. 

**Posso usar meus próprio recursos do Azure?**

Sim, o serviço de experimentação e gestão de modelos funcionam em conjunto com vários armazenamentos de dados do Azure, cargas de trabalho e outros serviços de computação. Consulte a nossa documentação técnica para obter mais detalhes sobre os serviços do Azure necessários.

**Suporte no local e cenários de implementação na cloud?**

Sim. Aceitamos no local e cenários de implementação através de contentores do Docker na cloud. Destinos de execução local incluem: implementações de Docker de nó único, [Microsoft SQL Server com os serviços de ML](https://docs.microsoft.com/sql/advanced-analytics/r/r-services), Hadoop, ou do Spark. Também é suportada a nuvem as implementações através do Docker, incluindo: colocar em cluster implementações através de clusters do Azure Container Service e o Kubernetes, o HDInsight ou o Spark. Cenários de borda são suportados através de contentores do Docker e Azure IOT Edge. 

**Pode executar uma imagem do Docker que foi criada com a CLI do Azure Machine Learning através de outro anfitrião?**

Sim. Pode utilizar a imagem como um serviço web em qualquer anfitrião do docker, desde que o anfitrião tem recursos de computação suficiente para alojar a imagem do docker.

**Suporta reparametrização dos modelos implementados?**

Sim, pode implementar várias versões do mesmo modelo. Gestão de modelos irá suportar as atualizações de serviço para modelos todos atualizados e imagens.

## <a name="workbench"></a>Workbench

**O que é o Azure Machine Learning Workbench?**

O Azure Machine Learning Workbench é um aplicativo complementar, criado para cientistas de dados profissionais. Disponível para Windows e Mac, o Machine Learning Workbench fornece descrição geral, gerenciamento e controle para soluções de aprendizagem automática. O Machine Learning Workbench inclui acesso a estruturas de ia de ponta da Microsoft e a Comunidade de código-fonte aberto. Incluímos o toolkits de ciência de dados mais populares, incluindo o TensorFlow, Microsoft Cognitive Toolkit, Spark ML e scikit-saiba e muito mais. Também ativámos a integração com IDEs de ciência de dados populares, como o Jupyter notebooks, PyCharm e Visual Studio Code. O Machine Learning Workbench tem capacidades de preparação de dados internos de exemplo, compreender e preparar dados, estejam eles estruturados ou rapidamente. Nossa ferramenta de preparação de dados nova, chamada [PROSA](https://microsoft.github.io/prose/), baseia-se na tecnologia de última geração da Microsoft Research.  

**É um IDE a Bancada de trabalho?**

Não. O Machine Learning Workbench foi projetado como um complemento para IDEs populares, como o Jupyter Notebooks, PyCharm e Visual Studio Code, mas não é um IDE totalmente funcional. O Machine Learning Workbench oferece algum texto básico de recursos de edição, mas a depuração, intellisense e outros normalmente utilizados recursos IDE não são suportados. Recomendamos que utilize o seu IDE favorito para o desenvolvimento de código, edição e depuração. Também pode ser útil tentar [Visual Studio Code Tools para IA](https://www.visualstudio.com/downloads/ai-tools-vscode).

**Existe um custo para utilizar o Azure Machine Learning Workbench?**

Não. O Azure Machine Learning Workbench é um aplicativo gratuito. Pode transferi-lo para qualquer número de máquinas e de utilizadores, conforme necessitar. Para utilizar o Azure Machine Learning Workbench, precisa de uma conta de Experimentação. .  

**Suporta as capacidades da linha de comandos?**

Sim, o Azure Machine Learning oferece uma interface CLI completa. A CLI do Machine Learning é instalada por predefinição com o Azure Machine Learning Workbench. Ele também é fornecido como parte da máquina de virtual de ciência de dados do Linux no Azure e será integrado a [da CLI do Azure](https://docs.microsoft.com/cli/azure?view=azure-cli-latest)


**Pode utilizar blocos de notas do Jupyter com a Bancada de trabalho?**

Sim! Pode executar blocos de notas do Jupyter na bancada de trabalho, com a Bancada de trabalho que o aplicativo de hospedagem de cliente, assim como usaria um browser como um cliente. 

**Os kernels do Jupyter Notebook são suportados?**

A versão atual do Jupyter incluído com a Bancada de trabalho é iniciado um kernel de Python 3 e um kernel adicional para cada ficheiro de "runconfig" na sua pasta aml_config. Configurações suportadas incluem:
- Local Python
- Python no Docker local ou remoto

## <a name="data-formats-and-capabilities"></a>Formatos de dados e capacidades

**Os formatos de arquivo são atualmente suportados para ingestão de dados da bancada de trabalho?**

As ferramentas de preparação de dados da bancada de trabalho suportam atualmente ingestão dos seguintes formatos: 
- Ficheiros delimitados, como CSV, TSV, etc.  
- Ficheiros de largura fixa
- Ficheiros de texto sem formatação
- Excel (. xls/xlsx)
- Ficheiros JSON
- Ficheiros parquet 
- Arquivos personalizados (scripts), se sua solução necessitar de ingestão de dados de outras fontes, o código Python pode ser utilizado para... 

**As localizações de armazenamento de dados são atualmente suportadas?**

Para pré-visualização pública, Bancada de trabalho oferece suporte a ingestão de dados a partir de: 
- Disco rígido local ou localização de armazenamento de rede mapeadas
- BLOB do Azure ou de armazenamento do Azure (requer uma subscrição do Azure)
- Azure SQL Server
- Microsoft SQL Server


**Quais são os tipos de preparação de dados, preparação e transformações estão disponíveis?**

Para a pré-visualização pública, a Bancada de trabalho suporta "Derivar coluna por exemplo", "Dividir coluna por exemplo", "Clustering de texto", "Como lidar com valores em falta" e muitos outros.  Bancada de trabalho também suporta a conversão de tipo de dados, a agregação de dados (CONTAGEM, média, desvio, etc.) e as associações de dados complexos. Para obter uma lista completa das capacidades suportadas, visite a nossa documentação do produto. 

**Existem limites de tamanho de dados impostos pelo Azure Machine Learning Workbench, experimentação ou gestão de modelos?**

Não, os novos serviços não impõe quaisquer limitações de dados. No entanto, existem limitações introduzidas pelo ambiente no qual está a executar a preparação de dados, a preparação de modelos, a experimentação ou a implementação. Por exemplo, se estiver a filtrar um ambiente local para a formação, estão limitados pelo espaço disponível no disco rígido. Em alternativa, se estiver a filtrar o HDInsight, estão limitadas por qualquer tamanho associado ou restrições de computação. 

## <a name="algorithms-and-libraries"></a>Algoritmos e bibliotecas

**Que algoritmos são suportados no Azure Machine Learning Workbench?**

Nossos produtos de pré-visualização e serviços incluem o melhor da Comunidade de código-fonte aberto. Suportamos uma grande variedade de algoritmos e bibliotecas, incluindo o TensorFlow, scikit-saiba, Apache Spark e o Microsoft Cognitive Toolkit. O Azure Machine Learning Workbench também empacota a [Microsoft revoscalepy](https://docs.microsoft.com/sql/advanced-analytics/python/what-is-revoscalepy) pacote.

**Como é que do Azure Machine Learning se relaciona com o Microsoft Cognitive Toolkit?**

O [Microsoft Cognitive Toolkit](https://docs.microsoft.com/cognitive-toolkit/) é uma das muitas estruturas suportadas pelas nossas novas ferramentas e serviços. O Cognitive Toolkit é um Kit de ferramentas de aprendizagem profunda unificada que permite consumir e combinar popular modelos de machine learning incluindo Feed-Forward redes Neurais profundas redes Convolucionais para, sequência de sequência e redes recorrentes. Para obter mais informações sobre o Microsoft Cognitive Toolkit, visite nosso [documentação do produto](https://docs.microsoft.com/cognitive-toolkit/). 