---
title: Resolução de problemas do Azure Backup Agent
description: Resolver problemas de instalação e registo do agente de cópia de segurança do Azure
services: backup
author: saurabhsensharma
manager: shreeshd
ms.service: backup
ms.topic: conceptual
ms.date: 7/25/2018
ms.author: saurse
ms.openlocfilehash: 2c8978cfba8fc56d4dbc565cb3a91c75d9d54679
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43700200"
---
# <a name="troubleshoot-microsoft-azure-recovery-services-mars-agent-issues"></a>Resolver problemas do agente dos serviços de recuperação do Azure (MARS) da Microsoft
## <a name="recommended-steps"></a>Passos recomendados
Consulte este artigo para resolver erros durante a configuração, o registo, a cópia de segurança e restaurar com o agente MARS.

## <a name="invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Credenciais de cofre inválidas fornecidas. O ficheiro está danificado ou não ter as credenciais mais recentes associadas ao serviço de recuperação.
| Detalhes do erro | Causas possíveis | Ações recomendadas |
| ---     | ---     | ---    |
| **Erro** </br> *Foram fornecidas credenciais de cofre inválidas. O ficheiro está danificado ou não ter as credenciais mais recentes associadas ao serviço de recuperação. (ID: 34513)* | <ul><li> As credenciais do cofre são inválidas (ou seja, eles foram transferidos mais de 48 horas antes da hora de registo).<li>Agente de MARS não consegue transferir os ficheiros para o diretório Temp do Windows. <li>As credenciais do cofre estão numa localização de rede. <li>TLS 1.0 está desativado<li> Um servidor proxy configurado está a bloquear a ligação. <br> |  <ul><li>Transferir as credenciais do cofre novo.<li>Aceda a **opções da Internet** > **segurança** > **Internet**. Em seguida, selecione **nível personalizado**e desloque-se até ver o ficheiro transferir secção. Em seguida, selecione **ativar**.<li>Também poderá ter de adicionar o site para [sites fidedignos](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Altere as definições para utilizar um servidor proxy. Em seguida, forneça o proxy de detalhes do servidor. <li> Corresponde a data e hora com a sua máquina.<li>Vá para c: / Windows/Temp e verificar se existem mais de 60 000 ou 65.000 ficheiros com a extensão. tmp. Se existirem, elimine estes ficheiros.<li>Pode verificar isto em funcionamento o pacote SDP no servidor. Se obtiver um erro a indicar que as transferências de ficheiros não são permitidas, é provável que haja um grande número de ficheiros no diretório c: / Windows/Temp.<li>Certifique-se de que tem o .NET framework 4.6.2 instalado. <li>Se tiver desativado o TLS 1.0 devido a conformidade com PCI, consulte este [resolução de problemas de página](https://support.microsoft.com/help/4022913). <li>Se tiver software antivírus instalado no servidor, exclua os seguintes ficheiros da análise de software antivírus: <ul><li>CBengine.exe<li>CSC.exe, que está relacionado com o .NET Framework. Há um CSC.exe para todas as versões do .NET que está instalada no servidor. Exclua ficheiros de CSC.exe que estão associados a todas as versões do .NET framework no servidor afetado. <li>Localização de pasta ou cache de rascunho. <br>*A localização predefinida para a pasta de rascunho ou o caminho de localização de cache é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>O agente de serviço de recuperação do Microsoft Azure não conseguiu ligar ao Microsoft Azure Backup

| Detalhes do erro | Causas possíveis | Ações recomendadas |
| ---     | ---     | ---    |
| **Erro** </br><ol><li>*O agente de serviço de recuperação do Microsoft Azure não conseguiu ligar ao Microsoft Azure Backup. (ID: 100050) Verifique as definições de rede e certifique-se de que conseguir estabelecer ligação à internet*<li>*Autenticação do proxy (407) necessária* |Bloquear a ligação de proxy. |  <ul><li>Vá até **opções da Internet** > **segurança** > **Internet**. Em seguida, selecione **nível personalizado** e desloque-se até ver o ficheiro transferir secção. Selecione **Ativar**.<li>Também poderá ter de adicionar o site para [sites fidedignos](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Altere as definições para utilizar um servidor proxy. Em seguida, forneça o proxy de detalhes do servidor. <li>Se tiver software antivírus instalado no servidor, exclua os seguintes ficheiros da análise de software antivírus. <ul><li>CBEngine.exe (em vez de dpmra.exe).<li>CSC.exe (relacionados com o .NET Framework). Há um CSC.exe para todas as versões do .NET que está instalada no servidor. Exclua ficheiros de CSC.exe que estão associados a todas as versões do .NET framework no servidor afetado. <li>Localização de pasta ou cache de rascunho. <br>*A localização predefinida para a pasta de rascunho ou o caminho de localização de cache é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.

## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Falha ao definir a chave de encriptação de cópias de segurança seguras

| Detalhes do erro | Causas possíveis | Ações recomendadas |
| ---     | ---     | ---    |      
| **Erro** </br>*Falha ao definir a chave de encriptação para ativação de cópias de segurança seguras completamente não teve êxito, mas a frase de acesso de encriptação foi guardado o ficheiro seguinte*. |<li>Servidor já está registado com outro cofre.<li>Durante a configuração, a frase de acesso estava danificado| Anular o registo do servidor do cofre e registe novamente com uma nova frase de acesso.

## <a name="the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>A ativação não foi concluída com êxito. A operação atual falhou devido a um erro de serviço interno [0x1FC07]

| Detalhes do erro | Causas possíveis | Ações recomendadas |
|---------|---------|---------|
|**Erro** </br><ol>*A ativação não foi concluída com êxito. A operação atual falhou devido a um erro de serviço interno [0x1FC07]. Repita a operação após algum tempo. Se o problema persistir, contacte o suporte da Microsoft*     | <li> A pasta de rascunho se encontra num volume que não tem espaço suficiente. <li> A pasta de rascunho foi movida incorretamente para outra localização. <li> O ficheiro de OnlineBackup.KEK está em falta.         | <li>Atualizar para o [versão mais recente](http://aka.ms/azurebackup_agent) do agente MARS.<li>Mova a localização de pasta ou cache de rascunho para um volume com espaço livre igual a 5 a 10% do tamanho total dos dados de cópia de segurança. Ao mover corretamente a localização da cache, consulte os passos em [perguntas sobre o Azure Backup Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Certifique-se de que o ficheiro de OnlineBackup.KEK está presente. <br>*A localização predefinida para a pasta de rascunho ou o caminho de localização de cache é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.        |

## <a name="error-34506-the-encryption-passphrase-stored-on-this-computer-is-not-correctly-configured"></a>Erro 34506. A frase de acesso de encriptação armazenado neste computador não está configurada corretamente

| Detalhes do erro | Causas possíveis | Ações recomendadas |
|---------|---------|---------|
|**Erro** </br><ol>*Erro 34506. A frase de acesso de encriptação armazenado neste computador não está configurada corretamente*.    | <li> A pasta de rascunho se encontra num volume que não tem espaço suficiente. <li> A pasta de rascunho foi movida incorretamente para outra localização. <li> O ficheiro de OnlineBackup.KEK está em falta.        | <li>Atualizar para o [versão mais recente](http://aka.ms/azurebackup_agent) do agente MARS.<li>Mova a pasta de rascunho ou a localização da cache para um volume com espaço livre equivalente aos 5 a 10% do tamanho total dos dados de cópia de segurança. Ao mover corretamente a localização da cache, consulte os passos em [perguntas sobre o Azure Backup Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Certifique-se de que o ficheiro de OnlineBackup.KEK está presente. <br>*A localização predefinida para a pasta de rascunho ou o caminho de localização de cache é C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.         |

## <a name="backups-do-not-run-as-per-schedule"></a>As cópias de segurança não são executados de acordo com a agenda
Execute os seguintes passos quando cópias de segurança agendadas não obter acionadas automaticamente, enquanto as cópias de segurança manuais funcionam sem problemas. 
 
<li>Certifique-se de que o PowerShell 3.0 ou superior está instalado no servidor. Para verificar a versão do PowerShell, execute o seguinte comando e certifique-se de que o *principais* número de versão é igual ou superior a 3.

`$PSVersionTable.PSVersion`
<li>Certifique-se de que o caminho seguinte faz parte do *PSMODULEPATH* variável de ambiente.

`<MARS agent installation path>\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup`
<li>Se a diretiva de execução do powershell para o *LocalMachine* é definido para restrito, o cmdlet do powershell que aciona a tarefa de cópia de segurança poderão falhar. Execute os seguintes comandos no modo elevado para verificar e defina a política de execução para qualquer um *Unrestricted* ou *RemoteSigned*

`PS C:\WINDOWS\system32> Get-ExecutionPolicy -List`

`PS C:\WINDOWS\system32> Set-ExecutionPolicy Unrestricted`
<li>Vá para Painel de controlo -> Ferramentas administrativas -> agendador de tarefas -> expanda Microsoft -> selecione cópia de segurança Online
<li>Clique duas vezes a tarefa de 'Microsoft-OnlineBackup' e vá até a guia 'Acionadores'.
<li>Certifique-se de que o "Estado" da tarefa está definido como "Enabled". Caso contrário, clique em "Editar" e selecione a caixa de verificação 'Enabled'
<li>Navegue para o *opções de segurança* secção a *geral* separador
<li>Certifique-se de que a conta de utilizador selecionada para executar a tarefa está *sistema* ou grupo de administradores locais no servidor

> [!TIP]
> É recomendado reiniciar o servidor depois de efetuar os passos acima para garantir que as alterações feitas são aplicadas de forma consistente


## <a name="troubleshooting-restore-issues"></a>Resolução de problemas de restauro

### <a name="failure-to-mount-the-recovery-volume-during-recovery-of-files-and-folders"></a>Falha ao montar o Volume de recuperação durante a recuperação de ficheiros e pastas
Se a cópia de segurança do Azure não com êxito montar o volume de recuperação, mesmo após alguns minutos do clique em **montar** ou falha para montar o volume de recuperação com um ou mais erros, siga os passos abaixo para começar a recuperar normalmente.

1.  Cancele o processo de montagem em curso no caso de ele esteve em execução durante vários minutos.

2.  Certifique-se de que está na versão mais recente do agente do Backup do Azure. Para obter as informações de versão do agente de cópia de segurança do Azure, clique em **sobre o Microsoft Azure Recovery Services Agent** sobre o **ações** painel do Microsoft Azure Backup da consola e certifique-se de que o **Versão** número é igual ou superior à versão mencionada [neste artigo](https://go.microsoft.com/fwlink/?linkid=229525). Pode baixar a versão mais recente do [aqui](https://go.microsoft.com/fwLink/?LinkID=288905)

3.  Aceda a **Gestor de dispositivos** -> **controladores de armazenamento** e certifique-se de que consegue localizar **Iniciador do Microsoft iSCSI**. Se pode localizá-lo, vá diretamente para o passo 7 abaixo. 

4.  Se não conseguir localizar o serviço Iniciador do Microsoft iSCSI, conforme mencionado no passo 3, verifique se pode encontrar uma entrada sob **Gestor de dispositivos** -> **controladores de armazenamento** chamado  **Dispositivo desconhecido** com o ID de Hardware **ROOT\ISCSIPRT**.

5.  Clique com o botão direito do rato em **dispositivo desconhecido** e selecione **atualizar Driver**.

6.  Atualizar o driver, selecionando a opção para **pesquisar automaticamente software de driver atualizado**. Conclusão da atualização deve ser alterado **dispositivo desconhecido** ao **Iniciador do Microsoft iSCSI** conforme mostrado abaixo. 

    ![Encriptação](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Aceda a **Gerenciador de tarefas** -> **serviços (Local)** -> **serviço iniciador iSCSI da Microsoft**. 

    ![Encriptação](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)
    
8.  Reinicie o serviço Iniciador do iSCSI da Microsoft, clicando com o botão direito no serviço, clicar em Stop e ainda mais clicar novamente e clicar em **iniciar**.

9.  Repita a recuperação instantânea de restaurar a utilizar. 

Se a recuperação continuar a falhar, reinicie o servidor/cliente. Se não é desejável uma reinicialização ou a recuperação continuar a falhar, mesmo após o reinício do servidor, tente recuperar a partir de uma máquina alternativa ao seguir os passos listados na [neste artigo](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte
Se precisar de ajuda, ainda [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.

## <a name="next-steps"></a>Passos Seguintes
* Obter mais detalhes sobre [a efetuar cópias de segurança do Windows Server com o Azure Backup Agent](tutorial-backup-windows-server-to-azure.md).
* Se precisar de restaurar uma cópia de segurança, utilize este artigo para [restaurar ficheiros para uma máquina Windows](backup-azure-restore-windows-server.md).
