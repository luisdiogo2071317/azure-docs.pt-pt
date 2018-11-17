---
title: Módulo do PowerShell para Machine Learning | Documentos da Microsoft
description: O módulo do PowerShell para Azure Machine Learning está disponível no modo de pré-visualização pública. Utilize o PowerShell para criar e gerir áreas de trabalho, experiências, serviços da web e muito mais.
keywords: experimentação, regressão linear, algoritmos de machine learning, tutorial de machine learning, técnicas de modelação preditiva, experiência de ciência de dados
services: machine-learning
documentationcenter: ''
author: hning86
ms.custom: (previous ms.author haining)
ms.author: amlstudiodocs
manager: mwinkle
editor: cgronlun
ms.assetid: a9001cc2-3aa0-47e1-b175-1f76408ba1d1
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.openlocfilehash: c46b99de8a93c54246d52a3cb369fd926e7c60cd
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/16/2018
ms.locfileid: "51822046"
---
# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>Módulo PowerShell para Microsoft Azure Machine Learning
O módulo do PowerShell para o Azure Machine Learning é uma ferramenta poderosa que permite-lhe utilizar o Windows PowerShell para gerir áreas de trabalho, experiências, conjuntos de dados, serviços web clássicos e muito mais.

Pode ver a documentação e transferir o módulo, juntamente com o código-fonte completo, em [ https://aka.ms/amlps ](https://aka.ms/amlps). 

> [!NOTE]
> O módulo do PowerShell do Azure Machine Learning está atualmente no modo de pré-visualização. O módulo irá continuar a ser melhorado e expandido durante este período de pré-visualização. Esteja atento a [do Cortana Intelligence e o blogue de Machine Learning](https://blogs.technet.microsoft.com/machinelearning/) para notícias e informações.

## <a name="what-is-the-machine-learning-powershell-module"></a>O que é o módulo PowerShell do Machine Learning?
O módulo do PowerShell do Machine Learning é um. Com base em NET módulo DLL que permite-lhe gerir totalmente as áreas de trabalho do Azure Machine Learning, experiências, conjuntos de dados, serviços web clássicos e pontos finais de serviço do web clássico do Windows PowerShell. 

Juntamente com o módulo, pode baixar o código-fonte completo que inclui um corretamente separados [ C# camada de API](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs). Pode referenciar esta DLL a partir do seu próprio projeto .NET e gerir o Azure Machine Learning através de código do .NET. Além disso, a DLL depende das APIs REST subjacente que pode utilizar diretamente a partir do seu cliente favorito.

## <a name="what-can-i-do-with-the-powershell-module"></a>O que posso fazer com o módulo do PowerShell?
Seguem-se algumas das tarefas que pode realizar com este módulo do PowerShell. Consultar [documentação completa](https://aka.ms/amlps) destas e de muitas outras funções.

* Aprovisionar uma nova área de trabalho utilizando um certificado de gestão ([New- AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace))
* Exportar e importar um ficheiro JSON que representa um gráfico de experimentação ([Export-AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) e [Import-AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
* Executar uma experiência ([Start-AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
* Criar um serviço Web a partir de uma experiência preditiva ([New-AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
* Criar um ponto final no serviço web publicado ([Add-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
* Invocar um ponto final de serviço Web RRS e/ou BES ([Invoke-AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) e [Invoke-AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Eis um exemplo rápido de como utilizar o PowerShell para executar uma experiência existente:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Para um caso de utilização mais aprofundado, consulte este artigo sobre como utilizar o módulo do PowerShell para automatizar uma tarefa normalmente pedida: [criar muitos modelos do Machine Learning e web pontos finais de serviço a partir de uma experiência com o PowerShell](create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Como posso começar?
Para começar a utilizar o PowerShell do Machine Learning, transfira o [pacote da versão](https://github.com/hning86/azuremlps/releases) a partir do GitHub e siga as [instruções de instalação](https://github.com/hning86/azuremlps/blob/master/README.md). As instruções explicam como desbloquear a DLL transferida/descompactada e, em seguida, importe-o para o ambiente do PowerShell. A maioria dos cmdlets requerem que forneça o ID da área de trabalho, o token de autorização da área de trabalho e a região do Azure onde está a área de trabalho. A maneira mais simples de fornecer os valores é por meio de um ficheiro Config JSON predefinido. As instruções também explicam como configurar este ficheiro. 

Se desejar, pode clonar a árvore de git, modificar o código e compilá-lo localmente usando o Visual Studio.

## <a name="next-steps"></a>Passos Seguintes
Pode encontrar a documentação completa para o módulo do PowerShell no [ https://aka.ms/amlps ](https://aka.ms/amlps). 

Para obter um exemplo expandido de como utilizar o módulo num cenário do mundo real, confira o caso de utilização detalhada [criar muitos modelos do Machine Learning e web pontos finais de serviço a partir de uma experiência com o PowerShell](create-models-and-endpoints-with-powershell.md).
