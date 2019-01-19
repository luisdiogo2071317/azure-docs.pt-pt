---
title: Resolver problemas de falhas de instalação de push do serviço de mobilidade quando ativar a replicação para recuperação após desastre | Documentos da Microsoft
description: Resolver erros de instalação de serviços de mobilidade ao ativar a replicação para recuperação após desastre
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.author: ramamill
ms.date: 01/18/2019
ms.openlocfilehash: e397540d33df8a509e10f52fde41fc178cdba67e
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2019
ms.locfileid: "54411752"
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Resolver problemas de instalação de push do serviço de mobilidade

Instalação do serviço de mobilidade é uma etapa importante durante a ativar a replicação. O sucesso deste passo depende exclusivamente das pré-requisitos de reunião e trabalhar com configurações suportadas. Se as falhas mais comuns que enfrenta durante a instalação do serviço de mobilidade devido a:

* Erros de credencial/privilégio
* Falhas de início de sessão
* Erros de conectividade
* Sistemas operacionais sem suporte
* Falhas de instalação do VSS

Quando ativa a replicação, o Azure Site Recovery tenta enviar por push instalar agente do serviço de mobilidade na sua máquina virtual. Como parte desse processo, o servidor de configuração tenta se conectar com a máquina virtual e copie o agente. Para ativar a instalação com êxito, siga as orientações de resolução de problemas passo a passo, indicada abaixo.

## <a name="credentials-check-errorid-95107--95108"></a>Verificação de credenciais (ErrorID: 95107 & 95108)

* Verifique se a conta de utilizador selecionada durante a ativar a replicação está **válido e precisas**.
* O Azure Site Recovery requer **raiz** conta ou conta de utilizador com **privilégios de administrador** para efetuar a instalação de push. Caso contrário, a instalação push será bloqueada na máquina de origem.
  * Para Windows (**erro 95107**), verifique se a conta de utilizador tem acesso administrativo, local ou domínio, na máquina de origem.
  * Se não estiver a utilizar uma conta de domínio, terá de desativar o controlo de acesso de utilizador remoto no computador local.
    * Para desativar o controlo de acesso de utilizador remoto, na chave do Registro HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System, adicione um DWORD novo: LocalAccountTokenFilterPolicy. Defina o valor como 1. Para executar este passo, execute o seguinte comando da linha de comandos:

         `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
  * Para Linux (**erro 95108**), tem de escolher a conta de raiz para o êxito da instalação do agente de mobilidade. Além disso, os serviços SFTP devem ser executados. Para ativar a autenticação de subsistema e palavra-passe SFTP no ficheiro sshd_config:
    1. Inicie sessão como raiz.
    2. Aceda ao ficheiro /etc/ssh/sshd_config, encontre a linha que começa com PasswordAuthentication.
    3. Anule os comentários da linha e altere o valor como Sim.
    4. Encontre a linha que começa com o subsistema de e anule os comentários da linha.
    5. Reinicie o serviço de sshd.

Se pretender modificar as credenciais da conta de utilizador escolhido, siga as instruções dadas [aqui](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="insufficient-privileges-failure-errorid-95517"></a>Falha de privilégios insuficientes (ErrorID: 95517)

Quando o utilizador optou por instalar o agente de mobilidade não tem privilégios de administrador, o servidor de processos de servidor/aumentar horizontalmente de configuração não terá permissão para copiar o software do agente de mobilidade na máquina de origem. Portanto, este erro é um resultado de acesso negado falha. Certifique-se de que a conta de utilizador tem privilégios de administrador.

Se pretender modificar as credenciais da conta de utilizador escolhido, siga as instruções dadas [aqui](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="insufficient-privileges-failure-errorid-95518"></a>Falha de privilégios insuficientes (ErrorID: 95518)

Quando o estabelecimento de relação de confiança de domínio entre o domínio principal e a estação de trabalho falha ao tentar iniciar sessão no computador de origem, não consegue instalar o agente de mobilidade com o id de erro 95518. Por isso, certifique-se de que a conta de utilizador utilizada para instalar o agente de mobilidade tem privilégios administrativos para iniciar sessão através de domínio primário, a máquina de origem.

Se pretender modificar as credenciais da conta de utilizador escolhido, siga as instruções dadas [aqui](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="login-failure-errorid-95519"></a>Falha ao iniciar sessão (ErrorID: 95519)

A conta de utilizador selecionada durante a ativar a replicação foi desativada. Para ativar a conta de utilizador, consulte o artigo [aqui](https://aka.ms/enable_login_user) ou execute o seguinte comando, substituindo o texto *username* com o nome de utilizador reais.
`net user 'username' /active:yes`

## <a name="login-failure-errorid-95520"></a>Falha ao iniciar sessão (ErrorID: 95520)

Várias tentativas falhadas os esforços para acessar um computador irão bloquear a conta de utilizador. A falha pode ser devido a:

* Credenciais fornecidas durante a configuração de configuração estiverem incorretas ou
* A conta de utilizador selecionada durante a ativar a replicação está errada

Então, modificar as credenciais escolhidas ao seguir as instruções dadas [aqui](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation) e repita a operação após algum tempo.

## <a name="login-failure-errorid-95521"></a>Falha ao iniciar sessão (ErrorID: 95521)

Este erro ocorre quando os servidores de início de sessão não estão disponíveis na máquina de origem. Indisponibilidade dos servidores de início de sessão irá causar a falha do pedido de início de sessão e, portanto, não é possível instalar o agente de mobilidade. Para início de sessão com êxito, certifique-se de que os servidores de início de sessão estão disponíveis na máquina de origem e iniciar o serviço de início de sessão. Para obter instruções detalhadas, clique em [aqui](https://support.microsoft.com/en-in/help/139410/err-msg-there-are-currently-no-logon-servers-available).

## <a name="login-failure-errorid-95522"></a>Falha ao iniciar sessão (ErrorID: 95522)

O serviço de início de sessão não está em execução no seu computador de origem e causou a falha do pedido de início de sessão. Portanto, não é possível instalar o agente de mobilidade. Para resolver, certifique-se de que o serviço de início de sessão está em execução na máquina de origem para início de sessão com êxito. Para iniciar o serviço de início de sessão, execute o comando "net start início de sessão" prompt de comando ou iniciar o serviço de "NetLogon" do Gestor de tarefas.

## <a name="connectivity-failure-errorid-95117--97118"></a>**Falha de conectividade (ErrorID: 95117 & 97118)**

Servidor de configuração / servidor de processos de escalamento horizontal tenta ligar à origem de VM para instalar o agente de mobilidade. Este erro ocorre quando a máquina de origem não está acessível devido a problemas de conectividade de rede. Para resolver,

* Certifique-se de que é possível efetuar o ping sua máquina de origem do servidor de configuração. Se tiver optado por servidor de processos de escalamento horizontal durante a ativar a replicação, certifique-se de que pode efetuar o ping sua máquina de origem do servidor de processos.
  * Na linha de comando de máquina do servidor de origem, usar o Telnet para executar ping no servidor de configuração / aumentar horizontalmente o servidor de processos com a porta de https (135), conforme mostrado abaixo para ver se existem problemas de conectividade de rede ou problemas de bloqueio de porta de firewall.

     `telnet <CS/ scale-out PS IP address> <135>`
* Além disso, para **VM do Linux**,
  * Verifique se os pacotes openssh, openssh-server e openssl mais recentes estão instalados.
  * Verifique e certifique-se de que o Secure Shell (SSH) está ativado e está em execução na porta 22.
  * Serviços SFTP devem estar em execução. Para ativar a autenticação de subsistema e palavra-passe SFTP no ficheiro sshd_config,
    * Inicie sessão como raiz.
    * Aceda ao ficheiro /etc/ssh/sshd_config, encontre a linha que começa com PasswordAuthentication.
    * Anule os comentários da linha e altere o valor para Sim
    * Encontre a linha que começa com o subsistema de e anule os comentários da linha
    * Reinicie o serviço de sshd.
* Poderia ter falhado uma tentativa de ligação, se não existe nenhuma resposta adequada após um período de tempo ou ligação estabelecida falhou porque o anfitrião ligado não respondeu.
* Pode ser um conectividade de rede/domínio/problemas relacionados à. Também pode ser devido a nome DNS resolver o problema ou problema de esgotamento de porta TCP. Verifique se existem quaisquer tais problemas conhecidos no seu domínio.

## <a name="connectivity-failure-errorid-95523"></a>Falha de conectividade (ErrorID: 95523)

Este erro ocorre quando a rede no qual reside a máquina de origem não foi encontrada ou pode ter sido eliminada ou já não está disponível. É a única forma de resolver o erro, garantindo que a rede existe.

## <a name="file-and-printer-sharing-services-check-errorid-95105--95106"></a>Arquivo e impressora a partilharem a verificação de serviços (ErrorID: 95105 & 95106)

Após a verificação de conectividade, verifique se o arquivo e impressora a partilharem o serviço está ativado na sua máquina virtual. Estas definições são necessários para copiar o agente de mobilidade no computador de origem.

Para **windows 2008 R2 e versões anteriores**,

* Para ativar a partilha de impressão através da Firewall do Windows e de ficheiros
  * Painel de controlo Open -> sistema e segurança -> Firewall do Windows -> no painel esquerdo, clique em Avançadas definições -> clique em regras de entrada na árvore da consola.
  * Localize as regras de arquivo e impressora partilha (NB sessão-) e partilha de ficheiros e impressoras (SMB-In). Para cada regra, clique com o botão direito a regra e, em seguida, clique em **ativar regra**.
* Para ativar a partilha com diretiva de grupo de ficheiros
  * Volte a iniciar, digitar GPMC. msc e pesquisar.
  * No painel de navegação, abra as seguintes pastas: Política de computador local, configuração do usuário, modelos administrativos, componentes do Windows e a partilha de rede.
  * No painel de detalhes, faça duplo clique **impedir que os utilizadores a partilhar ficheiros no seu perfil**. Para desativar a definição de política de grupo e ativar a capacidade do utilizador partilhar ficheiros, clique em desativado. Clique em OK para guardar as alterações. Para obter mais informações, clique em [aqui](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754359(v=ws.10)).

Para **versões posteriores**, siga as instruções fornecidas [aqui](vmware-azure-install-mobility-service.md) para ativar a partilha de impressoras e ficheiros.

## <a name="windows-management-instrumentation-wmi-configuration-check-error-code-95103"></a>Verificação de configuração do Windows Management Instrumentation (WMI) (código de erro: 95103)

Depois de verificar os serviços de ficheiros e impressoras, ative o serviço WMI para perfis de privada, pública e domínio através da firewall. Estas definições são necessários para concluir a execução remota no computador de origem. Para ativar,

* Aceda ao painel de controlo, clique em segurança e, em seguida, clique em Firewall do Windows.
* Clique em alterar as definições e, em seguida, clique no separador exceções.
* Na janela de exceções, selecione a caixa de verificação para o Windows Management Instrumentation (WMI) para permitir tráfego WMI através da firewall. 

Também pode ativar o tráfego WMI através da firewall no prompt de comando. Utilize o seguinte comando `netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes`
Outros artigos de resolução de problemas do WMI foi possível encontrar os artigos seguintes.

* [Teste básico de WMI](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [Resolução de problemas de WMI](https://msdn.microsoft.com/library/aa394603(v=vs.85).aspx)
* [Resolução de problemas com scripts do WMI e serviços WMI](https://technet.microsoft.com/library/ff406382.aspx#H22)

## <a name="unsupported-operating-systems"></a>Sistemas operacionais sem suporte

Outro motivo mais comum de falha pode dever de sistema operativo não suportado. Certifique-se de que estiver a utilizar a versão de Kernel do sistema operativo suportada para instalação com êxito do serviço de mobilidade. Evite a utilização de patch privada.
Para ver a lista de sistemas operativos e versões de kernel suportadas pelo Azure Site Recovery, consulte a nossa [documento de matriz de suporte](vmware-physical-azure-support-matrix.md#replicated-machines).

## <a name="boot-and-system-partitions--volumes-are-not-the-same-disk-errorid-95309"></a>Partições de arranque e de sistema / volumes não são o mesmo disco (ErrorID: 95309)

Antes de 9.20 partições de versão, de arranque e de sistema / volumes em discos diferentes foi uma configuração não suportada. Partir [versão 9.20](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), esta configuração é suportada. Utilize a versão mais recente para esse suporte.

## <a name="boot-disk-not-found-errorid-95310"></a>Não foi encontrado um disco de inicialização (ErrorID: 95310)

Não é possível proteger uma máquina virtual sem um disco de arranque. Isso é para garantir uma recuperação sem problemas de máquina virtual durante a operação de ativação pós-falha. Ausência de disco de arranque resulta no caso de falha para arrancar o computador após a ativação pós-falha. Certifique-se de que a máquina virtual contém o disco de arranque e repita a operação. Além disso, tenha em atenção de que vários discos de arranque na mesma máquina não é suportado.

## <a name="multiple-boot-disks-found-errorid-95311"></a>Foram encontrados vários discos de arranque (ErrorID: 95311)

Uma máquina virtual com vários discos de arranque não é um [suportado configuração](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage).

## <a name="system-partition-on-multiple-disks-errorid-95313"></a>Partição do sistema em vários discos (ErrorID: 95313)

Antes da versão 9.20, partição de raiz ou colocado em vários discos de volume era uma configuração não suportada. Partir [versão 9.20](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), esta configuração é suportada. Utilize a versão mais recente para esse suporte.

## <a name="grub-uuid-failure-errorid-95320"></a>Falha de GRUB UUID (ErrorID: 95320)

Se o GRUB do computador de origem estiver a utilizar o nome de dispositivo em vez de UUID, não consegue instalar o agente de mobilidade. Contacte o administrador de sistema para fazer as alterações ao ficheiro GRUB.

## <a name="lvm-support-from-920-version"></a>Suporte da versão 9.20 LVM

Antes da versão 9.20, LVM era suportada para discos de dados apenas. /Boot deve estar numa partição de disco e não ser um volume LVM.

Partir [versão 9.20](https://support.microsoft.com/en-in/help/4478871/update-rollup-31-for-azure-site-recovery), [disco de SO na LVM](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) é suportada. Utilize a versão mais recente para esse suporte.

## <a name="insufficient-space-errorid-95524"></a>Espaço insuficiente (ErrorID: 95524)

Quando o agente de mobilidade é copiado no computador de origem, pelo menos 100 MB de espaço livre é necessário. Por isso, certifique-se de que sua máquina de origem é necessário espaço livre e repita a operação.

## <a name="vss-installation-failures"></a>Falhas de instalação do VSS

A instalação do VSS é uma parte da instalação do agente de mobilidade. Este serviço é utilizado no processo de pontos de recuperação consistente com a geração do aplicativo. As falhas durante a instalação de VSS podem ocorrer devido a vários motivos. Para identificar os erros exatos, consulte **c:\ProgramData\ASRSetupLogs\ASRUnifiedAgentInstaller.log**. Alguns erros comuns e os passos de resolução estão realçados na secção seguinte.

### <a name="vss-error--2147023170-0x800706be---exit-code-511"></a>Erro VSS-2147023170 [0x800706BE] - código de saída 511

Este problema é principalmente utilizado quando uma software antivírus está a bloquear as operações de serviços do Azure Site Recovery. Para resolver isso,

1. Excluir todas as pastas mencionadas [aqui](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program).
2. Siga as diretrizes publicadas pelo seu fornecedor de software antivírus para desbloquear o registo do DLL no Windows.

### <a name="vss-error-7-0x7---exit-code-511"></a>Erro VSS 7 [0x7] - código de saída 511

Este é um erro de tempo de execução e é causado devido a memória insuficiente para instalar o VSS. Certifique-se para aumentar o espaço em disco para a conclusão com êxito desta operação.

### <a name="vss-error--2147023824-0x80070430---exit-code-517"></a>Erro VSS-2147023824 [0x80070430] - código de saída 517

Este erro ocorre quando o serviço Azure Site Recovery VSS Provider está [marcado para eliminação](https://msdn.microsoft.com/en-us/library/ms838153.aspx). Tente instalar manualmente VSS na máquina de origem ao executar a seguinte linha de comando

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

### <a name="vss-error--2147023841-0x8007041f---exit-code-512"></a>Erro VSS-2147023841 [0x8007041F] - código de saída 512

Este erro ocorre quando a base de dados do Azure Site Recovery VSS Provider service está [bloqueado](https://msdn.microsoft.com/en-us/library/ms833798.aspx). Tente instalar manualmente VSS na máquina de origem ao executar a seguinte linha de comando

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

### <a name="vss-exit-code-806"></a>O código 806 de saída do VSS

Este erro ocorre quando a conta de utilizador utilizada para a instalação não tem permissões para executar o comando CSScript. Fornece as permissões necessárias para a conta de utilizador para executar o script e repita a operação.

### <a name="other-vss-errors"></a>Outros erros VSS

Tente instalar o serviço do fornecedor VSS manualmente na máquina de origem ao executar a seguinte linha de comando

`C:\Program Files (x86)\Microsoft Azure Site Recovery\agent>"C:\Program Files (x86)\Microsoft Azure Site Recovery\agent\InMageVSSProvider_Install.cmd"`

## <a name="next-steps"></a>Passos Seguintes

[Saiba como](vmware-azure-tutorial.md) para configurar a recuperação após desastre para VMs de VMware.
