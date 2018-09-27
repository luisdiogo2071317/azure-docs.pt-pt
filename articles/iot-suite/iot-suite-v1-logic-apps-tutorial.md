---
title: O Azure IoT Suite e o Logic Apps | Documentos da Microsoft
description: Um tutorial sobre como conectar o Logic Apps para o Azure IoT Suite para o processo comercial.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 4629a7af-56ca-4b21-a769-5fa18bc3ab07
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: corywink
ms.openlocfilehash: 4a1db86f4b715533dfea545365eaf66de0574c5e
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47106925"
---
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Tutorial: Ligar a aplicação lógica à sua solução do Azure IoT Suite monitorização remota pré-configurada
O [Microsoft Azure IoT Suite] [ lnk-internetofthings] solução pré-configurada de monitorização remota é uma ótima maneira de começar rapidamente com um conjunto de funcionalidades de ponto-a-ponto que exemplifique uma solução de IoT. Este tutorial explica como adicionar a aplicação lógica para o Microsoft Azure IoT Suite solução pré-configurada de monitorização remota. Estes passos demonstram como pode tirar ainda mais a sua solução de IoT ao ligá-los para um processo comercial.

*Se procura-se para obter instruções sobre como aprovisionar a solução pré-configurada de uma monitorização remota, consulte [Tutorial: introdução ao soluções pré-configuradas do IoT][lnk-getstarted].*

Antes de começar este tutorial, deve:

* Aprovisione a solução pré-configurada de monitorização remota na sua subscrição do Azure.
* Crie uma conta do SendGrid para que possa enviar um e-mail que aciona o processo de negócios. Pode inscrever-se para uma conta de avaliação gratuita no [SendGrid](https://sendgrid.com/) ao clicar em **experimentar gratuitamente**. Após o Registro para a sua conta de avaliação gratuita, tem de criar uma [chave de API](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) no SendGrid que concede permissões para enviar correio. Precisará desta chave de API mais tarde no tutorial.

Para concluir este tutorial, terá de Visual Studio 2015 ou Visual Studio 2017 para modificar as ações no back-end de solução pré-configurada.

Partindo do princípio de que já provisionou suas monitorização remota a solução pré-configurada, navegue para o grupo de recursos para essa solução a [portal do Azure][lnk-azureportal]. O grupo de recursos tem o mesmo nome que o nome da solução que escolheu quando aprovisionou a sua solução de monitorização remota. No grupo de recursos, pode ver todos os recursos do Azure aprovisionados para a sua solução. Captura de ecrã seguinte mostra um exemplo **grupo de recursos** solução pré-configurada do painel para uma monitorização remota:

![](media/iot-suite-v1-logic-apps-tutorial/resourcegroup.png)

Para começar, configure a aplicação lógica para utilizar a solução pré-configurada.

## <a name="set-up-the-logic-app"></a>Configurar a aplicação lógica
1. Clique em **adicionar** na parte superior do painel do grupo de recursos no portal do Azure.
2. Procure **aplicação lógica**, selecione-o e, em seguida, clique em **criar**.
3. Preencha os **Name** e utilizar o mesmo **subscrição** e **grupo de recursos** que utilizou quando aprovisionou a sua solução de monitorização remota. Clique em **Criar**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/createlogicapp.png)
4. Quando a implementação estiver concluída, pode ver que a aplicação lógica está listada como um recurso no seu grupo de recursos.
5. Clique na aplicação de lógica para navegar para o painel de aplicação lógica, selecione o **aplicação lógica em branco** modelo para abrir o **estruturador de aplicações lógicas**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappsdesigner.png)
6. Selecione **pedir**. Esta ação Especifica que uma solicitação de HTTP de entrada com um JSON específico formatado payload atua como um acionador.
7. Cole o seguinte código no esquema de JSON do corpo de pedido:
   
    ```json
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
   
   > [!NOTE]
   > Depois de guardar a aplicação lógica, mas primeiro tem de adicionar uma ação, pode copiar o URL para o HTTP post.
   > 
   > 
8. Clique em **+ novo passo** no seu acionador manual. Em seguida, clique em **adicionar uma ação**.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappcode.png)
9. Procure **SendGrid - enviar e-mail** e clique no mesmo.
   
    ![](media/iot-suite-v1-logic-apps-tutorial/logicappaction.png)
10. Introduza um nome para a ligação, tal como **SendGridConnection**, introduza o **chave de API do SendGrid** que criou quando configurar a sua conta do SendGrid e clique em **criar**.
    
    ![](media/iot-suite-v1-logic-apps-tutorial/sendgridconnection.png)
11. Adicionar endereços de e-mail que possui duas a **partir** e **para** campos. Adicione **alerta de monitorização remota [DeviceId]** para o **assunto** campo. Na **corpo do E-Mail** campo, adicione **dispositivo [DeviceId] comunicou [measurementName] com o valor [measuredValue].** Pode adicionar **[DeviceId]**, **[measurementName]**, e **[measuredValue]** ao clicar no **pode inserir os dados dos passos anteriores** secção.
    
    ![](media/iot-suite-v1-logic-apps-tutorial/sendgridaction.png)
12. Clique em **guardar** no menu superior.
13. Clique a **pedir** acionador e copie a **Http Post para este URL** valor. Este URL é necessário mais tarde neste tutorial.

> [!NOTE]
> As aplicações lógicas permitem-lhe executar [muitos tipos diferentes de ação] [ lnk-logic-apps-actions] incluindo ações no Office 365. 
> 
> 

## <a name="set-up-the-eventprocessor-web-job"></a>Configurar a tarefa de Web de Processadordeeventos
Nesta secção, vai ligar a solução pré-configurada para a aplicação lógica que criou. Para concluir essa tarefa, adicione o URL para acionar a aplicação lógica para a ação que é acionado quando um valor de sensor do dispositivo excede um limiar.

1. Utilize o seu cliente de git para clonar a versão mais recente do [azure-iot-remote-monitoring repositório do github][lnk-rmgithub]. Por exemplo:
   
    ```cmd
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```
2. No Visual Studio, abra a **RemoteMonitoring.sln** da cópia local do repositório.
3. Abra o **ActionRepository.cs** de ficheiros a **infraestrutura\\repositório** pasta.
4. Atualização do **actionIds** dicionário com o **Http Post para este URL** da sua aplicação lógica que anotou da seguinte forma:
   
    ```csharp
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this URL>" },
        { "Raise Alarm", "<Http Post to this URL>" }
    };
    ```
5. Guardar as alterações na solução e sair do Visual Studio.

## <a name="deploy-from-the-command-line"></a>Implementar a partir da linha de comandos
Nesta secção, vai implementar a versão atualizada da solução de monitorização remota para substituir a versão atualmente em execução no Azure.

1. A seguir a [configuração do programador] [ lnk-devsetup] instruções para configurar o ambiente para a implementação.
2. Para implementar localmente, siga os [implementação local] [ lnk-localdeploy] instruções.
3. Para implementar para a cloud e atualizar a implementação de nuvem existente, siga os [implementação na nuvem] [ lnk-clouddeploy] instruções. Utilize o nome da sua implementação original como o nome da implementação. Por exemplo, se a implementação original foi chamada **demologicapp**, utilize o seguinte comando:
   
   ```cmd
   build.cmd cloud release demologicapp
   ```
   
   Quando o script de compilação é executada, certifique-se de que utilize a mesma conta do Azure, subscrição, região e instância do Active Directory que utilizou quando aprovisionou a solução.

## <a name="see-your-logic-app-in-action"></a>Ver a sua aplicação lógica em ação
A solução pré-configurada de monitorização remota tem duas regras configuradas por predefinição, quando Aprovisiona uma solução. Ambas as regras são sobre o **SampleDevice001** dispositivo:

* Temperatura > 38.00
* Humidade > 48.00

Os acionadores de regra de temperatura a **elevar alarme** ação e a Umidade regra acionadores a **SendMessage** ação. Pressupondo que usou o mesmo URL para ambas as ações a **ActionRepository** classe, os acionadores da aplicação lógica para a regra. Regras de utilizam o SendGrid para enviar um e-mail para o **para** endereço com detalhes do alerta.

> [!NOTE]
> A aplicação lógica continua a acionar sempre que o limiar é cumprido. Para evitar mensagens de correio eletrónico desnecessárias, pode desativar as regras no seu portal de solução ou desativar a aplicação lógica no [portal do Azure][lnk-azureportal].
> 
> 

Para além de receber e-mails, também pode ver quando é executada a aplicação lógica no portal do:

![](media/iot-suite-v1-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Passos Seguintes
Agora que já utilizou uma aplicação lógica para ligar a solução pré-configurada de um processo comercial, pode saber mais sobre as opções para personalizar as soluções pré-configuradas:

* [Utilizar a telemetria dinâmica com a solução pré-configurada de monitorização remota][lnk-dynamic]
* [Metadados de informações do dispositivo na solução pré-configurada de monitorização remota][lnk-devinfo]

[lnk-dynamic]: iot-suite-v1-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-v1-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-v1-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
