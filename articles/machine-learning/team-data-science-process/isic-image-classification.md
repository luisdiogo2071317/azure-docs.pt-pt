---
title: Classificação de imagem com o pacote de aprendizagem do Azure (AML) para problemas de visão do computador (AMLPCV) e o processo de ciência de dados de equipa (TDSP) | Microsoft Docs
description: Descreve a utilização de TDSP (processo de ciência de dados de equipa) e AMLPCV para classificação de imagem
services: machine-learning, team-data-science-process
documentationcenter: ''
author: xibingao
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: xibingao
ms.openlocfilehash: 40beeea5c699488d48065a08f5dce0ae212ba617
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34736394"
---
# <a name="skin-cancer-image-classification-with-azure-machine-learning-aml-package-for-computer-vision-amlpcv-and-team-data-science-process-tdsp"></a>Classificação de imagem de Cancer skin com o pacote do Azure Machine Learning (AML) para o computador visão (AMLPCV) e o processo de ciência de dados de equipa (TDSP)

## <a name="introduction"></a>Introdução

Este artigo mostra como utilizar o [Azure Machine Learning pacote para o computador visão (AMLPCV)](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest) para preparar, testar e implementar uma **imagem classificação** modelo. Este exemplo utiliza a estrutura TDSP e modelos no [Azure Machine Learning Workbench](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation). O exemplo completo é fornecido nestas instruções. Utiliza [CNTK](https://www.microsoft.com/en-us/cognitive-toolkit/) como avançada aprender framework e de preparação é efetuada uma [VM de ciência de dados](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview) máquina GPU. Implementação utilizar a CLI do Azure ML Operationalization.

Muitas aplicações no domínio de visão computador podem ser framed como problemas de classificação de imagem. Estes incluem a criação de modelos que responder a perguntas como "é um objeto presente na imagem?" (pode ser objeto *preguiçoso*, *carro*, ou *são enviados*) e mais complexo questões, tais como "que classe gravidade de disease olho é evinced pela análise retinal este patient?" AMLPCV simplifica o processamento de dados de classificação de imagem e o pipeline de modelação. 

## <a name="link-to-github-repository"></a>Associar ao repositório do GitHub
Podemos fornecer aqui resumo documentação sobre o exemplo. Pode encontrar documentação mais extensa no [GitHub site](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification).

## <a name="team-data-science-process-tdsp-walkthrough-with-amlpcv"></a>Instruções processo (TDSP) de ciência de dados do equipa com AMLPCV

Estas instruções utilizam o [processo de ciência de dados de equipa (TDSP)](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview) ciclo de vida.

As instruções abrangem os seguintes passos de ciclo de vida:

### <a name="1-data-acquisionhttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode01dataacquisitionandunderstanding"></a>[1. Acquision de dados](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/01_data_acquisition_and_understanding)
Conjunto de dados ISIC é utilizado para a tarefa de classificação de imagem. ISIC (a internacional Skin imagens colaboração) é um partership entre academia e da indústria para facilitar a aplicação de digital skin imaging estudar e ajudar a reduzir melanoma mortality. O [arquivo ISIC](https://isic-archive.com/#images) contém mais de 13.000 imagens de lesion skin com etiquetas benignas ou malignant. Iremos transferir uma amostra das imagens de arquivo ISIC.

### <a name="2-modelinghttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode02modeling"></a>[2. Modelação](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/02_modeling)
No passo de modelação, os substeps seguintes são executados. 

<b>2.1 criação de conjunto de dados do</b><br>

Para gerar um objeto do conjunto de dados no AMLPCV, forneça um diretório de raiz de imagens no disco local. 

<b>2.2 imagem visualização e a anotação</b><br>

Visualizar as imagens no objeto do conjunto de dados e corrigir algumas das etiquetas, se necessário.

<b>2.3 imagem Augmentation</b><br>

Aumentar um objeto de conjunto de dados utilizando as transformações descritas a [imgaug](https://github.com/aleju/imgaug) biblioteca.

<b>2.4 DNN modelo definição</b><br>

Defina a arquitetura de modelo utilizada no passo de preparação. Seis modelos de rede neuronal profundidade de por preparado diferentes são suportados em AMLPCV: AlexNet, Resnet-18, Resnet-34 e Resnet a 50, Resnet 101 e Resnet 152.

<b>2.5 classificador formação</b><br>

Preparar as redes neurais com predefinido ou parâmetros personalizados.

<b>2.6 avaliação e visualização</b><br>

O substep fornece funcionalidades para avaliar o desempenho do modelo treinado um conjunto de dados de teste independentes. As métricas de avaliação incluem precisão, precisão e devolução de chamada e curva ROC.

Esses substeps são explicados em detalhe no bloco de notas do Jupyter correspondente. Também fornecemos-lhe orientações para desativar os parâmetros, tais como velocidade, tamanho do lote mini e taxa dropout para melhorar ainda mais o desempenho do modelo de aprendizagem.

### <a name="3-deploymenthttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode03deployment"></a>[3. Implementação](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/03_deployment)

Este passo operationalizes modelo produzido a partir do passo de modelação. Introduz os pré-requisitos de operationalization e o programa de configuração. Por fim, o consumo do serviço web, também é explicado. Com este tutorial, pode saber a criação de modelos de aprendizagem profunda AMLPCV e operacionalizar o modelo no Azure.

## <a name="next-steps"></a>Próximos Passos
Ler mais documentação no [Azure Machine Learning pacote para o computador visão (AMLPCV)](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest) e [processo de ciência de dados de equipa (TDSP)](https://aka.ms/tdsp) para começar a utilizar.


## <a name="references"></a>Referências

* [Do Azure Machine Learning pacote para problemas de visão do computador (AMLPCV)](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest)
* [Azure Machine Learning Workbench](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation)
* [VM de ciência de dados](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)

