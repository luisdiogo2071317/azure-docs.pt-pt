---
title: Gerir as suas informações de segurança - Azure Active Directory | Documentos da Microsoft
description: Saiba como gerir as suas informações de segurança, incluindo como trabalhar com as definições da verificação de dois passos.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.openlocfilehash: 36d1b4ef9501c94fe385c37f31ee7e8c9f57106e
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197233"
---
# <a name="manage-your-security-info-preview"></a>Gerir as suas informações de segurança (pré-visualização)

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Pode utilizar as informações de segurança para iniciar sessão na sua conta escolar ou profissional ou a repor a palavra-passe.

Quando iniciar sessão, dependendo das definições da sua organização, poderá ver uma caixa de verificação que indica **não perguntar novamente durante X dias**. Esta permite que a caixa de verificação que mantenha-se iniciou sessão no seu dispositivo para o número de dias que permite que o administrador, sem a necessidade de reverification.

## <a name="change-your-info"></a>Alterar as suas informações
Pode atualizar ou adicionar informações de segurança ou alterar o padrão, com base no que é permitido pelo seu administrador e a sua organização.

### <a name="to-change-your-info"></a>Para alterar as suas informações

1. Inicie sessão na sua conta escolar ou profissional.

2. Vá para myapps.microsoft.com, selecione o seu nome no canto superior direito da página e, em seguida, selecione **perfil**.

3. Na **gerir a conta** área, selecione **editar as informações de segurança**.

    ![Ecrã de perfil com a ligação de informações de segurança de editar realçada](media/security-info/security-info-profile.png)

4. Utilize o método predefinido para aprovar o acesso e para ver os detalhes de informações de segurança atual, se o administrador tiver configurado esta experiência para a sua organização.

5. Sobre o **manter a sua conta segura** página, pode:

    - Selecione **adicionar informações de segurança** adicionar métodos adicionais.

    - Selecione **alterar padrão** para alterar o método predefinido.

    - Selecione o **lápis** ícone ao lado de um método existente para atualizar as suas informações.

    ![Ecrã de informações de segurança com informações existentes, editável](media/security-info/security-info-edit.png)

6. Depois de efetuar as alterações, pode sair da página e as suas alterações serão guardadas.

Se não vir estas opções ou não for possível aceder à página de myapps.microsoft.com, é possível que a sua organização utiliza as opções personalizadas ou uma página personalizada. Terá de contactar o administrador para obter mais ajuda.

## <a name="manage-your-security-info-for-a-lost-or-potentially-compromised-device"></a>Gerir as suas informações de segurança para um dispositivo perdido ou potencialmente comprometido

Se perder o dispositivo ou o dispositivo será comprometido, terá de Refazer o processo de verificação para todos os seus dispositivos anteriormente fidedignos.

### <a name="to-manage-your-security-info-for-lost-or-potentially-compromised-devices"></a>Para gerir as suas informações de segurança para dispositivos perdidos ou potencialmente comprometidos

1. Inicie sessão na sua conta escolar ou profissional.

2. Vá para myapps.microsoft.com, selecione o seu nome no canto superior direito da página e, em seguida, selecione **perfil**.

3. Na **gerir a conta** área, selecione **MFA esquecer em dispositivos memorizados**.
    
    Escolher isso significa de opção que terá que passar pelo processo de multi-factor Authentication (MFA) novamente depois de iniciar sessão.

    ![Ecrã de perfil com a ligação de forget realçada](media/security-info/security-info-forget.png)

## <a name="common-problems-and-solutions-with-your-security-info"></a>Problemas comuns e soluções com as informações de segurança

Este artigo ajuda-o a resolver problemas de suas informações de segurança, incluindo problemas relacionados com a verificação de dois passos.

|Problema|Solução|
|-------|--------|
|Não tenho meu telefone comigo|É possível que não tem seu telemóvel consigo, o tempo todo, mas que ainda quer iniciar sessão na sua conta escolar ou profissional. Para corrigir este problema, pode iniciar sessão através de um método de autenticação diferentes que não requeira o seu telefone, por exemplo, o número de telefone do escritório. Para adicionar métodos adicionais às suas informações de segurança, pode seguir os passos a [alterar as suas informações de](#change-your-info) secção.|
|Eu perdi meu telefone ou foi roubado|Infelizmente, pode ocorrer perda de seu telefone ou a ser roubado. Nesta situação, é altamente recomendável que permite que sua organização para que sua equipe de TI pode repor as palavras-passe de aplicação e desmarque quaisquer memorizadas dispositivos da sua lista de dispositivos fidedignos. Pode também se esqueça de seus próprios dispositivos fidedignos ao seguir os passos a [gerir as suas informações de segurança para um dispositivo perdido ou potencialmente comprometido](#manage-your-security-info-for-a-lost-or-potentially-compromised-device) secção.|
|Tenho um novo número de telefone|Existem duas formas para corrigir este problema. Pode iniciar sessão com um método de autenticação alternativo que não requeira o seu número de telefone, tais como e-mail, ou se não for uma opção, pode contactar a sua organização da equipe de TI e tê-las a limpar as definições. Para adicionar métodos adicionais às suas informações de segurança, pode seguir os passos a [alterar as suas informações de](#change-your-info) secção.|
|Meu método de predefinição está errado|Pode atualizar o seu método padrão em suas opções de segurança. Para obter detalhes específicos, pode ir para o [alterar as suas informações de](#change-your-info) secção.|
|Eu não estou a receber uma mensagem de texto ou chamada no meu dispositivo móvel|Se tiver recebido com êxito textos ou chamadas telefónicas para o seu dispositivo móvel no passado, este problema é mais provável com o fornecedor de telefone, não a sua conta. Certifique-se de que tem o sinal de célula boa e que pode receber mensagens de texto e chamadas telefónicas. Pode pedir um amigo de chamada ou texto, como um teste.<br><br>Se com êxito pode receber mensagens de texto e telefone, mas ainda não foi obtido a notificação, pode tentar usando um método diferente. Pode adicionar métodos adicionais às suas informações de segurança ao seguir os passos a [alterar as suas informações de](#change-your-info) secção. Se não tiver outro método para adicionar, pode contactar o suporte da empresa e peça-lhe para limpar as definições para que pode configurar seus métodos da próxima vez que iniciar sessão.<br><br>Se tiver problemas, muitas vezes, devido à receção de célula ruim, recomendamos que utilize a aplicação Microsoft Authenticator no seu dispositivo móvel. A aplicação pode gerar códigos de segurança aleatória que utiliza para iniciar sessão, e esses códigos não requerem qualquer ligação de sinal ou de internet da célula. Para obter mais informações sobre a aplicação Microsoft Authenticator, consulte a [começar com a aplicação Microsoft Authenticator](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to) artigo.|
|Nenhuma das opções nesta tabela tem resolveu o meu problema|Se já tentou estes passos de resolução de problemas, mas é ainda em execução em problemas; Contacte o suporte da empresa, eles devem ser capazes de ajudá-lo.|

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre as informações de segurança de [descrição geral de informações (pré-visualização) de segurança](user-help-security-info-overview.md).

- Saiba mais sobre a verificação de dois passos no [descrição geral de verificação de dois passos](user-help-two-step-verification-overview.md) artigo. 

- Siga um dos seguintes artigos de procedimentos para saber mais sobre como configurar seus dispositivos na área de informações de segurança:

    - [Configurar as informações de segurança para utilizar uma aplicação de autenticação](security-info-setup-auth-app.md)

    - [Configurar as informações de segurança para utilizar chamadas telefónicas](security-info-setup-phone-number.md)

    - [Definir informações de segurança para utilizar as mensagens de texto](security-info-setup-text-msg.md)

    - [Configurar as informações de segurança para utilizar o e-mail](security-info-setup-email.md)

    - [Definir informações de segurança para utilizar perguntas de segurança](security-info-setup-questions.md)

- Repor a palavra-passe, se tiver perdido ou esquecidas, do [portal de reposição de palavra-passe](https://passwordreset.microsoftonline.com/) ou siga os passos a [repor a palavra-passe profissional ou escolar](user-help-reset-password.md) artigo.

- Obter solução de problemas dicas e ajuda para problemas de início de sessão no [não pode iniciar sessão na sua conta Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) artigo.
