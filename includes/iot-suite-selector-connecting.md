> [!div class="op_single_selector"]
> * [C em Windows](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [C em Linux](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [Node.js (genérico)](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> * [Node.js no Raspberry Pi](../articles/iot-suite/iot-suite-connecting-pi-node.md)
> * [C no Raspberry Pi](../articles/iot-suite/iot-suite-connecting-pi-c.md)

Neste tutorial, implementa um **Chiller** dispositivo que envia a telemetria seguinte para a monitorização remota [solução pré-configurada](../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md):

* Temperatura
* pressão
* Humidade

De simplicidade, o código gera os valores de telemetria de exemplo para o **Chiller**. Podia expandir o exemplo ao ligar sensores reais para o seu dispositivo e envia a telemetria real.

O dispositivo de exemplo também:

* Envia os metadados para a solução para descrever as respetivas capacidades.
* Responde a ações acionadas a partir de **dispositivos** página na solução.
* Responder a alterações de configuração enviar a partir de **dispositivos** página na solução.

Para concluir este tutorial, precisa de uma conta ativa do Azure. Se não tiver uma conta, pode criar uma de avaliação gratuita em apenas alguns minutos. Para obter mais detalhes, consulte [Avaliação gratuita do Azure](http://azure.microsoft.com/pricing/free-trial/).

## <a name="before-you-start"></a>Antes de começar

Antes de escrever qualquer código para o seu dispositivo, implementar a solução pré-configurada de monitorização remota e adicionar um novo dispositivo físico para a solução.

### <a name="deploy-your-remote-monitoring-preconfigured-solution"></a>Implementar a solução pré-configurada de monitorização remota

O **Chiller** dispositivo que criou neste tutorial envia dados para uma instância do [monitorização remota](../articles/iot-suite/iot-suite-remote-monitoring-explore.md) solução pré-configurada. Se já que ainda não aprovisionou a solução pré-configurada de monitorização remota na sua conta do Azure, consulte o artigo [implementar a solução pré-configurada de monitorização remota](../articles/iot-suite/iot-suite-remote-monitoring-deploy.md)

Quando o processo de implementação para a conclusão da solução de monitorização remota, clique em **iniciar** para abrir o dashboard de solução no seu browser.

![O dashboard de solução](media/iot-suite-selector-connecting/dashboard.png)

### <a name="add-your-device-to-the-remote-monitoring-solution"></a>Adicionar o seu dispositivo à solução de monitorização remota

> [!NOTE]
> Se já adicionou um dispositivo na sua solução, pode ignorar este passo. No entanto, o passo seguinte requer a cadeia de ligação do dispositivo. Pode obter a cadeia de ligação de um dispositivo do [portal do Azure](https://portal.azure.com) ou utilizando o [az iot](https://docs.microsoft.com/cli/azure/iot?view=azure-cli-latest) ferramenta CLI.

Para que um dispositivo ligue à solução pré-configurada, este tem de se identificar no Hub IoT utilizando credenciais válidas. Terá a oportunidade para guardar a cadeia de ligação do dispositivo que contenha estas credenciais ao adicionar o dispositivo a solução. Incluem a cadeia de ligação do dispositivo na sua aplicação de cliente mais tarde no tutorial.

Para adicionar um dispositivo à sua solução de monitorização remota, conclua os seguintes passos no **dispositivos** página na solução:

1. Escolha **+ novo dispositivo**e, em seguida, escolha **físico** como o **tipo de dispositivo**:

    ![Adicionar um dispositivo físico](media/iot-suite-selector-connecting/devicesprovision.png)

1. Introduza **físico chiller** como o ID de dispositivo. Escolha o **chave simétrica** e **automática gerar chaves** opções:

    ![Escolha as opções de dispositivo](media/iot-suite-selector-connecting/devicesoptions.png)

1. Escolha **aplicar**. Em seguida, anote o **ID de dispositivo**, **chave primária**, e **chave primária de cadeia de ligação** valores:

    ![Obter credenciais](media/iot-suite-selector-connecting/credentials.png)

Agora já adicionou um dispositivo físico para a solução pré-configurada de monitorização remota e indicado respetiva cadeia de ligação do dispositivo. Nas secções seguintes, implementar a aplicação cliente que utiliza a cadeia de ligação do dispositivo para ligar à sua solução.

A aplicação cliente implementa incorporada **Chiller** modelo do dispositivo. Um modelo de dispositivo da solução pré-configurada Especifica o seguinte sobre um dispositivo:

* As propriedades de que relatórios de dispositivo para a solução. Por exemplo, um **Chiller** dispositivo comunica informações sobre o respetivo firmware e localização.
* Os tipos de telemetria que o dispositivo envia para a solução. Por exemplo, um **Chiller** dispositivo envia temperatura, humidade e valores de pressão.
* Os métodos que pode agendar da solução para execução no dispositivo. Por exemplo, um **Chiller** dispositivo tem de implementar **reiniciar**, **FirmwareUpdate**, **EmergencyValveRelease**, e  **IncreasePressure** métodos.