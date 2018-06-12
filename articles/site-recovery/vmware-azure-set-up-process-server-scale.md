---
title: Configurar um servidor de processos no Azure para a VM de VMware e reativação pós-falha do servidor físico com o Azure Site Recovery | Microsoft Docs
description: Este artigo descreve como configurar um servidor de processos no Azure, a reativação pós-falha VMs do Azure para VMware.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 06/10/2018
ms.author: raynew
ms.openlocfilehash: 3e53954341136a293052f9af755515a5552432fe
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35300852"
---
# <a name="set-up-additional-process-servers-for-scalability"></a>Configurar servidores de processos adicionais para escalabilidade

Por predefinição, quando está a replicar VMs de VMware ou servidores físicos a utilização do Azure [recuperação de Site](site-recovery-overview.md), um servidor de processos está instalado na máquina do servidor de configuração e é utilizado para coordenar a transferência de dados entre a recuperação de sites e a infraestrutura no local. Para aumentar a capacidade e dimensionar a sua implementação de replicação, pode adicionar servidores de processos adicionais autónomo. Este artigo descreve como efetuar esta ação.

## <a name="before-you-start"></a>Antes de começar

### <a name="capacity-planning"></a>Planeamento de capacidade

Certifique-se de que já realizou [planeamento de capacidade](site-recovery-plan-capacity-vmware.md) para replicação de VMware. Isto ajuda a identificar como e quando deve implementar mais servidores de processos.

### <a name="sizing-requirements"></a>Requisitos de dimensionamento 

Certifique-se os requisitos de dimensionamento resumidos na tabela. Em geral, se pode dimensionar a sua implementação para mais de 200 máquinas de origem ou tem um total diariamente churn taxa de mais do que 2 TB, terá de servidores de processos adicionais para processar o volume de tráfego.

| **Servidor de processos adicionais** | **Tamanho da cache do disco** | **Taxa de alteração de dados** | **Máquinas protegidas** |
| --- | --- | --- | --- |
|4 vCPUs (2 sockets * 2 núcleos @ GHz 2,5), 8 GB de memória |300 GB |250 GB ou inferior |Replicar máquinas 85 ou menos. |
|8 vCPUs (2 sockets * 4 núcleos @ GHz 2,5), 12 GB de memória |600 GB |250 GB a 1 TB |Replicar entre 85 150 máquinas. |
|12 vCPUs (2 sockets * 6 núcleos @ GHz 2,5) 24 GB de memória |1 TB |1 TB para 2 TB |Replicar entre 150 225 máquinas. |

### <a name="prerequisites"></a>Pré-requisitos

Os pré-requisitos para o servidor de processos adicionais estão resumidos na tabela seguinte.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]


## <a name="download-installation-file"></a>Transferir o ficheiro de instalação

Transfira o ficheiro de instalação para o servidor de processos da seguinte forma:

1. Inicie sessão no portal do Azure e navegue para o Cofre de serviços de recuperação.
2. Abra **infraestrutura de recuperação de sites** > **VMWare e máquinas físicas** > **servidores de configuração** (em para VMware & física Máquinas).
3. Selecione o servidor de configuração para desagregar os detalhes do servidor. Em seguida, clique em **+ o servidor de processos**.
4. No **servidor de processos adicionar** >  **escolha onde pretende implementar o servidor de processos**, selecione **no local do servidor de processos de implementar, escalamento horizontal**.

  ![Adicionar página de servidores](./media/vmware-azure-set-up-process-server-scale/add-process-server.png)
1. Clique em **transferir o Microsoft Azure Site Recovery Unified configuração**. Esta ação transfere a versão mais recente do ficheiro de instalação.

  > [!WARNING]
  A versão de instalação do servidor de processo deve ser o mesmo como, ou anterior, a versão do servidor de configuração tem de executar. É uma forma simples para garantir a compatibilidade da versão a utilizar o mesmo programa de instalação, o que utilizou mais recentemente para instalar ou atualizar o seu servidor de configuração.

## <a name="install-from-the-ui"></a>Instale a partir da IU

Instale da seguinte forma. Depois de configurar o servidor, migre as máquinas de origem para utilizá-lo.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="install-from-the-command-line"></a>Instalar a partir da linha de comandos

Instale, executando o seguinte comando:

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
```

Onde os parâmetros da linha de comando são os seguintes:

[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]

Por exemplo:

```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```
### <a name="create-a-proxy-settings-file"></a>Criar um ficheiro de definições de proxy

Se precisar de configurar um proxy, o parâmetro ProxySettingsFilePath aceita um ficheiro como entrada. Pode criar o ficheiro da seguinte forma e transmita-o como parâmetro de entrada ProxySettingsFilePath.

```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre [gerir processar as definições do servidor](vmware-azure-manage-process-server.md)
