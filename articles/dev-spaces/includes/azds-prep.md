---
title: incluir ficheiro
description: incluir ficheiro
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: d44f33b0e9f71c1d8d6e2c9878b08f9fa0e1f8a1
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991042"
---
## <a name="preparing-code-for-docker-and-kubernetes-development"></a>Preparar o código para implementação do Docker e Kubernetes
Neste momento, tem uma aplicação Web básica que pode ser executada localmente. Agora, irá colocá-la em contentor ao criar recursos que definem o contentor da aplicação e como esta será implementada no Kubernetes. É fácil executar esta tarefa com o Azure Dev Spaces: 

1. Inicie o VS Code e abra a pasta `webfrontend`. (Pode ignorar os pedidos predefinidos para adicionar recursos de erro ou restaurar o projeto.)
1. Abra o Terminal Integrado no VS Code (através do menu **Ver > Terminal Integrado**).
1. Execute este comando (certifique-se de que **webfrontend** é a sua pasta atual):

    ```cmd
    azds prep --public
    ```

O comando `azds prep` da CLI do Azure gera recursos do Docker e Kubernetes com as predefinições:
* `./Dockerfile` descreve a imagem de contentor da aplicação e como o código-fonte é construído e executado no contentor.
* Um [gráfico Helm](https://docs.helm.sh) em `./charts/webfrontend` descreve como implementar o contentor no Kubernetes.

Por enquanto, não é necessário entender o conteúdo completo desses ficheiros. No entanto, importa realçar que **os mesmos recursos de configuração como código do Kubernetes e do Docker podem ser utilizados do desenvolvimento à produção, dando maior consistência em diferentes ambientes.**
 
É também geral um ficheiro com o nome `./azds.yaml` pelo comando `prep`, que é o ficheiro de configuração do Azure Dev Spaces. Este complementa os artefactos do Docker e do Kubernetes com uma configuração adicional que permite uma experiência de desenvolvimento iterativa no Azure.
