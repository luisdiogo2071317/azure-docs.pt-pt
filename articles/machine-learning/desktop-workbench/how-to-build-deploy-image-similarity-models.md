---
title: Criar e implementar um modelo de semelhança de imagem usando o pacote do Azure Machine Learning para imagem digitalizada.
description: Saiba como criar, formar, testar e implementar um modelo de imagem visão do computador similaridade com o pacote do Azure Machine Learning para imagem digitalizada.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 05/20/2018
ROBOTS: NOINDEX
ms.openlocfilehash: b1b4e3e19adb38a69e16aaa98300972df3bb71a8
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093654"
---
# <a name="build-and-deploy-image-similarity-models-with-azure-machine-learning"></a>Criar e implementar modelos de semelhança de imagem com o Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


Neste artigo, saiba como utilizar o pacote do Azure Machine Learning para imagem digitalizada de computador (AMLPCV) para dar formação, testar e implementar um modelo de semelhança de imagem. Para uma descrição geral deste pacote e a respetiva documentação de referência detalhada, consulte https://aka.ms/aml-packages/vision.

Um grande número de problemas do domínio de visão do computador pode ser resolvido com a semelhança de imagem. Esses problemas incluem a criação de modelos que responder a perguntas como:
+ _É um OBJETO presente na imagem? Por exemplo, "cachorro", "car", "Enviar" e assim por diante_
+ _O que a classe de severidade de doenças de olho é evinced por verificação de retinal este paciente?_

Ao criar e implementar este modelo com AMLPCV, que execute os seguintes passos:
1. Criação de conjunto de dados
2. Visualização da imagem e a anotação
3. Aumento de imagem
4. Definição de modelo de rede Neural (DNN) profunda
5. Treinamento de classificador
6. Avaliação e visualização
7. Implementação do serviço Web
8. Teste de carga do Web service

[CNTK](https://www.microsoft.com/en-us/cognitive-toolkit/) é utilizado como a estrutura de aprendizagem profunda, treinamento é executado localmente num computador com a tecnologia de GPU, como o ([VM de ciência de dados de aprendizagem profunda](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)), e a implementação utiliza a CLI do Azure ML Operacionalização.

## <a name="prerequisites"></a>Pré-requisitos

1. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

1. As seguintes contas e aplicação tem de ser configurados e instalados:
   - Uma conta de Experimentação do Azure Machine Learning 
   - Uma conta de gestão de modelos do Azure Machine Learning
   - O Azure Machine Learning Workbench instalado

   Se esses três são ainda não foi criados ou instalados, siga os [instalação manual de início rápido do Azure Machine Learning e Bancada de trabalho](../desktop-workbench/quickstart-installation.md) artigo. 

1. Tem de estar instalado o pacote de Aprendizado de máquina do Azure para imagem digitalizada. Saiba como instalar este pacote, conforme descrito em https://aka.ms/aml-packages/vision.

## <a name="sample-data-and-notebook"></a>Dados de exemplo e o bloco de notas

### <a name="get-the-jupyter-notebook"></a>Obter o bloco de notas do Jupyter

Transferir o bloco de notas para executar o exemplo descrito aqui por conta própria.

> [!div class="nextstepaction"]
> [Obter o bloco de notas do Jupyter](https://aka.ms/aml-packages/vision/notebooks/object_detection)

### <a name="load-the-sample-data"></a>Carregar os dados de exemplo

O exemplo seguinte utiliza um conjunto de dados consiste em imagens de tableware 63. Cada imagem é identificada como pertencente a um dos quatro diferentes classes (bowl, cup, cutlery, prato). O número de imagens neste exemplo é pequeno para que este exemplo pode ser executado rapidamente. Na prática, devem ser fornecidas, pelo menos, 100 imagens por classe. Todas as imagens estão localizadas em *"... /sample_data/imgs_recycling / "* em subdiretórios chamados"bowl","cup","cutlery"e"prato".

![Conjunto de dados do Azure Machine Learning](media/how-to-build-deploy-image-classification-models/recycling_examples.jpg)

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o pacote do Azure Machine Learning para imagem digitalizada nestes artigos:

+ Saiba como [melhorar a precisão desse modelo](how-to-improve-accuracy-for-computer-vision-models.md).

+ Leitura a [descrição geral do pacote](https://aka.ms/aml-packages/vision).

+ Explore os [documentação de referência](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision) para este pacote.

+ Saiba mais sobre [outros pacotes de Python para o Azure Machine Learning](reference-python-package-overview.md).
