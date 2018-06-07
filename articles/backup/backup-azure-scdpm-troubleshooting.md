---
title: Resolver problemas do System Center Data Protection Manager com o Backup do Azure
description: Resolva problemas do System Center Data Protection Manager.
services: backup
author: adigan
manager: shreeshd
ms.service: backup
ms.topic: conceptual
ms.date: 11/24/2017
ms.author: adigan
ms.openlocfilehash: d3776df8184523999433059e95bc72e1d3abb1c7
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34606448"
---
# <a name="troubleshoot-system-center-data-protection-manager"></a>Resolver Problemas do System Center Data Protection Manager

Este artigo descreve as soluções para problemas que poderá encontrar ao utilizar o Data Protection Manager.

Para notas de versão mais recentes para o System Center Data Protection Manager, consulte o [documentação do System Center](https://docs.microsoft.com/system-center/dpm/dpm-release-notes?view=sc-dpm-2016). Pode saber mais sobre o suporte para o Data Protection Manager no [nesta matriz](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-2016).


## <a name="error-replica-is-inconsistent"></a>Erro: A réplica está inconsistente

Uma réplica pode ser inconsistente pelos seguintes motivos:
- A tarefa de criação de réplica falha.
- Existem problemas com o diário de alterações.
- O mapa de bits do filtro de nível de volume contém erros.
- A máquina de origem foi encerrado inesperadamente.
- O registo de sincronização é excedida.
- A réplica está inconsistente verdadeiramente.

Para resolver este problema, execute as seguintes ações:
- Para remover o estado inconsistente, execute a verificação de consistência manualmente ou agendar uma verificação de consistência diária.
- Certifique-se de que está a utilizar a versão mais recente do servidor de cópia de segurança do Microsoft Azure e do Data Protection Manager.
- Certifique-se de que o **consistência automática** definição está ativada.
- Tente reiniciar os serviços de linha de comandos. Utilize o `net stop dpmra` comando seguido `net start dpmra`.
- Certifique-se de que está a cumprir os requisitos de conectividade e largura de banda de rede.
- Verifique se a máquina de origem foi encerrada inesperadamente.
- Certifique-se de que o disco está em bom estado e de que existe espaço suficiente para a réplica.
- Certifique-se de que existem não existem tarefas de cópia de segurança duplicadas que estejam a executar em simultâneo.

## <a name="error-online-recovery-point-creation-failed"></a>Erro: Falha de criação do ponto de recuperação Online

Para resolver este problema, execute as seguintes ações:
- Certifique-se de que está a utilizar a versão mais recente do agente do Backup do Azure.
- Experimente criar manualmente um ponto de recuperação na área de tarefas de proteção.
- Certifique-se de que execute uma verificação de consistência na origem de dados.
- Certifique-se de que está a cumprir os requisitos de conectividade e largura de banda de rede.
- Quando os dados de réplica estão num estado inconsistente, crie um ponto de recuperação do disco desta origem de dados.
- Certifique-se de que a réplica está presente e não em falta.
- Certifique-se de que a réplica tem espaço suficiente para criar o diário de número USN () de sequência de atualização.

## <a name="error-unable-to-configure-protection"></a>Erro: Não é possível configurar a proteção

Este erro ocorre quando o servidor do Data Protection Manager não consegue contactar o servidor protegido. 

Para resolver este problema, execute as seguintes ações:
- Certifique-se de que está a utilizar a versão mais recente do agente do Backup do Azure.
- Certifique-se de que existe conectividade (rede/firewall/proxy) entre o servidor do Data Protection Manager e o servidor protegido.
- Se estiver a proteger um SQL server, certifique-se de que o **propriedades de início de sessão** > **NT AUTHORITY\SYSTEM** propriedade mostra o **sysadmin** definição ativada.

## <a name="error-server-not-registered-as-specified-in-vault-credential-file"></a>Erro: Não registar o servidor conforme especificado no ficheiro de credenciais do Cofre

Este erro ocorre durante o processo de recuperação para dados do servidor de cópia de segurança do Data Protection Manager/Azure. O ficheiro de credenciais do cofre que é utilizado no processo de recuperação não pertence ao Cofre de serviços de recuperação para o servidor de cópia de segurança do Data Protection Manager/Azure.

Para resolver este problema, execute estes passos:
1. Transferir o ficheiro de credenciais do cofre a partir do Cofre de serviços de recuperação para o qual o servidor de cópia de segurança do Data Protection Manager/Azure está registado.
2. Tente registar o servidor com o cofre, utilizando o ficheiro de credenciais do cofre transferido mais recentemente.

## <a name="error-no-recoverable-data-or-selected-server-not-a-data-protection-manager-server"></a>Erro: Não existem dados recuperáveis ou servidor selecionado não um servidor do Data Protection Manager

Este erro ocorre pelos seguintes motivos:
- Não existem outros servidores de cópia de segurança do Data Protection Manager/Azure estão registados no Cofre de serviços de recuperação.
- Os servidores ainda ainda não adicionou os metadados.
- O servidor selecionado não é um servidor de cópia de segurança do Data Protection Manager/Azure.

Quando a outros servidores de cópia de segurança do Data Protection Manager/Azure estão registados no Cofre de serviços de recuperação, execute estes passos para resolver o problema:
1. Certifique-se de que o agente de cópia de segurança do Azure mais recente está instalado.
2. Depois de assegurar que o agente mais recente está instalado, aguarde um dia antes de começar o processo de recuperação. A cópia de segurança pela tarefa noturna carrega os metadados para todas as cópias de segurança protegidas para a nuvem. Os dados de cópia de segurança, em seguida, estão disponíveis para recuperação.

## <a name="error-provided-encryption-passphrase-doesnt-match-passphrase-for-server"></a>Erro: O frase de acesso de encriptação fornecida não corresponde à frase de acesso para o servidor

Este erro ocorre durante o processo de encriptação ao recuperar dados do servidor de cópia de segurança do Data Protection Manager/Azure. A frase de acesso de encriptação que é utilizado no processo de recuperação não corresponde à frase de acesso de encriptação do servidor. Como resultado, o agente não é possível desencriptar os dados e a recuperação falhará.

> [!IMPORTANT]
> Se se esquecer ou perder a frase de acesso de encriptação, não existem sem outros métodos para recuperar os dados. A única opção consiste em voltar a gerar o frase de acesso. Utilize a nova frase de acesso para encriptar os dados de cópia de segurança futuros.
>
> Quando estiver a recuperar dados, fornece sempre o frase de acesso de encriptação mesmo associado ao servidor de cópia de segurança do Data Protection Manager/Azure. 
>
