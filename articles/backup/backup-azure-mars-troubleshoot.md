---
title: Resolução de problemas do Azure Backup Agent
description: Resolver problemas de instalação e o registo do agente de cópia de segurança do Azure
services: backup
author: saurabhsensharma
manager: shreeshd
ms.service: backup
ms.topic: conceptual
ms.date: 12/4/2017
ms.author: saurse
ms.openlocfilehash: 89a39f6189367f91248b3868b1e1cb9f6abf0407
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36228395"
---
# <a name="troubleshoot-azure-backup-agent-configuration-and-registration-issues"></a>Resolver problemas de registo e de configuração do agente de cópia de segurança do Azure
## <a name="recommended-steps"></a>Passos recomendados
Consulte as ações recomendadas nas tabelas seguintes para resolver erros que poderá encontrar durante a configuração ou o registo do agente de cópia de segurança do Azure.

[!INCLUDE [backup-upgrade-mars-agent.md](../../includes/backup-upgrade-mars-agent.md)]

## <a name="invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Credenciais do cofre inválidas fornecidas. O ficheiro está danificado ou não ter as credenciais mais recentes associadas ao serviço de recuperação.
| Detalhes do erro | Causas possíveis | Ações recomendadas |
| ---     | ---     | ---    |
| **Erro** </br> *Credenciais do cofre inválidas fornecidas. O ficheiro está danificado ou não ter as credenciais mais recentes associadas ao serviço de recuperação. (ID: 34513)* | <ul><li> As credenciais do cofre são inválidas (ou seja, foram transferidas mais de 48 horas antes da data de registo).<li>O agente de cópia de segurança do Azure não é possível transferir um ficheiro temporário para a pasta temporária do Windows. <li>As credenciais do cofre estão numa localização de rede. <li>TLS 1.0 está desativada<li> Um servidor proxy configurado está a bloquear a ligação. <br> |  <ul><li>Transferir as novas credenciais do cofre.<li>Aceda a **opções da Internet** > **segurança** > **Internet**. Em seguida, selecione **Personalizar nível**e desloque-se até ver o ficheiro transferir secção. Em seguida, selecione **ativar**.<li>Também poderá ter de adicionar o site para [fidedigna sites](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Altere as definições para utilizar um servidor proxy. Em seguida, forneça o proxy de detalhes do servidor. <li> Corresponde a data e hora ao seu computador.<li>Aceda ao Windows/c/Temp e verificar se existem mais de 60 000 ou 65,000 ficheiros com a extensão de .tmp. Se existirem, elimine estes ficheiros.<li>Pode testar isto, executando o pacote SDP no servidor. Se obtiver um erro a indicar que as transferências de ficheiros não são permitidas, é muito provável que existem um grande número de ficheiros no diretório do Windows/c/Temp.<li>Certifique-se de que tem o .NET framework 4.6.2 instalado. <li>Se tiver desativado TLS 1.0 devido a conformidade de PCI, consulte este [resolução de problemas de página](https://support.microsoft.com/help/4022913). <li>Se tiver software antivírus instalado no servidor, exclua os seguintes ficheiros da análise de software antivírus: <ul><li>CBengine.exe<li>CSC.exe, que está relacionado com .NET Framework. Não há um CSC.exe para cada versão do .NET que está instalado no servidor. Exclua ficheiros CSC.exe que estão associados a todas as versões do .NET framework no servidor de afetados. <li>Localização de pasta ou a cache de rascunho. <br>*A localização predefinida para a pasta scratch ou o caminho de localização da cache é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>O agente de serviço de recuperação do Microsoft Azure não conseguiu ligar ao Microsoft Azure Backup

| Detalhes do erro | Causas possíveis | Ações recomendadas |
| ---     | ---     | ---    |
| **Erro** </br><ol><li>*O agente de serviço de recuperação do Microsoft Azure não conseguiu ligar ao Microsoft Azure Backup. (ID: 100050) Verifique as definições de rede e certifique-se de que consegue aceder à internet*<li>*Autenticação de proxy (407) necessária* |Bloquear a ligação de proxy. |  <ul><li>Vá para **opções da Internet** > **segurança** > **Internet**. Em seguida, selecione **Personalizar nível** e desloque-se até ver o ficheiro transferir secção. Selecione **Ativar**.<li>Também poderá ter de adicionar o site para [fidedigna sites](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Altere as definições para utilizar um servidor proxy. Em seguida, forneça o proxy de detalhes do servidor. <li>Se tiver software antivírus instalado no servidor, exclua os seguintes ficheiros da análise de software antivírus. <ul><li>CBEngine.exe (em vez de dpmra.exe).<li>CSC.exe (relacionados com o .NET Framework). Não há um CSC.exe para cada versão do .NET que está instalado no servidor. Exclua ficheiros CSC.exe que estão associados a todas as versões do .NET framework no servidor de afetados. <li>Localização de pasta ou a cache de rascunho. <br>*A localização predefinida para a pasta scratch ou o caminho de localização da cache é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.

## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Falha ao definir a chave de encriptação de cópias de segurança seguras

| Detalhes do erro | Causas possíveis | Ações recomendadas |
| ---     | ---     | ---    |      
| **Erro** </br>*Falha ao definir a chave de encriptação de cópias de segurança seguras ativação completamente não teve êxito, mas foi guardado o frase de acesso de encriptação para o seguinte ficheiro*. |<li>Servidor já está registado noutro cofre.<li>Durante a configuração, o frase de acesso estava danificado| Anular o registo do servidor do cofre e registe novamente com uma nova frase de acesso.

## <a name="the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>A ativação não foi concluída com êxito. A operação atual falhou devido a um erro interno do serviço [0x1FC07]

| Detalhes do erro | Causas possíveis | Ações recomendadas |
| ---     | ---     | ---    |          
| **Erro** </br><ol><li>*A ativação não foi concluída com êxito. A operação atual falhou devido a um erro interno do serviço [0x1FC07]. Repita a operação após algum tempo. Se o problema persistir, contacte o suporte da Microsoft*| <li> A pasta scratch se encontra num volume que não tem espaço suficiente. <li> A pasta scratch foi movida incorretamente para outra localização. <li> O ficheiro OnlineBackup.KEK está em falta. | <li>Atualizar para o [versão mais recente](http://aka.ms/azurebackup_agent) do agente MARS.<li>Mova a pasta scratch ou a localização da cache para um volume com espaço livre equivalente a 5-10% do tamanho total dos dados de cópia de segurança. Ao mover corretamente a localização da cache, consulte os passos descritos para [perguntas sobre o agente de cópia de segurança do Azure](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Certifique-se de que o ficheiro de OnlineBackup.KEK está presente. <br>*A localização predefinida para a pasta scratch ou o caminho de localização da cache é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.
  
## <a name="error-34506-the-encryption-passphrase-stored-on-this-computer-is-not-correctly-configured"></a>Erro 34506. O frase de acesso de encriptação armazenada neste computador não está corretamente configurada

| Detalhes do erro | Causas possíveis | Ações recomendadas |
| ---     | ---     | ---    |          
| **Erro** </br><ol><li>*Erro 34506. O frase de acesso de encriptação armazenada neste computador não está corretamente configurada*. | <li> A pasta scratch se encontra num volume que não tem espaço suficiente. <li> A pasta scratch foi movida incorretamente para outra localização. <li> O ficheiro OnlineBackup.KEK está em falta. | <li>Atualizar para o [versão mais recente](http://aka.ms/azurebackup_agent) do agente MARS.<li>Mova a pasta scratch ou a localização da cache para um volume com espaço livre equivalente a 5-10% do tamanho total dos dados de cópia de segurança. Ao mover corretamente a localização da cache, consulte os passos descritos para [perguntas sobre o agente de cópia de segurança do Azure](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Certifique-se de que o ficheiro de OnlineBackup.KEK está presente. <br>*A localização predefinida para a pasta scratch ou o caminho de localização da cache é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.  

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte
Se ainda precisar de ajuda, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para obter o seu problema resolvido rapidamente.

## <a name="next-steps"></a>Passos Seguintes
* Obter mais detalhes [como fazer cópias de segurança do Windows Server com o agente de cópia de segurança do Azure](tutorial-backup-windows-server-to-azure.md).
* Se precisar de restaurar uma cópia de segurança, utilize este artigo para [restaurar ficheiros para uma máquina Windows](backup-azure-restore-windows-server.md).
