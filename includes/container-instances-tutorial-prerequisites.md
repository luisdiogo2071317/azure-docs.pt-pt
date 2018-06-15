---
title: incluir ficheiro
description: incluir ficheiro
services: container-instances
author: mmacy
ms.service: container-instances
ms.topic: include
ms.date: 03/20/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 95a8cf4eca7969631d069a31c9d08ba52c8b02b4
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
ms.locfileid: "30193487"
---
Tem de cumprir os requisitos seguintes para concluir este tutorial:

**CLI do Azure**: Tem de ter a CLI do Azure versão 2.0.29 ou posterior instalada no computador local. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, consulte [Instalar o Azure CLI 2.0][azure-cli-install].

**Docker**: Este tutorial pressupõe conhecimentos básicos dos principais conceitos do Docker, como contentores, imagens de contentor e comandos básicos do `docker`. Para um manual de noções básicas do Docker e do contentor, veja a [descrição geral do Docker][docker-get-started].

**Motor do Docker**: Para concluir este tutorial, terá de ter o Motor do Docker instalado localmente. O Docker oferece pacotes que configuram o ambiente do Docker no [macOS][docker-mac], no [Windows][docker-windows] e no [Linux][docker-linux].

> [!IMPORTANT]
> Uma vez que a shell do Azure Cloud não inclui o daemon do Docker, *tem* de instalar a CLI do Azure e o Motor do Docker no seu *computador local* para concluir este tutorial. Não pode utilizar o Azure Cloud Shell para este tutorial.

<!-- LINKS - External -->
[docker-get-started]: https://docs.docker.com/engine/docker-overview/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - Internal -->
[azure-cli-install]: /cli/azure/install-azure-cli