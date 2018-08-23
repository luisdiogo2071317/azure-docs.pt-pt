---
title: Perguntas comuns sobre o Microsoft Azure Service Fabric | Documentos da Microsoft
description: Perguntas freqüentes sobre o Service Fabric e suas respostas
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: ''
ms.assetid: 5a179703-ff0c-4b8e-98cd-377253295d12
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: chackdan
ms.openlocfilehash: 0b731e94675992e59f79b61a2f3a15fa20bdf8a7
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42057381"
---
# <a name="commonly-asked-service-fabric-questions"></a>Service Fabric perguntas mais frequentes

Existem muitas perguntas freqüentes sobre o Service Fabric pode fazer e como ele deve ser usado. Este documento aborda muitas dessas perguntas comuns e suas respostas.

## <a name="cluster-setup-and-management"></a>A configuração do cluster e gestão

### <a name="how-do-i-roll-back-my-service-fabric-cluster-certificate"></a>Como eu agrego volta meu certificado de cluster do Service Fabric?

A reverter qualquer atualização ao seu aplicativo requer a deteção de falhas de estado de funcionamento antes do quórum de cluster do Service Fabric consolidação da alteração; só podem ser efetuadas o alterações confirmadas rollforward. Engenheiro de escalonamento através de serviços de suporte ao cliente, poderá ser necessária para recuperar o cluster, se foi introduzida uma alteração de certificado de quebra de não monitorizado.  [Atualização da aplicação do Service Fabric](https://review.docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade?branch=master) aplica-se [parâmetros da atualização da aplicação](https://review.docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-parameters?branch=master), e oferece promessa de atualização de um período de indisponibilidade de zero.  Após a nossa aplicação recomendada monitorizados de modo de atualização, progresso automática nos domínios de atualização é baseado em verificações de estado de funcionamento falha back sem interrupção, passando automaticamente se atualizar um serviço predefinido.
 
Se o cluster ainda está aproveitando a propriedade clássica de Thumbprint do certificado no modelo do Resource Manager, é recomendado que [cluster de alteração de thumbprint do certificado para o nome comum](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-change-cert-thumbprint-to-cn), para tirar partido dos segredos modernos funcionalidades de gestão.

### <a name="can-i-create-a-cluster-that-spans-multiple-azure-regions-or-my-own-datacenters"></a>Pode criar um cluster que abranja diversas regiões do Azure ou o meus próprio centros de dados?

Sim. 

A principal tecnologia de clustering do Service Fabric pode ser usada para combinar máquinas em execução em qualquer lugar do mundo, desde que eles têm conectividade de rede entre si. No entanto, criar e executar o cluster podem ser complicados.

Se estiver interessado neste cenário, é recomendável que entrar ou contacte através do [lista de problemas do Github de recursos de infraestrutura do serviço](https://github.com/azure/service-fabric-issues) ou através do seu representante de suporte para obter orientações adicionais. A equipe de recursos de infraestrutura do serviço está trabalhando para oferecer clareza adicional, orientações e recomendações para este cenário. 

Alguns aspetos a ter em conta: 

1. O recurso de cluster do Service Fabric no Azure é regional hoje em dia, como conjuntos de dimensionamento da máquina virtual que o cluster é criado. Isso significa que, se ocorrer uma falha regional podem perder a capacidade para gerir o cluster através do Azure Resource Manager ou o portal do Azure. Isto pode acontecer, apesar do cluster permanece em execução e seria capaz de interagir diretamente com o mesmo. Além disso, hoje em dia Azure não oferece a capacidade de ter uma única rede virtual que pode ser utilizada em várias regiões. Isso significa que um cluster de várias regiões no Azure requer um [endereços IP públicos para cada VM em conjuntos de dimensionamento de VM](../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md#public-ipv4-per-virtual-machine) ou [Gateways de VPN do Azure](../vpn-gateway/vpn-gateway-about-vpngateways.md). Essas opções de funcionamento em rede possuem diferentes impactos nos custos, desempenho, e ao design de aplicativo algum grau, por isso, cuidado análise e planeamento é necessária antes da criação desse ambiente.
2. A manutenção, gestão, e monitorização destas máquinas pode se tornar complicado, especialmente quando estendido em _tipos_ dos ambientes, tais como entre fornecedores de serviços cloud diferentes ou entre recursos no local e o Azure. Deve ter cuidado para garantir que as atualizações, monitorização, gestão e diagnóstico é compreendido para o cluster e as aplicações antes de executar cargas de trabalho de produção em tal ambiente. Se já tiver experiência solucionar esses problemas no Azure ou dentro de seus próprios datacenters, em seguida, é provável que essas mesmas soluções podem ser aplicadas quando a criação ou a executar o cluster do Service Fabric. 

### <a name="do-service-fabric-nodes-automatically-receive-os-updates"></a>Nós do Service Fabric recebe automaticamente as atualizações do SO?

Não hoje em dia, mas também se trata de uma solicitação comum que tenciona do Azure para entregar.

Até lá, temos [fornecido um aplicativo](service-fabric-patch-orchestration-application.md) que os sistemas de operativos sob os nós do Service Fabric se manter atualizado e corrigida.

O desafio com as atualizações do SO é que, normalmente, requer um reinício da máquina, o que resulta em perda de disponibilidade temporário. Por si só, isso não é um problema, uma vez que o Service Fabric automaticamente irá redirecionar o tráfego para os serviços para outros nós. No entanto, se as atualizações do SO não são coordenadas entre o cluster, existe o potencial que muitos de nós descer ao mesmo tempo. Tais reinícios simultâneos podem causar a perda de disponibilidade completa para um serviço ou em, pelo menos, para uma partição específica (para um serviço com estado).

No futuro, planeamos oferecer suporte uma política de atualização de SO que é totalmente automatizada e coordenada entre domínios de atualização, garantindo que a disponibilidade é mantida apesar das reinicializações e outras falhas inesperadas.

### <a name="can-i-use-large-virtual-machine-scale-sets-in-my-sf-cluster"></a>Pode utilizar os conjuntos de dimensionamento de máquinas virtuais de grande no meu cluster SF? 

**Resposta de curta** - não. 

**Há muito tempo a responder** – embora os conjuntos de dimensionamento de máquinas virtuais de grande permitem-lhe dimensionar uma máquina virtual de instâncias de VM de até 1000 de conjunto de dimensionamento, isso é feito pelo uso de grupos de colocação (PGs). Domínios de falha (FDs) e domínios de atualização (UDs) só são consistentes dentro de uma colocação grupo Service fabric utiliza domínios de falha e domínios de atualização para tomar decisões de colocação das suas instâncias de serviço/réplicas do serviço. Uma vez que os domínios de falha e domínios de atualização são comparáveis apenas dentro de um grupo de colocação, SF não é possível usá-lo. Por exemplo, se a VM1 no PG1 tiver uma topologia de FD = 0 e VM9 no PG2 tem uma topologia de FD = 4, não significa que VM1 e VM2 estiverem em dois Racks de Hardware diferentes, que, por conseguinte, SF não é possível utilizar os valores de FD nesse caso para tomar decisões de colocação.

Existem outros problemas com conjuntos de dimensionamento de máquinas virtuais de grande atualmente, como a falta de nível 4 de suporte de balanceamento de carga. Consulte para [detalhes em grandes conjuntos de dimensionamento](../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md)



### <a name="what-is-the-minimum-size-of-a-service-fabric-cluster-why-cant-it-be-smaller"></a>O que é o tamanho mínimo de um cluster do Service Fabric? Por que não pode ser menor?

O tamanho mínimo suportado para um cluster do Service Fabric executar cargas de trabalho de produção é cinco nós. Para cenários de dev/test, damos suporte a três clusters de nós.

Estes valores mínimos existem porque o cluster do Service Fabric é executado um conjunto de serviços de monitorização de estado do sistema, incluindo o serviço de nomenclatura e o Gestor de ativação pós-falha. Estes serviços, que controlam os serviços que foram implementados para o cluster e em que atualmente estão hospedados, dependem da consistência forte. Essa consistência forte, por sua vez, depende da capacidade de adquirir um *quórum* para qualquer determinada atualização para o estado desses serviços, onde um quórum representa uma maioria rigorosa das réplicas (N/2 + 1) para um determinado serviço.

Com esse segundo plano, vamos examinar algumas configurações de cluster possíveis:

**Um nó**: esta opção não fornece elevada disponibilidade desde a perda do nó único para qualquer motivo significa que a perda de todo o cluster.

**Dois nós**: um quórum de um serviço implementado em dois nós (N = 2) é 2 (2/2 + 1 = 2). Quando uma única réplica é perdida, é impossível criar um quórum. Uma vez que executar uma atualização de serviço requer temporariamente interromper uma réplica, isso não é uma configuração de útil.

**Três nós**: com três nós (N = 3), o requisito para criar um quórum ainda é dois nós (3/2 + 1 = 2). Isso significa que pode perder um nó individual e ainda manter o quórum.

A configuração de cluster de três nós é suportada para dev/test porque pode efetuar atualizações e resistir a falhas de nó individual, com segurança, desde que não aconteçam em simultâneo. Para cargas de trabalho de produção, tem de ser resilientes a uma falha deste tipo simultânea, pelo que são necessários nós de cinco.

### <a name="can-i-turn-off-my-cluster-at-nightweekends-to-save-costs"></a>Pode desativar meu cluster na noite/fins de semana para reduzir os custos?

Geralmente, não. Service Fabric armazena o estado em discos locais, efémeros, que significa que se a máquina virtual for movida para outro anfitrião, os dados não move com ele. Na operação normal, que não é um problema como o novo nó seja colocado atualizadas por outros nós. No entanto, se parar todos os nós e reiniciá-las mais tarde, existe uma possibilidade significativa que a maioria de nós começam no novos anfitriões e marca o sistema não é possível recuperar.

Se gostaria de criar clusters para testar a aplicação antes de ser implantado, é recomendável criar dinamicamente esses clusters como parte da sua [pipeline de implementação de integração contínua/contínua](service-fabric-tutorial-deploy-app-with-cicd-vsts.md).


### <a name="how-do-i-upgrade-my-operating-system-for-example-from-windows-server-2012-to-windows-server-2016"></a>Como posso atualizar o meu sistema operacional (por exemplo, do Windows Server 2012 para Windows Server 2016)?

Enquanto estamos a trabalhar numa experiência aprimorada, hoje em dia, é responsável pela atualização. Tem de atualizar a imagem do SO nas máquinas virtuais do cluster em uma VM ao mesmo tempo. 

### <a name="can-i-encrypt-attached-data-disks-in-a-cluster-node-type-virtual-machine-scale-set"></a>Pode encriptar discos de dados anexados num tipo de nó de cluster (conjunto de dimensionamento de máquina virtual)?
Sim.  Para obter mais informações, consulte [criar um cluster com discos de dados anexados](../virtual-machine-scale-sets/virtual-machine-scale-sets-attached-disks.md#create-a-service-fabric-cluster-with-attached-data-disks), [encriptar discos (PowerShell)](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-ps.md), e [encriptar discos (CLI)](../virtual-machine-scale-sets/virtual-machine-scale-sets-encrypt-disks-cli.md).

### <a name="what-are-the-directories-and-processes-that-i-need-to-exclude-when-running-an-anti-virus-program-in-my-cluster"></a>Quais são os diretórios e os processos que preciso serem excluídos ao executar um programa de software antivírus no meu cluster?

| **Diretórios de excluído antivírus** |
| --- |
| Programa Files\Microsoft Service Fabric |
| FabricDataRoot (a partir de configuração de cluster) |
| FabricLogRoot (a partir de configuração de cluster) |

| **Antivírus excluídos processos** |
| --- |
| Fabric.exe |
| FabricHost.exe |
| FabricInstallerService.exe |
| FabricSetup.exe |
| FabricDeployer.exe |
| ImageBuilder.exe |
| FabricGateway.exe |
| FabricDCA.exe |
| FabricFAS.exe |
| FabricUOS.exe |
| FabricRM.exe |
| FileStoreService.exe |
 
### <a name="how-can-my-application-authenticate-to-keyvault-to-get-secrets"></a>Como o meu aplicativo pode autenticar para o Cofre de chaves para obter segredos?
Seguem-se significa para a sua aplicação obter as credenciais para autenticar no Cofre de chaves:

A. Durante a tarefa de compilação/remessa seus aplicativos, pode extrair um certificado para o pacote de dados da sua aplicação SF e utilizá-lo para autenticar para o Cofre de chaves.
B. Para anfitriões MSI ativada do conjunto de dimensionamento de máquinas virtuais, pode desenvolver um SetupEntryPoint simples do PowerShell para a sua aplicação SF conseguir [um token de acesso do ponto de extremidade MSI](https://docs.microsoft.com/en-us/azure/active-directory/managed-service-identity/how-to-use-vm-token)e, em seguida, [recuperar seus segredos do Cofre de chaves](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/Get-AzureKeyVaultSecret?view=azurermps-6.5.0)

## <a name="application-design"></a>Design do aplicativo

### <a name="whats-the-best-way-to-query-data-across-partitions-of-a-reliable-collection"></a>O que é a melhor forma de consultar dados em partições de uma coleção fiável?

As coleções fiáveis são normalmente [particionada](service-fabric-concepts-partitioning.md) para permitir o Escalamento horizontal para melhor desempenho e o débito. Isso significa que o estado para um determinado serviço pode ser distribuído em dezenas ou centenas de máquinas. Para realizar operações durante esse conjunto de dados completo, tem algumas opções:

- Crie um serviço que consulta todas as partições de outro serviço para obter os dados necessários.
- Crie um serviço que pode receber dados de todas as partições de outro serviço.
- Envie periodicamente dados para um repositório externo a partir de cada serviço. Essa abordagem é apropriada somente se as consultas que estiver a efetuar não fazem parte de sua lógica de negócio de núcleo.


### <a name="whats-the-best-way-to-query-data-across-my-actors"></a>O que é a melhor forma de consultar dados em meu atores?

Atores são concebidos para serem independentes unidades de estado e de computação, pelo que não é recomendado para executar consultas abrangentes do Estado de ator em tempo de execução. Se tiver uma necessidade de consulta entre o conjunto completo de estado do ator, deve considerar o:

- Substituir seus serviços de ator com o reliable services com monitorização de estado, para que o número de rede solicitações para recolher todos os dados do número de atores para o número de partições no seu serviço.
- Criando seus atores para enviar periodicamente o respetivo estado de um armazenamento externo para consultar mais fácil. Como acima, esta abordagem só é viável se as consultas que estiver a efetuar não são necessárias para o comportamento de tempo de execução.

### <a name="how-much-data-can-i-store-in-a-reliable-collection"></a>A quantidade de dados posso armazenar numa Reliable Collection?

Serviços fiáveis normalmente são particionados, para que a quantidade que pode armazenar apenas é limitada pelo número de máquinas que ter no cluster e a quantidade de memória disponível nessas máquinas.

Por exemplo, suponha que tenha uma coleção fiável num serviço com 100 partições e 3 réplicas, armazenamento de objetos de média de 1 kb de tamanho. Agora suponha que tem um cluster de 10 máquina com 16gb de memória por máquina. Para manter a simplicidade e para ser conservador, partem do princípio de que o sistema operativo e serviços do sistema, o tempo de execução do Service Fabric e os serviços de consumam 6gb de que, deixando de 10gb por máquina ou 100 gb para o cluster.

Tendo em mente que cada objeto tem de ser armazenado três vezes (uma primária e duas réplicas), teria memória suficiente para aproximadamente 35 milhões objetos na sua coleção quando funciona na capacidade total. No entanto, recomendamos que está a ser resiliente à perda simultânea de um domínio de falha e um domínio de atualização, que representa cerca de 1/3 da capacidade e seria reduzir o número de aproximadamente 23 milhões.

Tenha em atenção que este cálculo também supõe que:

- Se a distribuição de dados pelas partições é aproximadamente uniforme ou que está a comunicar as métricas de carregamento para o Gestor de recursos do Cluster. Por predefinição, o Service Fabric carrega saldo com base na contagem de réplica. No exemplo anterior, que poderia colocar 10 réplicas primárias e 20 réplicas secundárias em cada nó no cluster. Isso funciona bem para a carga é distribuída uniformemente pelas partições. Se a carga não é mesmo, tem de comunicar carga para que o Gestor de recursos pode pack réplicas menores em conjunto e permitir maior réplicas consumir mais memória num nó individual.

- Que o serviço fiável em questão é apenas um Estado de armazenamento no cluster. Uma vez que pode implementar múltiplos serviços a um cluster, tem de estar atento aos recursos que cada uma tem de executar e gerir o seu estado.

- Que o próprio cluster não está crescendo ou redução. Se adicionar mais máquinas, o Service Fabric serão reequilibrar as réplicas para aproveitar a capacidade adicional de até que o número de máquinas excede o número de partições no seu serviço, uma vez que uma réplica individual não pode abranger máquinas. Por outro lado, se reduzir o tamanho do cluster ao remover máquinas, as réplicas são incluídas mais rigidamente e tem menos capacidade geral.

### <a name="how-much-data-can-i-store-in-an-actor"></a>A quantidade de dados posso armazenar num ator?

Como com o reliable services, a quantidade de dados que pode armazenar num serviço de ator é limitada apenas pelo espaço total em disco e memória disponível em todos os nós do cluster. No entanto, atores individuais são mais eficazes quando são utilizadas para encapsular uma pequena quantidade de estado e a lógica comercial associado. Como regra geral, um ator individual deve ter o estado que é medido em quilobytes.

## <a name="other-questions"></a>Outras perguntas

### <a name="how-does-service-fabric-relate-to-containers"></a>Como é que o Service Fabric se relaciona com contentores?

Os contentores oferecem uma forma simples de serviços de pacote e suas dependências, de modo que eles executam consistentemente em todos os ambientes e podem operar de maneira isolada numa única máquina. Service Fabric oferece uma forma de implementar e gerir serviços, incluindo [serviços que foram empacotados num contêiner](service-fabric-containers-overview.md).

### <a name="are-you-planning-to-open-source-service-fabric"></a>Está a planear para o código-fonte aberto Service Fabric?

Temos aberto partes do Service Fabric ([estrutura de serviços fiáveis](https://github.com/Azure/service-fabric-services-and-actors-dotnet), [framework dos reliable actors](https://github.com/Azure/service-fabric-services-and-actors-dotnet), [bibliotecas de integração do ASP.NET Core](https://github.com/Azure/service-fabric-aspnetcore), [ Service Fabric Explorer](https://github.com/Azure/service-fabric-explorer), e [CLI do Service Fabric](https://github.com/Azure/service-fabric-cli)) no GitHub e aceitar contribuições da Comunidade para esses projetos. 

Estamos [anunciou recentemente](https://blogs.msdn.microsoft.com/azureservicefabric/2018/03/14/service-fabric-is-going-open-source/) que estamos a planear aberto o tempo de execução do Service Fabric. Neste momento temos o [repositório do Service Fabric](https://github.com/Microsoft/service-fabric/) até no GitHub com o Linux criar e testar ferramentas, que significa que pode clonar o repositório, crie o Service Fabric para Linux, executar testes básicos, problemas em aberto e submeter pedidos pull. Estamos a trabalhar arduamente para obter o Windows, ambiente de compilação migrado, juntamente com um ambiente completo de CI.

Siga os [blog do Service Fabric](https://blogs.msdn.microsoft.com/azureservicefabric/) para obter mais detalhes à medida que estão a ser anunciados.

## <a name="next-steps"></a>Passos Seguintes

- [Saiba mais sobre conceitos-chave Service Fabric e melhores práticas](https://mva.microsoft.com/en-us/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=tbuZM46yC_5206218965)
