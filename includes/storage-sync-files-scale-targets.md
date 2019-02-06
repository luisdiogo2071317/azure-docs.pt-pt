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
ms.openlocfilehash: fea31bf1c5869164dde1eb7014b2aacc8fcc5e9c
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/05/2019
ms.locfileid: "55757631"
---
| Recurso | Destino | Limite de disco rígido |
|----------|--------------|------------|
| Serviços de sincronização de armazenamento por região | 15 Serviços de sincronização de armazenamento | Sim |
| Grupos de sincronização por serviço de sincronização de armazenamento | 100 grupos de sincronização | Sim |
| Servidores registados por serviço de sincronização de armazenamento | servidores de 99 | Sim |
| Pontos finais da cloud por grupo de sincronização | ponto final da 1 cloud | Sim |
| Pontos finais do servidor por grupo de sincronização | 50 pontos finais do servidor | Não |
| Pontos finais do servidor por servidor | 30 pontos finais do servidor | Sim |
| Tamanho do ponto final | 4 TiB | Não |
| Objetos de sistema (diretórios e arquivos) por grupo de sincronização de ficheiros | objetos de milhões de 25 | Não |
| Número máximo de ficheiro objetos de sistema (diretórios e arquivos) num diretório | 200 000 objetos | Sim |
| Máximo de objeto (diretórios e arquivos) descritor tamanho de segurança | 4 KiB | Sim |
| Tamanho dos ficheiros | 100 GiB | Não |
| Tamanho mínimo do ficheiro para um ficheiro para ser colocado em camadas | 64 KiB | Sim |
| Sessões de sincronização simultâneas | Agente de v4: Limite varia com base nos recursos de sistema disponíveis. <BR> Agente de v3: 2 de sincronização do Active Directory de sessões por processador ou máximo de 8 sessões de sincronização do Active Directory por servidor | Sim
