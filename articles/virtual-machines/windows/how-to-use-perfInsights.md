---
title: Como utilizar PerfInsights no Microsoft Azure | Microsoft Docs
description: Aprende como utilizar PerfInsights para resolver problemas de desempenho da VM do Windows.
services: virtual-machines-windows'
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: f15875610e2035c6f4c10c36e19c02f3e045b3ea
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2017
---
# <a name="how-to-use-perfinsights"></a>Como utilizar PerfInsights 

[PerfInsights](http://aka.ms/perfinsightsdownload) é um script automatizado que recolhe informações de diagnóstico úteis. Também executa cargas de esforço de e/s e fornece um relatório de análise para ajudar a resolver problemas de desempenho do Windows máquina virtual no Azure. Isto pode ser executado em máquinas virtuais como um script de autónomo ou diretamente a partir do portal através da instalação [extensão de VM de diagnóstico do Azure desempenho](performance-diagnostics-vm-extension.md).

Se ocorrerem problemas de desempenho com máquinas virtuais, antes de contactar o suporte, execute este script.

## <a name="supported-troubleshooting-scenarios"></a>Suportado cenários de resolução de problemas

PerfInsights pode recolher e analisar vários tipos de informações. As secções seguintes abrangem os cenários comuns.

### <a name="collect-basic-configuration"></a>Recolher configuração básica 

Este cenário recolhe a configuração do disco e outras informações importantes, incluindo:

-   Registos de eventos

-   Estado da rede para todas as ligações de entrada e saídas

-   Definições de configuração de rede e de firewall

-   Lista de tarefas para todas as aplicações que estão atualmente em execução no sistema

-   Ficheiro de informações criado pela msinfo32 para a máquina virtual

-   Definições de configuração de base de dados Microsoft SQL Server (se a VM é identificada como um servidor que está a executar o SQL Server)

-   Contadores de fiabilidade de armazenamento

-   Correções importantes do Windows

-   Controladores de filtro instalado

Esta é uma coleção passiva de informações que não devem afetar o sistema. 

>[!Note]
>Este cenário é automaticamente incluído em cada um dos seguintes cenários.

### <a name="benchmarking"></a>Direcionamento de caminhos

Este cenário é executado o [Diskspd](https://github.com/Microsoft/diskspd) teste benchmark (IOPS e MBPS) para todas as unidades que estão ligados à VM. 

> [!Note]
> Este cenário pode afetar o sistema e não deve ser executado num sistema de produção em direto. Se necessário, execute este cenário numa janela de manutenção dedicada para evitar quaisquer problemas. Maior carga de trabalho que é causada por um teste de rastreio ou benchmark pode afetar negativamente o desempenho da VM.
>

### <a name="slow-vm-analysis"></a>Análise VM lenta 

Este cenário é executado um [contador de desempenho](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) rastreio, utilizando os contadores que são especificados no ficheiro Generalcounters.txt. Se a VM é identificada como um servidor que está a executar o SQL Server, é executado um rastreio de contador de desempenho. Isto é feito utilizando os contadores que se encontram no ficheiro Sqlcounters.txt e também inclui dados de diagnóstico de desempenho.

### <a name="slow-vm-analysis-and-benchmarking"></a>Análise VM lenta e direcionamento de caminhos

Este cenário é executado um [contador de desempenho](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) rastreio que é seguido um [Diskspd](https://github.com/Microsoft/diskspd) teste benchmark. 

> [!Note]
> Este cenário pode afetar o sistema e não deve ser executado num sistema de produção em direto. Se necessário, execute este cenário numa janela de manutenção dedicada para evitar quaisquer problemas. Maior carga de trabalho que é causada por um teste de rastreio ou benchmark pode afetar negativamente o desempenho da VM.
>

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
|              | Média Bytes/leitura               |
|              | Média Bytes/escrita              |
| Débito   | Bytes de dados/seg                |
|              | Bytes lidos/seg                |
|              | Escrever Bytes/seg               |
| Comprimento da fila | Média Comprimento da fila de leitura        |
|              | Média Comprimento da fila de escrita       |
|              | Média Comprimento da fila de dados        |

### <a name="custom-slow-vm-analysis"></a>Análise VM lenta personalizado 

Quando executar uma análise VM lenta personalizada, selecione os rastreios para executar em paralelo. Pode executá-los todos os (contador de desempenho, Xperf, rede e StorPort) se quiser. O rastreio esteja concluída, a ferramenta é executada Diskspd benchmark, se estiver selecionada. 

> [!Note]
> Este cenário pode afetar o sistema e não deve ser executado num sistema de produção em direto. Se necessário, execute este cenário numa janela de manutenção dedicada para evitar quaisquer problemas. Maior carga de trabalho que é causada por um teste de rastreio ou benchmark pode afetar negativamente o desempenho da VM.
>

## <a name="what-kind-of-information-is-collected-by-the-script"></a>Que tipo de informações recolhido pelo script?

Regista informações sobre a VM do Windows, discos ou configuração de agrupamentos de armazenamento, contadores de desempenho, e vários os rastreios são recolhidos. Depende do cenário de desempenho que está a utilizar. A tabela seguinte fornece detalhes:

|Dados recolhidos                              |  |  | Cenários de desempenho |  |  | |
|----------------------------------|----------------------------|------------------------------------|--------------------------|--------------------------------|----------------------|----------------------|
|                              | Recolher configuração básica | Direcionamento de caminhos | Análise VM lenta | Análise VM lenta e direcionamento de caminhos | Análise de ficheiros do Azure | Análise VM lenta personalizado |
| Informações de registos de eventos      | Sim                        | Sim                                | Sim                      | Sim                            | Sim                  | Sim                  |
| Informações do sistema               | Sim                        | Sim                                | Sim                      | Sim                            | Sim                  | Sim                  |
| Mapa de volume                       | Sim                        | Sim                                | Sim                      | Sim                            | Sim                  | Sim                  |
| Mapa de disco                         | Sim                        | Sim                                | Sim                      | Sim                            | Sim                  | Sim                  |
| Tarefas em execução                    | Sim                        | Sim                                | Sim                      | Sim                            | Sim                  | Sim                  |
| Contadores de fiabilidade de armazenamento     | Sim                        | Sim                                | Sim                      | Sim                            | Sim                  | Sim                  |
| Informações de armazenamento              | Sim                        | Sim                                | Sim                      | Sim                            | Sim                  | Sim                  |
| Saída de fsutil                    | Sim                        | Sim                                | Sim                      | Sim                            | Sim                  | Sim                  |
| Informações do controlador de filtro               | Sim                        | Sim                                | Sim                      | Sim                            | Sim                  | Sim                  |
| Netstat saída                   | Sim                        | Sim                                | Sim                      | Sim                            | Sim                  | Sim                  |
| Configuração da rede            | Sim                        | Sim                                | Sim                      | Sim                            | Sim                  | Sim                  |
| Configuração da firewall           | Sim                        | Sim                                | Sim                      | Sim                            | Sim                  | Sim                  |
| Configuração do SQL Server         | Sim                        | Sim                                | Sim                      | Sim                            | Sim                  | Sim                  |
| Os rastreios de diagnóstico de desempenho * | Sim                        | Sim                                | Sim                      |                                | Sim                  | Sim                  |
| Rastreio de contador de desempenho * *     |                            |                                    |                          |                                |                      | Sim                  |
| Rastreio de contador do SMB * *             |                            |                                    |                          |                                | Sim                  |                      |
| Rastreio de contador do SQL Server * *      |                            |                                    |                          |                                |                      | Sim                  |
| Rastreio XPerf                      |                            |                                    |                          |                                |                      | Sim                  |
| Rastreio StorPort                   |                            |                                    |                          |                                |                      | Sim                  |
| Rastreio de rede                    |                            |                                    |                          |                                | Sim                  | Sim                  |
| Rastreio de benchmark de Diskspd ***      |                            | Sim                                |                          | Sim                            |                      |                      |
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

## <a name="run-the-perfinsights-script-on-your-vm"></a>Execute o script de PerfInsights na VM

### <a name="what-do-i-have-to-know-before-i-run-the-script"></a>O que é necessário saber antes de posso executar o script? 

#### <a name="script-requirements"></a>Requisitos de script

-  Este script tem de ser executado na VM que tenha o problema de desempenho. 

-  São suportados os seguintes sistemas operativos: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016; Windows 8.1 e Windows 10.

#### <a name="possible-problems-when-you-run-the-script-on-production-vms"></a>Possíveis problemas ao executar o script em produção VMs

-  Para o cenário de "Personalizada lenta VM Analysis Services" que está configurado para utilizar Xperf ou Diskspd ou qualquer benchmarking cenários, o script poderá afetar negativamente o desempenho da VM. Não deve executar estes cenários num ambiente de produção.

-  Para o cenário de "Personalizada lenta VM Analysis Services" que está configurado para utilizar Diskspd ou qualquer benchmarking cenários, certifique-se de que nenhuma outra atividade em segundo plano interfere com a carga de trabalho de e/s.

-  Por predefinição, o script utiliza a unidade de armazenamento temporário para recolher dados. Se o rastreio permanece ativada durante mais tempo, a quantidade de dados que são recolhidos pode ser relevante. Isto pode reduzir a disponibilidade de espaço em disco temporário e, por conseguinte, pode afetar a qualquer aplicação que se baseie nesta unidade.

### <a name="how-do-i-run-perfinsights"></a>Como posso executar PerfInsights? 

Pode executar PerfInsights numa máquina virtual através da instalação [extensão de VM de diagnóstico do Azure desempenho](performance-diagnostics-vm-extension.md). Também pode executá-lo como um script de autónomo. 

**Instalar e executar PerfInsights do portal do Azure**

Para obter mais informações sobre esta opção, consulte [instalar Azure desempenho VM extensão de diagnóstico](performance-diagnostics-vm-extension.md#install-the-extension).  

**Execute o script de PerfInsights no modo autónomo**

Para executar o script de PerfInsights, siga estes passos:


1. Transferir [PerfInsights.zip](http://aka.ms/perfinsightsdownload).

2. O ficheiro PerfInsights.zip de desbloqueio. Para tal, o ficheiro PerfInsights.zip com o botão direito e selecione **propriedades**. No **geral** separador, selecione **desbloqueio**e, em seguida, selecione **OK**. Isto garante que o script é executado sem avisos adicionais de segurança.  

    ![Captura de ecrã de PerfInsights propriedades, com o desbloqueio realçado](media/how-to-use-perfInsights/unlock-file.png)

3.  Expandir o ficheiro PerfInsights.zip comprimido para a unidade temporária (por predefinição, este é normalmente a unidade D). O ficheiro comprimido deve conter os seguintes ficheiros e pastas:

    ![Captura de ecrã dos ficheiros na pasta zip](media/how-to-use-perfInsights/file-folder.png)

4.  Abra o Windows PowerShell como administrador e, em seguida, execute o script de PerfInsights.ps1.

    ```
    cd <the path of PerfInsights folder >
    Powershell.exe -ExecutionPolicy UnRestricted -NoProfile -File .\\PerfInsights.ps1
    ```

    Poderá ter de introduzir "y", para confirmar que pretende alterar a política de execução.

    No **Repare e consentimento** caixa de diálogo, tem a opção partilhar informações de diagnóstico com Support da Microsoft. Também terá de consentir para o contrato de licença para continuar. Faça as suas seleções e, em seguida, selecione **executar Script**.

    ![Caixa de diálogo de captura de ecrã de aviso e consentimento](media/how-to-use-perfInsights/disclaimer.png)

5.  Submeta o número de cenário, se estiver disponível, ao executar o script. Em seguida, selecione **OK**.
    
    ![Captura de ecrã da caixa de diálogo de ID de suporte](media/how-to-use-perfInsights/enter-support-number.png)

6.  Selecione a unidade de armazenamento temporário. O script pode deteção automática a letra de unidade da unidade. Se ocorrerem problemas nesta fase, poderá ser-lhe pedido para selecionar a unidade (unidade predefinida é D). Registos gerados são armazenados aqui, no registo de\_pasta de coleção. Depois de introduzir ou aceitar a letra de unidade, selecione **OK**.

    ![Captura de ecrã da caixa de diálogo de unidade temporária](media/how-to-use-perfInsights/enter-drive.png)

7.  Selecione um cenário de resolução de problemas da lista fornecida.

       ![Captura de ecrã da lista de cenários de resolução de problemas](media/how-to-use-perfInsights/select-scenarios.png)

Também pode executar PerfInsights sem a IU. O seguinte comando executa a "VM lenta uma análise" cenário sem a IU de resolução de problemas. Pede-lhe consentimento para a mesma exclusão de responsabilidade e EULA mencionados no passo 4.

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30"

Se pretender PerfInsights para ser executado no modo silencioso, utilize o **- AcceptDisclaimerAndShareDiagnostics** parâmetro. Por exemplo, utilize o seguinte comando:

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30 -AcceptDisclaimerAndShareDiagnostics"

### <a name="how-do-i-troubleshoot-issues-while-running-the-script"></a>Como posso resolver problemas ao executar o script?

Se o script termina anormalmente, poderá regressar num estado consistente, executando o script, juntamente com o comutador de limpeza, da seguinte forma:

    powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -Cleanup"

Se ocorrerem problemas durante a deteção automática de unidade temporária, poderá ser-lhe pedido para selecionar a unidade (unidade predefinida é D).

![Captura de ecrã da caixa de diálogo de unidade temporária](media/how-to-use-perfInsights/enter-drive.png)

O script desinstala as ferramentas e remove pastas temporárias.

### <a name="troubleshoot-other-script-issues"></a>Resolver outros problemas de script 

Se ocorrerem problemas ao executar o script, prima Ctrl + C para interromper o script. Se continuar a ocorrer a falha de script, mesmo após várias tentativas, execute o script no modo de depuração utilizando o "-Debug" opção de parâmetro no arranque.

Após a falha ocorre, copie a saída completa da consola do PowerShell e enviá-lo para o agente de Support da Microsoft que está a prestar assistência para ajudar a resolver o problema.

### <a name="how-do-i-run-the-script-in-custom-slow-vm-analysis-mode"></a>Como se executar o script no modo de "Personalizada lenta VM analysis"?

Ao selecionar o **análise VM lenta personalizada**, pode ativar várias rastreios em paralelo (para selecionar vários rastreios, utilize a tecla Shift):

![Captura de ecrã da lista dos cenários](media/how-to-use-perfInsights/select-scenario.png)

Quando seleciona o rastreio de contador de desempenho, Xperf rastreio, rastreio da rede ou Storport rastreio cenários, siga as instruções nas caixas de diálogo subsequentes. Tente reproduzir o problema de desempenho lento depois de iniciar os rastreios.

Iniciar um rastreio através da caixa de diálogo seguinte:

![Caixa de diálogo de captura de ecrã de início desempenho contador rastreio](media/how-to-use-perfInsights/start-trace-message.png)

Para parar os rastreios, tem de confirmar o comando numa segunda caixa de diálogo.

![Caixa de diálogo de captura de ecrã de parar desempenho contador rastreio](media/how-to-use-perfInsights/stop-trace-message.png)
![captura de ecrã de parar todos os rastreios a caixa de diálogo](media/how-to-use-perfInsights/ok-trace-message.png)

Quando os rastreios nem de operações estiverem concluídas, um novo ficheiro aparece no d:\\registo\_coleção (ou unidade temporária). O nome do ficheiro é **CollectedData\_aaaa-MM-dd\_hh\_mm\_ss.zip.** Pode enviar este ficheiro para o agente de suporte para análise.

## <a name="review-the-diagnostics-report"></a>Reveja o relatório de diagnóstico

Dentro do **CollectedData\_aaaa-MM-dd\_hh\_mm\_ss.zip** ficheiro, pode encontrar um relatório HTML que explica em detalhe findings de PerfInsights. Para rever o relatório, expanda o **CollectedData\_aaaa-MM-dd\_hh\_mm\_ss.zip** de ficheiros e, em seguida, abra o **PerfInsights Report.html** ficheiro.

Selecione o **Findings** separador.

![Captura de ecrã do relatório PerfInsights](media/how-to-use-perfInsights/findingtab.png)
![captura de ecrã do relatório PerfInsights](media/how-to-use-perfInsights/findings.PNG)

> [!NOTE] 
> Se os problemas que possam causar problemas de desempenho conhecidos findings categorizados como crítica. Findings categorizados como configurações de não ideal representam importante que não necessariamente a causar problemas de desempenho. Findings categorizados como informativo são apenas as instruções informativos.

Reveja as recomendações e ligações para todas as findings críticos e importantes. Saiba mais sobre como podem afetar o desempenho bem como sobre as melhores práticas para configurações de desempenho otimizado.

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

## <a name="next-steps"></a>Passos seguintes

Pode carregar registos de diagnóstico e de relatórios ao Microsoft Support para uma revisão mais aprofundada. O suporte poderá pedir que transmitir o resultado que é gerado pelo PerfInsights para ajudá-lo com o processo de resolução de problemas.

A seguinte captura de ecrã mostra uma mensagem semelhante ao que poderá receber:

![Captura de ecrã da mensagem de exemplo do Support da Microsoft](media/how-to-use-perfInsights/supportemail.png)

Siga as instruções na mensagem para aceder à área de trabalho de transferência de ficheiros. Para segurança adicional, tem de alterar a palavra-passe na primeira utilização.

Depois de iniciar sessão, irá encontrar uma caixa de diálogo para carregar o **CollectedData\_aaaa-MM-dd\_hh\_mm\_ss.zip** ficheiro que foi recolhido por PerfInsights.
