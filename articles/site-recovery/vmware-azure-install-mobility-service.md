---
title: Instalar o serviço de mobilidade de recuperação após desastre de VMs de VMware e servidores físicos para o Azure | Documentos da Microsoft
description: Saiba como instalar o agente do serviço de mobilidade para recuperação após desastre de VMs de VMware e servidores físicos para o Azure com o serviço Azure Site Recovery.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: ramamill
ms.openlocfilehash: 14be544c53bf3393466cfa33b2ad815f07d0005d
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51007421"
---
# <a name="install-the-mobility-service-for-disaster-recovery-of-vmware-vms-and-physical-servers"></a>Instalar o serviço de mobilidade de recuperação após desastre de VMs de VMware e servidores físicos

Quando define a recuperação após desastre para VMs de VMware e servidores físicos usando [do Azure Site Recovery](site-recovery-overview.md), instalar o [serviço de mobilidade de recuperação de Site](vmware-physical-mobility-service-overview.md) em cada VM de VMware no local e o servidor físico.  O serviço de mobilidade obtém dados, escreve na máquina e reencaminha-os para o servidor de processos de recuperação de sites.

## <a name="install-on-windows-machine"></a>Instalar no computador do Windows

Em cada máquina do Windows que pretende proteger, faça o seguinte:

1. Certifique-se de que existe conectividade de rede entre o computador e o servidor de processos. Se ainda não tiver configurado um servidor de processos separados, em seguida, por padrão ele é executado no servidor de configuração.
1. Crie uma conta que possa ser utilizada pelo servidor de processos para aceder ao computador. A conta deve ter direitos de administrador local ou domínio. Utilize esta conta apenas para a instalação de push e para as atualizações do agente.
2. Se não usar uma conta de domínio, desative o controlo de acesso de utilizador remoto no computador local da seguinte forma:
    - Na chave do Registro HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System, adicione um DWORD novo: **LocalAccountTokenFilterPolicy**. Defina o valor como **1**.
    -  Para fazer isso num prompt de comando, execute o seguinte comando:  
   "REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d
3. Na Firewall do Windows no computador que pretende proteger, selecione **permitir uma aplicação ou funcionalidade através da Firewall**. Ativar **de ficheiros e partilha de impressoras** e **Windows Management Instrumentation (WMI)**. Para computadores que pertençam a um domínio, pode configurar as definições de firewall utilizando um objeto de diretiva de grupo (GPO).

   ![Definições de firewall](./media/vmware-azure-install-mobility-service/mobility1.png)

4. Adicione a conta que criou no CSPSConfigtool. Para tal, inicie sessão no seu servidor de configuração.
5. Abra **cspsconfigtool.exe**. Está disponível como um atalho na área de trabalho e na pasta %programdata%\home\svsystems\bin.
6. Sobre o **gerir contas** separador, selecione **adicionar conta**.
7. Adicione a conta que criou.
8. Introduza as credenciais que utiliza para ativar a replicação para um computador.

## <a name="install-on-linux-machine"></a>Instalar na máquina do Linux

Em cada máquina do Linux que pretende proteger, efetue o seguinte:

1. Certifique-se de que existe conectividade de rede entre a máquina Linux e o servidor de processos.
2. Crie uma conta que possa ser utilizada pelo servidor de processos para aceder ao computador. A conta deve ser um utilizador **raiz** no servidor Linux de origem. Utilize esta conta apenas para a instalação de push e nas atualizações.
3. Verifique se o ficheiro /etc/hosts no servidor Linux de origem tem entradas que mapeiam o nome de anfitrião local para endereços IP associados a todos os adaptadores de rede.
4. Instale os pacotes openssh, openssh-server e openssl mais recentes no computador que quer replicar.
5. Certifique-se de que o Secure Shell (SSH) está ativado e em execução na porta 22.
4. Ative a autenticação de subsistema e palavra-passe SFTP no ficheiro sshd_config. Para tal, inicie sessão como **raiz**.
5. Na **/etc/ssh/sshd_config** ficheiro, encontre a linha que começa com **PasswordAuthentication**.
6. Anule os comentários da linha e altere o valor para **Sim**.
7. Encontre a linha que começa com **subsistema**, e anule os comentários da linha.

      ![Linux](./media/vmware-azure-install-mobility-service/mobility2.png)

8. Reinicie o serviço **sshd**.
9. Adicione a conta que criou no CSPSConfigtool. Para tal, inicie sessão no seu servidor de configuração.
10. Abra **cspsconfigtool.exe**. Está disponível como um atalho na área de trabalho e na pasta %programdata%\home\svsystems\bin.
11. Sobre o **gerir contas** separador, selecione **adicionar conta**.
12. Adicione a conta que criou.
13. Introduza as credenciais que utiliza para ativar a replicação para um computador.

## <a name="next-steps"></a>Passos Seguintes

Depois do serviço de mobilidade está instalado, no portal do Azure, selecione **+ replicar** para começar a proteger estas VMs. Saiba mais sobre como ativar a replicação para [VMs(vmware-azure-enable-replication.md) de VMware e [servidores físicos](physical-azure-disaster-recovery.md#enable-replication).


