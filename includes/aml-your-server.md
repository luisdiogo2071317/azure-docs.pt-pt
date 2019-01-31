---
title: incluir ficheiro
description: incluir ficheiro
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 01/25/2019
ms.openlocfilehash: 18ba86ce7876ba8275eb4853e4fc9ea0f35fa186
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55302215"
---
1. Concluir o [guia de início rápido do Azure Machine Learning Python](../articles/machine-learning/service/quickstart-create-workspace-with-python.md) para instalar o SDK e criar uma área de trabalho.  Fique à vontade ignorar a **utilizar o bloco de notas** secção se desejar.
1. Clone o [repositório do GitHub](https://aka.ms/aml-notebooks).

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Adicione um ficheiro de configuração da área de trabalho com qualquer um dos seguintes métodos:
    * Copiar o **aml_config\config.json** ficheiro que criou com o guia de introdução de pré-requisitos para o diretório clonado.
    * Criar uma nova área de trabalho com o código na [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).
1. Inicie o servidor de blocos de notas a partir do diretório clonado.
    
    ```shell
    jupyter notebook
    ```