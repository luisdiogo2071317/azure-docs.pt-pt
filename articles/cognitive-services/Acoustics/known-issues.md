---
title: Problemas conhecidos com o plug-in do projeto Acoustics
titlesuffix: Azure Cognitive Services
description: Poderá encontrar os seguintes problemas conhecidos ao utilizar a pré-visualização do Designer para Acoustics do projeto.
services: cognitive-services
author: kylestorck
manager: cgronlun
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: kylestorck
ms.openlocfilehash: 02f56306110e904f41c1b94346c58b33ce0b575c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55156195"
---
# <a name="known-issues"></a>Problemas conhecidos
Poderá encontrar os seguintes problemas conhecidos ao utilizar a pré-visualização do Designer para Acoustics do projeto.

## <a name="acoustic-parameters-are-lost-when-you-rename-a-scene"></a>Parâmetros acústicos são perdidos quando mudar o nome de uma cena

Se renomear uma cena, todos os parâmetros acústicos que pertencem a essa cena não irão transferir automaticamente para a cena nova. Eles continuarão a existir no ficheiro de elemento antigo no entanto. Procure o **SceneName_AcousticParameters.asset** dentro do ficheiro a **Editor** diretório junto ao seu ficheiro de cena. Mudar o nome do ficheiro para refletir o novo nome de cena.

## <a name="runtime-voxels-are-a-different-size-than-scene-preview-voxels"></a>Tempo de execução voxels são um tamanho diferente de cena voxels de pré-visualização

Se o fizer um **Calculate** sobre o **sondas** separador e vista voxels, em seguida, efetue um voxels criar e ver em tempo de execução para a cena da mesma, os voxels têm tamanhos diferentes. Voxels mostrados pré-criar são voxels utilizado na simulação. Voxels mostrados em tempo de execução são utilizados para a interpolação entre pontos de sonda. Isso pode causar uma inconsistência onde portais aparecerá abertos no tempo de execução que não são, na verdade, aberto.

## <a name="unity-crashes-when-closing-project"></a>Unity é interrompida ao fechar o projeto

Nas versões mais recentes do Unity (2018.2 +), há um bug conhecido onde Unity falhará ao fechar seu projeto. Isso é controlado [nesta edição do Unity](https://issuetracker.unity3d.com/issues/crash-on-assetdatabase-getassetimporterversions-when-closing-a-specific-unity-project).

## <a name="trouble-deploying-to-android"></a>Problemas de implementação para Android
Para utilizar o projeto Acoustics no Android, altere o destino de compilação para o Android. Algumas versões do Unity tem um bug com a implantação de plug-ins áudio – Certifique-se não estiver a utilizar uma versão afetada pela [este bug](https://issuetracker.unity3d.com/issues/android-ios-audiosource-playing-through-google-resonance-audio-sdk-with-spatializer-enabled-does-not-play-on-built-player).

## <a name="i-get-an-error-that-could-not-find-metadata-file-systemsecuritydll"></a>Recebo uma mensagem de erro que "não foi possível localizar o ficheiro de metadados System.Security.dll'

Certifique-se de que a versão de tempo de execução de scripts nas definições de Player está definida como **.NET 4.x equivalente**e reinicie o Unity.

## <a name="im-having-authentication-problems-when-connecting-to-azure"></a>Estou a ter problemas de autenticação ao ligar ao Azure

Verificar novamente já utilizou as credenciais corretas para a sua conta do Azure, que a sua conta suporta o tipo de nó solicitado na criar e que o relógio do seu sistema está correta.

## <a name="canceling-a-bake-leaves-the-bake-tab-in-deleting-state"></a>A cancelar uma criar deixa o separador de criar no estado de "eliminação"
Projeto Acoustics irá limpar todos os recursos do Azure para uma tarefa na conclusão com êxito ou cancelamento que pode demorar até 5 minutos.

## <a name="next-steps"></a>Passos Seguintes
* Introdução à [integração de acústica no projeto do Unity](getting-started.md)

