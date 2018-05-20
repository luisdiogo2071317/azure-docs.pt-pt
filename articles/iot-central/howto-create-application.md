---
title: Criar uma aplicação do Azure IoT Central | Microsoft Docs
description: Como um administrador, como criar uma aplicação do Azure IoT Central.
services: iot-central
author: TanmayBhagwat
ms.author: tanmayb
ms.date: 03/20/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 83879c6b81985f61b9fcff665e9f764c1346592e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="create-your-azure-iot-central-application"></a>Criar a sua aplicação do Centro de IoT do Azure

Criar a aplicação do Microsoft Azure IoT Central a [Criar aplicação](https://apps.microsoftiotcentral.com/create) página. Para criar uma aplicação do Azure IoT Central, tem de concluir todos os campos nesta página e, em seguida, escolha **criar**. Este artigo tem mais informações sobre cada um dos campos.

![Criar uma página da aplicação](media\howto-create-application\image1.png)

## <a name="payment-plan"></a>Plano de pagamento

Pode criar uma versão de avaliação ou uma aplicação paga. Saiba mais sobre as aplicações de avaliação e pagas nesta página.

## <a name="application-name"></a>Nome da Aplicação

O nome da sua aplicação é apresentado no **Gestor de aplicação** página e dentro de cada aplicação do Azure IoT Central. Pode escolher qualquer nome para a sua aplicação do Azure IoT Central. Escolha um nome que faz sentido para si e para outros utilizadores na sua organização.

## <a name="application-url"></a>URL da Aplicação

O URL da aplicação é a ligação à sua aplicação. Pode guardar um marcador no seu browser ou partilhar com outras pessoas.

Ao introduzir o nome para a sua aplicação, o seu URL de aplicação é gerado automaticamente. Se preferir, pode escolher um URL diferente para a sua aplicação. Cada URL Central de IoT do Azure tem de ser exclusivo. Verá uma mensagem de erro se o URL que escolher já foi atribuído.

## <a name="directory"></a>Diretório

Apenas em aplicações pagas.

Escolha um inquilino do Azure Active Directory para criar uma aplicação do Azure IoT Central. Um inquilino do Azure Active Directory contém identidades de utilizador, as credenciais e outras informações organizacionais. Várias subscrições do Azure podem ser associadas com um único inquilino do Azure Active Directory.

Se não tiver um inquilino do Azure Active Directory, é criada uma por si quando cria uma subscrição do Azure.

Para obter mais informações, consulte [do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).

## <a name="azure-subscription"></a>Subscrição do Azure

Uma subscrição do Azure permite-lhe criar instâncias de serviços do Azure. Centro de IoT do Azure automaticamente localiza todas as subscrições do Azure que tem acesso e apresenta-os numa lista pendente no **Criar aplicação** página. Escolha uma subscrição do Azure para criar uma nova aplicação de IoT do Azure Central.

Se não tiver uma subscrição do Azure, pode criar um nesta página. Depois de criar a subscrição do Azure, navegue de volta para o **Criar aplicação** página. É apresentada a sua nova subscrição no **subscrição do Azure** pendente.

Para obter mais informações, consulte [subscrições do Azure](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing).

## <a name="region"></a>Região

Apenas em aplicações pagas.

Escolha a região onde pretende criar a sua aplicação Central de IoT do Azure. Normalmente, deverá escolher a região mais próxima fisicamente os seus dispositivos para obter um desempenho ideal.

Para obter mais informações, consulte [regiões do Azure](https://docs.microsoft.com/en-us/azure/guides/developer/azure-developer-guide#azure-regions).

Pode ver as regiões que Central de IoT do Azure está disponível na [produtos disponíveis por região](https://azure.microsoft.com/regions/services/) página.

> [!Note]
> Depois de escolher uma região, mais tarde não é possível mover a aplicação para a região diferente.

## <a name="application-template"></a>Modelo de aplicação

Pode escolher um dos modelos de aplicação disponível para a sua nova aplicação do Azure IoT Central. Um modelo de aplicação pode conter os itens predefinidos, tais como modelos de dispositivos e dashboards para o ajudar a começar a utilizar:

| Modelo de aplicação | Descrição |
| -------------------- | ----------- |
| Aplicação personalizada   | Cria uma aplicação vazia para que possa preencher com os seus próprios dispositivos e modelos de dispositivos. |
| Exemplo Contoso       | Cria uma aplicação que inclui um modelo de dispositivo para um dispositivo ligado simple. Utilize este modelo para começar a explorar os Central de IoT do Azure. |
| Devkits de exemplo       | Cria uma aplicação preparada para ligar um dispositivo MXChip ou Raspberry Pi com modelos do dispositivo. Utilize este modelo, se for um programador de dispositivo conseguirmos uma com o código de um destes dispositivos. |

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como criar uma aplicação do Azure IoT Central, eis o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Administrar a sua aplicação](howto-administer.md)