---
title: Dispositivo configuração recomendados para o Azure IoT Hub | Microsoft Docs
description: Saiba mais sobre as melhores práticas para configurar dispositivos de IoT à escala
author: chrisgre
manager: briz
ms.author: chrisgre
ms.date: 06/24/2018
ms.topic: conceptual
ms.service: iot-hub
services: iot-hub
ms.openlocfilehash: 7314c5ec05bec61e5bbbc406b6a39a7c5a8f011f
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036268"
---
# <a name="best-practices-for-device-configuration-within-an-iot-solution"></a>Melhores práticas para a configuração de dispositivo numa solução de IoT

Gestão de dispositivos automática no IoT Hub do Azure automatiza muitas tarefas repetitivas e complexas de gerir dispositivos grande fleets através da totalidade da respetiva ciclos de vida. Este artigo define muitas das melhores práticas para várias funções envolvidas no desenvolvimento e funcionamento de uma solução de IoT.

* **Fabricante de hardware de IoT/integrador:** hardware de fabricantes de IoT, integradores reuni hardware a partir de vários fabricantes ou fornecedores de hardware a fornecer para uma implementação de IoT fabricado ou integrado por outros fornecedores. Envolvidas no desenvolvimento e integração de firmware, sistemas operativos incorporados e software incorporado.
* **Para programadores de solução IoT:** o desenvolvimento de uma solução de IoT é geralmente feito por um programador de solução. Este programador pode fazer parte de uma equipa interna ou um integrador de sistema especificar melhor nesta atividade. O Programador de solução IoT pode desenvolver vários componentes da solução IoT a partir do zero, integrar vários componentes de open source ou padrão ou personalizar uma [acelerador de solução IoT][lnk-solution].
* **Operador de solução IoT:** após IoT solução for implementada, necessita de operações de longa duração, monitorização, atualizações e manutenção. Estas tarefas podem ser realizadas por um agrupamento interna que é composta pelos especialistas de tecnologia de informação, operações de hardware e equipas de manutenção e especialistas de domínio que monitorizar o comportamento correto da infraestrutura do IoT geral.

## <a name="understand-automatic-device-management-for-configuring-iot-devices-at-scale"></a>Compreender a gestão de dispositivos automático para configurar dispositivos de IoT à escala

Gestão de dispositivos automática inclui muitos benefícios da [dispositivos duplos] [ lnk-device-twins] e [duplos módulo] [ lnk-module-twins] sincronizar pretendida e Estados comunicados entre a nuvem e de dispositivos.  [Configurações de dispositivos automático] [ lnk-auto-device-config] automaticamente atualizar grandes conjuntos de duplos e summerize progresso e conformidade. Os passos de alto nível seguintes descrevem como automática gestão de dispositivos é desenvolvidas e utilizado:

* O **fabricante de hardware de IoT/integrador** implementa as funcionalidades de gestão de dispositivos dentro de uma utilização de aplicações incorporadas [dispositivos duplos][lnk-device-twins]. Estas funcionalidades podem incluir atualizações de firmware, a instalação de software e atualização e gestão de definições.
* O **para programadores de solução IoT** implementa a camada de gestão de operações de gestão de dispositivos utilizando [dispositivos duplos] [ lnk-device-twins] e [automático de dispositivos configurações][lnk-auto-device-config]. A solução deve incluir a definir uma interface de operador para efetuar tarefas de gestão de dispositivos.
* O **operador de solução IoT** utiliza a solução de IoT para efetuar tarefas de gestão de dispositivos, particularmente para agrupar dispositivos em conjunto, alterações de configuração, como atualizações de firmware de iniciar, monitorizar o progresso e resolver problemas que surgir.

## <a name="iot-hardware-manufacturerintegrator"></a>Fabricante de hardware de IoT/integrador

Seguem-se as melhores práticas para os fabricantes de hardware e integradores de lidar com o desenvolvimento de software incorporados:

* **Implementar [dispositivos duplos][lnk-device-twins]:** dispositivos duplos ativar a sincronização de configuração pretendida a partir da nuvem e de relatórios atuais propriedades de configuração e de dispositivo.  A melhor forma de implementar dispositivos duplos incorporados aplicações é através de [SDKs IoT do Azure][lnk-azure-sdk].  Dispositivos duplos são mais adequados para a configuração porque estes: um. suporta a comunicação bidirecional b. permitir para ambos os c Estados de dispositivo ligado e desligado. Siga o princípio da consistência eventual d. são totalmente queriable na nuvem
* **Estruturar o dispositivo duplo para gestão de dispositivos:** o dispositivo duplo deve ser estruturado, de modo a que as propriedades de gestão do dispositivo estão logicamente agrupadas em secções. Se o fizer, irá ativar as alterações de configuração ser isolados sem afetar outras secções do duplo. Por exemplo, crie uma secção de propriedades pretendidas de firmware, de outra secção de software e uma terceira secção para as definições de rede. 
* **Atributos de dispositivo que são úteis para gestão de dispositivos de relatório:** atributos como dispositivo físico marca e modelo, firmware, o sistema operativo, o número de série, e outros identificadores são úteis para relatórios e como parâmetros de filtragem alterações de configuração.
* **Definir os Estados de principais para os relatórios do Estado e o progresso:** devem ser enumerados Estados de nível superior para que podem ser comunicadas ao operador. Por exemplo, uma atualização de firmware reportarão estado como atual, transferir, Applying, em curso e erro.  Defina os campos adicionais para obter mais informações sobre cada Estado.  

## <a name="iot-solution-developer"></a>Para programadores de solução IoT

Seguem-se as melhores práticas para programadores de solução IoT que estão a criar sistemas com base no Azure:

* **Implementar [dispositivos duplos][lnk-device-twins]:** dispositivos duplos ativar a sincronização de configuração pretendida a partir da nuvem e de relatórios atuais propriedades de configuração e de dispositivo.  A melhor forma de implementar dispositivos duplos dentro de aplicações de soluções em nuvem é efetuada através de [SDKs IoT do Azure][lnk-azure-sdk].  Dispositivos duplos são mais adequados para a configuração porque estes: um. suporta a comunicação bidirecional b. permitir para ambos os c Estados de dispositivo ligado e desligado. Siga o princípio da consistência eventual d. são totalmente queriable na nuvem
* **Organizar os dispositivos utilizando as etiquetas do dispositivo duplo:** a solução deve permitir que o operador definir os anéis de qualidade ou outros conjuntos de dispositivos com base em várias estratégias de implementação, tais como canary. Organização do dispositivo pode ser implementada dentro da sua solução com etiquetas do dispositivo duplo e [consultas][lnk-queries].  Organização do dispositivo é necessária permitir a configuração roll outs com segurança e com exatidão.
* **Implementar [configurações de dispositivos automático][lnk-auto-device-config]:** implementar configurações automático de dispositivos e configuração de monitor muda para grandes conjuntos de dispositivos de IoT através de dispositivos duplos.  Configurações de dispositivos automático segmentar conjuntos de dispositivos duplos através de **destino condição,** que uma consulta no dispositivo etiquetas duplo ou comunicadas propriedades. O **destino conteúdo** é o conjunto de propriedades pretendidos que serão definidas dentro dos destino dispositivos duplos. O conteúdo de destino deve cumprir a estrutura de duplo de dispositivo definida pelo fabricante/integrador do hardware IoT.  O **métricas** são comunicadas propriedades consultas no dispositivo duplo e também devem alinhar com a estrutura de duplo de dispositivo definida pelo fabricante/integrador do hardware IoT. Configurações de dispositivos automática também tem o benefício do IoT Hub efetuar operações de duplo de dispositivo a uma taxa que nunca excederá o [os limites] [ lnk-throttling] para operações de leitura do dispositivo duplo e atualizações.
* **Utilizar [serviço de aprovisionamento de dispositivos][lnk-dps]:** os programadores de solução devem utilizar o serviço de aprovisionamento de dispositivos para atribuir etiquetas do dispositivo duplo a novos dispositivos, para que estes serão automaticamente configurado pelo **configurações de dispositivos automático** que se destinam a ser duplos com essa tag. 

## <a name="iot-solution-operator"></a>Operador de solução IoT

Seguem-se as melhores práticas para os operadores de solução de IoT que através de uma solução de IoT incorporadas no Azure:

* **Dispositivos para gestão de organizar:** IoT a solução deve definir ou permitir a criação de anéis de qualidade ou outros conjuntos de dispositivos com base em várias estratégias de implementação, tais como canary. Os conjuntos de dispositivos serão utilizados para implementar as alterações de configuração e efetuar outras operações de gestão de dispositivos à escala.
* **Efetuar alterações à configuração utilizando uma agregação faseada fora:** uma agregação faseada fora é um processo geral num operador implementa as alterações a um conjunto broadening de dispositivos de IoT. O objetivo é efetuar alterações gradualmente para reduzir o risco de efetuar o dimensionamento wide interrompendo as alterações.  O operador deve utilizar a interface a solução para criar um [configuração de dispositivos automático][lnk-auto-device-config], e a condição de filtragem deve de destino um conjunto inicial de dispositivos (por exemplo, um grupo canary).  O operador deve, em seguida, validar a alteração da configuração do conjunto inicial de dispositivos.  Após a conclusão da validação, o operador atualizará a configuração de dispositivos automático para incluir um conjunto maior de dispositivos. O operador também deve definir a prioridade para a configuração ser superiores às outras configurações atualmente visadas para esses dispositivos.  A agregação saída pode ser monitorizada com as métricas comunicadas pela configuração de dispositivos automático. 
* **Efetuar reverte em caso de erros ou configurações incorretas:** uma configuração de dispositivos automática faz com que erros ou configurações incorretas pode ser revertida alterando o **filtragem condição** para que os dispositivos não já cumpre a condição de filtragem.  Certifique-se de que o se outra configuração de dispositivos automático de menor prioridade ainda é direcionada para esses dispositivos.  Certifique-se de que a reversão concluída com êxito ao visualizar as métricas: A configuração de back revertida deve deixar de Mostrar estado para dispositivos untargeted e as métricas a configuração segundo devem agora incluem contagens para os dispositivos que ainda são visados.


## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre a implementação do dispositivos duplos no [compreender e utilizar dispositivos duplos no IoT Hub][lnk-device-twins]
* Siga os passos para criar, atualizar ou eliminar uma configuração de dispositivos automático no [configurar e monitorizar dispositivos de IoT à escala][lnk-auto-device-config].
* Implementar um padrão de atualização de firmware a utilizar dispositivos duplos e configurações de dispositivos automático [Tutorial: implemente um processo de atualização de firmware do dispositivo][lnk-firmware-update].

<!-- Links -->
[lnk-device-twins]: iot-hub-devguide-device-twins.md
[lnk-module-twins]: iot-hub-devguide-module-twins.md
[lnk-auto-device-config]: iot-hub-auto-device-config.md
[lnk-firmware-update]: tutorial-firmware-update.md
[lnk-throttling]: iot-hub-devguide-quotas-throttling.md
[lnk-queries]: iot-hub-devguide-query-language.md
[lnk-dps]: ../iot-dps/how-to-manage-enrollments.md
[lnk-azure-sdk]: https://github.com/Azure/azure-iot-sdks
[lnk-solution]: https://azure.microsoft.com/features/iot-accelerators/