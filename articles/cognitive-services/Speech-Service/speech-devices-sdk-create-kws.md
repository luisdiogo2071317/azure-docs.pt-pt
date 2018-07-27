---
title: Criar uma palavra de reativação personalizado
description: A criar uma palavra de reativação personalizado para o SDK de dispositivos de voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: 615a901c70fff92141442699ea6e4b8fce1c9ace
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282578"
---
# <a name="create-a-custom-wake-word-using-speech-service"></a>Criar uma palavra de reativação personalizado com o serviço de voz

O dispositivo está sempre à escuta de uma palavra de reativação (ou expressão). Por exemplo, "EI Cortana" é uma palavra de reativação para o Assistente de Cortana. Quando o usuário disser a palavra de reativação, o dispositivo começa a enviar todos os de áudio subsequente para a cloud até que o utilizador para a falar. Personalizar a sua palavra de reativação é uma forma eficaz para diferenciar o seu dispositivo e fortalecer a sua imagem corporativa.

Neste artigo, saiba como criar uma palavra de reativação personalizado para o seu dispositivo.

## <a name="choosing-an-effective-wake-word"></a>Escolher uma palavra em vigor a partir de reativação

Considere as seguintes diretrizes ao escolher uma palavra de reativação.

* O word reativação deve ser uma palavra em inglês ou uma expressão. Deve demorar mais do que dois segundos para dizer.

* Palavras de 4 a 7 sílabas funcionam melhor. Por exemplo, "EI, computador" é uma palavra de reativação boa, embora apenas "Olá" é um fraco.

* Palavras de reativação devem seguir as regras de pronúncia inglesa comuns.

* Uma palavra de exclusiva ou até mesmo produzida que segue as regras comuns de pronúncia inglesa poderia reduzir falsos positivos. Por exemplo, "computerama" pode ser uma palavra de reativação boa.

* Não escolha uma palavra em comum. Por exemplo, "" ilimitado"e"Ir"são palavras que as pessoas dizem com frequência na conversação comum. Podem ser acionadores falso para o seu dispositivo.

* Evite utilizar uma palavra de reativação que poderia ter pronunciations alternativos. Os utilizadores teria de saber a pronúncia "right" para que o seu dispositivo para responder. Por exemplo, "509" poderia ser pronunciado como "cinco zero nine", "cinco AH nove", ou "cinco centenas e nove." "R.E.I." pode ser pronunciado como "R E eu" ou "Ray." "Ao vivo" poderia ser pronunciado como [līv] ou [liv].

* Não utilize carateres especiais, símbolos ou dígitos. Por exemplo, "Go #" e "20 + gatos" não seria bom reativação palavras. No entanto, "aceda pequenos" ou "vinte plus gatos" poderia trabalhar. Ainda pode usar os símbolos em sua imagem corporativa e usar o marketing e documentação para reforçar a pronúncia adequada.

> [!NOTE]
> Se escolher uma palavra de marca registada como o word de reativação, certifique-se de que possui essa marca comercial ou, então, tem a permissão do proprietário sobre marcas registadas para utilizá-lo. A Microsoft não se responsabilizará por quaisquer problemas legais que podem surgir da sua escolha de palavras de reativação.

## <a name="creating-your-wake-word"></a>Criar a sua palavra de reativação

Antes de poder utilizar uma palavra de reativação personalizado com o seu dispositivo, tem de criá-lo com o serviço de geração de reativação de Word personalizado Microsoft. Depois de fornecer uma palavra de reativação, o serviço de produz um ficheiro que, em seguida, implementar em seu kit de programação para ativar a sua palavra de reativação no seu dispositivo.

1. Vá para o [portal do serviço de voz personalizada](https://cris.ai/).

2. Crie uma nova conta com o endereço de e-mail no qual recebeu o convite para o Azure Active Directory. 

    ![Criar nova conta](media/speech-devices-sdk/wake-word-1.png)
 
3.  Depois de iniciar sessão, preencha o formulário, em seguida, clique em **começar a jornada.**

    ![sessão iniciada com êxito](media/speech-devices-sdk/wake-word-3.png)
 
4. O **Word de reativação personalizado** página não está disponível ao público, portanto, não há nenhum link que leva-o aí. Clique ou colar em vez disso, esta ligação: https://cris.ai/customkws.

    ![página oculta](media/speech-devices-sdk/wake-word-4.png)
 
6. Introduza a palavra de reativação de sua escolha, em seguida, **submeter** -lo.

    ![Introduza a palavra de reativação](media/speech-devices-sdk/wake-word-5.png)
 
7. Pode demorar alguns minutos para que os ficheiros sejam gerados. Deverá ver um círculo a girar no separador do browser. Após alguns momentos, uma barra de informações for apresentada a pedir-lhe transferir um `.zip` ficheiro.

    ![receber o ficheiro. zip](media/speech-devices-sdk/wake-word-6.png)

8. Guardar o `.zip` ficheiro para o seu computador. Precisa esse arquivo para implementar a palavra de reativação personalizado para o kit de desenvolvimento, seguindo as instruções em [começar com o SDK de dispositivos de voz](speech-devices-sdk-qsg.md).

9. Pode agora **terminar sessão.**

## <a name="next-steps"></a>Passos Seguintes

Para começar, obtenha uma [conta gratuita do Azure](https://azure.microsoft.com/free/) e inscreva-se para o SDK de dispositivos de voz.

> [!div class="nextstepaction"]
> [Inscreva-se o SDK de dispositivos de voz](get-speech-devices-sdk.md)

