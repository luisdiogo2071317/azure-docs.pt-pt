---
title: "Compreender os IDs de instância para as VMs de conjunto de dimensionamento de VM do Azure | Microsoft Docs"
description: "Compreender que as VMs de conjunto de IDs de instância para o dimensionamento de VM do Azure"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: negat
ms.openlocfilehash: 3a43dc86f1fb53dfde4bce3938faaa30e18f5a6d
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/02/2018
---
# <a name="understand-instance-ids-for-azure-vm-scale-set-vms"></a>Compreender que as VMs de conjunto de IDs de instância para o dimensionamento de VM do Azure
Este artigo descreve os IDs de instância para conjuntos de dimensionamento e as várias formas que superfície.

## <a name="scale-set-instance-ids"></a>Os IDs de instância de conjunto de dimensionamento

Cada VM num conjunto de dimensionamento obtém um ID de instância que identifica exclusivamente. Conjunto de VM na escala desta instância que ID é utilizado no conjunto de dimensionamento APIs para efetuar operações numa específica. Por exemplo, pode especificar um ID de instância específica a recriação de imagem quando utilizar a API de recriação de imagem:

API de REST: `POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/reimage?api-version={apiVersion}` (para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/reimage))

PowerShell: `Set-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName} -InstanceId {instanceId} -Reimage` (para obter mais informações, consulte o [Powershell documentação](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmssvm))

CLI: `az vmss reimage -g {resourceGroupName} -n {vmScaleSetName} --instance-id {instanceId}` (para obter mais informações, consulte o [documentação CLI](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_reimage)).

Pode obter a lista de IDs de instância enumerando todas as instâncias num conjunto de dimensionamento:

API de REST: `GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualMachines?api-version={apiVersion}` (para obter mais informações, consulte o [documentação da REST API](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesetvms/list))

PowerShell: `Get-AzureRmVmssVM -ResourceGroupName {resourceGroupName} -VMScaleSetName {vmScaleSetName}` (para obter mais informações, consulte o [Powershell documentação](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmssvm))

CLI: `az vmss list-instances -g {resourceGroupName} -n {vmScaleSetName}` (para obter mais informações, consulte o [documentação CLI](https://docs.microsoft.com/cli/azure/vmss?view=azure-cli-latest#az_vmss_list_instances)).

Também pode utilizar [resources.azure.com](https://resources.azure.com) ou [Azure SDKs](https://azure.microsoft.com/downloads/) para listar as VMs num conjunto de dimensionamento.

A apresentação de saída exata depende as opções de que fornecer ao comando, mas eis algumas saídas de exemplo a partir da CLI do:

```
$ az vmss show -g {resourceGroupName} -n {vmScaleSetName}
[
  {
    "instanceId": "85",
    "latestModelApplied": true,
    "location": "westus",
    "name": "nsgvmss_85",
    .
    .
    .
```

Como pode ver, a propriedade de "instanceId" é apenas um número decimal. A instância IDs pode ser reutilizada para novas instâncias depois instâncias antigas são eliminadas.

>[!NOTE]
> Não há **nenhuma garantia** na instância de forma IDs atribuídos às VMs no conjunto de dimensionamento. Estes poderão parecer sequencialmente crescente por vezes, mas tal nem sempre é o caso. Não execute uma dependência de forma específica na qual os IDs de instância são atribuídos às VMs.

## <a name="scale-set-vm-names"></a>Os nomes de VM de conjunto de dimensionamento

Na saída de exemplo acima, há também um "name" para a VM. Este nome assume a forma "{nome de conjunto de dimensionamento} _ {id de instância}". Este nome é aquele que vir no portal do Azure quando listar as instâncias num conjunto de dimensionamento:

![](./media/virtual-machine-scale-sets-instance-ids/vmssInstances.png)

A parte {id de instância} com o nome é o mesmo número decimal que a propriedade de "instanceId" discutido anteriormente.

## <a name="instance-metadata-vm-name"></a>Nome da VM de metadados de instância

Se consultar o [instância metadados](../virtual-machines/windows/instance-metadata-service.md) de dentro de um conjunto de dimensionamento VM, verá um "name" na saída:

```
{
  "compute": {
    "location": "westus",
    "name": "nsgvmss_85",
    .
    .
    .
```

Este nome é igual ao nome discutido anteriormente.

## <a name="scale-set-vm-computer-name"></a>Nome do computador VM de conjunto de dimensionamento

Cada VM com uma escala definir também obtém um nome de computador atribuído ao mesmo. Este nome de computador é o nome do anfitrião da VM no [resolução dentro da rede virtual de nomes de DNS fornecidos pelo Azure](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). Este nome de computador tem o formato "{computer-name-prefix}{base-36-instance-id}".

O {base-36--id de instância} está a ser [base 36](https://en.wikipedia.org/wiki/Base36) e é sempre seis dígitos de comprimento. Se a representação 36 base do número demora menos de seis dígitos, o {base-36--id de instância} será preenchido com zeros para torná-lo seis dígitos de comprimento. Por exemplo, uma instância com {prefixo de nome de computador} "nsgvmss" e de instância ID 85 tem computador. o nome "nsgvmss00002D".

>[!NOTE]
> O prefixo de nome do computador é uma propriedade do modelo de conjunto de dimensionamento, que pode definir, pelo que pode ser diferente do nome do conjunto de dimensionamento em si.