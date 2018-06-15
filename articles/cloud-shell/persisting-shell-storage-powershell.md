---
title: Manter os ficheiros no PowerShell na Shell de nuvem do Azure (pré-visualização) | Microsoft Docs
description: Instruções sobre como o Shell de nuvem do Azure persistir ficheiros.
services: azure
documentationcenter: ''
author: maertendmsft
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: damaerte
ms.openlocfilehash: b87c4a408393e4ae341898e8cfa23e9acbcb4fc2
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32154403"
---
[!INCLUDE [features-introblock](../../includes/cloud-shell-persisting-shell-storage-introblock.md)]

## <a name="how-powershell-in-azure-cloud-shell-preview-works"></a>Como funciona o PowerShell na Shell de nuvem do Azure (pré-visualização)
PowerShell na Shell de nuvem (pré-visualização) persiste ficheiros através do método seguinte: 
* Montar a partilha de ficheiros do Azure especificado como `clouddrive` no seu `$Home` diretório para interação direta de partilha de ficheiros.

## <a name="list-clouddrive-azure-file-shares"></a>Lista `clouddrive` partilhas de ficheiros do Azure
O `Get-CloudDrive` comando obtém as informações da partilha de ficheiros do Azure atualmente montadas pelo `clouddrive` na Shell de nuvem. <br>
![Get-CloudDrive em execução](media/persisting-shell-storage-powershell/Get-Clouddrive.png)

## <a name="unmount-clouddrive"></a>Desmonte `clouddrive`
Pode desmontar uma partilha de ficheiros do Azure que está montada para nuvem Shell em qualquer altura. Se a partilha de ficheiros do Azure foi removida, será solicitado para criar e montar uma nova partilha de ficheiros do Azure na próxima sessão.

O `Dismount-CloudDrive` comando unmounts uma partilha de ficheiros do Azure da conta do storage atual. Desmontar a `clouddrive` termina a sessão atual. Será solicitado ao utilizador para criar e montar uma nova partilha de ficheiros do Azure durante a sessão seguinte.
![Dismount-CloudDrive em execução](media/persisting-shell-storage-powershell/Dismount-Clouddrive.png)

[!INCLUDE [features-endblock](../../includes/cloud-shell-persisting-shell-storage-endblock.md)]

## <a name="next-steps"></a>Passos Seguintes
[Início rápido para o PowerShell](quickstart-powershell.md) <br>
[Saiba mais sobre os ficheiros do Azure](https://docs.microsoft.com/azure/storage/storage-introduction#file-storage) <br>
[Saiba mais sobre as etiquetas de armazenamento](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) <br>