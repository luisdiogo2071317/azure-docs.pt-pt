---
title: "Gerir o servidor de configuração para a recuperação de desastre do VMware com o Azure Site Recovery | Microsoft Docs"
description: "Este artigo descreve como gerir um servidor de configuração existente para a recuperação de desastre do VMware para o Azure com o Azure Site Recovery."
services: site-recovery
author: AnoopVasudavan
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: anoopkv
ms.openlocfilehash: ddb1c9426874634fea54f1b67509d4ac2af70eba
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="manage-the-configuration-server-for-vmware-vms"></a>Gerir o servidor de configuração para as VMs de VMware

Configurar um servidor de configuração no local ao utilizar [do Azure Site Recovery](site-recovery-overview.md) para recuperação após desastre de VMs de VMware e servidores físicos para o Azure. O servidor de configuração coordena as comunicações entre no local VMware e o Azure e gere a replicação de dados. Este artigo resume tarefas comuns para gerir o servidor de configuração após a respetiva implementação.


## <a name="modify-vmware-settings"></a>Modificar as definições de VMware

Modificar definições para o servidor VMware ao qual se liga o servidor de configuração.

1. Inicie sessão no computador que executa o servidor de configuração.
2. Inicie o Gestor de configuração de recuperação de Site do Azure de atalho para o ambiente de trabalho. Em alternativa, abra [esta ligação](https://configuration-server-name/IP:44315).
3. Selecione **gerir vCenter Server/vSPhere ESXi servidor**, e, em seguida, efetue o seguinte:

    * Para associar um servidor diferente do VMware com o servidor de configuração, selecione **adicionar vCenter Server/vSphere ESXi servidor**. Introduza os detalhes do servidor.

    * Para atualizar as credenciais utilizadas para ligar ao servidor do VMware para a deteção automática de VMs de VMware, selecione **editar**. Introduza novas credenciais e, em seguida, selecione **OK**.

    ![Modificar VMware](./media/vmware-azure-manage-configuration-server/modify-vmware-server.png)

## <a name="modify-credentials-for-mobility-service-installation"></a>Modificar as credenciais para a instalação do serviço de mobilidade

Modificar as credenciais utilizadas para instalar automaticamente o serviço de mobilidade nas VMs de VMware ativar para replicação.

1. Inicie sessão no computador que executa o servidor de configuração.
2. Inicie o Gestor de configuração de recuperação de Site a partir no atalho do ambiente de trabalho. Em alternativa, abra [esta ligação](https://configuration-server-name/IP:44315).
3. Selecione **gerem credenciais de máquina virtual**e introduza as credenciais de novo. Em seguida, selecione **OK** para atualizar as definições.

    ![Modificar as credenciais do serviço de mobilidade](./media/vmware-azure-manage-configuration-server/modify-mobility-credentials.png)

## <a name="modify-proxy-settings"></a>Modificar as definições de proxy

Modificar as definições de proxy utilizadas pela máquina do servidor de configuração para acesso à internet para o Azure. Se tiver uma máquina do servidor de processo além do servidor de processo predefinido em execução na máquina do servidor de configuração, modifique as definições em ambas as máquinas.

1. Inicie sessão no computador que executa o servidor de configuração.
2. Inicie o Gestor de configuração de recuperação de Site a partir no atalho do ambiente de trabalho. Em alternativa, abra [esta ligação](https://configuration-server-name/IP:44315).
3. Selecione **gerir conectividade**e atualize os valores de proxy. Em seguida, selecione **guardar** para atualizar as definições.

## <a name="add-a-network-adapter"></a>Adicionar um adaptador de rede

O modelo de abrir virtualização formato OVF () implementa o servidor de configuração de VM com um único adaptador de rede. Pode [adicionar um adaptador adicional para a VM)](vmware-azure-deploy-configuration-server.md#add-an-additional-adapter), mas tem de adicioná-lo antes de registar o servidor de configuração no cofre.

Para adicionar um adaptador depois de registar o servidor de configuração no cofre, adicione o adaptador nas propriedades da VM. Em seguida, voltar a registar o servidor no cofre.


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Voltar a registar um servidor de configuração no cofre do mesmo

Pode registar o servidor de configuração no cofre do mesmo se for necessário. Se tiver um computador de servidor de processos adicionais, além do servidor de processo predefinido em execução no servidor de configuração, voltar a registar ambas as máquinas.


  1. No cofre, abra **gerir** > **infraestrutura de recuperação de Site** > **servidores de configuração**.
  2. No **servidores**, selecione **transferir chave de registo** para transferir o ficheiro de credenciais do cofre.
  3. Inicie sessão máquina do servidor de configuração.
  4. No **%ProgramData%\ASR\home\svagent\bin**, abra **cspsconfigtool.exe**.
  5. No **registo de cofre** separador, selecione **procurar** e localize o ficheiro de credenciais do cofre que transferiu.
  6. Se for necessário, forneça detalhes do servidor proxy. Em seguida, selecione **Registar**.
  7. Abra uma janela de comando do PowerShell de administrador e execute o seguinte comando:

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

## <a name="upgrade-the-configuration-server"></a>Atualizar o servidor de configuração

Executar update rollups para atualizar o servidor de configuração. As atualizações podem ser aplicadas para até versões N-4. Por exemplo:

- Se executar 9.7, 9.8, 9.9 ou 9.10, pode atualizar diretamente para 9.11.
- Se executar o 9.6 ou anterior e pretende atualizar para 9.11, primeiro tem de atualizar para versão 9.7. antes de 9.11.

Ligações para os update rollups para atualizar para todas as versões do servidor de configuração estão disponíveis no [wiki atualizações página](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

Atualize o servidor da seguinte forma:

1. Transfira o ficheiro de instalador de atualização para o servidor de configuração.
2. Faça duplo clique para executar o instalador.
3. O instalador Deteta a versão atual em execução na máquina.
4. Selecione **OK** para confirmar e executar a atualização. 


## <a name="delete-or-unregister-a-configuration-server"></a>Elimine ou anular o registo de um servidor de configuração

1. Desativar [desative a proteção](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) para todas as VMs no servidor de configuração.
2. [Desassocie](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) e [eliminar](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) todas as políticas de replicação do servidor de configuração.
3. [Eliminar](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) todos os anfitriões de servidores/vSphere vCenter que estão associados com o servidor de configuração.
4. No cofre, abra **infraestrutura de recuperação de Site** > **servidores de configuração**.
5. Selecione o servidor de configuração que pretende remover. Em seguida, no **detalhes** página, selecione **eliminar**.

    ![Eliminar o servidor de configuração](./media/vmware-azure-manage-configuration-server/delete-configuration-server.png)
   

### <a name="delete-with-powershell"></a>Eliminar com o PowerShell

Opcionalmente, pode eliminar o servidor de configuração utilizando o PowerShell.

1. [Instalar](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) o módulo Azure PowerShell.
2. Inicie sessão sua conta do Azure utilizando este comando:
    
    `Login-AzureRmAccount`
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
> Pode utilizar o **-Force** opção em Remove-AzureRmSiteRecoveryFabric para eliminação forçada do servidor de configuração.
 


## <a name="renew-ssl-certificates"></a>Renovar os certificados SSL

O servidor de configuração tem um servidor web integradas, que orquestra atividades do serviço de mobilidade, servidores de processos e servidores de destino mestre ligados ao mesmo. O servidor web utiliza um certificado SSL para autenticar clientes. O certificado expira após três anos e pode ser renovado em qualquer altura.

### <a name="check-expiry"></a>Verificação de expiração

Para implementações de servidor de configuração antes de Maio de 2016, a expiração de certificado foi definida para um ano. Se tiver um certificado que está prestes a expirar, ocorre o seguinte:

- Quando a data de expiração é dois meses ou menos, inicia o serviço envio de notificações no portal e por correio eletrónico (se subscrever notificações de recuperação de Site).
- É apresentada uma faixa de notificação na página de recurso do cofre. Para obter mais informações, selecione de faixa.
- Se vir um **atualizar agora** botão, indica que alguns componentes no seu ambiente ainda não foram atualizados para 9.4.xxxx.x ou versões superiores. Atualize os componentes antes de renovar o certificado. Não é possível renovar em versões anteriores.

### <a name="renew-the-certificate"></a>Renovar o certificado

1. No cofre, abra **infraestrutura de recuperação de Site** > **servidor de configuração**. Selecione o servidor de configuração necessárias.
2. A data de expiração é apresentado em **estado de funcionamento do servidor de configuração**.
3. Selecione **renovar certificados**. 


## <a name="next-steps"></a>Passos Seguintes

Reveja os tutoriais para configurar a recuperação após desastre de [VMs de VMware](vmware-azure-tutorial.md) para o Azure.
