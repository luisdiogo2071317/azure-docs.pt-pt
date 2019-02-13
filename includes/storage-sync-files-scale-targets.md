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
ms.openlocfilehash: c82717be3791a1bc397bbbdb369c55cd2ba04439
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56213166"
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
| Número máximo de ficheiro objetos de sistema (diretórios e arquivos) num diretório | 1 000 000 objetos | Sim |
| Máximo de objeto (diretórios e arquivos) descritor tamanho de segurança | 4 KiB | Sim |
| Tamanho dos ficheiros | 100 GiB | Não |
| Tamanho mínimo do ficheiro para um ficheiro para ser colocado em camadas | 64 KiB | Sim |
| Sessões de sincronização simultâneas | Agente de v4 e posterior: Limite varia com base nos recursos de sistema disponíveis. <BR> Agente de v3: 2 de sincronização do Active Directory de sessões por processador ou máximo de 8 sessões de sincronização do Active Directory por servidor | Sim
