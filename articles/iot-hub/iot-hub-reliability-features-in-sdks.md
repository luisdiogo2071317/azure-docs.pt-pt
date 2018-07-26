---
title: Como gerir a conectividade e o sistema de mensagens confiável através de SDKs de dispositivo do IoT Hub do Azure
description: Saiba como melhorar a conectividade do dispositivo e de mensagens ao utilizar os SDKs do dispositivo do IoT Hub do Azure
services: iot-hub
keywords: ''
author: yzhong94
ms.author: yizhon
ms.date: 07/07/2018
ms.topic: article
ms.service: iot-hub
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 7717c026b4c09f47159fe62640f9a2eedf409d30
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247686"
---
# <a name="how-to-manage-connectivity-and-reliable-messaging-using-azure-iot-hub-device-sdks"></a>Como gerir a conectividade e o sistema de mensagens confiável através de SDKs de dispositivo do IoT Hub do Azure

Este guia fornece diretrizes de alto nível para criar aplicações de dispositivo resiliente, ao tirar partido das funcionalidades de mensagens fiáveis e conectividade em SDKs de dispositivo do IoT do Azure. O objetivo deste artigo é ajudar a responder a perguntas e lidar com esses cenários:

- gerir ligação de rede
- Gerir a alternância entre diferentes conexões de redes
- restabelecimento de ligação devido a erros de ligação transitória do serviço de gestão

Detalhes de implementação pode variam consoante o idioma, consulte a documentação da API ligada ou SDK específica para obter mais detalhes.

- [SDK de C/Python/iOS](https://github.com/azure/azure-iot-sdk-c)
- [SDK do .NET](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
- [SDK Java](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md)
- [Nó de SDK](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them)


## <a name="designing-for-resiliency"></a>Conceber para resiliência

Dispositivos IoT frequentemente contam com ligações de rede não contínuos e/ou instável, como o GSM ou da satélite. Além disso, ao interagir com serviços baseados na nuvem, podem ocorrer erros devido a condições temporárias, como a disponibilidade do serviço intermitente e falhas de nível de infra-estrutura (geralmente conhecidas como falhas transitórias). Uma aplicação em execução num dispositivo tem de gerir a conexão e mecanismos de restabelecimento de ligação, bem como a lógica de repetição para a enviar/receber mensagens. Além disso, os requisitos da estratégia de repetição são altamente dependem do cenário de IoT, que o dispositivo no qual participam no, e o dispositivo contexto e capacidades.

Os SDKs do dispositivo do IoT Hub do Azure têm como objetivo para simplificar a ligar e comunicar da cloud para o dispositivo e dispositivo-para-cloud, fornecendo uma maneira eficiente e abrangente da conexão e enviar/receber mensagens de e para o IoT Hub do Azure. Os desenvolvedores também podem modificar a implementação existente para desenvolver a estratégia de repetição certa para um determinado cenário.

Os recursos relevantes do SDK que oferecem suporte a conectividade e o sistema de mensagens confiável são abordados nas seções a seguir.

## <a name="connection-and-retry"></a>Ligação e tente novamente

Esta seção fornece uma descrição geral dos padrões de restabelecimento de ligação e tente novamente disponíveis na gestão de ligações, orientações de implementação para utilizar a política de repetição diferentes na sua aplicação de dispositivo e APIs relevantes para os SDKs do dispositivo.

### <a name="error-patterns"></a>Padrões de erro
Falhas de ligação podem ocorrer em muitos níveis:

-  Erros de rede, como um soquete desligado e erros de resolução de nomes
- Erros de nível de protocolo para HTTP, AMQP e MQTT de transporte, como ligações desanexado ou sessões expirou
- Erros de nível de aplicativo que resultam de qualquer um dos erros de locais, como credenciais inválidas ou comportamento, como que excediam a cota ou limitação de serviço

Os SDKs do dispositivo detectar erros em todos os três níveis.  Erros relacionados com o sistema operacional e erros de hardware não são detetados e processados pelos SDKs do dispositivo.  O design se baseia [The transitório falhas manipulação orientações](https://docs.microsoft.com/azure/architecture/best-practices/transient-faults#general-guidelines) do Centro de arquitetura do Azure.

### <a name="retry-patterns"></a>Repita os padrões de

O processo geral para repetição quando forem detetados erros de ligação é: 
1. O SDK Deteta o erro e o erro associado na rede, o protocolo ou o aplicativo.
2. Com base no tipo de erro, as utilizações SDK o erro de filtro para decidir se repita precisa de ser realizada.  Se um **erro irrecuperável** é identificado pelo SDK, vão ser paradas operações (ligação e de envio/receção) e o SDK irá informar o utilizador. Um erro irrecuperável é um erro que o SDK pode identificar e determinar que não pode ser recuperado, por exemplo, uma autenticação ou um erro de ponto de extremidade ruim.
3. Se um **erro recuperável** é identificado, o SDK começa voltar a tentar com a política de repetição especificada até que expire o tempo limite definido.
4. Quando o tempo limite definido expira, o SDK pára a tentar ligar ou enviar e informa ao usuário.
5.  O SDK permite ao utilizador anexar um retorno de chamada para receber as alterações de estado da ligação. 

As políticas de repetição três são fornecidas:
- **Término exponencial com interferência**: Esta é a política de repetição predefinida aplicada.  Ele tende a ser agressivo no início, mais lento e, em seguida, chega a um atraso máximo que não seja excedido.  O design se baseia [repita a orientação do Centro de arquitetura do Azure](https://docs.microsoft.com/azure/architecture/best-practices/retry-service-specific).
- **Repetição personalizada**: pode implementar uma política de repetição personalizada e inseri-lo no RetryPolicy consoante o idioma que selecionou. Pode criar uma política de repetição é adequada para o seu cenário.  Isso não está disponível no SDK de C.
- **Nenhuma repetição**: há uma opção para definir a política de repetição para "nenhuma repetição," que desativa a lógica de repetição.  O SDK tenta ligar-se de uma vez e enviar uma mensagem de uma vez, partindo do princípio de que a ligação ser estabelecida. Esta política, normalmente, seria usada em casos em que existem preocupações de largura de banda ou custo.   Se esta opção for escolhida, as mensagens que não obedeçam a enviar são perdidas e não podem ser recuperadas. 

### <a name="retry-policy-apis"></a>APIs de política de repetição

   | SDK | Método SetRetryPolicy | Implementações de política | Orientações de implementação |
   |-----|----------------------|--|--|
   |  C/Python/iOS  | [IOTHUB_CLIENT_RESULT IoTHubClient_SetRetryPolicy](https://github.com/Azure/azure-iot-sdk-c/blob/2018-05-04/iothub_client/inc/iothub_client.h#L188)        | **Predefinido**: [IOTHUB_CLIENT_RETRY_EXPONENTIAL_BACKOFF](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)<BR>**Personalizada:** utilização disponível [retryPolicy](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)<BR>**Nenhuma repetição:** [IOTHUB_CLIENT_RETRY_NONE](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)  | [Implementação de C/Python/iOS](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#)  |
   | Java| [SetRetryPolicy](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.sdk.iot.device._device_client_config.setretrypolicy?view=azure-java-stable)        | **Predefinido**: [ExponentialBackoffWithJitter classe](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/NoRetry.java)<BR>**Personalizada:** implementar [RetryPolicy interface](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/RetryPolicy.java)<BR>**Nenhuma repetição:** [classe NoRetry](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/NoRetry.java)  | [Implementação de Java](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md) |[SDK do .NET](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
   | .NET| [DeviceClient.SetRetryPolicy](/dotnet/api/microsoft.azure.devices.client.deviceclient.setretrypolicy?view=azure-dotnet#Microsoft_Azure_Devices_Client_DeviceClient_SetRetryPolicy_Microsoft_Azure_Devices_Client_IRetryPolicy) | **Predefinido**: [ExponentialBackoff classe](/dotnet/api/microsoft.azure.devices.client.exponentialbackoff?view=azure-dotnet)<BR>**Personalizada:** implementar [IRetryPolicy interface](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.iretrypolicy?view=azure-dotnet)<BR>**Nenhuma repetição:** [classe NoRetry](/dotnet/api/microsoft.azure.devices.client.noretry?view=azure-dotnet) | [Implementação em c#]() |
   | Nó| [setRetryPolicy](/javascript/api/azure-iot-device/client?view=azure-iot-typescript-latest#azure_iot_device_Client_setRetryPolicy) | **Predefinido**: [ExponentialBackoffWithJitter classe](/javascript/api/azure-iot-common/exponentialbackoffwithjitter?view=azure-iot-typescript-latest)<BR>**Personalizada:** implementar [RetryPolicy interface](/javascript/api/azure-iot-common/retrypolicy?view=azure-iot-typescript-latest)<BR>**Nenhuma repetição:** [classe NoRetry](/javascript/api/azure-iot-common/noretry?view=azure-iot-typescript-latest) | [Implementação de nó](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them) |
   

Seguem-se exemplos de código que ilustram este fluxo. 

#### <a name="net-implementation-guidance"></a>Orientações de implementação do .NET

O exemplo de código abaixo mostra como definir e definir a política de repetição predefinida:

   ```csharp
   # define/set default retry policy
   RetryPolicy retryPolicy = new ExponentialBackoff(int.MaxValue, TimeSpan.FromMilliseconds(100), TimeSpan.FromSeconds(10), TimeSpan.FromMilliseconds(100));
   SetRetryPolicy(retryPolicy);
   ```

Para evitar a utilização elevada da CPU, as repetições são limitadas, se o código falhar imediatamente (por exemplo, quando não existe nenhuma rede ou a rota para o destino), para que o tempo mínimo para executar a próxima repetição é de 1 segundo. 

Se o serviço está a responder com um erro de limitação, a política de repetição é diferente e não pode ser alterada através da API pública:

   ```csharp
   # throttled retry policy
   RetryPolicy retryPolicy = new ExponentialBackoff(RetryCount, TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(60), TimeSpan.FromSeconds(5));
   SetRetryPolicy(retryPolicy);
   ```

O mecanismo de repetição irá parar quando `DefaultOperationTimeoutInMilliseconds`, que está definido em 4 minutos.

#### <a name="other-languages-implementation-guidance"></a>Outras diretrizes de implementação de idiomas
Para outros idiomas, consulte a documentação de implementação abaixo.  Exemplos que demonstram a utilização de APIs são fornecidas no repositório de política de repetição.
- [SDK de C/Python/iOS](https://github.com/azure/azure-iot-sdk-c)
- [SDK do .NET](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
- [SDK Java](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md)
- [Nó de SDK](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them)

## <a name="next-steps"></a>Passos Seguintes
- [Utilizar o dispositivo e SDKs de serviço](.\iot-hub-devguide-sdks.md)
- [Utilizar o SDK do dispositivo IoT para C](.\iot-hub-device-sdk-c-intro.md)
- [Desenvolver para dispositivos restritos](.\iot-hub-devguide-develop-for-constrained-devices.md)
- [Desenvolver para dispositivos móveis](.\iot-hub-how-to-develop-for-mobile-devices.md)