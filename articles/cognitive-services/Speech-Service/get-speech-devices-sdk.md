---
title: Obter os dispositivos de reconhecimento de voz SDK | Microsoft Docs
description: Saiba como obter acesso para o SDK de dispositivos de reconhecimento de voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 4706ea623dccd2dbb4164bd9cccf22cff121884a
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "35356288"
---
# <a name="get-the-cognitive-services-speech-devices-sdk"></a>Inscrever os dispositivos de reconhecimento de voz serviços cognitivos SDK

## <a name="requesting-access"></a>Pedir acesso

O SDK de dispositivos de reconhecimento de voz está em pré-visualização limitada e tem de estar inscritos no programa. Atualmente, a Microsoft prefers grandes empresas como candidatas para acesso para este produto.

Para obter acesso para o SDK de dispositivos de reconhecimento de voz, siga estes passos:

1. Vá para o SDK de dispositivos de reconhecimento de voz Microsoft [formulário de inscrição](https://aka.ms/sdsdk-signup).
1. Leia o [contrato de licença](speech-devices-sdk-license.md).
1. Se aceitar os termos deste contrato de licença, selecione "Concordo."
1. Responda às perguntas no formulário.
1. Submeta o formulário. 
1. Se o seu endereço de e-mail já não faz parte do Azure Active Directory, receberá um e-mail de convite como a abaixo após a aprovação. Se o seu endereço de e-mail já se encontra no Azure Active Directory, será apresentada uma mensagem de correio eletrónico da equipa do Microsoft Speech após a aprovação e poderá avançar diretamente para [transferir o SDK de dispositivos de reconhecimento de voz](#download-the-speech-devices-sdk).

## <a name="approval-e-mail"></a>Mensagem de correio eletrónico de aprovação

```
From: Microsoft Speech Team from Microsoft (via Microsoft) <invites@microsoft.com> 
Subject: You're invited to the Microsoft organization 
```

![Mensagem de correio eletrónico](media/speech-devices-sdk/get-sdk-1.png)

## <a name="accept-access"></a>Aceitar o acesso
Execute os passos abaixo para associar o Azure Active Directory com o endereço de e-mail que indicou durante o registo. Este processo atribui acesso para o SDK de dispositivos de reconhecimento de voz [transferir site](https://shares.datatransfer.microsoft.com/).

1. Clique em **começar** na mensagem de e-mail que recebeu. Se a sua organização já for um cliente do Office 365, será pedido que inicie sessão e poderá avançar diretamente para o passo 8.

2. Clique em **seguinte** na janela do browser iniciado.

    ![janela de autenticação](media/speech-devices-sdk/get-sdk-2.png)

3. Crie uma conta Microsoft, se ainda não tiver um. Introduza o mesmo endereço de e-mail que tiver recebido o e-mail de convite no passo 6 acima.

    ![criar a conta Microsoft](media/speech-devices-sdk/get-sdk-3.png)

4. Clique em **seguinte** para criar uma palavra-passe.

5. Quando lhe for pedido para verificar o seu e-mail, volte a sua pasta a receber de correio eletrónico para obter o código de verificação que é enviado.
 
7. Colar ou escreva o código de segurança da mensagem de e-mail na caixa de diálogo. Neste exemplo, é "8406". Clique depois em **Seguinte**.

    ![Certifique-se de e-mail](media/speech-devices-sdk/get-sdk-6.png)
 
8. Quando vir a aplicação de painel de acesso na janela do browser, tiver confirmado que o seu endereço de e-mail (no passo 6) agora faz parte do Azure Active Directory. Tem agora acesso para o site de transferência do SDK de dispositivos de reconhecimento de voz.

## <a name="download-the-speech-devices-sdk"></a>Transferir os SDK de dispositivos de reconhecimento de voz

Vá para o [site de transferência do SDK de dispositivos de reconhecimento de voz](https://shares.datatransfer.microsoft.com/) e inicie sessão com a Microsoft Account que criou anteriormente. Agora, pode transferir o SDK de dispositivos de reconhecimento de voz, o código de exemplo associados e o material de referência, seguindo estes passos.

![site de transferência do SDK](media/speech-devices-sdk/get-sdk-7.png)

1. Transfira e instale a ferramenta Aspera Connect quando lhe for pedido pelo browser.

    ![Transferir Aspera ligar](media/speech-devices-sdk/get-sdk-8.png)
 
1. Clique em **Sim** para mudar para Aspera ligar.

    ![mudar para Aspera ligar](media/speech-devices-sdk/get-sdk-9.png)
 
1. Clique em **permitir** para confirmar a transferir os ficheiros com Aspera Connect.

    ![Transferir com Aspera Connect](media/speech-devices-sdk/get-sdk-10.png)
 
1. Feche janela de transferências da Aspera estabelecer a ligação depois dos ficheiros foram transferidos.

    ![Janela de transferências da Aspera estabelecer a ligação](media/speech-devices-sdk/get-sdk-11.png)
 
Por predefinição, os ficheiros são transferidos para o seu **transfere** pasta. Pode terminar este site agora. 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Começar com o SDK de dispositivos de reconhecimento de voz](speech-devices-sdk-qsg.md)
