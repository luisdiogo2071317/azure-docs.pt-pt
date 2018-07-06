---
title: Gerir o servidor de configuração para a recuperação de desastre do VMware com o Azure Site Recovery | Documentos da Microsoft
description: Este artigo descreve como gerir um servidor de configuração existente para a recuperação de desastre do VMware no Azure com o Azure Site RecoveryS.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: raynew
ms.openlocfilehash: 29fa232e328ec0b16cb4e00fb16e3be458936cd7
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37857253"
---
# <a name="manage-the-configuration-server-for-vmware-vms"></a>Gerir o servidor de configuração para VMs de VMware

Configurar um servidor de configuração no local quando utiliza [do Azure Site Recovery](site-recovery-overview.md) para recuperação após desastre de VMs de VMware e servidores físicos para o Azure. O servidor de configuração coordena as comunicações entre VMware e Azure no local e gere a replicação de dados. Este artigo resume as tarefas comuns para gerir o servidor de configuração após a sua implementação.



## <a name="modify-vmware-settings"></a>Modificar as definições do VMware

Pode acessar o servidor de configuração da seguinte forma:
    - Iniciar sessão para a VM no qual está implementada e iniciar o Azure Site Recovery Configuration Manager a partir do atalho de desktop.
    - Em alternativa, pode acessar o servidor de configuração remotamente a partir de **https://*ConfigurationServerName*/:44315 /**. Inicie sessão com credenciais de administrador.
   
### <a name="modify-vmware-server-settings"></a>Modificar as definições do servidor VMware

1. Para associar um servidor diferente do VMware com o servidor de configuração, após o início de sessão, selecione **Adicionar servidor do vCenter Server/vSphere ESXi**.
2. Introduza os detalhes e, em seguida, selecione **OK**.


### <a name="modify-credentials-for-automatic-discovery"></a>Modificar as credenciais para a deteção automática

1. Para atualizar as credenciais utilizadas para ligar ao servidor do VMware para a deteção automática de VMs de VMware, após o início de sessão, selecione **editar**.
2. Introduza as credenciais de novo e, em seguida, selecione **OK**.

    ![Modificar o VMware](./media/vmware-azure-manage-configuration-server/modify-vmware-server.png)


## <a name="modify-credentials-for-mobility-service-installation"></a>Modificar as credenciais para a instalação do serviço de mobilidade

Modificar as credenciais utilizadas para instalar automaticamente o serviço de mobilidade nas VMs de VMware ativar para replicação.

1. Após o início de sessão, selecione **gerir as credenciais da máquina virtual**
2. Introduza as credenciais de novo e, em seguida, selecione **OK**.

    ![Modificar as credenciais do serviço de mobilidade](./media/vmware-azure-manage-configuration-server/modify-mobility-credentials.png)

## <a name="modify-proxy-settings"></a>Modificar as definições de proxy

Modifica as definições de proxy utilizadas pela máquina do servidor de configuração para acesso à internet para o Azure. Se tiver uma máquina do servidor de processo, além do servidor de processos padrão em execução na máquina do servidor de configuração, modifique as definições em ambas as máquinas.

1. Depois de iniciar sessão no servidor de configuração, selecione **gerir conectividade**.
2. Atualize os valores de proxy. Em seguida, selecione **guardar** para atualizar as definições.

## <a name="add-a-network-adapter"></a>Adicionar uma placa de rede

O modelo de formato OVF (Open Virtualization) implementa o servidor de configuração de VM com um único adaptador de rede.

- Pode [adicionar outro adaptador à VM](vmware-azure-deploy-configuration-server.md#add-an-additional-adapter), mas deve adicioná-lo antes de registar o servidor de configuração no cofre.
- Para adicionar um adaptador depois de registar o servidor de configuração no cofre, adicione o adaptador nas propriedades da VM. Em seguida, terá de voltar a registar o servidor no cofre.


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Voltar a registar um servidor de configuração no mesmo Cofre

Pode voltar a registar o servidor de configuração no mesmo cofre se for necessário. Se tiver uma máquina de servidor de processos adicionais, além do servidor de processos padrão em execução na máquina do servidor de configuração, voltar a registar ambas as máquinas.


  1. No cofre, abra **Manage** > **infraestrutura do Site Recovery** > **Configuration Servers**.
  2. Na **servidores**, selecione **chave de registo do Download** para transferir o ficheiro de credenciais do cofre.
  3. Inicie sessão na máquina do servidor de configuração.
  4. Na **%ProgramData%\ASR\home\svsystems\bin**, abra **cspsconfigtool.exe**.
  5. Sobre o **registo do cofre** separador, selecione **procurar**e localize o ficheiro de credenciais de cofre que transferiu.
  6. Se for necessário, forneça detalhes do servidor proxy. Em seguida, selecione **Registar**.
  7. Abra uma janela de comando do PowerShell de administrador e execute o seguinte comando:

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

## <a name="upgrade-the-configuration-server"></a>Atualizar o servidor de configuração

Execute update rollups para atualizar o servidor de configuração. As atualizações podem ser aplicadas para até versões N-4. Por exemplo:

- Se executar 9.7, 9.8, 9.9 ou 9.10, pode atualizar diretamente para 9.11.
- Se executar 9.6 ou anterior e pretende atualizar para o 9.11, primeiro tem de atualizar para versão 9.7. antes de 9.11.

Links para update rollups para o atualizar para todas as versões do servidor de configuração estão disponíveis no [página de atualizações de wiki](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

Atualize o servidor da seguinte forma:

1. No cofre, aceda ao **Manage** > **infraestrutura do Site Recovery** > **Configuration Servers**.
2. Se está disponível uma atualização, um link será exibido na **versão do agente** > coluna.
    ![Atualização](./media/vmware-azure-manage-configuration-server/update2.png)
3. Transfira o ficheiro de instalador de atualização para o servidor de configuração.

    ![Atualizar](./media/vmware-azure-manage-configuration-server/update1.png)

4. Clique duas vezes para executar o instalador.
5. O instalador Deteta a versão atual em execução na máquina. Clique em **Sim** para iniciar a atualização.
6. Quando a atualização for concluída valida a configuração do servidor.

    ![Atualizar](./media/vmware-azure-manage-configuration-server/update3.png)
    
7. Clique em **concluir** para fechar o instalador.

## <a name="delete-or-unregister-a-configuration-server"></a>Eliminar ou anular o registo de um servidor de configuração

1. [Desative a proteção](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) para todas as VMs sob o servidor de configuração.
2. [Desassociar](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) e [eliminar](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) todas as políticas de replicação do servidor de configuração.
3. [Eliminar](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) todos os anfitriões de vSphere/servidores vCenter que estão associados com o servidor de configuração.
4. No cofre, abra **infraestrutura do Site Recovery** > **servidores de configuração**.
5. Selecione o servidor de configuração que pretende remover. Em seguida, no **detalhes** página, selecione **eliminar**.

    ![Eliminar o servidor de configuração](./media/vmware-azure-manage-configuration-server/delete-configuration-server.png)
   

### <a name="delete-with-powershell"></a>Eliminar com o PowerShell

Opcionalmente, pode eliminar o servidor de configuração com o PowerShell.

1. [Instalar](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) o módulo Azure PowerShell.
2. Inicie sessão sua conta do Azure, utilizando este comando:
    
    `Connect-AzureRmAccount`
3. Selecione a subscrição do cofre.

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Defina o contexto do cofre.
    
    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Obter o servidor de configuração.

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Elimine o servidor de configuração.

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> Pode utilizar o **-força** opção em Remove-AzureRmSiteRecoveryFabric para eliminação forçada do servidor de configuração.
 
## <a name="generate-configuration-server-passphrase"></a>Gerar frase de acesso do servidor de configuração

1. Inicie sessão no seu servidor de configuração e, em seguida, abra uma janela de linha de comandos como administrador.
2. Para alterar o diretório para a pasta Reciclagem, execute o comando **cd %ProgramData%\ASR\home\svsystems\bin**
3. Para gerar o ficheiro da frase de acesso, execute **genpassphrase.exe - v > MobSvc.passphrase**.
4. A frase de acesso será armazenado no arquivo localizado em **%ProgramData%\ASR\home\svsystems\bin\MobSvc.passphrase**.

## <a name="renew-ssl-certificates"></a>Renovar certificados SSL

O servidor de configuração tem um servidor de web incorporadas, que coordena as atividades do serviço de mobilidade, servidores de processos e os servidores de destino mestre conectados a ele. O servidor web utiliza um certificado SSL para autenticar clientes. O certificado expira após três anos e pode ser renovado em qualquer altura.

### <a name="check-expiry"></a>Verificação de expiração

Para implementações de servidor de configuração antes de Maio de 2016, a expiração de certificado foi definida para um ano. Se tiver um certificado que está prestes a expirar, ocorre o seguinte:

- Quando a data de expiração é de dois meses ou menos, o serviço começa a enviar notificações no portal e por e-mail (se subscrever notificações de recuperação de Site).
- É apresentada uma faixa de notificação na página de recursos do cofre. Para obter mais informações, selecione a faixa.
- Se vir uma **atualizar agora** botão, ele indica que alguns componentes no seu ambiente ainda não foram atualizados para 9.4.xxxx.x ou versões superiores. Atualize os componentes antes de renovar o certificado. Não é possível renovar em versões anteriores.

### <a name="renew-the-certificate"></a>Renovar o certificado

1. No cofre, abra **infraestrutura do Site Recovery** > **servidor de configuração**. Selecione o servidor de configuração necessárias.
2. A data de expiração é apresentado em **estado de funcionamento do servidor de configuração**.
3. Selecione **renovar certificados**. 


## <a name="next-steps"></a>Passos Seguintes

Reveja os tutoriais para configurar a recuperação após desastre de [VMs de VMware](vmware-azure-tutorial.md) para o Azure.
