---
title: Diagnóstico no Azure Stack
description: Como recolher ficheiros de registo para obter um diagnóstico no Azure Stack
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 08/22/2018
ms.author: jeffgilb
ms.reviewer: adshar
ms.openlocfilehash: a36609ae63351070bb28469d9ccf1f3deb7bc6ff
ms.sourcegitcommit: a62cbb539c056fe9fcd5108d0b63487bd149d5c3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/22/2018
ms.locfileid: "42616954"
---
# <a name="azure-stack-diagnostics-tools"></a>Ferramentas de diagnóstico do Azure Stack

O Azure Stack é uma grande coleção de componentes a trabalhar em conjunto e interagir entre si. Todos estes componentes geram seus próprios registos exclusivos. Isso pode tornar o diagnóstico de problemas uma tarefa desafiadora, especialmente para erros provenientes de múltiplos, interação de componentes do Azure Stack. 

Nossas ferramentas de diagnóstico ajudam a garantir que o mecanismo de coleta de log é fácil e eficiente. O diagrama seguinte mostra como fazer logon ferramentas de coleta em projetos de Azure Stack:

![Ferramentas de diagnóstico do Azure Stack](media/azure-stack-diagnostics/get-azslogs.png)
 
 
## <a name="trace-collector"></a>Recoletor do rastreio
 
O Recoletor do rastreio está ativado por predefinição e seja executada continuamente em segundo plano para recolher todos os registos de rastreio de eventos para Windows (ETW) de serviços de componentes do Azure Stack. Os registos ETW são armazenados numa partilha local comum com um limite de idade de cinco dias. Quando o limite é alcançado, os ficheiros mais antigos são eliminados à medida que são criados novos. O tamanho máximo de predefinição permitido para cada arquivo é 200 MB. Uma verificação de tamanho ocorre a cada 2 minutos, e se o ficheiro atual for > = 200 MB, é guardado e um novo ficheiro é gerado. Também existe um limite de 8 GB no tamanho total do ficheiro gerado por sessão do evento. 

## <a name="log-collection-tool"></a>Ferramenta de registo de coleção
 
O cmdlet do PowerShell **Get-AzureStackLog** pode ser utilizado para recolher registos de todos os componentes num ambiente do Azure Stack. Salva-os nos arquivos zip numa localização definido pelo utilizador. Se a equipe de suporte técnico do Azure Stack tem os seus registos para ajudar a resolver um problema, eles poderão pedir-lhe para executar esta ferramenta.

> [!CAUTION]
> Estes ficheiros de registo podem conter informações de identificação pessoal (PII). Tenha isso em conta antes de publicar publicamente quaisquer ficheiros de registo.
 
Seguem-se alguns tipos de registo de exemplo que são recolhidos:
*   **Registos de implementação do Azure Stack**
*   **Registos de eventos do Windows**
*   **Registos de Panther**
*   **Registos do cluster**
*   **Registos de diagnóstico de armazenamento**
*   **Registos ETW**

Estes ficheiros são recolhidos e guardados numa partilha pelo coletor de rastreio. O **Get-AzureStackLog** cmdlet do PowerShell, em seguida, pode ser utilizado para recolhê-los quando necessário.

### <a name="to-run-get-azurestacklog-on-azure-stack-integrated-systems"></a>Para executar Get-AzureStackLog no Azure Stack de sistemas integrados 
Para executar a ferramenta de coleção de log num sistema integrado, tem de ter acesso ao ponto final com privilégios (PEP). Eis um script de exemplo, pode executar usando o PEP para recolher registos num sistema integrado:

```powershell
$ip = "<IP ADDRESS OF THE PEP VM>" # You can also use the machine name instead of IP here.
 
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
 
$shareCred = Get-Credential
 
$s = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred

$fromDate = (Get-Date).AddHours(-8)
$toDate = (Get-Date).AddHours(-2)  #provide the time that includes the period for your issue
 
Invoke-Command -Session $s {    Get-AzureStackLog -OutputSharePath "<EXTERNAL SHARE ADDRESS>" -OutputShareCredential $using:shareCred  -FilterByRole Storage -FromDate $using:fromDate -ToDate $using:toDate}

if($s)
{
    Remove-PSSession $s
}
```

- Os parâmetros **OutputSharePath** e **OutputShareCredential** são utilizados para carregar os registos para uma pasta partilhada externa.
- Conforme mostrado no exemplo anterior, o **FromDate** e **ToDate** parâmetros podem ser utilizados para recolher registos de um período de tempo específico. Isso pode ser útil para cenários como recolher registos após a aplicação de um pacote de atualização num sistema integrado.


 
### <a name="to-run-get-azurestacklog-on-an-azure-stack-development-kit-asdk-system"></a>Para executar Get-AzureStackLog num sistema de Azure Stack Development Kit (ASDK)
1. Inicie sessão como **AzureStack\CloudAdmin** no anfitrião.
2. Abra uma janela do PowerShell como administrador.
3. Executar o **Get-AzureStackLog** cmdlet do PowerShell.

**Exemplos:**

  Recolha todos os registos para todas as funções:

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs
  ```

  Recolha registos de funções VirtualMachines e bare-metal:

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal
  ```

  Recolha registos de funções VirtualMachines e bare-metal, com data de filtragem para ficheiros de registo para as últimos 8 horas:
    
  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8)
  ```

  Recolha registos de funções VirtualMachines e bare-metal, com data de filtragem para ficheiros de registo para o período de tempo entre 8 horas atrás e 2 horas atrás:

  ```powershell
  Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date).AddHours(-2)
  ```

### <a name="parameter-considerations-for-both-asdk-and-integrated-systems"></a>Considerações de parâmetro para ASDK e sistemas integrados

- Se o **FromDate** e **ToDate** não foram especificados parâmetros, os registos são recolhidos para as últimos quatro horas por predefinição.
- Utilize o **FilterByNode** parâmetro para filtrar registos por nome do computador. Por exemplo: ```Get-AzureStackLog -OutputPath <path> -FilterByNode azs-xrp01```
- Utilize o **FilterByLogType** parâmetro para filtrar registos por tipo. Pode optar por filtrar por ficheiro, a partilha ou WindowsEvent. Por exemplo: ```Get-AzureStackLog -OutputPath <path> -FilterByLogType File```
- Pode utilizar o **TimeOutInMinutes** parâmetro para definir o tempo limite para a recolha de registos. Ele é definido como 150 (2,5 horas) por predefinição.
- Na versão 1805 e posterior, a recolha de informação de registo de ficheiros está desativada por predefinição. Para ativá-la, utilize o **IncludeDumpFile** mudar o parâmetro. 
- Atualmente, pode utilizar o **FilterByRole** parâmetro para a recolha de registos de filtro para as seguintes funções:

 |   |   |   |    |
 | - | - | - | -  |   
 |ACS|Computação|InfraServiceController|QueryServiceCoordinator|
 |ACSBlob|CPI|Infraestrutura|QueryServiceWorker|
 |ACSDownloadService|CRP|KeyVaultAdminResourceProvider|SeedRing|
 |ACSFabric|DatacenterIntegration|KeyVaultControlPlane|SeedRingServices|
 |ACSFrontEnd|DeploymentMachine|KeyVaultDataPlane|SLB|
 |ACSMetrics|DiskRP|KeyVaultInternalControlPlane|SlbVips|
 |ACSMigrationService|Domínio|KeyVaultInternalDataPlane|SQL|
 |ACSMonitoringService|ECE|KeyVaultNamingService|SRP|
 |ACSSettingsService|EventAdminRP|MDM|Armazenamento|
 |ACSTableMaster|EventRP|MetricsAdminRP|StorageAccounts|
 |ACSTableServer|ExternalDNS|MetricsRP|StorageController|
 |ACSWac|Recursos de infraestrutura|MetricsServer|Inquilino|
 |ADFS|FabricRing|MetricsStoreService|TraceCollector|
 |ApplicationController|FabricRingServices|MonAdminRP|URP|
 |ASAppGateway|FirstTierAggregationService|MonitoringAgent|Utilização|
 |AzureBridge|FRP|MonRP|UsageBridge|
 |AzureMonitor|Galeria|NC|virtualMachines|
 |AzureStackBitlocker|Gateway|Rede|FOI|
 |BareMetal|HealthMonitoring|NonPrivilegedAppGateway|WASBootstrap|
 |BRP|HintingServiceV2|NRP|WASPUBLIC|
 |AC|HRP|OboService|WindowsDefender|
 |CacheService|IBC|OEM|     |
 |Nuvem|identityProvider|OnboardRP|     |   
 |Cluster|iDns|PXE|     |
 |   |   |   |    |


### <a name="bkmk_gui"></a>Recolher registos com uma interface gráfica do usuário
Em vez de fornecer os parâmetros necessários para o cmdlet Get-AzureStackLog obter os registos do Azure Stack, também pode aproveitar as ferramentas do Azure Stack de código-fonte aberto disponível localizadas no repositório de ferramentas de GitHub de ferramentas do principal do Azure Stack em http://aka.ms/AzureStackTools.

O **ERCS_AzureStackLogs.ps1** script do PowerShell é armazenado no repositório de ferramentas do GitHub e é atualizado regularmente. Para garantir que tem a versão mais recente disponível, deve baixá-lo diretamente a partir do http://aka.ms/ERCS. Iniciada a partir de uma sessão administrativa do PowerShell, o script liga-se para o ponto final com privilégios e executa o Get-AzureStackLog com parâmetros fornecidos. Não se for fornecido nenhum parâmetro, o script por predefinição, a pedir para os parâmetros por meio de uma interface gráfica do usuário.

Para saber mais sobre o script do ERCS_AzureStackLogs.ps1 PowerShell, pode assistir [um breve vídeo](https://www.youtube.com/watch?v=Utt7pLsXEBc) ou ver o script [ficheiro Leia-me](https://github.com/Azure/AzureStack-Tools/blob/master/Support/ERCS_Logs/ReadMe.md) localizado no repositório do GitHub de ferramentas do Azure Stack. 

### <a name="additional-considerations"></a>Considerações adicionais

* O comando demora algum tempo para ser executado com base no qual estão a recolher os registos de funções. Fatores também incluem a duração de tempo especificada para a recolha de registos e os números de nós no ambiente do Azure Stack.
* Como iniciar a sessão de execuções de coleção, verifique a nova pasta criada no **OutputSharePath** parâmetro especificado no comando.
* Cada função tem os respetivos registos de dentro de arquivos zip individuais. Dependendo do tamanho dos registos recolhidos, uma função pode ter seus registos divididos em vários arquivos zip. Para uma função, se quiser que todos os arquivos de log descompactei para uma única pasta, utilize uma ferramenta que pode descompactar em massa (por exemplo, 7zip). Selecione todos os arquivos compactados para a função e selecione **extrair aqui**. Isso unzips todos os ficheiros de registo para essa função numa única pasta intercalada.
* Um arquivo chamado **Get-AzureStackLog_Output.log** também é criado na pasta que contém os ficheiros de registo zipados. Este ficheiro é um log de saída do comando, o que pode ser utilizado para resolução de problemas durante a recolha de registos. Por vezes, o ficheiro de registo inclui `PS>TerminatingError` entradas que podem ser ignoradas com segurança, a menos que os ficheiros de registo esperado estão em falta após as execuções de coleção de registo.
* Para investigar uma falha específica, os registos podem ser necessárias de mais de um componente.
    -   Sistema e registos de eventos para todas as VMs de infraestrutura são recolhidos no *VirtualMachines* função.
    -   Sistema e registos de eventos para todos os anfitriões são recolhidos no *bare-metal* função.
    -   Registos de eventos de Cluster de ativação pós-falha e Hyper-V são recolhidos no *armazenamento* função.
    -   Registos de ACS são recolhidos no *armazenamento* e *ACS* funções.

> [!NOTE]
> Limites de tamanho e age são impostos no registos recolhidos conforme é essencial para garantir uma utilização eficiente do seu espaço de armazenamento para garantir que não obtém inundado de registos. No entanto, quando diagnosticar o problema, às vezes, terá de registos que poderão já não existir devido a estes limites. Portanto, é **altamente recomendado** que descarregar os registos para um espaço de armazenamento externo (uma conta de armazenamento no Azure, um dispositivo de armazenamento adicionais no local etc.) a cada 8 a 12 horas e mantenha-os lá para 1 a 3 meses, dependendo da sua requisitos. Além disso, certifique-se que esta localização de armazenamento está encriptada.

## <a name="next-steps"></a>Passos Seguintes
[Resolução de problemas do Microsoft Azure Stack](azure-stack-troubleshooting.md)

