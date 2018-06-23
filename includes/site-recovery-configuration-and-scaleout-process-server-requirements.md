---
title: incluir ficheiro
description: incluir ficheiro
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 06/10/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: bc1beb53c5919914919e666ecf1836ec45d32ccf
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36319731"
---
**Requisitos do servidor/processo de configuração**

**Componente** | **Requisito** 
--- | ---
**DEFINIÇÕES DE HARDWARE** | 
Núcleos de CPU | 8 
RAM | 16 GB
Número de discos | 3, incluindo o disco de SO, disco de cache do servidor de processo e unidade de retenção para reativação pós-falha 
Espaço livre em disco (cache do servidor de processo) | 600 GB
Espaço livre em disco (disco de retenção) | 600 GB
 | 
**DEFINIÇÕES DE SOFTWARE** | 
Sistema operativo | Windows Server 2012 R2 <br> Windows Server 2016
Região do sistema operativo | Inglês (en-us)
Funções do Windows Server | Não ative estas funções: <br> - Active Directory Domain Services <br>- Serviços de Informação da Internet <br> - Hyper-V 
Políticas de grupo | Não ative estas políticas de grupo: <br> -Impedi o acesso à linha de comandos. <br> -Impedi o acesso ao registo ferramentas de edição. <br> -Confia lógica para anexos de ficheiros. <br> -Ative a execução do Script. <br> [Saiba mais](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | -Qualquer Web site predefinido do pré-existentes <br> -Qualquer Web site/aplicação preexistente escutar na porta 443 <br>-Ativar [autenticação anónima](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Ativar [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) definição 
| 
**DEFINIÇÕES DE REDE** | 
Tipo de endereço IP | Estático 
Acesso à Internet | O servidor precisa de aceder a estes URLs (diretamente ou através do proxy) <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi (se estiver a configurar um servidor de configuração) <br> - time.nist.gov <br> - time.windows.com 
Portas | 443 (Canal de controlo e orquestração)<br>9443 (Transporte de dados) 
Tipo NIC | VMXNET3 (se o servidor de configuração de uma VM de VMware)
 | 
**SOFTWARE PARA INSTALAÇÃO** | 
VMware vSphere PowerCLI | [PowerCLI versão 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) deve ser instalada se o servidor de configuração está em execução numa VMware VM.
MYSQL | Deve ser instalado o MySQL. Pode instalar manualmente ou recuperação de sites pode instalá-lo.

**Servidor de configuração/processe os requisitos de dimensionamento**

**CPU** | **Memória** | **Disco de cache** | **Taxa de alteração de dados** | **Máquinas replicadas**
--- | --- | --- | --- | ---
8 vCPUs<br/><br/> 2 sockets * 4 núcleos @ 2,5 GHz | 16GB | 300 GB | 500 GB ou inferior | < 100 máquinas
12 vCPUs<br/><br/> 2 socks * 6 núcleos @ 2,5 GHz | 18 GB | 600 GB | 500 GB - 1 TB | máquinas de 100 a 150
16 vCPUs<br/><br/> 2 socks * 8 núcleos @ 2,5 GHz | 32 GB | 1 TB | 1-2 TB | 150-200 máquinas

