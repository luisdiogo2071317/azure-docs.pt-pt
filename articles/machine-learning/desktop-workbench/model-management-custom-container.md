---
title: Personalizar a imagem de contentor utilizada para implementar modelos de ML do Azure | Documentos da Microsoft
description: Este artigo descreve como personalizar uma imagem de contentor para modelos do Azure Machine Learning
services: machine-learning
author: tedway
ms.author: tedway
manager: mwinkle
ms.reviewer: mldocs, raymondl
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 03/26/2018
ROBOTS: NOINDEX
ms.openlocfilehash: e49743de817bc1fe92afcbc80764771f6df66c56
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53271271"
---
# <a name="customize-the-container-image-used-for-azure-ml-models"></a>Personalizar a imagem de contentor utilizada para modelos do Azure ML

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



Este artigo descreve como personalizar uma imagem de contentor para modelos do Azure Machine Learning.  O Azure ML Workbench utiliza contentores para implementar modelos de aprendizagem automática. Os modelos são implementados, juntamente com as respetivas dependências, e o Azure ML baseia-se uma imagem a partir do modelo, as dependências e os ficheiros associados.

## <a name="how-to-customize-the-docker-image"></a>Como personalizar a imagem do Docker
Personalize a imagem do Docker Azure ML implementa a utilizar:

1. Uma `dependencies.yml` ficheiro: para gerir as dependências que podem ser instaladas a partir de [PyPi]( https://pypi.python.org/pypi), pode utilizar o `conda_dependencies.yml` de ficheiros do projeto Bancada de trabalho ou crie os seus próprios. Esta é a abordagem recomendada para a instalação de dependências de Python são instaláveis no pip.

   Comando da CLI de exemplo:
   ```azurecli
   az ml image create -n <my Image Name> --manifest-id <my Manifest ID> -c amlconfig\conda_dependencies.yml
   ```

   Exemplo de ficheiro conda_dependencies: 
   ```yaml
   name: project_environment
   dependencies:
      - python=3.5.2
      - scikit-learn
      - ipykernel=4.6.1
      
      - pip:
        - azure-ml-api-sdk==0.1.0a11
        - matplotlib
   ```
        
2. Ficheiro de passos de um Docker: utilizar esta opção, personaliza a imagem implementada pela instalação de dependências que não podem ser instaladas a partir PyPi. 

   O ficheiro deve incluir passos de instalação do Docker como um DockerFile. Os comandos seguintes são permitidos no ficheiro: 

    EXECUTAR, ENV, ARG, ETIQUETA, EXPOR

   Comando da CLI de exemplo:
   ```azurecli
   az ml image create -n <my Image Name> --manifest-id <my Manifest ID> --docker-file <myDockerStepsFileName> 
   ```

   Imagem, o manifesto e o serviço comandos aceitam o sinalizador de ficheiro do docker.

   Ficheiro de passos de Docker de exemplo:
   ```docker
   # Install tools required to build the project
   RUN apt-get update && apt-get install -y git libxml2-dev
   # Install library dependencies
   RUN dep ensure -vendor-only
   ```

> [!NOTE]
> A imagem de base para contentores do Azure ML é o Ubuntu e não pode ser alterada. Se especificar uma imagem de base diferente, ela será ignorada.

## <a name="next-steps"></a>Passos Seguintes
Agora que personalizou sua imagem de contentor, pode implementá-la a um cluster para utilização em grande escala.  Para obter detalhes sobre como configurar um cluster para a implementação de serviço da web, consulte [configuração de gestão de modelo](deployment-setup-configuration.md). 
