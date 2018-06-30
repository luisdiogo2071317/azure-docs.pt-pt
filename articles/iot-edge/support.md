---
title: O suporte da plataforma do Azure IoT Edge | Microsoft Docs
description: Plataformas suportadas pelo Azure IoT Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 6/21/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 05a571c4491122ec5c7c35f6bccc4b8c332a4be2
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130714"
---
# <a name="azure-iot-edge-support"></a>Suporte de limite de IoT do Azure
Existem várias formas para procurar o ressarcimento de suporte para o produto de limite de IoT do Azure.

**Relatório de erros** – a maioria das desenvolvimento entra no produto Azure IoT Edge acontece no projeto de open source do limite de IoT. Erros podem ser comunicados no [página problemas](https://github.com/azure/iotedge/issues) do projeto. Correções tornar rapidamente os seus forma do projeto em atualizações de produto.

**Equipa de suporte ao cliente da Microsoft** – os utilizadores que têm um [plano de suporte](https://azure.microsoft.com/support/plans/) pode interagir com a equipa de suporte ao cliente da Microsoft através da criação de um pedido de suporte diretamente a partir de [portal do Azure]( https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac).

**Funcionalidade pedidos** – produto o limite de IoT do Azure controla os pedidos de funcionalidades através do produto [página de voz do utilizador](https://feedback.azure.com/forums/907045-azure-iot-edge).

## <a name="operating-systems"></a>Sistemas Operativos
Limite de IoT do Azure é executado na maioria dos sistemas operativos que podem ser executados contentores; No entanto, todos estes não são igualmente suportados. Sistemas operativos estão agrupados em camadas que representam o nível de suporte, os utilizadores podem esperar.

### <a name="tier-1"></a>Escalão 1
Sistemas de camada 1 podem considerar como oficialmente suportado. Isto significa que Microsoft:
* tem estes sistema operativo nos testes automatizados
* Fornece pacotes de instalação para os mesmos

Geralmente disponível
* Ubuntu Server 18.04
* Ubuntu Server 16.04
* Raspbian stretch

Pré-visualização pública
* Servidor do Windows 10 1803
* Windows 10 Enterprise de IoT (com de 2018 de Abril de atualização)
* Windows 10 IoT Core (com de 2018 de Abril de atualização)

### <a name="tier-2"></a>Escalão 2
Podem considerar como compatível com o Azure IoT Edge e podem ser utilizados relativamente facilmente sistemas de camada 2. Isto significa que:
* Efetuou ad-hoc testar nas plataformas da Microsoft ou sabe de um parceiro a ser executada com êxito do Azure IoT Edge na plataforma
* Pacotes de instalação para outras plataformas podem funcionar nas plataformas seguintes

Ubuntu 18.04

Ubuntu 16.04

Vento River 8

Yocto

Debian

Mac

## <a name="container-engines"></a>Motores de contentor
Limite de IoT do Azure necessita de um motor de contentor para iniciar módulos, independentemente do sistema operativo no qual está a ser executado. A Microsoft fornece um motor de contentor, moby-motor para cumprir este requisito. Se baseia no projeto Moby open source. Docker CE e Docker EE são outros motores de contentor populares. Estes também são baseadas no projeto de origem é aberto Moby e são compatíveis com o Azure IoT Edge. A Microsoft fornece melhor esforço suporte para sistemas com os motores de contentor; No entanto, a Microsoft não têm a capacidade para enviar correções para problemas nas mesmas. Por este motivo, a Microsoft recomenda a utilização de motor de moby em sistemas de produção.


<!-- Links -->
[lnk-edge-blog]: https://azure.microsoft.com/blog/securing-the-intelligent-edge/ 
