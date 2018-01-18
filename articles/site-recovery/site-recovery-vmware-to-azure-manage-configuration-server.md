---
title: " Gerir o servidor de configuração para a recuperação de desastre do VMware com o Azure Site Recovery | Microsoft Docs"
description: "Este artigo descreve como gerir um servidor de configuração existente para a recuperação de desastre do VMware para o Azure, com o serviço do Azure Site Recovery."
services: site-recovery
author: AnoopVasudavan
ms.service: site-recovery
ms.topic: article
ms.date: 01/15/2017
ms.author: anoopkv
ms.openlocfilehash: e9e4bfc86df2cae1facac62472c915d91fb8c84c
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="manage-the-configuration-server"></a>Gerir o servidor de configuração

Configurar um servidor de configuração no local ao utilizar o [do Azure Site Recovery](site-recovery-overview.md) serviço recuperação de desastres de VMs de VMware e servidores físicos para o Azure. O servidor de configuração coordena as comunicações entre no local VMware e o Azure e gere a replicação de dados. Este artigo resume tarefas comuns para gerir o servidor de configuração depois de ser implementado.

## <a name="modify-vmware-settings"></a>Modificar as definições de VMware

Modificar definições para o servidor VMware ao qual se liga o servidor de configuração.

1. Inicie sessão no computador que executa o servidor de configuração.
2. Inicie o Gestor de configuração de recuperação de Site do Azure no atalho do ambiente de trabalho. Em alternativa, abra **https://configuration-server-name/IP:44315**.
3. Clique em **gerir vCenter Server/vSPhere ESXi servidor**:
    - Para associar um servidor diferente do VMware com o servidor de configuração, clique em **adicionar vCenter Server/vSphere ESXi servidor**e especifique os detalhes do servidor.
    - Para atualizar as credenciais utilizadas para ligar ao servidor do VMware para a deteção automática de VMs de VMware, clique em **editar**. Especifique as novas credenciais e, em seguida, clique em **OK**.

        ![Modificar VMware](./media/site-recovery-vmware-to-azure-manage-configuration-server/modify-vmware-server.png)

## <a name="modify-credentials-for-mobility-service-installation"></a>Modificar as credenciais para a instalação do serviço de mobilidade

Modificar as credenciais utilizadas para instalar automaticamente o serviço de mobilidade em VMs de VMware ativar para replicação.

1. Inicie sessão no computador que executa o servidor de configuração.
2. Inicie o Gestor de configuração de recuperação de Site do Azure no atalho do ambiente de trabalho. Em alternativa, abra **https://configuration-server-name/IP:44315**.
3. Clique em **gerem credenciais de máquina virtual**e especifique as credenciais de novo. Em seguida, clique em **OK** para atualizar as definições.

    ![Modificar as credenciais do serviço de mobilidade](./media/site-recovery-vmware-to-azure-manage-configuration-server/modify-mobility-credentials.png)

## <a name="modify-proxy-settings"></a>Modificar as definições de proxy

Modificar as definições de proxy utilizadas pela máquina do servidor de configuração para acesso à internet para o Azure. Se tiver um computador de servidor de processos adicionais, além do servidor de processo predefinido em execução no servidor de configuração, modifique as definições em ambas as máquinas.

1. Inicie sessão no computador que executa o servidor de configuração.
2. Inicie o Gestor de configuração de recuperação de Site do Azure no atalho do ambiente de trabalho. Em alternativa, abra **https://configuration-server-name/IP:44315**.
3. Clique em **gerir conectividade**e atualize os valores de proxy. Em seguida, clique em **guardar** para atualizar as definições.

## <a name="add-a-network-adapter"></a>Adicionar um adaptador de rede

O modelo OVF implementa o servidor de configuração de VM com um único adaptador de rede. Pode [adicionar um adaptador adicional para a VM)](how-to-deploy-configuration-server.md#add-an-additional-adapter), mas terá de fazê-lo antes de registar o servidor de configuração no cofre.

Se precisar de adicionar um adaptador depois de ter registado o servidor de configuração no cofre, terá de adicionar o adaptador nas propriedades da VM e, em seguida, voltar a registar o servidor no cofre.


## <a name="reregister-a-configuration-server-in-the-same-vault"></a>Voltar a registar um servidor de configuração no cofre do mesmo

Pode registar o servidor de configuração no cofre do mesmo se for necessário. Se tiver um computador de servidor de processos adicionais, além do servidor de processo predefinido em execução no servidor de configuração, voltar a registar ambas as máquinas.

  1. No cofre, abra **gerir** > **infraestrutura de recuperação de Site** > **servidores de configuração**.
  2. No **servidores**, clique em **transferir chave de registo**. Esta ação transfere o ficheiro de credenciais do cofre.
  3. Inicie sessão no computador do servidor de configuração.
  4. No **%ProgramData%\ASR\home\svagent\bin**, abra **cspsconfigtool.exe**.
  5. No **registo de cofre** separador, clique em Procurar e localizado o ficheiro de credenciais do cofre que transferiu.
  6. Se for necessário, forneça detalhes do servidor proxy. Em seguida, clique em **registar**.
  7. Abra uma janela de comando do PowerShell de administrador e execute o seguinte comando:

      ```
      $pwd = ConvertTo-SecureString -String MyProxyUserPassword
      Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber – ProxyUserName domain\username -ProxyPassword $pwd
      net stop obengine
      net start obengine
      ```

## <a name="delete-or-unregister-a-configuration-server"></a>Elimine ou anular o registo de um servidor de configuração

1. Desativar [desative a proteção](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) para todas as VMs no servidor de configuração.
2. [Desassocie](site-recovery-setup-replication-settings-vmware.md#dissociate-a-configuration-server-from-a-replication-policy) e [eliminar](site-recovery-setup-replication-settings-vmware.md#delete-a-replication-policy) todas as políticas de replicação do servidor de configuração.
3. [Eliminar](site-recovery-vmware-to-azure-manage-vCenter.md#delete-a-vcenter-in-azure-site-recovery) todos os anfitriões de servidores/vSphere vCenters que estão associados com o servidor de configuração.
4. No cofre, abra **infraestrutura de recuperação de Site** > **servidores de configuração**
5. Clique no servidor de configuração que pretende remover. Em seguida, no **detalhes** página, clique em **eliminar**.

    ![Eliminar o servidor de configuração](./media/site-recovery-vmware-to-azure-manage-configuration-server/delete-configuration-server.png)
   

### <a name="delete-with-powershell"></a>Eliminar com o PowerShell

Pode, opcionalmente, elimine o servidor de configuração com o PowerShell:

1. [Instalar](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.4.0) módulo Azure PowerShell
2. Iniciar sessão na sua conta do Azure utilizando o comando:
    
    `Login-AzureRmAccount`
3. Selecione a subscrição do cofre:

     `Get-AzureRmSubscription –SubscriptionName <your subscription name> | Select-AzureRmSubscription`
3.  Defina o contexto de cofre:
    
    ```
    $vault = Get-AzureRmRecoveryServicesVault -Name <name of your vault>
    Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault
    ```
4. Obter o servidor de configuração:

    `$fabric = Get-AzureRmSiteRecoveryFabric -FriendlyName <name of your configuration server>`
6. Elimine o servidor de configuração:

    `Remove-AzureRmSiteRecoveryFabric -Fabric $fabric [-Force] `

> [!NOTE]
> Pode utilizar o **-Force** opção em Remove-AzureRmSiteRecoveryFabric para eliminação forçada do servidor de configuração.
 


## <a name="renew-ssl-certificates"></a>Renovar os certificados SSL

O servidor de configuração tem um servidor web integradas, que orquestra as atividades do serviço de mobilidade, servidores de processos e servidores de destino mestre ligados ao mesmo. O servidor web utiliza um certificado SSL para autenticar clientes. O certificado expira após três anos e pode ser renovado em qualquer altura.

### <a name="check-expiry"></a>Verificação de expiração

Para implementações de servidor de configuração antes de Maio de 2016, a expiração de certificado foi definida para um ano. Se tiver um certificado está prestes a expirar, ocorre o seguinte:

- Quando a data de expiração é dois meses ou menos, inicia o serviço envio de notificações no portal e por correio eletrónico (se subscrever notificações do Azure Site Recovery).
- É apresentada uma faixa de notificação na página de recurso do cofre. Clique na faixa para obter mais detalhes.
- Se vir um **atualizar agora** botão, isto indica que existem alguns componentes no seu ambiente que ainda não foram atualizados para 9.4.xxxx.x ou versões superiores. Atualize componentes antes de renovar o certificado. Não é possível renovar em versões anteriores.

### <a name="renew-the-certificate"></a>Renovar o certificado

1. No cofre, abra **infraestrutura de recuperação de Site** > **servidor de configuração**e clique no servidor de configuração necessárias.
2. A data de expiração é apresentado em **estado de funcionamento do servidor de configuração**
3. Clique em **renovar certificados**. 


## <a name="next-steps"></a>Passos Seguintes

Reveja os tutoriais para configurar a recuperação após desastre de [VMs de VMware](tutorial-vmware-to-azure.md) e servers(tutorial-physical-to-azure.md) físico para o Azure.
