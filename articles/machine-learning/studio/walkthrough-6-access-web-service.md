---
title: 'Passo 6: Aceder ao serviço web - Azure Machine Learning Studio | Documentos da Microsoft'
description: 'Passo 6 da desenvolver uma solução preditiva instruções: aceder a um serviço de web do Active Directory do Azure Machine Learning Studio.'
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 6a65c89a-40ab-4673-8dd8-8eee0a150e3b
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.openlocfilehash: 5c7db2c2c6e21197eac0296d2a9a42d447adfb66
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53091116"
---
# <a name="walkthrough-step-6-access-the-azure-machine-learning-studio-web-service"></a>Passo 6 das instruções: Aceder ao serviço de web do Azure Machine Learning Studio

Esta é a última etapa do passo a passo, [desenvolver uma solução de Análise Preditiva no Azure Machine Learning](walkthrough-develop-predictive-solution.md)

1. [Criar uma área de trabalho do Machine Learning](walkthrough-1-create-ml-workspace.md)
2. [Carregar os dados existentes](walkthrough-2-upload-data.md)
3. [Criar uma nova experimentação](walkthrough-3-create-new-experiment.md)
4. [Dar formação e avaliar os modelos](walkthrough-4-train-and-evaluate-models.md)
5. [Implementar o serviço Web](walkthrough-5-publish-web-service.md)
6. **Aceder ao serviço Web**

- - -
No passo anterior nestas instruções, Implementámos um serviço web que utiliza o nosso modelo de previsão do risco de crédito. Agora os utilizadores podem enviar dados para o mesmo e receba resultados. 

O serviço Web é um serviço web do Azure que possa receber e devolver dados com REST APIs de uma de duas formas:  

* **Pedido/resposta** - o utilizador envia uma ou mais linhas de dados de crédito para o serviço utilizando um protocolo HTTP e o serviço responde com um ou mais conjuntos de resultados.
* **Execução de lote** – o utilizador armazena um ou mais linhas de dados de crédito no Azure blob e, em seguida, envia a localização do blob para o serviço. O serviço pontua todas as linhas de dados no blob de entrada, armazena os resultados em outro blob e devolve o URL nesse contentor.  

A forma mais rápida e fácil para aceder a um serviço web clássico é através da [aplicação de Web do serviço de solicitação-resposta do Azure ML](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) ou [modelo de aplicação ao Web do serviço de execução do Azure ML Batch](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).

Estes modelos de aplicação web podem criar uma aplicação web personalizado que sabe que os dados de entrada do seu serviço da web e o que irá devolver. Tudo o que precisa fazer é fornecer acesso ao serviço web e aos dados e o modelo faz o resto.

Para obter mais informações sobre como utilizar os modelos de aplicação web, consulte [consumir um serviço Web do Azure Machine Learning com um modelo de aplicação web](consume-web-service-with-web-app-template.md).

Também pode desenvolver um aplicativo personalizado para aceder ao serviço da web usando o código de inicialização que lhe é fornecido em R, C#, e linguagens de programação Python.

Pode encontrar detalhes completos [como consumir um serviço Web do Azure Machine Learning](consume-web-services.md).

