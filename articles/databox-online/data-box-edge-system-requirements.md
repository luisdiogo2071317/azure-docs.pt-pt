---
title: Requisitos de sistema do Edge de caixa de dados do Microsoft Azure | Documentos da Microsoft
description: Saiba mais sobre o software e os requisitos de rede para a sua caixa de dados do Azure Edge
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 11/06/2018
ms.author: alkohli
ms.openlocfilehash: 7f03953739eebc63c57b30750e15329f24a00537
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263583"
---
# <a name="azure-data-box-edge-system-requirements-preview"></a>Requisitos de sistema de borda de caixa de dados do Azure (pré-visualização)

Este artigo descreve os requisitos de sistema importante para a sua solução de borda de caixa de dados do Microsoft Azure e para os clientes conectados à borda de caixa de dados do Azure. Recomendamos que reveja as informações cuidadosamente antes de implementar o seu limite de caixa de dados. Pode consultar novamente a essas informações conforme necessário durante a implantação e operação subseqüente.

Os requisitos de sistema para o Edge de caixa de dados incluem:

- **Requisitos de software para anfitriões** -descreve as plataformas suportadas, browsers para a configuração local da interface do Usuário, os clientes do SMB e quaisquer requisitos adicionais para os clientes que aceder ao dispositivo.
- **Requisitos de rede para o dispositivo** -fornece informações sobre quaisquer requisitos de rede para a operação do dispositivo físico.

> [!IMPORTANT]
> O Data Box Edge está em pré-visualização. Reveja os [termos de utilização da pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de implementar esta solução.

## <a name="supported-os-for-clients-connected-to-device"></a>SO suportado para os clientes ligados ao dispositivo

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-supported-client-os.md)]

## <a name="supported-protocols-for-clients-accessing-device"></a>Protocolos suportados para os clientes a aceder ao dispositivo

[!INCLUDE [Supported protocols for clients accessing device](../../includes/data-box-edge-gateway-supported-client-protocols.md)]

## <a name="supported-storage-accounts"></a>Contas de armazenamento suportadas

[!INCLUDE [Supported storage accounts](../../includes/data-box-edge-gateway-supported-storage-accounts.md)]

## <a name="supported-storage-types"></a>Tipos de armazenamento suportadas

[!INCLUDE [Supported storage types](../../includes/data-box-edge-gateway-supported-storage-types.md)]

## <a name="supported-browsers-for-local-web-ui"></a>Browsers suportados para a IU web local

[!INCLUDE [Supported browsers for local web UI](../../includes/data-box-edge-gateway-supported-browsers.md)]

## <a name="port-configuration-for-data-box-edge"></a>Configuração da porta para dados de caixa de borda

A tabela seguinte lista as portas que têm de ser aberta na firewall para permitir o tráfego de gestão, na cloud ou SMB. Nesta tabela, *no* ou *entrada* refere-se para a direção do que acesso de pedidos de cliente recebidos para o seu dispositivo. *Out* ou *saída* refere-se para a direção em que o dispositivo do Edge de caixa de dados envia dados externamente, além da implementação, por exemplo, com a Internet à saída.

[!INCLUDE [Port configuration for device](../../includes/data-box-edge-gateway-port-config.md)]

## <a name="port-configuration-for-iot-edge"></a>Configuração da porta para o IoT Edge

O Azure IoT Edge permite a comunicação de saída a partir de um dispositivo do Edge no local para a cloud do Azure através de protocolos suportados do IoT Hub. Comunicação de entrada só é necessário para cenários específicos em que o IoT Hub do Azure precisa para enviar mensagens para o dispositivo Azure IoT Edge (por exemplo, a Cloud para mensagens do dispositivo).

Utilize a seguinte tabela para a configuração da porta para os servidores que alojam o runtime do Azure IoT Edge:

| Porta não. | Dentro ou para fora | Âmbito de porta | Necessário | Orientação |
|----------|-----------|------------|----------|----------|
| TCP 5671 (AMQP)| Saída       | WAN        | Sim      | Protocolo de comunicação padrão para o IoT Edge. Tem de estar aberta se Azure IoT Edge não está configurado para outros protocolos suportados ou AMQP é o protocolo de comunicação desejada. <br>5672 para AMQP não é suportado pelo IoT Edge. <br>Bloquear esta porta, quando o protocolo de suporte do Azure IoT Edge utiliza um Hub de IoT diferentes. |
| TCP 443 (HTTPS)| Saída       | WAN        | Sim      | Saída aberto para o aprovisionamento do IoT Edge. Se tiver um gateway transparente com dispositivos de folha que poderá enviar pedidos de método. Neste caso, a porta 443 não precisa de estar abertas a redes externas para ligar ao IoT Hub ou fornecer serviços de IoT Hub através do Azure IoT Edge. Assim, a regra de entrada pode ser restrita para abrirem apenas de entrada da rede interna. |
| TCP 5671 (AMQP) | Em        |            | Não       | Ligações de entrada devem ser bloqueadas.|
| TCP 443 (HTTPS) | Em        |            | Em alguns casos, veja os comentários | Ligações de entrada devem ser abertas apenas para cenários específicos. Se não-HTTP protocolos, como o AMQP, MQTT não pode ser configurado, as mensagens podem ser enviadas por WebSockets através da porta 443. |

Para obter informações completas, aceda a [Firewall e regras de configuração de porta para a implementação do IoT Edge](https://docs.microsoft.com/azure/iot-edge/troubleshoot).

## <a name="url-patterns-for-firewall-rules"></a>Padrões de URL para regras de firewall

Os administradores de rede, muitas vezes, podem configurar regras de firewall avançado com base nos padrões de URL para filtrar a entrada e o tráfego de saída. O dispositivo de limite de caixa de dados e o serviço dependem de outros aplicativos da Microsoft, como o Azure Service Bus, controlo de acesso do Azure Active Directory, contas de armazenamento e servidores do Microsoft Update. Os padrões de URL associados a estas aplicações podem ser utilizados para configurar regras de firewall. É importante compreender que os padrões de URL associados a estas aplicações podem alterar. Estas alterações exigem que o administrador de rede monitorizar e atualizar as regras de firewall para o seu limite de caixa de dados como e quando necessário.

Recomendamos que defina as regras de firewall para tráfego de saída, com base no limite de caixa de dados fixa livremente os endereços IP, na maioria dos casos. No entanto, pode utilizar as informações abaixo para definir as regras de firewall avançado que são necessários para criar ambientes seguros.

> [!NOTE]
> - O dispositivo (origem) IPs deve ser sempre definido para todas as interfaces de rede com capacidade de nuvem.
> - O destino IPs deve ser definido como [intervalos IP do datacenter do Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653).

|     Padrão de URL                                                                                                                                                                                                                                                                                                                                                                                                                                       |     Componente ou funcionalidade                                                                             |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
|    https://*.databoxedge.azure.com/*<br>https://*.servicebus.windows.net/*<br>https://login.windows.net                                                                                                                                                                                                                                                                                                        |    Serviço de borda de caixa de dados do Azure<br>Service Bus do Azure<br>Serviço de Autenticação    |
|    http://*.backup.windowsazure.com                                                                                                                                                                                                                                                                                                                                                                                                                   |    Ativação do dispositivo                                                                                    |
|    http://crl.microsoft.com/pki/*   http://www.microsoft.com/pki/*                                                                                                                                                                                                                                                                                                                                                                                    |    Revogação de certificados                                                                               |
|    https://*.core.windows.net/* https://*. data.microsoft.com http://*. msftncsi.com                                                                                                                                                                                                                                                                                                                                                                |    Contas de armazenamento do Azure e monitorização                                                                |
|    http://windowsupdate.microsoft.com<br>http://*. windowsupdate.microsoft.com<br>https://*. windowsupdate.microsoft.com<br>http://*. update.microsoft.com<br>https://*. update.microsoft.com<br>http://*. windowsupdate.com<br>http://download.microsoft.com<br>http://*. download.windowsupdate.com<br>http://wustat.windows.com<br>http://ntservicepack.microsoft.com<br>http://*. ws.microsoft.com<br>https://*. ws.microsoft.com<br>http://*.MP.microsoft.com        |    Servidores do Microsoft Update                                                                             |
|    http://*.Deploy.akamaitechnologies.com                                                                                                                                                                                                                                                                                                                                                                                                             |    Akamai CDN                                                                                           |
|    https://*.partners.extranet.microsoft.com/*                                                                                                                                                                                                                                                                                                                                                                                                        |    Pacote de suporte                                                                                      |
|    http://*.data.microsoft.com                                                                                                                                                                                                                                                                                                                                                                                                                        |    O serviço de telemetria no Windows, consulte a atualização para a experiência do cliente e a telemetria de diagnóstico      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                       |                                                                                                         |

## <a name="internet-bandwidth"></a>Largura de banda de Internet

[!INCLUDE [Internet bandwidth](../../includes/data-box-edge-gateway-internet-bandwidth.md)]

## <a name="next-step"></a>Passo seguinte

* [Implementar o seu limite de caixa de dados do Azure](data-box-Edge-deploy-prep.md)
