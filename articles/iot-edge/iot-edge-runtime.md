---
title: Compreender o tempo de execução do Azure IoT Edge | Documentos da Microsoft
description: Saiba mais sobre o tempo de execução do Azure IoT Edge e como ela capacita os seus dispositivos periféricos
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 08/13/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 9c9c04a8310a46605cf5733131db1418b7cb7f7a
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47218789"
---
# <a name="understand-the-azure-iot-edge-runtime-and-its-architecture"></a>Compreender o tempo de execução do Azure IoT Edge e respetiva arquitetura

O runtime do IoT Edge é uma coleção de programas que têm de ser instalado num dispositivo para o mesmo ser considerado um dispositivo IoT Edge. Coletivamente, os componentes do runtime do IoT Edge permitem que os dispositivos do IoT Edge receber o código para executar na periferia e comunicam os resultados. 

O runtime do IoT Edge efetua as seguintes funções em dispositivos IoT Edge:

* Instala e atualiza as cargas de trabalho no dispositivo.
* Mantém as normas de segurança do Azure IoT Edge no dispositivo.
* Garante que [módulos do IoT Edge] [lnk-módulos] estão sempre em execução.
* Reporta o estado de funcionamento dos módulos à cloud, para monitorização remota.
* Facilita a comunicação entre os dispositivos de folha a jusante e dispositivos IoT Edge.
* Facilita a comunicação entre os módulos no dispositivo IoT Edge.
* Facilita a comunicação entre o dispositivo do Azure IoT e a cloud.

![Runtime do IoT Edge comunica informações e o estado de funcionamento do módulo para o IoT Hub][1]

As responsabilidades do runtime do IoT Edge enquadram-se em duas categorias: gestão de comunicação e o módulo. Estas duas funções são executadas por dois componentes que compõem o runtime do IoT Edge. O hub de IoT Edge é responsável pela comunicação, enquanto o agente do IoT Edge gerencia a implantação e os módulos de monitorização. 

O hub do Edge e o agente do Edge são módulos, assim como qualquer outro módulo em execução num dispositivo IoT Edge. 

## <a name="iot-edge-hub"></a>Hub do IoT Edge

O hub do Edge é um dos dois módulos que constituem o tempo de execução do Azure IoT Edge. Ele atua como um proxy local para o IoT Hub ao expor os pontos de extremidade de protocolo mesmo como o IoT Hub. Esta consistência significa que os clientes (se dispositivos ou módulos) pode ligar-se para o runtime do IoT Edge como se fossem ao IoT Hub. 

>[!NOTE]
>Hub do Edge suporta clientes que se ligam através de MQTT ou AMQP. Não suporta clientes que utilizam HTTP. 

O hub do Edge não é uma versão completa do IoT Hub em execução localmente. Existem algumas coisas que o hub do Edge silenciosamente delega para o IoT Hub. Por exemplo, o hub do Edge encaminha pedidos de autenticação para o IoT Hub quando um dispositivo primeiro tenta ligar. Depois da primeira ligação está estabelecida, informações de segurança é colocado em cache localmente pelo hub do Edge. São permitidas ligações subsequentes do que o dispositivo sem precisarem autenticar para a cloud. 

>[!NOTE]
>O tempo de execução tem de estar ligado sempre que tentar autenticar um dispositivo.

Para reduzir a largura de banda sua solução de IoT Edge utiliza, o hub do Edge otimiza o número de conexões reais são efetuadas para a cloud. Hub do Edge usa a lógicas ligações de clientes, como módulos ou dispositivos de folha e combina-os para uma única ligação física para a cloud. Os detalhes desse processo são transparentes para o restante da solução. Os clientes acha que eles têm sua própria ligação para a cloud, apesar de tudo o que está a ser enviados pela mesma conexão. 

![Hub do Edge atua como um gateway entre vários dispositivos físicos e a cloud][2]

Hub do Edge pode determinar se está ligado ao IoT Hub. Se a ligação for perdida, o hub do Edge guarda as mensagens ou as atualizações de duplo localmente. Depois de uma conexão for restabelecida, ele sincroniza todos os dados. A localização utilizada para esta cache temporário é determinada por uma propriedade do duplo do módulo de hub do Edge. O tamanho da cache não está limitado e irá aumentar, desde que o dispositivo tem capacidade de armazenamento. 

### <a name="module-communication"></a>Comunicação de módulo

Hub do Edge facilita a comunicação de módulo de módulo. Utilizar o Hub do Edge como um mediador de mensagens mantém módulos independentes entre si. Módulos só precisam de especificar as entradas em que aceite mensagens e as saídas para que eles escrevem mensagens. Um desenvolvedor de soluções, em seguida, stitches essas entradas e saídas em conjunto, para que os módulos de processam os dados na ordem específica para essa solução. 

![Hub do Edge facilita a comunicação de módulo de módulo][3]

Para enviar dados para o hub do Edge, um módulo chama o método de SendEventAsync. O primeiro argumento especifica em qual saída deve enviar a mensagem. O seguinte pseudocódigo envia uma mensagem no output1:

   ```csharp
   ModuleClient client = new ModuleClient.CreateFromEnvironmentAsync(transportSettings); 
   await client.OpenAsync(); 
   await client.SendEventAsync(“output1”, message); 
   ```

Para receber uma mensagem, registe um retorno de chamada que processa as mensagens recebidas numa entrada específica. O seguinte pseudocódigo registra o messageProcessor de função a ser utilizada para processar todas as mensagens recebidas no input1:

   ```csharp
   await client.SetEventHandlerAsync(“input1”, messageProcessor, userContext);
   ```

O desenvolvedor de soluções é responsável por especificar as regras que determinam como o hub do Edge passa mensagens entre módulos. Regras de encaminhamento são definidas na cloud e enviadas para o hub do Edge no seu dispositivo duplo. A mesma sintaxe para as rotas do IoT Hub é usada para definir rotas entre módulos no Azure IoT Edge. 

<!--- For more info on how to declare routes between modules, see []. --->   

![Rotas entre módulos][4]

## <a name="iot-edge-agent"></a>Agente do IoT Edge

O agente do IoT Edge é o outro módulo que compõe o tempo de execução do Azure IoT Edge. Ele é responsável por instanciar módulos, garantindo que continuam em execução e comunicar o estado dos módulos de volta para o IoT Hub. Assim como qualquer outro módulo, o agente do Edge usa seu módulo duplo para armazenar estes dados de configuração. 

O [daemon de segurança de IoT Edge](iot-edge-security-manager.md) inicia o agente do Edge na inicialização do dispositivo. O agente obtém seu módulo duplo a partir do IoT Hub e inspeciona o manifesto de implantação. O manifesto de implantação é um ficheiro JSON que declara os módulos que têm de ser iniciado. 

Cada item no manifesto de implantação contém informações específicas sobre um módulo e é utilizado pelo agente do Edge para controlar o ciclo de vida do módulo. Algumas das propriedades mais interessantes são: 

* **Settings.Image** – a imagem de contentor que o agente do Edge utiliza para iniciar o módulo. O agente do Edge deve ser configurado com as credenciais para o registo de contentor, se a imagem estiver protegida por uma palavra-passe. As credenciais para o registo de contentor pode ser configurado remotamente utilizando o manifesto de implantação ou no próprio dispositivo Edge ao atualizar o `config.yaml` ficheiro na pasta do programa de IoT Edge.
* **settings.createOptions** – uma cadeia de caracteres é passada diretamente para o daemon do Docker ao iniciar o contentor de um módulo. A adição de opções de Docker nessa propriedade permite para obter opções avançadas, como a porta de reencaminhamento ou montar volumes para o contentor de um módulo.  
* **estado** – o estado em que o agente do Edge coloca o módulo. Este valor é geralmente definido para *em execução* como a maioria das pessoas querem o agente do Edge para iniciar imediatamente todos os módulos no dispositivo. No entanto, poderia especificar o estado inicial de um módulo a ser parado e aguarde uma hora no futuro informar o agente do Edge para iniciar um módulo. O agente do Edge relata o status de cada módulo para a cloud nas propriedades comunicadas. Uma diferença entre a propriedade pretendida e a propriedade comunicada é um indicador de um dispositivo com comportamento inadequado. Os Estados suportados são:
   * A transferir
   * A executar
   * Estado de funcionamento incorreto
   * Com Falhas
   * Parada
* **restartPolicy** – como o agente do Edge reinicia um módulo. Os valores possíveis incluem:
   * Nunca – o agente do Edge nunca reinicia o módulo.
   * onFailure - se o módulo falhar, o agente do Edge reinicia-lo. Se o módulo foi encerrado corretamente, o agente do Edge não reiniciá-lo.
   * Mau estado de funcionamento - se o módulo falha ou for considerado mau estado de funcionamento, o agente do Edge reinicia-lo.
   * Sempre - se o módulo falha, é considerado em mau estado de funcionamento ou encerra de qualquer forma, o agente do Edge reinicia-lo. 

O agente do IoT Edge envia a resposta de runtime para o IoT Hub. Aqui está uma lista de possíveis respostas:
  * 200 - OK
  * 400 - a configuração de implementação está incorreto ou é inválido.
  * 417 - o dispositivo não tem um conjunto de configurações de implementação.
  * 412 - a versão de esquema na configuração da implementação é inválida.
  * 406 - o dispositivo de limite é de relatórios de status offline ou não de envio.
  * 500 - Erro no runtime do edge.

### <a name="security"></a>Segurança

O agente do IoT Edge desempenha um papel fundamental na segurança de um dispositivo IoT Edge. Por exemplo, ele executa ações como verificar uma imagem do módulo antes de iniciá-los. 

Para obter mais informações sobre a estrutura de segurança do Azure IoT Edge, saiba mais sobre o [Gestor de segurança de IoT Edge](iot-edge-security-manager.md)

## <a name="next-steps"></a>Passos Seguintes

[Compreender o Azure IoT Edge certificados][lnk-certs]

<!-- Images -->
[1]: ./media/iot-edge-runtime/Pipeline.png
[2]: ./media/iot-edge-runtime/Gateway.png
[3]: ./media/iot-edge-runtime/ModuleEndpoints.png
[4]: ./media/iot-edge-runtime/ModuleEndpointsWithRoutes.png

<!-- Links -->
[lnk-certs]: iot-edge-certs.md
