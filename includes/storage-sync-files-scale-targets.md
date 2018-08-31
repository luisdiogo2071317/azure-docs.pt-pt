---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 07/18/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 3f70a8cf2df25f487de7cd1a8c8cbdf9431839f0
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43285639"
---
| Recurso | Destino | Limite de disco rígido |
|----------|--------------|------------|
| Serviços de sincronização de armazenamento por subscrição | 15 Serviços de sincronização de armazenamento | Não |
| Grupos de sincronização por serviço de sincronização de armazenamento | 100 grupos de sincronização | Sim |
| Servidores registados por serviço de sincronização de armazenamento | servidores de 99 | Sim |
| Pontos finais da cloud por grupo de sincronização | ponto final da 1 cloud | Sim |
| Pontos finais do servidor por grupo de sincronização | 50 pontos finais do servidor | Não |
| Pontos finais do servidor por servidor | pontos finais do servidor de 33-99 | Sim, mas varia com base na configuração (CPU, memória, volumes, alterações a ficheiros, contagem de ficheiros, etc.) |
| Tamanho do ponto final | 4 TiB | Não |
| Objetos de sistema (diretórios e arquivos) por grupo de sincronização de ficheiros | objetos de milhões de 25 | Não |
| Número máximo de ficheiro objetos de sistema (diretórios e arquivos) num diretório | 200 000 objetos | Sim |
| Comprimento do nome do objeto máximo (diretórios e arquivos) | 255 carateres | Sim |
| Máximo de objeto (diretórios e arquivos) descritor tamanho de segurança | 4 KiB | Sim |
| Tamanho dos ficheiros | 100 GiB | Não |
| Tamanho mínimo do ficheiro para um ficheiro para ser colocado em camadas | 64 KiB | Sim |
