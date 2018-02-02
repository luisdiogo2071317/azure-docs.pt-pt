---
title: Resolver problemas do System Center Data Protection Manager com o Backup do Azure | Microsoft Docs
description: Resolva problemas do System Center Data Protection Manager.
services: backup
documentationcenter: 
author: adigan
manager: shreeshd
editor: 
ms.assetid: 2d73c349-0fc8-4ca8-afd8-8c9029cb8524
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: pullabhk;markgal;adigan
ms.openlocfilehash: 2244a39217f54eb5906d05990f19fc8ca2fdeb0e
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="troubleshoot-system-center-data-protection-manager"></a>Resolver problemas do System Center Data Protection Manager

Pode encontrar as notas de versão mais recente do SC DPM [aqui](https://docs.microsoft.com/en-us/system-center/dpm/dpm-release-notes?view=sc-dpm-2016).
A matriz de suporte de proteção podem ser encontrada [aqui](https://docs.microsoft.com/en-us/system-center/dpm/dpm-protection-matrix?view=sc-dpm-2016).

## <a name="replica-is-inconsistent"></a>A réplica está inconsistente

Este erro pode ocorrer por vários motivos como - réplica criação falha na tarefa, problemas com o diário de alterações, erros de mapa de bits de filtro de nível de volume, a máquina de origem foi encerrado inesperadamente, capacidade excedida do registo de sincronização ou a réplica está inconsistente verdadeiramente. Siga estes passos para resolver este problema:
- Para remover o estado inconsistente, execute a verificação de consistência manualmente ou agendar diariamente a verificação de consistência.
- Certifique-se numa versão mais recente do servidor de MAB ou System Center DPM
- Certifique-se de que a verificação de consistência automática está ativada
- Experimente reiniciar os serviços de linha de comandos ("net stop dpmra" seguido de "net start dpmra")
- Certifique-se de que são cumpridos os requisitos de conectividade e largura de banda de rede
- Verifique se a máquina de origem foi encerrada inesperadamente
- Certifique-se de que o disco está em bom estado de funcionamento e não existe espaço suficiente para a réplica
- Certifique-se de que nenhuma tarefa de cópia de segurança duplicada estão em execução em simultâneo

## <a name="online-recovery-point-creation-failed"></a>Falha na criação do ponto de recuperação online

Siga estes passos para resolver este problema:
- Certifique-se numa versão mais recente do agente de cópia de segurança do Azure
- Tente criar manualmente um ponto de recuperação na área de tarefas de proteção
- Certifique-se de que executa a verificação de consistência na origem de dados
- Certifique-se de que são cumpridos os requisitos de conectividade e largura de banda de rede
- Dados de réplica estão num estado inconsistente. Criar um ponto de recuperação do disco desta origem de dados
- Certifique-se de que a réplica está presente e não em falta
- Réplica está a ter espaço suficiente para criar o diário USN

## <a name="unable-to-configure-protection"></a>Não é possível configurar a proteção

Este erro ocorre quando o servidor do DPM não consegue contactar o servidor protegido. Siga estes passos para resolver este problema:
- Certifique-se de que a versão mais recente do agente de cópia de segurança do Azure
- Certifique-se existe conectividade (rede/firewall/proxy) entre o servidor do DPM e o servidor protegido
- Se estiver a proteger o SQL Server, certifique-se de que NT AUTHORITY\SYSTEM tem sysadmin ativada a partir das propriedades de início de sessão

## <a name="this-server-is-not-registered-to-the-vault-specified-by-the-vault-credential"></a>Este servidor não está registado no cofre especificado pelas credenciais do Cofre

Este erro ocorre quando o ficheiro de credenciais do cofre selecionado não pertence ao Cofre de serviços de recuperação associado com o System Center DPM / servidor de cópia de segurança do Azure no qual a recuperação é tentada. Siga estes passos para resolver este problema:
- Transferir o ficheiro de credenciais do cofre dos serviços de recuperação cofre para que o System Center DPM / servidor de cópia de segurança do Azure está registado.
- Tente registar o servidor com o Cofre utilizando o ficheiro de credenciais do cofre transferidas mais recente.

## <a name="either-the-recoverable-data-is-not-available-or-the-selected-server-is-not-a-dpm-server"></a>Os dados recuperáveis não estão disponíveis, ou o servidor selecionado não é um servidor do DPM
Este erro ocorre quando existem não existem outros System Center DPM / cópia de segurança do Azure servidores registados no Cofre de serviços de recuperação, os servidores não carregou ainda os metadados ou o servidor selecionado não é um System Center DPM / servidor de cópia de segurança do Azure.
- Se existirem outros System Center DPM / servidores de cópia de segurança do Azure registado no Cofre de serviços de recuperação, certifique-se de que a cópia de segurança do Azure mais recente está instalado o agente.
- Se existirem outros System Center DPM / servidores de cópia de segurança do Azure registado no Cofre de serviços de recuperação, aguarde por um dia após a instalação para iniciar o processo de recuperação. A tarefa noturna carrega os metadados para todas as cópias de segurança protegidos na nuvem. Os dados estão disponíveis para recuperação.

## <a name="the-encryption-passphrase-provided-does-not-match-with-passphrase-associated-with-the-following-server"></a>O frase de acesso de encriptação fornecida não corresponde à frase de acesso associada ao seguinte servidor

> [!NOTE]
>Se esqueceu/perdido a frase de acesso de encriptação, não há nenhuma opção para recuperar os dados. A única opção consiste em voltar a gerar o frase de acesso e utilizar a encriptação de dados de cópia de segurança futuros.
>
>

Este erro ocorre quando o frase de acesso de encriptação utilizada no processo de encriptação de dados do System Center DPM / dados do servidor do Backup do Azure que está a ser recuperados não corresponde a frase de acesso de encriptação fornecida. O agente não é possível desencriptar os dados. Por conseguinte, a recuperação falhará. Siga estes passos para resolver este problema:
- Forneça a exato mesmo encriptação frase de acesso associada com o System Center DPM / servidor de cópia de segurança do Azure cujos dados está a ser recuperados. 
