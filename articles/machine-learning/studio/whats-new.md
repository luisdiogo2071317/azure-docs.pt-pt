---
title: O que há de novo - Azure Machine Learning Studio | Documentos da Microsoft
description: Novos recursos que estão disponíveis no Azure Machine Learning Studio.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: ddc716ed-2615-4806-bf27-6c9a5662a7f2
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2018
ms.openlocfilehash: 456e95540a69b4b7b504e98296f917db2826834e
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53082106"
---
# <a name="whats-new-in-azure-machine-learning-studio"></a>O que há de novo no Azure Machine Learning Studio?

## <a name="october-2018"></a>Outubro de 2018

O motor de linguagem R a [executar Script do R](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/execute-r-script) módulo adicionou uma versão de runtime do R novo – Microsoft R Open (MRO) 3.4.4. MRO 3.4.4 baseia-se no código-fonte aberto CRAN R 3.4.4 e, portanto, é compatível com os pacotes que funciona com essa versão do R.  Saiba mais sobre os pacotes R suportados no artigo, "[pacotes R suportados pelo Azure Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/r-packages-supported-by-azure-machine-learning#bkmk_List)".

## <a name="march-2017"></a>Março de 2017 
Nesta versão das atualizações do Microsoft Azure Machine Learning fornece a funcionalidade seguinte:

* Capacidade dedicada para as tarefas de BES do Azure Machine Learning

    Machine Learning Batch Pool processamento utiliza a [do Azure Batch](../../batch/batch-technical-overview.md) serviço para fornecer dimensionamento gerida pelo cliente para o serviço de execução de lote do Azure Machine Learning. Processamento de conjunto do batch permite-lhe criar conjuntos do Azure Batch no qual pode submeter tarefas do batch e as executa de forma previsível.

    Para obter mais informações, consulte [serviço Azure Batch para trabalhos de Machine Learning](dedicated-capacity-for-bes-jobs.md).


## <a name="august-2016"></a>Agosto de 2016 
Nesta versão das atualizações do Microsoft Azure Machine Learning fornecem as seguintes funcionalidades:
* Serviços Web clássicos, podem agora ser geridos no novo [serviços de Web do Microsoft Azure Machine Learning](https://services.azureml.net/) portal que fornece um local para gerir todos os aspetos do seu serviço Web.    
  * Que fornece o serviço web [estatísticas de utilização](manage-new-webservice.md).
  * Simplifica o teste de chamadas de remoto-pedido do Azure Machine Learning com dados de exemplo.
  * Fornece uma nova página de teste do serviço de execução do Batch com o histórico de envio de dados e de trabalho de exemplo.
  * Fornece uma gestão mais fácil do ponto final.

## <a name="july-2016"></a>Julho de 2016 
Nesta versão das atualizações do Microsoft Azure Machine Learning fornecem as seguintes funcionalidades:
* Serviços Web são agora geridos como recursos do Azure geridos através do [do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) interfaces, permitindo que para os seguintes melhoramentos:
  * Existem novas [REST APIs](https://msdn.microsoft.com/library/azure/Dn950030.aspx) implementar e gerir o seu Gestor de recursos com base em serviços da Web.
  * Existe um novo [serviços de Web do Microsoft Azure Machine Learning](https://services.azureml.net/) portal que fornece um local para gerir todos os aspetos do seu serviço Web.
* Incorpora um novo modelo de implementação de serviço web baseados em assinatura e em várias regiões com o Resource Manager com base em APIs aproveitando o fornecedor de recursos do Resource Manager para serviços da Web.
* Apresenta novos [planos de preços](https://azure.microsoft.com/pricing/details/machine-learning/) e planear os recursos de gerenciamento usando a RP de Gestor de recursos novo para faturação.
  * Agora, pode [implementar seu serviço web em várias regiões](how-to-deploy-to-multiple-regions.md) sem a necessidade de criar uma subscrição em cada região.
* Fornece o serviço web [estatísticas de utilização](manage-new-webservice.md).
* Simplifica o teste de chamadas de remoto-pedido do Azure Machine Learning com dados de exemplo.
* Fornece uma nova página de teste do serviço de execução do Batch com o histórico de envio de dados e de trabalho de exemplo.

Além disso, o Machine Learning Studio foi atualizado para permitir a implementação para o novo modelo de serviço da Web ou continuar a implementar o modelo de serviço Web clássico. 

