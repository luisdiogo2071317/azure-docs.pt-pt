---
title: Resolver erros de cópia de segurança com a máquina virtual do Azure
description: Resolver problemas de cópia de segurança e restauro de máquinas virtuais do Azure
services: backup
author: trinadhk
manager: shreeshd
ms.service: backup
ms.topic: conceptual
ms.date: 01/21/2018
ms.author: trinadhk
ms.openlocfilehash: a5828b4e4f42c349246845bd003e874fb0352bae
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008081"
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Resolver problemas das cópias de segurança de máquina virtuais do Azure
Pode solucionar erros encontrados ao utilizar o Azure Backup com informações listadas na tabela abaixo.

| Detalhes do erro | Solução |
| --- | --- |
| Não foi possível executar a operação visto que a VM já não existe. -Pare a proteção de máquina virtual sem eliminar dados de cópia de segurança. Obter mais detalhes em http://go.microsoft.com/fwlink/?LinkId=808124 |Isto acontece quando a VM principal é eliminada, mas a política de cópia de segurança continua à procura de uma VM para criar cópias de segurança. Para corrigir este erro: <ol><li> Recriar a máquina virtual com o mesmo nome e o mesmo nome de grupo de recursos [nome do serviço cloud]<br>(OR)</li><li> Pare a proteção de máquina virtual com ou sem eliminar os dados de cópia de segurança. [Obter mais detalhes](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| A operação de instantâneo falhou devido a nenhuma conectividade de rede na máquina virtual - Certifique-se de que a VM tem acesso à rede. Para o instantâneo tenha êxito, o IP de datacenter do Azure da lista branca intervalos ou configurar um servidor proxy para acesso à rede. Para obter mais detalhes, consulte http://go.microsoft.com/fwlink/?LinkId=800034. Se já estiver a utilizar o servidor proxy, certifique-se de que as definições do servidor proxy estão configuradas corretamente | Este erro é apresentado quando negar a conectividade de internet de saída na máquina virtual. Conectividade com a Internet é necessária para a extensão de instantâneo VM tirar um instantâneo de discos subjacentes da máquina virtual. [Saiba mais](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine) sobre como corrigir falhas de instantâneo devido a acesso à rede bloqueado. |
| Agente da VM não consegue comunicar com o serviço de cópia de segurança do Azure. – Certifique-se a VM tem conectividade de rede e o agente da VM está em execução e mais recentes. Para obter mais informações, consulte  http://go.microsoft.com/fwlink/?LinkId=800034 |Este erro é apresentado se existe um problema com o agente da VM ou o acesso de rede para a infraestrutura do Azure é bloqueado de alguma forma. [Saiba mais](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#vm-agent-unable-to-communicate-with-azure-backup) sobre a depuração de segurança de VM de problemas de instantâneos.<br> Se o agente da VM não está a causar problemas, em seguida, reinicie a VM. Às vezes um Estado incorreto da VM pode causar problemas e reiniciar a VM repõe esta "Estado incorreto". |
| VM está num Estado de aprovisionamento falhado –. Reinicie a VM e certifique-se de que a VM está no estado de execução ou encerramento para cópia de segurança | Isto ocorre quando uma das falhas de extensões de oportunidades potenciais do Estado da VM para estar no Estado com falhas de aprovisionamento. Ir para a lista de extensões e ver se existe uma extensão com falha, removê-lo e tente reiniciar a máquina virtual. Se todas as extensões estão no estado de execução, verifique se o serviço de agente VM está em execução. Caso contrário, reinicie o serviço de agente VM. | 
| A operação de extensão de VMSnapshot falhou para o managed disks – tente novamente a operação de cópia de segurança. Se o problema, siga as instruções em "http://go.microsoft.com/fwlink/?LinkId=800034'. Se continuar, contacte o suporte da Microsoft | Este erro quando ocorre uma falha do serviço de cópia de segurança do Azure acionar um instantâneo. [Saiba mais](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#vmsnapshot-extension-operation-failed) sobre a VM de depuração de problemas de instantâneos. |
| Pode copiar o instantâneo da máquina virtual, devido a insuficiente espaço livre na conta de armazenamento - Certifique-se de que a conta de armazenamento tem espaço livre equivalente aos dados presentes nos discos de armazenamento premium ligados à máquina virtual | Em caso de VMs do premium na pilha de cópia de segurança de VM V1, podemos copiar o instantâneo para a conta de armazenamento. Isso é certificar-se de que o tráfego de gestão de cópia de segurança, que funciona em instantâneo, não limita o número de IOPS disponíveis para a aplicação utilizando os discos premium. A Microsoft recomenda que atribuir apenas 50% (17,5 TB) de espaço de conta de armazenamento total para que o serviço de cópia de segurança do Azure, pode copiar o instantâneo de conta e a transferência de dados do armazenamento a partir desta localização copiada na conta de armazenamento para o cofre. | 
| Não é possível executar a operação porque o agente da VM não está a responder |Este erro é apresentado se existe um problema com o agente da VM ou o acesso de rede para a infraestrutura do Azure é bloqueado de alguma forma. Para VMs do Windows, verifique o estado de serviço do agente VM nos serviços e se o agente é apresentado em programas no painel de controlo. Experimente remover o programa a partir do controlo de painel e volte a instalar o agente, conforme mencionado [abaixo](#vm-agent). Depois de reinstalar o agente, acione uma cópia de segurança ad hoc para verificar. |
| Falha na operação de extensão de serviços de recuperação. -Certifique-se de que o agente mais recente da máquina virtual está presente na máquina virtual e serviço de agente está em execução. Repita a operação de cópia de segurança e se falhar, contacte o suporte da Microsoft. |Este erro é apresentado quando o agente da VM está desatualizado. Consulte a secção "Atualizar o agente da VM" abaixo para atualizar o agente da VM. |
| Máquina virtual não existe. -Certifique-se de que a máquina virtual existe ou selecione uma máquina virtual diferente. |Isto acontece quando a VM principal é eliminada mas a política de cópia de segurança continua a ter um aspeto de uma VM efetuar a cópia de segurança. Para corrigir este erro: <ol><li> Recriar a máquina virtual com o mesmo nome e o mesmo nome de grupo de recursos [nome do serviço cloud]<br>(OR)<br></li><li>Pare a proteção da máquina virtual sem eliminar os dados de cópia de segurança. [Obter mais detalhes](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| Falha na execução de comando. -Outra operação está atualmente em curso neste item. Aguarde até que a operação anterior seja concluída e tente novamente |Uma cópia de segurança existente na VM está em execução e não é possível iniciar uma nova tarefa enquanto a tarefa existente está em execução. |
| Copiar VHDs a partir do Cofre de cópia de segurança excedeu o limite, tente novamente a operação dentro de alguns minutos. Se o problema persistir, contacte o Suporte da Microsoft. | Isto acontece se existir um erro transitório no lado de armazenamento ou se o serviço de cópia de segurança não está obtendo IOPS suficientes de conta de armazenamento que aloja a VM para poder transferir os dados dentro do período de tempo limite para o cofre. Certifique-se de que seguiu [melhores práticas](backup-azure-vms-introduction.md#best-practices) ao configurar a cópia de segurança. Tente mover a VM para um armazenamento diferente da conta que não é carregado e repita cópia de segurança.|
| Cópia de segurança falhou com um erro interno - repita a operação dentro de alguns minutos. Se o problema persistir, contacte Support da Microsoft |Pode obter este erro por motivos de 2: <ol><li> Existe um problema transitório ao aceder ao armazenamento de VM. Verifique se existem [estado do Azure](https://azure.microsoft.com/status/) para ver se há qualquer problema relacionado com a computação, armazenamento ou funcionamento em rede na região em curso. Em seguida, repita a tarefa de cópia de segurança assim que o problema estiver resolvido. <li>A VM original foi eliminada e, portanto, não é possível efetuar o ponto de recuperação. Para manter os dados de cópia de segurança para uma VM eliminada, mas remova os erros de cópia de segurança: desproteger a VM e escolha a opção para manter os dados. Esta ação para a tarefa de cópia de segurança agendada e as mensagens de erro recorrentes. |
| Falha ao instalar a extensão de serviços de recuperação do Azure no item selecionado - agente da VM é um pré-requisito para a extensão de serviços de recuperação do Azure. Instalar o agente de VM do Azure e reinicie a operação de registo |<ol> <li>Verifique se o agente da VM foi instalado corretamente. <li>Certifique-se de que o sinalizador sobre a configuração VM está definido corretamente.</ol> [Leia mais](#validating-vm-agent-installation) sobre como instalar o agente da VM e como validar a instalação do agente VM. |
| A instalação da extensão falhou com o erro "COM+ não conseguiu comunicar com o coordenador de transações do Microsoft Distributed |Isso normalmente significa que o serviço COM+ não está em execução. Contacte o suporte da Microsoft para obter ajuda a corrigir o problema. |
| Operação de instantâneo falhou com o erro de operação VSS "Esta unidade está bloqueada pela encriptação de unidade de disco BitLocker. Tem de desbloquear esta unidade do painel de controlo. |Desativa o BitLocker para todas as unidades na VM e verifique se o problema VSS foi resolvido |
| VM não está num Estado que permita cópias de segurança. |<ul><li>Verifique se o VM está num estado transitório entre em execução e encerrar para baixo. Se for, aguarde que o estado VM ser um deles e acionar novamente a cópia de segurança. <li> Se a VM é uma VM do Linux e o módulo de kernel usa [segurança avançada Linux], tem de excluir o caminho de agente do Linux (_/var/lib/waagent_) de segurança para tornar a extensão de cópia de segurança-se de que é instalada.  |
| Máquina Virtual do Azure não foi encontrado. |Isto acontece quando a VM principal é eliminada mas a política de cópia de segurança continua a ter um aspeto de uma VM efetuar cópia de segurança. Para corrigir este erro: <ol><li>Recriar a máquina virtual com o mesmo nome e o mesmo nome de grupo de recursos [nome do serviço cloud] <br>(OR) <li> Desative a proteção para esta VM para que as tarefas de cópia de segurança não serão criadas. </ol> |
| Agente da máquina virtual não está presente na máquina virtual - instale quaisquer pré-requisitos e o agente da VM e, em seguida, reinicie a operação. |[Leia mais](#vm-agent) sobre a instalação do agente VM e como validar a instalação do agente VM. |
| Operação de instantâneo falhou devido a escritores VSS num Estado incorreto |Terá de reiniciar os escritores VSS (cópia sombra de Volume de serviço) que estão num Estado incorreto. Para conseguir isso, a partir de uma linha de comandos elevada, execute _gravadores de lista vssadmin_. Saída contém todos os escritores VSS e o respetivo estado. Para cada cujo estado é não "[1] estável" um escritor VSS, reinicie o escritor VSS executando os seguintes comandos numa linha de comandos elevada:<br> _serviceName net stop_ <br> _serviceName net start_|
| Operação de instantâneo falhou devido a uma falha de análise da configuração |Isto acontece devido a permissões alteradas no diretório MachineKeys: _%systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br>Execute o comando abaixo e certifique-se de que as permissões no diretório de MachineKeys padrão-um:<br>_ICACLS %systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br><br> Permissões padrão são:<br>Everyone:(R,W) <br>BUILTIN\Administrators:(F)<br><br>Se vir as permissões no diretório de MachineKeys diferente da predefinição,. Siga os passos abaixo para as permissões corretas, eliminar o certificado e acionar a cópia de segurança.<ol><li>Corrigi permissões no diretório de MachineKeys.<br>Utilizar propriedades de segurança do Explorer e definições de segurança avançadas no diretório, repor os valores predefinidos as permissões, remover qualquer objeto de utilizador adicional (a predefinição) a partir do diretório e certifique-se de que a "todos" permissões que tinham acesso especial para:<br>-Listar pasta / Ler dados <br>-Leia atributos <br>-Leia atributos expandidos <br>-Criar ficheiros / escrever dados <br>-Criar pastas / anexar dados<br>-Atributos de escrita<br>-Escrever atributos estendidos<br>-Permissões de leitura<br><br><li>Eliminar todos os certificados com o campo "Emitido para" = "Windows Azure Management para extensões de serviço" ou "Windows Azure CRP gerador de certificados".<ul><li>[Abra a consola de certificados (computador Local)](https://msdn.microsoft.com/library/ms788967(v=vs.110).aspx)<li>Eliminar todos os certificados (em pessoal -> certificados) com o campo "Emitido para" = "Windows Azure Management para extensões de serviço" ou "Windows Azure CRP gerador de certificados".</ul><li>Cópia de segurança VM acionador. </ol>|
| O serviço de cópia de segurança do Azure não tem permissões suficientes para o Key Vault para cópia de segurança das máquinas virtuais encriptadas. |Serviço de cópia de segurança deve ser fornecido estas permissões no PowerShell com os passos mencionados na **ativar cópia de segurança** secção [documentação do PowerShell](backup-azure-vms-automation.md). |
|Instalação da extensão falhou com o erro do instantâneo - o COM+ não conseguiu comunicar com o coordenador de transações distribuídas da Microsoft | Volte a tentar iniciar o serviço do windows "sistema aplicação COM+" (num prompt de comando elevado - _net iniciar COMSysApp_). <br>Se falhar ao iniciar,. Siga os passos abaixo:<ol><li> Valide que a conta de início de sessão do serviço "Coordenador de transações distribuídas" é "Serviço de rede". Se não for,. Altere-o para "Serviço de rede", reinicie este serviço e, em seguida, tente iniciar o serviço "sistema aplicação COM+". "<li>Se continuar a falhar iniciar, desinstalar/instalar o serviço "Coordenador de transações distribuídas" pelos seguintes passos abaixo:<br> -Parar o serviço MSDTC<br> -Abra uma linha de comandos (cmd) <br> -Execute o comando "msdtc-desinstalar" <br> -Execute o comando "msdtc-instalar" <br> -Iniciar o serviço MSDTC<li>Inicie o serviço do windows "sistema aplicação COM+" e depois de ser iniciada, acionar a cópia de segurança do portal.</ol> |
|  Operação de instantâneo falhou devido ao erro COM+ | A ação recomendada consiste em reiniciar o serviço do windows "sistema aplicação COM+" (num prompt de comando elevado - _net iniciar COMSysApp_). Se o problema persistir, reinicie a VM. Se reiniciar a VM não ajudar, tente [removendo a extensão de VMSnapshot](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout#cause-3-the-backup-extension-fails-to-update-or-load) e acionar a cópia de segurança manualmente. |
| Falha ao fixar um ou mais pontos de montagem da VM para tirar um instantâneo consistente do sistema de ficheiros | Utilize os passos seguintes: <ol><li>Verificar o estado do sistema de ficheiros de todos os dispositivos montados usando _'tune2fs'_ comando.<br> Por exemplo,:, como tune2fs -l/desenvolvimento/sdb1 \| grep "Estado do sistema de ficheiros" <li>Desmonte os dispositivos para qual sistema de ficheiros Estado não está usando limpa _'umount'_ comando <li> Executar verificação de FileSystemConsistency nestes dispositivos usando _'fsck'_ comando <li> Os dispositivos de montagem novamente e repita a cópia de segurança.</ol> |
| A operação de instantâneo falhou devido a falha na criação de canal de comunicação de rede segura | <ol><Li> Abra o Editor de registo ao executar regedit.exe num modo elevado. <li> Identifique todas as versões do. NetFramework presente no sistema. Eles estão presentes na hierarquia de chave de registo "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft" <li> Para cada um. NetFramework presente na chave de registo, adicionar o seguinte chave: <br> "SchUseStrongCrypto"=dword:00000001 </ol>|
| A operação de instantâneo falhou devido a falha na instalação do Visual C++ Redistributable para Visual Studio 2012 | Navegue para C:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot\agentVersion e instalar vcredist2012_x64. Certifique-se de que o valor de chave de registo para permitir que esta instalação do serviço está definido como o valor correto ou seja, o valor da chave de registo _HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Msiserver_ está definido como 3 e 4 não. Se ainda está a enfrentar problemas com a instalação, reinicie o serviço de instalação, executando _MSIEXEC /UNREGISTER_ seguido _MSIEXEC /REGISTER_ num prompt de comando elevado.  |


## <a name="jobs"></a>Tarefas
| Detalhes do erro | Solução |
| --- | --- |
| O cancelamento não é suportado para este tipo de tarefa:. Aguarde até que a tarefa seja concluída. |Nenhuma |
| A tarefa não está num Estado cancelável -. Aguarde até que a tarefa seja concluída. <br>OU<br> A tarefa selecionada não está num Estado cancelável -. Aguarde a conclusão da tarefa. |Em todos os probabilidade, a tarefa estiver quase concluída. Aguarde até que a tarefa é concluída.|
| Não é possível cancelar a tarefa porque não está em curso - cancelamento só é suportado para as tarefas que estão em curso. Cancele da tentativa em curso tarefa. |Isto acontece devido a um estado transitório. Aguarde um minuto e repita a operação de cancelamento. |
| Falha ao cancelar a tarefa - Aguarde até a conclusão da tarefa. |Nenhuma |

## <a name="restore"></a>Restauro
| Detalhes do erro | Solução |
| --- | --- |
| Falha no restauro com um erro interno de Cloud |<ol><li>Serviço em nuvem para o qual está a tentar restaurar está configurado com as definições de DNS. Pode verificar <br>$deployment = Get-AzureDeployment -ServiceName "ServiceName" -Slot "Production"     Get-AzureDns -DnsSettings $deployment.DnsSettings<br>Se houver um endereço configurado, isso significa que as definições de DNS estão configuradas.<br> <li>Serviço em nuvem para o qual está a tentar restaurar está configurado com ReservedIP e VMs existentes no serviço em nuvem estão no estado parado.<br>Pode verificar que um serviço em nuvem reservou o IP com os seguintes cmdlets do powershell:<br>$deployment = Get-AzureDeployment -ServiceName "servicename" -Slot "Production" $dep.ReservedIPName <br><li>Está a tentar restaurar uma máquina virtual com as seguintes configurações de rede especial no mesmo serviço cloud. <br>-Máquinas virtuais em configuração de Balanceador de carga (interno e externo)<br>-Máquinas virtuais com vários IPs reservados<br>-Máquinas virtuais com vários NICs<br>Selecione um novo serviço cloud de interface do Usuário ou veja [restaurar considerações](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations) para VMs com configurações de rede especiais.</ol> |
| O nome DNS selecionado já está a ser utilizado - Especifique um nome DNS diferente e tente novamente. |O nome DNS aqui refere-se para o nome do serviço cloud (normalmente, terminando. cloudapp.net). Isso deve ser exclusivo. Se encontrar este erro, tem de escolher um nome VM diferente durante o restauro. <br><br> Este erro é mostrado apenas aos utilizadores do portal do Azure. A operação de restauro através do PowerShell será bem sucedida porque só restaura os discos e não criar a VM. O erro irá ser enfrentado quando a VM explicitamente é criada por si, depois da operação de restauro de disco. |
| A configuração de rede virtual especificada não está correta – especifique uma configuração de rede virtual diferente e tente novamente. |Nenhuma |
| O serviço cloud especificado está a utilizar um IP reservado que não coincide com a configuração da máquina virtual a ser restaurada – Especifique um serviço cloud diferente, que não é utilizar o IP reservado, ou escolha outro ponto de recuperação para restaurar a partir de. |Nenhuma |
| Serviço cloud atingiu o limite do número de pontos finais de entrada - repita a operação especificando um serviço cloud diferente ou através de um ponto final existente. |Nenhuma |
| Conta de armazenamento do cofre e o destino da cópia de segurança estão em duas regiões diferentes, certifique-se de que a conta de armazenamento especificada na operação de restauro está na mesma região do Azure que o Cofre de cópia de segurança. |Nenhuma |
| Conta de armazenamento especificada para a operação de restauro não é suportado - contas de armazenamento apenas básico/Standard com localmente redundante ou definições de replicação geográfica são suportadas. Selecione uma conta de armazenamento suportadas |Nenhuma |
| Tipo de conta de armazenamento especificada para a operação de restauro não está online - Certifique-se de que a conta de armazenamento especificada na operação de restauro está online |Isto pode acontecer devido a um erro transitório no armazenamento do Azure ou devido a uma falha. Escolha outra conta de armazenamento. |
| Foi atingida a Quota do grupo de recursos - elimine alguns grupos de recursos do portal do Azure ou contacte o suporte do Azure para aumentar os limites. |Nenhuma |
| A sub-rede selecionada não existe -. Selecione uma sub-rede existente |Nenhuma |
| O Serviço de Cópia de Segurança não tem autorização para aceder a recursos na sua subscrição. |Para resolver isso, primeiro restaurar discos com os passos mencionados na secção **restauro backup discos** na [configuração de restauro de VM escolher](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Depois disso, utilize os passos de PowerShell mencionados [criar uma VM a partir de discos restaurados](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) para criar VM completa a partir de discos restaurados. |

## <a name="backup-or-restore-taking-time"></a>Cópia de segurança ou restauro demorar tempo
Se vir seu backup(>12 hours) ou restauro tirar time(>6 hours):
* Compreender [fatores que contribuem para o tempo de cópia de segurança](backup-azure-vms-introduction.md#total-vm-backup-time) e [fatores que contribuem para restaurar o tempo](backup-azure-vms-introduction.md#total-restore-time).
* Certifique-se de que siga [melhores práticas de cópia de segurança](backup-azure-vms-introduction.md#best-practices). 

## <a name="vm-agent"></a>Agente da VM
### <a name="setting-up-the-vm-agent"></a>Como configurar o agente da VM
Normalmente, o agente da VM já está presente nas VMs que são criadas a partir da galeria do Azure. No entanto, as máquinas virtuais que são migradas dos datacenters no local não tem o agente da VM instalado. Para essas VMs, o agente de VM tem de ser instalada explicitamente.

Para VMs do Windows:

* Transfira e instale o [MSI do agente](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Precisa de privilégios de Administrador para concluir a instalação.
* Para máquinas de virtuais de clássico, [Atualize a propriedade VM](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) para indicar que o agente está instalado. Este passo não é necessário para as máquinas virtuais do Resource Manager.

Para VMs do Linux:

* Instale o mais recente do repositório de distribuição. Estamos **altamente recomendável** instalar agente apenas por meio de repositório de distribuição. Para obter detalhes sobre o nome do pacote, consulte [repositório de agente do Linux](https://github.com/Azure/WALinuxAgent) 
* Para VMs clássicas, [Atualize a propriedade VM](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) para indicar que o agente está instalado. Este passo não é necessário para as máquinas virtuais do Resource Manager.

### <a name="updating-the-vm-agent"></a>Atualizar o Agente da VM
Para VMs do Windows:

* A atualização do Agente da VM é tão simples como reinstalar os [binários do Agente da VM](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). No entanto, precisa garantir que nenhuma operação de cópia de segurança está em execução enquanto o agente da VM está a ser atualizado.

Para VMs do Linux:

* Siga as instruções [atualizar o agente de VM do Linux](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Estamos **altamente recomendável** agente a atualizar apenas por meio de repositório de distribuição. Não é recomendável baixar o código do agente a partir do github diretamente e atualizá-lo. Se o agente mais recente não está disponível para a sua distribuição, contacte o suporte de distribuição para obter instruções sobre como instalar o agente mais recente. Pode verificar mais recente [agente do Windows Azure Linux](https://github.com/Azure/WALinuxAgent/releases) informações no repositório do github.

### <a name="validating-vm-agent-installation"></a>A validar a instalação do agente de VM
Como verificar a versão do agente da VM em VMs do Windows:

1. Inicie sessão na máquina virtual do Azure e navegue para a pasta *C:\WindowsAzure\Packages*. Deve encontrar o ficheiro WaAppAgent.exe presente.
2. Clique com o botão direito do rato no ficheiro, aceda a **Propriedades** e, em seguida, selecione o separador **Detalhes**. O campo de versão do produto deve ser 2.6.1198.718 ou superior

## <a name="troubleshoot-vm-snapshot-issues"></a>Resolver problemas de instantâneos VM
Cópia de segurança VM depende de emissão de comandos de instantâneo para o armazenamento subjacente. Não ter acesso a armazenamento ou a atrasos numa execução de tarefa de instantâneo pode fazer com que a tarefa de cópia de segurança efetuar a ativação. Pode causar falha de tarefa de instantâneo.

1. Acesso à rede para o armazenamento está bloqueado com o NSG<br>
    Saiba mais sobre como [ativar o acesso de rede](backup-azure-arm-vms-prepare.md#establish-network-connectivity) para o armazenamento através de qualquer lista branca de IPs ou através de servidor proxy.
2. As VMs com cópia de segurança do Sql Server configurada podem fazer com que o atraso na tarefa de instantâneo <br>
   Por predefinição VM backup backup completo de VSS problemas em VMs do Windows. Em VMs que estejam a executar o Sql Servers e se a cópia de segurança do Sql Server está configurada, isto poderá provocar atraso na execução de instantâneo. . Defina após a chave de registo, se ocorrerem falhas de cópia de segurança devido a problemas de instantâneos.

   ```
   [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
   "USEVSSCOPYBACKUP"="TRUE"
   ```
3. Estado da VM informado incorretamente porque a VM é encerrada no RDP.  <br>
   Se o ter de encerrar a máquina virtual numa RDP, verifique novamente no portal do que o estado da VM é refletido corretamente. Caso contrário, tente encerrar a VM no portal usando a opção "Encerrar" no dashboard VM.
4. Se mais de quatro VMs partilharem o mesmo serviço cloud, configure várias políticas de cópia de segurança para preparar as cópia de segurança vezes por isso, não existem que cópias de segurança VM mais de quatro são iniciadas ao mesmo tempo. Tente se espalhar início vezes uma hora a distância entre as políticas de cópia de segurança.
5. VM está em execução na CPU/memória elevada.<br>
   Se a máquina virtual estiver em execução na CPU elevada usage(>90%) ou tarefa de instantâneo de memória, é colocado em fila, atrasada e, eventualmente, serão obtém o limite de tempo. Experimente a cópia de segurança a pedido em tais situações.

<br>

## <a name="networking"></a>Redes
Como todas as extensões, a extensão de cópia de segurança precisa de acesso à internet pública para trabalhar. Sem acesso à Internet pública pode se manifestar de várias formas:

* A instalação da extensão pode falhar
* As operações de cópia de segurança (como o instantâneo do disco) podem falhar
* Apresentar o estado da operação de cópia de segurança pode falhar

A necessidade de resolução de endereços públicos da internet tem sido articulada [aqui](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). Terá de verificar as configurações de DNS para a VNET e certifique-se de que os URIs de Azure pode ser resolvido.

Assim que a resolução de nomes é feita corretamente, acesso a IPs do Azure também tem de ser fornecido. Para desbloquear o acesso para a infraestrutura do Azure, siga um destes passos:

1. Intervalos de IP do datacenter do Azure da lista de permissões.
   * Obter a lista de [IPs de datacenter do Azure](https://www.microsoft.com/download/details.aspx?id=41653) de permissões.
   * Desbloquear a IPs utilizando a [New-NetRoute](https://docs.microsoft.com/powershell/module/nettcpip/new-netroute) cmdlet. Execute este cmdlet dentro da VM do Azure, numa janela elevada do PowerShell (executar como administrador).
   * Adicione regras ao NSG (se tiver um no local) para permitir o acesso para IPs.
2. Criar um caminho para o fluxo do tráfego HTTP
   * Se tiver alguma restrição de rede no local (um grupo, por exemplo) implementar um servidor de proxy HTTP para encaminhar o tráfego. Podem encontrar passos para implementar um servidor Proxy de HTTP [aqui](backup-azure-arm-vms-prepare.md#establish-network-connectivity).
   * Adicione regras ao NSG (se tiver um no local) para permitir o acesso à INTERNET do Proxy de HTTP.

> [!NOTE]
> DHCP tem de estar ativado no computador convidado para cópia de segurança de VM de IaaS trabalhar.  Se precisar de um IP privado estático, deve configurá-la através da plataforma. A opção de DHCP no interior da VM deve ser ativada à esquerda.
> Ver mais informações sobre [definir um IP estático de privada interna](../virtual-network/virtual-networks-reserved-private-ip.md).
>
>
