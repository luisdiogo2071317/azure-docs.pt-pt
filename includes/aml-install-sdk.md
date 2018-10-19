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
ms.date: 07/27/2018
ms.openlocfilehash: 12f0f17a7c25696d2c8ff5b427f4b103617e5678
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49431524"
---
Numa janela de linha de comandos ou shell, crie e ative o ambiente do gestor de pacotes conda com numpy e cython. Este exemplo utiliza o Python 3.6.

  + No Windows:
       ```sh 
       conda create -n myenv Python=3.6 cython numpy
       conda activate myenv
       ```

  + No Linux ou MacOS:
       ```sh 
       conda create -n myenv Python=3.6 cython numpy
       source activate myenv
       ```

Instale o SDK.
   ```sh 
   pip install azureml-sdk
   ```
