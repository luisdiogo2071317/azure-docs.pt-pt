---
title: Implementar a API do Azure para FHIR com o PowerShell
description: Implemente a API do Azure para FHIR com o PowerShell.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: quickstart
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 0cea06da1815fa1128b16f1427690141823e82d8
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823570"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-powershell"></a>Início rápido: Implementar a API do Azure para FHIR com o PowerShell

Neste início rápido, irá aprender como implantar a API do Azure para FHIR com o PowerShell.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

## <a name="register-the-azure-api-for-fhir-resource-provider"></a>Registar a API do Azure para o fornecedor de recursos FHIR

Se o `Microsoft.HealthcareApis` fornecedor de recursos já não está registado para a sua subscrição, pode registá-lo com:

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.HealthcareApis
```

## <a name="create-azure-resource-manager-template"></a>Criar o modelo Azure Resource Manager

Crie um modelo Azure Resource Manager com o seguinte conteúdo:

[!code-json[](samples/azuredeploy.json)]

Guarde-o com o nome `azuredeploy.json`

## <a name="create-azure-resource-manager-parameter-file"></a>Criar ficheiro de parâmetros do Azure Resource Manager

Crie um ficheiro de parâmetros de modelo do Azure Resource Manager com o seguinte conteúdo:

[!code-json[](samples/azuredeploy.parameters.json)]

Guarde-o com o nome `azuredeploy.parameters.json`

Os valores de ID de objeto `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` e `yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy` correspondem ao objeto de IDs de utilizadores específicos do Azure Active Directory ou principais no diretório associado à subscrição de serviço. Se quiser saber o ID de objeto de um utilizador específico, pode encontrá-lo com um comando como:

```azurepowershell-interactive
$(Get-AzureADUser -Filter "UserPrincipalName eq 'myuser@consoso.com'").ObjectId
```

Continue a ler o guia de procedimentos [encontrar IDs de objeto de identidade](find-identity-object-ids.md) para obter mais detalhes.

## <a name="create-azure-resource-group"></a>Criar grupo de recursos do Azure

```azurepowershell-interactive
$rg = New-AzureRmResourceGroup -Name "myResourceGroupName" -Location westus2
```

## <a name="deploy-template"></a>Implementar o modelo

```azurepowershell-interactive
New-AzureRmResourceGroupDeployment -ResourceGroup $rg.ResourceGroupName -TemplateFile azuredeploy.json -TemplateParameterFile azuredeploy.parameters.json
```

## <a name="fetch-capability-statement"></a>Obter a declaração de capacidade

Será capaz de validar que a API do Azure para a conta FHIR está em execução, buscando uma instrução de capacidade FHIR:

```azurepowershell-interactive
$metadata = Invoke-WebRequest -Uri $metadataUrl "https://nameOfFhirAccount.azurehealthcareapis.com/metadata"
$metadata.RawContent
```

## <a name="clean-up-resources"></a>Limpar recursos

Se não pretender continuar a utilizar esta aplicação, elimine o grupo de recursos com os seguintes passos:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name $rg.ResourceGroupName
```

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, implementou a API do Azure para FHIR na sua subscrição. Para saber como aceder à API de FHIR com o Postman, avance para o tutorial do Postman.

>[!div class="nextstepaction"]
>[Aceda à API de FHIR com o Postman](access-fhir-postman-tutorial.md)