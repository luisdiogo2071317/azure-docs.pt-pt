---
title: Resolver problemas de falhas de instalação de push do serviço de mobilidade quando ativar a replicação para recuperação após desastre | Documentos da Microsoft
description: Resolver erros de instalação de serviços de mobilidade ao ativar a replicação para recuperação após desastre
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.author: ramamill
ms.date: 11/27/2018
ms.openlocfilehash: b3e2beb0245fa790dc60cf742d6ad8938de187f4
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52832586"
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Resolver problemas de instalação de push do serviço de mobilidade

Instalação do serviço de mobilidade é uma etapa importante durante a ativar a replicação. O sucesso deste passo depende exclusivamente das pré-requisitos de reunião e trabalhar com configurações suportadas. As falhas mais comuns que enfrenta durante a instalação do serviço de mobilidade estão devido a

* Erros de credencial/privilégio
* Erros de conectividade
* Sistemas operacionais sem suporte
* Falhas de instalação do VSS

Quando ativa a replicação, o Azure Site Recovery tenta enviar por push instalar agente do serviço de mobilidade na sua máquina virtual. Como parte desse processo, o servidor de configuração tenta se conectar com a máquina virtual e copie o agente. Para ativar a instalação com êxito, siga as orientações de resolução de problemas passo a passo, indicada abaixo.

## <a name="credentials-check-errorid-95107--95108"></a>Verificação de credenciais (ErrorID: 95107 & 95108)

* Verifique se a conta de utilizador selecionada durante a ativar a replicação está **válido e precisas**.
* O Azure Site Recovery requer **privilégio de administrador** para efetuar a instalação de push.
  * Para Windows, verifique se a conta de utilizador tem acesso administrativo, local ou domínio, na máquina de origem.
  * Se não estiver a utilizar uma conta de domínio, terá de desativar o controlo de acesso de utilizador remoto no computador local.
    * Para desativar o controlo de acesso de utilizador remoto, na chave do Registro HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System, adicione um DWORD novo: LocalAccountTokenFilterPolicy. Defina o valor como 1. Para executar este passo, execute o seguinte comando da linha de comandos:

         `REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`
  * Para o Linux, tem de escolher a conta de raiz para o êxito da instalação do agente de mobilidade.

Se pretender modificar as credenciais da conta de utilizador escolhido, siga as instruções dadas [aqui](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="connectivity-check-errorid-95117--97118"></a>**Verificação de conectividade (ErrorID: 95117 & 97118)**

* Certifique-se de que é possível efetuar o ping sua máquina de origem do servidor de configuração. Se tiver optado por servidor de processos de escalamento horizontal durante a ativar a replicação, certifique-se de que pode efetuar o ping sua máquina de origem do servidor de processos.
  * Na linha de comando de máquina do servidor de origem, usar o Telnet para executar ping no servidor de configuração / aumentar horizontalmente o servidor de processos com a porta de https (135), conforme mostrado abaixo para ver se existem problemas de conectividade de rede ou problemas de bloqueio de porta de firewall.

     `telnet <CS/ scale-out PS IP address> <135>`
  * Verificar o estado do serviço **InMage Scout VX Agent – Sentinel/Outpost**. Inicie o serviço, se não está em execução.
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

## <a name="file-and-printer-sharing-services-check-errorid-95105--95106"></a>Arquivo e impressora a partilharem a verificação de serviços (ErrorID: 95105 & 95106)

Após a verificação de conectividade, verifique se o arquivo e impressora a partilharem o serviço está ativado na sua máquina virtual.

Para **windows 2008 R2 e versões anteriores**,

* Para ativar a partilha de impressão através da Firewall do Windows e de ficheiros
  * Painel de controlo Open -> sistema e segurança -> Firewall do Windows -> no painel esquerdo, clique em Avançadas definições -> clique em regras de entrada na árvore da consola.
  * Localize as regras de arquivo e impressora partilha (NB sessão-) e partilha de ficheiros e impressoras (SMB-In). Para cada regra, clique com o botão direito a regra e, em seguida, clique em **ativar regra**.
* Para ativar a partilha com diretiva de grupo de ficheiros
  * Volte a iniciar, digitar GPMC. msc e pesquisar.
  * No painel de navegação, abra as seguintes pastas: política de computador Local, configuração do usuário, modelos administrativos, componentes do Windows e a partilha de rede.
  * No painel de detalhes, faça duplo clique **impedir que os utilizadores a partilhar ficheiros no seu perfil**. Para desativar a definição de política de grupo e ativar a capacidade do utilizador partilhar ficheiros, clique em desativado. Clique em OK para guardar as alterações. Para obter mais informações, clique em [aqui](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754359(v=ws.10)).

Para **versões posteriores**, siga as instruções fornecidas [aqui](vmware-azure-install-mobility-service.md) para ativar a partilha de impressoras e ficheiros.

## <a name="windows-management-instrumentation-wmi-configuration-check"></a>Verificação de configuração do Windows Management Instrumentation (WMI)

Depois de verificar os serviços de ficheiros e impressoras, ative o serviço WMI através da firewall.

* No painel de controlo, clique em segurança e, em seguida, clique em Firewall do Windows.
* Clique em alterar as definições e, em seguida, clique no separador exceções.
* Na janela de exceções, selecione a caixa de verificação para o Windows Management Instrumentation (WMI) para permitir tráfego WMI através da firewall. 

Também pode ativar o tráfego WMI através da firewall no prompt de comando. Utilize o seguinte comando `netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes`
Outros artigos de resolução de problemas do WMI foi possível encontrar os artigos seguintes.

* [Teste básico de WMI](https://blogs.technet.microsoft.com/askperf/2007/06/22/basic-wmi-testing/)
* [Resolução de problemas de WMI](https://msdn.microsoft.com/library/aa394603(v=vs.85).aspx)
* [Resolução de problemas com scripts do WMI e serviços WMI](https://technet.microsoft.com/library/ff406382.aspx#H22)

## <a name="unsupported-operating-systems"></a>Sistemas operacionais sem suporte

Outro motivo mais comum de falha pode dever de sistema operativo não suportado. Certifique-se de que estiver a utilizar a versão de Kernel do sistema operativo suportada para instalação com êxito do serviço de mobilidade.

Para saber mais sobre os sistemas operativos suportados pelo Azure Site Recovery, consulte a nossa [documento de matriz de suporte](vmware-physical-azure-support-matrix.md#replicated-machines).

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