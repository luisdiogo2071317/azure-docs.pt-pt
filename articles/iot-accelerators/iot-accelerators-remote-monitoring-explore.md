---
title: Introdução à solução de monitorização remota - Azure | Microsoft Docs
description: Este tutorial utiliza cenários simulados para apresentar o acelerador de solução de monitorização remota. Estes cenários são criados quando implementar o acelerador de solução de monitorização remota pela primeira vez.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 05/01/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: c8a055dd0dc8e2d9a9a64b492fb134caaacda489
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="explore-the-capabilities-of-the-remote-monitoring-solution-accelerator"></a>Explore as capacidades do acelerador de solução de monitorização remota

Este tutorial mostra as principais capacidades de solução de monitorização remota. Para apresentar estas capacidades, o tutorial showcases cenários comuns do cliente, utilizando uma aplicação de IoT simulada para uma empresa denominada Contoso.

O tutorial ajuda-o a compreende os cenários típicos de IoT a solução de monitorização remota fornece out of box.

Neste tutorial, ficará a saber como:

>[!div class="checklist"]
> * Visualizar e filtrar dispositivos no dashboard
> * Responder a um alerta
> * Atualizar o firmware nos seus dispositivos
> * Organizar os elementos
> * Parar e iniciar dispositivos simulados

O vídeo seguinte mostra as obter instruções sobre a solução de monitorização remota:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Part-28-An-introduction-to-Azure-IoT-through-the-new-Remote-Monitoring-Preconfigured-Solution/Player]

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, precisa de uma instância implementada da solução de monitorização remota na sua subscrição do Azure.

Se ainda não implementado a solução de monitorização remota ainda, deve efetuar o [implementar o acelerador de solução de monitorização remota](iot-accelerators-remote-monitoring-deploy.md) tutorial.

## <a name="the-contoso-sample-iot-deployment"></a>A implementação de IoT de exemplo do Contoso

Pode utilizar a implementação de IoT de exemplo de Contoso para compreender os cenários básicos remoto solução de monitorização fornece out of box. Estes cenários são baseados em implementações de IoT reais. Provavelmente, irá escolher Personalizar a solução de monitorização remota para satisfazer os seus requisitos específicos, mas o exemplo de Contoso ajuda-o a aprender as noções básicas.

> [!NOTE]
> Se utilizou a CLI para implementar o acelerador de solução, o ficheiro `deployment-{your deployment name}-output.json` contém informações sobre a implementação, tais como o URL para aceder a amostra implementada.

O exemplo de Contoso aprovisiona um conjunto de dispositivos simulados e regras para agir nos mesmos. Assim que compreender os cenários básicos, pode continuar a explorar mais funcionalidades solução [executar avançada de monitorização de dispositivos com a solução de monitorização remota](iot-accelerators-remote-monitoring-monitor.md).

Contoso é uma empresa que gere uma variedade de recursos em ambientes diferentes. Contoso planeia utilizar a capacidade das aplicações de IoT baseada na nuvem para monitorizar e gerir vários recursos a partir de uma aplicação centralizada remotamente. As secções seguintes fornecem um resumo da configuração inicial do exemplo Contoso:

> [!NOTE]
> Demonstração de Contoso é apenas uma forma para aprovisionar dispositivos simulados e criar regras. Outras opções de aprovisionamento incluem a criação dos seus próprios dispositivos personalizados. Para obter mais informações sobre como criar os seus próprios dispositivos e as regras, consulte [gerir e configurar os seus dispositivos](iot-accelerators-remote-monitoring-manage.md) e [detetar problemas através de regras baseadas em limiares](iot-accelerators-remote-monitoring-automate.md).

### <a name="contoso-devices"></a>Dispositivos de contoso

A Contoso utiliza diferentes tipos de dispositivos inteligentes. Estes dispositivos satisfazer diferentes funções da empresa, enviar vários fluxos de telemetria. Além disso, cada tipo de dispositivo tem propriedades diferentes de dispositivos e suportados métodos.

A tabela seguinte mostra um resumo dos tipos de dispositivo aprovisionada:

| Tipo de dispositivo        | Telemetria                                  | Propriedades                                  | Etiquetas                    | Métodos                                                                                      |
| ------------------ | ------------------------------------------ | ------------------------------------------- | ----------------------- | -------------------------------------------------------------------------------------------- |
| Chiller            | Pressão de temperatura, humidade,            | Tipo, versão de Firmware, de modelo               | Localização, piso, Campus | Reiniciar o computador, pressão de aumento de atualização, versão de emergência Valve, de Firmware                          |
| Dispositivo de fazer o protótipo | Temperatura, pressão, geolocalização        | Tipo, versão de Firmware, de modelo               | Localização, modo          | Reiniciar o computador, atualização de Firmware, o dispositivo de movimentação, paragem dispositivo, versão de temperatura, aumento de temperatura |
| Motor             | Tank fuel nível, Coolant sensor, Vibration | Tipo, versão de Firmware, de modelo               | Localização, piso, Campus | Atualização de firmware, tank vazio, tank de preenchimento                                              |
| Camião              | Temperatura de carga de geolocalização, velocidade,     | Tipo, versão de Firmware, de modelo               | Localização de carga          | Inferior temperatura de carga, a temperatura do aumento de carga, a atualização de Firmware                         |
| Abordagem           | Piso, Vibration, temperatura              | Tipo, versão de Firmware, o modelo, geolocalização | Localização, Campus        | Abordagem de parar, a abordagem de início, a atualização de Firmware                                               |

> [!NOTE]
> Os Contoso demonstração Aprovisiona dois dispositivos de exemplo por tipo. Para cada tipo, um funciona corretamente dentro dos limites definidos como normal Contoso e um tem algum tipo de funcionar incorretamente ocorrer sistematicamente. Na secção seguinte, pode obter informações sobre as regras que Contoso define para os dispositivos. Estas regras definem os limites de comportamento correto.

### <a name="contoso-rules"></a>Regras de contoso

Operadores contoso sabe os limiares que determinam se um dispositivo está a funcionar corretamente. Por exemplo, um chiller não está a funcionar corretamente se o pressão esta comunica for superior a 250 PSI. A tabela seguinte mostra as regras baseadas em limiares que Contoso define para cada tipo de dispositivo:

| Nome da Regra | Descrição | Limiar | Gravidade | Dispositivos afetados |
| --------- | ----------- | --------- | -------- | ---------------- |
| Pressão chiller demasiado elevada | Alerta se chillers alcançar superior do que os níveis de pressão normal   |P > 250 psi       | Crítica | Chillers            |
| Temp de dispositivo de fazer o protótipo demasiado elevada  | Alerta se os dispositivos de fazer o protótipo alcancem superior do que os níveis de temperatura normal  |T &GT; 80&deg; F |Crítica | Dispositivos de fazer o protótipo |
| Tank motor vazio  | Alertas se o motor fuel tank ficar vazio                     | F < 5 gallons | Informações     | Motores             |
| Superior a temperatura de carga normal | Alerta se a temperatura de carga do camião é superior à normal                 | T &LT; 45&deg; F |Aviso  | Trucks              |
| Vibration abordagem parado      | Alertas se abordagem parar completamente (com base no nível de vibration)                     | V < 0.1 mm |Aviso  | Elevators           |

### <a name="operate-the-contoso-sample-deployment"></a>Operar a implementação de exemplo do Contoso

Constatou agora a configuração inicial do exemplo Contoso. As secções seguintes descrevem os três cenários de exemplo da Contoso que ilustram como um operador pode utilizar o acelerador de solução.

## <a name="respond-to-a-pressure-alert"></a>Responder a um alerta de pressão

Este cenário mostra como identificar e responder a um alerta que é acionado por um dispositivo chiller. O chiller está localizado em Redmond, na compilação 43, piso 2.

Como um operador, vê no dashboard que há um alerta relacionadas com a pressão de um chiller. Pode efetuar um deslocamento panorâmico e zoom no mapa para ver mais detalhes.

1. No **Dashboard** na página de **alertas** grelha, pode ver o **pressão Chiller demasiado elevada** alerta. O chiller também é realçado no mapa:

    ![Dashboard mostra alerta pressão e dispositivos no mapa](./media/iot-accelerators-remote-monitoring-explore/dashboardalarm.png)

1. Para navegar para o **manutenção** página, escolha **manutenção** no menu de navegação. No **manutenção** página, pode ver os detalhes da regra que acionou o alerta de pressão chiller.

1. A lista de alertas mostra o número de vezes que o alerta foi acionado, em que as confirmações e alertas abertas e fechadas:

    ![Página de manutenção mostra a lista de alertas que tenham acionada](./media/iot-accelerators-remote-monitoring-explore/maintenancealarmlist.png)

1. O último alerta na lista é a mais recente. Clique em de **Chiller pressão demasiado elevada** alerta para ver a telemetria e de dispositivos associados. A telemetria mostra picos de pressão para o chiller:

    ![Página de manutenção mostra a telemetria para o alerta selecionado](./media/iot-accelerators-remote-monitoring-explore/maintenancetelemetry.png)

Agora ter identificado o problema que acionou o alerta e o dispositivo associado. Como um operador, os passos seguintes são reconhecer o alerta e mitigar o problema.

1. Para indicar que está a trabalhar agora num alerta, altere o **estado do alerta** para **confirmado**:

    ![Selecione e confirmar o alerta](./media/iot-accelerators-remote-monitoring-explore/maintenanceacknowledge.png)

1. Para agir sobre o chiller, selecione-o e, em seguida, escolha **tarefas**. Selecione **executar o método**, em seguida, **EmergencyValveRelease**, adicionar um nome de tarefa **ChillerPressureRelease**e escolha **aplicar**. Estas definições de criar uma tarefa que é executada imediatamente:

    ![Selecione o dispositivo e agendar uma ação](./media/iot-accelerators-remote-monitoring-explore/maintenanceschedule.png)

1. Para ver o estado da tarefa, voltar para o **manutenção** e ver a lista de tarefas na página a **tarefas** vista. Pode ver que a tarefa foi executada para libertar o pressão valve no chiller:

    ![O estado das tarefas na vista tarefas](./media/iot-accelerators-remote-monitoring-explore/maintenancerunningjob.png)

Por fim, confirme que os valores de telemetria do chiller são novamente para normal.

1. Para ver a grelha de alertas, navegue para o **Dashboard** página.

1. Para ver a telemetria do dispositivo, selecione o dispositivo para o alerta original no mapa e confirme que é a normal.

1. Para fechar o incidente, navegue para o **manutenção** página, selecione o alerta e definir o estado **fechado**:

    ![Selecione e fechar o alerta](./media/iot-accelerators-remote-monitoring-explore/maintenanceclose.png)

## <a name="update-device-firmware"></a>Atualizar o firmware do dispositivo

Contoso é testar um novo tipo de dispositivo no campo. Como parte do ciclo de teste, tem de garantir que firmware do dispositivo de atualizações de trabalho corretamente. Os passos seguintes mostram como utilizar a solução de monitorização remota para atualizar o firmware em vários dispositivos.

Para efetuar as tarefas de gestão de dispositivo necessários, utilize o **dispositivos** página. Comece por filtrar para todos os dispositivos de fazer o protótipo:

1. Navegue para o **dispositivos** página. Escolha o **dispositivos de fazer o protótipo** filtrar no **filtros** pendente:

    ![Utilize um filtro na página de dispositivos](./media/iot-accelerators-remote-monitoring-explore/devicesprototypingfilter.png)

    > [!TIP]
    > Clique em **gerir** para gerir os filtros disponíveis.

1. Selecione um dos dispositivos de fazer o protótipo:

    ![Selecione um dispositivo na página de dispositivos](./media/iot-accelerators-remote-monitoring-explore/devicesselect.png)

1. Clique em de **tarefas** botão, escolha **executar o método**e, em seguida, escolha **atualização de Firmware**. Introduza os valores para **nome da tarefa**, **versão de Firmware**, e **URI de Firmware**. Escolha **aplicar** para agendar a tarefa para executar agora:

    ![Agendar a atualização de firmware no dispositivo](./media/iot-accelerators-remote-monitoring-explore/devicesschedulefirmware.png)

    > [!NOTE]
    > Com os dispositivos simulados pode utilizar qualquer URL que quiser como o **URI de Firmware** valor e qualquer valor como para o **versão de Firmware**. Os dispositivos simulados não aceder ao URL.

1. Tenha em atenção o número de dispositivos afeta a tarefa e escolha **aplicar**:

    ![Submeter a tarefa de atualização de firmware](./media/iot-accelerators-remote-monitoring-explore/devicessubmitupdate.png)

Pode utilizar o **manutenção** página para controlar a tarefa que é executada.

1. Para ver a lista de tarefas, navegue para o **manutenção** página e clique em **tarefas**.

1. Localize os eventos relacionados com a tarefa que criou. Certifique-se de que o processo de atualização de firmware foi iniciado corretamente.

<!-- 05/01 broken 
You can create a filter to verify the firmware version updated correctly.

1. To create a filter, navigate to the **Devices** page and select **Manage device groups**:

    ![Manage device groups](./media/iot-accelerators-remote-monitoring-explore/devicesmanagefilters.png)

1. Create a filter that includes only devices with the new firmware version:

    ![Create device filter](./media/iot-accelerators-remote-monitoring-explore/devicescreatefilter.png)

1. Return to the **Devices** page and verify that the device has the new firmware version. -->

## <a name="organize-your-assets"></a>Organizar os elementos

Contoso tem duas equipas diferentes para as atividades do serviço de campo:

* A equipa de criação de Smart gere chillers, elevators e motores.
* A equipa de Vehicle inteligente gere trucks e dispositivos de fazer o protótipo.

Para facilitar como um operador para organizar e gerir os seus dispositivos, quer marcá-los com o nome do agrupamento adequado.

Pode criar os nomes de etiqueta para utilizar com dispositivos.

1. Para apresentar todos os dispositivos, navegue para o **dispositivos** página e escolha o **todos os dispositivos** filtro:

    ![Mostrar todos os dispositivos](./media/iot-accelerators-remote-monitoring-explore/devicesalldevices.png)

1. Selecione o **Trucks** e **fazer o protótipo** dispositivos. Em seguida, escolha **tarefas**:

    ![Selecione os dispositivos protótipo e camião](./media/iot-accelerators-remote-monitoring-explore/devicesmultiselect.png)

1. Escolha **Tag** e, em seguida, crie uma nova etiqueta de texto chamada **FieldService** com um valor **ConnectedVehicle**. Escolha um nome para a tarefa. Em seguida, clique em **aplicar**:

    ![Adicionar etiquetas a dispositivos protótipo e camião](./media/iot-accelerators-remote-monitoring-explore/devicesaddtag.png)

1. Selecione o **Chiller**, **abordagem**, e **motor** dispositivos. Em seguida, escolha **tarefas**:

    ![Selecione dispositivos chiller, motor e a abordagem](./media/iot-accelerators-remote-monitoring-explore/devicesmultiselect2.png)

1. Escolha **Tag** e, em seguida, crie uma nova etiqueta de texto chamada **FieldService** com um valor **SmartBuilding**. Escolha um nome para a tarefa. Em seguida, clique em **aplicar**:

    ![Adicionar etiquetas ao chiller, motor e a abordagem de dispositivos](./media/iot-accelerators-remote-monitoring-explore/devicesaddtag2.png)

Pode utilizar os valores de etiqueta para criar filtros.

1. No **dispositivos** página, escolha **gerir grupos de dispositivos**:

    ![Gerir grupos de dispositivos](./media/iot-accelerators-remote-monitoring-explore/devicesmanagefilters.png)

1. Criar um novo filtro que utiliza o nome de etiqueta **FieldService** e valor **SmartBuilding**. Guardar o filtro como **Smart edifício**.

1. Criar um novo filtro que utiliza o nome de etiqueta **FieldService** e valor **ConnectedVehicle**. Guardar o filtro como **ligado Vehicle**.

Agora, o operador de Contoso pode consultar dispositivos com base na equipa de sistema operativo sem a necessidade de alterar nada nos dispositivos.

## <a name="stop-simulated-devices"></a>Parar dispositivos simulados

Pode utilizar o menu de definições para os dispositivos simulados. Isto ajuda a reduzir os custos de teste e explorar a solução. Para iniciar ou parar os dispositivos simulados:

1. Escolha o **definições** ícone.

1. Em seguida, ative **Flowing** ou desativar:

    ![Menu de definições](./media/iot-accelerators-remote-monitoring-explore/settings.png)

## <a name="customize-the-ui"></a>Personalizar a IU

No menu de definições, pode aplicar customisations básicas para o acelerador de solução de monitorização remota. Pode:

- Alternar entre os temas claros e escuros.
- Altere o nome da solução.
- Carregar um logótipo personalizado.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a:

>[!div class="checklist"]
> * Visualizar e filtrar dispositivos no dashboard
> * Responder a um alerta
> * Atualizar o firmware nos seus dispositivos
> * Organizar os elementos
> * Parar e iniciar dispositivos simulados

Agora que tem explorou a solução de monitorização remota, se os passos sugeridos saber mais sobre as funcionalidades avançadas de solução de monitorização remota:

* [Monitorizar os seus dispositivos](./iot-accelerators-remote-monitoring-monitor.md).
* [Gerir os seus dispositivos](./iot-accelerators-remote-monitoring-manage.md).
* [Automatizar a sua solução com regras](./iot-accelerators-remote-monitoring-automate.md).
* [Manter a sua solução](iot-accelerators-remote-monitoring-maintain.md).
