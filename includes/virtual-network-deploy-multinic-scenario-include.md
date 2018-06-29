---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: cab04a7eafbc21e0d26cd5a287f3dbee8d3d22b7
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "31805109"
---
## <a name="scenario"></a>Cenário
Este documento explica como através de uma implementação que utiliza vários NICs nas VMs num cenário específico. Neste cenário, tem uma duas camadas IaaS carga de trabalho alojada no Azure. Cada camada está implementada na sua própria sub-rede numa rede virtual (VNet). A camada de front-end é composta por vários servidores web, agrupados num Balanceador de carga definido para elevada disponibilidade. A camada de back-end é composta por vários servidores de base de dados. Os servidores de base de dados são implementados com dois NICs cada, um para acesso de base de dados, outra para gestão. O cenário também inclui grupos de segurança de rede (NSGs) para controlar o tráfego é permitido para cada sub-rede e NIC na implementação. A imagem seguinte mostra a arquitetura básica deste cenário:

![Cenário de MultiNIC](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

