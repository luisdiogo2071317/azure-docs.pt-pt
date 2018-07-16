## <a name="customize-and-extend-the-device-management-actions"></a>Personalizar e estender o dispositivo ações de gestão

Suas soluções de IoT podem expandir o conjunto definido de padrões de gestão de dispositivos ou ativar padrões personalizados com o dispositivo duplo e primitivos de método de cloud-para-dispositivo. Outros exemplos de ações de gestão do dispositivo incluem a reposição de fábrica, atualização de firmware, atualização de software, gerenciamento de energia, gestão de rede e a conectividade e encriptação de dados.

## <a name="device-maintenance-windows"></a>Janelas de manutenção do dispositivo

Normalmente, pode configurar dispositivos para realizar ações ao mesmo tempo que minimiza as interrupções e tempo de inatividade. Janelas de manutenção do dispositivo são um padrão usado para definir a hora quando um dispositivo deve atualizar a respetiva configuração. Suas soluções de back-end podem utilizar as propriedades pretendidas do dispositivo duplo para definir e ativar uma política no seu dispositivo, que permite uma janela de manutenção. Quando um dispositivo recebe a política de janela de manutenção, pode utilizar a propriedade comunicada do dispositivo duplo para comunicar o estado da política. A aplicação de back-end, em seguida, pode utilizar consultas twin do dispositivo para confirmar a conformidade de dispositivos e de cada política.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, utilizou um método direto para acionar a reinicialização remota num dispositivo. Utilizou as propriedades reportadas para reportar a última vez que o reinício do dispositivo e consultado o dispositivo duplo para detetar a última vez que o reinício do dispositivo a partir da cloud.

Para continuar a introdução ao IoT Hub e padrões de gestão de dispositivos como remota sobre a atualização de firmware do ar, consulte:

[Tutorial: Como fazer uma atualização de firmware][lnk-fwupdate]

Para saber como expandir o seu IoT chama o método de solução e a agenda em vários dispositivos, veja a [agendar e transmitir tarefas] [ lnk-tutorial-jobs] tutorial.

Para continuar a introdução ao IoT Hub, veja [introdução ao IoT Edge][lnk-iot-edge].

[lnk-fwupdate]: ../articles/iot-hub/tutorial-firmware-update.md
[lnk-tutorial-jobs]: ../articles/iot-hub/iot-hub-node-node-schedule-jobs.md
[lnk-iot-edge]: ../articles/iot-edge/tutorial-simulate-device-linux.md