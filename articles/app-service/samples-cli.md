---
title: Exemplos da CLI do Azure - serviço de aplicações | Documentos da Microsoft
description: Exemplos da CLI do Azure - serviço de aplicações
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: 53e6a15a-370a-48df-8618-c6737e26acec
ms.service: app-service
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 12/12/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: af28f0fc8a75f32678d94523a01f991ab3d27351
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2018
ms.locfileid: "53585201"
---
# <a name="cli-samples-for-azure-app-service"></a>Exemplos da CLI para o serviço de aplicações do Azure

A tabela seguinte inclui ligações para scripts bash criados com a CLI do Azure.

| | |
|-|-|
|**Criar aplicação**||
| [Criar uma aplicação web e implementar ficheiros com FTP](./scripts/cli-deploy-ftp.md?toc=%2fcli%2fazure%2ftoc.json)| Cria uma aplicação web do Azure e implementa um arquivo ao mesmo através de FTP. |
| [Criar uma aplicação Web e implementar código a partir do GitHub](./scripts/cli-deploy-github.md?toc=%2fcli%2fazure%2ftoc.json)| Cria uma aplicação web do Azure e implementa código a partir de um repositório do GitHub público. |
| [Criar uma aplicação Web com implementação contínua do GitHub](./scripts/cli-continuous-deployment-github.md?toc=%2fcli%2fazure%2ftoc.json)| Cria uma aplicação web do Azure com a publicação contínua a partir de um repositório do GitHub que possui. |
| [Criar uma aplicação Web e implementar código a partir de um repositório do Git local](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma aplicação web do Azure e configura o push de código de um repositório de Git local. |
| [Criar uma aplicação Web e implementar código para um ambiente de teste](./scripts/cli-deploy-staging-environment.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma aplicação web do Azure com um bloco de implementação para alterações de código de teste. |
| [Criar uma aplicação Web do Núcleo ASP.NET num contentor do Docker](./scripts/cli-linux-docker-aspnetcore.md?toc=%2fcli%2fazure%2ftoc.json)| Cria uma aplicação web do Azure no Linux e carrega uma imagem do Docker a partir do Docker Hub. |
|**Configurar aplicação**||
| [Mapear domínios personalizados para uma aplicação Web](./scripts/cli-configure-custom-domain.md?toc=%2fcli%2fazure%2ftoc.json)| Cria uma aplicação web do Azure e mapeia um nome de domínio personalizado ao mesmo. |
| [Vincular um certificado SSL personalizado a uma aplicação web](./scripts/cli-configure-ssl-certificate.md?toc=%2fcli%2fazure%2ftoc.json)| Cria uma aplicação web do Azure e vincula o certificado SSL de um nome de domínio personalizado ao mesmo. |
|**Aplicação de dimensionamento**||
| [Dimensionar uma aplicação Web manualmente](./scripts/cli-scale-manual.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma aplicação web do Azure e dimensiona-a entre 2 instâncias. |
| [Dimensionar uma aplicação Web para todo o mundo com uma arquitetura de elevada disponibilidade](./scripts/cli-scale-high-availability.md?toc=%2fcli%2fazure%2ftoc.json) | Cria duas aplicações web do Azure em duas regiões geográficas diferentes e disponibiliza-os através de um único ponto final de utilização do Gestor de tráfego do Azure. |
|**Ligar a aplicação aos recursos**||
| [Ligar uma aplicação web para uma base de dados SQL](./scripts/cli-connect-to-sql.md?toc=%2fcli%2fazure%2ftoc.json)| Cria uma aplicação web do Azure e uma base de dados SQL, em seguida, adiciona a cadeia de ligação de base de dados para as definições da aplicação. |
| [Ligar uma aplicação Web a uma conta de armazenamento](./scripts/cli-connect-to-storage.md?toc=%2fcli%2fazure%2ftoc.json)| Cria uma aplicação web do Azure e uma conta de armazenamento, em seguida, adiciona a cadeia de ligação de armazenamento para as definições da aplicação. |
| [Ligar uma aplicação web para uma Cache do Azure para Redis](./scripts/cli-connect-to-redis.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma aplicação web do Azure e uma Cache do Azure para Redis, em seguida, adiciona os detalhes de ligação redis às definições da aplicação.) |
| [Ligar uma aplicação web ao Cosmos DB](./scripts/cli-connect-to-documentdb.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma aplicação web do Azure e um Cosmos DB e, em seguida, adiciona os detalhes de ligação do Cosmos DB às definições da aplicação. |
|**Criar cópias de segurança e restaurar a aplicação**||
| [Fazer uma cópia de segurança de uma aplicação web](./scripts/cli-backup-onetime.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma aplicação web do Azure e cria uma cópia de segurança única para o mesmo. |
| [Criar uma cópia de segurança agendada para uma aplicação web](./scripts/cli-backup-scheduled.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma aplicação web do Azure e cria uma cópia de segurança agendada para o mesmo. |
| [Restaura uma aplicação web a partir de uma cópia de segurança](./scripts/cli-backup-restore.md?toc=%2fcli%2fazure%2ftoc.json) | Restaura uma aplicação web do Azure a partir de uma cópia de segurança. |
|**Monitorizar aplicação**||
| [Monitorizar uma aplicação web com os registos de servidor web](./scripts/cli-monitor.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma aplicação web do Azure, ativa o registo para ele e transfere os registos para o computador local. |
| | |