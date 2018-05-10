---
title: FPGAs e de aprendizagem do Azure
description: Saiba como acelerar as redes neurais profundo com FPGAs e modelos.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 05/07/2018
ms.openlocfilehash: 493b3f8de617146af534349e18ed49b83c46dee8
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="what-is-a-field-programmable-gate-array-fpga"></a>O que é uma matriz de porta programável campo (FPGA)?

As matrizes de porta programável campo (FPGA) são circuitos integrado que podem ser reconfigurados conforme necessário. Pode alterar um FPGA conforme necessário para implementar a lógica de novo. Azure Machine Learning Hardware acelerados modelos permitem-lhe implementar modelos de formação FPGAs na nuvem do Azure.

Esta funcionalidade está ligada ao projeto Brainwave, que processa traduzir as redes neurais profundo (DNN) para um programa FPGA. 

## <a name="why-use-an-fpga"></a>Porquê utilizar um FPGA?

FPGAs fornecer ultra-baixa latência e são particularmente eficazes para a classificação de dados no tamanho do lote um (não é necessário para um tamanho de lote grande).  Estão disponíveis no Azure e económica.  Projeto Brainwave pode parallelize DNNs previamente treinados entre estes FPGAs para aumentar horizontalmente o seu serviço.

## <a name="next-steps"></a>Passos Seguintes

[Implementar um modelo como um serviço web num FPGA](how-to-deploy-fpga-web-service.md)

[Saiba como instalar o SDK FPGA](reference-fpga-package-overview.md)