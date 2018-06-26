---
title: Como utilizar PerfInsights no Microsoft Azure | Microsoft Docs
description: Aprende como utilizar PerfInsights para resolver problemas de desempenho da VM do Windows.
services: virtual-machines-windows'
documentationcenter: ''
author: anandhms
manager: cshepard
editor: na
tags: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 2f496f906eef416b35e2e59b2db93481ce65acb1
ms.sourcegitcommit: e34afd967d66aea62e34d912a040c4622a737acb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36946538"
---
# <a name="how-to-use-perfinsights"></a>Como utilizar PerfInsights

[PerfInsights](http://aka.ms/perfinsightsdownload) é uma ferramenta de diagnóstico de ajuda autónoma que recolhe e analisa os dados de diagnóstico e fornece um relatório para ajudar a resolver problemas de desempenho do Windows máquina virtual no Azure. PerfInsights pode ser executado em máquinas virtuais como uma ferramenta autónoma ou diretamente a partir do portal através da instalação [extensão de VM de diagnóstico do Azure desempenho](performance-diagnostics-vm-extension.md).

Se ocorrerem problemas de desempenho com máquinas virtuais, antes de contactar o suporte, execute esta ferramenta.

## <a name="supported-troubleshooting-scenarios"></a>Suportado cenários de resolução de problemas

PerfInsights pode recolher e analisar vários tipos de informações. As secções seguintes abrangem os cenários comuns.

### <a name="quick-performance-analysis"></a>Análise de desempenho rápido

Este cenário recolhe a configuração do disco e outras informações importantes, incluindo:

-   Registos de eventos

-   Estado da rede para todas as ligações de entrada e saídas

-   Definições de configuração de rede e de firewall

-   Lista de tarefas para todas as aplicações que estão atualmente em execução no sistema

-   Definições de configuração de base de dados Microsoft SQL Server (se a VM é identificada como um servidor que está a executar o SQL Server)

-   Contadores de fiabilidade de armazenamento

-   Correções importantes do Windows

-   Controladores de filtro instalado

Esta é uma coleção passiva de informações que não devem afetar o sistema. 

>[!Note]
>Este cenário é automaticamente incluído em cada um dos seguintes cenários:

### <a name="benchmarking"></a>Direcionamento de caminhos

Este cenário é executado o [Diskspd](https://github.com/Microsoft/diskspd) teste benchmark (IOPS e MBPS) para todas as unidades que estão ligados à VM. 

> [!Note]
> Este cenário pode afetar o sistema e não deve ser executado num sistema de produção em direto. Se necessário, execute este cenário numa janela de manutenção dedicada para evitar quaisquer problemas. Maior carga de trabalho que é causada por um teste de rastreio ou benchmark pode afetar negativamente o desempenho da VM.
>

### <a name="slow-vm-analysis"></a>Análise VM lenta

Este cenário é executado um [contador de desempenho](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) rastreio, utilizando os contadores que são especificados no ficheiro RuleEngineConfig.json. Se a VM é identificada como um servidor que está a executar o SQL Server, é executado um rastreio de contador de desempenho. Isto é feito utilizando os contadores que se encontram no ficheiro RuleEngineConfig.json. Este cenário também inclui dados de diagnóstico de desempenho.

### <a name="azure-files-analysis"></a>Análise de ficheiros do Azure

Este cenário é executada uma captura de contador de desempenho especial, juntamente com um rastreio de rede. A captura inclui todos os os Server Message Block (SMB) cliente partilhas contadores. Seguem-se alguns chaves SMB cliente partilha contadores de desempenho que fazem parte de captura de:

| **Tipo**     | **Contador de partilhas de cliente do SMB** |
|--------------|-------------------------------|
| IOPS         | Pedidos de dados/seg             |
|              | Pedidos de leitura/seg             |
|              | Escrever pedidos/seg            |
| Latência      | Média de pedidos de dados/seg         |
|              | Média de seg/leitura                 |
|              | Média de seg/escrita                |
| Tamanho de e/s      | Média Pedido de bytes/dados       |
|              | Média Bytes/Read               |
|              | Média Bytes/escrita              |
| Débito   | Bytes de dados/seg                |
|              | Bytes lidos/seg                |
|              | Escrever Bytes/seg               |
| Comprimento da fila | Média Comprimento da fila de leitura        |
|              | Média Comprimento da fila de escrita       |
|              | Média Comprimento da fila de dados        |

### <a name="advanced-slow-vm-analysis"></a>Avançadas de análise VM lenta

Quando executar uma análise VM lenta avançada, selecione os rastreios para executar em paralelo. Se quiser, pode executá-los todos os (contador de desempenho, Xperf, rede e StorPort).  

> [!Note]
> Este cenário pode afetar o sistema e não deve ser executado num sistema de produção em direto. Se necessário, execute este cenário numa janela de manutenção dedicada para evitar quaisquer problemas. Maior carga de trabalho que é causada por um teste de rastreio ou benchmark pode afetar negativamente o desempenho da VM.
>

## <a name="what-kind-of-information-is-collected-by-perfinsights"></a>Que tipo de informações recolhido pelo PerfInsights?

Regista informações sobre a VM do Windows, discos ou configuração de agrupamentos de armazenamento, contadores de desempenho, e vários os rastreios são recolhidos. Depende do cenário de desempenho que está a utilizar. A tabela seguinte fornece detalhes:

|Dados recolhidos                              |  |  | Cenários de desempenho |  |  | |
|----------------------------------|----------------------------|------------------------------------|--------------------------|--------------------------------|----------------------|----------------------|
|                               | Análise de desempenho rápido | Direcionamento de caminhos | Análise VM lenta | Análise de ficheiros do Azure | Avançadas de análise VM lenta |
| Informações de registos de eventos       | Sim                        | Sim                                | Sim                      | Sim                  | Sim                  |
| Informações do sistema                | Sim                        | Sim                                | Sim                      | Sim                  | Sim                  |
| Mapa de volume                        | Sim                        | Sim                                | Sim                      | Sim                  | Sim                  |
| Mapa de disco                          | Sim                        | Sim                                | Sim                      | Sim                  | Sim                  |
| Tarefas em execução                     | Sim                        | Sim                                | Sim                      | Sim                  | Sim                  |
| Contadores de fiabilidade de armazenamento      | Sim                        | Sim                                | Sim                      | Sim                  | Sim                  |
| Informações de armazenamento               | Sim                        | Sim                                | Sim                      | Sim                  | Sim                  |
| Saída de fsutil                     | Sim                        | Sim                                | Sim                      | Sim                  | Sim                  |
| Informações do controlador de filtro                | Sim                        | Sim                                | Sim                      | Sim                  | Sim                  |
| Netstat saída                    | Sim                        | Sim                                | Sim                      | Sim                  | Sim                  |
| Configuração da rede             | Sim                        | Sim                                | Sim                      | Sim                  | Sim                  |
| Configuração da firewall            | Sim                        | Sim                                | Sim                      | Sim                  | Sim                  |
| Configuração do SQL Server          | Sim                        | Sim                                | Sim                      | Sim                  | Sim                  |
| Os rastreios de diagnóstico de desempenho *  | Sim                        | Sim                                | Sim                      | Sim                  | Sim                  |
| Rastreio de contador de desempenho * *      |                            |                                    | Sim                      |                      | Sim                  |
| Rastreio de contador do SMB * *              |                            |                                    |                          | Sim                  |                      |
| Rastreio de contador do SQL Server * *       |                            |                                    | Sim                      |                      | Sim                  |
| Rastreio XPerf                       |                            |                                    |                          |                      | Sim                  |
| Rastreio StorPort                    |                            |                                    |                          |                      | Sim                  |
| Rastreio de rede                     |                            |                                    |                          | Sim                  | Sim                  |
| Rastreio de benchmark de Diskspd ***       |                            | Sim                                |                          |                      |                      |
|       |                            |                         |                                                   |                      |                      |

### <a name="performance-diagnostics-trace-"></a>Rastreio de diagnóstico de desempenho (*)

Executa um motor baseado em regras em segundo plano para recolher dados e diagnosticar problemas de desempenho em curso. Atualmente são suportadas as seguintes regras:

- Regra de HighCpuUsage: Deteta períodos de utilização elevados da CPU e mostra os consumidores de utilização da CPU superiores durante os períodos.
- Regra de HighDiskUsage: Deteta períodos de utilização elevada de disco em discos físicos e mostra os consumidores de utilização de disco superior durante os períodos.
- Regra de HighResolutionDiskMetric: mostra métricas de latência IOPS, débito e e/s por 50 milissegundos para cada disco físico. Ajuda a identificar rapidamente os períodos de limitação de disco.
- Regra de HighMemoryUsage: Deteta períodos de utilização elevada da memória e mostra a memória superior consumidores de utilização durante os períodos.

> [!NOTE] 
> Atualmente, são suportadas versões do Windows que incluem o .NET Framework 3.5 ou versões posteriores.

### <a name="performance-counter-trace-"></a>Rastreio de contador de desempenho (*)

Recolhe os seguintes contadores de desempenho:

- \Process, \Processor, \Memory, \Thread, \PhysicalDisk e \LogicalDisk
- \Cache\Dirty páginas, \Cache\Lazy escrita esvaziamentos/seg, \Server\Pool na memória não paginável, falhas e falhas de \Server\Pool bloco paginado
- Selecionar contadores em \Network Interface, \IPv4\Datagrams \IPv6\Datagrams, \TCPv4\Segments, \TCPv6\Segments, \Network adaptador, \WFPv4\Packets, \WFPv6\Packets, \UDPv4\Datagrams, \UDPv6\Datagrams, \TCPv4\Connection, \TCPv6\Connection, \ Policy\Packets de QoS de rede, a atividade de cartão de Interface de rede de processador de \Per e \Microsoft Winsock BSP

#### <a name="for-sql-server-instances"></a>Para instâncias do SQL Server
- Gestor de servidor: memória intermédia \SQL, estatísticas de agrupamento \SQLServer:Resource e \SQLServer:SQL Statistics\
- \SQLServer:Locks, \SQLServer:General, estatísticas
- Métodos de \SQLServer:Access

#### <a name="for-azure-files"></a>Para ficheiros do Azure
Partilhas de cliente \SMB

### <a name="diskspd-benchmark-trace-"></a>Rastreio de benchmark de Diskspd (*)
Testes de carga de trabalho de e/s de Diskspd (disco do SO [escrita] e unidades de agrupamento [leitura/escrita])

## <a name="run-the-perfinsights-tool-on-your-vm"></a>Execute a ferramenta de PerfInsights na VM

### <a name="what-do-i-have-to-know-before-i-run-the-tool"></a>O que é necessário saber antes de posso executar a ferramenta? 

#### <a name="tool-requirements"></a>Requisitos de ferramenta

-  Esta ferramenta tem de ser executada na VM que tenha o problema de desempenho. 

-  São suportados os seguintes sistemas operativos: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016; Windows 8.1 e Windows 10.

#### <a name="possible-problems-when-you-run-the-tool-on-production-vms"></a>Possíveis problemas quando a ferramenta é executada em VMs de produção

-  Para o cenário benchmarking ou o cenário "Avançadas de análise de VM lenta" que está configurado para utilizar Xperf ou Diskspd, a ferramenta poderá afetar negativamente o desempenho da VM. Estes cenários não devem ser executados num ambiente de produção em direto.

-  Para o cenário benchmarking ou o cenário "Avançadas de análise de VM lenta" que está configurado para utilizar Diskspd, certifique-se de que nenhuma outra atividade em segundo plano interfere com a carga de trabalho de e/s.

-  Por predefinição, a ferramenta utiliza a unidade de armazenamento temporário para recolher dados. Se o rastreio permanece ativada durante mais tempo, a quantidade de dados que são recolhidos pode ser relevante. Isto pode reduzir a disponibilidade de espaço em disco temporário e, por conseguinte, pode afetar a qualquer aplicação que se baseie nesta unidade.

### <a name="how-do-i-run-perfinsights"></a>Como posso executar PerfInsights? 

Pode executar PerfInsights numa máquina virtual através da instalação [extensão de VM de diagnóstico do Azure desempenho](performance-diagnostics-vm-extension.md). Também pode executá-la como uma ferramenta autónoma. 

**Instalar e executar PerfInsights do portal do Azure**

Para obter mais informações sobre esta opção, consulte [instalar Azure desempenho VM extensão de diagnóstico](performance-diagnostics-vm-extension.md#install-the-extension).  

**Executar PerfInsights no modo autónomo**

Para executar a ferramenta de PerfInsights, siga estes passos:


1. Transferir [PerfInsights.zip](http://aka.ms/perfinsightsdownload).

2. O ficheiro PerfInsights.zip de desbloqueio. Para tal, o ficheiro PerfInsights.zip com o botão direito e selecione **propriedades**. No **geral** separador, selecione **desbloqueio**e, em seguida, selecione **OK**. Isto garante que a ferramenta é executada sem avisos adicionais de segurança.  

    ![Captura de ecrã de PerfInsights propriedades, com o desbloqueio realçado](media/how-to-use-perfInsights/unlock-file.png)

3.  Expandir o ficheiro PerfInsights.zip comprimido para a unidade temporária (por predefinição, este é normalmente a unidade D). 

4.  Abra a linha de comandos do Windows como administrador e, em seguida, executar PerfInsights.exe para ver os parâmetros de linha de comandos disponíveis.

    ```
    cd <the path of PerfInsights folder>
    PerfInsights
    ```
    ![Saída de uma linha de comando de captura de ecrã de PerfInsights](media/how-to-use-perfInsights/PerfInsightsCommandline.png)
    
    A sintaxe básica para a execução de cenários de PerfInsights é:
    
    ```
    PerfInsights /run <ScenarioName> [AdditionalOptions]
    ```

    Pode utilizar o exemplo a execução lento VM cenário para 5 minutos abaixo:
    
    ```
    PerfInsights /run vmslow /d 300 /AcceptDisclaimerAndShareDiagnostics
    ```

    Pode utilizar o exemplo seguinte para executar o cenário avançado com rastreios de contador de desempenho e Xperf para 5 minutos:
    
    ```
    PerfInsights /run advanced xp /d 300 /AcceptDisclaimerAndShareDiagnostics
    ```

    Pode utilizar o exemplo para executar o cenário de VM lento de 5 minutos e carregar o ficheiro zip do resultado para a conta de armazenamento abaixo:
    
    ```
    PerfInsights /run vmslow /d 300 /AcceptDisclaimerAndShareDiagnostics /sa <StorageAccountName> /sk <StorageAccountKey>
    ```

    Pode ver todos os cenários disponíveis e as opções utilizando o **/lista** comando:
    
    ```
    PerfInsights /list
    ```

    >[!Note]
    >Antes de executar um cenário, PerfInsights pede ao utilizador para aceitar a partilha de informações de diagnóstico e para aceitar o EULA. Utilize **/AcceptDisclaimerAndShareDiagnostics** opção para ignorar estes pedidos. 
    >
    >Se tiver um pedido de suporte de Active Directory com a Microsoft e em execução PerfInsights por pedido de estiver a trabalhar com o engenheiro de suporte, certifique-se de fornecer o suporte permissão número utilizando o **/sr** opção.
    >
    >Por predefinição, PerfInsights irá tentar atualizar-se para a versão mais recente, se disponível. Utilize **/SkipAutoUpdate** ou **/sau** parâmetro para ignorar a atualização automática.  
    >
    >Se a duração **/d** não for especificado, PerfInsights irá solicitar-lhe para repro o problema ao executar vmslow, azurefiles e cenários avançados. 

Quando os rastreios nem de operações estiverem concluídas, é apresentado um novo ficheiro na mesma pasta que PerfInsights. O nome do ficheiro é **CollectedData\_aaaa-MM-dd\_hh-mm-ss-fff.zip.** Pode enviar este ficheiro para o agente de suporte para o Analysis Services ou abrir o relatório dentro do ficheiro zip para rever findings e recomendações.

## <a name="review-the-diagnostics-report"></a>Reveja o relatório de diagnóstico

Dentro do **CollectedData\_aaaa-MM-dd\_hh-mm-ss-fff.zip** ficheiro, pode encontrar um relatório HTML que explica em detalhe findings de PerfInsights. Para rever o relatório, expanda o **CollectedData\_aaaa-MM-dd\_hh-mm-ss-fff.zip** de ficheiros e, em seguida, abra o **PerfInsights Report.html** ficheiro.

Selecione o **Findings** separador.

![Captura de ecrã do relatório PerfInsights](media/how-to-use-perfInsights/findingtab.png)
![captura de ecrã do relatório PerfInsights](media/how-to-use-perfInsights/findings.PNG)

> [!NOTE] 
> Se os problemas que poderão causar problemas de desempenho conhecidos findings categorizados como elevada. Findings categorizado como média representam não ideal configurações que não necessariamente a causar problemas de desempenho. Findings categorizados como baixa são apenas as instruções informativos.

Reveja as recomendações e ligações para todas as findings médias e alta. Saiba mais sobre como podem afetar o desempenho bem como sobre as melhores práticas para configurações de desempenho otimizado.

### <a name="storage-tab"></a>Separador de armazenamento

O **Findings** secção apresenta várias recomendações relacionadas com o armazenamento e findings.

O **disco mapa** e **Volume mapa** secções descrevem como lógica volumes e físico discos estão relacionados uns aos outros.

Numa perspetiva de disco físico (mapa de disco), a tabela mostra todos os volumes lógicos que estão em execução no disco. No exemplo seguinte, **PhysicalDrive2** executa dois volumes lógicos criados em várias partições (J e H):

![Captura de ecrã do separador de disco](media/how-to-use-perfInsights/disktab.png)

Numa perspetiva de volume (mapa de Volume), as tabelas apresentam todos os discos físicos em cada volume lógico. Tenha em atenção que para discos RAID/dinâmico, poderá executar a um volume lógico em vários discos físicos. No exemplo seguinte, *c:\\montar* é um ponto de montagem configurado como *SpannedDisk* em discos físicos, 2 e 3:

![Captura de ecrã do separador de volume](media/how-to-use-perfInsights/volumetab.png)

### <a name="sql-tab"></a>Separador SQL

Se o destino da VM aloja quaisquer instâncias do SQL Server, consulte um separador adicional no relatório, com o nome **SQL**:

![Separador de captura de ecrã do SQL Server](media/how-to-use-perfInsights/sqltab.png)

Esta secção contém um **Findings** separador e separadores adicionais para cada uma das instâncias do SQL Server alojadas na VM.

O **Findings** separador contém uma lista de todos os SQL relacionadas com problemas de desempenho encontrados, juntamente com as recomendações.

No exemplo seguinte, **PhysicalDrive0** (em execução a unidade C) é apresentado. Isto acontece porque tanto o **modeldev** e **modellog** ficheiros estão localizados na unidade C e são de diferentes tipos de (por exemplo, o registo de transação e o ficheiro de dados, respetivamente).

![Captura de ecrã das informações de registo](media/how-to-use-perfInsights/loginfo.png)

Os separadores para instâncias específicas do SQL Server contém uma secção geral que apresenta informações básicas sobre a instância selecionada. Os separadores também contenham secções adicionais para as informações avançadas, incluindo opções de utilizador, as configurações e definições.

### <a name="diagnostic-tab"></a>Separador Diagnóstico
O **diagnóstico** separador contém informações sobre principais consumidores de memória, disco e da CPU no computador durante a execução de PerfInsights. Também pode encontrar informações sobre patches crítico que o sistema poderá estar em falta, a lista de tarefas e eventos de sistema importantes. 

## <a name="references-to-the-external-tools-used"></a>Referências para as ferramentas externas utilizadas

### <a name="diskspd"></a>Diskspd

O Diskspd é um armazenamento de carga e o desempenho do gerador de ferramenta de teste da Microsoft. Para obter mais informações, consulte [Diskspd](https://github.com/Microsoft/diskspd).

### <a name="xperf"></a>XPerf

XPerf é uma ferramenta da linha de comandos para capturar rastreios a partir do Toolkit de desempenho do Windows. Para obter mais informações, consulte [Toolkit de desempenho do Windows – Xperf](https://blogs.msdn.microsoft.com/ntdebugging/2008/04/03/windows-performance-toolkit-xperf/).

## <a name="next-steps"></a>Passos Seguintes

Pode carregar registos de diagnóstico e de relatórios ao Microsoft Support para uma revisão mais aprofundada. O suporte poderá pedir que transmitir o resultado que é gerado pelo PerfInsights para ajudá-lo com o processo de resolução de problemas.

A seguinte captura de ecrã mostra uma mensagem semelhante ao que poderá receber:

![Captura de ecrã da mensagem de exemplo do Support da Microsoft](media/how-to-use-perfInsights/supportemail.png)

Siga as instruções na mensagem para aceder à área de trabalho de transferência de ficheiros. Para segurança adicional, tem de alterar a palavra-passe na primeira utilização.

Depois de iniciar sessão, irá encontrar uma caixa de diálogo para carregar o **CollectedData\_aaaa-MM-dd\_hh-mm-ss-fff.zip** ficheiro que foi recolhido por PerfInsights.
