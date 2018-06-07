---
title: Certificações do Microsoft Azure para SAP | Microsoft Docs
description: Lista atualizada de configurações atuais e certificações de SAP na plataforma do Azure.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2018
ms.author: rclaus
ms.custom: ''
ms.openlocfilehash: f2d342558e83c54e101e0ff9128611da9bec1e77
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34656963"
---
# <a name="sap-certifications-and-configurations-running-on-microsoft-azure"></a>SAP certificações e configurações em execução no Microsoft Azure

SAP e a Microsoft tem um comprimento histórico de trabalhar em conjunto numa parceria segura que tem mútua vantagens para os seus clientes. Microsoft está constantemente a atualizar a sua plataforma e submeter novos detalhes de certificação para SAP para garantir que o Microsoft Azure é a melhor plataforma para executar as cargas de trabalho do SAP. O seguinte contorno de tabelas do Azure suportadas configurações e lista de certificações de SAP a crescer. 

## <a name="sap-hana-certifications"></a>Certificações SAP HANA
Referências:

- [SAP HANA certificadas IaaS plataformas](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure) para suporte de SAP HANA para nativo VMs do Azure e instâncias de grande HANA.

| Produto SAP | SO Suportados | Ofertas do Azure |
| --- | --- | --- |
| Edição de programador do SAP HANA (incluindo o software de cliente HANA composto SQLODBC, ODBO Windows apenas, ODBC, JDBC controladores, HANA studio e a base de dados HANA) | Red Hat Enterprise Linux, SUSE Linux Enterprise | Família de série D VM |
| HANA num de negócio | SUSE Linux Enterprise | DS14_v2 |
| SAP HANA DE S/4 | Red Hat Enterprise Linux, SUSE Linux Enterprise | Disponibilidade controlada para GS5 M64s, M64ms, M128s, M128ms, SAP HANA no Azure (instâncias de grandes) |
| Suite no HANA, OLTP | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5 para cenários de não produção, M64s M64ms, M128s, M128ms, SAP HANA no Azure (instâncias de grandes) |
| HANA Enterprise para BW, OLAP | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5 M64s, M64ms, M128s, M128ms, SAP HANA no Azure (instâncias de grandes) |
| SAP BW/4 HANA | Red Hat Enterprise Linux, SUSE Linux Enterprise | GS5 M64s, M64ms, M128s, M128ms, SAP HANA no Azure (instâncias de grandes) |

Todas as VMs do Azure são certificadas para SAP HANA vertical até ao momento.

## <a name="sap-netweaver-certifications"></a>Certificações SAP NetWeaver
O Microsoft Azure está certificado para os seguintes produtos SAP, com suporte total da Microsoft e da SAP.
Referências:

- [1928533 - aplicações SAP no Azure: os tipos de produtos suportados e VM do Azure](https://launchpad.support.sap.com/#/notes/1928533) para todas as aplicações de NetWeaver SAP com base, incluindo SAP TREX, SAP LiveCache e servidor de conteúdo de SAP. Todas as bases de dados e, excluindo SAP HANA.


| Produto SAP | SO convidado | RDBMS | Tipos de Máquina Virtual |
| --- | --- | --- | --- |
| Software SAP Business Suite | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows e Oracle Linux apenas), DB2, SAP ASE |A5 para A11, D11 para D14, DS11 para DS14, DS11_v2 para DS15_v2, GS1 para GS5, D2s_v3 para D64s_v3, E2s_v3 para E64s_v3, M64s para M128ms |
| SAP Business All-in-One | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows e Oracle Linux apenas), DB2, SAP ASE |A5 para A11, D11 para D14, DS11 para DS14, DS11_v2 para DS15_v2, GS1 para GS5, D2s_v3 para D64s_v3, E2s_v3 para E64s_v3, M64s para M128ms |
| SAP BusinessObjects BI | Windows |N/A |A5 para A11, D11 para D14, DS11 para DS14, DS11_v2 para DS15_v2, GS1 para GS5, D2s_v3 para D64s_v3, E2s_v3 para E64s_v3, M64s para M128ms |
| SAP NetWeaver | Windows, SUSE Linux Enterprise, Red Hat Enterprise Linux, Oracle Linux |SQL Server, Oracle (Windows e Oracle Linux apenas), DB2, SAP ASE |A5 para A11, D11 para D14, DS11 para DS14, DS11_v2 para DS15_v2, GS1 para GS5, D2s_v3 para D64s_v3, E2s_v3 para E64s_v3, M64s para M128ms |

## <a name="other-sap-workload-supported-on-azure"></a>Outras cargas de trabalho do SAP suportadas no Azure

| Produto SAP | SO convidado | RDBMS | Tipos de Máquina Virtual |
| --- | --- | --- | --- |
| SAP Business no SQL Server | Windows  | SQL Server | Certificado de NetWeaver todos os tipos VM |
| SAP BPC 10.01 MS SP08 | Windows e Linux | | Todos os tipos de NetWeaver certificadas VM<br /> A nota #2451795 |
| Plataforma de negócio objetos BI do SAP | Windows e Linux | | A nota #2145537 |
| Serviços de dados SAP 4.2 | | | A nota #2288344 |
| Plataforma de comércio Hybris SAP 5. x e 6. x | Windows | SQL Server, Oracle | Certificado de NetWeaver todos os tipos VM<br /> [Hybris Wiki](https://wiki.hybris.com/display/SUP/Using+the+hybris+Platform+with+the+Cloud) |
