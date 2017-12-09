---
title: "Resolver problemas do agente de cópia de segurança do Azure (MARS Agent) | Microsoft Docs"
description: "Resolver problemas de instalação e registo do agente do Backup do Azure"
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shreeshd
editor: 
ms.assetid: 778c6ccf-3e57-4103-a022-367cc60c411a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/4/2017
ms.author: saurse;markgal;
ms.openlocfilehash: d05b951277515f3100aefcfb06a17b661267cb37
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="troubleshooting-azure-backup-agent-configurationregistration-issues"></a>Resolução de problemas de configuração/registo do agente de cópia de segurança do Azure
## <a name="recommended-steps"></a>Passos recomendados
Consulte as ações recomendadas especificadas abaixo para resolver erros correspondentes vistos durante a configuração ou o registo do agente do Backup do Azure.

## <a name="error-invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Erro: Inválido fornecidas credenciais do cofre. O ficheiro está danificado ou não ter as credenciais mais recentes associadas ao serviço de recuperação.
| Detalhes do erro | Causas possíveis | Ações recomendadas |
| ---     | ---     | ---    |
| **Erro** </br> *Credenciais do cofre inválidas fornecidas. O ficheiro está danificado ou não ter as credenciais mais recentes associadas ao serviço de recuperação. (ID: 34513)* | <ol><li> As credenciais do Cofre não são válidas (ou seja. Estas foram transferidas mais de 48 horas antes da data de registo).<li>   O agente do Backup do Azure não é possível transferir um ficheiro temporário para a pasta temporária do Windows. <li>As credenciais do cofre estão numa localização de rede. <li>TLS 1.0 está desativada<li> Um servidor proxy configurado está a bloquear a ligação <br> |  <ol><li>Transferir as novas credenciais do Cofre<li>Vá para opções da internet > Segurança > Internet > clique em Personalizar nível > desloque-se até verá secção de transferência de ficheiro e selecione ativar.<li>Também poderá ter de adicionar o site para [fidedigna sites](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Alterar as definições para utilizar o proxy e atribua os detalhes do proxy <li> Corresponder a data e hora (UTC) com o seu computador<li>Aceda ao Windows/c/Temp e verificar se existem ficheiros de mais de 60 000 ou 65,000 (com a extensão de .tmp) e eliminar estes ficheiros.<li>Pode testar isto, executando o pacote SDP no servidor. Se receber o erro transferências de ficheiros que não são permitidas, podem ser razoavelmente certeza sobre grande número de ficheiros no diretório do Windows/c/Temp.<li>Certifique-se de que tem o .NET framework 4.6.2 instalado <li>Se tiver desativado TLS 1.0 devido a conformidade de PCI, consulte este [resolver problemas de ligação](https://support.microsoft.com/help/4022913). <li>Se tiver um software antivírus instalado no servidor, exclua os seguintes ficheiros da análise de software antivírus. <ol><li>CBengine.exe<li>CSC.exe (relacionados com o .NET Framework. Não há um CSC.exe por versão de .NET instalado no servidor. Excluir todos os ficheiros de CSC.exe associados a todas as versões do .NET framework no servidor de afetados.) <li>Localização de pasta ou a cache de rascunho. <br>*A localização predefinida para a pasta scratch ou o caminho de localização da cache é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*

## <a name="error-the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>Erro: O agente de serviço de recuperação do Microsoft Azure não conseguiu ligar ao Microsoft Azure Backup

| Detalhes do erro | Causas possíveis | Ações recomendadas |
| ---     | ---     | ---    |
| **Erro** </br><ol><li>*O agente de serviço de recuperação do Microsoft Azure não conseguiu ligar ao Microsoft Azure Backup. (ID: 100050) Verifique as definições de rede e certifique-se de que consegue aceder à internet*<li>*Autenticação de proxy (407) necessária* |Proxy de bloquear a ligação * |  <ol><li>Vá para opções da internet > Segurança > Internet > clique em Personalizar nível > desloque-se até verá secção de transferência de ficheiro e selecione ativar.<li>Também poderá ter de adicionar o site para [fidedigna sites](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Alterar as definições para utilizar o proxy e atribua os detalhes do proxy <li>Se tiver um software antivírus instalado no servidor, exclua os seguintes ficheiros da análise de software antivírus. <ol><li>CBengine.exe<li>(em vez de dpmra.exe)<li>CSC.exe (relacionados com o .NET Framework. Não há um CSC.exe por versão de .NET instalado no servidor. Excluir todos os ficheiros de CSC.exe associados a todas as versões do .NET framework no servidor de afetados.) <li>Rascunho de localização de pasta ou cache <br>*A localização predefinida para a pasta scratch ou o caminho de localização da cache é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*

## <a name="error-failed-to-set-the-encryption-key-for-secure-backups"></a>Erro: Falha ao definir a chave de encriptação de cópias de segurança seguras

| Detalhes do erro | Causas possíveis | Ações recomendadas |
| ---     | ---     | ---    |      
| **Erro** </br>*Falha ao definir a chave de encriptação de cópias de segurança seguras. A operação atual falhou devido a um erro interno do serviço erro' inválido entrada'. Repita a operação após algum tempo. Se o problema persistir, contacte o suporte da Microsoft* |Servidor já está registado noutro Cofre| Anular o registo do servidor do cofre e registar novamente.

## <a name="error-the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>Erro: A ativação não foi concluída com êxito. A operação atual falhou devido a um erro interno do serviço [0x1FC07]

| Detalhes do erro | Causas possíveis | Ações recomendadas |
| ---     | ---     | ---    |          
| **Erro** </br><ol><li>*A ativação não foi concluída com êxito. A operação atual falhou devido a um erro interno do serviço [0x1FC07]. Repita a operação após algum tempo. Se o problema persistir, contacte o suporte da Microsoft* <li>*Erro 34506. O frase de acesso de encriptação armazenada neste computador não está corretamente configurada* | <li> Pasta scratch se encontra num volume que não tem espaço suficiente <li> Pasta scratch é movida incorretamente para outra localização <li> Ficheiro OnlineBackup.KEK está em falta | <li>Mova a pasta scratch ou a localização da cache para um volume com espaço livre equivalente a 5-10% do tamanho total dos dados de cópia de segurança. Consulte os passos mencionados [neste artigo](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup) ao mover corretamente a localização da cache.<li> Certifique-se de que o ficheiro de OnlineBackup.KEK está presente <br>*A localização predefinida para a pasta scratch ou o caminho de localização da cache é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte
Se ainda precisar de ajuda, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para obter o seu problema resolvido rapidamente.

## <a name="next-steps"></a>Passos seguintes
* Obter mais detalhes [fazer uma cópia de segurança do servidor do Windows com o agente de cópia de segurança do Azure](tutorial-backup-windows-server-to-azure.md).
* Se precisar de restaurar uma cópia de segurança, utilize este artigo para [restaurar ficheiros para uma máquina Windows](backup-azure-restore-windows-server.md).
