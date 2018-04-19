---
title: Personalizar a imagem de contentor utilizada para implementação de modelos do Azure ML | Microsoft Docs
description: Este artigo descreve como personalizar uma imagem de contentor para os modelos do Azure Machine Learning
services: machine-learning
author: tedway
ms.author: tedway, raymondl
manager: mwinkle
ms.reviewer: mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 3/26/2018
ms.openlocfilehash: f56b651c40187e42361ac12f0cbf4e509385e0d2
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="customize-the-container-image-used-for-azure-ml-models"></a>Personalizar a imagem de contentor utilizada para modelos do Azure ML

Este artigo descreve como personalizar uma imagem de contentor para os modelos do Azure Machine Learning.  Azure ML Workbench utiliza contentores para implementar modelos de machine learning. Os modelos são implementados em conjunto com as respetivas dependências e do Azure ML cria uma imagem a partir do modelo, as dependências e os ficheiros associados.

## <a name="how-to-customize-the-docker-image"></a>Como personalizar a imagem do Docker
Personalize a imagem de Docker do Azure ML implementa a utilizar:

1. A `dependencies.yml` ficheiro: para gerir as dependências que podem ser instaladas a partir de [PyPi]( https://pypi.python.org/pypi), pode utilizar o `conda_dependencies.yml` de ficheiros do projeto Workbench ou criar os seus próprios. Esta é a abordagem de Recomendamos para instalação de dependências de Python que podem ser instaladas pip.

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
        
2. Um Docker passos ficheiro: utilizar esta opção, personalizar a imagem de implementado através da instalação de dependências que não podem ser instaladas na PyPi. 

   O ficheiro deve incluir passos de instalação do Docker como um DockerFile. Os comandos seguintes são permitidos no ficheiro: 

    EXECUTAR, ENV, ARG, ETIQUETA, EXPOR

   Comando da CLI de exemplo:
   ```azurecli
   az ml image create -n <my Image Name> --manifest-id <my Manifest ID> --docker-file <myDockerStepsFileName> 
   ```

   Imagem, o manifesto e o serviço comandos aceitam o sinalizador de ficheiro de docker.

   Ficheiro de passos de Docker de exemplo:
   ```docker
   # Install tools required to build the project
   RUN apt-get update && apt-get install -y git libxml2-dev
   # Install library dependencies
   RUN dep ensure -vendor-only
   ```

> [!NOTE]
> A imagem de base para contentores do Azure ML é Ubuntu e não pode ser alterada. Se especificar uma imagem base diferentes, serão ignorada.

## <a name="next-steps"></a>Passos Seguintes
Agora que tiver personalizado a imagem de contentor, pode implementá-la para um cluster para utilização em grande escala.  Para obter mais informações sobre como configurar um cluster para implementação do serviço web, consulte [configuração de gestão de modelo](deployment-setup-configuration.md). 
