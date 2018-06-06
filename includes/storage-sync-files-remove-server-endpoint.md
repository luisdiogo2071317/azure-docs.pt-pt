---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 05/31/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 33dff710d83bd12a8db343a89c6e4576d1397ba7
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738688"
---
Não: remover um ponto final do servidor não é como um servidor a ser reiniciado! A remover e recriar o ponto final do servidor quase nunca são uma solução adequada para corrigir problemas com a sincronização, na nuvem em camadas ou outros aspetos da sincronização de ficheiros do Azure. A remoção de um ponto final do servidor é uma operação destrutivas e pode resultar na perda de dados no caso de camadas de ficheiros existem fora do espaço de nomes de ponto final do servidor (consulte [por que motivo em camadas existem ficheiros fora do espaço de nomes de ponto final do servidor](../articles/storage/files/storage-files-faq.md#afs-tiered-files-out-of-endpoint) para obter mais informações) ou nos ficheiros inacessíveis para os ficheiros em camadas existentes no espaço de nomes de ponto final do servidor. Estes problemas não serão resolvido quando o ponto final do servidor é recriado. Ficheiros em camadas podem existir no seu espaço de nomes de ponto final do servidor, mesmo se nunca teve nuvem camadas ativado. Por conseguinte, recomendamos que não remover o ponto final do servidor, a menos que pretende parar a sincronização de ficheiros do Azure a utilizar com esta pasta específica ou tenha sido explicitamente instruído para fazê-lo por um engenheiro de Microsoft. Para obter mais informações sobre pontos finais do servidor de remover, consulte [remover um ponto final do servidor](../articles/storage/files/storage-sync-files-server-endpoint.md#remove-a-server-endpoint).    