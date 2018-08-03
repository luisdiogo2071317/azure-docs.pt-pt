---
title: Elevada disponibilidade e recuperação após desastre do SAP HANA no Azure (instâncias grandes) | Documentos da Microsoft
description: Estabelecer a elevada disponibilidade e o plano de recuperação após desastre do SAP HANA no Azure (instâncias grandes)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/27/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 554991c7c0f11a095a11ae24dbb693a1a3ba50fd
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39430125"
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>Instâncias grandes do SAP HANA elevada disponibilidade e recuperação após desastre no Azure 

>[!IMPORTANT]
>Esta documentação não é nenhuma substituição da documentação de administração do SAP HANA ou SAP Notes. Espera-se que o leitor tenha uma compreensão sólida do e experiência em administração de SAP HANA e operações, especialmente com os tópicos de cópia de segurança, restauro, elevada disponibilidade e recuperação após desastre. Nesta documentação, capturas de ecrã do SAP HANA Studio são apresentadas. Conteúdo, estrutura e a natureza das telas de ferramentas de administração do SAP e das ferramentas propriamente ditas podem ser alteradas a partir do SAP HANA versão para versão.

É importante que exerce o etapas e os processos executados no seu ambiente e com as suas versões do HANA e versões. Alguns processos descritos nesta documentação são simplificados para uma melhor compreensão geral e não se destinam a ser utilizado como obter os passos detalhados para handbooks operação eventual. Se quiser criar handbooks de operação para suas configurações, terá de testar e testar seus processos e os processos relacionados com suas configurações específicas de documentos. 


Elevada disponibilidade e recuperação após desastre (DR) são aspectos cruciais da execução do seu SAP HANA de missão crítica no servidor do Azure (instâncias grandes). É importante trabalhar com o SAP, o integrador de sistemas ou Microsoft corretamente arquitetar e implementar o direito de elevada disponibilidade e estratégias de recuperação após desastre. Também é importante considerar o objetivo de ponto de recuperação (RPO) e o objetivo de tempo de recuperação, que são específicas do ambiente.

A Microsoft suporta alguns recursos de alta disponibilidade do SAP HANA com instâncias grandes do HANA. Estas funcionalidades incluem:

- **Replicação de armazenamento**: capacidade do sistema de armazenamento para replicar todos os dados para outro carimbo de instância grande do HANA noutra região do Azure. SAP HANA funciona independentemente neste método. Esta funcionalidade é o mecanismo de recuperação de desastres do padrão oferecido para as instâncias grandes do HANA.
- **Replicação do sistema HANA**: A [replicação de todos os dados no SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html) para um sistema separado do SAP HANA. O objetivo de tempo de recuperação é minimizado através da replicação de dados em intervalos regulares. SAP HANA suporta assíncronos e síncronos modos dentro da memória e síncronos. Modo síncrono é utilizado apenas para sistemas de SAP HANA que estão dentro do mesmo datacenter ou inferior a 100 km de distância. Com o design atual de carimbos de instância grande do HANA, a replicação do sistema HANA pode ser utilizada para elevada disponibilidade numa região apenas. Replicação do sistema HANA requer um proxy inverso de terceiros ou o componente de roteamento para configurações de recuperação após desastre para outra região do Azure. 
- **Ativação pós-falha automática do anfitrião**: uma solução de recuperação de falha local para SAP HANA, que é uma alternativa para a replicação de sistema do HANA. Se o nó principal ficar indisponível, configure um ou mais nós de SAP HANA em espera no modo de escalamento horizontal e SAP HANA automaticamente efetua a ativação pós-falha para um nó em modo de espera.

SAP HANA no Azure (instâncias grandes) é disponibilizado em duas regiões do Azure em quatro áreas geopolíticas (dos EUA, Austrália, Europa e Japão). Duas regiões dentro de uma área geopolítica que alojam os carimbos de data / instância grande do HANA estão ligados aos circuitos de rede dedicado separado. Estes são utilizados para replicar os instantâneos de armazenamento para fornecer métodos de recuperação de desastres. A replicação não for estabelecida por predefinição, mas é configurada para os clientes que adquirirem a funcionalidade de recuperação após desastre. Replicação de armazenamento é dependente da utilização de instantâneos de armazenamento para instâncias grandes do HANA. Não é possível escolher uma região do Azure como uma região de DR é uma área geopolíticas diferentes. 

A tabela seguinte mostra as combinações e atualmente elevados disponibilidade e após desastre recuperação métodos suportados:

| Cenário suportado no HANA nas instâncias grandes | Opção de elevada disponibilidade | Opção de recuperação após desastre | Comentários |
| --- | --- | --- | --- |
| Nó único | Não está disponível. | Configuração de DR dedicada.<br /> Configuração de Multipurpose DR. | |
| Ativação pós-falha automática do anfitrião: Escalamento horizontal (com ou sem o modo de espera)<br /> incluindo 1 + 1 | Possível com o modo de espera que a função do Active Directory.<br /> HANA controla o comutador de função. | Configuração de DR dedicada.<br /> Configuração de Multipurpose DR.<br /> Sincronização de DR através de replicação de armazenamento. | São anexados a conjuntos de volume do HANA para todos os nós.<br /> Site de DR tem de ter o mesmo número de nós. |
| Replicação do sistema HANA | Possível com a configuração primária ou secundária.<br /> Secundário se move para a função primária num caso de ativação pós-falha.<br /> Replicação do sistema HANA e o sistema operacional controla ativação pós-falha. | Configuração de DR dedicada.<br /> Configuração de Multipurpose DR.<br /> Sincronização de DR através de replicação de armazenamento.<br /> DR através de replicação de sistema HANA ainda não é possível sem componentes de terceiros. | Um conjunto separado de volumes de disco são anexados a cada nó.<br /> Apenas os volumes de disco de réplica secundária no site de produção são replicados para a localização de DR.<br /> Um conjunto de volumes é necessário no site de DR. | 

Uma configuração de DR dedicada é onde a unidade de instância grande do HANA no site de DR não é utilizada para executar qualquer carga de trabalho ou sistema de não produção. A unidade é passiva e é implementada apenas se for executada uma ativação pós-falha de desastre. No entanto, esta configuração não é uma opção preferencial para muitos clientes.

Consultar [cenários suportados de HLI](hana-supported-scenario.md) para obter detalhes de ethernet e esquema de armazenamento para a sua arquitetura.

> [!NOTE]
> [Implementações do SAP HANA MCOD](https://launchpad.support.sap.com/#/notes/1681092) (várias instâncias de HANA numa só unidade) como a sobreposição de cenários de trabalho com o HA e DR métodos listados na tabela. Uma exceção é o uso do HANA System Replication com um cluster de ativação pós-falha automática com base em Pacemaker. Nesse caso suporta apenas uma instância HANA por unidade. Para [SAP HANA MDC](https://launchpad.support.sap.com/#/notes/2096000) implementações, apenas métodos HA e DR com base no armazenamento funcionam se mais do que um inquilino é implementado. Com um inquilino implementado, todos os métodos listados são válidos.  

Uma configuração de DR multipurpose é onde a unidade de instância grande do HANA no site de DR é executada uma carga de trabalho de não produção. Em caso de desastre, encerrar o sistema de não produção, montar os conjuntos de armazenamento replicado volume (adicionais) e, em seguida, inicie a instância do HANA de produção. A maioria dos clientes que utilizam a funcionalidade de recuperação de desastres de instância grande do HANA Utilize esta configuração. 


Pode encontrar mais informações sobre o SAP HANA de elevada disponibilidade nos seguintes artigos SAP: 

- [Documento técnico de alta disponibilidade do SAP HANA](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [Guia de administração do SAP HANA](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [Vídeo de SAP HANA Academy a replicação do sistema do SAP HANA](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [Nota de suporte #1999880 – perguntas frequentes sobre o SAP HANA System Replication de SAP](https://apps.support.sap.com/sap/support/knowledge/preview/en/1999880)
- [SAP suporte Nota 2165547 # – SAP HANA novamente a cópia de segurança e restauro num ambiente de replicação de sistema do SAP HANA](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [SAP suporte Nota #1984882 – utilizar o SAP HANA System Replication para o Exchange de Hardware com o tempo de inatividade mínimo/Zero](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="network-considerations-for-disaster-recovery-with-hana-large-instances"></a>Considerações de rede para recuperação após desastre com instâncias grandes do HANA

Para tirar partido da funcionalidade de recuperação após desastre do HANA nas instâncias grandes, terá de criar a conectividade de rede para as duas regiões do Azure. Precisa de uma ligação do circuito do ExpressRoute do Azure no local na sua região do Azure principal e outra ligação do circuito no local para a sua região de recuperação após desastre. Esta medida abrange uma situação em que há um problema com uma região do Azure, incluindo uma localização de Router de periferia Enterprise (MSEE) da Microsoft.

Como uma segunda medida, pode ligar-se todas as redes virtuais do Azure que se ligam ao SAP HANA no Azure (instâncias grandes) numa única região para um circuito do ExpressRoute que se liga instâncias grandes do HANA na outra região. Com isso *cruzada ligar*serviços em execução no Azure, rede virtual na região 1 pode ligar a unidades de instância grande do HANA região 2 e o caminho inverso. Esta medida aborda um caso em que apenas uma das localizações MSEE que se liga a sua localização no local com o Azure fica offline.

O gráfico seguinte ilustra uma configuração resiliente para casos de recuperação de desastres:

![Configuração ideal para recuperação após desastre](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)



## <a name="other-requirements-with-hana-large-instances-storage-replication-for-disaster-recovery"></a>Outros requisitos com a replicação de armazenamento de instâncias grandes do HANA para recuperação após desastre

Além dos requisitos anteriores para uma configuração de recuperação após desastre com instâncias grandes do HANA, tem de:

- Solicitar o SAP HANA em SKUs de Azure (instâncias grandes) do mesmo tamanho como seu SKUs de produção e implementá-las na região de recuperação após desastre. Nas implementações de cliente atual, estas instâncias são utilizadas para executar as instâncias do HANA de não produção. Estas configurações são denominadas *multipurpose configurações de DR*.   
- Solicitar armazenamento adicional no site de DR para cada um dos seus SAP HANA no Azure (instâncias grandes) SKUs que pretende recuperar no site de recuperação após desastre. Compra de armazenamento adicional permite-lhe alocar os volumes de armazenamento. Pode alocar os volumes que são o destino da replicação de armazenamento da sua região do Azure de produção para a recuperação após desastre de região do Azure.
- No caso, em que tiver configurado HSR primário e configurar a replicação de armazenamento com base no site de DR, tem de comprar armazenamento adicional no site de DR por isso, os dois principais e os dados de nós secundários obtém replicados para o site de DR.

 

## <a name="backup-and-restore"></a>Cópia de segurança e restauro

Um dos aspectos mais importantes para bancos de dados operacionais é protegê-los contra eventos catastróficos. A causa destes eventos pode ser qualquer coisa, desde desastres naturais para erros de usuário simples.

Cópia de segurança de uma base de dados, com a capacidade de restaurá-lo para qualquer ponto no tempo (por exemplo, antes que alguém eliminado dados críticos), permite que o restauro para um Estado que é o mais próximo possível da forma que era antes da interrupção.

Dois tipos de cópias de segurança devem ser executados para obter melhores resultados:

- As cópias de segurança de base de dados: cópias de segurança completas, incrementais ou diferenciais
- Backups de log de transação

Para além das cópias de segurança de base de dados completo realizadas em nível de aplicativo, pode executar cópias de segurança com instantâneos de armazenamento. Instantâneos de armazenamento não substituem backups de log de transação. Backups de log de transação permanecem importantes para restaurar a base de dados para um determinado ponto no tempo ou para esvaziar os registos de transações consolidadas já. No entanto, os instantâneos de armazenamento podem acelerar a recuperação ao fornecer rapidamente uma imagem de roll-forward da base de dados. 

SAP HANA no Azure (instâncias grandes) oferece duas opções de cópia de segurança e restauro:

- Tipo faça mesmo (DIY). Depois de calcular para garantir que existe espaço suficiente em disco, efetue o banco de dados completo e backups de log utilizando um dos seguintes métodos de cópia de segurança de disco. Pode criar cópias de segurança diretamente para os volumes anexados às unidades instância grande do HANA, ou para partilhas de ficheiros de rede (NFS) que são configurados numa máquina virtual do Azure (VM). No último caso, os clientes a configurar uma VM do Linux no Azure, anexar o armazenamento do Azure para a VM e partilhe o armazenamento através de um servidor NFS configurado nesta VM. Se efetuar a cópia de segurança face a volumes que anexar diretamente às unidades de instância grande do HANA, terá de copiar as cópias de segurança para uma conta de armazenamento do Azure (depois de configurar uma VM do Azure que exporta as partilhas NFS baseiam-se no armazenamento do Azure). Também pode utilizar um cofre de cópias de segurança do Azure ou o armazenamento de esporádico do Azure. 

   Outra opção é usar uma ferramenta de proteção de dados de terceiros para armazenar as cópias de segurança depois são copiados para uma conta de armazenamento do Azure. A opção de cópia de segurança faça mesmo também poderá ser necessária para os dados que precise armazenar por períodos mais longos de tempo para fins de auditoria e conformidade. Em todos os casos, as cópias de segurança são copiadas para as partilhas NFS representadas através de uma VM e o armazenamento do Azure.

- Infraestrutura de cópia de segurança e restaurar a funcionalidade. Também pode utilizar a cópia de segurança e restaurar a funcionalidade que fornece a infra-estrutura subjacente do SAP HANA no Azure (instâncias grandes). Esta opção atende a necessidade de cópias de segurança e restauros rápidos. O restante desta seção aborda a funcionalidade de cópia de segurança e restauro é oferecida com instâncias grandes do HANA. Esta secção também inclui a cópia de segurança de relação e restauro tem que desastre funcionalidade de recuperação oferecidos pelo instâncias grandes do HANA.

>   [!NOTE]
>   A tecnologia de instantâneo que é usada pela infra-estrutura subjacente de instâncias grandes do HANA tem uma dependência no instantâneos do SAP HANA. Neste momento, os instantâneos de SAP HANA não funcionam em conjunto com vários inquilinos de contentores de multi-inquilino da base de dados do SAP HANA. Se apenas um inquilino é implementado, os instantâneos de SAP HANA fazer o trabalho e este método pode ser utilizado.

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Utilizar instantâneos de armazenamento do SAP HANA no Azure (instâncias grandes)

A infraestrutura de armazenamento subjacente do SAP HANA no Azure (instâncias grandes) suporta instantâneos de armazenamento de volumes. Cópia de segurança e restauro de volumes é suportada com as seguintes considerações:

- Em vez de cópias de segurança completa da base de dados, são tirados instantâneos de volume de armazenamento com frequência.
- Quando acionar um instantâneo sobre /hana/data e /hana/shared (inclui /usr/sap) volumes, a tecnologia de instantâneo inicia um instantâneo antes de ser executada o instantâneo de armazenamento do SAP HANA. Este instantâneo do SAP HANA é o ponto de instalação para restaurações de eventual registo após a recuperação do instantâneo de armazenamento. Para o instantâneo do HANA ter êxito tem uma instância ativa do HANA.  No cenário HSR, o instantâneo de armazenamento não é suportado no nó secundário atual em que não é possível efetuar o instantâneo HANA.
- Depois do instantâneo de armazenamento tiver sido executado com êxito, o instantâneo do SAP HANA é eliminado.
- Backups de log de transação são utilizados com freqüência e são armazenados no /hana/logbackups volume ou no Azure. Pode disparar o volume de /hana/logbackups que contém os backups de log de transação para tirar um instantâneo em separado. Nesse caso, não é necessário executar um instantâneo do HANA.
- Se tem de restaurar uma base de dados para um determinado ponto no tempo, solicite esse suporte do Microsoft Azure (por uma indisponibilidade de produção) ou o SAP HANA no restauro de gestão de serviço do Azure para um determinado instantâneo de armazenamento. Um exemplo é uma restauração planeada de um sistema de proteção de segurança para o estado original.
- O instantâneo do SAP HANA que está incluído no instantâneo de armazenamento é um ponto de deslocamento para a aplicação de backups de log de transações que foram executadas e armazenados após o instantâneo de armazenamento foi criado.
- Estas cópias de segurança do registo de transação serão direcionadas para restaurar a base de dados para um determinado ponto no tempo.

Pode executar o direcionamento três classes de volumes de instantâneos de armazenamento:

- Um instantâneo combinado sobre/hana/dados e /hana/shared (inclui/usr/sap). Este instantâneo requer a criação de um instantâneo do SAP HANA como preparação para o instantâneo de armazenamento. O instantâneo do SAP HANA certifica-se de que a base de dados está num estado consistente de um ponto de vista do armazenamento. E que para o restauro processar ou seja um ponto de conjunto de cópia de segurança no.
- Um instantâneo separado sobre/hana/logbackups.
- Uma partição de sistema operativo.

Obtenha as mais recentes scripts de instantâneo e documentação de [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). 

### <a name="storage-snapshot-considerations"></a>Considerações de instantâneo de armazenamento

>[!NOTE]
>Instantâneos de armazenamento consumam espaço de armazenamento que foi alocado para as unidades de instância grande do HANA. É necessário considerar os seguintes aspetos de agendamento de instantâneos de armazenamento e o número de instantâneos de armazenamento para manter. 

A mecânica específica de instantâneos de armazenamento para o SAP HANA no Azure (instâncias grandes) incluem:

- Um instantâneo de armazenamento específico (no ponto no tempo quando é tirado) consome armazenamento pouco.
- Como as alterações de conteúdo de dados e o conteúdo em ficheiros de alteração no volume de armazenamento de dados do SAP HANA, o instantâneo tem de armazenar o conteúdo original do bloco, bem como as alterações de dados.
- Como resultado, o instantâneo de armazenamento aumenta de tamanho. Existe mais tempo de instantâneo, fica do quanto maior for o instantâneo de armazenamento.
- Mais alterações efetuadas para o volume de base de dados do SAP HANA ao longo do tempo de vida de um instantâneo de armazenamento, o maior o consumo de espaço do instantâneo de armazenamento.

SAP HANA no Azure (instâncias grandes) é fornecido com tamanhos de volume fixo para os volumes de dados e de registo do SAP HANA. Execução de instantâneos desses volumes come no espaço de volume. Precisa determinar quando deve agendar instantâneos de armazenamento. Também terá de monitorizar o consumo de espaço dos volumes de armazenamento, bem como gerir o número de instantâneos que armazena. Pode desativar os instantâneos de armazenamento quando importar massas de dados ou efetuar outras alterações significativas na base de dados do HANA. 


As secções seguintes fornecem informações para efetuar estes instantâneos, incluindo recomendações gerais:

- Embora o hardware possa suportar 255 instantâneos por volume, quer manter bem abaixo deste número. A recomendação é 250 ou menos.
- Antes de executar os instantâneos de armazenamento, monitorizar e manter o controle de espaço livre.
- Reduza o número de instantâneos de armazenamento com base no espaço livre. Pode reduzir o número de instantâneos que mantenha ou pode expandir os volumes. É possível pedir armazenamento adicional em unidades de 1 terabyte.
- Durante as atividades, como mover dados para SAP HANA com ferramentas de migração de plataforma para SAP (R3load) ou o restauro de bases de dados do SAP HANA de cópias de segurança, desative os instantâneos de armazenamento no /hana/data volume. 
- Durante a maior reorganizations das tabelas de SAP HANA, instantâneos de armazenamento devem ser evitados, se possível.
- Os instantâneos de armazenamento são um pré-requisito para tirar partido do desastre capacidades de recuperação do SAP HANA no Azure (instâncias grandes).

### <a name="prerequisites-for-using-self-service-storage-snapshots"></a>Pré-requisitos para utilizar instantâneos de armazenamento de self-service

Para garantir que o script de instantâneos é executado com êxito, certifique-se de que o Perl está instalado no sistema operativo Linux no servidor de instâncias grandes do HANA. Perl vem pré-instalado na sua unidade de instância grande do HANA. Para verificar a versão do Perl, utilize o seguinte comando:

`perl -v`

![A chave pública é copiada ao executar este comando](./media/hana-overview-high-availability-disaster-recovery/perl_screen.png)


### <a name="set-up-storage-snapshots"></a>Configurar instantâneos de armazenamento

Para configurar a instantâneos de armazenamento com instâncias grandes do HANA, siga estes passos:
1. Certifique-se de que o Perl está instalado no sistema operativo Linux no servidor de instâncias grandes do HANA.
1. Modificar o/etc/ssh/ssh\_config para adicionar a linha _MACs hmac-sha1_.
1. Crie uma conta de utilizador de cópia de segurança do SAP HANA no nó principal para cada instância do SAP HANA que está a executar, se aplicável.
1. Instale o cliente do SAP HANA HDB em todos os servidores de instâncias grandes do SAP HANA.
1. No primeiro servidor SAP HANA nas instâncias grandes de cada região, crie uma chave pública para aceder a infraestrutura de armazenamento subjacente, que controla a criação do instantâneo.
1. Copiar os scripts e o ficheiro de configuração da [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) para a localização dos **hdbsql** na instalação do SAP HANA.
1. Modificar a *HANABackupDetails.txt* ficheiro conforme necessário para as especificações de cliente adequado.

Obtenha as mais recentes scripts de instantâneo e documentação de [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). 

### <a name="consideration-for-mcod-scenarios"></a>Consideração para cenários MCOD
Se estiver a executar uma [cenário MCOD](https://launchpad.support.sap.com/#/notes/1681092) com várias instâncias do SAP HANA numa só unidade de instância grande do HANA, tem volumes de armazenamento separada aprovisionados para cada uma das instâncias do SAP HANA. Na versão atual da automação de instantâneo de self-service, não pode iniciar instantâneos separados em todos os sistemas de instância HANA SID (identificador). A funcionalidade oferece verificações para o SAP HANA nas instâncias registado do servidor no ficheiro de configuração (consulte este artigo) e executa um instantâneo simultâneo dos volumes de todas as instâncias registadas na unidade.
 

### <a name="step-1-install-the-sap-hana-hdb-client"></a>Passo 1: Instalar o cliente do SAP HANA HDB

O sistema operativo Linux instalado no SAP HANA no Azure (instâncias grandes) inclui as pastas e os scripts necessários para executar os instantâneos de armazenamento do SAP HANA para fins de recuperação após desastre e cópia de segurança. A verificação para versões mais recentes na [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). A versão de lançamento mais recente dos scripts é 3.x. Scripts de diferentes podem ter diferentes versões secundárias dentro da mesma versão principal.

>[!IMPORTANT]
>Quando mudar de versão 2.1 para a versão 3.x dos scripts, tenha em atenção que a estrutura de alguma sintaxe e o ficheiro de configuração foi alterada. Consulte os textos explicativos nas secções específicas. 

É da responsabilidade do cliente para instalar o cliente de SAP HANA HDB das unidades de instância grande do HANA enquanto estiver a instalar o SAP HANA.

### <a name="step-2-change-the-etcsshsshconfig"></a>Passo 2: Alterar a/etc/ssh/ssh\_config

Alteração `/etc/ssh/ssh_config` adicionando os _MACs hmac-sha1_ linha conforme mostrado aqui:
```
#   RhostsRSAAuthentication no
#   RSAAuthentication yes
#   PasswordAuthentication yes
#   HostbasedAuthentication no
#   GSSAPIAuthentication no
#   GSSAPIDelegateCredentials no
#   GSSAPIKeyExchange no
#   GSSAPITrustDNS no
#   BatchMode no
#   CheckHostIP yes
#   AddressFamily any
#   ConnectTimeout 0
#   StrictHostKeyChecking ask
#   IdentityFile ~/.ssh/identity
#   IdentityFile ~/.ssh/id_rsa
#   IdentityFile ~/.ssh/id_dsa
#   Port 22
Protocol 2
#   Cipher 3des
#   Ciphers aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc
#   MACs hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160
MACs hmac-sha1
#   EscapeChar ~
#   Tunnel no
#   TunnelDevice any:any
#   PermitLocalCommand no
#   VisualHostKey no
#   ProxyCommand ssh -q -W %h:%p gateway.example.com
```

### <a name="step-3-create-a-public-key"></a>Passo 3: Criar uma chave pública

Para ativar o acesso para as interfaces de instantâneo de armazenamento do seu inquilino de instância grande do HANA, tem de estabelecer um procedimento de início de sessão por meio de uma chave pública. No SAP HANA primeiro no servidor do Azure (instâncias grandes) no seu inquilino, crie uma chave pública para ser utilizado para aceder a infraestrutura de armazenamento. A chave pública garante que uma palavra-passe não é necessário para iniciar sessão para as interfaces de instantâneo de armazenamento. Criar uma chave pública também significa que não é necessário manter as credenciais de palavra-passe. No Linux no servidor do SAP HANA nas instâncias grandes, execute o seguinte comando para gerar a chave pública:
```
  ssh-keygen –t dsa –b 1024
```
É a nova localização **_/root/.ssh/id\_dsa.pub**. Não introduza uma palavra-passe real; caso contrário, tem de introduzir a palavra-passe de cada vez que iniciar sessão. Em vez disso, selecione **Enter** duas vezes para remover o requisito de "Introduzir palavra-passe" para iniciar sessão.

Certifique-se de que a chave pública foi corrigida conforme esperado, alterando as pastas a serem **/root/.ssh/** e, em seguida, executando o `ls` comando. Se a chave está presente, pode copiá-lo ao executar o seguinte comando:

![A chave pública é copiada ao executar este comando](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

Neste momento, contacte o SAP HANA no Azure Service Management e fornecê-los com a chave pública. O representante do serviço usa a chave pública para registá-lo na infraestrutura de armazenamento subjacente, que é retirar para o seu inquilino de instância grande do HANA.

### <a name="step-4-create-an-sap-hana-user-account"></a>Passo 4: Criar uma conta de utilizador do SAP HANA

Para iniciar a criação de instantâneos de SAP HANA, terá de criar uma conta de utilizador no SAP HANA que podem utilizar os scripts de instantâneo de armazenamento. Crie uma conta de utilizador do SAP HANA no SAP HANA Studio para esta finalidade. O utilizador tem de ser criado sob o SYSTEMDB e não sob a base de dados do SID para MDC. No ambiente de contentor único, o utilizador está configurada na base de dados do inquilino. Esta conta tem de ter os seguintes privilégios: **cópia de segurança do administrador** e **catálogo leitura**. Neste exemplo, é o nome de utilizador **SCADMIN**. O nome da conta de utilizador criado no HANA Studio diferencia maiúsculas de minúsculas. Verifique se seleciona **não** para exigir que o utilizador alterar a palavra-passe no seu próximo início de sessão.

![Criar um utilizador no HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

Se utilizar implementações MCOD com várias instâncias do SAP HANA numa só unidade, terá de repetir este passo para cada instância do SAP HANA.

### <a name="step-5-authorize-the-sap-hana-user-account"></a>Passo 5: Autorizar a conta de utilizador do SAP HANA

Neste passo, irá autorizar a conta de utilizador do SAP HANA que criou, para que os scripts não tem de submeter as palavras-passe em tempo de execução. O comando de SAP HANA `hdbuserstore` permite a criação de uma chave de utilizador do SAP HANA, que é armazenada num ou mais nós de SAP HANA. A chave de utilizador permite ao utilizador aceder SAP HANA, sem ter de gerir palavras-passe de dentro do processo de criação de scripts. O processo de criação de scripts é abordado neste artigo.

>[!IMPORTANT]
>Execute o seguinte comando em que o usuário que os scripts estão planeados para ser executado. Caso contrário, o script não pode funcionar corretamente.

Introduza o `hdbuserstore` comando da seguinte forma:

**Para a configuração do HANA não MDC**
```
hdbuserstore set <key> <host>:<3[instance]15> <user> <password>
```

**Para a configuração de MDC HANA**
```
hdbuserstore set <key> <host>:<3[instance]13> <user> <password>
```

No exemplo a seguir, o usuário estiver **SCADMIN01**, é o nome de anfitrião **lhanad01**, e o número de instância é **01**:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Se utilizar uma implementação de HANA MCOD com várias instâncias do SAP HANA numa só unidade, terá de repetir o passo para cada instância do SAP HANA e o utilizador de cópia de segurança associado na unidade.

Se tiver uma configuração de escalamento horizontal do SAP HANA, terá de gerir todos os scripts de um único servidor. Neste exemplo, a chave de SAP HANA **SCADMIN01** tem de ser alterado para cada anfitrião de uma forma que mostra os anfitriões que está relacionado com a chave. Corrigir a conta de cópia de segurança do SAP HANA com o número de instância do banco de dados do HANA. A chave tem de ter privilégios administrativos no anfitrião para o qual está atribuída e o utilizador de cópia de segurança para configurações de escalamento horizontal tem de ter direitos de acesso a todas as instâncias do SAP HANA. Supondo que os três nós de escalamento horizontal tem os nomes **lhanad01**, **lhanad02**, e **lhanad03**, a sequência de comandos é semelhante a este:

```
hdbuserstore set SCADMIN01 lhanad01:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad02:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad03:30115 SCADMIN <password>
```

### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>Passo 6: Obter os scripts de instantâneo, os instantâneos de configurar e testar a configuração e a conectividade

Baixe a versão mais recente dos scripts da [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). Copiar scripts transferidos e o arquivo de texto para o diretório de trabalho **hdbsql**. Para instalações de HANA atuais, este diretório está no seguinte formato: /hana/shared/D01/exe/linuxx86\_64/hdb. 
``` 
azure_hana_backup.pl 
azure_hana_replication_status.pl 
azure_hana_snapshot_details.pl 
azure_hana_snapshot_delete.pl 
testHANAConnection.pl 
testStorageSnapshotConnection.pl 
removeTestStorageSnapshot.pl
azure_hana_dr_failover.pl
azure_hana_test_dr_failover.pl 
HANABackupCustomerDetails.txt 
``` 

Ao lidar com Perl scripts: 

- Nunca modificar os scripts, exceto se instruído pelo Microsoft Operations.
- Quando lhe for pedido para modificar o script ou um ficheiro de parâmetros, utilize sempre o editor de texto do Linux, como "vi" e não um editor de Windows, como o bloco de notas. Com um editor de Windows, poderá danificar o formato de ficheiro.
- Utilize sempre os scripts mais recentes. Pode baixar a versão mais recente do GitHub.
- Utilize a mesma versão de scripts em todo o cenário.
- Testar os scripts e obtenha confortável com os parâmetros necessários e a saída do script antes de os utilizar diretamente no sistema de produção.
- Não altere o nome do ponto de montagem de servidor aprovisionado pelo Microsoft Operations. Estes scripts contam com estes pontos de montagem padrão para estar disponível para uma execução com êxito.


A finalidade das diferentes scripts e arquivos é o seguinte:

- **Azure\_hana\_backup.pl**: este script está agendado com o utilitário de agendamento de Cron do Linux para executar os instantâneos de armazenamento nos dados do HANA e volumes partilhados, o volume de logbackups/hana/ou o sistema operativo.
- **Azure\_hana\_replicação\_status.pl**: este script fornece os detalhes básicos sobre o estado de replicação do site de produção para o site de recuperação após desastre. Os monitores de script para se certificar de que a replicação está a ser efetuada, e mostra o tamanho dos itens que estão a ser replicados. Ele também fornece orientações se uma replicação está a demorar demasiado tempo ou se a ligação estiver inativa.
- **Azure\_hana\_instantâneo\_details.pl**: este script fornece uma lista de detalhes básicos sobre todos os instantâneos, por volume, que existem no seu ambiente. Este script pode ser executado no servidor principal ou numa unidade de servidor na localização de recuperação após desastre. O script fornece as informações seguintes, divididas por cada volume que contém instantâneos:
   * O tamanho de instantâneos total num volume
   * Os seguintes detalhes de cada instantâneo desse volume: 
      - Nome do instantâneo 
      - Hora de criação 
      - Tamanho do instantâneo
      - Frequência do instantâneo
      - ID de cópia de segurança do HANA associado a esse instantâneo, se forem relevantes
- **Azure\_hana\_instantâneo\_delete.pl**: este script elimina um instantâneo de armazenamento ou um conjunto de instantâneos. Pode utilizar o ID de cópia de segurança do SAP HANA como encontrada no HANA Studio ou o nome do instantâneo de armazenamento. Atualmente, o ID da cópia de segurança só está associado aos instantâneos criados para os volumes de dados/registos/partilhados do HANA. Caso contrário, se introduzir o ID do instantâneo, ele procura todos os instantâneos que correspondem a ID do instantâneo introduzido.  
- **testHANAConnection.pl**: este script testa a ligação à instância do SAP HANA e é necessário para configurar os instantâneos de armazenamento.
- **testStorageSnapshotConnection.pl**: este script tem duas finalidades. Em primeiro lugar, ele garante que a unidade de instância grande do HANA que executa os scripts tem acesso para a máquina virtual de armazenamento atribuída e para a interface de instantâneo de armazenamento das suas instâncias grandes do HANA. O segundo objetivo é criar um instantâneo temporário para a instância HANA que está a testar. Este script deve ser executado para cada instância HANA num servidor para garantir que os scripts de cópia de segurança funcionam conforme esperado.
- **removeTestStorageSnapshot.pl**: este script elimina o instantâneo de teste criado com o script **testStorageSnapshotConnection.pl**.
- **Azure\_hana\_dr\_failover.pl**: este script inicia um failover de DR para outra região. O script precisa ser executado na unidade instância grande do HANA na região DR ou na unidade que pretende efetuar a ativação pós-falha. Este script para a replicação de armazenamento do lado do primário para o lado secundário, restaura o instantâneo mais recente nos volumes de DR e fornece o mountpoints para a DR de volumes.
- **Azure\_hana\_testar\_dr\_failover.pl**: este script executa uma ativação pós-falha de teste para o site de DR. Ao contrário do azure_hana_dr_failover.pl script, esta execução não interrompe a replicação de armazenamento do primário para o secundário. Em vez disso, clones dos volumes de armazenamento replicado são criados no lado do DR, e são fornecidos mountpoints dos volumes clonados. 
- **HANABackupCustomerDetails.txt**: este ficheiro é um ficheiro de configuração modificável que precisa modificar adaptá-los à sua configuração de SAP HANA. O *HANABackupCustomerDetails.txt* ficheiro é o ficheiro de controlo e a configuração para o script que executa os instantâneos de armazenamento. Ajuste o arquivo para os seus objetivos e a configuração. Recebe o **nome da cópia de segurança de armazenamento** e o **endereço de IP de armazenamento** do SAP HANA no Azure Service Management quando implementar suas instâncias. Não é possível modificar a sequência de ordenação ou espaçamento de qualquer uma das variáveis existentes neste ficheiro. Se o fizer, os scripts não são executados corretamente. Além disso, recebe o endereço IP do nó de aumentar verticalmente ou do nó principal (se aumentar horizontalmente) do SAP HANA no Azure Service Management. Também saber o número de instância HANA que ocorre durante a instalação do SAP HANA. Agora, precisa adicionar um nome de cópia de segurança para o ficheiro de configuração.

O ficheiro de configuração seria semelhante ao seguinte exemplo, para uma implementação de aumento vertical ou horizontal, depois de preencher o nome do servidor da unidade de instância grande do HANA e o endereço IP do servidor. Preencha todos os campos necessários para cada SID do HANA SAP que pretende criar cópias de segurança ou recuperar.

Também pode comentar as linhas de instâncias que não pretende criar cópias de segurança durante um período de tempo, adicionando um "#" na frente de um campo obrigatório. Também não tem de introduzir todas as instâncias SAP HANA que estão contidas num servidor se não é necessário para criar cópias de segurança ou recuperar essa instância específica. O formato deve ser mantido para todos os campos, ou todos os scripts lance uma mensagem de erro e o script termina. É possível eliminar linhas necessárias adicionais de quaisquer detalhes de informações de SID que não estiver a utilizar após a última instância do SAP HANA em utilização. Todas as linhas devem ser preenchidas, comentadas ou eliminadas.

>[!IMPORTANT]
>A estrutura do arquivo alterado com a mudança da versão 2.1 para a versão 3.x. Se pretender utilizar os scripts de versão 3.x, terá de se adaptar a estrutura de ficheiros de configuração. 


```
HANA Server Name: testing01
HANA Server IP Address: 172.18.18.50
```

Para cada instância que configurar na unidade instância grande do HANA, ou para a configuração de escalamento horizontal, terá de definir os dados da seguinte forma:

    
```
######***SID #1 Information***#####
SID1: h01
###Provided by Microsoft Operations###
SID1 Storage Backup Name: clt1h01backup
SID1 Storage IP Address: 172.18.18.11
######     Customer Provided    ######
SID1 HANA instance number: 00
SID1 HANA HDBuserstore Name: SCADMINH01
```
Para aumentar horizontalmente e configurações do HANA System Replication, repita esta configuração em cada um de nós. Esta medida certifica-se de que, a casos de falha, as cópias de segurança e a replicação de armazenamento eventual continuam a funcionar.   

Depois de colocar todos os dados de configuração para o *HANABackupCustomerDetails.txt* de ficheiros, verifique se as configurações estão corretas para os dados da instância HANA. Utilize o script `testHANAConnection.pl`, que é independente de uma configuração de aumento vertical ou horizontal do SAP HANA.

```
testHANAConnection.pl
```

Se tiver uma configuração de escalamento horizontal do SAP HANA, certifique-se de que a instância principal do HANA tem acesso a todos os necessário HANA servidores e instâncias. Não há parâmetros para o script de teste, mas tem de adicionar os dados para o *HANABackupCustomerDetails.txt* ficheiro de configuração para o script seja executado corretamente. Apenas os códigos de erro do shell comando são devolvidos, para que não é possível para o script para erro verificar todas as instâncias. Mesmo assim, o script fornece alguns comentários úteis para que possa verificar novamente.

Para executar o script, introduza o seguinte comando:
```
 ./testHANAConnection.pl
```
Se o script com êxito obtém o estado da instância do HANA, ele exibe uma mensagem que a ligação do HANA foi concluída com êxito.


A próxima etapa de teste é verificar a conectividade para o armazenamento com base nos dados que colocou na *HANABackupCustomerDetails.txt* configuração de ficheiros e, em seguida, executar um instantâneo de teste. Antes de executar o `azure_hana_backup.pl` script, tem de executar este teste. Se um volume contém não existem instantâneos, é impossível determinar se o volume está vazio, ou se houver uma falha SSH para obter os detalhes de instantâneo. Por esse motivo, o script executa dois passos:

- Verifica que estão acessíveis para os scripts executar os instantâneos de máquina de virtual de armazenamento e as interfaces do inquilino.
- Cria um instantâneo de cópia, ou teste de cada volume por instância HANA.

Por esse motivo, a instância HANA é incluída como um argumento. Se falhar a execução, não é possível fornecer o erro na verificação da ligação de armazenamento. Mesmo se não houver nenhuma verificação de erro, o script que fornece sugestões úteis.

1. Execute a sequência de comandos para executar este teste:

   ```
   ssh <StorageUserName>@<StorageIP>
   ```

   O nome de utilizador de armazenamento e o endereço IP de armazenamento foram fornecidos a a entregar o direito de permanência da unidade de instância grande do HANA.

1. Execute o script de teste:
   ```
    ./testStorageSnapshotConnection.pl
   ```

O script tenta iniciar sessão para o armazenamento utilizando a chave pública fornecida nos passos de configuração anteriores bem como com os dados configurados no *HANABackupCustomerDetails.txt* ficheiro. Se o início de sessão for bem-sucedida, é apresentado o seguinte conteúdo:

```
**********************Checking access to Storage**********************
Storage Access successful!!!!!!!!!!!!!!
```

Se ocorrerem problemas na conexão com a consola de armazenamento, a saída é semelhante a isto:

```
**********************Checking access to Storage**********************
WARNING: Storage check status command 'volume show -type RW -fields volume' failed: 65280
WARNING: Please check the following:
WARNING: Was publickey sent to Microsoft Service Team?
WARNING: If passphrase entered while using tool, publickey must be re-created and passphrase must be left blank for both entries
WARNING: Ensure correct IP address was entered in HANABackupCustomerDetails.txt
WARNING: Ensure correct Storage backup name was entered in HANABackupCustomerDetails.txt
WARNING: Ensure that no modification in format HANABackupCustomerDetails.txt like additional lines, line numbers or spacing
WARNING: ******************Exiting Script*******************************
```

Após um êxito início de sessão para as interfaces de máquina virtual de armazenamento, o script continua na fase 2 e cria um instantâneo de teste. O resultado é mostrado aqui para uma configuração de escalamento horizontal de três nós do SAP HANA:

```
**********************Creating Storage snapshot**********************
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_shared_hm3_t020_vol ...
Snapshot created successfully.
```

Se o instantâneo de teste tiver sido executado com êxito com o script, pode continuar a configurar os instantâneos de armazenamento real. Se não for bem-sucedida, investigue os problemas antes de prosseguir. O instantâneo de teste deve permanecer por perto, até que os primeiros instantâneos reais são feitos.


### <a name="step-7-perform-snapshots"></a>Passo 7: Executar instantâneos

Quando os passos de preparação estiverem concluídos, pode começar a configurar a configuração de instantâneos de armazenamento real. O script ser agendada funciona com configurações de aumento vertical e horizontal do SAP HANA. Para execução do script de segurança regular e periódica, agende o script usando o utilitário de cron. 

Pode criar três tipos de cópias de segurança do instantâneo:
- **HANA**: uma cópia de segurança do instantâneo combinado em que os volumes que contêm dados/hana/e/hana/partilhados (que contém também /usr/sap) estão abrangidas pelo instantâneo de coordenada. Um restauro de ficheiro único é possível a partir deste instantâneo.
- **Registos**: uma cópia de segurança do instantâneo do volume de logbackups/hana /. Não existe nenhum instantâneo HANA é acionado para executar este instantâneo de armazenamento. Este volume de armazenamento deve conter os backups de log de transação do SAP HANA. São executadas com mais frequência para restringir o crescimento do registo e impedir a potencial perda de dados. Um restauro de ficheiro único é possível a partir deste instantâneo. Não, reduza a frequência para menos de 3 minutos.
- **Arranque**: um instantâneo do volume que contém o número de unidade lógica (LUN) de arranque da instância grande do HANA. Esta cópia de segurança do instantâneo é possível apenas com o tipo I SKUs de instâncias grandes do HANA. Arquivo único não é possível efetuar restauros de instantâneo do volume que contém o LUN de arranque.


>[!NOTE]
> A sintaxe de chamada para esses três tipos de instantâneos alterados com a mudança para os versão 3.x scripts, os quais suportam implementações MCOD. Não é necessário especificar o SID do HANA de uma instância mais. Tem de certificar-se de que o SAP HANA nas instâncias de uma unidade está configurado no arquivo de configuração *HANABackupCustomerDetails.txt*.

>[!NOTE]
> Quando executar o script pela primeira vez, poderá mostrar alguns erros inesperados no ambiente de múltiplos SID. Executar novamente o script corrige o problema.



A nova sintaxe de chamada para a execução de instantâneos de armazenamento com o script *azure_hana_backup.pl* semelhante ao seguinte:

```
HANA backup covering /hana/data and /hana/shared (includes/usr/sap)
./azure_hana_backup.pl hana <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

For /hana/logbackups snapshot
./azure_hana_backup.pl logs <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

For snapshot of the volume storing the boot LUN
./azure_hana_backup.pl boot <HANA Large Instance Type> <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

```

Os detalhes dos parâmetros são os seguintes: 

- O primeiro parâmetro caracteriza o tipo da cópia de segurança do instantâneo. Os valores permitidos são **hana**, **registos**, e **arranque**. 
- O parâmetro **<HANA Large Instance Type>** é necessário para backups de volume de arranque apenas. Existem dois valores válidos com "TypeI" ou "TypeII" depende da unidade de instância grande do HANA. Para saber que tipo de sua unidade é, consulte [descrição geral do SAP HANA (instâncias grandes) e a arquitetura no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).  
- O parâmetro **< snapshot_prefix >** é um instantâneo ou um rótulo de cópia de segurança para o tipo de instantâneo. Ele tem duas finalidades: uma é que atribua um nome, para que saiba o que esses instantâneos são sobre. A segunda finalidade é para o script *azure\_hana\_backup.pl* para determinar o número de instantâneos de armazenamento que são mantidos sob essa etiqueta específica. Se agendar dois backups de instantâneo de armazenamento do mesmo tipo (como **hana**), com duas etiquetas diferentes e definir que 30 instantâneos devem ser mantidos para cada um, acaba tendo 60 instantâneos de armazenamento dos volumes afetados. Apenas alpha numérico ("A-Z, a-z, 0-9"), um caráter de sublinhado ("_") e travessão ("-") são permitidos carateres. 
- O parâmetro **< snapshot_frequency >** está reservado para futuros desenvolvimentos e não tem qualquer impacto. Defini-lo como "3min" ao executar cópias de segurança do tipo **log**e para "15 min" ao executar os outros tipos de cópia de segurança.
- O parâmetro **<number of snapshots retained>** define o período de retenção dos instantâneos indiretamente ao definir o número de instantâneos com o mesmo prefixo de instantâneo (etiqueta). Este parâmetro é importante para execuções agendadas por meio de cron. Se o número de instantâneos com a mesma snapshot_prefix exceder o número fornecido por este parâmetro, o instantâneo mais antigo é eliminado antes de executar um novo instantâneo de armazenamento.

No caso de escalamento horizontal, o script faz uma verificação adicional para se certificar de que pode acessar todos os servidores do HANA. O script também verifica que todas as instâncias HANA retornam o status adequado das instâncias antes de criar um instantâneo do SAP HANA. O instantâneo do SAP HANA é seguido de um instantâneo de armazenamento.

A execução do script `azure_hana_backup.pl` cria o armazenamento de instantâneos nas seguintes três fases:

1. Executa um instantâneo do SAP HANA
1. Executa um instantâneo de armazenamento
1. Remove o instantâneo do SAP HANA que foi criado antes da execução do instantâneo de armazenamento

Para executar o script, chamá-lo a partir da pasta executável do HDB ao qual foi copiado. 

O período de retenção é administrado com o número de instantâneos que são enviados como um parâmetro ao executar o script. A quantidade de tempo que é abrangido pelos instantâneos de armazenamento é uma função do período de execução e do número de instantâneos submetido como um parâmetro ao executar o script. Se o número de instantâneos são mantidos exceder o número que são nomeados como um parâmetro na chamada do script, o instantâneo de armazenamento mais antigo da etiqueta do mesmo é eliminado antes de um novo instantâneo é executado. O número é conceder à medida que o último parâmetro da chamada é o número que pode utilizar para controlar o número de instantâneos são mantidos. Com este número, também pode controlar, indiretamente, o espaço em disco utilizado para instantâneos. 

> [!NOTE]
>Assim que altere o rótulo, a contagem começa novamente. Tem de ser strict no etiquetagem, para que os instantâneos não são eliminados acidentalmente.

### <a name="snapshot-strategies"></a>Estratégias de instantâneo
A frequência de instantâneos para os diferentes tipos depende se utilizar a funcionalidade de recuperação de desastres de instância grande do HANA. Esta funcionalidade baseia-se em instantâneos de armazenamento, que podem exigir recomendações especiais para os períodos de frequência e a execução de instantâneos de armazenamento. 

As considerações e recomendações que se seguem, a suposição é que possa *não* utilizar a funcionalidade de recuperação após desastre que oferece instâncias grandes do HANA. Em vez disso, utilize os instantâneos de armazenamento para ter cópias de segurança e ser capaz de fornecer a recuperação de ponto no tempo nos últimos 30 dias. Devido às limitações do número de instantâneos e espaço, os clientes consideraram os seguintes requisitos:

- O tempo de recuperação para a recuperação de ponto no tempo.
- O espaço utilizado.
- O ponto de recuperação e objetivos de tempo de recuperação para potencial recuperação de desastres.
- A execução eventual de cópias de segurança de base de dados completo HANA em relação a discos. Sempre que uma cópia de segurança da base de dados completo em relação a discos ou o **backint** interface é executada, a execução de instantâneos de armazenamento falha. Se planeja executar cópias de segurança de base de dados completo em cima de instantâneos de armazenamento, certifique-se de que a execução de instantâneos de armazenamento é desativada durante este período.
- O número de instantâneos por volume (limitado a 250).


Para os clientes que não utilizam a funcionalidade de recuperação após desastre de instâncias grandes do HANA, o período de instantâneo é menos freqüente. Nesses casos, os clientes efetuar os instantâneos combinados em /hana/data e /hana/shared (inclui /usr/sap) em períodos de 12 ou 24 horas e que mantenham os instantâneos durante um mês. O mesmo acontece com os instantâneos do volume de cópia de segurança de registo. No entanto, a execução de backups de log de transação de SAP HANA com o volume de cópia de segurança do registo ocorre em 5 minutos para períodos de 15 minutos.

Instantâneos de armazenamento agendada são melhor efetuados com cron. Utilizar o mesmo script para todas as cópias de segurança e necessidades de recuperação após desastre e o que modificar as entradas de script de acordo com as várias pedido tempos de backup. Estes instantâneos são todos agendados forma diferente de cron dependendo do tempo de execução: por hora, 12 horas, diária ou semanal. 

Segue-se um exemplo de uma agenda cron no/etc/crontab:
```
00 1-23 * * * ./azure_hana_backup.pl hana hourlyhana 15min 46
10 00 * * *  ./azure_hana_backup.pl hana dailyhana 15min 28
00,05,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
22 12 * * *  ./azure_hana_backup.pl log dailylogback 3min 28
30 00 * * *  ./azure_hana_backup.pl boot TypeI dailyboot 15min 28
```
No exemplo anterior, existe um instantâneo combinado por hora, que abrange os volumes que contêm os dados/hana/e /hana/shared (inclui o sap/usr /) locais. Utilize este tipo de instantâneo para uma recuperação mais rápida do ponto anterior no tempo dentro os últimos dois dias. Além disso, existe um instantâneo diário nesses volumes. Por isso, tem dois dias de cobertura por instantâneos por hora, além de quatro semanas de cobertura por instantâneos diários. Além disso, o volume de cópia de segurança de registo de transação é uma cópia de segurança diária. Estas cópias de segurança são mantidas durante quatro semanas também. Como pode ver na terceira linha da crontab, a cópia de segurança do registo de transação de HANA é agendada para execução a cada 5 minutos. As horas de início das tarefas de cron diferentes que são executadas instantâneos de armazenamento são escalonadas, para que os instantâneos não são executados em simultâneo num certo ponto no tempo. 

O exemplo seguinte, vai realizar um instantâneo combinado que abrange os volumes que contêm as/hana/dados/hana/partilhados (incluindo/usr/sap) localizações e à hora. Mantenha esses instantâneos para dois dias. Os instantâneos de volumes de cópia de segurança de registo de transação são executados numa base de 5 minutos e são mantidos durante 4 horas. Como antes, a cópia de segurança do arquivo de log de transações de HANA é agendada para execução a cada 5 minutos. O instantâneo do volume de cópia de segurança do registo de transação é realizado com um atraso de 2 minutos, depois da cópia de segurança do registo de transação foi iniciada. Nesses dois minutos, a cópia de segurança do registo de transação do SAP HANA deve ser concluído em circunstâncias normais. Como antes, o volume que contém o arranque LUN uma vez por dia é apoiado por um instantâneo de armazenamento e é mantido por quatro semanas.

```
10 0-23 * * * ./azure_hana_backup.pl hana hourlyhana 15min 48
0,5,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup.pl log logback 3min 48
30 00 * * *  ./azure_hana_backup.pl boot TypeII dailyboot 15min 28
```

O gráfico seguinte ilustra as sequências de exemplo anterior, excluindo o LUN de arranque:

![Relação entre as cópias de segurança e instantâneos](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot_updated0921.PNG)

SAP HANA executa escritas regulares com o volume de /hana/log documentar as alterações confirmadas para a base de dados. Regularmente, o SAP HANA escreve um ponto de reposição no volume de /hana/data. Conforme especificado no crontab, uma cópia de segurança do registo de transação do SAP HANA é executada a cada 5 minutos. Também verá que um instantâneo do SAP HANA é executado como resultado de acionar um instantâneo de armazenamento combinado sobre os volumes /hana/data e /hana/shared a cada hora. Depois do instantâneo do HANA for bem-sucedida, o instantâneo de armazenamento combinado é executado. As instruções na crontab, o instantâneo de armazenamento no /hana/logbackup volume é executado cada 5 minutos, cerca de 2 minutos após a cópia de segurança do registo de transações do HANA.

> 

>[!IMPORTANT]
> A utilização de instantâneos de armazenamento para cópias de segurança do SAP HANA é valiosa apenas quando os instantâneos são realizados em conjunto com backups de log de transação do SAP HANA. Estas cópias de segurança do registo de transação tem de abranger os períodos de tempo entre os instantâneos de armazenamento. 

Se tiver definido um compromisso para os utilizadores de uma recuperação de ponto no tempo de 30 dias, terá de:

- Em casos extremos, acessar um armazenamento combinado de instantâneo sobre /hana/data e /hana/shared que é 30 dias de antiguidade.
- Ter backups de log de transação contíguo que abrangem o tempo entre qualquer um dos instantâneos de armazenamento combinado. Então, o instantâneo mais antigo do volume de cópia de segurança do registo de transação tem de ser 30 dias de antiguidade. Não é o caso se copiar os backups de log de transação para outra partilha NFS que está localizada no armazenamento do Azure. Nesse caso, poderia obter backups de log de transação antigos do que a partilha NFS.

Para tirar partido dos instantâneos de armazenamento e a replicação de armazenamento eventual dos backups de log de transação, terá de alterar a localização a que o SAP HANA escreve os backups de log de transação. Pode fazer esta alteração no HANA Studio. Embora o SAP HANA faz o backup de segmentos de registo completo automaticamente, deve especificar um intervalo de cópia de segurança de registo para ser determinística. Isso é especialmente verdadeiro quando utiliza a opção de recuperação após desastre, uma vez que geralmente deseja executar backups de log com um período de determinístico. No caso seguinte, 15 minutos são definidos como o intervalo de cópia de segurança do registo.

![Registos de agendar cópia de segurança do SAP HANA no SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Também pode escolher as cópias de segurança que são mais freqüentes que a cada 15 minutos. Uma definição mais freqüente, muitas vezes, é utilizada em conjunto com a funcionalidade de recuperação após desastre de instâncias grandes do HANA. Alguns clientes efetuar cópias de segurança de registo de transações a cada 5 minutos.  

Se a base de dados nunca foi feito o backup, a etapa final é efetuar uma cópia de segurança da base de dados baseados em ficheiros para criar uma única entrada de cópia de segurança que tem de existir dentro do catálogo de cópia de segurança. Caso contrário, o SAP HANA não pode iniciar as cópias de segurança de registo especificado.

![Fazer uma cópia de segurança baseados em ficheiros para criar uma única entrada de cópia de segurança](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


Após os primeiros instantâneos de armazenamento com êxito tiverem sido executados, pode eliminar o instantâneo de teste que foram executado no passo 6. Para tal, execute o script `removeTestStorageSnapshot.pl`:
```
./removeTestStorageSnapshot.pl
```

Segue-se um exemplo de saída do script:
```
Checking Snapshot Status for h80
**********************Checking access to Storage**********************
Storage Snapshot Access successful.
**********************Getting list of volumes that match HANA instance specified**********************
Collecting set of volumes hosting HANA matching pattern *h80* ...
Volume show completed successfully.
Adding volume hana_data_h80_mnt00001_t020_vol to the snapshot list.
Adding volume hana_log_backups_h80_t020_vol to the snapshot list.
Adding volume hana_shared_h80_t020_vol to the snapshot list.
**********************Adding list of snapshots to volume list**********************
Collecting set of snapshots for each volume hosting HANA matching pattern *h80* ...
**********************Displaying Snapshots by Volume**********************
hana_data_h80_mnt00001_t020_vol
Test_HANA_Snapshot.2018-02-06_1753.3
Test_HANA_Snapshot.2018-02-06_1815.2
….
Command completed successfully.
Exiting with return code: 0
Command completed successfully.
```


### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Monitorizar o número e tamanho de instantâneos no volume de disco

Num volume de armazenamento específico, pode monitorizar o número de instantâneos e o consumo de armazenamento desses instantâneos. O `ls` comando não mostra os diretórios de instantâneo ou ficheiros. No entanto, o comando de SO Linux `du` mostra detalhes sobre os instantâneos de armazenamento, porque eles são armazenados nos volumes mesmo. O comando pode ser utilizado com as seguintes opções:

- `du –sh .snapshot`: Esta opção fornece um total de todos os instantâneos dentro do diretório de instantâneo.
- `du –sh --max-depth=1`: Esta opção apresenta uma lista de todos os instantâneos são salvas no **.snapshot** pasta e o tamanho de cada instantâneo.
- `du –hc`: Esta opção fornece o tamanho total utilizado por todos os instantâneos.

Utilize estes comandos para se certificar de que os instantâneos que são executados e armazenados não estão a consumir todos o armazenamento nos volumes.

>[!NOTE]
>Os instantâneos da inicialização LUN não estão visíveis com os comandos anteriores.

### <a name="getting-details-of-snapshots"></a>A obter os detalhes de instantâneos
Para obter mais detalhes sobre os instantâneos, também pode utilizar o script `azure_hana_snapshot_details.pl`. Este script pode ser executado em ambos os locais, se houver um servidor ativo na localização de recuperação após desastre. O script fornece o seguinte resultado, dividido por cada volume que contém instantâneos: 
   * O tamanho de instantâneos total num volume
   * Os seguintes detalhes de cada instantâneo desse volume: 
      - Nome do instantâneo 
      - Hora de criação 
      - Tamanho do instantâneo
      - Frequência do instantâneo
      - ID de cópia de segurança do HANA associado a esse instantâneo, se forem relevantes

Segue-se um exemplo da sintaxe de execução do script:

```
./azure_hana_snapshot_details.pl 
```

Uma vez que o script tenta obter o ID de cópia de segurança do HANA, tem de ligar à instância do SAP HANA. Esta ligação requer que o ficheiro de configuração *HANABackupCustomerDetails.txt* definir corretamente. Uma saída de dois instantâneos num volume poderá ter o seguinte aspeto:

```
**********************************************************
****Volume: hana_shared_SAPTSTHDB100_t020_vol       ***********
**********************************************************
Total Snapshot Size:  411.8MB
----------------------------------------------------------
Snapshot:   customer.2016-09-20_1404.0
Create Time:   "Tue Sep 20 18:08:35 2016"
Size:   2.10MB
Frequency:   customer 
HANA Backup ID:   
----------------------------------------------------------
Snapshot:   customer2.2016-09-20_1532.0
Create Time:   "Tue Sep 20 19:36:21 2016"
Size:   2.37MB
Frequency:   customer2
HANA Backup ID:   
```


### <a name="file-level-restore-from-a-storage-snapshot"></a>Restauro ao nível do ficheiro a partir de um instantâneo de armazenamento
Para os tipos de instantâneo **hana** e **registos**, pode acessar os instantâneos diretamente nos volumes do **.snapshot** diretório. Há um subdiretório para cada um dos instantâneos. Pode copiar cada ficheiro no Estado em que estava no ponto do instantâneo desse subdiretório à estrutura de diretório real. Na versão atual do script, há **não** restaurar o script fornecido para o restauro de instantâneo como um Self-Service (embora o restauro de instantâneo pode ser executado como parte da DR self-service scripts no site de DR durante a ativação pós-falha). Tem de contactar a equipa de operações do Microsoft ao abrir um pedido de serviço para restaurar um instantâneo desejado dos instantâneos existentes disponíveis.

>[!NOTE]
>Arquivo único restore não funciona para instantâneos da inicialização LUN independente do tipo de unidades de instância grande do HANA. O **.snapshot** diretório não está exposto no LUN de arranque. 


### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Reduzindo o número de instantâneos num servidor

Tal como explicado anteriormente, pode reduzir o número de determinados rótulos de instantâneos que armazena. Os dois últimos parâmetros do comando para iniciar um instantâneo são a etiqueta e o número de instantâneos que pretenda manter.

```
./azure_hana_backup.pl hana dailyhana 15min 28
```

No exemplo anterior, é a etiqueta de instantâneo **dailyhana** e é o número de instantâneos com esta etiqueta a ser mantido **28**. Como responde ao consumo de espaço em disco, poderá reduzir o número de instantâneos armazenados. A maneira mais fácil para reduzir o número de instantâneos para 15, por exemplo, é executar o script com o último parâmetro definido como **15**:

```
./azure_hana_backup.pl hana dailyhana 15min 15
```

Se executar o script com esta definição, o número de instantâneos, incluindo o novo instantâneo de armazenamento, é 15. Os instantâneos de mais recentes 15 são mantidos e os 15 instantâneos mais antigos são eliminados.

 >[!NOTE]
 > Este script reduz o número de instantâneos apenas se houver mais de 1 horas depois de instantâneos. O script não elimine os instantâneos que são menos de 1 horas depois. Estas restrições relacionadas com a funcionalidade de recuperação após desastre opcional fornecida.

Se já não pretende manter um conjunto de instantâneos com a etiqueta de cópia de segurança **hanadaily** nos exemplos de sintaxe, pode executar o script com **0** como o número de retenção. Essa etiqueta de correspondência de todos os instantâneos, em seguida, são removidos. No entanto, a remoção de todos os instantâneos pode afetar as capacidades da funcionalidade de recuperação após desastre de instâncias grandes do HANA.

Uma segunda opção para eliminar instantâneos específicos é utilizar o script `azure_hana_snapshot_delete.pl`. Este script foi desenvolvido para eliminar um instantâneo ou um conjunto de instantâneos, com o ID de cópia de segurança do HANA como encontrada no HANA Studio ou por meio do nome de instantâneo em si. Atualmente, o ID da cópia de segurança só está associado ao instantâneos criados para o **hana** tipo de instantâneo. As cópias de segurança do tipo de instantâneo **logs** e **arranque** não efetue um instantâneo do SAP HANA e então, não há nenhum ID de cópia de segurança a serem encontradas para os instantâneos. Se o nome do instantâneo é introduzido, ele procura por todos os instantâneos em volumes diferentes que corresponde ao nome do instantâneo introduzido. 

Chame o script que tem de especificar o SID da instância do HANA, utilizando a sintaxe de chamada do script:

```
./azure_hana_snapshot_delete.pl <SID>

```

Execute o script como usuário **raiz**.

Se selecionar um instantâneo, pode eliminar cada instantâneo individualmente. Primeiro a fornecer o volume que contém o instantâneo e, em seguida, fornecer o nome de instantâneo. Se o instantâneo existe no volume e é mais do que 1 hora depois, é eliminado. Pode encontrar os nomes de volume e nomes de instantâneo, executando o `azure_hana_snapshot_details` script. 

>[!IMPORTANT]
>Se houver dados que só existe no instantâneo que está a eliminar, depois de eliminar o instantâneo, que os dados são perdidos para sempre.

   

### <a name="recover-to-the-most-recent-hana-snapshot"></a>Recuperar para o instantâneo mais recente do HANA

Num cenário de produção para baixo, o processo de recuperação a partir de um instantâneo de armazenamento pode ser iniciado como um incidente de cliente com suporte do Microsoft Azure. É uma questão de urgência alta se dados foi eliminados num sistema de produção, e a única forma de recuperá-la restaurar a base de dados de produção.

Numa situação de diferente, um ponto anterior no tempo de recuperação poderá ser urgência baixa e planeadas dias com antecedência. Pode planear esta recuperação com o SAP HANA no Azure Service Management em vez de gerar um sinalizador de alta prioridade. Por exemplo, poderá ser planejando atualizar o software SAP através da aplicação de um novo pacote de aprimoramento. Em seguida, terá de reverter para um instantâneo que representa o estado antes da atualização do pacote de aprimoramento.

Antes de enviar o pedido, terá de preparar. O SAP HANA na equipe de gerenciamento de serviços do Azure, em seguida, pode manipular a solicitação e fornecer os volumes restaurados. Em seguida, restaurar a base de dados do HANA com base nos instantâneos. 

A seguir mostra como preparar para o pedido:

>[!NOTE]
>Interface do usuário pode variar das capturas de ecrã seguintes, consoante a versão de SAP HANA que está a utilizar.

1. Decida qual instantâneo para restaurar. Apenas o volume de dados do hana é restaurado, a menos que instruir o contrário. 

1. Encerre a instância do HANA.

 ![Encerrar a instância HANA](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

1. Desmonte os volumes de dados em cada nó de base de dados do HANA. Se os volumes de dados ainda são instalados no sistema operativo, o restauro do instantâneo de falha.
 ![Desmontar os volumes de dados em cada nó de base de dados HANA](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

1. Abra um pedido de suporte do Azure e inclui instruções sobre a restauração de um instantâneo específico.

 - Durante o restauro: SAP HANA no Azure Service Management poderá pedir que participe de uma chamada de conferência para se certificar de coordenação, verificação e confirmação de que o instantâneo de armazenamento correta é restaurado. 

 - Após o restauro: SAP HANA no Azure Service Management notifica-o quando o instantâneo de armazenamento foi restaurado.

1. Depois do processo de restauro estiver concluído, voltar a montar os volumes de dados.

 ![Voltar a montar os volumes de dados](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

1. Selecione as opções de recuperação no SAP HANA Studio, se eles não surgir automaticamente quando voltar a ligar ao HANA DB através do SAP HANA Studio. O exemplo seguinte mostra uma restauração para o último instantâneo do HANA. Um instantâneo de armazenamento incorpora um instantâneo do HANA. Se restaurar para o instantâneo mais recente do armazenamento, deve ser o instantâneo mais recente do HANA. (Se restaurar para um instantâneo de armazenamento mais antigo, tem de localizar o instantâneo do HANA com base no tempo que o instantâneo de armazenamento foi criado.)

 ![Selecione as opções de recuperação no SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

1. Selecione **recuperar a base de dados para um instantâneo de cópia de segurança ou de armazenamento de dados específicos**.

 ![A janela de especificar o tipo de recuperação](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

1. Selecione **especificar cópia de segurança sem catálogo**.

 ![A janela de especificar localização de cópia de segurança](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

1. Na **tipo de destino** lista, selecione **instantâneo**.

 ![Especificar a cópia de segurança para a janela de recuperação](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

1. Selecione **concluir** para iniciar o processo de recuperação.

 ![Selecione "Concluir" para iniciar o processo de recuperação](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

1. A base de dados do HANA é restaurado e recuperado para o instantâneo do HANA que está incluído no instantâneo de armazenamento.

 ![Base de dados HANA é restaurado e recuperado para o instantâneo do HANA](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recover-to-the-most-recent-state"></a>Recuperar para o estado mais recente

O processo seguinte restaura o instantâneo do HANA que está incluído no instantâneo de armazenamento. Em seguida, restaura os backups de log de transação para o estado mais recente da base de dados antes de restaurar o instantâneo de armazenamento.

>[!IMPORTANT]
>Antes de continuar, certifique-se de que tem uma cadeia completa e contínuo de backups de log de transação. Sem estas cópias de segurança, não é possível restaurar o estado atual da base de dados.

1. Conclua os passos 1 a 6 no [recuperar para o instantâneo mais recente do HANA](#recovering-to-the-most-recent-hana-snapshot).

1. Selecione **recuperar a base de dados para o estado mais recente**.

 ![Selecionar "Recuperar a base de dados para o estado mais recente"](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

1. Especifique a localização das cópias de segurança do registo HANA mais recentes. A localização tem de conter todos os HANA transação backups de log partir do instantâneo do HANA para o estado mais recente.

 ![Especifique a localização das cópias de segurança do registo HANA mais recentes](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

1. Selecione uma cópia de segurança como base do qual deseja recuperar a base de dados. Neste exemplo, o instantâneo do HANA na captura de ecrã é o instantâneo do HANA que foi incluído no instantâneo de armazenamento. 

 ![Selecione uma cópia de segurança como base do qual deseja recuperar a base de dados](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

1. Limpar o **cópias de segurança do uso Delta** caixa de verificação se não existirem diferenças entre a hora do instantâneo HANA e o estado mais recente.

 ![Desmarque a caixa de verificação "Cópias de segurança do uso Delta", não se existem nenhuma deltas](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

1. No ecrã resumo, selecione **concluir** para iniciar o procedimento de restauração.

 ![Clique em "Concluir" no ecrã resumo](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recover-to-another-point-in-time"></a>Recuperar para um outro ponto no tempo
Para recuperar para um ponto no tempo entre o instantâneo HANA (incluído no instantâneo de armazenamento) e outro que é posterior a recuperação de ponto no tempo de instantâneo do HANA, execute os seguintes passos:

1. Certifique-se de que tem todos os backups de log de transação de instantâneo do HANA para o tempo que pretende recuperar a.
1. Começar o procedimento em [recuperar para o estado mais recente](#recovering-to-the-most-recent-state).
1. No passo 2 do procedimento, além da **especifique o tipo de recuperação** janela, selecione **recuperar a base de dados para o seguinte ponto no tempo**e, em seguida, especifique o ponto no tempo. 
1. Conclua os passos 3 a 6.

### <a name="monitor-the-execution-of-snapshots"></a>Monitorizar a execução de instantâneos

Como utilizar instantâneos de armazenamento de instâncias grandes do HANA, também tem de monitorizar a execução desses instantâneos. O script que executa um instantâneo de armazenamento grava a saída num arquivo e, em seguida, salva-o para a mesma localização que os scripts Perl. Um arquivo separado foi escrito para cada instantâneo de armazenamento. O resultado de cada ficheiro mostra as várias fases que executa o script de instantâneo:

1. Localiza os volumes que precisam para criar um instantâneo.
1. Localiza os instantâneos tirados desses volumes.
1. Elimina o eventual instantâneos existentes para corresponder ao número de instantâneos que especificou.
1. Cria um instantâneo do SAP HANA.
1. Cria o instantâneo de armazenamento através de volumes.
1. Elimina o instantâneo do SAP HANA.
1. Muda o nome do instantâneo mais recente para **.0**.

A parte mais importante do cab de script identificado é esta parte:
```
**********************Creating HANA snapshot**********************
Creating the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data create snapshot"" ...
HANA snapshot created successfully.
**********************Creating Storage snapshot**********************
Taking snapshot hourly.recent for hana_data_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_backup_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_shared_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for sapmnt_lhanad01_t020_vol ...
Snapshot created successfully.
**********************Deleting HANA snapshot**********************
Deleting the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data drop snapshot"" ...
HANA snapshot deletion successfully.
```
Pode ver deste exemplo como o script regista a criação do instantâneo do HANA. No caso de escalamento horizontal, este processo é iniciado no nó principal. O nó principal inicia a criação síncrona dos instantâneos de SAP HANA em cada um de nós de trabalho. O instantâneo de armazenamento, em seguida, é criado. Após a execução bem-sucedida de instantâneos de armazenamento, o instantâneo HANA é eliminado. A eliminação do instantâneo HANA é iniciada a partir do nó principal.


## <a name="disaster-recovery-principles"></a>Princípios de recuperação após desastre
Instâncias grandes do HANA oferecem uma funcionalidade de recuperação após desastre entre carimbos de data / instância grande do HANA em diferentes regiões do Azure. Por exemplo, se implementar unidades de instância grande do HANA na região e.u.a. centro-oeste do Azure, pode utilizar as unidades de instância grande do HANA na região E.U. a leste como unidades de recuperação após desastre. Como mencionado anteriormente, recuperação após desastre não é configurada automaticamente, uma vez que ela requer que paga para outra unidade de instância grande do HANA na região DR. A configuração de recuperação após desastre funciona para instalações de aumentar verticalmente, bem como de escalamento horizontal. 

Nos cenários implementados até agora, os clientes utilizam a unidade na região DR para executar sistemas de não produção que utilizam uma instância HANA instalada. A unidade de instância grande do HANA tem de ser do mesmo SKU como o SKU utilizado para fins de produção. A imagem seguinte mostra que a configuração de disco entre a unidade do servidor na região do Azure de produção e a região de recuperação após desastre é semelhante a:

![Configuração de configuração de DR do ponto de vista do disco](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_setup.PNG)

Conforme mostrado neste gráfico de descrição geral, em seguida, terá de encomendar um segundo conjunto de volumes de disco. Os volumes de disco de destino são o mesmo tamanho que os volumes de produção para a instância de produção em unidades de recuperação após desastre. Estes volumes de disco estão associados com a unidade do servidor de instância grande do HANA no site de recuperação após desastre. Os seguintes volumes são replicados a partir da região de produção para o site de DR:

- / hana/dados
- / hana/logbackups 
- /Hana/Shared (inclui o sap/usr /)

O volume de /hana/log não é replicado porque o registo de transações do SAP HANA não é necessária da forma que a restauração a partir desses volumes. 

A base da funcionalidade de recuperação após desastre oferecido a funcionalidade de replicação de armazenamento é oferecida pela infraestrutura de instância grande do HANA. A funcionalidade que é utilizada no lado do armazenamento não é um fluxo constante de alterações que são replicados de forma assíncrona, à medida que as alterações ocorrem ao volume de armazenamento. Em vez disso, é um mecanismo que se baseia no fato de que os instantâneos destes volumes são criados em intervalos regulares. O delta entre um instantâneo já replicado e um novo instantâneo que ainda não foi replicado, em seguida, é transferido para o site de recuperação após desastre em volumes de disco de destino.  Estes instantâneos são armazenados nos volumes e, no caso de uma ativação pós-falha recuperação após desastre, têm de ser restaurada nesses volumes.  

A primeira transferência dos dados do volume completos deve ter antes da quantidade de dados torna-se mais pequena do que as diferenças entre os instantâneos. Como resultado, os volumes no site de DR contenham cada um dos instantâneos de volume realizada no site de produção. Eventualmente, pode utilizar esse sistema de DR para obter um estado anterior para recuperar dados perdidos, sem reverter o sistema de produção.

No caso de Implantações de MCOD com várias instâncias do SAP HANA independentes numa só unidade de instância grande do HANA, espera-se que todas as instâncias do SAP HANA estão a obter armazenamento replicado para o lado do DR.

Em casos em que utilize o HANA System Replication como funcionalidade de elevada disponibilidade no seu site de produção e utilizar a replicação de armazenamento para o site de DR, os volumes de ambos os nós de site primário para a instância de DR são replicados. Tem de comprar armazenamento adicional (tamanho da mesmo a partir do nó principal) no site de DR para acomodar os replicação a partir de primários e secundários para o DR. 



>[!NOTE]
>A funcionalidade de replicação de armazenamento de instância grande do HANA é espelhamento e replicação de instantâneos de armazenamento. Se não efetuar instantâneos de armazenamento introduzida no [Backup e restauração](#backup-and-restore) secção deste artigo, não pode existir qualquer replicação para o site de recuperação após desastre. Execução de instantâneo de armazenamento é um pré-requisito para a replicação de armazenamento para o site de recuperação após desastre.



## <a name="preparation-of-the-disaster-recovery-scenario"></a>Preparação do cenário de recuperação após desastre
Neste cenário, tem um sistema de produção em execução nas instâncias grandes do HANA a região do Azure de produção. Para obter os passos que se seguem, vamos supor que o SID desse sistema HANA é "PRD=IIS&sbp=&PVER=5.0&ID=500;100&cat", e de que tem um sistema de não produção em execução nas instâncias grandes do HANA na região do Azure de DR. Para o último, vamos supor que o seu SID é "TST." A imagem seguinte mostra esta configuração:

![Início do programa de configuração de DR](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start1.PNG)

Se a instância de servidor já não tiver sido ordenada com o conjunto de volume de armazenamento adicional, SAP HANA no Azure Service Management anexa o conjunto adicional de volumes como um destino para a réplica de produção para a unidade de instância grande do HANA no qual está a executar o TST Instância do HANA. Para essa finalidade, terá de fornecer o SID da sua instância do HANA de produção. Depois de SAP HANA no Azure Service Management confirma o anexo desses volumes, terá de montar os volumes para a unidade de instância grande do HANA.

![Passo seguinte de configuração de DR](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start2.PNG)

A próxima etapa é para instalar a segunda instância do SAP HANA na unidade na região do Azure de DR, em que executou a instância TST HANA instância grande do HANA. A instância recém-instalada do SAP HANA tem de ter o mesmo SID. Os utilizadores criados tem de ter o mesmo UID e ID de grupo que tenha a instância de produção. Se a instalação foi concluída com êxito, terá de:

- Execute o passo 2 da preparação de instantâneo de armazenamento abordado anteriormente neste artigo.
- Crie uma chave pública para a unidade de DR de unidade de instância grande do HANA, se ainda não o fez isso. Consulte o passo 3 da preparação de instantâneo de armazenamento abordado anteriormente neste artigo.
- Manter o *HANABackupCustomerDetails.txt* com a nova instância HANA e teste se a conectividade para o armazenamento funciona corretamente.  
- Pare a instância do SAP HANA recentemente instalada na unidade instância grande do HANA na região do Azure de DR.
- Desmonte estes volumes PRD=IIS&sbp=&PVER=5.0&ID=500;100&cat e contacte o SAP HANA no Azure Service Management. Os volumes não podem permanecer montados para a unidade, pois não pode estar acessíveis ao funcionar como destino de replicação de armazenamento.  

![Passo de configuração de DR antes de estabelecer a replicação](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start3.PNG)

A equipe de operações estabelece a relação de replicação entre os volumes de PRD=IIS&sbp=&PVER=5.0&ID=500;100&cat a região do Azure de produção e os volumes de PRD=IIS&sbp=&PVER=5.0&ID=500;100&cat na região do Azure de DR.

>[!IMPORTANT]
>O volume de /hana/log não é replicado porque não é necessário restaurar a base de dados do SAP HANA replicada para um estado consistente no site de recuperação após desastre.

Em seguida, configurar ou ajustar o agendamento de cópia de segurança de instantâneo de armazenamento para obter o RTO e RPO, no caso de desastre. Para minimizar o objetivo de ponto de recuperação, defina os seguintes intervalos de replicação no serviço de instância grande do HANA:
- Para os volumes abrangidos pelo instantâneo combinado (tipo de instantâneo **hana**), o conjunto para replicar a cada 15 minutos para os destinos de volume de armazenamento equivalente no site de recuperação após desastre.
- Para o volume de cópia de segurança de registo de transação (tipo de instantâneo **registos**), o conjunto para replicar a cada 3 minutos para os destinos de volume de armazenamento equivalente no site de recuperação após desastre.

Para minimizar o objetivo de ponto de recuperação, configure o seguinte:
- Efetuar uma **hana** instantâneo de armazenamento do tipo (consulte "passo 7: executar instantâneos") a cada 30 minutos para 1 hora.
- Execute backups de log de transação do SAP HANA a cada 5 minutos.
- Efetuar uma **registos** escreva cada 5-15 minutos de instantâneos de armazenamento. Com este período de intervalo, alcançar um RPO de cerca de 15 a 25 minutos.

Com esta configuração, a seqüência de backups de log de transação, instantâneos de armazenamento e a replicação da transação HANA registar dados de volume e/hana/cópia de segurança e, como os dados mostrados neste gráfico, pode ser /hana/shared (inclui o sap/usr /):

 ![Relação entre um instantâneo de cópia de segurança de registo de transação e um espelho snap num eixo de tempo](./media/hana-overview-high-availability-disaster-recovery/snapmirror.PNG)

Para alcançar um RPO ainda melhor no caso de recuperação após desastre, pode copiar os backups de log de transação do HANA do SAP HANA no Azure (instâncias grandes) para a outra região do Azure. Para obter esta redução de RPO adicional, execute os seguintes passos:

1. Cópia de segurança da transação do HANA inicie sessão com tanta freqüência que possível para /hana/logbackups.
1. Utilize o rsync para copiar os backups de log de transação para as NFS partilha alojada máquinas virtuais do Azure. As VMs estão em redes virtuais do Azure na região do Azure de produção e nas regiões de DR. Tem de se ligar a ambas as redes virtuais do Azure para o circuito a ligar a instâncias grandes do HANA de produção para o Azure. Ver os gráficos da [considerações sobre a recuperação após desastre com o HANA nas instâncias grandes de rede](#Network-considerations-for-disaster recovery-with-HANA-Large-Instances) secção. 
1. Mantenha que os backups de log de transação na região na VM anexados para o NFS exportados armazenamento.
1. Um caso de ativação pós-falha do desastre, complemente os backups de log de transação encontradas no /hana/logbackups volume com mais recentemente backups de log de transação no NFS partilham no site de recuperação após desastre. 
1. Inicie uma cópia de segurança do registo de transações para restaurar a cópia de segurança mais recente que pode ser guardada em para a região de DR.

Quando as operações de instância grande do HANA confirmar a configuração de relação de replicação e iniciar as cópias de segurança de instantâneos de armazenamento de execução, começa a replicação de dados.

![Passo de configuração de DR antes de estabelecer a replicação](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start4.PNG)

Conforme o andamento da replicação, os instantâneos nos volumes PRD=IIS&sbp=&PVER=5.0&ID=500;100&cat nas regiões do Azure de DR não forem restaurado. Apenas são armazenados. Se os volumes são instalados num estado desse tipo, eles representam o estado em que desmontados desses volumes depois da instância de PRD=IIS&sbp=&PVER=5.0&ID=500;100&cat SAP HANA foi instalada na unidade do servidor na região do Azure de DR. Eles também representam as cópias de segurança de armazenamento que ainda não foram restauradas.

Em caso de uma ativação pós-falha, também pode optar por restaurar para um instantâneo de armazenamento mais antigo em vez do instantâneo de armazenamento mais recentes.

## <a name="disaster-recovery-failover-procedure"></a>Procedimento de ativação pós-falha de recuperação após desastre
Existem dois casos a serem consideradas ao fazer failover para o site de DR:

- Terá da base de dados do SAP HANA voltar para o estado mais recente dos dados. Neste caso, é um script de self-service com a qual pode realizar a ativação pós-falha sem a necessidade de contactar a Microsoft. No entanto, para a reativação pós-falha, terá de trabalhar com a Microsoft.
- Que pretende restaurar para um instantâneo de armazenamento que não seja o instantâneo mais recente replicado. Neste caso, terá de trabalhar com a Microsoft. 

>[!NOTE]
>As etapas a seguir precisam ser executadas na unidade instância grande do HANA, que representa a unidade de DR. 
 
Para restaurar para os instantâneos de armazenamento replicado mais recente, execute os seguintes passos: 

1. Encerre a instância de não produção do HANA na unidade de recuperação após desastre de instâncias grandes do HANA que estiver a executar. Isto acontece porque existe uma instância de produção de HANA Inativas pré-instalado.
1. Certifique-se de que não existem processos de SAP HANA estão em execução. Utilize o seguinte comando para esta verificação: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. A saída deve mostrar-lhe a **hdbdaemon** processos no estado parado e outros processos HANA num Estado em execução ou iniciado.
1. Na unidade de instância grande do HANA do site de DR, execute o script *azure_hana_dr_failover.pl*. O script pede para um SID de HANA SAP para serem restaurados. Quando solicitado, digite um ou o único SID do SAP HANA que foram replicadas e é mantida na *HANABackupCustomerDetails.txt* ficheiro na unidade instância grande do HANA no site de DR. 

      Se pretender ter várias instâncias do SAP HANA a ativação pós-falha, terá de executar o script várias vezes. Quando solicitado, introduza o SID do HANA SAP que pretende efetuar a ativação pós-falha e a restaurar. Após a conclusão, o script mostra uma lista de pontos de montagem dos volumes que são adicionados à unidade de instância grande do HANA. Esta lista inclui os volumes de DR restaurados.

1. Monte os volumes de recuperação após desastre restaurada utilizando comandos de sistema operativo Linux para a unidade de instância grande do HANA no site de recuperação após desastre. 
1. Inicie a instância de produção do SAP HANA Inativas.
1. Se optar por copiar registos de cópia de segurança de registo de transações para reduzir o tempo RPO, terá de unir essas cópias de segurança do registo de transação para o diretório de logbackups montado recentemente DR/hana /. Não substitua as cópias de segurança existentes. Copie as cópias de segurança mais recente não tem sido replicadas com a replicação mais recentes de um instantâneo de armazenamento.
1. Também pode restaurar ficheiros únicos fora os instantâneos que tem sido replicados para o volume de /hana/shared/PRD na região do Azure de DR. 

Também pode testar a ativação pós-falha de DR sem afetar a relação de replicação real. Para efetuar uma ativação pós-falha de teste, siga os passos anteriores, 1 e 2 e, em seguida, continuar com o seguinte passo 3.

>[!IMPORTANT]
>Fazer *não* executar quaisquer transações de produção na instância que criou no site através do processo de DR **uma ativação pós-falha de teste** com o script que introduziu no passo 3. Esse comando cria um conjunto de volumes que não têm qualquer relação para o site primário. Como resultado, é a sincronização de volta para o site primário *não* possíveis. 

Passo 3 para o teste de ativação pós-falha:

Na unidade de instância grande do HANA do site de DR, execute o script **azure_hana_test_dr_failover.pl**. Esse script é *não* a parar a relação de replicação entre o site primário e o site de DR. Em vez disso, este script é clonagem de volumes de armazenamento de DR. Depois do processo de clonagem tiver êxito, os volumes clonados são restaurados para o estado do instantâneo mais recente e, em seguida, montados para a unidade de DR. O script pede para um SID de HANA SAP para serem restaurados. Escreva o único ou de um SID do SAP HANA que foram replicadas e é mantida na *HANABackupCustomerDetails.txt* ficheiro na unidade instância grande do HANA no site de DR. 

Se pretender ter várias instâncias do SAP HANA para testar, terá de executar o script várias vezes. Quando solicitado, escreva SID SAP HANA de instância que pretende testar ativação pós-falha. Após a conclusão, o script mostra uma lista de pontos de montagem dos volumes que são adicionados à unidade de instância grande do HANA. Esta lista inclui os volumes de DR clonados.

Continue para o passo 4.

   >[!NOTE]
   >Se precisar de fazer a ativação pós-falha para o site de DR para resgatar a alguns dados que foi eliminados há horas e tem dos volumes de DR para ser definido como um instantâneo anterior, este procedimento aplica-se. 

1. Encerre a instância de não produção do HANA na unidade de recuperação após desastre de instâncias grandes do HANA que estiver a executar. Isto acontece porque existe uma instância de produção de HANA Inativas pré-instalado.
1. Certifique-se de que não existem processos de SAP HANA estão em execução. Utilize o seguinte comando para esta verificação: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. A saída deve mostrar-lhe a **hdbdaemon** processos no estado parado e outros processos HANA num Estado em execução ou iniciado.
1. Determine para qual nome de instantâneo ou um ID de cópia de segurança do SAP HANA que pretende ter o site de recuperação após desastre restaurado. Em casos de recuperação após desastre real, este instantâneo é normalmente o instantâneo mais recente. Se precisar de recuperar dados perdidos, escolha um instantâneo anterior.
1. Contacte o suporte do Azure através de um pedido de suporte de alta prioridade. Pedir o restauro de nesse instantâneo (com o nome e data do instantâneo) ou o ID de cópia de segurança do HANA no site de DR. A predefinição é que o lado de operações restaura/hana/volume de dados apenas. Se quiser ter os/hana/logbackups volumes também, terá de estado de especificamente. *Não restaure o volume de /hana/shared.* Em vez disso, deve escolher ficheiros específicos, como global.ini fora do **.snapshot** diretório e respetivos subdiretórios depois de voltar a montar o/hana/volume partilhado de PRD=IIS&sbp=&PVER=5.0&ID=500;100&cat. 

   No lado de operações, ocorrem os seguintes passos:

   a. A replicação de instantâneos do volume de produção para os volumes de recuperação após desastre está parada. Este interrupção pode ter acontecido já se uma falha no site de produção é o motivo pelo qual que precisa executar o procedimento de recuperação após desastre.
   
   b. O armazenamento de nome de instantâneo ou instantâneo com o ID de cópia de segurança que escolheu é restaurado em volumes de recuperação após desastre.
   
   c. Após o restauro, os volumes de recuperação após desastre estão disponíveis para ser montado para as unidades de instância grande do HANA na região de recuperação após desastre.
      
1. Monte os volumes de recuperação após desastre para a unidade de instância grande do HANA no site de recuperação após desastre. 
1. Inicie a instância de produção do SAP HANA Inativas.
1. Se optar por copiar registos de cópia de segurança de registo de transações para reduzir o tempo RPO, terá de unir essas cópias de segurança do registo de transação para o diretório de logbackups montado recentemente DR/hana /. Não substitua as cópias de segurança existentes. Copie as cópias de segurança mais recente não tem sido replicadas com a replicação mais recentes de um instantâneo de armazenamento.
1. Também pode restaurar ficheiros únicos fora os instantâneos que tem sido replicados para o volume de /hana/shared/PRD na região do Azure de DR.

A seguinte seqüência de etapas envolve a recuperar a instância de produção do SAP HANA com base no instantâneo de armazenamento restaurada e os backups de log de transação que estão disponíveis:

1. Altere a localização de cópia de segurança para **/hana/logbackups** com o SAP HANA Studio.
   ![Alterar a localização de cópia de segurança para a recuperação de DR](./media/hana-overview-high-availability-disaster-recovery/change_backup_location_dr1.png)

1. Examina as localizações de ficheiros de cópia de segurança e sugere o backup de log de transação mais recente para restaurar para o SAP HANA. A análise pode demorar alguns minutos até que uma tela, como aparece o seguinte: ![lista de backups de log de transação para a recuperação de DR](./media/hana-overview-high-availability-disaster-recovery/backup_list_dr2.PNG)

1. Ajuste algumas das configurações padrão:

      - Limpar **utilizar cópias de segurança de Delta**.
      - Selecione **inicializar a área de Log**.

   ![Definir a área de log de inicialização](./media/hana-overview-high-availability-disaster-recovery/initialize_log_dr3.PNG)

1. Selecione **Concluir**.

   ![Concluir o restauro de DR](./media/hana-overview-high-availability-disaster-recovery/finish_dr4.PNG)

Uma janela de progresso, como mostrado aqui, deve aparecer. Tenha em atenção que o exemplo é de um restauro de recuperação após desastre de uma configuração de SAP HANA de escalamento horizontal de três nós.

![Progresso de restauro](./media/hana-overview-high-availability-disaster-recovery/restore_progress_dr5.PNG)

Se o restauro parece suspender no **concluir** ecrã e não mostrar a tela de progresso, confirme que todas as instâncias do SAP HANA em nós de trabalho estão em execução. Se necessário, inicie manualmente o SAP HANA nas instâncias.


### <a name="failback-from-a-dr-to-a-production-site"></a>Reativação pós-falha a partir de um DR para um site de produção
Pode a reativação pós-falha a partir de um DR para um site de produção. Vamos examinar um cenário em que a ativação pós-falha para o site de recuperação após desastre foi causada por problemas na região do Azure de produção e não pela sua necessidade de recuperar dados perdidos. Tenham sido executados sua carga de trabalho de produção do SAP durante algum tempo no site de recuperação após desastre. Como são resolvidos os problemas do site de produção, pretende reativação pós-falha para o seu site de produção. Porque não pode perder os dados, o passo de volta para o site de produção envolve vários passos e cooperação íntima com o SAP HANA na equipe de operações do Azure. Cabe-lhe a si para acionar a equipe de operações para iniciar a sincronização para o site de produção após os problemas são resolvidos.

Esta é a seqüência de etapas a serem executadas:

1. O SAP HANA na equipe de operações do Azure obtém o acionador para sincronizar os volumes de armazenamento de produção dos volumes de armazenamento de recuperação após desastre, que agora representam o estado de produção. Neste estado, a unidade de instância grande do HANA no site de produção é encerrada.
1. O SAP HANA na equipe de operações do Azure monitoriza a replicação e certifica-se de que ela é atualizada com antes informando-o.
1. Encerrar as aplicações que utilizam a instância HANA de produção no local de recuperação de desastres. É, em seguida, executar uma cópia de segurança do registo de transações do HANA. Em seguida, parar a instância HANA em execução nas unidades de instância grande do HANA no site de recuperação após desastre.
1. Depois da instância HANA em execução na unidade instância grande do HANA no local de recuperação de desastres é encerrada, a equipe de operações manualmente sincroniza os volumes de disco novamente.
1. O SAP HANA na equipe de operações do Azure começa novamente a unidade de instância grande do HANA no site de produção e distribui-lo para. Certifique-se que a instância do SAP HANA está num Estado de encerramento no tempo de inicialização da unidade de instância grande do HANA.
1. Execute os mesmos passos de restauro de base de dados como fez quando anteriormente fazer failover para o site de recuperação após desastre.

### <a name="monitor-disaster-recovery-replication"></a>Monitorizar a replicação de recuperação após desastre

Pode monitorizar o estado de seu progresso de replicação de armazenamento ao executar o script `azure_hana_replication_status.pl`. Este script deve ser executado a partir de uma unidade em execução na localização de recuperação após desastre a funcionar conforme esperado. O script funciona independentemente se a replicação está ativa. O script pode ser executado para cada unidade de instância grande do HANA do seu inquilino na localização de recuperação após desastre. Ele não pode ser usado para obter detalhes sobre o volume de arranque.

Chame o script com este comando:
```
./replication_status.pl <HANA SID>
```

A saída é dividida, consoante o volume, nas seguintes secções:  

- Estado da ligação
- Atividade de replicação atual
- Instantâneo mais recente replicado 
- Tamanho do instantâneo mais recente
- Tempo de latência atual entre os instantâneos (entre a última replicação de instantâneo concluído e agora)

O estado da ligação é apresentado como **Active Directory** , a menos que a ligação entre locais está inativo ou existe um evento de ativação pós-falha em curso. A atividade de replicação endereços independentemente de quaisquer dados está atualmente a ser replicados ou está inativos, ou se outras atividades são a acontecer à ligação. O último instantâneo replicado apenas deve aparecer como `snapmirror…`. O tamanho do último instantâneo, em seguida, é apresentado. Por fim, o tempo de atraso é mostrado. O tempo de atraso representa o tempo da replicação agendada para quando concluída a replicação. Um tempo de atraso pode ser maior do que uma hora para a replicação de dados, especialmente na replicação inicial, apesar dos replicação foi iniciada. O tempo de latência continua a aumentar até que seja concluída a replicação contínua.

Segue-se um exemplo da saída:

```
hana_data_hm3_mnt00002_t020_dp
-------------------------------------------------
Link Status: Broken-Off
Current Replication Activity: Idle
Latest Snapshot Replicated: snapmirror.c169b434-75c0-11e6-9903-00a098a13ceb_2154095454.2017-04-21_051515
Size of Latest Snapshot Replicated: 244KB
Current Lag Time between snapshots: -   ***Less than 90 minutes is acceptable***
```












