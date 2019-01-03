---
title: Sistemas operativos, mecanismos de contentor - Azure IoT Edge suportados | Documentos da Microsoft
description: Saiba quais sistemas operacionais podem executar o daemon de Azure IoT Edge e tempo de execução e mecanismos de contentor suportados para os seus dispositivos de produção
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/17/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 6443260de0a8bd8531edb303fa581d281034fef3
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2018
ms.locfileid: "53555613"
---
# <a name="azure-iot-edge-supported-systems"></a>Sistemas de suporte do Azure IoT Edge

Existem várias formas para buscar suporte para o produto do Azure IoT Edge.

**Relatório de bugs** – a maior parte do desenvolvimento que vai para o produto do Azure IoT Edge acontece no projeto de código-fonte aberto do IoT Edge. Bugs podem ser comunicados sobre o [página problemas](https://github.com/azure/iotedge/issues) do projeto. Correções rapidamente se tornam do projeto para atualizações de produtos.

**Equipa de suporte ao cliente da Microsoft** – os utilizadores que têm um [plano de suporte](https://azure.microsoft.com/support/plans/) pode envolver a equipe de suporte ao cliente da Microsoft através da criação de um pedido de suporte diretamente a partir do [portal do Azure](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac).

**Pedidos de funcionalidades** – produto o Azure IoT Edge controla as solicitações de recursos através do produto [página de voz do utilizador](https://feedback.azure.com/forums/907045-azure-iot-edge).

## <a name="operating-systems"></a>Sistemas operativos
O Azure IoT Edge é executado na maioria dos sistemas operacionais que possam executar contentores; No entanto, todos esses não são igualmente suportados. Sistemas operativos são agrupados em camadas que representam o nível de suporte, os usuários podem esperar.

### <a name="tier-1"></a>Escalão 1
Sistemas de escalão 1 podem ser considerados como oficialmente suportada. Isso significa que Microsoft:
* tem estas sistema operacional em testes automatizados
* Fornece os pacotes de instalação das mesmas

Disponível em geral
| Sistema Operativo | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| Raspbian stretch | Não | Sim|
| Ubuntu Server 16.04 | Sim | Não |
| Ubuntu Server 18.04 | Sim | Não |

Pré-visualização pública
| Sistema Operativo | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| Compilação do Windows 10 IoT Core 17763 | Sim | Não |
| Windows 10 criar 17763 para contentores do Windows<br><br>Windows 10 compilação 14393 ou mais recente para contentores do Linux\* | Sim | Não |
| Windows Server 2019 para contentores do Windows<br><br>Windows Server 2016 ou mais recente para contentores do Linux\* | Sim | Não |

\* A Microsoft fornece pacotes de instalação para contentores do Linux em dispositivos Windows para desenvolvimento e teste apenas. Não se trata de uma configuração suportada para utilização em produção. 

### <a name="tier-2"></a>Escalão 2
Os sistemas de camada 2 podem ser considerados como compatível com o Azure IoT Edge e podem ser utilizados com relativa facilidade. Isso significa que:
* A Microsoft fez ad-hoc nas plataformas de teste ou sabe-se de um parceiro com êxito com o Azure IoT Edge na plataforma
* Pacotes de instalação para outras plataformas podem funcionar nessas plataformas

| Sistema Operativo | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| CentOS 7.5 | Sim | Sim |
| Debian 8 | Sim | Sim |
| Debian 9 | Sim | Sim |
| RHEL 7.5 | Sim | Sim |
| Ubuntu 18.04 | Sim | Sim |
| Ubuntu 16.04 | Sim | Sim |
| O rio de vento 8 | Sim | Não |
| Yocto | Sim | Não |

## <a name="container-engines"></a>Mecanismos de contentor
O Azure IoT Edge tem um mecanismo de contentor para iniciar módulos, independentemente do sistema operativo no qual está em execução. A Microsoft fornece um mecanismo de contentor, o mecanismo moby, para cumprir este requisito. Baseia-se no projeto de código-fonte aberto Moby. Docker CE e o Docker EE são outros mecanismos de contentores populares. Eles também são baseados no projeto de código é aberto Moby e são compatíveis com o Azure IoT Edge. A Microsoft fornece o melhor de esforço de suporte para sistemas usando esses mecanismos de contentor; No entanto, a Microsoft não tem a capacidade de enviarem correções para problemas nas mesmas. Por esse motivo, a Microsoft recomenda a utilização de mecanismo de moby em sistemas de produção.

