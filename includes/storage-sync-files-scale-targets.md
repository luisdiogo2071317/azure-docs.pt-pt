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
ms.openlocfilehash: a29f1c4a625552dd958884c6a172bee470e61ca6
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/12/2018
ms.locfileid: "49312479"
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
| Sessões de sincronização simultâneas | 2 de sincronização do Active Directory de sessões por processador ou máximo de 8 sessões de sincronização do Active Directory por servidor | Sim |
