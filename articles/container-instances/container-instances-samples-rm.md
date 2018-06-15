---
title: Amostras de modelo do Azure Resource Manager - instâncias de contentor do Azure
description: Exemplos de modelo do Azure Resource Manager para instâncias de contentor do Azure
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 05/17/2018
ms.author: marsma
ms.openlocfilehash: fcc2e6c52e773d95bcdfe43d881fce036fae6513
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34260169"
---
# <a name="azure-resource-manager-templates-for-azure-container-instances"></a>Modelos Azure Resource Manager para instâncias de contentor do Azure

Os modelos de exemplo seguintes implementar instâncias de contentor em diversas configurações.

Para opções de implementação, consulte o [implementação](#deployment) secção. Se gostaria de criar os seus próprios modelos, as instâncias de contentor do Azure [referência de modelo do Resource Manager] [ ref] fornece detalhes sobre o formato de modelo e propriedades disponíveis.

## <a name="sample-templates"></a>Modelos de exemplo

| | |
|-|-|
| **Aplicações** ||
| [Wordpress][app-wp] | Cria um Web site WordPress e a respetiva base de dados MySQL numa instância de contentor. O conteúdo de site do WordPress e a base de dados MySQL são mantidas a um Azure Files partilhar. |
| [NAV MS SQL Server e IIS][app-nav] | Implementa um único contentor do Windows com um excelentes Dynamics NAV autónomo / ambiente Dynamics 365 comercial Central. |
| **Volumes** ||
| [emptyDir][vol-emptydir] | Implementa dois contentores de Linux que partilham um volume emptyDir. |
| [GitRepo][vol-gitrepo] | Implementa um contentor de Linux que clones um repositório do GitHub e monta-lo como um volume. |
| [segredo][vol-secret] | Implementa um contentor de Linux com um certificado PFX montado como um volume secreto. |
| **Redes** ||
| [Contentor expostos de UDP][net-udp] | Implementa um contentor do Windows ou Linux que expõe uma porta UDP. |
| [Contentor de Linux com o IP público][net-publicip] | Implementa um único contentor de Linux acessível através de um IP público. |
| **Recursos do Azure** ||
| [Criar conta de armazenamento do Azure e a partilha de ficheiros][az-files] | Utiliza a CLI do Azure numa instância de contentor para criar uma conta do storage e uma partilha de ficheiros do Azure.

## <a name="deployment"></a>Implementação

Tem várias opções para implementar recursos com modelos do Resource Manager:

[CLI do Azure][deploy-cli]

[Azure PowerShell][deploy-powershell]

[Portal do Azure][deploy-portal]

[REST API][deploy-rest]

<!-- LINKS - External -->
[app-nav]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-dynamicsnav
[app-wp]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-wordpress
[az-files]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-storage-file-share
[net-publicip]: https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-linuxcontainer-public-ip
[net-udp]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-udp
[repo]: https://github.com/Azure/azure-quickstart-templates
[vol-emptydir]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-linuxcontainer-volume-emptydir
[vol-gitrepo]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-linuxcontainer-volume-gitrepo
[vol-secret]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-aci-linuxcontainer-volume-secret

<!-- LINKS - Internal -->
[deploy-cli]: ../azure-resource-manager/resource-group-template-deploy-cli.md
[deploy-portal]: ../azure-resource-manager/resource-group-template-deploy-portal.md
[deploy-powershell]: ../azure-resource-manager/resource-group-template-deploy.md
[deploy-rest]: ../azure-resource-manager/resource-group-template-deploy-rest.md
[ref]: /azure/templates/microsoft.containerinstance/containergroups