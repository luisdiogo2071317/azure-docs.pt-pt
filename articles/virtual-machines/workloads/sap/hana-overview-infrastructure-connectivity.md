---
title: Infraestrutura e a conectividade ao SAP HANA no Azure (instâncias grandes) | Documentos da Microsoft
description: Configure a infraestrutura de conectividade necessária para utilizar o SAP HANA no Azure (instâncias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c61dffc6fd9d0c65f5e925daebdf2a02cfb5d6ba
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44161361"
---
# <a name="sap-hana-large-instances-deployment"></a>Implementação de SAP HANA (instâncias grandes) 

Após a compra de SAP HANA no Azure (instâncias grandes) é finalizada entre si e a equipa da conta do Microsoft enterprise, as seguintes informações é necessário pela Microsoft para implementar unidades de instância grande do HANA:

- Nome do cliente
- Informações de contacto de negócio (incluindo o endereço de email e número de telefone)
- Informações de contato técnicas (incluindo Email endereço e número de telefone)
- Técnico de informação de contacto (incluindo Email endereço e número de telefone) de rede
- Região de implementação do Azure (E.U.A. oeste, E.U.A. leste, leste da Austrália, Sudeste da Austrália, Europa Ocidental e norte da Europa a partir de Julho de 2017)
- Confirmar o SAP HANA no Azure (instâncias grandes) SKU (configuração)
- Como já detalhadas no documento de visão geral e arquitetura para instâncias grandes do HANA, para cada região do Azure que está a ser implementada:
    - / 29 intervalo de endereços IP para ligações de ER P2P que ligar VNets do Azure para instâncias grandes do HANA
    - Um /24 bloco CIDR utilizados para o conjunto IP do HANA grandes instâncias de servidor
- Os valores de intervalos de endereços IP utilizados no atributo de espaço de endereços VNet de cada VNet do Azure que liga para o HANA nas instâncias grandes
- Dados para cada sistema de instâncias grandes do HANA:
  - Nome de anfitrião desejado - o ideal é que com o nome de domínio completamente qualificado.
  - Endereço IP pretendido para a unidade de instância grande do HANA fora do intervalo de endereços do conjunto de IP do servidor - tenha em atenção que os 30 primeiros endereços IP no intervalo de endereços de conjunto de IP do servidor estão reservados para utilização interna no HANA nas instâncias grandes
  - Nome em SID do SAP HANA para a instância do SAP HANA (obrigatório para criar os volumes de disco necessário relacionados com o SAP HANA). O SID do HANA é necessário para criar as permissões para sidadm nos volumes de NFS, que estão a obter anexados à unidade de instância grande do HANA. Também é utilizado como um dos componentes do nome dos volumes de disco que ser montados. Se quiser executar mais de uma instância HANA na unidade, precisa de listar vários SIDs do HANA. Cada versão recebe um conjunto separado de volumes atribuídos.
  - O groupid que tem do utilizador sidadm no SO Linux é necessário para criar os volumes de disco necessário relacionados com o SAP HANA. A instalação de SAP HANA, normalmente, cria o grupo de sapsys com um id de grupo de 1001. O utilizador de sidadm faz parte desse grupo
  - O ID de utilizador que tem do utilizador sidadm no SO Linux é necessário para criar os volumes de disco necessário relacionados com o SAP HANA. Se estiver a executar várias instâncias do HANA na unidade, precisa listar todos os <sid>utilizadores do adm 
- ID de subscrição do Azure para a subscrição do Azure para o SAP HANA no Azure HANA nas instâncias grandes vão ser ligados diretamente. Este ID de subscrição faz referência a subscrição do Azure, o que vai ser cobrado com a instância grande do HANA unidade (s).

Depois de fornecer as informações, a Microsoft Aprovisiona o SAP HANA no Azure (instâncias grandes) e retornará as informações necessárias para ligar as VNets do Azure para instâncias grandes do HANA e para acessar as unidades de instância grande do HANA.

Antes de ler este artigo, familiarize-se com [termos comuns de instâncias grandes do HANA](hana-know-terms.md) e o [SKUs de instâncias grandes do HANA](hana-available-skus.md).

Pode usar a seguinte sequência para ligar a instâncias grandes do HANA após a respetiva implementação no Microsoft:

1. [Ligar VMs do Azure para instâncias grandes do HANA](hana-connect-azure-vm-large-instances.md)
2. [Ligar uma VNet para ExpressRoute de instâncias grandes do HANA](hana-connect-vnet-express-route.md)
3. [Requisitos de rede adicional (opcionais)](hana-additional-network-requirements.md)

**Passos seguintes?**

- Consultar [ligar VMs do Azure para instâncias grandes do HANA](hana-connect-azure-vm-large-instances.md).
- Consultar [ligação de uma VNet para ExpressRoute de instância grande do HANA](hana-connect-vnet-express-route.md).