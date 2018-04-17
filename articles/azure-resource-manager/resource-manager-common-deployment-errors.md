---
title: Resolver erros comuns de implementação do Azure | Microsoft Docs
description: Descreve como resolver erros comuns ao implementar recursos do Azure com o Azure Resource Manager.
services: azure-resource-manager
documentationcenter: ''
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: Erro de implementação, a implementação do azure, implementar no azure
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/08/2018
ms.author: tomfitz
ms.openlocfilehash: 01cc75c522b85f7b4a4b38c92f42cb113d016b73
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Resolver erros comuns de implementação do Azure com o Azure Resource Manager

Este artigo descreve alguns erros de implementação do Azure comuns que poderá encontrar e fornece informações para resolver os erros. Se não é possível localizar o código de erro para o erro de implementação, consulte o artigo [localizar o código de erro](#find-error-code).

## <a name="error-codes"></a>Códigos de erro

| Código de erro | Mitigação | Mais informações |
| ---------- | ---------- | ---------------- |
| AccountNameInvalid | Siga as restrições de nomenclatura para contas de armazenamento. | [Resolver o nome de conta de armazenamento](resource-manager-storage-account-name-errors.md) |
| AccountPropertyCannotBeSet | Verifique as propriedades da conta de armazenamento disponível. | [storageAccounts](/azure/templates/microsoft.storage/storageaccounts) |
| AllocationFailed | O cluster ou a região não dispõe de recursos disponíveis ou não suporta o tamanho da VM pedido. Repetir o pedido num momento posterior, ou peça um tamanho VM diferente. | [Problemas de aprovisionamento e a atribuição de Linux](../virtual-machines/linux/troubleshoot-deployment-new-vm.md), [problemas de aprovisionamento e a alocação para o Windows](../virtual-machines/windows/troubleshoot-deployment-new-vm.md) e [resolver problemas de falhas de atribuição](../virtual-machines/windows/allocation-failure.md)|
| AnotherOperationInProgress | Aguarde pela conclusão da operação em simultâneo. | |
| AuthorizationFailed | A conta ou principal de serviço não tem acesso suficiente para concluir a implementação. Verifique a sua conta pertencer a função e o acesso para o âmbito da implementação. | [Controlo de acesso baseado em funções do Azure](../role-based-access-control/role-assignments-portal.md) |
| BadRequest | Enviado valores de implementação que não corresponde ao que é esperado pelo Gestor de recursos. Consulte a mensagem de estado interna para obter ajuda na resolução de problemas. | [Referência de modelo](/azure/templates/) e [suportado localizações](resource-manager-templates-resources.md#location) |
| Conflito | Está a pedir uma operação que não é permitida no estado atual do recurso. Por exemplo, redimensionamento de disco é permitido apenas quando criar uma VM ou quando a VM é desalocada. | |
| DeploymentActive | Aguarde pela implementação simultânea para este grupo de recursos para concluir. | |
| DeploymentFailed | O erro de DeploymentFailed é um erro geral que não fornece os detalhes que tem de resolver o erro. Consulte os detalhes do erro para um código de erro que fornece mais informações. | [Localizar o código de erro](#find-error-code) |
| DeploymentQuotaExceeded | Se atingir o limite de 800 implementações por grupo de recursos, elimine as implementações de histórico que já não são necessárias. Pode eliminar as entradas do histórico com [eliminar a implementação do grupo az](/cli/azure/group/deployment#az_group_deployment_delete) para a CLI do Azure, ou [Remove-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/remove-azurermresourcegroupdeployment) no PowerShell. Eliminar uma entrada do histórico de implementação não afeta os recursos de implementar. | |
| DnsRecordInUse | O nome do registo DNS tem de ser exclusivo. Fornecer um nome diferente ou modifique o registo existente. | |
| ImageNotFound | Verifique as definições de imagem VM. |  |
| InUseSubnetCannotBeDeleted | Pode encontrar este erro quando tentar atualizar um recurso, mas o processamento do pedido por eliminar e criar o recurso. Certifique-se especificar todos os valores não alterados. | [Atualizar recurso](/azure/architecture/building-blocks/extending-templates/update-resource) |
| InvalidAuthenticationTokenTenant | Obter acesso token para o inquilino adequado. Só pode obter o token do inquilino que a conta pertence. | |
| InvalidContentLink | É muito provável que foi efetuada uma tentativa ligar a um modelo aninhado que não está disponível. Verifique novamente o URI fornecido para o modelo aninhado. Se o modelo existe numa conta do storage, certificar-se de que o URI está acessível. Poderá ter de transmitir um token SAS. | [modelos ligados](resource-group-linked-templates.md) |
| InvalidParameter | Um dos valores fornecidos para um recurso não corresponde ao valor esperado. Este erro poderá resultar de várias condições diferentes. Por exemplo, uma palavra-passe pode ser insuficiente, ou um nome de blob poderá estar incorreto. Consulte a mensagem de erro para determinar qual o valor tem de ser corrigido. | |
| InvalidRequestContent | Os valores de implementação a incluir valores que não são esperados ou estão em falta necessário valores. Confirme os valores para o tipo de recurso. | [Referência de modelo](/azure/templates/) |
| InvalidRequestFormat | Ativar o registo de depuração ao executar a implementação e verificar o conteúdo do pedido. | [O registo de depuração](#enable-debug-logging) |
| InvalidResourceNamespace | Verificar o espaço de nomes de recurso especificado no **tipo** propriedade. | [Referência de modelo](/azure/templates/) |
| InvalidResourceReference | O recurso ainda não existe ou está incorretamente referenciado. Verifique se tem de adicionar uma dependência. Certifique-se de que a utilização de **referência** função inclui os parâmetros necessários para o seu cenário. | [Resolver dependências do](resource-manager-not-found-errors.md) |
| InvalidResourceType | Verifique o recurso de tipo que especificou no **tipo** propriedade. | [Referência de modelo](/azure/templates/) |
| InvalidSubscriptionRegistrationState | Registe a sua subscrição com o fornecedor de recursos. | [Resolver o registo](resource-manager-register-provider-errors.md) |
| InvalidTemplate | Verifique a sintaxe do modelo de erros. | [Resolver modelo inválido](resource-manager-invalid-template-errors.md) |
| InvalidTemplateCircularDependency | Remova dependências desnecessárias. | [Resolver dependências circulares](resource-manager-invalid-template-errors.md#circular-dependency) |
| LinkedAuthorizationFailed | Verifique se a sua conta pertencer ao mesmo inquilino como o grupo de recursos que está a implementar. | |
| LinkedInvalidPropertyId | O ID de recurso para um recurso não está a resolver corretamente. Certifique-se de que forneça valores todos requeridos para o ID de recurso, incluindo o ID de subscrição, nome do grupo de recursos, tipo de recurso, nome de recurso principal (se necessário) e nome do recurso. | |
| LocationRequired | Forneça uma localização para o seu recurso. | [Definir localização](resource-manager-templates-resources.md#location) |
| MismatchingResourceSegments | Certifique-se de que aninhada recurso tem o número correto de segmentos de nome e tipo. | [Resolva os segmentos de recursos](resource-manager-invalid-template-errors.md#incorrect-segment-lengths)
| MissingRegistrationForLocation | Verifique o estado de registo do fornecedor de recursos e localizações suportadas. | [Resolver o registo](resource-manager-register-provider-errors.md) |
| MissingSubscriptionRegistration | Registe a sua subscrição com o fornecedor de recursos. | [Resolver o registo](resource-manager-register-provider-errors.md) |
| NoRegisteredProviderFound | Verifique o estado de registo do fornecedor de recursos. | [Resolver o registo](resource-manager-register-provider-errors.md) |
| NãoLocalizado | Poderá estar a tentar implementar um recurso dependente em paralelo com um recurso principal. Verifique se tem de adicionar uma dependência. | [Resolver dependências do](resource-manager-not-found-errors.md) |
| OperationNotAllowed | A implementação está a tentar uma operação que exceda a quota para a subscrição, o grupo de recursos ou a região. Se for possível, de rever a implementação para se manter nas quotas. Caso contrário, considere solicitar uma alteração à sua quotas. | [Resolver quotas](resource-manager-quota-errors.md) |
| ParentResourceNotFound | Certifique-se de que existe um recurso de principal antes de criar o elemento subordinado recursos. | [Resolver o recurso principal](resource-manager-parent-resource-errors.md) |
| PrivateIPAddressInReservedRange | O endereço IP especificado inclui um intervalo de endereços necessário pelo Azure. Alterar o endereço IP para evitar o intervalo reservado. | [Endereços IP](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PrivateIPAddressNotInSubnet | O endereço IP especificado está fora do intervalo de sub-rede. Alterar o endereço IP para coincidir com o intervalo de sub-rede. | [Endereços IP](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |
| PropertyChangeNotAllowed | Algumas propriedades não podem ser alteradas num recurso implementado. Ao atualizar um recurso, limite as suas alterações às propriedades permitidas. | [Atualizar recurso](/azure/architecture/building-blocks/extending-templates/update-resource) |
| RequestDisallowedByPolicy | A subscrição inclui uma política de recurso que impeça uma ação que está a tentar efetuar durante a implementação. Localize a política que bloqueia a ação. Se possível, modifique a sua implementação para satisfazer as limitações da política. | [Resolver políticas](resource-manager-policy-requestdisallowedbypolicy-error.md) |
| ReservedResourceName | Forneça um nome de recurso que não inclua um nome reservado. | [Nomes de recursos reservados](resource-manager-reserved-resource-name.md) |
| ResourceGroupBeingDeleted | Aguarde pela eliminação concluir. | |
| ResourceGroupNotFound | Verifique o nome do grupo de recursos de destino para a implementação. Já deve existir na sua subscrição. Verifique o contexto de subscrição. | [CLI do Azure](/cli/azure/account?#az_account_set) [PowerShell](/powershell/module/azurerm.profile/set-azurermcontext) |
| ResourceNotFound | A implementação referencia um recurso que não é possível resolver. Certifique-se de que a utilização de **referência** função inclui os parâmetros necessários para o seu cenário. | [Resolver referências](resource-manager-not-found-errors.md) |
| ResourceQuotaExceeded | A implementação está a tentar criar recursos excedem a quota para a subscrição, o grupo de recursos ou a região. Se for possível, rever a sua infraestrutura de permanecer nas quotas. Caso contrário, considere solicitar uma alteração à sua quotas. | [Resolver quotas](resource-manager-quota-errors.md) |
| SkuNotAvailable | Selecione o SKU (por exemplo, o tamanho da VM) que está disponível para a localização que selecionou. | [Resolver SKU](resource-manager-sku-not-available-errors.md) |
| StorageAccountAlreadyExists | Forneça um nome exclusivo para a conta de armazenamento. | [Resolver o nome de conta de armazenamento](resource-manager-storage-account-name-errors.md)  |
| StorageAccountAlreadyTaken | Forneça um nome exclusivo para a conta de armazenamento. | [Resolver o nome de conta de armazenamento](resource-manager-storage-account-name-errors.md) |
| StorageAccountNotFound | Verifique a subscrição, o grupo de recursos e o nome da conta de armazenamento que está a tentar utilizar. | |
| SubnetsNotInSameVnet | Uma máquina virtual só pode ter uma rede virtual. Quando implementar vários NICs, certifique-se de que pertencem à mesma rede virtual. | [Vários NICs](../virtual-machines/windows/multiple-nics.md) |
| TemplateResourceCircularDependency | Remova dependências desnecessárias. | [Resolver dependências circulares](resource-manager-invalid-template-errors.md#circular-dependency) |
| TooManyTargetResourceGroups | Reduza o número de grupos de recursos para uma única implementação. | [Implementação entre grupos de recursos](resource-manager-cross-resource-group-deployment.md) |

## <a name="find-error-code"></a>Localizar o código de erro

Existem dois tipos de erros que pode receber:

* Erros de validação
* Erros de implementação

Erros de validação resultam de cenários que podem ser determinados antes da implementação. Incluem erros de sintaxe no seu modelo, ou tentar implementar recursos que iriam exceder as quotas de subscrição. Erros de implementação resultam de condições que ocorrem durante o processo de implementação. Incluem a tentar aceder a um recurso que está a ser implementado em paralelo.

Ambos os tipos de erros de devolverem um código de erro que utilizar para resolver problemas relacionados com a implementação. Ambos os tipos de erros são apresentados no [registo de atividade](resource-group-audit.md). No entanto, erros de validação não aparecem no seu histórico de implementação porque a implementação nunca foi iniciada.

### <a name="validation-errors"></a>Erros de validação

Quando implementar através do portal, verá um erro de validação depois de submeter os seus valores.

![Mostrar Erro de validação de portal](./media/resource-manager-common-deployment-errors/validation-error.png)

Selecione a mensagem para obter mais detalhes. Na imagem seguinte, verá um **InvalidTemplateDeployment** erro e uma mensagem que indica uma política de implementação está bloqueado.

![Mostrar detalhes de validação](./media/resource-manager-common-deployment-errors/validation-details.png)

### <a name="deployment-errors"></a>Erros de implementação

Quando a operação ser aprovado na validação, mas falha durante a implementação, consulte o erro nas notificações. Selecione a notificação.

![Erro de notificação](./media/resource-manager-common-deployment-errors/notification.png)

Consulte mais detalhes sobre a implementação. Selecione a opção para encontrar mais informações sobre o erro.

![falhada de implementação](./media/resource-manager-common-deployment-errors/deployment-failed.png)

Consulte a mensagem de erro e códigos de erro. Tenha em atenção de que existem dois códigos de erro. O código de erro primeiro (**DeploymentFailed**) é um erro geral que não fornece os detalhes tem de resolver o erro. O segundo código de erro (**StorageAccountNotFound**) fornece os detalhes que precisa. 

![detalhes do erro](./media/resource-manager-common-deployment-errors/error-details.png)

## <a name="enable-debug-logging"></a>Ativar o registo de depuração

Por vezes, precisa de mais informações sobre o pedido e resposta para saber o que aconteceu de errado. Ao utilizar o PowerShell ou a CLI do Azure, pode pedir que informações adicionais são registadas durante uma implementação.

- PowerShell

   No PowerShell, defina o **DeploymentDebugLogLevel** parâmetro a todos os, ResponseContent ou RequestContent.

  ```powershell
  New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All
  ```

   Examine o pedido de conteúdo com o seguinte cmdlet:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json
  ```

   Em alternativa, a resposta conteúda com:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json
  ```

   Estas informações podem ajudar a determinar se um valor no modelo está a ser definido incorretamente.

- CLI do Azure

   Examine as operações de implementação com o seguinte comando:

  ```azurecli
  az group deployment operation list --resource-group ExampleGroup --name vmlinux
  ```

- Modelo aninhado

   Para registar informações de depuração para um modelo aninhado, utilize o **debugSetting** elemento.

  ```json
  {
      "apiVersion": "2016-09-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri": "{template-uri}",
              "contentVersion": "1.0.0.0"
          },
          "debugSetting": {
             "detailLevel": "requestContent, responseContent"
          }
      }
  }
  ```

## <a name="create-a-troubleshooting-template"></a>Criar um modelo de resolução de problemas

Em alguns casos, a forma mais fácil para resolver o seu modelo é testar partes do mesmo. Pode criar um simplificada modelo permite-lhe focar-se na parte que achar está a causar o erro. Por exemplo, suponha que está a receber um erro ao referenciar um recurso. Em vez de lidar com um modelo completo, crie um modelo que devolve a parte que poderá estar a causar o problema. Pode ajudar a determinar se estão a ser transmitidos nos parâmetros do direitos, utilizando as funções de modelo corretamente, e ao obter o recurso que esperava.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageName": {
        "type": "string"
    },
    "storageResourceGroup": {
        "type": "string"
    }
  },
  "variables": {},
  "resources": [],
  "outputs": {
    "exampleOutput": {
        "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageName')), '2016-05-01')]",
        "type" : "object"
    }
  }
}
```

Ou, suponha que tiver com erros de implementação que achar que estão relacionados com incorretamente dependências. Teste o modelo ao eliminá-lo para modelos simplificados. Em primeiro lugar, crie um modelo que implementa apenas um único recurso (como um SQL Server). Quando tiver a certeza de que tem esse recurso corretamente definido, adicione um recurso que depende (tal como uma base de dados do SQL Server). Quando tiver esses dois recursos corretamente definidos, adicione outros recursos dependentes (como políticas de auditoria). Entre cada implementação de teste, elimine o grupo de recursos para se certificar de que tem as dependências de teste.


## <a name="next-steps"></a>Passos Seguintes
* Para saber mais sobre as ações de auditoria, consulte [auditar operações com o Resource Manager](resource-group-audit.md).
* Para saber mais sobre as ações para determinar os erros durante a implementação, consulte [ver as operações de implementação](resource-manager-deployment-operations.md).
