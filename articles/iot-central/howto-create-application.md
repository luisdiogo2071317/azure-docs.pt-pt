---
title: Criar uma aplicação do Azure IoT Central | Microsoft Docs
description: Como um administrador, como criar uma aplicação do Azure IoT Central.
services: iot-central
ms.service: iot-central
author: tbhagwat3
ms.author: tanmayb
ms.date: 07/09/2018
ms.topic: conceptual
manager: peterpr
ms.openlocfilehash: 1fbe3ea142e1dd738cd341f57d2b8f48b539ac75
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39003771"
---
# <a name="create-your-azure-iot-central-application"></a>Criar a sua aplicação do Azure IoT Central

Criar a aplicação do Microsoft Azure IoT Central a partir da [Criar aplicação](https://apps.microsoftiotcentral.com/create) página. Para criar uma aplicação do Azure IoT Central, tem de concluir todos os campos nesta página e, em seguida, escolha **criar**. Encontrará tem mais informações sobre cada um dos campos abaixo.

![Criar a página de aplicativo](media\howto-create-application\image1.png)

## <a name="payment-plan"></a>Plano de pagamento

Pode criar uma versão de avaliação ou uma aplicação paga. Saiba mais sobre as aplicações de avaliação e pagas no [do Azure IoT Central página de preços](https://azure.microsoft.com/pricing/details/iot-central/)....

## <a name="application-name"></a>Nome da Aplicação

O nome da sua aplicação é apresentado na **Gerenciador de aplicativos** página e dentro de cada aplicação Azure IoT Central. Pode escolher qualquer nome para a sua aplicação do Azure IoT Central. Escolha um nome que faça sentido para si e para outras pessoas na sua organização.

## <a name="application-url"></a>URL da Aplicação

O URL da aplicação é a ligação à sua aplicação. Pode salvar um indicador para ele no seu browser ou partilhá-lo com outras pessoas.

Ao introduzir o nome para a sua aplicação, o URL do aplicativo é gerado automaticamente. Se preferir, pode escolher um URL diferente para a sua aplicação. Cada URL de Central de IoT do Azure tem de ser exclusivo no Azure IoT Central. Verá uma mensagem de erro se o URL que escolher já foi atribuído.

## <a name="directory"></a>Diretório

Só em aplicativos pagos.

Escolha um inquilino do Azure Active Directory para criar uma aplicação do Azure IoT Central. Um inquilino do Azure Active Directory contém as identidades dos utilizadores, credenciais e outras informações organizacionais. Várias subscrições do Azure podem ser associadas um único inquilino do Azure Active Directory.

Se não tiver um inquilino do Azure Active Directory, é criada uma para quando cria uma subscrição do Azure.

Para obter mais informações, consulte [do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).

## <a name="azure-subscription"></a>Subscrição do Azure

Uma subscrição do Azure permite-lhe criar instâncias de serviços do Azure. Azure IoT Central automaticamente encontra todas as subscrições do Azure que tem acesso e exibe-os numa lista suspensa na **Criar aplicação** página. Escolha uma subscrição do Azure para criar uma nova aplicação de IoT do Azure Central.

Se não tiver uma subscrição do Azure, pode criar um da [página de inscrição do Azure](https://aka.ms/createazuresubscription). Depois de criar a subscrição do Azure, navegue de volta para o **Criar aplicação** página. A nova assinatura aparece no **subscrição do Azure** pendente.

Para obter mais informações, consulte [subscrições do Azure](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing).

## <a name="region"></a>Região

Só em aplicativos pagos.

Selecione a região onde pretende criar a sua aplicação de Central de IoT do Azure. Normalmente, deve escolher a região mais próxima fisicamente aos seus dispositivos para obter um desempenho ideal.

Para obter mais informações, consulte [regiões do Azure](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#azure-regions).

Pode ver os em que regiões do Azure IoT Central está disponível na [produtos disponíveis por região](https://azure.microsoft.com/regions/services/) página.

> [!Note]
> Assim que escolher uma região, não pode mais tarde mudar a sua aplicação para uma região diferente.

## <a name="application-template"></a>Modelo de aplicativo

Pode escolher um dos modelos de aplicativos disponíveis para a sua nova aplicação do Azure IoT Central. Um modelo de aplicação pode conter itens predefinidos, tais como modelos de dispositivos e dashboards para ajudá-lo a começar a utilizar.

| Modelo de aplicativo | Descrição |
| -------------------- | ----------- |
| Aplicação personalizada   | Cria uma aplicação vazia para que possa preencher com os seus próprios modelos de dispositivos e dispositivos. |
| Exemplo Contoso       | Cria uma aplicação que inclui um modelo de dispositivo para um dispositivo conectado simple. Utilize este modelo para começar a explorar o Azure IoT Central. |
| Devkits de exemplo       | Cria um aplicativo pronto para estabelecer ligação um dispositivo MXChip ou Raspberry Pi com modelos de dispositivos. Utilize este modelo, se for um desenvolvedor de dispositivo fazer experiências com o código em um desses dispositivos. |

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu criar uma aplicação do Azure IoT Central, este é o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Administrar a sua aplicação](howto-administer.md)