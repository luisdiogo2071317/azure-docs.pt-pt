---
title: Suportado cenários de SAP HANA no Azure (instâncias de grande) | Microsoft Docs
description: Cenários suportados e os respetivos detalhes de arquitetura para SAP HANA no Azure (instâncias de grandes)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/25/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4b703f6d141005cf3cf29531a0586eebb61693a2
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36754576"
---
# <a name="supported-scenarios-for-hana-large-instances"></a>Cenários suportados de instâncias de grande HANA
Este documento descreve os cenários suportados, juntamente com os detalhes de arquitetura para instâncias grande HANA (HLI).

>[!NOTE]
>Se o seu cenário necessário não está aqui mencionado, contacte a equipa de gestão de serviço da Microsoft para avaliar os requisitos.
Antes de prosseguir com a unidade HLI aprovisionamento, valide a estrutura com SAP ou o parceiro de implementação de serviço.

## <a name="terms-and-definitions"></a>Termos e definições
Vamos compreender os termos e as definições utilizadas no documento.

- SID: Identificador de sistema para o sistema HANA.
- HLI: Hana grande instâncias.
- DR: Um desastre site de recuperação.
- Normal DR: uma configuração de sistema com um recurso dedicado para fins de DR apenas utilizado.
- Multipurpose DR: Um sistema de site de DR configurado para utilizar o ambiente de não produção, juntamente com a instância de produção configurada para utilizar para eventos de DR. 
- SID único: Um sistema com uma instância instalada.
- Várias SID: Um sistema com várias instâncias configurado. Também chamado um ambiente MCOS.


## <a name="overview"></a>Descrição geral
As instâncias de grande HANA suporta a variedade de arquiteturas de realizar os seus requisitos empresariais. A lista seguinte abrange os cenários e os respetivos detalhes de configuração. 

O design de arquitetura derivada é puramente a partir da perspetiva de infraestrutura e tem de consultar o SAP ou os parceiros de implementação para a implementação de HANA. Se não forem apresentados os cenários, contacte a equipa da conta Microsoft para rever a arquitetura e deriva de uma solução para si.

**Arquiteturas destes são totalmente em conformidade com a estrutura TDI (adaptados a integração de dados) e suportado pela SAP.**

Este documento descreve os detalhes dos dois componentes em cada arquitetura suportada:

- Ethernet
- Armazenamento

### <a name="ethernet"></a>Ethernet

Cada servidor aprovisionado está pré-configurada com os conjuntos de ethernet. Seguem-se os detalhes da ethernet configurada em cada unidades HLI.

- **A**: Isto é utilizado para/de acesso de cliente.
- **B**: Isto é utilizado para a comunicação do nó ao nó. Isto é configurado em todos os servidores (independentemente da topologia de pedido), mas só é utilizado para os cenários de escalamento horizontal.
- **C**: esta interface é utilizada para o nó para a conectividade de armazenamento.
- **D**: esta interface é utilizada para o nó para ligação do dispositivo ISCSI para a configuração STONITH. Esta interface está configurada apenas quando o programa de configuração HSR é solicitado.  

| INTERFACES DE LÓGICA DE NIC | TIPO DE SKU | Nome com SUSE SO | Nome com um SO RHEL | Caso de utilização|
| --- | --- | --- | --- | --- |
| A | ESCREVA I | eth0.tenant | eno1.tenant | Cliente HLI |
| B | ESCREVA I | eth2.tenant | eno3.tenant | Nó de nó |
| C | ESCREVA I | eth1.tenant | eno2.tenant | Nó de armazenamento |
| D | ESCREVA I | eth4.tenant | eno4.tenant | STONITH |
| A | TIPO II | VLAN<tenantNo> | team0.tenant | Cliente HLI |
| B | TIPO II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Nó de nó |
| C | TIPO II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Nó de armazenamento |
| D | TIPO II | VLAN < tenantNo + 3 > | team0.tenant + 3 | STONITH |

Utilize as interfaces com base na topologia configurada na unidade HLI. Por exemplo, a interface "B" está definida para a comunicação de nó para nós, que é útil quando tem uma topologia de escalamento horizontal configurada. No caso de configuração de dimensionamento de nó único, não é utilizada nesta interface. Reveja os cenários necessários (mais tarde neste documento) para obter mais informações sobre a utilização de interface. 

Se for necessário, pode definir os cartões NIC adicionais na sua própria. No entanto, a configuração nos NICs existentes não pode ser alterada.

>[!NOTE]
>Pode ainda localizar interfaces adicionais que são interfaces físicas ou bonding. Deve considerar as interfaces above-mentioned do seu incidente utilizado, pode ser ignorado rest / ou não ser tempered com.


### <a name="storage"></a>Armazenamento
Armazenamento pré-configurado com base na topologia pedida. Os tamanhos dos volumes e pontodemontagem variam com base no número de servidores, SKUs e topologia configurado. Reveja os cenários necessários (mais tarde neste documento) para obter mais informações. Se for necessário mais armazenamento, pode comprá-lo num incremento de TB.

>[!NOTE]
>O pontodemontagem /usrsap/<SID> é uma ligação simbólica para pontodemontagem hana/partilhado.


## <a name="supported-scenarios"></a>Cenários suportados

Os diagramas de arquitetura notações seguintes são utilizadas para os gráficos:

![Legends.PNG](media/hana-supported-scenario/Legends.PNG)

A lista seguinte mostra os cenários suportados:

1. Nó único com um SID
2. Nó único MCOS
3. Nó único com DR (Normal)
4. Nó único com DR (Multipurpose)
5. HSR com STONITH
6. HSR com DR (Normal / Multipurpose) 
7. Ativação pós-falha automática de anfitrião (1 + 1) 
8. Aumentar horizontalmente com o modo de espera
9. Aumentar horizontalmente sem o modo de espera
10. Aumentar horizontalmente com a DR



## <a name="1-single-node-with-one-sid"></a>1. Nó único com um SID

Esta topologia suporta um nó numa escala a configuração com um SID.

### <a name="architecture-diagram"></a>Diagrama da arquitetura  

![Nó de único com SID.png um](media/hana-supported-scenario/Single-node-with-one-SID.png)

### <a name="ethernet"></a>Ethernet
As seguintes interfaces de rede são pré-configuradas:

| INTERFACES DE LÓGICA DE NIC | TIPO DE SKU | Nome com SUSE SO | Nome com um SO RHEL | Caso de utilização|
| --- | --- | --- | --- | --- |
| A | ESCREVA I | eth0.tenant | eno1.tenant | Cliente HLI |
| B | ESCREVA I | eth2.tenant | eno3.tenant | Configurada, mas não em utilização |
| C | ESCREVA I | eth1.tenant | eno2.tenant | Nó de armazenamento |
| D | ESCREVA I | eth4.tenant | eno4.tenant | Configurada, mas não em utilização |
| A | TIPO II | VLAN<tenantNo> | team0.tenant | Cliente HLI |
| B | TIPO II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Configurada, mas não em utilização |
| C | TIPO II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Nó de armazenamento |
| D | TIPO II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Configurada, mas não em utilização |

### <a name="storage"></a>Armazenamento
As seguintes mountpoints são pré-configuradas:

| Pontodemontagem | Caso de utilização | 
| --- | --- |
|/Hana/Shared/SID | Instalação HANA | 
|/Hana/data/SID/mnt00001 | Instalar ficheiros de dados | 
|/Hana/log/SID/mnt00001 | Instalar ficheiros de registo | 
|/Hana/logbackups/SID | Os registos de Refazer |

### <a name="key-considerations"></a>Considerações principais
- /usr/SAP/SID é uma ligação simbólica para /hana/shared/SID.

## <a name="2-single-node-mcos"></a>2. Nó único MCOS

Esta topologia suporta um nó numa escala a configuração com vários SIDs.

### <a name="architecture-diagram"></a>Diagrama da arquitetura  

![Single-nó-mcos.png](media/hana-supported-scenario/single-node-mcos.png)

### <a name="ethernet"></a>Ethernet
As seguintes interfaces de rede são pré-configuradas:

| INTERFACES DE LÓGICA DE NIC | TIPO DE SKU | Nome com SUSE SO | Nome com um SO RHEL | Caso de utilização|
| --- | --- | --- | --- | --- |
| A | ESCREVA I | eth0.tenant | eno1.tenant | Cliente HLI |
| B | ESCREVA I | eth2.tenant | eno3.tenant | Configurada, mas não em utilização |
| C | ESCREVA I | eth1.tenant | eno2.tenant | Nó de armazenamento |
| D | ESCREVA I | eth4.tenant | eno4.tenant | Configurada, mas não em utilização |
| A | TIPO II | VLAN<tenantNo> | team0.tenant | Cliente HLI |
| B | TIPO II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Configurada, mas não em utilização |
| C | TIPO II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Nó de armazenamento |
| D | TIPO II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Configurada, mas não em utilização |

### <a name="storage"></a>Armazenamento
As seguintes mountpoints são pré-configuradas:

| Pontodemontagem | Caso de utilização | 
| --- | --- |
|/Hana/Shared/SID1 | Instalação HANA SID1 | 
|/Hana/data/SID1/mnt00001 | Instalar ficheiros de dados para SID1 | 
|/Hana/log/SID1/mnt00001 | Instalar ficheiros de registo para SID1 | 
|/Hana/logbackups/SID1 | Refazer registos para SID1 |
|/Hana/Shared/SID2 | Instalação HANA SID2 | 
|/Hana/data/SID2/mnt00001 | Instalar ficheiros de dados para SID2 | 
|/Hana/log/SID2/mnt00001 | Instalar ficheiros de registo para SID2 | 
|/Hana/logbackups/SID2 | Refazer registos para SID2 |

### <a name="key-considerations"></a>Considerações principais
- /usr/SAP/SID é uma ligação simbólica para /hana/shared/SID.
- Distribuição de tamanho de volume baseia-se desativar o tamanho de base de dados na memória. Consulte o [descrição geral e arquitetura](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-architecture) secção para saber que base de dados tamanhos na memória são suportadas com multisid ambiente.

## <a name="3-single-node-with-dr-normal"></a>3. Nó único com DR (Normal)
 
Esta topologia suporta um nó numa escala a configuração com um ou vários SIDs com a replicação baseada em armazenamento para o site de DR para um SID site primário. No diagrama, apenas único SID é descrito no site primário, mas multisid (MCOS) é bem suportado.

### <a name="architecture-diagram"></a>Diagrama da arquitetura  

![Único nó com dr.png](media/hana-supported-scenario/Single-node-with-dr.png)

### <a name="ethernet"></a>Ethernet
As seguintes interfaces de rede são pré-configuradas:

| INTERFACES DE LÓGICA DE NIC | TIPO DE SKU | Nome com SUSE SO | Nome com um SO RHEL | Caso de utilização|
| --- | --- | --- | --- | --- |
| A | ESCREVA I | eth0.tenant | eno1.tenant | Cliente HLI |
| B | ESCREVA I | eth2.tenant | eno3.tenant | Configurada, mas não em utilização |
| C | ESCREVA I | eth1.tenant | eno2.tenant | Nó de armazenamento |
| D | ESCREVA I | eth4.tenant | eno4.tenant | Configurada, mas não em utilização |
| A | TIPO II | VLAN<tenantNo> | team0.tenant | Cliente HLI |
| B | TIPO II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Configurada, mas não em utilização |
| C | TIPO II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Nó de armazenamento |
| D | TIPO II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Configurada, mas não em utilização |

### <a name="storage"></a>Armazenamento
As seguintes mountpoints são pré-configuradas:

| Pontodemontagem | Caso de utilização | 
| --- | --- |
|/Hana/Shared/SID | Instalação HANA do SID | 
|/Hana/data/SID/mnt00001 | Instalar ficheiros de dados para o SID | 
|/Hana/log/SID/mnt00001 | Instalar ficheiros de registo para o SID | 
|/Hana/logbackups/SID | Refazer registos para o SID |


### <a name="key-considerations"></a>Considerações principais
- /usr/SAP/SID é uma ligação simbólica para /hana/shared/SID.
- Para MCOS: Distribuição de tamanho de Volume baseia-se desativar o tamanho de base de dados na memória. Consulte o [descrição geral e arquitetura](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-architecture) secção para saber que base de dados tamanhos na memória são suportadas com multisid ambiente.
- Na DR: os volumes e mountpoints estão configurados (marcados como "Necessária para instalação HANA") para a instalação de instância HANA sempre a unidade de DR HLI de produção. 
- Na DR: os dados, logbackups e volumes partilhados (marcados como "Replicação de armazenamento") são replicados através de instantâneos do site de produção. Estas estão montados durante o período de ativação pós-falha apenas. Consulte [procedimento de ativação pós-falha de recuperação de desastres](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery#disaster-recovery-failover-procedure) para obter mais detalhes.


## <a name="4-single-node-with-dr-multipurpose"></a>4. Nó único com DR (Multipurpose)
 
Esta topologia suporta um nó numa escala a configuração com um ou vários SIDs com a replicação baseada em armazenamento para o site de DR para um SID site primário. No diagrama, apenas único SID é descrito no site primário, mas multisid (MCOS) é bem suportado. No site de DR, unidade HLI é utilizada para a instância de pergunta e resposta enquanto estiver a executar operações de produção do site primário. No momento da ativação pós-falha de DR (ou ativação pós-falha de teste), a instância de pergunta e resposta no site de DR está desativada.

### <a name="architecture-diagram"></a>Diagrama da arquitetura  

![nó de único com multipurpose.png dr](media/hana-supported-scenario/single-node-with-dr-multipurpose.png)

### <a name="ethernet"></a>Ethernet
As seguintes interfaces de rede são pré-configuradas:

| INTERFACES DE LÓGICA DE NIC | TIPO DE SKU | Nome com SUSE SO | Nome com um SO RHEL | Caso de utilização|
| --- | --- | --- | --- | --- |
| A | ESCREVA I | eth0.tenant | eno1.tenant | Cliente HLI |
| B | ESCREVA I | eth2.tenant | eno3.tenant | Configurada, mas não em utilização |
| C | ESCREVA I | eth1.tenant | eno2.tenant | Nó de armazenamento |
| D | ESCREVA I | eth4.tenant | eno4.tenant | Configurada, mas não em utilização |
| A | TIPO II | VLAN<tenantNo> | team0.tenant | Cliente HLI |
| B | TIPO II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Configurada, mas não em utilização |
| C | TIPO II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Nó de armazenamento |
| D | TIPO II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Configurada, mas não em utilização |

### <a name="storage"></a>Armazenamento
As seguintes mountpoints são pré-configuradas:

| Pontodemontagem | Caso de utilização | 
| --- | --- |
|**No site primário**|
|/Hana/Shared/SID | Instalar o HANA para produção SID | 
|/Hana/data/SID/mnt00001 | Instalar ficheiros de dados de produção SID | 
|/Hana/log/SID/mnt00001 | Instalar ficheiros de registo para a produção SID | 
|/Hana/logbackups/SID | Refazer registos para produção SID |
|**No site de DR**|
|/Hana/Shared/SID | Instalar o HANA para produção SID | 
|/Hana/data/SID/mnt00001 | Instalar ficheiros de dados de produção SID | 
|/Hana/log/SID/mnt00001 | Instalar ficheiros de registo para a produção SID | 
|/Hana/Shared/QA-SID | Instalação HANA para o SID de pergunta e resposta | 
|/Hana/data/QA-SID/mnt00001 | Instalar ficheiros de dados para o SID de pergunta e resposta | 
|/Hana/log/QA-SID/mnt00001 | Instalar ficheiros de registo para o SID de pergunta e resposta |
|/Hana/logbackups/QA-SID | Refazer registos para o SID de pergunta e resposta |

### <a name="key-considerations"></a>Considerações principais
- /usr/SAP/SID é uma ligação simbólica para /hana/shared/SID.
- Para MCOS: Distribuição de tamanho de Volume baseia-se desativar o tamanho de base de dados na memória. Consulte o [descrição geral e arquitetura](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-architecture) secção para saber que base de dados tamanhos na memória são suportadas com multisid ambiente.
- Na DR: os volumes e mountpoints estão configurados (marcados como "Necessária para instalação HANA") para a instalação de instância HANA sempre a unidade de DR HLI de produção. 
- Na DR: os dados, logbackups e volumes partilhados (marcados como "Replicação de armazenamento") são replicados através de instantâneos do site de produção. Estas estão montados durante o período de ativação pós-falha apenas. Consulte [procedimento de ativação pós-falha de recuperação de desastres](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery#disaster-recovery-failover-procedure) para obter mais detalhes. 
- Na DR: os dados, logbackups, registo, volumes partilhados de pergunta e resposta (marcados como "Instalação de instância de pergunta e resposta") estão configurados para a instalação de instância de pergunta e resposta.

## <a name="5-hsr-with-stonith"></a>5. HSR com STONITH
 
Esta topologia suporta dois nós para a configuração de replicação de sistema HANA (HSR). 

**A partir de agora, esta arquitetura é suportada apenas para o sistema operativo SUSE.**


### <a name="architecture-diagram"></a>Diagrama da arquitetura  

![HSR com STONITH.png](media/hana-supported-scenario/HSR-with-STONITH.png)



### <a name="ethernet"></a>Ethernet
As seguintes interfaces de rede são pré-configuradas:

| INTERFACES DE LÓGICA DE NIC | TIPO DE SKU | Nome com SUSE SO | Nome com um SO RHEL | Caso de utilização|
| --- | --- | --- | --- | --- |
| A | ESCREVA I | eth0.tenant | eno1.tenant | Cliente HLI |
| B | ESCREVA I | eth2.tenant | eno3.tenant | Configurada, mas não em utilização |
| C | ESCREVA I | eth1.tenant | eno2.tenant | Nó de armazenamento |
| D | ESCREVA I | eth4.tenant | eno4.tenant | Utilizado para STONITH |
| A | TIPO II | VLAN<tenantNo> | team0.tenant | Cliente HLI |
| B | TIPO II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Configurada, mas não em utilização |
| C | TIPO II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Nó de armazenamento |
| D | TIPO II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Utilizado para STONITH |

### <a name="storage"></a>Armazenamento
As seguintes mountpoints são pré-configuradas:

| Pontodemontagem | Caso de utilização | 
| --- | --- |
|**No nó principal**|
|/Hana/Shared/SID | Instalar o HANA para produção SID | 
|/Hana/data/SID/mnt00001 | Instalar ficheiros de dados de produção SID | 
|/Hana/log/SID/mnt00001 | Instalar ficheiros de registo para a produção SID | 
|/Hana/logbackups/SID | Refazer registos para produção SID |
|**No nó secundário**|
|/Hana/Shared/SID | HANA instalar para o SID secundário | 
|/Hana/data/SID/mnt00001 | Instalar ficheiros de dados para o SID secundário | 
|/Hana/log/SID/mnt00001 | Instalar ficheiros de registo para o SID secundário | 
|/Hana/logbackups/SID | Refazer registos para o SID secundário |

### <a name="key-considerations"></a>Considerações principais
- /usr/SAP/SID é uma ligação simbólica para /hana/shared/SID.
- Para MCOS: Distribuição de tamanho de Volume baseia-se desativar o tamanho de base de dados na memória. Consulte o [descrição geral e arquitetura](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-architecture) secção para saber que base de dados tamanhos na memória são suportadas com multisid ambiente.
- STONITH: Um SBD está configurado para a configuração de STONITH. No entanto, uma utilização de STONITH é opcional.


## <a name="6-hsr-with-dr"></a>6. HSR com DR
 
Esta topologia suporta dois nós para a configuração de replicação de sistema HANA (HSR). Tanto a DR normal e com várias finalidades é suportada. 

No diagrama, multipurpose cenário é descrito onde no site de DR, unidade HLI é utilizada para a instância de pergunta e resposta enquanto estiver a executar operações de produção do site primário. No momento da ativação pós-falha de DR (ou ativação pós-falha de teste), a instância de pergunta e resposta no site de DR está desativada. 



### <a name="architecture-diagram"></a>Diagrama da arquitetura  

![HSR com DR.png](media/hana-supported-scenario/HSR-with-DR.png)

### <a name="ethernet"></a>Ethernet
As seguintes interfaces de rede são pré-configuradas:

| INTERFACES DE LÓGICA DE NIC | TIPO DE SKU | Nome com SUSE SO | Nome com um SO RHEL | Caso de utilização|
| --- | --- | --- | --- | --- |
| A | ESCREVA I | eth0.tenant | eno1.tenant | Cliente HLI |
| B | ESCREVA I | eth2.tenant | eno3.tenant | Configurada, mas não em utilização |
| C | ESCREVA I | eth1.tenant | eno2.tenant | Nó de armazenamento |
| D | ESCREVA I | eth4.tenant | eno4.tenant | Utilizado para STONITH |
| A | TIPO II | VLAN<tenantNo> | team0.tenant | Cliente HLI |
| B | TIPO II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Configurada, mas não em utilização |
| C | TIPO II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Nó de armazenamento |
| D | TIPO II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Utilizado para STONITH |

### <a name="storage"></a>Armazenamento
As seguintes mountpoints são pré-configuradas:

| Pontodemontagem | Caso de utilização | 
| --- | --- |
|**No nó principal no site primário**|
|/Hana/Shared/SID | Instalar o HANA para produção SID | 
|/Hana/data/SID/mnt00001 | Instalar ficheiros de dados de produção SID | 
|/Hana/log/SID/mnt00001 | Instalar ficheiros de registo para a produção SID | 
|/Hana/logbackups/SID | Refazer registos para produção SID |
|**No nó secundário no site primário**|
|/Hana/Shared/SID | HANA instalar para o SID secundário | 
|/Hana/data/SID/mnt00001 | Instalar ficheiros de dados para o SID secundário | 
|/Hana/log/SID/mnt00001 | Instalar ficheiros de registo para o SID secundário | 
|/Hana/logbackups/SID | Refazer registos para o SID secundário |
|**No site de DR**|
|/Hana/Shared/SID | Instalar o HANA para produção SID | 
|/Hana/data/SID/mnt00001 | Instalar ficheiros de dados de produção SID | 
|/Hana/log/SID/mnt00001 | Instalar ficheiros de registo para a produção SID | 
|/Hana/Shared/QA-SID | Instalação HANA para o SID de pergunta e resposta | 
|/Hana/data/QA-SID/mnt00001 | Instalar ficheiros de dados para o SID de pergunta e resposta | 
|/Hana/log/QA-SID/mnt00001 | Instalar ficheiros de registo para o SID de pergunta e resposta |
|/Hana/logbackups/QA-SID | Refazer registos para o SID de pergunta e resposta |

### <a name="key-considerations"></a>Considerações principais
- /usr/SAP/SID é uma ligação simbólica para /hana/shared/SID.
- Para MCOS: Distribuição de tamanho de Volume baseia-se desativar o tamanho de base de dados na memória. Consulte o [descrição geral e arquitetura](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-architecture) secção para saber que base de dados tamanhos na memória são suportadas com multisid ambiente.
- STONITH: Um SBD está configurado para a configuração de STONITH. No entanto, uma utilização de STONITH é opcional.
- Na DR: **dois conjuntos de volumes de armazenamento são necessários** para replicação de nó primário e secundário.
- Na DR: os volumes e mountpoints estão configurados (marcados como "Necessária para instalação HANA") para a instalação de instância HANA sempre a unidade de DR HLI de produção. 
- Na DR: os dados, logbackups e volumes partilhados (marcados como "Replicação de armazenamento") são replicados através de instantâneos do site de produção. Estas estão montados durante o período de ativação pós-falha apenas. Consulte [procedimento de ativação pós-falha de recuperação de desastres](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery#disaster-recovery-failover-procedure) para obter mais detalhes. 
- Na DR: os dados, logbackups, registo, volumes partilhados de pergunta e resposta (marcados como "Instalação de instância de pergunta e resposta") estão configurados para a instalação de instância de pergunta e resposta.


## <a name="7-host-auto-failover-11"></a>7. Ativação pós-falha automática de anfitrião (1 + 1)
 
Esta topologia suporta dois nós numa configuração de ativação pós-falha do anfitrião automática. Não há um nó com a função de mestre/trabalho e outro como um modo de espera. **SAP suporta este cenário, apenas para S/4 HANA.** Consulte a nota OSS "[2408419 - SAP S/4HANA - suporte de vários nós](https://launchpad.support.sap.com/#/notes/2408419)" para obter mais detalhes.



### <a name="architecture-diagram"></a>Diagrama da arquitetura  

![SCA](media/hana-supported-scenario/scaleup-with-standby.png)

### <a name="ethernet"></a>Ethernet
As seguintes interfaces de rede são pré-configuradas:

| INTERFACES DE LÓGICA DE NIC | TIPO DE SKU | Nome com SUSE SO | Nome com um SO RHEL | Caso de utilização|
| --- | --- | --- | --- | --- |
| A | ESCREVA I | eth0.tenant | eno1.tenant | Cliente HLI |
| B | ESCREVA I | eth2.tenant | eno3.tenant | Nó para comunicação de nó |
| C | ESCREVA I | eth1.tenant | eno2.tenant | Nó de armazenamento |
| D | ESCREVA I | eth4.tenant | eno4.tenant | Configurada, mas não em utilização |
| A | TIPO II | VLAN<tenantNo> | team0.tenant | Cliente HLI |
| B | TIPO II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Nó para comunicação de nó |
| C | TIPO II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Nó de armazenamento |
| D | TIPO II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Configurada, mas não em utilização |

### <a name="storage"></a>Armazenamento
As seguintes mountpoints são pré-configuradas:

| Pontodemontagem | Caso de utilização | 
| --- | --- |
|**Em nós principais e de reserva**|
|hana/partilhado | Instalar o HANA para produção SID | 
|/Hana/data/SID/mnt00001 | Instalar ficheiros de dados de produção SID | 
|/Hana/log/SID/mnt00001 | Instalar ficheiros de registo para a produção SID | 
|/Hana/logbackups/SID | Refazer registos para produção SID |



### <a name="key-considerations"></a>Considerações principais
- /usr/SAP/SID é uma ligação simbólica para /hana/shared/SID.
- No modo de espera: os volumes e mountpoints estão configurados (marcados como "Necessária para instalação HANA") para a instalação de instância HANA na unidade em modo de espera.
 

## <a name="8-scale-out-with-standby"></a>8. Aumentar horizontalmente com o modo de espera
 
Esta topologia suporta múltiplos nós de uma configuração de ampliação. Não há um nó com a função de mestre, um ou mais nós com a função de trabalho e um ou mais nós como modo de espera. No entanto, podem existir apenas um nó principal em qualquer momento de determinado de tempo.


### <a name="architecture-diagram"></a>Diagrama da arquitetura  

![scaleout-nm-standby.png](media/hana-supported-scenario/scaleout-nm-standby.png)

### <a name="ethernet"></a>Ethernet
As seguintes interfaces de rede são pré-configuradas:

| INTERFACES DE LÓGICA DE NIC | TIPO DE SKU | Nome com SUSE SO | Nome com um SO RHEL | Caso de utilização|
| --- | --- | --- | --- | --- |
| A | ESCREVA I | eth0.tenant | eno1.tenant | Cliente HLI |
| B | ESCREVA I | eth2.tenant | eno3.tenant | Nó para comunicação de nó |
| C | ESCREVA I | eth1.tenant | eno2.tenant | Nó de armazenamento |
| D | ESCREVA I | eth4.tenant | eno4.tenant | Configurada, mas não em utilização |
| A | TIPO II | VLAN<tenantNo> | team0.tenant | Cliente HLI |
| B | TIPO II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Nó para comunicação de nó |
| C | TIPO II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Nó de armazenamento |
| D | TIPO II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Configurada, mas não em utilização |

### <a name="storage"></a>Armazenamento
As seguintes mountpoints são pré-configuradas:

| Pontodemontagem | Caso de utilização | 
| --- | --- |
|**Em nós de trabalho e modo de espera do mestre**|
|hana/partilhado | Instalar o HANA para produção SID | 
|/Hana/data/SID/mnt00001 | Instalar ficheiros de dados de produção SID | 
|/Hana/log/SID/mnt00001 | Instalar ficheiros de registo para a produção SID | 
|/Hana/logbackups/SID | Refazer registos para produção SID |


## <a name="9-scale-out-without-standby"></a>9. Aumentar horizontalmente sem o modo de espera
 
Esta topologia suporta múltiplos nós de uma configuração de ampliação. Não há um nó com a função principal e um ou nós de modo a função de trabalho. No entanto, podem existir apenas um nó principal em qualquer momento de determinado de tempo.


### <a name="architecture-diagram"></a>Diagrama da arquitetura  

![scaleout nm.png](media/hana-supported-scenario/scaleout-nm.png)


### <a name="ethernet"></a>Ethernet
As seguintes interfaces de rede são pré-configuradas:

| INTERFACES DE LÓGICA DE NIC | TIPO DE SKU | Nome com SUSE SO | Nome com um SO RHEL | Caso de utilização|
| --- | --- | --- | --- | --- |
| A | ESCREVA I | eth0.tenant | eno1.tenant | Cliente HLI |
| B | ESCREVA I | eth2.tenant | eno3.tenant | Nó para comunicação de nó |
| C | ESCREVA I | eth1.tenant | eno2.tenant | Nó de armazenamento |
| D | ESCREVA I | eth4.tenant | eno4.tenant | Configurada, mas não em utilização |
| A | TIPO II | VLAN<tenantNo> | team0.tenant | Cliente HLI |
| B | TIPO II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Nó para comunicação de nó |
| C | TIPO II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Nó de armazenamento |
| D | TIPO II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Configurada, mas não em utilização |

### <a name="storage"></a>Armazenamento
As seguintes mountpoints são pré-configuradas:

| Pontodemontagem | Caso de utilização | 
| --- | --- |
|**Em nós de trabalho e mestre**|
|hana/partilhado | Instalar o HANA para produção SID | 
|/Hana/data/SID/mnt00001 | Instalar ficheiros de dados de produção SID | 
|/Hana/log/SID/mnt00001 | Instalar ficheiros de registo para a produção SID | 
|/Hana/logbackups/SID | Refazer registos para produção SID |


### <a name="key-considerations"></a>Considerações principais
- /usr/SAP/SID é uma ligação simbólica para /hana/shared/SID.

## <a name="10-scale-out-with-dr"></a>10. Aumentar horizontalmente com a DR
 
Esta topologia suporta múltiplos nós de uma ampliação com um DR. DR normal e com várias finalidades é suportada. No diagrama, apenas o objetivo DR é descrito. Pode pedir esta topologia com ou sem que o nó em modo de espera.


### <a name="architecture-diagram"></a>Diagrama da arquitetura  

![scaleout com dr.png](media/hana-supported-scenario/scaleout-with-dr.png)


### <a name="ethernet"></a>Ethernet
As seguintes interfaces de rede são pré-configuradas:

| INTERFACES DE LÓGICA DE NIC | TIPO DE SKU | Nome com SUSE SO | Nome com um SO RHEL | Caso de utilização|
| --- | --- | --- | --- | --- |
| A | ESCREVA I | eth0.tenant | eno1.tenant | Cliente HLI |
| B | ESCREVA I | eth2.tenant | eno3.tenant | Nó para comunicação de nó |
| C | ESCREVA I | eth1.tenant | eno2.tenant | Nó de armazenamento |
| D | ESCREVA I | eth4.tenant | eno4.tenant | Configurada, mas não em utilização |
| A | TIPO II | VLAN<tenantNo> | team0.tenant | Cliente HLI |
| B | TIPO II | VLAN < tenantNo + 2 > | team0.tenant + 2 | Nó para comunicação de nó |
| C | TIPO II | VLAN < tenantNo + 1 > | team0.tenant + 1 | Nó de armazenamento |
| D | TIPO II | VLAN < tenantNo + 3 > | team0.tenant + 3 | Configurada, mas não em utilização |

### <a name="storage"></a>Armazenamento
As seguintes mountpoints são pré-configuradas:

| Pontodemontagem | Caso de utilização | 
| --- | --- |
|**No nó principal**|
|hana/partilhado | Instalar o HANA para produção SID | 
|/Hana/data/SID/mnt00001 | Instalar ficheiros de dados de produção SID | 
|/Hana/log/SID/mnt00001 | Instalar ficheiros de registo para a produção SID | 
|/Hana/logbackups/SID | Refazer registos para produção SID |
|**No nó de DR**|
|hana/partilhado | Instalar o HANA para produção SID | 
|/Hana/data/SID/mnt00001 | Instalar ficheiros de dados de produção SID | 
|/Hana/log/SID/mnt00001 | Instalar ficheiros de registo para a produção SID | 


### <a name="key-considerations"></a>Considerações principais
- /usr/SAP/SID é uma ligação simbólica para /hana/shared/SID.
-  Na DR: os volumes e mountpoints estão configurados (marcados como "Necessária para instalação HANA") para a instalação de instância HANA sempre a unidade de DR HLI de produção. 
- Na DR: os dados, logbackups e volumes partilhados (marcados como "Replicação de armazenamento") são replicados através de instantâneos do site de produção. Estas estão montados durante o período de ativação pós-falha apenas. Consulte [procedimento de ativação pós-falha de recuperação de desastres](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery#disaster-recovery-failover-procedure) para obter mais detalhes. 


## <a name="next-steps"></a>Passos Seguintes
- Consulte [infraestrutura e a conectividade](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-infrastructure-connectivity) para HLI
- Consulte [elevada disponibilidade e recuperação após desastre](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/hana-overview-high-availability-disaster-recovery) para HLI