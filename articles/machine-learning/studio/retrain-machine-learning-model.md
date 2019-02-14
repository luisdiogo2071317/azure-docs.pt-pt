---
title: Voltar a preparar um modelo de Machine Learning Studio
titleSuffix: Azure Machine Learning Studio
description: Saiba como voltar a preparar um modelo e atualizar o serviço Web para utilizar o modelo treinado recentemente no Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 04/19/2017
ms.openlocfilehash: f7558876391d25d2f6f3dd1fede4cb0d13d72bf0
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56236265"
---
# <a name="retrain-an-azure-machine-learning-studio-model"></a>Voltar a preparar um modelo do Azure Machine Learning Studio
Como parte do processo de operacionalização de modelos de machine learning no Azure Machine Learning, o seu modelo é preparado e guardado. , Em seguida, utilizá-lo para criar um serviço Web preditivo. O serviço Web, em seguida, pode ser consumido em web sites, dashboards e aplicações móveis. 

Modelos de que criar com o Machine Learning, normalmente, não são estáticos. À medida que novos dados ficam disponíveis ou quando o consumidor da API tem seus próprios dados tem de reestruturar o modelo. 

Reparametrização pode ocorrer com frequência. Com o recurso de API de reparametrização programática, por meio de programação pode voltar a preparar o modelo com as APIs de reparametrização e atualize o serviço da Web com o modelo treinado recentemente. 

Este documento descreve o processo de reparametrização e mostra-lhe como utilizar as APIs de reparametrização.

## <a name="why-retrain-defining-the-problem"></a>Por que voltar a preparar: definir o problema
Como parte de processo de treinamento de aprendizagem automática, é preparado um modelo com um conjunto de dados. Modelos de que criar com o Machine Learning, normalmente, não são estáticos. À medida que novos dados ficam disponíveis ou quando o consumidor da API tem seus próprios dados tem de reestruturar o modelo.

Nestes cenários, uma API programática fornece uma forma conveniente de permitir o consumidor ou das APIs criar um cliente que pode, de forma única ou regular, voltar a preparar o modelo usando seus próprios dados. Em seguida, podem avaliar os resultados da reparametrização e atualizar a API de serviço da Web para utilizar o modelo treinado recentemente.

> [!NOTE]
> Se tiver uma experimentação de preparação existente e o serviço Web novo, pode querer confira reenviar um serviço Web preditivo existente em vez de seguir o passo a passo mencionada na secção seguinte.
> 
> 

## <a name="end-to-end-workflow"></a>Fluxo de trabalho ponto a ponto
O processo envolve os seguintes componentes: Uma experimentação de preparação e de uma experimentação preditiva publicaram como um serviço Web. Para ativar a reciclagem profissional um modelo preparado, a experimentação de preparação tem de ser publicada como um serviço Web com a saída de um modelo preparado. Isto permite o acesso à API para o modelo para a reparametrização. 

Os seguintes passos aplicam-se aos serviços de novo e Web clássico:

Crie o serviço Web preditivo inicial:

* Criar uma experimentação de preparação
* Criar uma experimentação preditiva web
* Implementar um serviço web preditivo

Voltar a preparar o serviço Web:

* Experimentação de preparação de atualização para permitir a reparametrização
* Implementar o serviço web reparametrização
* Utilize o código de serviço de execução do Batch para voltar a preparar o modelo

> [!NOTE] 
> Para implementar um novo serviço web tem de ter permissões suficientes na subscrição para a qual estiver a implementar o serviço web. Para obter mais informações, consulte [gerir um serviço Web através do portal do Azure Machine Learning Web Services](manage-new-webservice.md). 

Se tiver implementado um serviço Web clássico:

* Criar um novo ponto final do serviço Web preditivo
* Obter o URL de PATCHES e o código
* Utilize o URL de PATCHES para apontar o novo ponto final para o modelo retrained 

Caso se depare com dificuldades reparametrização um serviço Web clássico, veja [resolução de problemas relativos a novas preparações de um serviço Web clássico do Azure Machine Learning](troubleshooting-retraining-models.md).

Se tiver implementado um serviço Web novo:

* Inicie sessão na sua conta do Azure Resource Manager
* Obter a definição de serviço Web
* Exportar a definição de serviço da Web como JSON
* Atualizar a referência para o `ilearner` BLOBs no JSON
* Importar o JSON para uma definição de serviço Web
* Atualização do serviço Web com a nova definição de serviço da Web

O processo de configuração de reparametrização para um serviço Web clássico envolve os seguintes passos:

![Visão geral do processo de reparametrização][1]

O processo de configuração de reparametrização para um serviço Web novo envolve os seguintes passos:

![Visão geral do processo de reparametrização][7]

## <a name="other-resources"></a>Outros Recursos
* [Modelos de reparametrização e atualizar o Azure Machine Learning com o Azure Data Factory](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
* [Criar muitos modelos do Machine Learning e web pontos finais de serviço a partir de uma experiência com o PowerShell](create-models-and-endpoints-with-powershell.md)
* O [AML reparametrização modelos usando APIs](https://www.youtube.com/watch?v=wwjglA8xllg) vídeo mostra-lhe como voltar a preparar modelos de Machine Learning criados no Azure Machine Learning com o PowerShell e APIs de reparametrização.

<!--image links-->
[1]: ./media/retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

