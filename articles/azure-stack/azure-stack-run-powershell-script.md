---
title: Implementar o Kit de desenvolvimento de pilha do Azure | Microsoft Docs
description: "Saiba como preparar o Kit de desenvolvimento de pilha do Azure e execute o script do PowerShell para implementá-la."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 0ad02941-ed14-4888-8695-b82ad6e43c66
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2017
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 470a45aea253e1e238983527427b600117e413fe
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/16/2017
---
# <a name="deploy-the-azure-stack-development-kit"></a>Implementar o Kit de desenvolvimento de pilha do Azure

*Aplica-se a: Azure da pilha Kit de desenvolvimento*

Para implementar o [Kit de desenvolvimento de pilha do Azure](azure-stack-poc.md), tem de concluir os seguintes passos:

1. [Transferir o pacote de implementação](https://azure.microsoft.com/overview/azure-stack/try/?v=try) para obter o Cloudbuilder.vhdx.
2. Prepare o cloudbuilder.vhdx para configurar o computador (o anfitrião do kit de desenvolvimento) no qual pretende instalar o kit de desenvolvimento. Após este passo, irá arrancar o anfitrião do kit de desenvolvimento para o Cloudbuilder.vhdx.
3. Implemente o kit de desenvolvimento no anfitrião do kit de desenvolvimento.

> [!NOTE]
> Para obter os melhores resultados, mesmo se pretender utilizar um ambiente desligado de pilha do Azure, é melhor implementar enquanto estiver ligado à internet. Dessa forma, a versão de avaliação do Windows Server 2016 incluída com a instalação do kit de desenvolvimento, pode ser ativada no momento da implementação.

## <a name="download-and-extract-the-development-kit"></a>Transferir e extrair o kit de desenvolvimento
1. Antes de iniciar a transferência, certifique-se de que o computador cumpre os seguintes pré-requisitos:

  - O computador tem de ter, pelo menos, 60 GB de espaço livre em disco em quatro separadas, idênticas unidades lógicas de disco rígidas além para o disco de sistema operativo.
  - [.NET framework 4.6 (ou uma versão posterior)](https://aka.ms/r6mkiy) tem de estar instalado.

2. [Aceda à página Introdução ao](https://azure.microsoft.com/overview/azure-stack/try/?v=try) onde pode transferir o Kit de desenvolvimento de pilha do Azure, forneça os detalhes e, em seguida, clique em **submeter**.
3. Transfira e execute o [Verificador de implementação para a Azure pilha Development Kit](https://go.microsoft.com/fwlink/?LinkId=828735&clcid=0x409) script Verificador de pré-requisitos. Este script autónomo realiza as verificações de pré-requisitos efetuadas pelo programa de configuração para Kit de desenvolvimento de pilha do Azure. Fornece uma forma para confirmar que está a cumprir os requisitos de hardware e software, antes de transferir o pacote maior para Kit de desenvolvimento de pilha do Azure.
4. Em **transferir o software**, clique em **Kit de desenvolvimento de pilha do Azure**.

  > [!NOTE]
  > A transferência ASDK (AzureStackDevelopmentKit.exe) é approximiately 10GB por si só. Se optar por transferir também o ficheiro ISO de versão de avaliação do Windows Server 2016, aumenta o tamanho de transferência para aproximadamente 17GB. Pode utilizar esse ficheiro ISO para criar e adicionar uma imagem de máquina virtual do Windows Server 2016 para o mercado de pilha do Azure após a conclusão da instalação ASDK. Tenha em atenção que esta imagem de avaliação do Windows Server 2016 só pode ser utilizada com o ASDK e está sujeita aos termos de licenciamento ASDK.

5. Depois de concluída a transferência, clique em **executar** para iniciar o ASDK Self-extractor (AzureStackDevelopmentKit.exe).
6. Rever e aceitar o contrato de licença apresentadas do **contrato de licença** página do Assistente de Self-Extractor e, em seguida, clique em **seguinte**.
7. Reveja as informações de declaração de privacidade apresentadas no **aviso importante** página do Assistente de Self-Extractor e, em seguida, clique em **seguinte**.
8. Selecione a localização dos ficheiros de configuração de pilha do Azure a ser extraído no **selecionar localização de destino** página do Assistente de Self-Extractor e, em seguida, clique em **seguinte**. A localização predefinida é *pasta atual*\Azure Kit de desenvolvimento de pilha. 
9. Reveja a resumo na localização de destino a **pronto para extrair** página do Assistente de Self-Extractor e, em seguida, clique em **extrair** para extrair o CloudBuilder.vhdx (aproximadamente 25 GB) e ThirdPartyLicenses.rtf ficheiros. Este processo irá demorar algum tempo a concluir.
10. Copiar ou mover o ficheiro de CloudBuilder.vhdx na raiz da unidade C:\ (C:\CloudBuilder.vhdx) no computador anfitrião ASDK.

> [!NOTE]
> Depois de extrair os ficheiros, pode eliminar o. EXE e. Ficheiros de reciclagem para recuperar espaço no disco rígido. Em alternativa, a cópia de segurança se estes ficheiros para que não precisa de transferir os ficheiros novamente se tiver de Reimplementar o ASDK.

## <a name="deploy-the-asdk-using-a-guided-experience"></a>Implementar o ASDK utilizando uma experiência assistida
O ASDK pode ser implementado com uma interface gráfica (GUI) fornecida ao descarregar e executar o script de PowerShell asdk installer.ps1.

> [!NOTE]
> A interface de utilizador do instalador para o Kit de desenvolvimento de pilha do Azure é um script sourced aberto com base em WCF e PowerShell.

### <a name="prepare-the-development-kit-host-using-a-guided-user-experience"></a>Preparar o anfitrião do kit de desenvolvimento utilizando uma experiência de utilizador orientada
Antes de poder instalar o ASDK no computador anfitrião, o ambiente de ASDK tem de ser preparado.
1. Inicie sessão como um administrador Local no computador anfitrião ASDK.
2. Certifique-se de que o ficheiro CloudBuilder.vhdx foi movido para a raiz da unidade C:\ (C:\CloudBuilder.vhdx).
3. Execute o seguinte script para transferir o ficheiro de instalador de kit de desenvolvimento (asdk installer.ps1) da [repositório do GitHub de pilha do Azure ferramentas](https://github.com/Azure/AzureStack-Tools) para o **C:\AzureStack_Installer** pasta no seu computador do anfitrião de kit de desenvolvimento:

  ```powershell
  # Variables
  $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
  $LocalPath = 'C:\AzureStack_Installer'
  # Create folder
  New-Item $LocalPath -Type directory
  # Download file
  Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
  ```

4. A partir de uma consola elevada do PowerShell, inicie o **C:\AzureStack_Installer\asdk-installer.ps1** script e, em seguida, clique em **preparar o ambiente**.
5. No **selecione Cloudbuilder vhdx** página do instalador, procure e selecione o **cloudbuilder.vhdx** ficheiro que transferiu e extraiu nos passos anteriores. Nesta página, pode também, opcionalmente, ativar o **adicionar controladores** caixa de verificação se precisar de adicionar controladores ao computador de anfitrião do kit de desenvolvimento.  
6. No **definições opcionais** , indique a conta de administrador local para o computador de anfitrião do kit de desenvolvimento. 

  > [!IMPORTANT]
  > Se não fornecer estas credenciais, terá de direta ou de KVM acesso ao anfitrião depois de reiniciar o computador como parte de como configurar o kit de desenvolvimento.

7. Também pode fornecer estas definições opcionais sobre o **definições opcionais** página:
    - **ComputerName**: esta opção define o nome para o anfitrião do kit de desenvolvimento. O nome tem de cumprir os requisitos de FQDN e tem de ser 15 carateres ou menos. A predefinição é um nome de computador aleatório gerado pelo Windows.
    - **Fuso horário**: define o fuso horário para o anfitrião do kit de desenvolvimento. A predefinição é (UTC-8:00) hora do Pacífico (E.U.A. e Canadá).
    - **Configuração de IP estático**: define a sua implementação utilizar um endereço IP estático. Caso contrário, quando o instalador reinicia com o cloudbuilder.vhx, as interfaces de rede são configuradas com DHCP.
11. Clique em **Seguinte**.
12. Se optou por uma configuração de IP estático no passo anterior, tem de agora:
    - Selecione um adaptador de rede. Certifique-se de que pode ligar ao adaptador antes de clicar em **seguinte**.
    - Certifique-se de que o **endereço IP**, **Gateway**, e **DNS** valores estão corretos e, em seguida, clique em **seguinte**.
13. Clique em **seguinte** para iniciar o processo de preparação.
14. Quando a preparação indica **concluído**, clique em **seguinte**.
15. Clique em **reiniciar agora** para arrancar o cloudbuilder.vhdx e continuar o processo de implementação.


### <a name="deploy-the-development-kit-using-a-guided-experience"></a>Implementar o kit de desenvolvimento utilizando uma experiência assistida
Depois de preparar o computador de anfitrião ASDK, o ASDK pode ser implementado para a imagem de CloudBuilder.vhdx utilizando os seguintes passos. 
1. Depois do computador anfitrião arranca com êxito na imagem do CloudBuilder.vhdx, inicie sessão com as credenciais de administrador especificadas nos passos anteriores. 
2. Abra uma consola elevada do PowerShell e execute o **\AzureStack_Installer\asdk-installer.ps1** script (o que pode agora ser numa unidade diferente na imagem CloudBuilder.vhdx). Clique em **Instalar**.
3. No **tipo** caixa pendente, selecione **nuvem do Azure** ou **do AD FS**.
    - **Nuvem do Azure**: configura o Azure Active Directory (Azure AD) como o fornecedor de identidade. Para utilizar esta opção, terá uma ligação à internet, o nome completo de um Azure AD inquilino de diretório no formato *domainname*. onmicrosoft.com ou um Azure AD verificar as credenciais de administrador global e nome de domínio personalizado para o diretório especificado. 
    - **AD FS**: O carimbo de predefinição será utilizado o serviço de diretório como o fornecedor de identidade. A conta predefinida para iniciar sessão com é azurestackadmin@azurestack.local, e a palavra-passe é fornecido como parte da configuração.
4. Em **palavra-passe de administrador Local**, além de **palavra-passe** caixa, escreva a palavra-passe de administrador local (que tem de coincidir com a palavra-passe de administrador local configurado atual) e, em seguida, clique em **Seguinte**.
5. Selecione um adaptador de rede a utilizar para o kit de desenvolvimento e, em seguida, clique em **seguinte**.
6. Selecione o DHCP ou a configuração de rede estático para a máquina virtual de BGPNAT01.
    - **DHCP** (predefinição): A máquina virtual obtém a configuração de rede IP do servidor DHCP.
    - **Estático**: Utilize esta opção apenas se o DHCP não é possível atribuir um endereço IP válido para a pilha do Azure aceder à Internet. Tem de ser especificado um endereço IP estático com o comprimento de subnetmask no formato CIDR (por exemplo, 10.0.0.5/24).
7. Opcionalmente, defina os seguintes valores:
    - **ID de VLAN**: define o ID de VLAN. Utilize esta opção apenas se o anfitrião e AzS BGPNAT01 tem de configurar o ID de VLAN para aceder à rede física (e a internet). 
    - **Reencaminhador DNS**: um servidor DNS é criado como parte da implementação da pilha do Azure. Para permitir que os computadores dentro da solução para resolver nomes fora de carimbo, forneça o seu servidor DNS da infraestrutura existente. O servidor DNS no carimbo reencaminha os pedidos de resolução de nome desconhecido para este servidor.
    - **Hora server**: Isto necessário campo define o servidor de tempo a ser utilizado pelo kit de desenvolvimento. Este parâmetro tem de ser fornecido como um endereço IP do servidor de hora válido. Os nomes dos servidores não são suportadas.
  
  > [!TIP]
  > Para localizar um servidor de tempo de endereço IP, visite [pool.ntp.org](http:\\pool.ntp.org) ou ping time.windows.com. 
  
8. Clique em **Seguinte**. 
9. No **verificar as propriedades de cartão de interface de rede** página, verá uma barra de progresso. 
    - Se diz **não é possível transferir uma atualização**, siga as instruções na página.
    - Quando indicar **concluído**, clique em **seguinte**.
10. No **resumo** página, clique em **implementar**. Aqui, também pode copiar os comandos de configuração do PowerShell que serão utilizados para instalar o kit de desenvolvimento.
11. Se estiver a utilizar uma implementação do Azure AD, ser-lhe-emos pedido para introduzir as credenciais de conta de administrador global do Azure AD alguns minutos depois de iniciado o programa de configuração.
12. O processo de implementação pode demorar algumas horas, durante o qual o sistema automaticamente reinicia uma vez. Quando a implementação for bem sucedida, será apresentada a consola do PowerShell: **concluída: a ação 'Implementação'**. Se a implementação falhar, pode pode [Reimplementar](azure-stack-redeploy.md) do zero ou utilize o PowerShell seguinte comandos, do mesma janela elevada do PowerShell, reiniciar a implementação do último passo com êxito:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

   > [!IMPORTANT]
   > Se pretender monitorizar o progresso da implementação, inicie sessão como azurestack\AzureStackAdmin quando o anfitrião do kit de desenvolvimento for reiniciado durante a configuração. Se iniciar sessão como um administrador local depois do computador está associado ao domínio, não verá o progresso da implementação. Não execute novamente a implementação, em vez disso, inicie sessão como azurestack\AzureStackAdmin validar que está em execução.
   

## <a name="deploy-the-asdk-using-powershell"></a>Implementar o ASDK através do PowerShell
Os passos anteriores walked, através da implementação ASDK utilizando uma experiência de utilizador orientada. Em alternativa, pode utilizar o PowerShell para implementar o ASDK no anfitrião do kit de desenvolvimento, seguindo os passos nesta secção.

### <a name="configure-the-asdk-host-computer-to-boot-from-cloudbuildervhdx"></a>Configure o computador do anfitrião ASDK para arrancar a partir de CloudBuilder.vhdx
Estes comandos irão configurar o computador de anfitrião ASDK para arrancar a partir de harddisk virtual (CloudBuilder.vhdx) transferido e extraído pilha do Azure. Depois de concluir estes passos, reinicie o computador de anfitrião ASDK.

1. Inicie uma linha de comandos como administrador.
2. Execute `bcdedit /copy {current} /d "Azure Stack"`
3. Cópia (CTRL + C), o valor CLSID devolvida, incluindo o {} necessária ' s. Este valor é referido como {CLSID} e terão de ser colado em (CTRL + V ou contexto) nos passos restantes.
4. Execute `bcdedit /set {CLSID} device vhd=[C:]\CloudBuilder.vhdx` 
5. Execute `bcdedit /set {CLSID} osdevice vhd=[C:]\CloudBuilder.vhdx` 
6. Execute `bcdedit /set {CLSID} detecthal on` 
7. Execute `bcdedit /default {CLSID}`
8. Para verificar as definições de arranque, execute `bcdedit`. 
9. Certifique-se de que o CloudBuilder.vhdx ficheiro foi movido para a raiz da unidade C:\ (C:\CloudBuilder.vhdx) e reinicie o computador de anfitrião do kit de desenvolvimento. Quando o ASDK reinício do anfitrião deve agora predefinido para arrancar a partir da CloudBuilder.vhdx VM. 

### <a name="prepare-the-development-kit-host-using-powershell"></a>Preparar o anfitrião do kit de desenvolvimento com o PowerShell 
Depois do computador de anfitrião do kit de desenvolvimento arranca com êxito na imagem do CloudBuilder.vhdx, pode abrir uma consola elevada do PowerShell e execute os comandos nesta secção para implementar o ASDK no anfitrião do kit de desenvolvimento.

> [!IMPORTANT] 
> Instalação de ASDK suporta exatamente uma placa de interface (NIC) para funcionamento em rede. Se tiver vários NICs, certifique-se de que só está ativado (e todos os outros estão desativados) antes de executar o script de implementação.

Pode implementar a pilha do Azure com o Azure AD ou AD FS como o fornecedor de identidade. Pilha do Azure, fornecedores de recursos e outras aplicações funcionam da mesma forma com ambos. Para saber mais sobre o que é suportado com o AD FS na pilha do Azure, consulte o [chave conceitos e funcionalidades](.\azure-stack-key-features.md) artigo.

> [!TIP]
> Se não forneça quaisquer parâmetros de configuração (consulte parâmetros opcionais InstallAzureStackPOC.ps1 e exemplos abaixo), será solicitado para os parâmetros obrigatórios.

### <a name="deploy-azure-stack-using-azure-ad"></a>Implementar a pilha do Azure com o Azure AD 
Para implementar o Azure pilha **utilizar o Azure AD como o fornecedor de identidade**, tem de ter conectividade à internet diretamente ou através de um proxy transparente. Execute os seguintes comandos do PowerShell para implementar o kit de desenvolvimento com o Azure AD:

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

O processo de implementação pode demorar algumas horas, durante o tempo, o sistema automaticamente reinicia uma vez. Quando a implementação for bem sucedida, será apresentada a consola do PowerShell: **concluída: a ação 'Implementação'**. Se a implementação falhar, pode tentar executar o script novamente utilizando o parâmetro-volte a executar. Em alternativa, pode [Reimplementar ASDK](.\azure-stack-redeploy.md) a partir do zero.
> [!IMPORTANT]
> Se pretender monitorizar o progresso da implementação, após o anfitrião ASDK é reiniciado, tem de iniciar sessão como AzureStack\AzureStackAdmin. Se iniciar sessão como um administrador local depois do computador anfitrião é reiniciado (e associado ao domínio azurestack.local), não verá o progresso da implementação. Não execute novamente a implementação, em vez disso, inicie sessão como azurestack validar que está em execução.
>

#### <a name="azure-ad-deployment-script-examples"></a>Exemplos de script de implementação do Azure AD
Pode aplicar o script de toda a implementação do Azure AD. Seguem-se alguns exemplos comentados que incluem algumas parâmetros opcionais.

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

Se o ambiente **não** ter DHCP activado, em seguida, tem de incluir os seguintes parâmetros adicionais para uma das opções acima (utilização de exemplo fornecidos): 

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
|Volte a executar|Opcional|Utilize este sinalizador para executar novamente a implementação. Todas as entradas anterior é utilizada. Reintroduzir dados anteriormente fornecidos não são suportados porque vários valores exclusivos são gerados e utilizados para implementação.|

## <a name="activate-the-administrator-and-tenant-portals"></a>Ativar os portais de administrador e de inquilino
Depois de implementações que utilizam o Azure AD, tem de ativar ambas as os pilha do Azure administrador portais e de inquilino. Esta ativação permitir para dar o portal de pilha do Azure e o Azure Resource Manager, as permissões corretas (listadas na página de consentimento) para todos os utilizadores do diretório.

- Para o portal de administrador, navegue para https://adminportal.local.azurestack.external/guest/signup, leia as informações e, em seguida, clique em **aceitar**. Depois de a aceitar, pode adicionar os administradores de serviço que também não são administradores de inquilinos do diretório.

- Para o portal de inquilinos, navegue para https://portal.local.azurestack.external/guest/signup, ler as informações e, em seguida, clique em **aceitar**. Depois de a aceitar, os utilizadores no diretório podem iniciar sessão no portal do inquilino. 

> [!NOTE] 
> Se os portais não estão ativados, apenas o administrador da directory pode iniciar sessão e utilizar os portais. Se qualquer outro utilizador inicia sessão, verá um erro a indicar que o administrador não concedidas permissões a outros utilizadores. Quando o administrador não pertence nativamente no diretório do que Azure pilha está registada, o diretório de pilha do Azure tem anexado ao URL de ativação. Por exemplo, se a pilha do Azure está registada para fabrikam.onmicrosoft.com e o utilizador de admin é admin@contoso.com, navegue para https://portal.local.azurestack.external/guest/signup/fabrikam.onmicrosoft.com para ativar o portal. 

## <a name="reset-the-password-expiration-policy"></a>Repor a política de expiração de palavra-passe 
Para se certificar de que a palavra-passe para o anfitrião do kit de desenvolvimento não expira antes de terminar o período de avaliação, siga estes passos depois de implementar o ASDK.

### <a name="to-change-the-password-expiration-policy-from-powershell"></a>Para alterar a política de expiração de palavra-passe a partir do Powershell:
A partir de uma consola elevada do Powershell, execute o comando:

```powershell
Set-ADDefaultDomainPasswordPolicy -MaxPasswordAge 180.00:00:00 -Identity azurestack.local
```

### <a name="to-change-the-password-expiration-policy-manually"></a>Para alterar manualmente a política de expiração de palavra-passe:
1. No anfitrião do kit de desenvolvimento, abra **gestão de políticas de grupo** e navegue para **gestão de políticas de grupo** – **floresta: azurestack.local** – **domínios** – **azurestack.local**.
2. Clique com o botão direito **política de domínio predefinida** e clique em **editar**.
3. No Editor de grupo política de gestão, navegue para **configuração do computador** – **políticas** – **definições do Windows** – **definições de segurança**– **Políticas de conta** – **política de palavra-passe**.
4. No painel da direita, faça duplo clique **antiguidade de palavra-passe máximo**.
5. No **antiguidade de palavra-passe máximo propriedades** caixa de diálogo, altere o **palavra-passe expira em** valor 180, em seguida, clique em **OK**.


## <a name="next-steps"></a>Passos seguintes

[Ligar ao Azure Stack](azure-stack-connect-azure-stack.md)

[Configurar o PowerShell para ambientes de pilha do Azure](azure-stack-powershell-configure-quickstart.md)

[Registar a pilha do Azure com a sua subscrição do Azure](azure-stack-register.md)



