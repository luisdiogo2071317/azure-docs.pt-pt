---
title: Serviço de fornecimento de dispositivos do IoT Hub - conceitos de aprovisionamento automático
description: Este artigo fornece uma descrição geral conceptual das fases de auto-aprovisionamento de dispositivos, utilizando o serviço de aprovisionamento de dispositivos do IoT, o IoT Hub e SDKs de cliente.
services: iot-dps
keywords: ''
author: BryanLa
ms.author: bryanla
ms.date: 03/27/2018
ms.topic: conceptual
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: ''
ms.openlocfilehash: e743f40a1f8ff71fe93f14217b410df348d9903d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="auto-provisioning-concepts"></a>Conceitos de aprovisionamento automático

Conforme descrito no [descrição geral](about-iot-dps.md), o serviço de aprovisionamento de dispositivos é um serviço de programa auxiliar que permite just-in-time o aprovisionamento de dispositivos a um IoT hub, sem necessidade de intervenção humana. Após o aprovisionamento com êxito, os dispositivos se ligam diretamente com o seu IoT Hub designado. Este processo é denominado automática de aprovisionamento e fornece que um registo de out of box e a configuração inicial de experiência de dispositivos.

## <a name="overview"></a>Descrição geral

Do Azure IoT automática aprovisionamento pode ser dividido em três fases:

1. **A configuração do serviço** -uma configuração única das instâncias do Azure IoT Hub e o serviço de aprovisionamento de dispositivos IoT Hub, estabelecê-las e criar a ligação entre eles.

   > [!NOTE]
   > Independentemente do tamanho da sua solução de IoT, mesmo que planeia suportar milhões de dispositivos, este é um **configuração única**.

2. **Inscrição de dispositivos** -o processo de efetuar a instância de serviço de aprovisionamento de dispositivos em consideração os dispositivos que irão tentar registar no futuro. [Inscrição](concepts-service.md#enrollment) é conseguido ao configurar informações de identidade de dispositivo no serviço de aprovisionamento, como uma "inscrição individuais" para um único dispositivo ou uma "inscrição de grupo" para vários dispositivos. Identidade baseada no [mecanismo de atestado](concepts-security.md#attestation-mechanism) o dispositivo foi concebido para utilizar, que permite que o serviço de aprovisionamento atestem a autenticidade do dispositivo durante o registo:

   - **TPM**: configurado como uma "inscrição individuais", a identidade de dispositivo é com base no ID de registo TPM e a chave de endossamento público. Dado que o TPM é um [especificação]((https://trustedcomputinggroup.org/work-groups/trusted-platform-module/)), o serviço espera apenas atestem pela especificação de, independentemente da implementação de TPM (hardware ou software). Consulte [aprovisionamento de dispositivos: atestado de identidade com TPM](https://azure.microsoft.com/blog/device-provisioning-identity-attestation-with-tpm/) para obter detalhes sobre atestado baseado no TPM. 

   - **X509**: configurado como um "inscrição individuais" ou "inscrição de grupo", a identidade de dispositivo baseia-se num certificado x. 509 digital que é carregado para a inscrição, como um ficheiro. pem ou. cer.

   > [!IMPORTANT]  
   > Apesar de não é um pré-requisito para utilizar os serviços de aprovisionamento de dispositivos, recomendamos vivamente que a utilização do dispositivo um módulo de segurança de Hardware (HSM) para armazenar informações de identidade de dispositivo confidenciais, tais como as chaves e certificados x. 509.

3. **Registo de dispositivos e configuração** - iniciada após arranque pelo software de registo, o que é criada com um SDK adequado para o mecanismo de dispositivo e do atestado de cliente do serviço de aprovisionamento de dispositivos. O software estabelece uma ligação ao serviço de aprovisionamento para a autenticação do dispositivo e registo subsequente no IoT Hub. Após o registo com êxito, o dispositivo é fornecido com os IoT Hub exclusivo ID e a ligação de informações do dispositivo, permitindo que a configuração inicial de extração e iniciar o processo de telemetria. Em ambientes de produção, nesta fase pode ocorrer semanas ou meses após as duas fases anteriores.

## <a name="roles-and-operations"></a>Funções e operações

As fases abordadas na secção anterior podem abranger semanas ou meses, devido a realities de produção, como o tempo de fabrico, envio, processo customs, etc. Além disso, estes podem abranger atividades em várias funções fornecidas as várias entidades envolvidas. Esta secção assume um mais aprofundado de olhos a várias funções e as operações relacionadas com cada fase, em seguida, ilustra o fluxo um diagrama de sequência. 

Também aprovisionamento automático coloca requisitos no fabricante do dispositivo, específico para ativar o mecanismo de atestado. Operações de fabrico também pode ocorrer independentemente da temporização de aprovisionamento automático fases, especialmente nos casos em que os novos dispositivos são fornecidos após o aprovisionamento automático já tiver sido estabelecido.

Uma série de inícios rápidos são fornecidos na tabela de índice para a esquerda, para ajudar a explicar o aprovisionamento automático através de experiência prática. Para facilitar/simplificar o learning processo, o software é utilizado para simular um dispositivo físico para a inscrição e de registo. Algumas inícios rápidos requerem a satisfazer as operações para várias funções, incluindo operações para funções inexistente, devido à natureza simulada de inícios rápidos.

| Função | Operação | Descrição | 
|------| --------- | ------------| 
| Fabricante | Codificar o URL de identidade e de registo | O fabricante com base no mecanismo de atestado utilizado, é responsável pela codificação as informações de identidade de dispositivo e o URL de registo do serviço de aprovisionamento de dispositivos.<br><br>**Inícios Rápidos**: uma vez que o dispositivo é simulado, não há nenhuma função de fabricante. Consulte a função de programador para obter detalhes sobre como pode obter estas informações, que são utilizadas na codificação um exemplo de aplicação de registo. |  
| | Fornecer a identidade de dispositivo | Como o originador as informações de identidade de dispositivo, o fabricante é responsável por comunicá-lo para o operador (ou um agente designado) ou inscrevê-lo diretamente para o serviço de aprovisionamento de dispositivos através de APIs.<br><br>**Inícios Rápidos**: uma vez que o dispositivo é simulado, não há nenhuma função de fabricante. Consulte a função de operador para obter detalhes sobre como depara-se a identidade de dispositivo, que é utilizada para inscrever um dispositivo simulado na sua instância do serviço de aprovisionamento de dispositivos. | 
| Operador | Configurar o aprovisionamento automático | Esta operação corresponde com a primeira fase de aprovisionamento automático.<br><br>**Inícios Rápidos**: executar a função de operador, configurar o serviço de aprovisionamento de dispositivos e instâncias do IoT Hub na sua subscrição do Azure. | Aprovisionamento de configuração automática |
|  | Inscrever a identidade de dispositivo | Esta operação corresponde com a segunda fase de aprovisionamento automático.<br><br>**Inícios Rápidos**: executar a função de operador, inscrever o seu dispositivo simulado na sua instância do serviço de aprovisionamento de dispositivos. A identidade de dispositivo é determinada pelo método do atestado que está a ser simulated no início rápido (TPM ou x. 509). Consulte a função de programador para detalhes de atestado. | 
| Serviço de aprovisionamento de dispositivos,<br>IoT Hub | \<todas as operações\> | Para ambas as uma implementação de produção com dispositivos físicos e inícios rápidos com dispositivos simulados, estas funções são verdadeiras através de serviços de IoT que configurar na sua subscrição do Azure. As funções/operações funciona exatamente da mesma, como os serviços de IoT estão indifferent para aprovisionamento de físico vs dispositivos simulados. | 
| Programador | Compilação/implementar o software de registo | Esta operação corresponde com a terceira fase de aprovisionamento automático. O programador é responsável por criar e implementar o software de registo para o dispositivo, utilizando o SDK adequado.<br><br>**Inícios Rápidos**: A aplicação de registo de exemplo que cria simula um dispositivo real, para a plataforma de idioma à escolha, o que é executada na estação de trabalho (em vez de implementar a política para um dispositivo físico). A aplicação de registo efetua as mesmas operações como um implementados num dispositivo físico. Especifique o método de atestado (certificado TPM ou os x. 509), juntamente com o URL de registo e o "ID de âmbito" da sua instância do serviço de aprovisionamento de dispositivos. A identidade de dispositivo é determinada pela lógica de atestado de SDK no tempo de execução, com base no método que especificar: <ul><li>**Atestado de TPM** -a estação de trabalho de desenvolvimento executa um [aplicação simulador TPM](how-to-use-sdk-tools.md#trusted-platform-module-tpm-simulator). Depois de executar, uma aplicação individual é utilizada para extrair o TPM "Chave de endossamento" e "ID de registo" para utilização na inscrição a identidade do dispositivo. A lógica de atestado de SDK também utiliza o simulador durante o registo para apresentar um token SAS assinado para autenticação e a verificação de inscrição.</li><li>**X509 atestado** -utilizar uma ferramenta para [gerar um certificado](how-to-use-sdk-tools.md#x509-certificate-generator). Depois de o gerado, crie o ficheiro de certificado necessário para utilização no registo. A lógica de atestado de SDK também utiliza o certificado durante o registo, de modo a apresentar para a autenticação e a verificação de inscrição.</li></ul> | 
| Dispositivo | Lido e registar | Esta operação corresponde com a terceira fase de aprovisionamento automático de concluído, o software de registo de dispositivos criado pelo programador. Consulte a função de programador para obter mais detalhes. Após o primeiro arranque: <ol><li>A aplicação estabelece ligação com a instância de serviço de aprovisionamento de dispositivos, pelo URL e o serviço "ID de âmbito" especificado durante o desenvolvimento global.</li><li>Assim que estiver ligado, o dispositivo é autenticado contra o método de atestado e a identidade especificada durante a inscrição.</li><li>Uma vez autenticado, o dispositivo está registado com a instância IoT Hub especificada pela instância do serviço de aprovisionamento.</li><li>Após o registo com êxito um ponto final de Hub de IoT e ID de dispositivo exclusivo são devolvidos para a aplicação de registo para comunicar com o IoT Hub.</li><li> A partir daí, o dispositivo pode obter sua inicial [dispositivo duplo](~/articles/iot-hub/iot-hub-devguide-device-twins.md) de estado para a configuração e iniciar o processo de criação de relatórios de dados de telemetria.</li></ol>**Inícios Rápidos**: uma vez que o dispositivo é simulado, o software de registo é executado na sua estação de trabalho de desenvolvimento.| 

O diagrama a seguir resume as funções e a sequência de operações durante o aprovisionamento automático do dispositivo:
<br><br>
![Sequência de automática de aprovisionamento de um dispositivo](./media/concepts-auto-provisioning/sequence-auto-provision-device-vs.png) 

> [!NOTE]
> Opcionalmente, o fabricante também pode efetuar a operação de "Inscrição identidade de dispositivo" com APIs de serviço de aprovisionamento de dispositivos (em vez de através do operador). Para ver um debate detalhado desta sequência e muito mais, consulte o [Zero registo de dispositivos de touch com o Azure IoT vídeo](https://myignite.microsoft.com/sessions/55087) (começando marcador 41:00)

## <a name="next-steps"></a>Passos Seguintes

Poderá ser útil marcar este artigo como um ponto de referência, como estiver a trabalhar através de inícios rápidos correspondentes de aprovisionamento automático. 

Comece por concluir um guia de introdução "Configurar o aprovisionamento automático", que melhor se adapta às sua preferência de ferramenta de gestão, o que orienta a fase de "Configuração de serviço":

- [Configurar o aprovisionamento automático utilizando a CLI do Azure](quick-setup-auto-provision-cli.md)
- [Configurar o aprovisionamento automático através do Portal do Azure](quick-setup-auto-provision.md)
- [Configurar o aprovisionamento automático utilizando um modelo do Resource Manager](quick-setup-auto-provision-rm.md)

Em seguida, prossiga com um guia de introdução "Auto-aprovisionar um dispositivo simulado", que se adequa aos seus mecanismo de atestado de dispositivo e a preferência de SDK do serviço de aprovisionamento de dispositivos/idioma. Este guia de introdução, pode percorrer as fases de "Inscrição de dispositivos" e "registo de dispositivos e a configuração": 

|  | Mecanismo de atestado de dispositivo simulada | Início rápido SDK/idioma |  |
|--|--|--|--|
|  | Trusted Platform Module (TPM) | [C](quick-create-simulated-device.md)<br>[Java](quick-create-simulated-device-tpm-java.md)<br>[C#](quick-create-simulated-device-tpm-csharp.md)<br>[Python](quick-create-simulated-device-tpm-python.md) |  |
|  | Certificado X.509 | [C](quick-create-simulated-device-x509.md)<br>[Java](quick-create-simulated-device-x509-java.md)<br>[C#](quick-create-simulated-device-x509-csharp.md)<br>[Node.js](quick-create-simulated-device-x509-node.md)<br>[Python](quick-create-simulated-device-x509-python.md) |  |




