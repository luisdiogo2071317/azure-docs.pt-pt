---
title: Requisitos de integração para o SAP HANA no Azure (instâncias grandes) | Documentos da Microsoft
description: Requisitos de integração para o SAP HANA no Azure (instâncias grandes).
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
ms.date: 09/04/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d287f79f4161c509f96b679d4b794c2906f2e020
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028230"
---
# <a name="onboarding-requirements"></a>Requisitos de integração

Esta lista monta requisitos para executar o SAP HANA no Azure (instâncias grandes).

**Microsoft Azure**

- Uma subscrição do Azure que pode ser ligada ao SAP HANA no Azure (instâncias grandes).
- Contrato de suporte Premier da Microsoft. Para obter informações específicas relacionadas com a executar o SAP no Azure, consulte [2015553 de n. º de nota de suporte de SAP – SAP no Microsoft Azure: pré-requisitos do suporte](https://launchpad.support.sap.com/#/notes/2015553). Se utilizar unidades de instância grande do HANA com 384 e mais CPUs, terá também de estender o Premier do contrato de suporte para incluir a resposta rápida do Azure.
- Reconhecimento dos SKUs de instância grande HANA é necessário depois de realizar um exercício de dimensionamento com o SAP.

**Conectividade de rede**

- ExpressRoute entre no local para o Azure: ligar o seu centro de dados no local ao Azure, certifique-se solicitar, pelo menos, uma ligação de 1 Gbps a partir do seu ISP. 

**Sistema operativo**

- Licenças para o SUSE Linux Enterprise Server 12 para aplicações SAP.

   > [!NOTE] 
   > O sistema de operativo fornecido pela Microsoft não se encontra registado SUSE. Ele não está ligado a uma instância de ferramenta de gestão de subscrição.

- SUSE Linux subscrição ferramenta de gestão implementados no Azure numa VM. Essa ferramenta fornece a capacidade para o SAP HANA no Azure (instâncias grandes) para ser registrado e respectivamente atualizado pelo SUSE. (Não há nenhum acesso à internet no Centro de dados HANA de instância grande). 
- Licenças para Red Hat Enterprise Linux 6.7 ou 7.x para o SAP HANA.

   > [!NOTE]
   > O sistema de operativo fornecido pela Microsoft não está registado com a Red Hat. Ele não está ligado a uma instância do Gestor de subscrições do Red Hat.

- Red Hat subscrição Manager implementados no Azure numa VM. O Gestor de subscrições do Red Hat fornece a capacidade para o SAP HANA no Azure (instâncias grandes) a ser registado e atualizado respectivamente da Red Hat. (Não há nenhum acesso direto à internet do inquilino implementadas no carimbo de instâncias grandes do Azure).
- O SAP requer que tenha suporte a um contrato com o seu fornecedor do Linux também. Este requisito não é removido pela solução de instância grande do HANA ou o fato de que executam o Linux no Azure. Ao contrário com algumas das imagens da galeria do Linux do Azure, a tarifa de serviço é *não* incluída na oferta de solução da instância grande do HANA. É da responsabilidade do cliente para satisfazer os requisitos de SAP sobre contratos de suporte com o distribuidor do Linux. 
   - Para o SUSE Linux, procure os requisitos de contratos de suporte [1984787 no SAP Note # - SUSE Linux Enterprise Server 12: observações de instalação](https://launchpad.support.sap.com/#/notes/1984787) e [1056161 no SAP Note # - suporte de prioridade do SUSE para aplicações SAP](https://launchpad.support.sap.com/#/notes/1056161).
   - Para Red Hat Linux, tem de ter os níveis de assinatura corretos que incluem o suporte e serviço de atualizações para os sistemas operativos de instância grande do HANA. Red Hat recomenda a subscrição do Red Hat Enterprise Linux para a solução SAP. Consulte https://access.redhat.com/solutions/3082481. 

Para a matriz de suporte de diferentes versões do SAP HANA com diferentes versões do Linux, consulte [SAP Note #2235581](https://launchpad.support.sap.com/#/notes/2235581).

Para a matriz de compatibilidade do sistema operativo e versões de firmware/controlador HLI, consulte [atualização do SO para HLI](os-upgrade-hana-large-instance.md).


> [!IMPORTANT] 
> Unidades de tipo II apenas o SLES 12 SP2 sistema operacional de versão é suportada neste momento. 


**Base de Dados**

- As licenças e componentes de instalação de software para o SAP HANA (plataforma ou enterprise edition).

**Aplicações**

- Contratos do suporte de licenças e componentes de instalação de software para todas as aplicações SAP que liguem ao SAP HANA e SAP relacionado.
- Licenças e componentes de instalação de software para todos os aplicativos não pertencentes à SAP utilizado em relação ao SAP HANA em ambientes do Azure (instâncias grandes) e relacionadas com contratos de suporte.

**Habilidades**

- Experiência com e conhecimento do IaaS do Azure e os respetivos componentes.
- Experiência com e o conhecimento de como implementar uma carga de trabalho SAP no Azure.
- Instalação do SAP HANA certificadas pessoal.
- SAP habilidades arquiteto projetar elevada disponibilidade e recuperação após desastre em torno do SAP HANA.

**SAP**

- Expectativa é que for um cliente do SAP e têm suporte para um contrato com o SAP.
- Especialmente para implementações da classe de tipo II de SKUs de instância grande do HANA, consulte o SAP em versões do SAP HANA e as configurações eventual no hardware de ampliação de grande dimensão.

**Passos seguintes?**
- Consulte [arquitetura do SAP HANA (instâncias grandes) no Azure](hana-architecture.md)