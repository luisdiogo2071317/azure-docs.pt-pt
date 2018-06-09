---
title: Telemetria de pilha do Azure | Microsoft Docs
description: Descreve como configurar as definições de telemetria de pilha do Azure com o PowerShell.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/07/2018
ms.author: jeffgilb
ms.reviewer: comartin
ms.openlocfilehash: ed3f09f942bdaa803ae8024d5c02230173190107
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248202"
---
# <a name="azure-stack-telemetry"></a>Telemetria de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Telemetria de pilha do Azure carrega automaticamente dados de sistema para a Microsoft via a experiência de utilizador ligado. Equipas da Microsoft utilizam os dados que recolhe de telemetria de pilha do Azure para melhorar as experiências de cliente. Estes dados também são utilizados para segurança, estado de funcionamento, qualidade e análise de desempenho.

Para um operador de pilha do Azure, telemetria pode fornecer informações valiosas para implementações empresariais e dá-lhe uma voz que ajuda a versões futuras de forma da pilha do Azure.

> [!NOTE]
> Também pode configurar a pilha do Azure para reencaminhar as informações de utilização para o Azure para faturação. Isto é necessário para clientes de pilha do Azure de vários nós que optem por pay-como-utiliza faturação. Relatórios de utilização é controlada de forma independente de telemetria e não são necessária para os clientes de vários nós que escolha o modelo de capacidade ou para os utilizadores do Kit de desenvolvimento de pilha do Azure. Nestes cenários, relatórios de utilização podem ser desativada [utilizando o script de registo](https://docs.microsoft.com/azure/azure-stack/azure-stack-usage-reporting).

Telemetria de pilha do Azure baseia-se no componente telemetria e experiência de utilizador ligado Windows Server 2016, que utiliza o [rastreio de eventos para o Windows (ETW)](https://msdn.microsoft.com/library/dn904632(v=vs.85).aspx) TraceLogging tecnologia para recolher e armazenar os dados e de eventos. Componentes de pilha do Azure utilizam a mesma tecnologia para publicar eventos e os dados recolhidos através do registo de eventos do sistema de operativo público e APIs de rastreio. Exemplos destes componentes de pilha do Azure incluem estes fornecedores: recurso de atualização, monitorização de recursos, recursos de armazenamento e recursos de rede. O componente ligado experiência de utilizador e a telemetria encripta os dados através de SSL e utiliza a afixação de certificado para transmitir dados através de HTTPS para o serviço de gestão de dados da Microsoft.

> [!IMPORTANT]
> Para ativar o fluxo de dados de telemetria, a porta 443 (HTTPS) tem de ser aberta na sua rede. O componente ligado experiência de utilizador e a telemetria liga ao serviço de gestão de dados da Microsoft em https://v10.vortex-win.data.microsoft.com. O componente ligado experiência de utilizador e a telemetria liga-se também ao https://settings-win.data.microsoft.com para transferir informações de configuração.

## <a name="privacy-considerations"></a>Considerações de privacidade

O serviço ETW encaminha os dados telemétricos para armazenamento na nuvem protegido. O princípio do menor privilégio orienta o acesso aos dados de telemetria. Equipa da Microsoft apenas com uma necessidade de negócio válido recebem acesso aos dados de telemetria. Microsoft não partilhar os dados de cliente pessoal com terceiros, exceto critério do cliente ou para as finalidades limitadas descritas no [declaração de privacidade do Microsoft](https://privacy.microsoft.com/PrivacyStatement). Os relatórios de negócio que são partilhados com OEMs e parceiros incluem dados agregados, anónimos. Decisões de partilha de dados que são efetuados por uma interna equipa da Microsoft, incluindo dados, legais e privacidade intervenientes de gestão.

A Microsoft Deteta no e práticas minimization de informações. Iremos esforçar-nos recolher apenas as informações que não é necessário e armazenam-o para apenas desde necessário para fornecer um serviço ou para análise. Muitas das informações sobre como o sistema de pilha do Azure e os serviços do Azure estão a funcionar é eliminada dentro de seis meses. Resumidos ou dados agregados serão mantidos para um período mais longo.

Estamos a compreender de que a privacidade e segurança das informações de cliente é importante.  Microsoft assume uma abordagem abrangente e thoughtful privacidade do cliente e a proteção de dados de clientes na pilha do Azure. Os administradores de TI tem controlos para personalizar funcionalidades e definições de privacidade em qualquer altura. É claro nosso compromisso para transparência e fidedignidade:

- Estamos a abrir com clientes sobre os tipos de dados, que podemos recolher.
- Colocarmos os clientes empresariais no controlo — podem personalizar as suas próprias definições de privacidade.
- Vamos colocar segurança e privacidade do cliente pela primeira vez.
- Estamos transparentes sobre os dados telemétricos obtém.
- Utilizamos os dados telemétricos para melhorar as experiências de cliente.

Microsoft não a pretende recolher dados confidenciais, tais como números de cartão de crédito, nomes de utilizador e palavras-passe, endereços de e-mail ou informações confidenciais semelhantes. Se determinadas informações confidenciais foram inadvertidamente recebidas, iremos eliminá-la.

## <a name="examples-of-how-microsoft-uses-the-telemetry-data"></a>Exemplos de como a Microsoft utiliza os dados de telemetria

Telemetria desempenha um papel importante para ajudar a identificar e corrigir problemas de fiabilidade críticas em implementações de cliente e configurações rapidamente. Conhecimentos aprofundados sobre os dados telemétricos pode ajudar a identificar problemas com os serviços ou configurações de hardware. Capacidade da Microsoft para obter estes dados de clientes e os melhoramentos de unidade para a ecossistema, gera a barra para a qualidade das soluções de pilha do Azure integradas.

Telemetria também ajuda a Microsoft para compreender melhor como os clientes implementar componentes, utilizar as funcionalidades e utilizar os serviços para atingir os seus objetivos de negócio. Estas informações ajudam a definir prioridades de engenharia investimentos nas áreas que podem afetar diretamente as experiências de cliente e cargas de trabalho.

Alguns exemplos incluem a utilização de cliente de contentores, armazenamento e as configurações de rede que estão associadas com funções de pilha do Azure. Também utilizamos o insights melhoramentos de unidade e intelligence para soluções de monitorização e gestão de pilha do Azure. Estas melhorias tornar mais fácil para os clientes diagnosticar problemas e poupar dinheiro fazendo suporte menos chama para a Microsoft.

## <a name="manage-telemetry-collection"></a>Gerir a coleção de telemetria

Não suportamos recomendada de desativar a telemetria na sua organização. No entanto, em alguns cenários Isto poderá ser necessário.

Nestes cenários, pode configurar o nível de telemetria enviado à Microsoft através da utilização de definições de registo antes de implementar o Azure pilha ou utilizando os pontos finais de telemetria depois de implementar pilha do Azure.

### <a name="telemetry-levels-and-data-collection"></a>Telemetria níveis e recolha de dados

Antes de alterar as definições de telemetria, deve compreender os níveis de telemetria e os dados que são recolhidos a cada nível.

As definições de telemetria são agrupadas em quatro níveis (0-3), que são cumulativas e categorizadas como os seguintes passos:

**0 (segurança)**</br>
Dados de segurança. Informações necessárias para manter o sistema operativo segura. Isto inclui dados sobre as definições de componente de telemetria e experiência de utilizador ligado e o Windows Defender. Sem telemetria específica de pilha do Azure é emitida neste nível.

**1 (básico)**</br>
Dados de segurança e os dados de estado de funcionamento básico e qualidade. Informações básicas de dispositivos, incluindo: dados relacionados com a qualidade, a compatibilidade de aplicações, dados de utilização de aplicações e dados a partir de **segurança** nível. Definir o nível de telemetria a telemetria de pilha do Azure permite básico. Os dados recolhidos a este nível incluem:

- *Informações do dispositivo básico* que fornece uma compreensão sobre os tipos e configurações de instâncias de Windows Server 2016 native e virtuais do ecossistema do. Isto inclui:

  - Atributos de máquina, como o OEM e o modelo.
  - Atributos de redes, tais como o número de adaptadores de rede e os respetivos velocidade.
  - Atributos de processador e memória, como o número de núcleos e a quantidade de memória instalada.
  - Atributos de armazenamento, tais como o número de unidades, o tipo de unidade e o tamanho de unidade.

- *Funcionalidade de telemetria*, incluindo a percentagem de eventos carregados, eventos ignorados e os dados de último carregar tempo.
- *Informações relacionadas com a qualidade* que ajuda a Microsoft a desenvolver uma compreensão básica do como está a efetuar a pilha do Azure. Por exemplo, a contagem de alertas críticos numa configuração de hardware específico.
- *Dados de compatibilidade* que ajuda a fornece conhecimento sobre quais os fornecedores de recursos estão instalados num sistema e uma máquina virtual. Esta identifica potenciais problemas de compatibilidade.

**2 (avançado)**</br>
Informações adicionais, incluindo: como são utilizados os serviços de pilha do Azure e o sistema operativo, como executar estes serviços, fiabilidade avançada dados e dados a partir de **segurança** e **básico** níveis.

> [!NOTE]
> Esta é a predefinição de telemetria.

**3 (completa)**</br>
Todos os dados necessários para identificar e ajudar a corrigir problemas, mais dados a partir de **segurança**, **básico**, e **avançado** níveis.

> [!IMPORTANT]
> Estes níveis de telemetria só se aplicam aos componentes de pilha do Microsoft Azure. Componentes de software sem ser da Microsoft e serviços que estão em execução no anfitrião de ciclo de vida de Hardware de parceiros de hardware de pilha do Azure podem comunicar com os seus serviços em nuvem fora estes níveis de telemetria. Deverá trabalhar com o fornecedor de solução de hardware de pilha do Azure para compreender as respetivas políticas de telemetria e a forma como pode optar ativamente por participar no ou ativamente por não participar.

Desativar a telemetria do Windows e a pilha do Azure também desativa a telemetria do SQL Server. Para obter mais informações sobre as implicações das definições de telemetria do Windows Server, consulte o [Windows telemetria documento](https://aka.ms/winservtelemetry).

### <a name="asdk-set-the-telemetry-level-in-the-windows-registry"></a>ASDK: definir o nível de telemetria no registo do Windows

Pode utilizar o Editor de registo do Windows para definir manualmente o nível de telemetria no computador anfitrião físico antes de implementar pilha do Azure. Se uma política de gestão já existe, por exemplo, a política de grupo, esta substitui esta definição de registo.

Antes de implementar a pilha do Azure no anfitrião do kit de desenvolvimento, arrancar CloudBuilder.vhdx e execute o seguinte script numa janela elevada do PowerShell:

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

### <a name="asdk-and-multi-node-enable-or-disable-telemetry-after-deployment"></a>ASDK e com vários nós: Ativar ou desativar a telemetria após a implementação

Para ativar ou desativar a telemetria após a implementação, tem de ter acesso para o Privileged ponto final (PEP) que está exposta nas ERCS VMs.

1. Para ativar: `Set-Telemetry -Enable`
2. Para desativar o: `Set-Telemetry -Disable`

Detalhes de parâmetro:
> . PARÂMETRO Enable - ativar carregamento de dados de telemetria</br>
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

## <a name="next-steps"></a>Passos Seguintes

- [Transferir o pacote de implementação de kit de desenvolvimento de pilha do Azure](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

- [Implementar o kit de desenvolvimento de pilha do Azure](azure-stack-run-powershell-script.md)
