---
title: Utilizar o ponto final com privilégios na pilha do Azure | Microsoft Docs
description: Mostra como utilizar o ponto final com privilégios (PEP) na pilha do Azure (para um operador de pilha do Azure).
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: mabrigg
ms.reviewer: fiseraci
ms.openlocfilehash: 9fb928b7cb8e1a83734b64a8b9c19bc3cf3203ba
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32153189"
---
# <a name="using-the-privileged-endpoint-in-azure-stack"></a>Utilizar o ponto final com privilégios na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Como um operador de pilha do Azure, deve utilizar o portal de administrador, PowerShell ou APIs do Azure Resource Manager para as tarefas de gestão mais diárias. No entanto, para algumas menos operações comuns, tem de utilizar o *ponto final com privilégios* (PEP). O PEP é uma consola de PowerShell remota pré-configurada que fornece-lhe apenas suficiente capacidades para o ajudar a efetuar uma tarefa necessária. O ponto final utiliza [JEA PowerShell (apenas suficiente administração)](https://docs.microsoft.com/powershell/jea/overview) para expor apenas um conjunto de cmdlets restrito. Para aceder a PEP e invocar o conjunto de cmdlets restrito, é utilizada uma conta de baixo privilégio. Não existem contas de administrador são necessárias. Para segurança adicional, processamento de scripts não é permitido.

Pode utilizar o PEP para efetuar tarefas, tais como o seguinte:

- Para efetuar tarefas de baixo nível, tais como [recolher registos de diagnóstico](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics#log-collection-tool).
- Para executar muitas tarefas de integração do Centro de dados de pós-implementação para sistemas integrados, como adicionar reencaminhadores do sistema de nomes de domínio (DNS) após a implementação, configurar a integração do Microsoft Graph, integração de serviços de Federação do Active Directory (AD FS), rotação de certificado, etc.
- Trabalhar com suporte para obter acesso temporário alto nível para resolução de problemas detalhada de um sistema integrado.

Regista o PEP cada ação (e o resultado correspondente) que efetua na sessão do PowerShell. Isto fornece a transparência completa e auditoria completa de operações. Pode manter estes ficheiros de registo de auditorias futuras.

> [!NOTE]
> No Kit de desenvolvimento a pilha de Azure (ASDK), pode executar alguns dos comandos disponíveis no PEP diretamente a partir de uma sessão do PowerShell no anfitrião do kit de desenvolvimento. No entanto, pode pretender testar algumas operações utilizando PEP, tais como recolha de registos, porque este é o único método disponível para executar determinadas operações num ambiente sistemas integrada.

## <a name="access-the-privileged-endpoint"></a>Aceder ao ponto final com privilégios

Aceder a PEP através de uma sessão remota do PowerShell na máquina virtual que aloja o PEP. Esta máquina virtual com o nome no ASDK, **AzS ERCS01**. Se estiver a utilizar um sistema integrado, existem três instâncias de PEP, cada execução dentro de uma máquina virtual (*prefixo*-ERCS01, *prefixo*-ERCS02, ou *prefixo*- ERCS03) em diferentes anfitriões para resiliência. 

Antes de iniciar este procedimento para um sistema integrado, certifique-se de que pode aceder a PEP por endereço IP ou através de DNS. Após a implementação inicial de pilha do Azure, pode aceder a PEP apenas por endereço IP porque a integração do DNS não estiver ainda definida cópias de segurança. O fornecedor do hardware OEM irá fornecer-lhe um ficheiro JSON com o nome **AzureStackStampDeploymentInfo** que contenha os endereços IP de PEP.


> [!NOTE]
> Por motivos de segurança, Requeremos se ligar ao PEP apenas a partir de uma máquina de virtual protegida com sobre o anfitrião de ciclo de vida de hardware ou a partir de um computador dedicado, seguro, tal como um [estação de trabalho de acesso privilegiado](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations). A configuração original do anfitrião de ciclo de vida de hardware não têm de ser modificada da sua configuração original, incluindo a instalação de software novo, nem deve ser utilizada para estabelecer ligação com o PEP.

1. Estabelece a fidedignidade.

    - Num sistema integrado, execute o seguinte comando a partir de uma sessão elevada do Windows PowerShell para adicionar o PEP como um anfitrião fidedigno na máquina virtual protegida em execução no anfitrião de ciclo de vida de hardware ou a estação de trabalho de acesso privilegiado.

      ````PowerShell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ````
    - Se estiver a executar o ADSK, inicie sessão para o anfitrião do kit de desenvolvimento.

2. A máquina virtual protegida em execução no anfitrião de ciclo de vida de hardware ou a estação de trabalho de acesso privilegiado, abra uma sessão do Windows PowerShell. Execute os seguintes comandos para estabelecer uma sessão remota na máquina virtual que aloja o PEP:
 
    - Num sistema integrado:
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName <IP_address_of_ERCS> `
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ````
      O `ComputerName` parâmetro pode ser o endereço IP ou o nome DNS de uma das máquinas virtuais que aloja o PEP. 
    - Se estiver a executar o ADSK:
     
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName azs-ercs01 `
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ```` 
   Quando lhe for pedido, utilize as seguintes credenciais:

      - **Nome de utilizador**: Especifique a conta de CloudAdmin, no formato  **&lt; *domínio Azure pilha*&gt;\cloudadmin**. (Para ASDK, o nome de utilizador é **azurestack\cloudadmin**.)
      - **Palavra-passe**: introduza a mesma palavra-passe que foi fornecido durante a instalação para a conta de administrador de domínio AzureStackAdmin.

    > [!NOTE]
    > Se não for possível estabelecer ligação ao ponto final do ERCS, tente os passos uma e duas com o endereço IP de uma VM ERCS ao qual que ainda não já tentou estabelecer ligação.

3.  Depois de ligar, a linha de comandos será alterado para **[*nome do endereço IP ou ERCS VM*]: PS >** ou **[azs ercs01]: PS >**, consoante o ambiente. Aqui, execute `Get-Command` para ver a lista dos cmdlets disponíveis.

    Muitos destes cmdlets destinam-se apenas para ambientes de sistema integrada (por exemplo, os cmdlets relacionados com a integração do Centro de dados). No ASDK, ter foi validados os seguintes cmdlets:

    - Desmarque-anfitrião
    - Close-PrivilegedEndpoint
    - Exit-PSSession
    - Get-AzureStackLog
    - Get-AzureStackStampInformation
    - Get-Command
    - Get-FormatData
    - Get-Help
    - Get-ThirdPartyNotices
    - Objeto de medida
    - New-CloudAdminUser
    - Out-predefinido
    - Remove-CloudAdminUser
    - Select-Object
    - Set-CloudAdminUserPassword
    - Test-AzureStack
    - Stop-AzureStack
    - Get-ClusterLog

## <a name="tips-for-using-the-privileged-endpoint"></a>Sugestões para utilizar o ponto final com privilégios 

Tal como mencionado acima, o PEP é um [PowerShell JEA](https://docs.microsoft.com/powershell/jea/overview) ponto final. Ao fornecer uma camada de segurança forte, um ponto final JEA reduz algumas das capacidades básicas do PowerShell, como a conclusão do processamento de scripts ou separador. Se tentar qualquer tipo de operação de script, a operação falhar com o erro **ScriptsNotAllowed**. Este comportamento está previsto.

Por isso, por exemplo, para obter a lista de parâmetros para um cmdlet específico, executar o seguinte comando:

```PowerShell
    Get-Command <cmdlet_name> -Syntax
```

Em alternativa, pode utilizar o [Import-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Utility/Import-PSSession?view=powershell-5.1) cmdlet para importar todos os cmdlets do PEP para a sessão atual no seu computador local. Ao fazê-lo, todos os cmdlets e funções do PEP estão agora disponíveis no seu computador local, juntamente com a conclusão de separador e, mais em geral, processamento de scripts. 

Para importar a sessão PEP no seu computador local, efetue os seguintes passos:

1. Estabelece a fidedignidade.

    -Num sistema integrado, execute o seguinte comando a partir de uma sessão elevada do Windows PowerShell para adicionar o PEP como um anfitrião fidedigno na máquina virtual protegida em execução no anfitrião de ciclo de vida de hardware ou a estação de trabalho de acesso privilegiado.

      ````PowerShell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ````
    - Se estiver a executar o ADSK, inicie sessão para o anfitrião do kit de desenvolvimento.

2. A máquina virtual protegida em execução no anfitrião de ciclo de vida de hardware ou a estação de trabalho de acesso privilegiado, abra uma sessão do Windows PowerShell. Execute os seguintes comandos para estabelecer uma sessão remota na máquina virtual que aloja o PEP:
 
    - Num sistema integrado:
      ````PowerShell
        $cred = Get-Credential

        $session = New-PSSession -ComputerName <IP_address_of_ERCS> `
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ````
      O `ComputerName` parâmetro pode ser o endereço IP ou o nome DNS de uma das máquinas virtuais que aloja o PEP. 
    - Se estiver a executar o ADSK:
     
      ````PowerShell
       $cred = Get-Credential

       $session = New-PSSession -ComputerName azs-ercs01 `
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ```` 
   Quando lhe for pedido, utilize as seguintes credenciais:

      - **Nome de utilizador**: Especifique a conta de CloudAdmin, no formato  **&lt; *domínio Azure pilha*&gt;\cloudadmin**. (Para ASDK, o nome de utilizador é **azurestack\cloudadmin**.)
      - **Palavra-passe**: introduza a mesma palavra-passe que foi fornecido durante a instalação para a conta de administrador de domínio AzureStackAdmin.

3. Importe a sessão PEP para o seu computador local
    ````PowerShell 
        Import-PSSession $session
    ````
4. Agora, pode utilizar a conclusão de separador e fazer scripting como habitualmente na sua sessão do PowerShell local com todas as funções e os cmdlets de PEP, sem diminuir a postura de segurança da pilha do Azure. Divirta-se!


## <a name="close-the-privileged-endpoint-session"></a>Feche a sessão de ponto final com privilégios

 Conforme mencionado anteriormente, regista o PEP cada ação (e o resultado correspondente) que efetua na sessão do PowerShell. Tem de fechar a sessão utilizando o `Close-PrivilegedEndpoint` cmdlet. Este cmdlet corretamente fecha o ponto final e transfere os ficheiros de registo para uma partilha de ficheiros externos de retenção.

Para fechar a sessão de ponto final:

1. Crie uma partilha de ficheiro externo que esteja acessível ao PEP. Num ambiente de kit de desenvolvimento, apenas pode criar uma partilha de ficheiros no anfitrião do kit de desenvolvimento.
2. Execute o `Close-PrivilegedEndpoint` cmdlet. 
3. Lhe for pedida para um caminho no qual pretende armazenar o ficheiro de registo transcript. Especifique a partilha de ficheiros que criou anteriormente, no formato &#92; &#92; *servername*&#92;*sharename*. Se não especificar um caminho, o cmdlet falhar e a sessão permanece aberta. 

    ![Resultado do cmdlet fechar PrivilegedEndpoint que mostra onde pode Especifica o caminho de destino transcript](media/azure-stack-privileged-endpoint/closeendpoint.png)

Depois dos ficheiros de registo transcript são transferidos com êxito para a partilha de ficheiros, se estiver a automaticamente eliminados do PEP. 

> [!NOTE]
> Se fechar a sessão PEP utilizando os cmdlets `Exit-PSSession` ou `Exit`, ou apenas a fechar a consola do PowerShell, os registos de transcript não transferir para uma partilha de ficheiros. Permanecem no PEP. Da próxima vez que executar `Close-PrivilegedEndpoint` e incluir uma partilha de ficheiros, os registos de transcript do anterior sessões também irão transferir. Não utilize `Exit-PSSession` ou `Exit` para fechar a sessão PEP; utilize `Close-PrivilegedEndpoint` em vez disso.


## <a name="next-steps"></a>Passos Seguintes
[Ferramentas de diagnóstico de pilha do Azure](azure-stack-diagnostics.md)
