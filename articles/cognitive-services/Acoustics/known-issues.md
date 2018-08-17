---
title: Problemas conhecidos com acoustics Plug-in - Serviços cognitivos
description: Poderá encontrar os seguintes problemas conhecidos ao utilizar a pré-visualização do Designer para Acoustics do projeto.
services: cognitive-services
author: kylestorck
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kylestorck
ms.openlocfilehash: b0dd5e0c365c65fc7b29752f7885acb71bdb3df8
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/17/2018
ms.locfileid: "40181696"
---
# <a name="known-issues"></a>Problemas conhecidos
Poderá encontrar os seguintes problemas conhecidos ao utilizar a pré-visualização do Designer para Acoustics do projeto.

## <a name="acoustic-parameters-are-lost-when-you-rename-a-scene"></a>Parâmetros acústicos são perdidos quando mudar o nome de uma cena

Se renomear uma cena, todos os parâmetros acústicos que pertencem a essa cena não irão transferir automaticamente para a cena nova. Eles continuarão a existir no ficheiro de elemento antigo no entanto. Procure o **SceneName_AcousticParameters.asset** dentro do ficheiro a **Editor** diretório junto ao seu ficheiro de cena. Mudar o nome do ficheiro para refletir o novo nome de cena.

## <a name="the-default-path-for-the-acousticsdata-folder-in-probes-tab-is-an-absolute-path"></a>O caminho padrão para a pasta de AcousticsData no separador de sondas é um caminho absoluto

Isso deverá ser predefinido para um caminho relativo para que seja mais fácil de compartilhar projetos entre funcionários. Como solução, altere o caminho relativa ao diretório do projeto.

## <a name="runtime-voxels-are-a-different-size-than-design-time-voxels"></a>Tempo de execução voxels são um tamanho diferente que o tempo de design voxels

Se o fizer um **Calculate** sobre o **sondas** separador e vista voxels, em seguida, efetue um voxels criar e ver em tempo de execução para a cena da mesma, os voxels têm tamanhos diferentes. Voxels mostrados pré-criar são voxels utilizado na simulação. Voxels mostrados em tempo de execução são utilizados para a interpolação entre pontos de sonda. Isso pode causar uma inconsistência onde portais aparecerá abertos no tempo de execução que não são, na verdade, aberto.

## <a name="uwp-builds-not-working"></a>UWP baseia-se não funcionar

Nas versões mais recentes do Unity (2018.2 +), UWP compilações são não que os sucedem. A fase de execução da compilação irá manipulação de compartimento e obterá erros "extensões Unity são ainda não inicializadas". Isso é controlado [nesta edição do Unity](https://fogbugz.unity3d.com/default.asp?1070491_1rgf14bakv5u779d).

## <a name="unity-crashes-when-closing-project"></a>Unity é interrompida ao fechar o projeto

Nas versões mais recentes do Unity (2018.2 +), há um bug conhecido onde Unity falhará ao fechar seu projeto. Isso é controlado [nesta edição do Unity](https://issuetracker.unity3d.com/issues/crash-on-assetdatabase-getassetimporterversions-when-closing-a-specific-unity-project).

## <a name="trouble-deploying-to-android"></a>Problemas de implementação para Android
Para utilizar o projeto Acoustics no Android, altere o destino de compilação para o Android. Algumas versões do Unity tem um bug com a implantação de plug-ins áudio – Certifique-se não estiver a utilizar uma versão afetada pela [este bug](https://issuetracker.unity3d.com/issues/android-ios-audiosource-playing-through-google-resonance-audio-sdk-with-spatializer-enabled-does-not-play-on-built-player).

## <a name="i-get-an-error-that-could-not-find-metadata-file-systemsecuritydll"></a>Recebo uma mensagem de erro que "não foi possível localizar o ficheiro de metadados System.Security.dll'

Certifique-se de que a versão de tempo de execução de scripts nas definições de Player está definida como **.NET 4.x equivalente**e reinicie o Unity.

## <a name="im-having-authentication-problems-when-connecting-to-azure"></a>Estou a ter problemas de autenticação ao ligar ao Azure

Verificar novamente já utilizou as credenciais corretas para a sua conta do Azure, que a sua conta suporta o tipo de nó solicitado na criar e que o relógio do seu sistema está correta.

## <a name="next-steps"></a>Passos Seguintes
* Introdução ao [integrar acoustics no seu projeto Unity](getting-started.md)

