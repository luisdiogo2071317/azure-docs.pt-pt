---
title: Exemplo do Script da CLI do Azure - Publicar um recurso | Microsoft Docs
description: Utilize o script da CLI do Azure para publicar um recurso.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/11/2018
ms.author: juliako
ms.openlocfilehash: 1ce85c18b93f22e9078e551deda74c6e6ff0b28b
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614927"
---
# <a name="cli-example-publish-an-asset"></a>Exemplo da CLI: publicar um elemento

O script da CLI do Azure neste artigo mostra como criar um Localizador de Transmissão em Fluxo e voltar aos URLs de Transmissão em Fluxo. 

## <a name="prerequisites"></a>Pré-requisitos 

- Instalar e utilizar a CLI localmente, este artigo requer a versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli). 

    Atualmente, nem todos [dos serviços de multimédia v3 CLI](https://aka.ms/ams-v3-cli-ref) comandos trabalham no Azure Cloud Shell. Recomenda-se para utilizar a CLI localmente.

- [Criar uma conta de Media Services](../create-account-cli-how-to.md).

## <a name="example-script"></a>Script de exemplo

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/publish-asset/Publish-Asset.sh "Publish an asset")]

## <a name="next-steps"></a>Passos Seguintes

Para obter mais exemplos, veja [Azure CLI samples](../cli-samples.md) (Exemplos de CLI do Azure).
