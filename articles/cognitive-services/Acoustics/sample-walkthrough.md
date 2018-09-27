---
title: Instruções de exemplo do projeto Acoustics - serviços cognitivos
description: Este passo a passo descreve a cena de exemplo do Unity para Acoustics de projeto, incluindo a implementação para a área de trabalho e VR.
services: cognitive-services
author: kegodin
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: e0c28645de8c45aaf89afb6b5116aa9a3cb04768
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227508"
---
# <a name="unity-sample-walkthrough"></a>Instruções de exemplo do Unity
Este é um passo a passo do exemplo Acoustics do projeto. Para obter mais informações sobre quais Acoustics de projeto são, consulte a [introdução ao projeto Acoustics](what-is-acoustics.md). Para obter ajuda na adicionar o pacote de projeto Acoustics para um projeto Unity já existente, utilize o [guia de introdução](getting-started.md).

## <a name="requirements-for-running-the-sample-project"></a>Requisitos para executar o projeto de exemplo
* Unity 2018.2 +, usando a versão de tempo de execução de scripts de 4.x do .NET
* Unity Editor para Windows de 64 bits
* O exemplo suporta Windows desktop, UWP e destinos de Android, incluindo montado, head apresenta (HMDs)
* Subscrição do Azure de lote necessária para criar processo

## <a name="sample-project-setup"></a>Definição do projeto de exemplo
Transferir e importar os **MicrosoftAcoustics.Sample.unitypackage**. Ao importar, incluindo de definições do projeto **Spatializer** e **versão de tempo de execução de scripts** são atualizados para atender aos requisitos do plug-in. Quando for concluído, verá um erro na consola do Unity partir **AcousticsGeometry.cs** sobre como alterar a versão de tempo de execução de scripts para **.NET 4.x equivalente**. Esta alteração de definições é feita como parte da importação do pacote, mas requer um reinício do Unity para entrar em vigor. Reinicie agora Unity.

## <a name="running-the-sample"></a>Executar o exemplo
O exemplo inclui uma cena de demonstração **Assets/AcousticsDemo/ProjectAcousticsDemo.unity**. Essa cena tem uma única origem áudio spatialized reprodução de um cubo de vírgula flutuante (com o nome **AudioHolder** no **hierarquia**). Para ajudar a fazer um script de navegação genérico, a câmera principal é um filho do objeto CameraHolder. 

![Vista de hierarquia](media/SampleHierarchyView.png)

A cena já foi integrada e tem um arquivo ACE associados com o **MicrosoftAcoustics** prefab no **hierarquia**. 

Ouça a forma como a cena parece clicando no botão de reprodução no editor do Unity. Utilize W, A, S, D e o mouse para mover-se. Para comparar a forma como a cena parece com e sem acoustics, clique no botão esquerdo do mouse ou o botão de controlador primário. Para percorrer as várias origens de som, clique no botão direito do mouse ou o botão voltar no seu controlador.

## <a name="targeting-other-platforms"></a>Filtragem de outras plataformas
O exemplo contém definições para ser executado no ambiente de trabalho do Windows, UWP, realidade mista do Windows, Android e Oculus Go. Por predefinição, o projeto está configurado para o ambiente de trabalho do Windows. Para uma plataforma VR de destino, vá para as definições de leitor (**Editar > definições do projeto > Player**), localizar o **XR definições**e verificar o **suportado de realidade Virtual** caixa de verificação.

![Ativar VR](media/VRSupport.png)  

Ligar um headset VR ao seu PC. Aceda a **ficheiro > definições de criação**e clique em **compilar e executar** para implementar o exemplo o headset VR. Navegue por meio da cena com os controladores de movimento para o headset ou tente utilizar W, A, S, D no teclado.    
Para Android e Oculus Go de destino, escolha Android a partir da **definições de criação** menu. Clique em **mudar de destino**, em seguida, **compilar e executar**. Isto irá implementar a cena de exemplo no seu dispositivo Android ligado. Para obter informações sobre o desenvolvimento de Unity para Android, consulte [documentação do Unity](https://docs.unity3d.com/Manual/android-GettingStarted.html).

![Android de destino](media/TargetAndroid.png)  

## <a name="next-steps"></a>Passos Seguintes
* [Criar uma conta do Azure](create-azure-account.md) para seu próprio incorpora
* Explorar o [criar processo](design-process.md)

