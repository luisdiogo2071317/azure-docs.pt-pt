---
title: Reenviar um serviço da web do Azure Machine Learning novo com o PowerShell | Documentos da Microsoft
description: Saiba como voltar a preparar um modelo e atualize o serviço web para utilizar o modelo treinado recentemente no Azure Machine Learning, utilizando os cmdlets do PowerShell de gestão do Machine Learning programaticamente.
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
ms.assetid: 3953a398-6174-4d2d-8bbd-e55cf1639415
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.openlocfilehash: 069a3022cf9b6423b95e8f9f35686965d2654be7
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39631083"
---
# <a name="retrain-a-new-resource-manager-based-web-service-using-the-machine-learning-management-powershell-cmdlets"></a>Reenviar um serviço da web baseados no Resource Manager novas com os cmdlets do PowerShell de gestão do Machine Learning
Quando voltar a preparar um novo serviço web, atualizar a definição de serviço web preditivo para referenciar o novo modelo treinado.

## <a name="prerequisites"></a>Pré-requisitos
Tem de configurar uma experimentação de preparação e de uma experimentação preditiva como mostrado na [Retrain Machine Learning dos modelos](retrain-models-programmatically.md).

> [!IMPORTANT]
> A experimentação preditiva tem de ser implementada como um Gestor de recursos do Azure (novo) com base em serviço web machine learning.
> Para implementar um novo serviço web tem de ter permissões suficientes na subscrição para a qual estiver a implementar o serviço web. Para obter mais informações, consulte [gerir um serviço Web através do portal do Azure Machine Learning Web Services](manage-new-webservice.md).

Para obter informações adicionais em Implantando serviços da web, consulte [implementar um serviço web do Azure Machine Learning](publish-a-machine-learning-web-service.md).

Este processo requer que instalou os Cmdlets do Azure Machine Learning. Para instalar os cmdlets de Machine Learning de informações, consulte a [Cmdlets do Azure Machine Learning](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/) referência no MSDN.

Copiar as seguintes informações a partir da saída reparametrização:

* BaseLocation
* RelativeLocation

Os passos que efetuar são:

1. Inicie sessão na sua conta do Azure Resource Manager.
2. Obter a definição de serviço web
3. Exportar a definição de serviço da Web como JSON
4. Atualize a referência para o blob de ilearner no JSON.
5. Importar o JSON para uma definição de serviço Web
6. Atualização do serviço web com a nova definição de serviço da Web

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Inicie sessão na sua conta do Azure Resource Manager
Primeiro tem de iniciar sessão sua conta do Azure de dentro do ambiente de PowerShell utilizando o [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet.

## <a name="get-the-web-service-definition"></a>Obter a definição de serviço Web
Em seguida, obter o serviço Web chamando o [Get-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/get-azurermmlwebservice) cmdlet. A definição de serviço Web é uma representação interna do modelo treinado do serviço web e não é modificável diretamente. Certifique-se de que estão a obter a definição de serviço da Web para a sua experimentação preditiva e não a sua experimentação de preparação.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Para determinar o nome do grupo de recursos de um serviço web existente, execute o cmdlet Get-AzureRmMlWebService sem quaisquer parâmetros para apresentar os serviços web na sua subscrição. Localize o serviço web e, em seguida, examinar sua ID de serviço web. O nome do grupo de recursos é o quarto elemento no ID, logo a seguir a *resourceGroups* elemento. No exemplo a seguir, o nome do grupo de recursos é a predefinição-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Em alternativa, para determinar o nome do grupo de recursos de um serviço web existente, inicie sessão no portal do Microsoft Azure Machine Learning Web Services. Selecione o serviço web. O nome do grupo de recursos é o elemento quinto do URL do serviço web, logo a seguir a *resourceGroups* elemento. No exemplo a seguir, o nome do grupo de recursos é a predefinição-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>Exportar a definição de serviço da Web como JSON
Para modificar a definição para o modelo preparado para utilizar o recentemente treinado modelo, primeiro tem de utilizar o [Export-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/export-azurermmlwebservice) cmdlet para os exportar para um ficheiro de formato JSON.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>Atualize a referência para o blob de ilearner no JSON.
No ativos, localize o [modelo preparado], atualize o *uri* valor no *locationInfo* nó com o URI do ilearner blob. O URI é gerado ao combinar a *BaseLocation* e o *RelativeLocation* da saída do BES reparametrização chamada. Este procedimento atualiza o caminho para referenciar o novo modelo treinado.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition"></a>Importar o JSON para uma definição de serviço Web
Tem de utilizar o [Import-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/import-azurermmlwebservice) cmdlet para converter o ficheiro JSON modificado novamente para uma definição de serviço Web que pode utilizar para atualizar a definição de serviço da Web.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>Atualização do serviço web com a nova definição de serviço da Web
Por último, utilize [AzureRmMlWebService atualização](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/update-azurermmlwebservice) cmdlet para atualizar a definição de serviço da Web.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Resumo
Utilizar os cmdlets de gerenciamento do PowerShell do Machine Learning, pode atualizar o modelo treinado de um serviço Web preditivo permitir cenários, tais como:

* Modelo periódico reparametrização com novos dados.
* Distribuição de um modelo para os clientes com o objetivo de informá-los de voltar a preparar o modelo usando seus próprios dados.

