---
title: Formatos de conteúdo de registo de contentor do Azure
description: Saiba mais sobre os formatos de conteúdo suportados no Azure Container Registry.
services: container-registry
author: dlepow
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 10/31/2018
ms.author: danlep
ms.openlocfilehash: e7155604339bc634078fd022e05ede5f902bc0d8
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634715"
---
# <a name="content-formats-supported-in-azure-container-registry"></a>Formatos de conteúdo suportados no Azure Container Registry

Utilize um repositório privado no Azure Container Registry para gerir um dos seguintes formatos de conteúdo. 

## <a name="docker-compatible-container-images"></a>Imagens de contentor compatível com o docker

* [V2 no manifesto de imagem no docker, esquema 1](https://docs.docker.com/registry/spec/manifest-v2-1/)

* [V2 de manifesto de imagem do docker, 2 de esquema](https://docs.docker.com/registry/spec/manifest-v2-2/) -inclui o manifesto de lista que permitem registos para armazenar imagens em várias plataformas numa referência única "imagem: etiqueta"

* [Abra a especificação de formato de imagem do contentor iniciativa (OCI)](https://github.com/opencontainers/image-spec/blob/master/spec.md) 


## <a name="helm-charts"></a>Gráficos Helm

O Azure Container Registry também pode alojar repositórios para [executar Helm gráficos](https://helm.sh/), um formato de empacotamento utilizado para gerir e implementar aplicações para Kubernetes rapidamente. [Cliente de Helm](https://docs.helm.sh/using_helm/#installing-helm) versão 2.11.0 ou posterior é suportado.

## <a name="next-steps"></a>Passos Seguintes

* Veja como [enviar e extrair](container-registry-get-started-docker-cli.md) imagens com o Azure Container Registry.

* Uso [tarefas ACR](container-registry-tasks-overview.md) para criar e testar imagens de contentor. 

* Utilize o [Moby BuildKit](https://github.com/moby/buildkit) para compilar e empacotar contentores no formato OCI.

* Configurar uma [repositório Helm](container-registry-helm-repos.md) alojado no Azure Container Registry. 


