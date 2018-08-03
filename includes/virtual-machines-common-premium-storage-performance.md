# <a name="azure-premium-storage-design-for-high-performance"></a>Armazenamento Premium do Azure: Design de alto desempenho

Este artigo fornece diretrizes para a criação de aplicativos de alto desempenho usando o armazenamento Premium do Azure. Pode utilizar as instruções fornecidas neste documento, combinado com melhores práticas de desempenho aplicáveis para as tecnologias utilizadas pela sua aplicação. Para ilustrar as diretrizes, usamos SQL Server em execução no armazenamento Premium como um exemplo em todo este documento.

Enquanto estamos a resolver cenários de desempenho para a camada de armazenamento neste artigo, precisará otimizar a camada de aplicativo. Por exemplo, se estiver a alojar um Farm do SharePoint no armazenamento Premium do Azure, pode utilizar os exemplos do SQL Server deste artigo para otimizar o servidor de base de dados. Além disso, Otimize o servidor Web e servidor de aplicações para obter a maior parte do desempenho do Farm do SharePoint.

Este artigo o ajudará a resposta a perguntas comuns sobre a otimização de desempenho de aplicações no armazenamento do Azure Premium, a seguir

* Como medir o desempenho da aplicação?  
* Por que motivo está a ver não esperado de alto desempenho?  
* Que fatores influenciam o desempenho da aplicação no armazenamento Premium?  
* Como é que esses fatores influenciam o desempenho da sua aplicação no armazenamento Premium?  
* Como pode otimizar para IOPS, largura de banda e latência?  

Nós fornecemos estas diretrizes especificamente para o armazenamento Premium como cargas de trabalho em execução no armazenamento Premium são altamente confidencial do desempenho. Nós fornecemos exemplos quando apropriado. Também pode aplicar alguns destas diretrizes para aplicativos em execução em VMs de IaaS com discos de armazenamento Standard.

Antes de começar, se estiver familiarizado com o armazenamento Premium, leia primeiro o [o armazenamento Premium: armazenamento de elevado desempenho para cargas de trabalho do Azure Virtual Machine](../articles/virtual-machines/windows/premium-storage.md) e [escalabilidade do armazenamento do Azure e metas de desempenho](../articles/storage/common/storage-scalability-targets.md)artigos.

## <a name="application-performance-indicators"></a>Indicadores de desempenho da aplicação
Vamos avaliar se o desempenho de uma aplicação bem ou não a utilizar como indicadores de desempenho, a rapidez uma aplicação está a processar um pedido de utilizador, a quantidade de dados está a processar uma aplicação por pedido, quantas solicitações é um aplicativo de processamento numa determinada período de tempo, que um usuário precisa esperar para obter uma resposta depois de submeter o pedido. São os termos de técnicos para estes indicadores de desempenho, IOPS, débito ou largura de banda e latência.

Nesta seção, abordaremos os indicadores de desempenho comuns no contexto de armazenamento Premium. Na seção a seguir, os requisitos de aplicação de coleta, aprenderá como medir estes indicadores de desempenho para a sua aplicação. Mais tarde na otimização de desempenho do aplicativo, aprenderá sobre os fatores que afetam estes indicadores de desempenho e recomendações para otimizá-los.

## <a name="iops"></a>IOPS
IOPS é o número de pedidos que a aplicação está a enviar para os discos de armazenamento num segundo. Uma operação de entrada/saída foi possível ler ou escrever, seqüencial ou aleatório. Aplicativos de OLTP, como um Web site do setor do retalho online precisam processar muitos pedidos de utilizador em simultâneo imediatamente. Os pedidos de utilizador são inserir e atualizar as transações de base de dados com utilização intensiva, o que a aplicação deve processar rapidamente. Por conseguinte, os aplicativos de OLTP exigem muito elevado IOPS. Tais aplicativos lidar com milhões de pedidos de e/s pequenos e aleatórios. Se tiver uma aplicação desse tipo, deve conceber a infraestrutura de aplicações para otimizar o IOPS. Na seção posterior, *Otimizando o desempenho da aplicação*, vamos discutir detalhadamente todos os fatores que tem de considerar para obter elevado IOPS.

Quando anexar um disco de armazenamento premium para a sua VM, de grande escala disposições do Azure para que um número garantido de IOPS de acordo com a especificação de disco. Por exemplo, um disco de P50 Aprovisiona 7500 IOPS. Cada grande escala de tamanho da VM também tem um limite IOPS específico que podem sustentar. Por exemplo, uma VM GS5 padrão tem 80 000 IOPS limitar.

## <a name="throughput"></a>Débito
Débito ou de largura de banda é a quantidade de dados que a aplicação estiver a enviar para os discos de armazenamento num intervalo especificado. Se a sua aplicação está a efetuar operações de entrada/saída com tamanhos de unidade de e/s grandes, ela exige um débito elevado. Aplicativos de armazém de dados tendem a emitir operações intensivas de análise de grandes partes de dados de aceder ao mesmo tempo e normalmente realizar operações em massa. Em outras palavras, esses aplicativos requerem um débito mais elevado. Se tiver uma aplicação desse tipo, deve conceber a sua infraestrutura para otimizar o débito. Na próxima seção, vamos abordar em detalhes os fatores que deve ajustar para atingir esse objetivo.

Quando anexar um disco de armazenamento premium a uma VM, de grande escala disposições do Azure a taxa de transferência de acordo com essa especificação de disco. Por exemplo, um disco de P50 Aprovisiona 250 MB por segundo disco débito. Cada grande escala de tamanho da VM também tem como limite de taxa de transferência específico que podem sustentar. Por exemplo, o padrão GS5 VM tem um débito máximo de 2000 MB por segundo. 

Existe uma relação entre o débito e IOPS, como mostra a fórmula abaixo.

![](media/premium-storage-performance/image1.png)

Portanto, é importante determinar os valores IOPS e débito ideal que seu aplicativo requer. Ao testar otimizar um, o outro também obtém afetado. Numa seção posterior, *Otimizando o desempenho da aplicação*, abordaremos em mais detalhes sobre a otimização de IOPS e débito.

## <a name="latency"></a>Latência
A latência é o tempo que demora um aplicativo para receber um único pedido e enviá-lo para os discos de armazenamento e enviar a resposta ao cliente. Esta é uma medida crítica de desempenho de um aplicativo, além de IOPS e débito. A latência de um disco de armazenamento premium é o tempo que demora a recuperar as informações de um pedido e comunicá-la para a sua aplicação. O armazenamento Premium fornece baixas latências consistentes. Se ativar a colocação em cache em discos de armazenamento premium de anfitrião de só de leitura, pode obter muito menor latência de leitura. Abordaremos colocação em cache do disco mais detalhadamente na seção posterior sobre *Otimizando o desempenho da aplicação*.

Quando pretender otimizar seu aplicativo para obter uma maior IOPS e débito, afetará a latência da sua aplicação. Depois de ajuste do desempenho do aplicativo, avalie sempre a latência do aplicativo para evitar o comportamento inesperado de alta latência.

## <a name="gather-application-performance-requirements"></a>Recolher requisitos de desempenho da aplicação
A primeira etapa na criação de aplicativos de alto desempenho em execução no armazenamento Premium do Azure é, para compreender os requisitos de desempenho da sua aplicação. Após recolher requisitos de desempenho, pode otimizar a sua aplicação para alcançar o desempenho ideal.

Na secção anterior, Explicamos os indicadores de desempenho comuns, IOPS, débito e latência. Tem de identificar que estes indicadores de desempenho são fundamentais para a sua aplicação para proporcionar a experiência de usuário que desejarem. Por exemplo, um IOPS é mais importante para aplicativos de OLTP processar milhões de transações num segundo. Ao passo que, débito elevado é fundamental para aplicativos de armazém de dados, processamento de grandes quantidades de dados num segundo. Latência extremamente baixa é crucial para aplicações em tempo real como Web sites de transmissão em fluxo de vídeo ao vivo.

Em seguida, medir os requisitos de desempenho máximo da sua aplicação durante seu ciclo de vida. Utilize a lista de verificação de exemplo abaixo como um começo. Registe os requisitos de desempenho máximo durante normal, pico e o horário comercial períodos de carga de trabalho. Ao identificar os requisitos para todos os níveis de cargas de trabalho, será capaz de determinar os requisitos de desempenho global do seu aplicativo. Por exemplo, a carga de trabalho normal de um site de comércio eletrônico será as transações funciona durante a maior parte dos dias num ano. A carga de trabalho de pico do Web site será as transações funciona durante a época festiva ou eventos de venda especial. O pico de carga de trabalho é normalmente experiente durante um período limitado, mas pode exigir a sua aplicação para expandir sua operação normal de dois ou mais vezes. Descubra o percentil de 50, o percentil 90 e a requisitos de percentil de 99. Isto ajuda a filtrar quaisquer exceções nos requisitos de desempenho e que se possa concentrar seus esforços na otimização para os valores de certos.

**Lista de verificação de requisitos de desempenho de aplicações**

| **Requisitos de desempenho** | **Percentil de 50** | **Percentil 90** | **Percentil de 99** |
| --- | --- | --- | --- |
| Um máximo de Transações por segundo | | | |
| Operações de leitura de % | | | |
| Operações de escrita % | | | |
| Operações de aleatório % | | | |
| Operações de % sequenciais | | | |
| Tamanho do pedido de e/s | | | |
| Débito médio | | | |
| Um máximo de Débito | | | |
| Min. Latência | | | |
| Latência média | | | |
| Um máximo de CPU | | | |
| CPU Média | | | |
| Um máximo de Memória | | | |
| Médio da memória | | | |
| Profundidade de fila | | | |

> [!NOTE]
> Deve considerar o dimensionamento esses números com base no crescimento futuro esperado da sua aplicação. É uma boa idéia planeie o crescimento antes do tempo, uma vez que poderia ser mais difícil alterar a infraestrutura para melhorar o desempenho mais tarde.
>
>

Se tiver uma aplicação existente e pretender mover para o armazenamento Premium, primeiro crie a lista de verificação acima para a aplicação existente. Em seguida, criar um protótipo de seu aplicativo no armazenamento Premium e projetar o aplicativo com base nas diretrizes descritas em *Otimizando o desempenho da aplicação* numa secção posterior deste documento. A secção seguinte descreve as ferramentas que pode utilizar para reunir as medidas de desempenho.

Crie uma lista de verificação semelhante à sua aplicação existente para o protótipo. Utilizar ferramentas de Benchmarking pode simular as cargas de trabalho e medir o desempenho na aplicação de protótipo. Consulte a secção sobre [Benchmarking](#benchmarking) para saber mais. Ao fazer para que possa determinar se o armazenamento Premium podem corresponder ou superar as suas necessidades de desempenho de aplicativo. Em seguida, pode implementar as mesmas diretrizes para a sua aplicação de produção.

### <a name="counters-to-measure-application-performance-requirements"></a>Contadores para medir os requisitos de desempenho de aplicações
A melhor forma de medir os requisitos de desempenho do seu aplicativo, é usar ferramentas de monitoramento de desempenho fornecidas pelo sistema operativo do servidor. Pode usar o PerfMon para Windows e iostat para Linux. Essas ferramentas capturam contadores correspondente a cada medida explicada na seção acima. Quando a aplicação está a executar o normal, pico e o horário comercial cargas de trabalho, tem de capturar os valores desses contadores.

Os contadores de PerfMon estão disponíveis para o processador, memória e cada disco lógico e o disco físico do seu servidor. Ao utilizar discos de armazenamento premium com uma VM, os contadores de disco físico são para cada disco de armazenamento premium e os contadores de disco lógico são para cada volume criado nos discos de armazenamento premium. Tem de capturar os valores para os discos que alojam a carga de trabalho de aplicação. Se existir um mapeamento um-para-um entre discos lógicos e físicos, pode consultar os contadores de discos físicos; caso contrário, consulte os contadores de disco lógico. No Linux, o comando iostat gera um relatório de utilização da CPU e disco. O relatório de utilização do disco fornece estatísticas por dispositivo físico ou partição. Se tiver um servidor de base de dados com seus dados e de registo em discos separados, recolha estes dados para os dois discos. Tabela a seguir descreve os contadores de discos, processador e memória:

| Contador | Descrição | PerfMon | iostat |
| --- | --- | --- | --- |
| **IOPS ou transações por segundo** |Número de pedidos de e/s emitida para o disco de armazenamento por segundo. |Leituras de disco/seg <br> Escritas de disco/seg |tps <br> r/s <br> w/s |
| **Leituras de disco e gravações** |% de leituras e escrever operações executadas no disco. |% De tempo de leitura de disco <br> % De tempo de escrita de disco |r/s <br> w/s |
| **Débito** |Quantidade de dados lidas ou escritas para o disco por segundo. |Bytes Lidos de Disco/seg <br> Bytes Escritos em Disco/seg |kB_read/s <br> kB_wrtn/s |
| **Latência** |Tempo total para concluir um pedido de e/s de disco. |Média disco seg/leitura <br> Média disco seg/escritas |await <br> svctm |
| **Tamanho de e/s** |O tamanho de e/s pedidos de problemas para os discos de armazenamento. |Bytes de média de disco/leitura <br> Disco de média de Bytes/escrita |avgrq sz |
| **Profundidade de fila** |Número de e/s pendentes pedidos à espera para ser lidas ou escritas no disco de armazenamento. |Comprimento de fila de disco atual |avgqu sz |
| **Máx. Memória** |Quantidade de memória necessária para executar a aplicação sem problemas |% Bytes dedicados em utilização |Utilizar vmstat |
| **Máx. CPU** |Quantidade de CPU necessária para executar a aplicação sem problemas |% Tempo do processador |% util |

Saiba mais sobre [iostat](https://linux.die.net/man/1/iostat) e [PerfMon](https://msdn.microsoft.com/library/aa645516.aspx).

## <a name="optimizing-application-performance"></a>Otimizando o desempenho da aplicação
Os principais fatores que influenciam o desempenho de um aplicativo em execução no armazenamento Premium são de natureza de e/s pedidos, tamanho da VM, o tamanho do disco, número de discos, colocação em cache do disco, a Multithreading e a profundidade de fila. Pode controlar alguns desses fatores com botões fornecidos pelo sistema. A maioria dos aplicativos podem não oferecer a uma opção para alterar o tamanho de e/s e a profundidade de fila diretamente. Por exemplo, se estiver a utilizar o SQL Server, não é possível escolher a profundidade de fila e tamanho de e/s. SQL Server escolhe os ideal e/s tamanho e a fila de profundidade valores para o maioria de desempenho. É importante compreender os efeitos de ambos os tipos de fatores sobre o desempenho da aplicação, para que possa Aprovisionar recursos adequados para atender às necessidades de desempenho.

Ao longo da seção, consulte a lista de verificação de requisitos de aplicação que criou, para identificar quanto de que precisa para otimizar o desempenho da aplicação. Baseado nisso, será capaz de determinar quais fatores nesta seção precisará ajustar. Observar os efeitos de cada fator sobre o desempenho da aplicação, execute as ferramentas de benchmark em sua configuração de aplicações. Consulte a [Benchmarking](#Benchmarking) secção no final deste artigo para obter os passos executar as ferramentas de benchmark comuns no Windows e VMs do Linux.

### <a name="optimizing-iops-throughput-and-latency-at-a-glance"></a>Otimizando o IOPS, débito e latência de relance

A tabela abaixo resume os fatores de desempenho e as etapas necessárias para otimizar o IOPS, débito e latência. As secções deste resumo a seguir descrevem cada fator é muito mais profundidade.

Para obter mais informações sobre tamanhos de VM e sobre o IOPS, débito e latência disponível para cada tipo de VM, consulte [tamanhos de VM do Linux](../articles/virtual-machines/linux/sizes.md) ou [tamanhos de VM do Windows](../articles/virtual-machines/windows/sizes.md).

| &nbsp; | **IOPS** | **Débito** | **Latência** |
| --- | --- | --- | --- |
| **Cenário de exemplo** |Aplicativo de OLTP de Enterprise que exigem muito elevadas de transações por segundo taxa. |Aplicação processamento quantidades de dados de armazém de dados da empresa. |Quase em tempo real aplicativos que exigem respostas instantâneas às solicitações dos usuários, como jogos online. |
| Fatores de desempenho | &nbsp; | &nbsp; | &nbsp; |
| **Tamanho de e/s** |Tamanho de e/s mais pequeno produz IOPS superior. |Tamanho de e/s maior para produz um débito mais elevado. | &nbsp;|
| **Tamanho da VM** |Utilize um tamanho VM que oferece maior do que as necessidades da sua aplicação de IOPS. |Utilize um tamanho de VM com o limite de débito superior a necessidades da sua aplicação. |Utilize um tamanho VM que ofertas aumentar os limites superiores a necessidades da sua aplicação. |
| **Tamanho do disco** |Utilize um tamanho de disco que oferece maior do que as necessidades da sua aplicação de IOPS. |Utilize um tamanho de disco com limite de taxa de transferência maior do que as necessidades da sua aplicação. |Utilize um tamanho de disco que ofertas aumentar os limites superiores a necessidades da sua aplicação. |
| **Limites de dimensionamento de disco e de VM** |Limite de IOPS do tamanho da VM escolhido deve ser superior a total de IOPS por discos de armazenamento premium ligados ao mesmo. |Limite de taxa de transferência do tamanho da VM escolhido deve ser superior a taxa de transferência total, orientada pelos discos de armazenamento premium ligados ao mesmo. |Limites de dimensionamento do tamanho da VM escolhida devem ser superiores a limites de dimensionamento de total de discos de armazenamento premium ligados. |
| **Colocação em cache do disco** |Ative a Cache de só de leitura nos discos de armazenamento premium com operações pesadas de leitura para obter mais elevada IOPS de leitura. | &nbsp; |Ative a Cache de só de leitura nos discos de armazenamento premium com operações pesadas prontos para obter leitura muito baixa latências. |
| **Repartição de disco** |Utilizar vários discos e da faixa de-los para obter um limite IOPS e débito mais elevado combinado. Tenha em atenção que o limite combinado por VM deve ser maior do que os limites combinados de discos premium ligados. | &nbsp; | &nbsp; |
| **Tamanho do stripe** |Tamanho mais pequeno do stripe para o padrão de e/s pequenas aleatória visto em aplicativos de OLTP. Por exemplo, utilize o tamanho da faixa de 64KB para a aplicação de OLTP do SQL Server. |Tamanho maior do stripe para o padrão de e/s grandes sequencial visto em aplicativos de armazém de dados. Por exemplo, utilize o tamanho da faixa de 256KB para a aplicação do armazém de dados do SQL Server. | &nbsp; |
| **Multithreading** |Utilização de multithreading para enviar o número mais elevado de pedidos para o armazenamento Premium que levará a maior IOPS e débito. Por exemplo, no SQL Server definir um valor MAXDOP elevado para alocar mais CPUs, ao SQL Server. | &nbsp; | &nbsp; |
| **Profundidade de fila** |IOPS superior resulta em maior profundidade de fila. |Maior profundidade de fila produz um débito mais elevado. |Menor profundidade de fila produz latências mais baixas. |

## <a name="nature-of-io-requests"></a>Natureza de pedidos de e/s
Um pedido de e/s é uma unidade de operação de entrada/saída que executará seu aplicativo. Identificando a natureza de pedidos de e/s, aleatórios ou sequenciais, de leitura ou escrita, pequena ou grande, irá ajudá-lo a determinar os requisitos de desempenho da sua aplicação. É muito importante compreender a natureza de pedidos de e/s, para tomar as decisões corretas ao conceber a sua infraestrutura de aplicações.

Tamanho de e/s é um dos fatores mais importantes. O tamanho de e/s é o tamanho do pedido de operação de entrada/saída gerado pela sua aplicação. O tamanho de e/s tem um impacto significativo no desempenho especialmente na IOPS e largura de banda que o aplicativo é capaz de alcançar. A fórmula seguinte mostra a relação entre o IOPS, tamanho de e/s e a largura de banda/débito.  
    ![](media/premium-storage-performance/image1.png)

Algumas aplicações permitem-lhe alterar o seu tamanho de e/s, enquanto alguns aplicativos não fazem. Por exemplo, o SQL Server determina o tamanho ideal de e/s em si e não fornece os utilizadores com qualquer botões para alterá-lo. Por outro lado, a Oracle disponibiliza a um parâmetro denominado [DB\_bloquear\_tamanho](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) com que pode configurar o tamanho do pedido de e/s da base de dados.

Se estiver a utilizar uma aplicação, que não permite-lhe alterar o tamanho de e/s, utilize as diretrizes deste artigo para otimizar o desempenho de KPI que é mais relevante para seu aplicativo. Por exemplo,

* Um aplicativo de OLTP gera milhões de pedidos de e/s pequenos e aleatórios. Para lidar com estes tipos de pedidos de e/s, tem de criar sua infraestrutura de aplicações para obter mais IOPS.  
* Uma aplicação de armazém de dados gera solicitações de e/s grandes e sequenciais. Para lidar com estes tipos de pedidos de e/s, deve projetar sua infraestrutura de aplicações para obter maior largura de banda ou débito.

Se estiver a utilizar uma aplicação, o que permite-lhe alterar o tamanho de e/s, utilize este princípio para o tamanho de e/s, além de outras diretrizes de desempenho,

* Tamanho de e/s mais pequeno para obter mais IOPS. Por exemplo, 8 KB para uma aplicação de OLTP.  
* Tamanho de e/s maior para obter a largura de banda/débito mais elevado. Por exemplo, 1024 KB para uma aplicação do armazém de dados.

Eis um exemplo sobre como é possível calcular o IOPS e largura de banda/débito para a sua aplicação. Considere uma aplicação a utilizar um disco de P30. O máximo IOPS e débito/largura de banda de um disco de P30 podem alcançar é 5000 IOPS e 200 MB por segundo, respetivamente. Agora, se a sua aplicação requer o IOPS máximo do disco P30 e utilizar um tamanho mais pequeno de e/s, como de 8 KB, a largura de banda resultante será capaz de fazer é 40 MB por segundo. No entanto, se seu aplicativo requer a débito/largura de banda máxima do disco de P30, e utilizar um tamanho de e/s maior, como 1024 KB, o IOPS resultante será menos, 200 IOPS. Por conseguinte, ajuste o tamanho de e/s, de modo que ele cumpre o requisito de IOPS e largura de banda/débito de ambos os seu aplicativo. Tabela a seguir resume os diferentes tamanhos de e/s e seus correspondentes IOPS e débito para um disco de P30.

| Requisito da aplicação | Tamanho de e/s | IOPS | Largura de banda/débito |
| --- | --- | --- | --- |
| IOPS Máx. |8 KB |5.000 |40 MB por segundo |
| Débito máximo |1024 KB |200 |200 MB por segundo |
| Débito máximo do + IOPS elevado |64 KB |3,200 |200 MB por segundo |
| IOPS máx + alto débito |32 KB |5.000 |160 MB por segundo |

Para obter o IOPS e largura de banda superior ao valor máximo de um disco de armazenamento premium única, utilize vários discos premium repartidos em conjunto. Por exemplo, da faixa de dois discos de P30 para obter um IOPS combinado de IOPS 10 000 ou de um débito combinado de 400 MB por segundo. Conforme explicado na próxima seção, tem de utilizar um tamanho VM que suporte o combinado IOPS e débito de disco.

> [!NOTE]
> À medida que aumenta o IOPS ou o débito também aumenta a outra, certifique-se de que não se comunicam com débito ou limites de IOPS do disco ou VM ao aumentar a qualquer deles.
>
>

Observar os efeitos de tamanho de e/s no desempenho do aplicativo, pode executar ferramentas de benchmark na sua VM e os discos. Crie várias execuções de teste e utilize um tamanho de e/s diferente para cada execução para ver o impacto. Consulte a [Benchmarking](#Benchmarking) secção no final deste artigo para obter mais detalhes.

## <a name="high-scale-vm-sizes"></a>Tamanhos de VM de grande escala
Quando começa a projetar um aplicativo, uma das primeiras coisas a fazer é, escolha uma VM para alojar a sua aplicação. O armazenamento Premium é fornecido com tamanhos de VM de escala elevada que podem executar aplicações que requerem uma potência de computação e um elevado desempenho de e/s de disco local. Estas VMs fornecem processadores mais rápidos, um rácio de memória/núcleo superior e um Solid-State unidade (SSD) para o disco local. Os exemplos de alta de dimensionamento de VMs, que suporta o armazenamento Premium são a série DS, DSv2 e GS VMs.

Alta de dimensionamento de VMs estão disponíveis em tamanhos diferentes com um número diferente de núcleos da CPU, memória, sistema operacional e tamanho de disco temporário. Cada tamanho de VM também tem o número máximo de discos de dados que podem ser anexadas à VM. Por conseguinte, o tamanho da VM escolhido afetará a quantidade de processamento, memória, e a capacidade de armazenamento está disponível para a sua aplicação. Também afeta a computação e custos de armazenamento. Por exemplo, seguem-se as especificações do maior tamanho VM numa série DS, série DSv2 e uma série GS:

| Tamanho da VM | Núcleos de CPU | Memória | Tamanhos de disco VM | Um máximo de Discos de dados | Tamanho da cache | IOPS | Limites de e/s de Cache de largura de banda |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS14 |16 |112 GB |SO = 1023 GB <br> Local SSD = 224 GB |32 |576 GB |50 000 IOPS <br> 512 MB por segundo |4000 IOPS e 33 MB por segundo |
| Standard_GS5 |32 |448 GB |SO = 1023 GB <br> Local SSD = 896 GB |64 |4224 GB |80 000 IOPS <br> 2.000 MB por segundo |5000 IOPS e 50 MB por segundo |

Para ver uma lista completa de todos os tamanhos de VM do Azure disponíveis, consulte [tamanhos de VM do Windows](../articles/virtual-machines/windows/sizes.md) ou [tamanhos de VM do Linux](../articles/virtual-machines/linux/sizes.md). Escolha um tamanho de VM que pode satisfazer e dimensionar-se aos seus requisitos de desempenho do aplicativo desejado. Além disso, tenha em conta considerações importantes sobre a seguir ao escolher tamanhos de VM.

*Limites de dimensionamento*  
Os limites IOPS máximos por VM e por disco são diferentes e independentes entre si. Certifique-se de que a aplicação está a orientar o IOPS dentro dos limites de VM, bem como os discos premium ligados à mesma. Caso contrário, desempenho de aplicações ultrapassado, origina limitação.

Por exemplo, suponha que um requisito de aplicativo é um máximo de 4000 IOPS. Para conseguir isso, aprovisionar um disco de P30 numa DS1 VM. O disco de P30 pode fornecer até 5000 IOPS. No entanto, a VM de DS1 está limitada a 3,200 IOPS. Conseqüentemente, o desempenho da aplicação serão restritos pelo limite VM em 3,200 IOPS e haja degradação do desempenho. Para evitar essa situação, escolha um tamanho VM e disco que será ambos atender aos requisitos da aplicação.

*Custo da operação*  
Em muitos casos, é possível que os custos gerais de operação de utilizar o armazenamento Premium é inferior a utilização do armazenamento Standard.

Por exemplo, considere um aplicativo exigir 16 000 IOPS. Para obter este desempenho, precisará de um padrão\_VM de IaaS do Azure de D14, que pode permitir que um IOPS máximo de 16.000 com 32 discos de 1 TB de armazenamento standard. Cada disco de armazenamento standard de 1TB pode atingir o máximo de 500 IOPS. O custo estimado desta VM por mês será de US $1,570. O custo mensal de 32 discos de armazenamento standard será de US $1,638. O custo mensal total estimado será de US $3,208.

No entanto, se alojado no armazenamento Premium, a mesma aplicação, terá um tamanho VM mais pequeno e menos discos de armazenamento premium, reduzindo o custo geral. Um padrão\_DS13 VM podem satisfazer o requisito de IOPS de 16.000 utilizar quatro discos de P30. A VM de DS13 tem um IOPS máximo de 25,600 e cada disco P30 tem um IOPS máximo de 5000. Em geral, esta configuração pode alcançar 5.000 x 4 = 20.000 IOPS. O custo estimado desta VM por mês será de US $1,003. O custo mensal de quatro discos de armazenamento de premium P30 será de US $544.34. O custo mensal total estimado será de US $1,544.

Tabela a seguir resume a análise detalhada do custo deste cenário para o Standard e o armazenamento Premium.

| &nbsp; | **Standard** | **Premium** |
| --- | --- | --- |
| **Custo de VM por mês** |US $1,570.58 (standard\_D14) |US $1,003.66 (standard\_DS13) |
| **Custo de discos por mês** |US $1,638.40 (32 x 1 TB de discos) |US $544.34 (4 x P30 discos) |
| **Custo global por mês** |$3,208.98 |$1,544.34 |

*Distribuições do Linux*  

Com o armazenamento Premium do Azure, obtém o mesmo nível de desempenho para as VMs que executam o Windows e Linux. Damos suporte a muitos tipos de distribuições Linux, e pode ver a lista completa [aqui](../articles/virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). É importante observar que diferentes distros mais adequadas para diferentes tipos de cargas de trabalho. Verá diferentes níveis de desempenho, consoante a distribuição de que sua carga de trabalho está a ser executada. Teste as distribuições de Linux com a sua aplicação e escolher o método que funcionará melhor.

Ao executar o Linux com o armazenamento Premium, verifique as atualizações mais recentes sobre os controladores necessários para garantir um desempenho elevado.

## <a name="premium-storage-disk-sizes"></a>Tamanhos de disco de armazenamento Premium
Armazenamento Premium do Azure oferece sete tamanhos de disco atualmente. Cada tamanho de disco tem um limite de escala diferente para IOPS, largura de banda e armazenamento. Escolha o melhor tamanho do disco de armazenamento Premium, consoante os requisitos de aplicação e a tamanho da VM de grande escala. A tabela abaixo mostra os tamanhos de sete discos e as respetivas funcionalidades. Tamanhos P4 e P6 estão atualmente apenas suportado para discos geridos.

| Tipo de discos Premium  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Tamanho do disco           | 32 GB | 64 GB | 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOPs por disco       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Débito por disco | 25 MB por segundo  | 50 MB por segundo  | 100 MB por segundo | 150 MB por segundo | 200 MB por segundo | 250 MB por segundo | 250 MB por segundo | 


Quantos discos que escolher depende do disco de tamanho de escolhida. Poderia usar um disco de P50 único ou vários discos de P10 para atender às necessidades da sua aplicação. Tenha em considerações de conta listadas abaixo, ao fazer a escolha.

*Limites de dimensionamento (IOPS e débito)*  
Os limites IOPS e débito de cada tamanho de disco Premium é diferente e independente dos limites de dimensionamento VM. Certifique-se de que ao total de IOPS e débito dos discos estão dentro dos limites de dimensionamento do tamanho da VM escolhido.

Por exemplo, se um requisito de aplicativo é um máximo de 250 MB por segundo de débito e estiver a utilizar uma VM DS4 com um único disco P30. A VM DS4 pode permitir que até 256 MB/seg débito. No entanto, um único disco P30 tem o limite de taxa de transferência de 200 MB/seg. Conseqüentemente, a aplicação serão restritos em 200 MB por segundo devido ao limite de disco. Para superar esse limite, aprovisionar mais do que um discos de dados à VM ou redimensionar os seus discos P40 ou P50.

> [!NOTE]
> Leituras servidas pela cache de não estão incluídas no disco IOPS e débito, pelo que não sujeita aos limites de disco. Cache tem o limite IOPS e débito separado por VM.
>
> Por exemplo, inicialmente sua leituras e gravações são 60MB/seg e 40MB/seg respetivamente. Ao longo do tempo, o cache warms cópia de segurança e serve mais e mais as leituras da cache. Em seguida, pode obter escrita mais alta débito do disco.
>
>

*Número de discos*  
Determine o número de discos, terá de avaliar os requisitos da aplicação. Cada tamanho de VM também tem um limite no número de discos que podem ser anexadas à VM. Normalmente, isto é duas vezes o número de núcleos. Certifique-se de que o tamanho da VM que escolher pode suportar o número de discos necessários.

Lembre-se de que os discos de armazenamento Premium têm capacidades de desempenho superior em comparação com discos de armazenamento Standard. Por conseguinte, se estiver a migrar a aplicação de VM de IaaS do Azure com o armazenamento Standard para o armazenamento Premium, provavelmente precisará menos os discos premium para obter um desempenho igual ou superior para a sua aplicação.

## <a name="disk-caching"></a>Colocação em cache do disco
As VMs de escala elevada que tiram partido do armazenamento Premium do Azure têm uma tecnologia de colocação em cache de várias camada chamada BlobCache. BlobCache usa uma combinação da RAM da Máquina Virtual e local SSD para colocar em cache. Esta cache está disponível para os discos de persistentes do armazenamento Premium e os discos da VM locais. Por predefinição, esta definição de cache é definida como leitura/escrita para discos de SO e só de leitura para os discos de dados alojados no armazenamento Premium. Com disco à colocação em cache ativada nos discos de armazenamento Premium, a VMs de grande escala pode alcançar extremamente altos níveis de desempenho que excedem o desempenho de disco subjacente.

> [!WARNING]
> Alterar a definição de cache de um disco do Azure desliga e voltar anexa o disco de destino. Se for o disco do sistema operativo, a VM é reiniciada. Pare todos os aplicativos/serviços que possam ser afetados por este interrupção antes de alterar a definição de cache do disco.
>
>

Para saber mais sobre como funciona o BlobCache, consulte o interior [armazenamento Premium do Azure](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) postagem de blog.

É importante ativar a cache no conjunto certo de discos. Se deve ativar a colocação em cache do disco num disco premium ou não dependerá o padrão de carga de trabalho desse disco irá processar. Tabela a seguir mostra as predefinições de definições de cache para discos de SO e dados.

| **Tipo de disco** | **Definição de Cache predefinida** |
| --- | --- |
| Disco do SO |ReadWrite |
| Disco de dados |ReadOnly |

Seguem-se as definições de cache de disco recomendado para discos de dados,

| **Definição de colocação em cache do disco** | **Recomendação sobre quando utilizar esta definição** |
| --- | --- |
| Nenhuma |Configure o cache do anfitrião como nenhum para discos de só de escrita e de escrita intensiva. |
| ReadOnly |Configure o cache do anfitrião como só de leitura para os discos só de leitura e de leitura / escrita. |
| ReadWrite |Configure o cache do anfitrião como ReadWrite apenas se o seu aplicativo manipula corretamente a escrita de dados em cache para discos persistentes quando necessário. |

*Só de leitura*  
Ao configurar a colocação em cache em dados de armazenamento Premium discos de só de leitura, pode obter a baixa latência de leitura e obter muito elevada IOPS de leitura e de débito para a sua aplicação. Isso é devido a duas razões,

1. Leituras efetuada a partir de cache, o que está na memória da VM e local SSD, são muito mais rápidas do que de leituras de disco de dados, que é o armazenamento de Blobs do Azure.  
1. O armazenamento Premium não conta as leituras servidas a partir da cache, para o disco IOPS e débito. Portanto, seu aplicativo é capaz de alcançar maior total de IOPS e débito.

*ReadWrite*  
Por predefinição, os discos de SO têm ReadWrite colocação em cache ativada. Adicionámos suporte para ReadWrite colocação em cache em dados de discos também recentemente. Se estiver a utilizar a colocação em cache do ReadWrite, tem de ter um modo adequado para escrever os dados da cache para discos persistentes. Por exemplo, o SQL Server processa escrever dados em cache para os discos de armazenamento persistente por conta própria. Utilizar a cache do ReadWrite com um aplicativo que não manipula a persistir os dados necessários pode levar a perda de dados, no caso de falha de VM.

Por exemplo, pode aplicar essas diretrizes para o SQL Server em execução no armazenamento Premium, ao fazer o seguinte,

1. Configure a cache de "Só de leitura" nos discos de armazenamento premium que alojem ficheiros de dados.  
   a.  O fast lê a partir do cache inferior o tempo de consulta do SQL Server, uma vez que as páginas de dados são obtidas muito mais rapidamente a partir da cache em comparação com diretamente a partir dos dados de discos.  
   b.  A servir leituras da cache, significa que há um débito adicionais disponíveis a partir de discos de dados premium. SQL Server pode utilizar este débito adicionais para obter mais páginas de dados e outras operações, como backup/restauração, carregamentos do batch e as recompilações de índice.  
1. Configurar "None" em cache nos discos de armazenamento premium que aloja os ficheiros de registo.  
   a.  Ficheiros de registo têm principalmente as operações de escrita intensiva. Por conseguinte, não beneficiam da cache só de leitura.

## <a name="disk-striping"></a>Repartição de disco
Quando uma escala elevada de que VM está ligada com vários discos persistentes de armazenamento premium, os discos podem repartidos em conjunto para agregar seus IOPs, a largura de banda e a capacidade de armazenamento.

No Windows, pode utilizar espaços de armazenamento para discos do stripe em conjunto. Tem de configurar uma coluna para cada disco num agrupamento. Caso contrário, o desempenho geral do volume repartido pode ser menor que o esperado devido a distribuição desigual de tráfego em todos os discos.

Importante:-Através da IU do Gestor de servidor, pode definir o número total de colunas até 8 para um volume repartido. Ao anexar mais de 8 discos, utilize o PowerShell para criar o volume. Com o PowerShell, pode definir o número de colunas igual ao número de discos. Por exemplo, se existirem 16 discos num conjunto único stripe; especificar 16 colunas na *NumberOfColumns* parâmetro do *New-VirtualDisk* cmdlet do PowerShell.

No Linux, utilize o utilitário MDADM para discos do stripe em conjunto. Para obter passos detalhados sobre os discos de repartição no Linux, consulte [configurar o RAID de Software no Linux](../articles/virtual-machines/linux/configure-raid.md).

*Tamanho do stripe*  
Uma configuração importantes no repartição de disco é o tamanho do stripe. O tamanho da faixa ou o tamanho do bloco é a parte mais pequeno de dados que o aplicativo puder atender num volume repartido. O tamanho do stripe que configurar depende do tipo de aplicativo e seu padrão de solicitação. Se escolher o tamanho do stripe errado, ele poderia levar a alinhamento incorreto da e/s, que leva à degradação do desempenho da sua aplicação.

Por exemplo, se um pedido de e/s gerado pela sua aplicação for maior do que o tamanho do stripe de disco, o sistema de armazenamento escreve-lo em limites de unidade repartidos em mais de um disco. Quando está na altura de aceder a esses dados, terá de procurar em mais do que um unidades do stripe para concluir o pedido. O efeito cumulativo desse comportamento pode levar a degradação do desempenho substanciais. Por outro lado, se o tamanho do pedido de e/s é menor do que o tamanho do stripe, e se é aleatório por natureza, os pedidos de e/s podem aumentar no mesmo disco provocar um estrangulamento e, por fim, degradar o desempenho de e/s.

Dependendo do tipo de carga de trabalho de que seu aplicativo está em execução, escolha um tamanho adequado do stripe. Para pedidos de e/s pequenas aleatórios, utilize um tamanho mais pequeno do stripe. Ao passo que, para o e/s sequenciais grandes pedidos de utilizar um tamanho maior do stripe. Descubra as stripe recomendações de tamanho para a aplicação que irá executar no armazenamento Premium. Para o SQL Server, configure o tamanho da faixa de 64KB para cargas de trabalho OLTP e 256KB para cargas de trabalho de armazenamento de dados. Ver [melhores práticas de desempenho para o SQL Server em VMs do Azure](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md#disks-guidance) para saber mais.

> [!NOTE]
> Pode do stripe em conjunto um máximo de 32 discos de armazenamento premium numa VM da série DS e de 64 discos de armazenamento premium numa VM da série GS.
>
>

## <a name="multi-threading"></a>Vários segmentos
Azure foi concebido a plataforma de armazenamento Premium para ser paralelo em grande escala. Por conseguinte, um aplicativo multithreaded alcança um desempenho muito superior que um aplicativo de thread único. Um aplicativo multithreaded divida as respetivas tarefas entre vários threads e aumenta a eficiência de sua execução utilizando os recursos VM e de disco para o máximo.

Por exemplo, se seu aplicativo é executado num único núcleo VM com dois threads, a CPU pode alternar entre os dois threads para obter eficiência. Enquanto um thread está aguardando num disco e/s para concluir, a CPU pode mudar para o outro thread. Dessa forma, dois threads podem ser feito mais do que faria com um único thread. Se a VM tiver mais de um núcleo, ela ainda mais diminui o tempo de execução, uma vez que cada núcleo pode executar tarefas em paralelo.

Não poderá alterar a forma como um aplicativo comercial implementa único thread ou vários segmentos. Por exemplo, o SQL Server é capaz de processar com várias CPUS e com vários núcleos. No entanto, o SQL Server decide em quais condições aproveitará um ou mais threads para processar uma consulta. Ele pode executar consultas e criar índices com multithreading. Para uma consulta que envolve associar tabelas grandes e ordenar os dados antes de retornar ao usuário, o SQL Server utilizará provavelmente vários threads. No entanto, um utilizador não é possível controlar se o SQL Server executa uma consulta com um único thread ou vários threads.

Existem definições de configuração que é possível alterar a influenciar este multithreading ou paralela de processamento de um aplicativo. Por exemplo, no caso do SQL Server é a configuração de grau de paralelismo máxima. Esta definição chamada MAXDOP, permite-lhe configurar o número máximo de processadores que do SQL Server pode usar ao paralela de processamento. Pode configurar MAXDOP para consultas individuais ou operações de índice. Isso é benéfico quando deseja equilibrar recursos do seu sistema para um aplicativo essencial de desempenho.

Por exemplo, digamos que seu aplicativo usando o SQL Server está a executar uma consulta grande e uma operação de índice ao mesmo tempo. Vamos assumir que queria que a operação de índice para ter um melhor desempenho em comparação com a consulta de grandes. Nesse caso, pode definir o valor MAXDOP da operação de índice seja maior do que o valor MAXDOP para a consulta. Dessa forma, o SQL Server tem mais de número de processadores que ele pode aproveitar para a operação de índice em comparação com o número de processadores, que ele pode dedicar à consulta de grandes dimensões. Lembre-se de que não controlar o número de threads do SQL Server irá utilizar para cada operação. Pode controlar o número máximo de processadores a ser dedicado para multithreading.

Saiba mais sobre [graus de paralelismo](https://technet.microsoft.com/library/ms188611.aspx) no SQL Server. Saiba quais são essas configurações que influenciam o multithreading em seu aplicativo e suas configurações para otimizar o desempenho.

## <a name="queue-depth"></a>Profundidade de fila
A profundidade de fila ou o comprimento da fila ou o tamanho da fila é o número de pedidos de e/s pendentes no sistema. O valor da profundidade de fila determina quantas operações de e/s que seu aplicativo pode alinhar, que irão processar os discos de armazenamento. Afeta todos os indicadores de desempenho do aplicativo de três que discutimos nas viz. neste artigo, IOPS, débito e latência.

Fila profundidade e vários segmentos estão intimamente relacionados. O valor de profundidade de fila indica quanto multithreading pode ser obtido pela aplicação. Se a profundidade de fila é grande, aplicativo pode executar outras operações ao mesmo tempo, em outras palavras, mais multithreading. Se a profundidade de fila é pequena, mesmo que a aplicação tem vários threads, ele não terá suficiente pedidos interessados para a execução simultânea.

Normalmente, a versão comercial aplicações não permitem que altere a profundidade de fila, porque se definir incorretamente fará mais mal do que bem. Aplicativos definirá o valor correto da profundidade de fila para obter o desempenho ideal. No entanto, é importante compreender esse conceito para que pode resolver problemas de desempenho com a sua aplicação. Também pode observar os efeitos da profundidade de fila ao executar as ferramentas de benchmark em seu sistema.

Alguns aplicativos fornecem definições para influenciar a profundidade de fila. Por exemplo, a definição de (grau máximo de paralelismo) MAXDOP no SQL Server é explicado na secção anterior. MAXDOP é uma forma para influenciar a profundidade de fila e multithreading, apesar de não alterar diretamente o valor de profundidade de fila do SQL Server.

*Profundidade de fila elevada*  
Linhas de uma profundidade de fila de alta segurança mais operações no disco. O disco sabe a próxima solicitação na sua fila antes do tempo. Conseqüentemente, o disco pode agendar operações com antecedência e processá-las numa seqüência ideal. Uma vez que a aplicação está a enviar mais pedidos para o disco, o disco pode processar mais IOs paralelas. Por fim, a aplicação poderá atingir o IOPS superior. Uma vez que a aplicação está a processar mais pedidos, também aumenta o débito total da aplicação.

Normalmente, um aplicativo pode alcançar um débito máximo com 8-16 + e/s pendentes por disco ligado. Se uma profundidade de fila é um, aplicativo é não enviar suficiente IOs para o sistema e processará menor a quantidade de num determinado período. Em outras palavras, a menos débito.

Por exemplo, no SQL Server, definindo o valor MAXDOP para uma consulta para "4" informa do SQL Server que pode utilizar até quatro núcleos para executar a consulta. SQL Server irá determinar o que é o melhor valor de profundidade de fila e o número de núcleos para a execução da consulta.

*Profundidade de fila ideal*  
Valor de profundidade de fila muito alto também tem suas desvantagens. Se o valor de profundidade de fila é demasiado elevada, a aplicação irá tentar IOPS muito elevado da unidade. A menos que o aplicativo tem discos persistentes com IOPS aprovisionadas suficientes, isso pode afetar negativamente a latências de aplicação. Fórmula a seguir mostra a relação entre o IOPS, a latência e a profundidade de fila.  
    ![](media/premium-storage-performance/image6.png)

Não deve configurar profundidade de fila para qualquer valor elevado, mas para um valor ideal, o que pode entregar suficiente IOPS para a aplicação sem afetar as latências. Por exemplo, se a latência de aplicação tem de ser 1 milissegundo, a profundidade de fila necessário para obter 5000 IOPS é, QD = 5000 x 0,001 = 5.

*Profundidade de fila para volumes repartidos*  
Para um volume repartido, manter uma profundidade de fila alto o suficiente, de modo a que cada disco tem uma profundidade de fila de pico individualmente. Por exemplo, considere um aplicativo que envia uma profundidade de fila de 2 e existem 4 discos no stripe. As duas solicitações de e/s serão feita dois discos e restantes dois discos de estar inativo. Por conseguinte, deve configure a profundidade de fila, de modo a que todos os discos podem estar ocupados. Fórmula abaixo mostra como determinar a profundidade de fila de volumes repartidos.  
    ![](media/premium-storage-performance/image7.png)

## <a name="throttling"></a>Limitação
Disposições de armazenamento Premium do Azure especificado o número de IOPS e débito consoante os tamanhos VM e os tamanhos de disco. Sempre que seu aplicativo tenta unidade IOPS ou débito acima estes limites de que a VM ou disco pode lidar com, o armazenamento Premium acelerá-los. Isso se manifesta na forma de degradação do desempenho em seu aplicativo. Isso pode significar latência superior, reduzir o débito ou IOPS de reduzir. Se não limitar o armazenamento Premium, sua completamente poderá falhar por que excedem o que são capazes de atingir seus recursos. Portanto, para evitar problemas de desempenho devido à limitação, sempre Aprovisione recursos suficientes para a sua aplicação. Leve em consideração o que discutimos nas seções de tamanhos de disco acima e tamanhos de VM. Padrão de referência é a melhor forma de descobrir quais recursos precisará para alojar a sua aplicação.

## <a name="benchmarking"></a>Padrão de referência
Padrão de referência é o processo de simulação de diferentes cargas de trabalho na sua aplicação e medir o desempenho de aplicações para cada carga de trabalho. Utilizando os passos descritos numa seção anterior, que reuniu os requisitos de desempenho do aplicativo. Ao executar o benchmark ferramentas nas VMs que aloja a aplicação, é possível determinar os níveis de desempenho que seu aplicativo pode alcançar com o armazenamento Premium. Nesta seção, fornecemos exemplos de comparações de benchmark entre uma VM DS14 padrão aprovisionado com discos de armazenamento Premium do Azure.

Usamos ferramentas comuns de benchmark Iometer e FIO, para Windows e Linux, respetivamente. Essas ferramentas gerar vários threads, simulando uma produção, como a carga de trabalho e medem o desempenho do sistema. Usando as ferramentas também pode configurar parâmetros como bloquear a profundidade tamanho e a fila, que normalmente não é possível alterar para uma aplicação. Isso lhe dá mais flexibilidade para orientar o máximo desempenho numa escala elevada VM aprovisionado com os discos premium para diferentes tipos de cargas de trabalho de aplicação. Para saber mais sobre cada ferramenta de benchmark visite [Iometer](http://www.iometer.org/) e [FIO](http://freecode.com/projects/fio).

Para seguir os exemplos abaixo, criar uma VM DS14 padrão e anexar discos de armazenamento Premium 11 a VM. Os discos de 11, configure 10 discos com o anfitrião de colocação em cache como "None" e do stripe-los num volume chamado NoCacheWrites. Configurar o anfitrião como "Só de leitura" colocação em cache no disco restante e criar um volume chamado CacheReads com este disco. Com esta configuração, será capaz de ver o máximo desempenho de leitura e escrita de uma VM DS14 padrão. Para obter passos detalhados sobre como criar uma VM de DS14 com discos premium, aceda a [criar e utilizar um armazenamento Premium de conta para um disco de dados de máquina virtual](../articles/virtual-machines/windows/premium-storage.md).

*Aquecer a Cache*  
O disco com o anfitrião só de leitura de colocação em cache será capaz de lhe dar mais elevada IOPS que o limite de disco. Para obter este máximo desempenho de leitura da cache do anfitrião, primeiro deve entender o cache do disco. Isto garante que a leitura do IOs que ferramenta de benchmark irá orientar no volume de CacheReads, na verdade, acessa o cache e não o disco diretamente. O resultado de acertos na cache em ESP adicionais da cache única ativada disco.

> **Importante:**  
> Deve entender o cache antes de executar o benchmark, sempre que a VM é reiniciada.
>
>

#### <a name="iometer"></a>Iometer
[Transferir a ferramenta de Iometer](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) na VM.

*Ficheiro de teste*  
Iometer utiliza um ficheiro de teste que é armazenado no volume no qual irá executar o teste de benchmark. Ele unidades de leituras e escritas neste ficheiro de teste para medir o disco IOPS e débito. Iometer cria este ficheiro de teste, se não tiver fornecido um. Crie um ficheiro de teste de 200GB chamado iobw.tst nos volumes CacheReads e NoCacheWrites.

*Especificações de acesso*  
As especificações, pedir o tamanho de e/s, % de leitura/escrita, % aleatório/sequenciais são configurados utilizando o separador "Especificações de acesso" no Iometer. Crie uma especificação de acesso para cada um dos cenários descritos abaixo. Criar as especificações de acesso e "Salvar" apropriadas com nome, como – RandomWrites\_8K, RandomReads\_8 K. Selecione a especificação correspondente ao executar o cenário de teste.

Um exemplo de especificações de acesso para o cenário de escrever o IOPS máximo é mostrado abaixo,  
    ![](media/premium-storage-performance/image8.png)

*Especificações de teste IOPS máximo*  
Para demonstrar as IOPs máximas, utilize o tamanho mais pequeno do pedido. Utilize o tamanho do pedido de 8K e criar especificações para escritas aleatórias e leituras.

| Especificação de acesso | Tamanho do pedido | % Aleatório | % De leitura |
| --- | --- | --- | --- |
| RandomWrites\_8K |8K |100 |0 |
| RandomReads\_8K |8K |100 |100 |

*Especificações de teste de débito máximo*  
Para demonstrar o débito máximo, utilize o maior tamanho do pedido. Utilize o tamanho do pedido de 64K e criar especificações para escritas aleatórias e leituras.

| Especificação de acesso | Tamanho do pedido | % Aleatório | % De leitura |
| --- | --- | --- | --- |
| RandomWrites\_64K |64K |100 |0 |
| RandomReads\_64K |64K |100 |100 |

*Executar o teste de Iometer*  
Execute os passos abaixo para entender cache

1. Criar duas especificações de acesso com valores apresentados abaixo,

   | Nome | Tamanho do pedido | % Aleatório | % De leitura |
   | --- | --- | --- | --- |
   | RandomWrites\_1 MB |1MB |100 |0 |
   | RandomReads\_1 MB |1MB |100 |100 |
1. Execute o teste de Iometer para inicializar o disco de cache com os parâmetros seguintes. Utilize três threads de trabalho para o volume de destino e uma profundidade de fila de 128. Defina a duração do teste de "Tempo de execução" para 2hrs na guia "Configuração de teste".

   | Cenário | Volume de destino | Nome | Duração |
   | --- | --- | --- | --- |
   | Inicialize o disco de Cache |CacheReads |RandomWrites\_1 MB |2hrs |
1. Execute o teste de Iometer para aquecer o disco de cache com os parâmetros seguintes. Utilize três threads de trabalho para o volume de destino e uma profundidade de fila de 128. Defina a duração do teste de "Tempo de execução" para 2hrs na guia "Configuração de teste".

   | Cenário | Volume de destino | Nome | Duração |
   | --- | --- | --- | --- |
   | Quente no disco de Cache |CacheReads |RandomReads\_1 MB |2hrs |

Depois do disco de cache é preparado, prossiga com os cenários de teste listados abaixo. Para executar o teste de Iometer, utilize, pelo menos, três threads de trabalho para **cada** volume de destino. Para cada thread de trabalho, selecione o volume de destino, defina a profundidade de fila e selecione uma das especificações de teste guardado, conforme mostrado na tabela abaixo, para executar o cenário de teste correspondente. A tabela também mostra os resultados esperados para IOPS e débito ao executar estes testes. Para todos os cenários, são utilizados um tamanho de e/s pequeno de 8KB e uma profundidade de fila de alta de 128.

| Cenário de teste | Volume de destino | Nome | Resultado |
| --- | --- | --- | --- |
| Um máximo de IOPS de leitura |CacheReads |RandomWrites\_8K |50 000 IOPS |
| Um máximo de IOPS de escrita |NoCacheWrites |RandomReads\_8K |64.000 IOPS |
| Um máximo de IOPS combinada |CacheReads |RandomWrites\_8K |100 000 IOPS |
| NoCacheWrites |RandomReads\_8K | &nbsp; | &nbsp; |
| Um máximo de MB/seg de leitura |CacheReads |RandomWrites\_64K |524 MB/seg |
| Um máximo de MB/s de escrita |NoCacheWrites |RandomReads\_64K |524 MB/seg |
| Combinado MB/seg |CacheReads |RandomWrites\_64K |1000 MB/seg |
| NoCacheWrites |RandomReads\_64K | &nbsp; | &nbsp; |

Seguem-se capturas de ecrã do Iometer resultados do teste para cenários IOPS e débito combinados.

*IOPS máximo combinado de leituras e gravações*  
![](media/premium-storage-performance/image9.png)

*Débito máximo combinado de leituras e gravações*  
![](media/premium-storage-performance/image10.png)

### <a name="fio"></a>FIO
FIO é uma ferramenta popular para o armazenamento de benchmark em VMs do Linux. Ele tem a flexibilidade para selecionar diferentes tamanhos de e/s, seqüenciais ou leituras aleatórias e escreve. Ele gera threads de trabalho ou processos para efetuar as operações de e/s especificadas. Pode especificar o tipo de operações de e/s, que cada thread de trabalho tem de efetuar o uso de arquivos de tarefa. Criamos um ficheiro de tarefa por cenário ilustrado nos exemplos abaixo. Pode alterar as especificações nesses arquivos de tarefa para diferentes cargas de trabalho em execução no armazenamento Premium. Os exemplos, estamos a utilizar uma execução de VM de 14 de DS Standard **Ubuntu**. Usar a mesma configuração descrita no início do [benchmark secção](#Benchmarking) e disponibilize a cache antes de executar os testes de benchmark.

Antes de começar, [transferir FIO](https://github.com/axboe/fio) e instalá-lo na sua máquina virtual.

Execute o seguinte comando para Ubuntu,

```
apt-get install fio
```

Nós usaremos quatro threads de trabalho para orientar operações de escrita e quatro segmentos de trabalho para operações de leitura que nos discos. Os operadores de escrita irão direcionar o tráfego no volume "nocache", que tem 10 discos com cache definido como "Nenhum". Os operadores de leitura irão direcionar o tráfego no volume "readcache", que tem o 1 disco com o conjunto de cache para "Só de leitura".

*Máximo IOPS de escrita*  
Crie o ficheiro de tarefa com especificações seguintes para obter o máximo IOPS de escrita. Nomeie-o "fiowrite.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Tenha em atenção as siga principais informações que estão de acordo com as diretrizes de design abordadas nas secções anteriores. Nestas especificações são essenciais para orientar o IOPS máximo,  

* Uma profundidade de fila de alta de 256.  
* Um tamanho de pequeno bloco de 8KB.  
* Vários threads para efetuar operações de escrita aleatórias.

Execute o seguinte comando para iniciar o teste FIO durante 30 segundos,  

```
sudo fio --runtime 30 fiowrite.ini
```

Enquanto executa o teste, será capaz de ver o número de escrever IOPS a VM e fornecendo os discos Premium. Conforme mostrado no exemplo abaixo, a VM DS14 está a fornecer a máxima limite de IOPS de 50 000 IOPS de escrita.  
    ![](media/premium-storage-performance/image11.png)

*IOPS de leitura do máximo*  
Crie o ficheiro de tarefa com especificações seguintes para obter o máximo IOPS de leitura. Nomeie-o "fioread.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Tenha em atenção as siga principais informações que estão de acordo com as diretrizes de design abordadas nas secções anteriores. Nestas especificações são essenciais para orientar o IOPS máximo,

* Uma profundidade de fila de alta de 256.  
* Um tamanho de pequeno bloco de 8KB.  
* Vários threads para efetuar operações de escrita aleatórias.

Execute o seguinte comando para iniciar o teste FIO durante 30 segundos,

```
sudo fio --runtime 30 fioread.ini
```

Enquanto executa o teste, será capaz de ver o número de ler IOPS a VM e fornecendo os discos Premium. Conforme mostrado no exemplo abaixo, a VM DS14 está a fornecer mais de 64.000 IOPS de leitura. Esta é uma combinação de disco e o desempenho da cache.  
    ![](media/premium-storage-performance/image12.png)

*IOPS máxima leitura e escrita*  
Criar o ficheiro de tarefa com o seguinte combinado de especificações para obter o máximo IOPS de escrita e leitura. Nomeie-o "fioreadwrite.ini".

```
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Tenha em atenção as siga principais informações que estão de acordo com as diretrizes de design abordadas nas secções anteriores. Nestas especificações são essenciais para orientar o IOPS máximo,

* Uma profundidade de fila de alta de 128.  
* Um tamanho de pequeno bloco de 4KB.  
* Leituras e escritas de vários threads que executam aleatório.

Execute o seguinte comando para iniciar o teste FIO durante 30 segundos,

```
sudo fio --runtime 30 fioreadwrite.ini
```

Enquanto executa o teste, poderá ver o número de leitura combinado e IOPS de escrita da VM e fornecendo os discos Premium. Conforme mostrado no exemplo abaixo, a VM DS14 está a fornecer mais de 100 000 leitura combinado e IOPS de escrita. Esta é uma combinação de disco e o desempenho da cache.  
    ![](media/premium-storage-performance/image13.png)

*Máximo combinado a taxa de transferência*  
Para obter o máximo combinado de leitura e escrita a taxa de transferência, utilize um tamanho de bloco maior e a profundidade de fila grandes com vários threads, execução de leituras e gravações. Pode utilizar um tamanho de bloco de 64KB e a profundidade de fila de 128.

## <a name="next-steps"></a>Próximos Passos
Saiba mais sobre o armazenamento Premium do Azure:

* [Armazenamento Premium: Armazenamento de Elevado Desempenho para Cargas de Trabalho de Máquinas Virtuais do Azure](../articles/virtual-machines/windows/premium-storage.md)  

Para usuários do SQL Server, leia os artigos sobre melhores práticas de desempenho para o SQL Server:

* [Melhores práticas de desempenho para SQL Server em máquinas virtuais do Azure](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md)
* [Armazenamento Premium do Azure proporciona um desempenho mais elevado para o SQL Server na VM do Azure](http://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx)
