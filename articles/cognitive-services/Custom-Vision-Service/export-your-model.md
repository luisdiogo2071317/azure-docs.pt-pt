---
title: Exportar o modelo de serviço de visão personalizada para serviços móveis de cognitivos do Azure - Service de visão personalizada - | Microsoft Docs
description: Saiba como exportar o modelo para utilização na criação de aplicações móveis.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: ce8f42d6239867dd217cddfc61a27d7835dc9c9b
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355916"
---
# <a name="export-your-model-for-use-with-mobile-devices"></a>Exportar o modelo para utilização com dispositivos móveis

O serviço de visão personalizada permite classificadores exportada seja executado offline. Pode incorporar o classificador exportado para uma aplicação e executá-la localmente num dispositivo para classificação em tempo real. 

Serviço de visão personalizada suporta as exportações seguintes:

* __Tensorflow__ para __Android__.
* __CoreML__ para __iOS11__.
* __ONNX__ para __Windows ML__.
* Um Windows ou Linux __contentor__. O contentor inclui um Tensorflow modelo e o código para utilizar a API de serviço de visão personalizada de serviço. 

> [!IMPORTANT]
> Serviço de visão personalizada apenas exporta __compactar__ domínios. Os modelos gerados pelo compact domínios estão otimizados para as restrições de classificação em tempo real em dispositivos móveis. Classificadores criadas com um domínio compact poderão ser ligeiramente inferior exatos de um domínio padrão com a mesma quantidade de dados de preparação.
>
> Para informações sobre como melhorar os classificadores, consulte o [melhorar a sua classificador](getting-started-improving-your-classifier.md) documento.

## <a name="convert-to-a-compact-domain"></a>Converter a um domínio compacto

> [!NOTE]
> Os passos nesta secção aplicam-se apenas se tiver um classificador existente que não está definido para compactar o domínio.
 
Para converter o domínio de um classificador existente, utilize os seguintes passos:

1. Do [página de visão personalizada](https://customvision.ai), selecione o __home page__ ícone para ver uma lista dos seus projetos. Também pode utilizar o [ https://customvision.ai/projects ](https://customvision.ai/projects) para ver os seus projetos.

    ![Imagem da lista de ícone e projetos de raiz](./media/export-your-model/projects-list.png)

2. Selecione um projeto e, em seguida, selecione o __engrenagem__ ícone no canto superior direito da página.

    ![Imagem do ícone engrenagem](./media/export-your-model/gear-icon.png)

3. No __domínios__ secção, selecione um __compactar__ domínio. Selecione __guardar alterações__ para guardar as alterações.

    ![Imagem da seleção de domínios](./media/export-your-model/domains.png)

4. Na parte superior da página, selecione __formação__ a reparametrização dos utilizam o novo domínio.

## <a name="export-your-model"></a>Exportar o modelo

Para exportar o modelo após reparametrização, utilize os seguintes passos:

1. Vá para o **desempenho** separador e selecione __exportar__. 

    ![Imagem do ícone de exportação](./media/export-your-model/export.png)

    > [!TIP]
    > Se o __exportar__ entrada não está disponível, em seguida, a iteração selecionada não utilizar um domínio compactação. Utilize o __iterações__ secção desta página para selecionar uma iteração que utiliza um domínio compact e, em seguida, selecione __exportar__.

2. Selecione o formato de exportação e, em seguida, selecione __exportar__ para transferir o modelo.

## <a name="next-steps"></a>Passos Seguintes

Integre o seu modelo exportado para uma aplicação. Estão disponíveis várias aplicações de exemplo:

* Um exemplo de [utilizando o seu modelo CoreML exportado de uma aplicação iOS](https://go.microsoft.com/fwlink/?linkid=857726) para classificação de imagem em tempo real com Swift
* Exemplo de aplicação do iOS para [utilizando o modelo exportado do CoreML com Xamarin](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel) para classificação de imagem em tempo real 
* Exemplo para [utilizando o seu modelo Tensorflow exportado de uma aplicação Android](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample) para classificação de imagem em tempo real 
* [Utilizar o seu modelo Tensorflow com o Windows](https://docs.microsoft.com/en-us/azure/cognitive-services/custom-vision-service/export-model-python)
* Exemplo para [utilizando o modelo exportado do ONNX com o Windows Machine Learning](https://azure.microsoft.com/en-us/resources/samples/cognitive-services-onnx-customvision-sample/)
