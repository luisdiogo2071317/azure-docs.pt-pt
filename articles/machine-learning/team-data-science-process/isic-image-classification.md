---
title: Classificação de imagens com o pacote do Azure Machine Learning para imagem digitalizada e o processo de ciência de dados de equipa (TDSP) | Documentos da Microsoft
description: Descreve a utilização do processo de ciência de dados de equipa (TDSP) e o pacote do Azure Machine Learning para imagem digitalizada para classificação de imagens.
services: machine-learning, team-data-science-process
documentationcenter: ''
author: deguhath
ms.author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.openlocfilehash: ee2e797f3838b8b6b36174d14c73e97fe9790315
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49392817"
---
# <a name="skin-cancer-image-classification-with-the-azure-machine-learning-package-for-computer-vision-and-team-data-science-process"></a>Da máscara de classificação de imagens de cancro com o pacote do Azure Machine Learning para imagem digitalizada e o processo de ciência de dados de equipa

Este artigo mostra-lhe como utilizar o [pacote do Azure Machine Learning para imagem digitalizada](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest) para preparar, testar e implementar uma *classificação de imagens* modelo. O exemplo usa a estrutura de processo de ciência de dados de equipa (TDSP) e modelos no [Azure Machine Learning Workbench](https://docs.microsoft.com/azure/machine-learning/service/quickstart-installation). Este passo a passo fornece o exemplo completo. Ele usa o [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) como o amplo framework de aprendizagem, treinamento e é executado num [máquina virtual de ciência de dados](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview) máquina GPU. A implementação utiliza operacionalização da CLI do Azure Machine Learning.

Muitos aplicativos do domínio de visão do computador podem ser estruturados como problemas de classificação de imagem. Estes incluem a criação de modelos que responder a perguntas simples como "é um objeto presente na imagem?" em que o objeto pode ser um cachorro, um carro ou uma nave. Também inclui respostas às perguntas mais complexas, tais como "o que a classe de severidade de doenças de olho é evinced por verificação de retinal este paciente?" O pacote do Azure Machine Learning para imagem digitalizada simplifica o processamento de dados de classificação de imagens e o pipeline de modelagem. 

## <a name="link-to-the-github-repository"></a>Ligação para o repositório do GitHub
Este artigo é um documento de resumo sobre o exemplo. Pode encontrar mais extensa documentação sobre o [site do GitHub](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification).

## <a name="team-data-science-process-walkthrough"></a>Instruções do Team Data Science Process

Estas instruções utilizam o [Team Data Science Process](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) ciclo de vida. Passo a passo inclui os seguintes passos de ciclo de vida.

### <a name="1-data-acquisitionhttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode01dataacquisitionandunderstanding"></a>[1. Aquisição de dados](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/01_data_acquisition_and_understanding)
O conjunto de dados de colaboração da máscara de geração de imagens do internacional (ISIC) é utilizado para a tarefa de classificação de imagem. ISIC é uma parceria entre a instituições académicas e do setor para facilitar a aplicação de capa digital estudar e ajudar a reduzir melanoma mortality de geração de imagens. O [arquivo ISIC](https://isic-archive.com/#images) contém mais de 13 000 imagens de lesion da máscara que estão identificadas como o benigno ou malignant. Baixe uma amostra das imagens do arquivo ISIC.

### <a name="2-modelinghttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode02modeling"></a>[2. Modelagem](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/02_modeling)
O passo de modelagem, os substeps seguintes são executadas.

#### <a name="dataset-creation"></a>Criação de conjunto de dados

Gere um objeto de conjunto de dados no pacote do Azure Machine Learning para imagem digitalizada, fornecendo um diretório de raiz de imagens no disco local. 

#### <a name="image-visualization-and-annotation"></a>Visualização da imagem e a anotação

Visualize as imagens no objeto de conjunto de dados e corrigir as etiquetas conforme necessário.

#### <a name="image-augmentation"></a>Aumento de imagem

Incrementar um objeto de conjunto de dados utilizando as transformações que são descritas a [imgaug](https://github.com/aleju/imgaug) biblioteca.

#### <a name="dnn-model-definition"></a>Definição de modelo DNN

Defina a arquitetura de modelo que é utilizada no passo de treinamento. Modelos de rede neural profunda com formação prévia seis são suportados no pacote do Azure Machine Learning para imagem digitalizada: AlexNet, utilizar o Resnet-18, utilizar o Resnet-34, utilizar o Resnet-50, utilizar o Resnet-101 e utilizar o Resnet-152.

#### <a name="classifier-training"></a>Treinamento de classificador

Prepare as redes neurais com predefinido ou parâmetros personalizados.

#### <a name="evaluation-and-visualization"></a>Avaliação e visualização

Fornece a funcionalidade para avaliar o desempenho do modelo preparado num conjunto de dados de teste independentes. As métricas de avaliação incluem a precisão, a precisão, lembre-se e curva cor MULTICLASSE.

Os substeps são explicados em detalhe no bloco de notas do Jupyter correspondente. O bloco de notas também fornece diretrizes para transformar os parâmetros, como o ritmo de aprendizagem, o tamanho do lote mini e a velocidade de dropout para melhorar ainda mais o desempenho do modelo.

### <a name="3-deploymenthttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode03deployment"></a>[3. Implementação](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/03_deployment)

Este passo operationalizes o modelo que é produzido a partir do passo de modelagem. Ele apresenta os pré-requisitos e a configuração necessária. Também é explicado o consumo do serviço web. Neste tutorial, vai aprender a criar modelos de aprendizagem profunda com o pacote do Azure Machine Learning para imagem digitalizada e operacionalizar o modelo no Azure.

## <a name="next-steps"></a>Passos Seguintes
- Leia a documentação adicional sobre [pacote do Azure Machine Learning para imagem digitalizada](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest).
- Leitura a [Team Data Science Process](https://aka.ms/tdsp) documentação para começar a utilizar.


## <a name="references"></a>Referências

* [Pacote de Machine Learning do Azure para imagem digitalizada](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest)
* [Azure Machine Learning Workbench](https://docs.microsoft.com/azure/machine-learning/service/quickstart-installation)
* [Máquina de virtual de ciência de dados](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)

