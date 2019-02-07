---
title: Implementar o servidor de Open FHIR de origem para o Azure com o PowerShell - APIs de cuidados de saúde Microsoft
description: Este início rápido explica como implementar o servidor de Microsoft Open FHIR de origem com o PowerShell.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 6118c9371df7aa0af685390dae28bceb496145d4
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823559"
---
# <a name="quickstart-deploy-open-source-fhir-server-using-powershell"></a>Início rápido: Implementar o servidor Open FHIR de origem com o PowerShell

Neste início rápido, irá aprender como implantar o servidor Open FHIR de Microsoft de origem para utilizar o Azure PowerShell.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Escolha um nome para o grupo de recursos que irá conter os recursos aprovisionados e criá-la:

```azurepowershell-interactive
$fhirServiceName = "MyFhirService"
$rg = New-AzureRmResourceGroup -Name $fhirServiceName -Location westus2
```

## <a name="deploy-the-fhir-server-template"></a>Implementar o modelo de servidor FHIR

O servidor de FHIR da Microsoft para o Azure [repositório do GitHub](https://github.com/Microsoft/fhir-server) contém um modelo que irá implementar todos os recursos necessários. Implemente-a com:

```azurepowershell-interactive
New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Microsoft/fhir-server/master/samples/templates/default-azuredeploy.json -ResourceGroupName $rg.ResourceGroupName -serviceName $fhirServiceName `
```

## <a name="verify-fhir-server-is-running"></a>Certifique-se de que o servidor FHIR está em execução

```azurepowershell-interactive
$metadataUrl = "https://" + $fhirServiceName + ".azurewebsites.net/metadata" 
$metadata = Invoke-WebRequest -Uri $metadataUrl
$metadata.RawContent
```

Irá demorar um minuto ou para o servidor responda pela primeira vez.

## <a name="clean-up-resources"></a>Limpar recursos

Se não pretender continuar a utilizar esta aplicação, elimine o grupo de recursos com os seguintes passos:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name $rg.ResourceGroupName
```

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, implementou o Microsoft Open FHIR servidor de origem para o Azure na sua subscrição. Para saber como aceder à API de FHIR com o Postman, avance para o tutorial do Postman.
 
>[!div class="nextstepaction"]
>[Aceda à API de FHIR com o Postman](access-fhir-postman-tutorial.md)