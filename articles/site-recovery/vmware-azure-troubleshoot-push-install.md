---
title: Resolver problemas de falhas de instalação de push do serviço de mobilidade quando ativar a replicação para recuperação após desastre | Documentos da Microsoft
description: Resolver erros de instalação de serviços de mobilidade ao ativar a replicação para recuperação após desastre
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.author: ramamill
ms.date: 10/29/2018
ms.openlocfilehash: c12683fee7022b84e097c2b71628776271611429
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50210997"
---
# <a name="troubleshoot-mobility-service-push-installation-issues"></a>Resolver problemas de instalação de push do serviço de mobilidade

Instalação do serviço de mobilidade é uma etapa importante durante a ativar a replicação. O sucesso deste passo depende exclusivamente das pré-requisitos de reunião e trabalhar com configurações suportadas. As falhas mais comuns que enfrenta durante a instalação do serviço de mobilidade estão devido a

* Erros de credencial/privilégio
* Erros de conectividade
* Sistemas operacionais sem suporte

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
  * Na linha de comando de máquina do servidor de origem, usar o Telnet para executar ping no servidor de configuração / aumentar horizontalmente servidor com a porta de https (predefinição 9443) de processos, conforme mostrado abaixo para ver se existem problemas de conectividade de rede ou problemas de bloqueio de porta de firewall.

     `telnet <CS/ scale-out PS IP address> <port>`

  * Se não conseguir ligar, permitir que o servidor de processos de porta de entrada 9443 no servidor de configuração / aumentar horizontalmente.
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

Para **versões posteriores**, siga as instruções fornecidas [aqui](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery) para ativar a partilha de impressoras e ficheiros

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

## <a name="next-steps"></a>Passos Seguintes

[Saiba como](vmware-azure-tutorial.md) para configurar a recuperação após desastre para VMs de VMware.