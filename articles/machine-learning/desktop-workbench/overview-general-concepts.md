---
title: Descrição geral conceptual de funcionalidades de pré-visualização do Azure Machine Learning | Documentos da Microsoft
description: Uma descrição geral conceptual dos recursos de pré-visualização do Azure Machine Learning, como as subscrições, contas, áreas de trabalho, projetos, etc.
services: machine-learning
author: serinakaye
ms.author: serinak
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/06/2017
ms.openlocfilehash: f63b9c077e64b642adfd8c7eed5026563eb6319a
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35647200"
---
# <a name="azure-machine-learning---concepts"></a>Azure Machine Learning - conceitos

Este artigo define e descreve os conceitos que precisa saber para utilizar o Azure Machine Learning. 

![Hierarquia de conceitos](media/overview-general-concepts/hierarchy.png)

- **Subscrição:** uma subscrição do Azure lhe concede acesso a recursos no Azure. Porque o Azure Machine Learning está profundamente integrado com computação, armazenamento e muitos outros recursos do Azure e serviços, a Bancada de trabalho requer que cada utilizador tem acesso a uma subscrição do Azure válida. Os utilizadores também tem de ter permissões suficientes nessa subscrição para criar recursos.


- **Conta de experimentação:** conta de experimentação é um recurso do Azure necessário pelo Azure ML e um veículo de faturação. Ela contém as áreas de trabalho, que por sua vez contêm projetos. Pode adicionar vários utilizadores, conhecidos como _postos de trabalho_, para uma conta de experimentação. Tem de ter acesso a uma conta de experimentação para poder utilizar o Azure ML Workbench para executar experimentações. 


- **Conta de gestão de modelos** uma conta de gestão também é um recurso do Azure requerido pelo Azure ML para gestão de modelos. Pode usá-lo para registar modelos e manifestos, criar serviços web em contentores e implementá-las localmente ou na cloud. Também é o veículo de faturação do Azure ML.


- **Área de trabalho:** uma área de trabalho é o componente principal para a partilha e colaboração no Azure ML. Projetos são agrupados dentro de uma área de trabalho. Uma área de trabalho, em seguida, pode ser partilhada com vários utilizadores que foram adicionados para a conta de experimentação.


- **Projeto:** no Azure Machine Learning, um projeto é o contentor lógico para todo o trabalho a ser feito para resolver um problema. Mapeiam para uma pasta de ficheiro individual no disco local e pode adicionar ficheiros ou sub-pastas à mesma. Um projeto, opcionalmente, pode ser associado um repositório de Git para controlo de código fonte e colaboração.  

- **Experimentação:** no Azure ML, uma experimentação é um ou mais ficheiros de código fonte que podem ser executados a partir de um ponto de entrada única. Pode conter tarefas, tais como ingestão de dados, engenharia de funcionalidades, preparação de modelos ou avaliação do modelo. Atualmente, o Azure ML oferece suporte a Python ou PySpark experimentações apenas.


- **Modelo:** no Azure Machine Learning, modelos de referir-se para o produto de uma experimentação do machine learning. Eles são receitas que quando aplicadas corretamente aos dados, gerar valores previstos. Modelos podem ser implementadas para teste ou produção ambientes e utilizadas para novos dados de classificação. Uma vez na produção, modelos podem ser monitorizados para dados de desempenho e desvios e reestruturar os conforme necessário. 

- **Destino de computação:** um destino de computação é o recurso de computação que configurou para a execução de suas experimentações. Pode ser seu computador local (Windows ou macOS), o contentor do Docker em execução no seu computador local ou numa VM do Linux no Azure ou um cluster do Spark do HDInsight.


- **Run:** do serviço experimentação define uma execução, como o tempo de vida de uma execução de experiência num destino de computação. O Azure ML captura informações de cada execução automaticamente e apresenta o histórico completo de uma experimentação específico na forma de histórico de execuções.

- **Ambiente:** no Azure Machine Learning, um ambiente denota um recurso informático específico que é utilizado para implementar e gerir os seus modelos. Pode ser seu computador local, uma VM do Linux no Azure ou um cluster de Kubernetes em execução no Azure Container Service, dependendo do contexto e a configuração. O modelo é alojado num contentor do Docker em execução nesses ambientes e exposto como um ponto de final de REST API.


- **Modelo gerenciado:** gestão de modelos permite-lhe implementar modelos como serviços da web, gerir várias versões dos seus modelos e monitorizar o desempenho e as métricas. Modelos geridos são registrados com uma conta de gestão de modelos do Azure Machine Learning.

- **Manifestos:** quando o sistema de gestão de modelos implementa um modelo em produção, ele inclui um manifesto que pode incluir o modelo, dependências, script de classificação, dados de exemplo e esquema. O manifesto é a receita usada para criar uma imagem de contentor do Docker. Utilizar a gestão de modelos, pode gerar automaticamente os manifestos, versões diferentes de criar e gerir esses manifestos. 


- **Imagens:** usa os manifestos para gerar (e voltar a gerar) imagens do Docker. Imagens do Docker em contentores criar flexibilidade para executá-los na nuvem, nas máquinas locais ou num dispositivo de IoT. Imagens são independentes e incluem todas as dependências necessárias para a classificação de dados novo com modelos. 

- **Serviços:** gestão de modelos permite-lhe implementar modelos como serviços web. A lógica de serviço Web e as dependências são encapsuladas numa imagem. Cada serviço Web é um conjunto de contentores com base em imagem preparado para pedidos de serviço para uma determinada URL. Um serviço web é contabilizado como uma única implementação.
