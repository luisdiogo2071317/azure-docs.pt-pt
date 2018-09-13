---
title: Criar uma palavra de reativação personalizado
description: Saiba como criar uma palavra de reativação personalizado para o SDK de dispositivos de voz.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: e1a74828213b595e6955a750904de7a3c2a5a865
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44719091"
---
# <a name="create-a-custom-wake-word-by-using-the-speech-service"></a>Criar uma palavra de reativação personalizado com o serviço de voz

O dispositivo está sempre à escuta de uma palavra de reativação (ou expressão). Por exemplo, "EI Cortana" é uma palavra de reativação para o Assistente de Cortana. Quando o usuário disser a palavra de reativação, o dispositivo envia áudio subsequente tudo para a cloud, até que o utilizador para a falar. Personalizar a sua palavra de reativação é uma forma eficaz para diferenciar o seu dispositivo e fortalecer a sua imagem corporativa.

Neste artigo, saiba como criar uma palavra de reativação personalizado para o seu dispositivo.

## <a name="choose-an-effective-wake-word"></a>Escolha uma palavra em vigor a partir de reativação

Considere as seguintes diretrizes ao escolher uma palavra de reativação:

* O word reativação deve ser uma palavra em inglês ou uma expressão. Deve demorar mais do que dois segundos para dizer.

* Palavras de 4 a 7 sílabas funcionam melhor. Por exemplo, "EI, computador" é uma palavra de reativação boa. Apenas "Olá" é ruim.

* Palavras de reativação devem seguir as regras de pronúncia inglesa comuns.

* Um único ou até mesmo uma palavra produzida que segue as regras de pronúncia inglesa comum pode reduzir os falsos positivos. Por exemplo, "computerama" pode ser uma palavra de reativação boa.

* Não escolha uma palavra em comum. Por exemplo, "" ilimitado"e"Ir"são palavras que as pessoas dizem com frequência na conversação comum. Elas podem ser acionadores falso para o seu dispositivo.

* Evite utilizar uma palavra de reativação que pode ter pronunciations alternativos. Os utilizadores teria de saber a pronúncia "right" para que o seu dispositivo para responder. Por exemplo, "509" pode ser pronunciado "cinco zero nine," "cinco AH nove," ou "cinco centenas e nove." "R.E.I." pode ser pronunciado "r-e-i" ou "ray." "Ao vivo" pode ser pronunciado "/līv/" ou "/liv/".

* Não utilize carateres especiais, símbolos ou dígitos. Por exemplo, "Go #" e "20 + gatos" não seria bom reativação palavras. No entanto, "aceda pequenos" ou "vinte plus gatos" poderá funcionar. Ainda pode usar os símbolos em sua imagem corporativa e usar o marketing e documentação para reforçar a pronúncia adequada.

> [!NOTE]
> Se escolher uma palavra de marca registada como o word de reativação, certifique-se de que seu essa marca comercial ou que têm permissão do proprietário de marca registada para utilizar a palavra. A Microsoft não se responsabilizará por quaisquer problemas legais que possam surgir da sua escolha de palavras de reativação.

## <a name="create-your-wake-word"></a>Criar a sua palavra de reativação

Antes de poder utilizar uma palavra de reativação personalizado com o seu dispositivo, tem de criar a palavra de reativação com o serviço de geração de reativação de Word personalizado Microsoft. Depois de fornecer uma palavra de reativação, o serviço de produz um ficheiro que pode implementar em seu kit de desenvolvimento para ativar a sua palavra de reativação no seu dispositivo.

1. Vá para o [portal do serviço de voz personalizada](https://cris.ai/).

2. Crie uma nova conta com o endereço de e-mail em que recebeu o convite para o Azure Active Directory. 

    ![Criar uma conta nova](media/speech-devices-sdk/wake-word-1.png)
 
3.  Depois de iniciar sessão, preencha o formulário e, em seguida, selecione **iniciar minha jornada**.

    ![iniciar sessão com êxito](media/speech-devices-sdk/wake-word-3.png)
 
4. O **Word de reativação personalizado** página não está disponível ao público, portanto, não há nenhum link que leva-o aí. Selecione ou cole em vez disso, esta ligação: https://cris.ai/customkws.

    ![A página do Word de reativação personalizada está oculta](media/speech-devices-sdk/wake-word-4.png)
 
6. Escreva a palavra de reativação à sua escolha e, em seguida, selecione **submeter palavra**.

    ![Introduza a palavra de reativação](media/speech-devices-sdk/wake-word-5.png)
 
7. Poderá demorar alguns minutos para que os ficheiros sejam gerados. Deverá ver um círculo a girar na janela do browser. Após alguns momentos, uma barra de informações for apresentada, que lhe pede para transferir um ficheiro. zip.

    ![Receber o ficheiro. zip](media/speech-devices-sdk/wake-word-6.png)

8. Guarde o ficheiro. zip para o seu computador. Precisa de ter este ficheiro para implementar a palavra de reativação personalizado para o kit de desenvolvimento. Para implementar a palavra de reativação personalizado, siga as instruções em [começar com o SDK de dispositivos de voz](speech-devices-sdk-qsg.md).

9. Selecione **terminar sessão.**

## <a name="next-steps"></a>Passos Seguintes

Para começar, obtenha uma [conta gratuita do Azure](https://azure.microsoft.com/free/) e inscreva-se para o SDK de dispositivos de voz.

> [!div class="nextstepaction"]
> [Inscreva-se o SDK de dispositivos de voz](get-speech-devices-sdk.md)

