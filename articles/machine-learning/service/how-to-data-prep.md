---
title: Preparar dados com o SDK de preparação de dados do Machine Learning para Python - Azure
description: Saiba como utilizar o Azure Machine Learning dados Prep SDK para Python para carregar dados de vários formatos, transformá-lo para ser mais utilizável e gravá-los para uma localização para os seus modelos aceder.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 09/24/2018
ms.openlocfilehash: 83373f5d6e4a2227bcafdbc4b25b686b0b8d651d
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867192"
---
# <a name="prepare-data-for-modeling-with-azure-machine-learning"></a>Preparar dados para modelagem com o Azure Machine Learning
 
Preparação de dados é uma parte importante de um fluxo de trabalho do machine learning. Os modelos será mais preciso e eficiente se tiverem acesso ao limpar dados num formato mais fácil de consumir. 

Pode se preparar seus dados em Python com o [SDK do Azure Machine Learning Data Prep](https://docs.microsoft.com/python/api/overview/azure/dataprep?view=azure-dataprep-py). 

## <a name="data-preparation-pipeline"></a>Pipeline de preparação de dados

Os passos de preparação de dados principais são:

1. [Carregar dados](how-to-load-data.md), que pode ser em vários formatos
2. [Transformar](how-to-transform-data.md) -lo numa estrutura mais utilizável
3. [Escrever](how-to-write-data.md) esses dados para uma localização acessível aos seus modelos

![Processo de preparação de dados](./media/concept-data-preparation/data-prep-process.png)

## <a name="next-steps"></a>Passos Seguintes
Revisão de um [bloco de notas do exemplo](https://github.com/Microsoft/MSDataPrepDocs/blob/master/Scenarios/GettingStarted/getting-started.ipynb) de preparação de dados com o SDK do Azure Machine Learning Data Prep.
