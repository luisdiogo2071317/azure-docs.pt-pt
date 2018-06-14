---
title: incluir ficheiro
description: incluir ficheiro
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/20/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: b9484336add0719749e9f0af56bdd70fa3906ef5
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/23/2018
ms.locfileid: "29532346"
---
Para adicionar duas etiquetas para um grupo de recursos, utilize o [atualização az da grupo](/cli/azure/group#az_group_update) comando:

```azurecli-interactive
az group update -n myResourceGroup --set tags.Environment=Test tags.Dept=IT
```

Vamos imaginar que pretende adicionar uma etiqueta de terceira. Execute o comando novamente com a nova tag. É acrescentado a para as etiquetas existentes.

```azurecli-interactive
az group update -n myResourceGroup --set tags.Project=Documentation
```

Recursos não herdam as etiquetas do grupo de recursos. Atualmente, o grupo de recursos tem três etiquetas, mas os recursos não tem quaisquer etiquetas. Para aplicar todas as etiquetas de um grupo de recursos para os respetivos recursos e manter as etiquetas existentes nos recursos, utilize o seguinte script:

```azurecli-interactive
# Get the tags for the resource group
jsontag=$(az group show -n myResourceGroup --query tags)

# Reformat from JSON to space-delimited and equals sign
t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')

# Get the resource IDs for all resources in the resource group
r=$(az resource list -g myResourceGroup --query [].id --output tsv)

# Loop through each resource ID
for resid in $r
do
  # Get the tags for this resource
  jsonrtag=$(az resource show --id $resid --query tags)
  
  # Reformat from JSON to space-delimited and equals sign
  rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
  
  # Reapply the updated tags to this resource
  az resource tag --tags $t$rt --id $resid
done
```

Em alternativa, pode aplicar etiquetas do grupo de recursos para os recursos sem manter as etiquetas existentes:

```azurecli-interactive
# Get the tags for the resource group
jsontag=$(az group show -n myResourceGroup --query tags)

# Reformat from JSON to space-delimited and equals sign
t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')

# Get the resource IDs for all resources in the resource group
r=$(az resource list -g myResourceGroup --query [].id --output tsv)

# Loop through each resource ID
for resid in $r
do
  # Apply tags from resource group to this resource
  az resource tag --tags $t --id $resid
done
```

Combinar uma única tag de vários valores, utilize uma cadeia JSON.

```azurecli-interactive
az group update -n myResourceGroup --set tags.CostCenter='{"Dept":"IT","Environment":"Test"}'
```

Para remover todas as etiquetas num grupo de recursos, utilize:

```azurecli-interactive
az group update -n myResourceGroup --remove tags
```
