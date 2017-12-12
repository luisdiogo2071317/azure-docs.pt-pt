---
title: Telemetria de pilha do Azure | Microsoft Docs
description: "Descreve como configurar as definições de telemetria de pilha do Azure com o PowerShell."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/08/2017
ms.author: jeffgilb
ms.reviewer: comartin
ms.openlocfilehash: 5cd8d4045764b753c5fdd81ade98d69c72709881
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="azure-stack-telemetry"></a>Telemetria de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Dados de sistema de pilha do Azure ou a telemetria, é carregada automaticamente para a Microsoft via a experiência de utilizador ligado. Dados recolhidos a partir da telemetria de pilha do Azure são utilizados por equipas da Microsoft principalmente para melhorar o nosso experiências de cliente e para análise de segurança, estado de funcionamento, qualidade e o desempenho.

Como um operador de pilha do Azure, telemetria pode fornecer informações valiosas para implementações empresariais e dá-lhe uma voz que ajuda a versões futuras de forma da pilha do Azure.

> [!NOTE]
> Pilha do Azure também pode ser configurada para as informações de utilização direta para o Azure para faturação. Isto é necessário para clientes de pilha do Azure de vários nós que optem por pay-como-utiliza faturação. Relatórios de utilização é controlada de forma independente de telemetria e não são necessária para os clientes de vários nós que escolha o modelo de capacidade ou para os utilizadores do Kit de desenvolvimento de pilha do Azure. Nestes cenários, relatórios de utilização podem ser desativada [utilizando o script de registo](https://docs.microsoft.com/azure/azure-stack/azure-stack-usage-reporting). 

Telemetria de pilha do Azure baseia-se no componente telemetria e experiência de utilizador ligado Windows Server 2016, que utiliza o [rastreio de eventos para o Windows (ETW)](https://msdn.microsoft.com/library/dn904632(v=vs.85).aspx) tecnologia de registo para recolher e armazenar os dados e eventos de telemetria de rastreio. Componentes de pilha do Azure utilizam a mesma tecnologia de registo para publicar eventos e os dados que são recolhidos através do registo de eventos do sistema de operativo público e APIs de rastreio. Exemplos de componentes de pilha do Azure incluem o fornecedor de recursos de rede, o fornecedor de recursos de armazenamento, monitorização do fornecedor de recursos e fornecedor de recursos de atualização. O componente ligado experiência de utilizador e a telemetria encripta os dados através de SSL e utiliza a afixação de certificado para transmitir dados de telemetria através de HTTPS para o serviço de gestão de dados da Microsoft.

> [!NOTE]
> Para suportar o fluxo de dados de telemetria, a porta 443 (HTTPS) tem de ser aberta na sua rede. O componente ligado experiência de utilizador e a telemetria liga ao serviço de gestão de dados da Microsoft em https://v10.vortex-win.data.microsoft.com. O componente ligado experiência de utilizador e a telemetria também estabelece ligação ao https://settings-win.data.microsoft.com para transferir informações de configuração.

## <a name="privacy-considerations"></a>Considerações de privacidade
O serviço ETW encaminha os dados telemétricos para armazenamento na nuvem protegido. O princípio de menos privilegiado guias acesso a dados de telemetria. Equipa da Microsoft apenas com uma necessidade de negócio válido têm permissão de acesso para os dados de telemetria. A Microsoft não partilhar dados pessoais dos nossos clientes com terceiros, exceto critério do cliente ou para as finalidades limitadas descritas no [declaração de privacidade de pilha do Azure](http://windows.microsoft.com/windows/preview-privacy-statement). Iremos partilhar relatórios de negócio com OEMs e os parceiros que incluem informações de telemetria anónimos, agregadas. Decisões de partilha de dados que são efetuados por uma interna equipa da Microsoft, incluindo dados, legais e privacidade intervenientes de gestão.

A Microsoft Deteta no e práticas minimization de informações. Iremos esforçar-nos recolher apenas as informações que precisamos e armazenamos para apenas longa conforme necessário, a fornecer um serviço ou para análise. Muitas das informações sobre como o sistema de pilha do Azure e os serviços do Azure estão a funcionar é eliminada dentro de seis meses. Resumidos ou dados agregados serão mantidos para um período mais longo.

Estamos a compreender de que a privacidade e segurança das informações dos nossos clientes é muito importante.  Iremos efetuou uma abordagem abrangente e thoughtful a privacidade do cliente e a proteção de dados de clientes com a pilha do Azure. Os administradores de TI tem controlos para personalizar funcionalidades e definições de privacidade em qualquer altura. É claro nosso compromisso para transparência e fidedignidade:
- Estamos abrir com clientes sobre os tipos de dados, que podemos recolher.
- Colocarmos os clientes empresariais no controlo — podem personalizar as suas próprias definições de privacidade.
- Vamos colocar segurança e privacidade do cliente pela primeira vez.
- Estamos transparentes sobre telemetria obtém.
- Telemetria são utilizadas para melhorar as experiências de cliente.

Microsoft pretender recolher informações confidenciais, tais como números de cartão de crédito, nomes de utilizador e palavras-passe, endereços de e-mail ou outras informações confidenciais da mesma forma. Se determinadas informações confidenciais foram inadvertidamente recebidas, iremos eliminá-la.

## <a name="examples-of-how-microsoft-uses-the-telemetry-data"></a>Exemplos de como a Microsoft utiliza os dados de telemetria
Telemetria desempenha um papel importante ajudar-nos rapidamente identificar e corrigir problemas de fiabilidade crítico nas configurações e implementações os nossos clientes. Informações sobre os dados de telemetria que iremos recolher ajuda-na identificar rapidamente problemas com os serviços ou configurações de hardware. Capacidade da Microsoft para obter estes dados de clientes e os melhoramentos de unidade para o ecossistema ajuda a aumentar a barra para a qualidade dos nossas integradas soluções de pilha do Azure. 

Telemetria também ajuda a Microsoft para compreender melhor como os clientes implementar componentes, utilizar as funcionalidades e utilizar os serviços para atingir os seus objetivos de negócio. Obter informações do que os dados ajuda-na priorizar nosso investimentos engenharia nas áreas que podem afetar diretamente os nossos clientes experiências e cargas de trabalho.

Alguns exemplos incluem a utilização de cliente de contentores, armazenamento e as configurações de rede que estão associadas com funções de pilha do Azure. Também utilizamos o insights melhoramentos de unidade e intelligence para algumas das nossa gestão e monitorização de soluções.  Isto ajuda os clientes para diagnosticar problemas de qualidade e poupar dinheiro fazendo suporte menos chama para a Microsoft.

## <a name="manage-telemetry-collection"></a>Gerir a coleção de telemetria
Não recomendamos que ative a telemetria na sua organização como telemetria fornece dados de unidades de funcionalidade melhorada de produto e a estabilidade. Reconhecemos no entanto, que em alguns cenários Isto poderá ser necessário. 

Nestes casos, pode configurar o nível de telemetria enviado à Microsoft por utilizando a pré-implementação das definições de registo ou a implementação de post pontos finais de telemetria.

### <a name="asdk-set-telemetry-level-in-the-windows-registry"></a>ASDK: definir o nível de telemetria no registo do Windows
O Editor de registo do Windows é utilizado para definir o nível de telemetria manualmente no computador anfitrião físico antes de implementar a pilha do Azure. Se uma política de gestão já existe, por exemplo, a política de grupo, irá substituir esta definição de registo. 

Antes de implementar a pilha do Azure no anfitrião do kit de desenvolvimento, arrancar o CloudBuilder.vhdx e execute o seguinte script numa janela elevada do PowerShell:

```powershell
### Get current AllowTelmetry value on DVM Host
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
### Set & Get updated AllowTelemetry value for ASDK-Host 
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name "AllowTelemetry" -Value '0' # Set this value to 0,1,2,or3.  
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
```

Os níveis de telemetria são cumulativas e categorizadas em quatro níveis (0-3):

**0 (security)**. Dados de segurança. Proteger as informações necessárias para ajudar a manter o sistema operativo, incluindo dados sobre as definições de componente de telemetria e ligado a experiência de utilizador e o Windows Defender. Sem telemetria específica de pilha do Azure é emitida neste nível.

**1 (básico)**. Dados de segurança e os dados de estado de funcionamento básico e qualidade. Informações básicas de dispositivos, incluindo: dados relacionados com a qualidade, a compatibilidade de aplicações, dados de utilização de aplicações e dados a partir do nível de segurança. Definir o nível de telemetria a telemetria de pilha do Azure permite básico. Os dados recolhidos a este nível incluem:

- **Informações do dispositivo básico** que ajuda a fornece uma compreensão sobre os tipos e configurações de instâncias de Windows Server 2016 nativas e virtualizadas no ecossistema, incluindo:
 - Atributos de máquina, como o OEM, de modelo, 
 - Atributos de redes, tais como o número e a velocidade dos adaptadores de rede
 - Processador e atributos de memória, como o número de núcleos, tamanho de memória 
 - Atributos de armazenamento, como o número de unidades, tipo e tamanho.
- **Funcionalidade de telemetria**, incluindo carregar de percentagem de eventos foi carregados, eventos ignorados e o último tempo.
- **Relacionadas com qualidade contato** que ajuda a Microsoft a desenvolver uma compreensão básica do como está a efetuar a pilha do Azure. Um exemplo é a contagem de alertas críticos numa configuração de hardware específico.
- **Dados de compatibilidade** que ajuda a fornece conhecimento sobre quais os fornecedores de recursos são instaladas num sistema e a máquina virtual e identifica potenciais problemas de compatibilidade.

**2 (avançada)**. As informações adicionais, incluindo: como é que o sistema operativo e outros serviços de pilha do Azure são utilizadas, como executar, dados de fiabilidade avançadas e dados dos níveis básico e de segurança. 

**3 (completa)**. Todos os dados necessários para identificar e ajudar a corrigir problemas, mais dados a partir de **segurança**, **básico**, e **avançado** níveis.

> [!NOTE]
> O valor de nível de telemetria predefinido é 2 (avançada).

Tenha em atenção que desativar a telemetria do Windows e a pilha do Azure irá também desativar a telemetria do SQL Server. Para obter informações adicionais sobre as implicações de telemetria a Windows Server definições volte a referência a [Windows telemetria documento](https://aka.ms/winservtelemetry). 

> [!IMPORTANT]
> Estes níveis de telemetria só se aplicam aos componentes de pilha do Microsoft Azure. Componentes de software sem ser da Microsoft e serviços que estão em execução no anfitrião de ciclo de vida de Hardware de parceiros de hardware de pilha do Azure podem comunicar com os seus serviços em nuvem fora estes níveis de telemetria. Deverá trabalhar com o fornecedor de solução de hardware de pilha do Azure para compreender as respetivas políticas de telemetria e a forma como pode optar ativamente por participar no ou ativamente por não participar. 

### <a name="asdk-and-multinode-enable-or-disable-telemetry-after-deployment"></a>ASDK e MultiNode: Ativar ou desativar a telemetria após a implementação

Para ativar ou desativar a telemetria após a implementação, tem de ter acesso para o Privileged ponto final (PEP) que está exposta nas ERCS VMs.
1.  Para ativar:`Set-Telemetry -Enable`
2.  Para desativar o:`Set-Telemetry -Disable`

Detalhe de parâmetro: 
> . PARÂMETRO Enable - ativar carregamento de dados de telemetria 

> . PARÂMETRO Disable - desativar carregamento de dados de telemetria  

**Script para ativar a telemetria:**
```powershell
$ip = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
$psSession = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred
Invoke-Command -Session $psSession {Set-Telemetry -Enable}
if($psSession)
{
    Remove-PSSession $psSession
}
```

**Script para desativar a telemetria:**
```powershell
$ip = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
$psSession = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred
Invoke-Command -Session $psSession {Set-Telemetry -Disable}
if($psSession)
{
    Remove-PSSession $psSession
}
```

## <a name="next-steps"></a>Passos seguintes
[Transferir o pacote de implementação de kit de desenvolvimento de pilha do Azure](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Implementar o kit de desenvolvimento de pilha do Azure](azure-stack-run-powershell-script.md)

