---
title: Criar uma palavra de reativação personalizado | Microsoft Docs
description: Criar uma palavra de reativação personalizado para o SDK de dispositivos de reconhecimento de voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: 2575ed24bb931ca4da05dd6663b976406af590e6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355669"
---
# <a name="create-a-custom-wake-word-using-speech-service"></a>Criar uma palavra de reativação personalizada com o serviço de reconhecimento de voz

O dispositivo está a escutar sempre de uma palavra de reativação (ou o frase). Por exemplo, "Hey Cortana" é uma palavra de reativação para o Assistente de Cortana. Quando o utilizador indica a palavra de reativação, o dispositivo é iniciado enviar todos os áudio subsequentes para a nuvem até que o utilizador pára falando. Personalizar a sua palavra de reativação é uma forma eficaz para diferenciar o seu dispositivo e reforçar a sua imagem corporativa.

Neste artigo, irá aprender a criar uma palavra de reativação personalizado para o seu dispositivo.

## <a name="choosing-an-effective-wake-word"></a>Escolher uma palavra de reativação Efetivo

Considere as seguintes diretrizes quando escolher uma palavra de reativação.

* A palavra de reativação deve ser uma palavra em inglês ou uma expressão. Deve demorar mais do que dois segundos para indicar.

* Palavras de 4 – 7 sílabas funcionam melhor. Por exemplo, "Hey, computador" é uma palavra de reativação boa, embora apenas "Hey" é um fraco.

* Reativação palavras devem seguir regras comuns de pronunciação em inglês.

* Uma palavra exclusiva ou made-up, mesmo que se segue regras comuns de pronunciação inglês pode reduzir os falsos positivos. Por exemplo, "computerama" pode ser uma palavra de reativação bom.

* Não escolha uma palavra comuns. Por exemplo, "eat" e "Ir" são palavras que pessoas frequentemente afirmam na conversação comum. Podem ser falsos acionadores para o seu dispositivo.

* Evite utilizar uma palavra de reativação que pode ter pronunciations alternativos. Os utilizadores teriam de conhecer a pronunciação "direita" para que o seu dispositivo para responder. Por exemplo, pode ser pronunciados "509" como "cinco zero nove", "cinco esqueceu nove", ou "cinco centena e nine." "R.E.I." pode ser pronunciados como "R I posso" ou "Ray." "Em direto" pode ser pronunciado como [līv] ou [liv].

* Não utilize carateres especiais, símbolos ou dígitos. Por exemplo, "Go #" e "20 + cats" não é possível reativação boa palavras. No entanto, "go sharp" ou "twenty plus cats" funcionaria. Ainda pode utilizar os símbolos na sua imagem corporativa e utilizar marketing e a documentação para impor a pronunciação adequada.

> [!NOTE]
> Se optar por uma marca registada palavra como o word de reativação, lembre-se de que é proprietário desse marca; caso contrário, tem a permissão do proprietário da marca utilizá-la. A Microsoft não está responsabilizado para quaisquer problemas legais que podem surgir na sua escolha de palavras de reativação.

## <a name="creating-your-wake-word"></a>Criar as suas word de reativação

Antes de poder utilizar uma palavra de reativação personalizada com o seu dispositivo, tem de criar com o serviço Microsoft personalizado geração Word reativação. Depois de fornecer uma palavra de reativação, o serviço de produz um ficheiro que, em seguida, implementar no seu kit de desenvolvimento para ativar a sua palavra de reativação no seu dispositivo.

1. Vá para o [portal do serviço de reconhecimento de voz personalizadas](https://cris.ai/).

2. Crie uma nova conta com o endereço de e-mail no qual foi recebido o convite do Azure Active Directory. 

    ![Criar nova conta](media/speech-devices-sdk/wake-word-1.png)
 
3.  Assim que a sessão iniciada, preencha o formulário, em seguida, clique em **iniciar o journey.**

    ![sessão iniciada com êxito](media/speech-devices-sdk/wake-word-3.png)
 
4. O **personalizada reativação Word** página não está disponível para o público, pelo que não existe nenhuma ligação que o leva não existe. Clique em ou colar em vez disso, esta ligação: https://cris.ai/customkws.

    ![página oculta](media/speech-devices-sdk/wake-word-4.png)
 
6. Escreva a palavra de reativação à sua escolha, em seguida, **submeter** -lo.

    ![Introduza a palavra de reativação](media/speech-devices-sdk/wake-word-5.png)
 
7. Pode demorar alguns minutos para que os ficheiros ser gerado. Deverá ver um círculo a girar no separador do browser. Depois de um momento, uma barra de informações for apresentada a pedir-lhe transferir um `.zip` ficheiro.

    ![receber o ficheiro. zip](media/speech-devices-sdk/wake-word-6.png)

8. Guardar o `.zip` ficheiro para o seu computador. É necessário este ficheiro para implementar a palavra de reativação personalizado para o kit de desenvolvimento, seguindo as instruções em [começar com o SDK de dispositivos de reconhecimento de voz](speech-devices-sdk-qsg.md).

9. Pode agora **terminar sessão.**

## <a name="next-steps"></a>Passos Seguintes

Para começar a utilizar, obtenha um [conta do Azure gratuita](https://azure.microsoft.com/free/) e inscrever-se para o SDK de dispositivos de reconhecimento de voz.

> [!div class="nextstepaction"]
> [Inscreva-se o SDK de dispositivos de reconhecimento de voz](get-speech-devices-sdk.md)

