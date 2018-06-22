---
title: Imagem de classificação com o pacote do Azure Machine Learning para problemas de visão do computador e o processo de ciência de dados de equipa (TDSP) | Microsoft Docs
description: Descreve a utilização do processo de ciência de dados de equipa (TDSP) e o pacote do Azure Machine Learning para visão de computador para classificação de imagem.
services: machine-learning, team-data-science-process
documentationcenter: ''
author: xibingao
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: xibingao
ms.openlocfilehash: f9e88cfb7185845e96f287b39bebaaa24320f537
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36300803"
---
# <a name="skin-cancer-image-classification-with-the-azure-machine-learning-package-for-computer-vision-and-team-data-science-process"></a>Da máscara de classificação de imagem cancer com o pacote do Azure Machine Learning para problemas de visão do computador e o processo de ciência de dados de equipa

Este artigo mostra como utilizar o [pacote do Azure Machine Learning para problemas de visão do computador](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest) para preparar, testar e implementar uma *imagem classificação* modelo. Este exemplo utiliza a estrutura de processo de ciência de dados de equipa (TDSP) e modelos no [Azure Machine Learning Workbench](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation). Esta explicação passo a passo fornece o exemplo completo. Utiliza o [cognitivos Toolkit de](https://www.microsoft.com/en-us/cognitive-toolkit/) como avançada aprender framework e de preparação é efetuada uma [máquina virtual de ciência de dados](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview) máquina GPU. Implementação utilizar a CLI de operationalization do Azure Machine Learning.

Muitas aplicações no domínio de visão computador podem ser framed como problemas de classificação de imagem. Estes incluem a criação de modelos que responder a perguntas simples como "é um objeto presente na imagem?" em que o objeto pode ser um preguiçoso, car ou incorporadas. Também inclui as respostas a perguntas mais complexas, tais como "que classe gravidade de disease olho é evinced pela análise retinal este patient?" O pacote do Azure Machine Learning para problemas de visão do computador simplifica o processamento de dados de classificação de imagem e o pipeline de modelação. 

## <a name="link-to-the-github-repository"></a>Ligação para o repositório do GitHub
Este artigo é um documento de resumo sobre a amostra. Pode encontrar documentação mais extensa no [GitHub site](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification).

## <a name="team-data-science-process-walkthrough"></a>Instruções do processo de ciência de dados de equipa

Estas instruções utilizam o [o processo de ciência de dados de equipa](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview) ciclo de vida. As instruções abrangem os seguintes passos de ciclo de vida.

### <a name="1-data-acquisitionhttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode01dataacquisitionandunderstanding"></a>[1. Aquisição de dados](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/01_data_acquisition_and_understanding)
O conjunto de dados de colaboração Imaging Skin internacional (ISIC) é utilizado para a tarefa de classificação de imagem. ISIC é uma parceria entre academia e da indústria para facilitar a aplicação de skin digital imaging estudar e ajudar a reduzir melanoma mortality. O [arquivo ISIC](https://isic-archive.com/#images) contém mais de 13.000 skin lesion as imagens que estão identificadas como benignas ou malignant. Transfira uma amostra das imagens a partir do arquivo ISIC.

### <a name="2-modelinghttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode02modeling"></a>[2. Modelação](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/02_modeling)
No passo modelação, os substeps seguintes são executados.

#### <a name="dataset-creation"></a>Criação de conjunto de dados

Gere um objeto do conjunto de dados no pacote do Azure Machine Learning para problemas de visão do computador, fornecendo um diretório de raiz de imagens no disco local. 

#### <a name="image-visualization-and-annotation"></a>Visualização de imagem e anotação

Visualize as imagens no objeto do conjunto de dados e corrigir as etiquetas conforme necessário.

#### <a name="image-augmentation"></a>Augmentation de imagem

Aumentar um objeto do conjunto de dados utilizando as transformações descritas o [imgaug](https://github.com/aleju/imgaug) biblioteca.

#### <a name="dnn-model-definition"></a>Definição de modelo DNN

Defina a arquitetura de modelo que é utilizada no passo de preparação. Seis modelos de rede neuronal profundo previamente formação são suportados no pacote do Azure Machine Learning para problemas de visão do computador: AlexNet, Resnet-18, Resnet 34, Resnet a 50, Resnet 101 e Resnet 152.

#### <a name="classifier-training"></a>Formação classificador

Preparar as redes neurais com predefinido ou parâmetros personalizados.

#### <a name="evaluation-and-visualization"></a>Avaliação e visualização

Fornece a funcionalidade para avaliar o desempenho do modelo treinado um conjunto de dados de teste independentes. As métricas de avaliação incluem precisão, precisão, devolução de chamada e curva ROC.

Os substeps são explicados em detalhe no bloco de notas do Jupyter correspondente. O bloco de notas também fornece diretrizes para desativar os parâmetros, como o ritmo de aprendizagem, tamanho do lote mini e velocidade de dropout para melhorar ainda mais o desempenho de modelo.

### <a name="3-deploymenthttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode03deployment"></a>[3. Implementação](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/03_deployment)

Este passo operationalizes o modelo que é produzido no passo de modelação. Introduz os pré-requisitos e o programa de configuração necessário. Também é explicado o consumo do serviço web. Neste tutorial, saiba criar modelos de profunda learning com o pacote do Azure Machine Learning para problemas de visão do computador e operacionalizar o modelo no Azure.

## <a name="next-steps"></a>Passos Seguintes
- Leia a documentação adicional sobre [pacote do Azure Machine Learning para problemas de visão do computador](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest).
- Leia o [o processo de ciência de dados de equipa](https://aka.ms/tdsp) documentação para começar a utilizar.


## <a name="references"></a>Referências

* [Pacote de Machine Learning do Azure para problemas de visão do computador](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest)
* [Azure Machine Learning Workbench](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation)
* [Máquina de virtual de ciência de dados](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)

