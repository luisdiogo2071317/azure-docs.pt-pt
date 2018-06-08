---
title: Implementar a pilha do Azure - PowerShell | Microsoft Docs
description: Neste artigo, instale o ASDK na linha de comandos com o PowerShell.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: ''
ms.date: 06/07/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: f0d7daa479f6e6ea345e010962488c1ecad5b7e2
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34849962"
---
# <a name="deploy-the-asdk-from-the-command-line"></a>Implementar o ASDK na linha de comandos
O ASDK é um ambiente de teste e desenvolvimento que pode implementar para avaliar e demonstrar serviços e funcionalidades de pilha do Azure. Para obtê-lo em execução, terá de preparar o hardware de ambiente e executar alguns scripts (esta ação irá demorar várias horas). Depois disso, pode inscrever-para os portais de administrador e utilizador para começar a utilizar a pilha do Azure.

## <a name="prerequisites"></a>Pré-requisitos 
Prepare o computador de anfitrião do kit de desenvolvimento. Planear o hardware, software e rede. O computador que aloja o kit de desenvolvimento (o anfitrião do kit de desenvolvimento) têm de cumprir requisitos de hardware, software e rede. Também tem de escolher entre a utilização do Azure Active Directory (Azure AD) ou os serviços de Federação do Active Directory (AD FS). Lembre-se de que estão em conformidade com os pré-requisitos antes de iniciar a implementação para que o processo de instalação seja executado facilmente. 

Antes de implementar o ASDK, certifique-se de que hardware de desenvolvimento planeada kit anfitrião do seu computador, sistema operativo, conta e configurações de rede cumprem os requisitos mínimos para instalar o ASDK.

**[Reveja a considerações de planeamento de implementação de ASDK](asdk-deploy-considerations.md)**

> [!TIP]
> Pode utilizar o [ferramenta de verificação de requisitos de implementação do Azure pilha](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) depois de instalar o sistema operativo para confirmar que o seu hardware cumpre todos os requisitos.

## <a name="download-and-extract-the-deployment-package"></a>Transferir e extraia o pacote de implementação
Depois de se certificar de que o computador de anfitrião de kit de desenvolvimento cumpre os requisitos básicos para instalar o ASDK, o passo seguinte é a mesma e extraiu o pacote de implementação ASDK. O pacote de implementação inclui o ficheiro de Cloudbuilder.vhdx, que é uma unidade de disco rígida virtual que inclui um sistema operativo de arranque e os ficheiros de instalação de pilha do Azure.

Pode transferir o pacote de implementação para o anfitrião do kit de desenvolvimento ou para outro computador. Os ficheiros extraídos implementação demorar até 60 GB de espaço livre em disco, pelo que utilizar outro computador pode ajudar a reduzir os requisitos de hardware para o anfitrião do kit de desenvolvimento.

**[Transferir e extrair o Kit de desenvolvimento de pilha do Azure (ASDK)](asdk-download.md)**

## <a name="prepare-the-development-kit-host-computer"></a>Preparar o computador de anfitrião do kit de desenvolvimento
Antes de poder instalar o ASDK no computador anfitrião, o ambiente tem de ser preparado e o sistema configurado para arranque de VHD. Após este passo, irá arrancar o anfitrião do kit de desenvolvimento para o Cloudbuilder.vhdx (um disco rígido virtual que inclui um sistema operativo de arranque e os ficheiros de instalação de pilha do Azure).

Utilize o PowerShell para configurar o computador de anfitrião ASDK para arrancar a partir CloudBuilder.vhdx. Estes comandos configurar o computador do anfitrião ASDK para arrancar a partir de harddisk virtual (CloudBuilder.vhdx) transferido e extraído pilha do Azure. Depois de concluir estes passos, reinicie o computador de anfitrião ASDK.

Para configurar o computador de anfitrião ASDK para arrancar a partir CloudBuilder.vhdx:

  1. Inicie uma linha de comandos como administrador.
  2. Execute `bcdedit /copy {current} /d "Azure Stack"`
  3. Cópia (CTRL + C), o valor CLSID devolvida, incluindo necessários {}' s. Este valor é referido como {CLSID} e terão de ser colado em (CTRL + V ou contexto) nos passos restantes.
  4. Execute `bcdedit /set {CLSID} device vhd=[C:]\CloudBuilder.vhdx` 
  5. Execute `bcdedit /set {CLSID} osdevice vhd=[C:]\CloudBuilder.vhdx` 
  6. Execute `bcdedit /set {CLSID} detecthal on` 
  7. Execute `bcdedit /default {CLSID}`
  8. Para verificar as definições de arranque, execute `bcdedit`. 
  9. Certifique-se de que o CloudBuilder.vhdx ficheiro foi movido para a raiz da unidade C:\ (C:\CloudBuilder.vhdx) e reinicie o computador de anfitrião do kit de desenvolvimento. Quando o computador de anfitrião ASDK é reiniciado, deve efetuar o da unidade de disco rígida da máquina virtual CloudBuilder.vhdx para começar a implementação de ASDK arranque. 

> [!IMPORTANT]
> Certifique-se de que tem acesso KVM para o computador de anfitrião do kit de desenvolvimento ou de física direta antes de reiniciar. Quando a VM é iniciado pela primeira vez, pede-lhe para concluir a configuração de servidor do Windows. Fornece as mesmas credenciais de administrador que utilizou para iniciar sessão no computador de anfitrião do kit de desenvolvimento. 

### <a name="prepare-the-development-kit-host-using-powershell"></a>Preparar o anfitrião do kit de desenvolvimento com o PowerShell 
Depois do kit de desenvolvimento computador anfitrião com êxito arranca em imagem CloudBuilder.vhdx, inicie sessão com as mesmas credenciais de administrador local que utilizou para iniciar sessão no computador de anfitrião do kit de desenvolvimento (e de que é fornecido como parte da finalizar o Windows Server O programa de configuração quando o computador anfitrião arranca a partir de VHD). 

> [!NOTE]
> Opcionalmente, também pode configurar [definições de telemetria de pilha do Azure](asdk-telemetry.md#set-telemetry-level-in-the-windows-registry) *antes* o ASDK a instalar.

Abra uma consola elevada do PowerShell e execute os comandos nesta secção para implementar o ASDK no anfitrião do kit de desenvolvimento.

> [!IMPORTANT] 
> Instalação de ASDK suporta exatamente uma placa de interface (NIC) para funcionamento em rede. Se tiver vários NICs, certifique-se de que só está ativado (e todos os outros estão desativados) antes de executar o script de implementação.

Pode implementar pilha do Azure com o Azure AD ou o Windows Server AD FS como o fornecedor de identidade. Pilha do Azure, fornecedores de recursos e outras aplicações funcionam da mesma forma com ambos.

> [!TIP]
> Se não forneça quaisquer parâmetros de configuração (consulte parâmetros opcionais InstallAzureStackPOC.ps1 e exemplos abaixo), são-lhe pedido para os parâmetros obrigatórios.

### <a name="deploy-azure-stack-using-azure-ad"></a>Implementar a pilha do Azure com o Azure AD 
Para implementar o Azure pilha **utilizar o Azure AD como o fornecedor de identidade**, tem de ter conectividade à internet diretamente ou através de um proxy transparente. 

Execute os seguintes comandos do PowerShell para implementar o kit de desenvolvimento com o Azure AD:

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator     
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password
  ```

Alguns minutos para instalação ASDK, que ser-lhe-á solicitado para credenciais do Azure AD. Tem de fornecer credenciais de administrador global para o seu inquilino do Azure AD. 

### <a name="deploy-azure-stack-using-ad-fs"></a>Implementar a pilha do Azure utilizando o AD FS 
Para implementar o kit de desenvolvimento **através do AD FS como o fornecedor de identidade**, execute os seguintes comandos do PowerShell (apenas tem de adicionar o parâmetro - UseADFS): 

  ```powershell
  cd C:\CloudDeployment\Setup     
  $adminpass = Get-Credential Administrator 
  .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -UseADFS
  ```

Nas implementações do AD FS, o serviço de diretório de carimbo de predefinido é utilizado como o fornecedor de identidade. A conta predefinida para iniciar sessão com é azurestackadmin@azurestack.local, e a palavra-passe será definida para o que é fornecido como parte dos comandos de configuração do PowerShell.

O processo de implementação pode demorar algumas horas, durante o tempo, o sistema automaticamente reinicia uma vez. Quando a implementação for bem sucedida, será apresentada a consola do PowerShell: **concluída: a ação 'Implementação'**. Se a implementação falhar, pode tentar executar o script novamente utilizando o parâmetro-volte a executar. Em alternativa, pode [Reimplementar ASDK](asdk-redeploy.md) a partir do zero.

> [!IMPORTANT]
> Se pretender monitorizar o progresso da implementação, após o anfitrião ASDK é reiniciado, tem de iniciar sessão como AzureStack\AzureStackAdmin. Se iniciar sessão como um administrador local depois do computador anfitrião é reiniciado (e associado ao domínio azurestack.local), não verá o progresso da implementação. Não execute novamente a implementação, em vez disso, inicie sessão como azurestack validar que está em execução.


#### <a name="azure-ad-deployment-script-examples"></a>Exemplos de script de implementação do Azure AD
Pode aplicar o script de toda a implementação do Azure AD. Seguem-se alguns exemplos comentados incluem alguns parâmetros opcionais.

Se a sua identidade do Azure AD apenas associada **um** diretório do Azure AD:
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -TimeServer 52.168.138.145 #Example time server IP address.
```

Se a sua identidade do Azure AD está associada **maior do que um** diretório do Azure AD:
```powershell
cd C:\CloudDeployment\Setup 
$adminpass = Get-Credential Administrator 
$aadcred = Get-Credential "<Azure AD global administrator account name>" #Example: user@AADDirName.onmicrosoft.com 
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -InfraAzureDirectoryTenantName "<Azure AD directory in the form of domainname.onmicrosoft.com or an Azure AD verified custom domain name>" -TimeServer 52.168.138.145 #Example time server IP address.
```

Se o ambiente tiver DHCP ativado, tem de incluir os seguintes parâmetros adicionais para uma das opções acima (utilização de exemplo fornecidos): 

```powershell
.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -InfraAzureDirectoryTenantAdminCredential $aadcred -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1 -TimeServer 10.222.112.26
```

### <a name="asdk-installazurestackpocps1-optional-parameters"></a>Parâmetros opcionais ASDK InstallAzureStackPOC.ps1
|Parâmetro|Necessários/opcionais|Descrição|
|-----|-----|-----|
|AdminPassword|Necessário|Define a conta de administrador local e todas as outras contas de utilizador de todas as máquinas virtuais criadas como parte da implementação do kit de desenvolvimento. Esta palavra-passe têm de corresponder a atual palavra-passe de administrador local no anfitrião.|
|InfraAzureDirectoryTenantName|Necessário|Define o diretório do inquilino. Utilize este parâmetro para especificar um diretório específico em que a conta do AAD tem permissões para gerir vários diretórios. Total de nome de um inquilino de diretório do AAD no formato. onmicrosoft.com ou um Azure AD verificar o nome de domínio personalizado.|
|TimeServer|Necessário|Utilize este parâmetro para especificar um servidor de tempo específico. Este parâmetro tem de ser fornecido como um endereço IP do servidor de hora válido. Os nomes dos servidores não são suportadas.|
|InfraAzureDirectoryTenantAdminCredential|Opcional|Define o nome de utilizador do Azure Active Directory e a palavra-passe. Estas credenciais do Azure tem de ser um ID da organização.|
|InfraAzureEnvironment|Opcional|Selecione o ambiente do Azure com o qual pretende registar esta implementação de pilha do Azure. As opções incluem público Azure, Azure do Azure - China, - US Government.|
|DNSForwarder|Opcional|Um servidor DNS é criado como parte da implementação da pilha do Azure. Para permitir que os computadores dentro da solução para resolver nomes fora de carimbo, forneça o seu servidor DNS da infraestrutura existente. O servidor DNS no carimbo reencaminha os pedidos de resolução de nome desconhecido para este servidor.|
|NatIPv4Address|Necessário para o suporte de NAT de DHCP|Define um endereço IP estático para MAS BGPNAT01. Só utilize este parâmetro se o DHCP não conseguir atribuir um endereço IP válido para aceder à Internet.|
|NatIPv4Subnet|Necessário para o suporte de NAT de DHCP|Prefixo de sub-rede IP utilizado para DHCP através de suporte NAT. Só utilize este parâmetro se o DHCP não conseguir atribuir um endereço IP válido para aceder à Internet.|
|PublicVlanId|Opcional|Define o ID de VLAN. Só utilize este parâmetro se o anfitrião e MAS BGPNAT01 tem de configurar o ID de VLAN para aceder à rede física (e a Internet). Por exemplo,.\InstallAzureStackPOC.ps1-Verbose - PublicVLan 305|
|Voltar a executar|Opcional|Utilize este sinalizador para executar novamente a implementação. Todas as entradas anterior é utilizada. Reintroduzir dados anteriormente fornecidos não são suportados porque vários valores exclusivos são gerados e utilizados para implementação.|


## <a name="perform-post-deployment-configurations"></a>Efetuar configurações de pós-implementação
Depois de instalar o ASDK, existem algumas verificações de pós-instalação recomendadas e alterações de configuração que devem ser efetuadas. Pode validar a instalação foi instalado com êxito utilizando o cmdlet de teste AzureStack e instalar as ferramentas do PowerShell de pilha do Azure e do GitHub. 

Deve também repor a política de expiração de palavra-passe para se certificar de que a palavra-passe para o anfitrião do kit de desenvolvimento não expira antes de terminar o período de avaliação.

> [!NOTE]
> Opcionalmente, também pode configurar [definições de telemetria de pilha do Azure](asdk-telemetry.md#enable-or-disable-telemetry-after-deployment) *depois* o ASDK a instalar.

**[Publicar tarefas de implementação ASDK](asdk-post-deploy.md)**

## <a name="register-with-azure"></a>Registar com o Azure
Tem de registar pilha do Azure com o Azure, para que possa [transferir itens do Azure marketplace](asdk-marketplace-item.md) à pilha do Azure.

**[Registar a pilha do Azure com o Azure](asdk-register.md)**

## <a name="next-steps"></a>Passos Seguintes
Parabéns! Depois de concluir estes passos, terá de um ambiente do kit de desenvolvimento com o [administrador](https://adminportal.local.azurestack.external) e [utilizador](https://portal.local.azurestack.external) portais. 

[Após a instalação de ASDK tarefas de configuração](asdk-post-deploy.md)

