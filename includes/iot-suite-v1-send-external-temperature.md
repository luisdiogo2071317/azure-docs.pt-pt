## <a name="configure-the-nodejs-simulated-device"></a>Configurar o dispositivo simulado do node. js
1. No dashboard de monitorização remota, clique em **+ adicionar um dispositivo** e, em seguida, adicione um *personalizadas dos dispositivos*. Tome nota do IoT Hub, nome de anfitrião, a id do dispositivo e a chave do dispositivo. Terá de-los mais tarde neste tutorial quando preparar a aplicação de cliente de dispositivo remote_monitoring.js.
2. Certifique-se de que a versão node. js 0.12.x ou posterior está instalado no computador de desenvolvimento. Executar `node --version` no prompt de comando ou numa shell para verificar a versão. Para obter informações sobre como utilizar um Gestor de pacotes para instalar o node. js no Linux, consulte [instalar o node. js através do Gestor de pacotes][node-linux].
3. Quando tiver instalado o node. js, clonar a versão mais recente do [azure-iot-sdk-nó] [ lnk-github-repo] repositório à sua máquina de desenvolvimento. Utilize sempre a **mestre** ramo para a versão mais recente das bibliotecas e amostras.
4. Da sua cópia local do [azure-iot-sdk-nó] [ lnk-github-repo] repositório, ficheiros de cópia os dois seguintes a partir da pasta de exemplos/nó/dispositivo para uma pasta vazia no computador de desenvolvimento:
   
   * Packages.JSON
   * remote_monitoring.js
5. Abra o ficheiro de remote_monitoring.js e procure a seguinte definição de variável:
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. Substitua **[cadeia de ligação de dispositivos do IoT Hub]** com a cadeia de ligação do dispositivo. Utilize os valores para o seu nome de anfitrião do IoT Hub, o id de dispositivo e a chave de dispositivo anotou no passo 1. Uma cadeia de ligação do dispositivo tem o seguinte formato:
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    Se for o nome de anfitrião do IoT Hub **contoso** e o id de dispositivo estiver **mydevice**, a cadeia de ligação se parece com o seguinte fragmento:
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Guarde o ficheiro. Execute os seguintes comandos num shell ou a linha de comandos na pasta que contém estes ficheiros para instalar os pacotes necessários e, em seguida, executar o aplicativo de exemplo:
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Observe a telemetria dinâmica em ação
O dashboard mostra a telemetria de temperatura e humidade dos dispositivos simulados existentes:

![O dashboard predefinido][image1]

Se selecionar o dispositivo simulado do node. js que executou na secção anterior, verá temperatura, humidade e telemetria de temperatura externa:

![Adicionar a temperatura externa ao dashboard][image2]

Solução de monitorização remota automaticamente detecta o tipo de telemetria de temperatura externa adicionais e adiciona-o para o gráfico no dashboard.

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-v1-send-external-temperature/image1.png
[image2]: media/iot-suite-v1-send-external-temperature/image2.png