---
title: incluir ficheiro
description: incluir ficheiro
services: batch
author: dlepow
ms.service: batch
ms.topic: include
ms.date: 05/04/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 62bb91a2e51c39caf31719f72d68a6edab9205bc
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38946232"
---
> [!NOTE]
> Quando criar uma conta do Batch, pode escolher entre dois modos de *alocação de conjuntos*: **subscrição de utilizador** e **serviço Batch**. Na maioria dos casos, deve utilizar o serviço Batch predefinido, no qual os conjuntos são alocados em segundo plano nas subscrições geridas do Azure. No modo de subscrição de utilizador alternativo, as VMs do Batch e outros recursos são criados diretamente na sua subscrição quando é criado um conjunto. O modo de subscrição de utilizador é necessário se quiser criar conjuntos do Batch com o [Azure Reserved VM Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/). Para criar uma conta do Batch no modo de subscrição de utilizador, também tem de registar a sua subscrição no Azure Batch e associar a conta ao Azure Key Vault.