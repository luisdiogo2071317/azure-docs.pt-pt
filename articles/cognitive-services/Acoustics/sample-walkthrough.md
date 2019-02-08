---
title: 'Exemplo: Projeto Acoustics'
titlesuffix: Azure Cognitive Services
description: Este tutorial descreve o cenário de exemplo de Unity para o Projeto Acoustics, incluindo a implementação no ambiente de trabalho e na VR.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: sample
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: ecd19ec343b2744a70456fd1d3ad226856768d71
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55880716"
---
# <a name="unity-sample-walkthrough"></a>Tutorial de exemplo de Unity
Este é um tutorial do exemplo do Projeto Acoustics. Para obter mais informações sobre o que é o Projeto Acoustics, consulte a [Introdução ao Projeto Acoustics](what-is-acoustics.md). Para obter ajuda na adição do pacote do Projeto Acoustics a um projeto Unity já existente, utilize o [Guia de Introdução](getting-started.md).

## <a name="requirements-for-running-the-sample-project"></a>Requisitos para executar o projeto de exemplo
* Unity 2018.2 +, com a versão de runtime de scripting de .NET 4.x
* Unity Editor para Windows de 64 bits
* O exemplo suporta computadores com Windows, UWP e destinos de Android, incluindo ecrãs de capacete (HMD)
* Subscrição do Azure Batch necessária para o processo de criação

## <a name="sample-project-setup"></a>Configuração do projeto de exemplo
Transferir e importar **MicrosoftAcoustics.Sample.unitypackage**. Durante a importação, as definições do projeto, incluindo **Spatializer** e **Versão de Runtime de Scripting** são atualizadas para satisfazer os requisitos do plug-in. Quando estiver concluída, verá um erro na consola do Unity a partir de **AcousticsGeometry.cs** sobre alterar a Versão de Runtime de Scripting para um **Equivalente a .NET 4.x**. Esta alteração de definições é executada como parte da importação do pacote, mas requer um reinício do Unity para entrar em vigor. Reinicie agora o Unity.

## <a name="running-the-sample"></a>Executar o exemplo
O exemplo inclui um cenário de demonstração **Assets/AcousticsDemo/ProjectAcousticsDemo.unity**. Este cenário possui três origens de som. Por predefinição, apenas uma origem de som está a ser reproduzida e as outras duas estão em pausa. Estão localizadas em **Origens de som** na **Hierarquia**. Para ajudar a elaborar um script de navegação genérico, a Câmara Principal é um elemento subordinado do objeto CameraHolder. 

![Vista de Hierarquia](media/SampleHierarchyView.png)

O cenário já foi criado e possui um ficheiro ACE associado a prefab **MicrosoftAcoustics** na **Hierarquia**. 

Ouça o som do cenário com um clique no botão de reprodução no editor do Unity. No ambiente de trabalho, utilize W, A, S, D e o rato para se mover. Para comparar a forma como a cena parece com e sem acoustics, prima a **R** botão até que o texto de sobreposição muda para vermelho e diz "Acoustics: Desativada." Para ver os atalhos de teclado para obter mais controlos, prima **F1**. Todos os controlos podem ser igualmente utilizados através de um clique no botão direito do rato para selecionar a ação a executar e, em seguida, com um clique no botão esquerdo do rato para executar a ação.

## <a name="targeting-other-platforms"></a>Selecionar outras plataformas como destino
O exemplo contém definições para serem executadas em computadores com Windows, UWP, Windows Mixed Reality, Android e Oculus Go. Por predefinição, o projeto está configurado para computadores com Windows. Para selecionar como destino uma plataforma de VR, vá para as definições de leitor (**Editar > Definições do Projeto > Leitor**), localize **Definições de XR**e selecione a caixa de verificação **Realidade Virtual Suportada**.

![Ativar VR](media/VRSupport.png)  

Ligar um VR headset ao seu PC. Aceda a **Ficheiro > Definições de Compilação** e clique em **Compilar e Executar** para implementar o exemplo para o seu VR headset. Navegue pelo cenário com os comandos de movimento do headset ou tente utilizar W, A, S, D no teclado.    
Para selecionar como destino o Android e o Oculus Go de destino, escolha Android a partir do menu **Definições de Compilação** menu. Clique em **Mudar destino** e, em seguida, em **Compilar e Executar**. Esta ação irá implementar o cenário de exemplo no seu dispositivo Android ligado. Para obter informações sobre o desenvolvimento de Unity para Android, consulte a [Documentação do Unity](https://docs.unity3d.com/Manual/android-GettingStarted.html).

![Android de destino](media/TargetAndroid.png)  

## <a name="next-steps"></a>Passos Seguintes
* [Criar uma conta do Azure](create-azure-account.md) para as suas próprias criações
* Explorar o [processo de conceção](design-process.md)

