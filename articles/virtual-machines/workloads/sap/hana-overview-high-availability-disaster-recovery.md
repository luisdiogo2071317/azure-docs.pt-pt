---
title: Elevada disponibilidade e recuperação após desastre de SAP HANA no Azure (instâncias de grande) | Microsoft Docs
description: Estabelecer a elevada disponibilidade e o plano de recuperação de desastres do SAP HANA no Azure (instâncias de grandes)
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
ms.date: 05/30/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9c4c126663d34d65cc7e0aa641bf93b848a5dcae
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34658320"
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>SAP HANA grande instâncias elevada disponibilidade e recuperação após desastre no Azure 

>[!IMPORTANT]
>Esta documentação não é nenhuma substituição da documentação de administração de SAP HANA ou SAP notas. É esperado que o leitor tem um sólida compreensão dos e conhecimentos em operações, especialmente com os tópicos de cópia de segurança, restauro, elevada disponibilidade e recuperação após desastre e de administração de SAP HANA. Nesta documentação, capturas de ecrã de SAP HANA Studio são apresentadas. Conteúdo, estrutura e a natureza ecrãs de ferramentas de administração do SAP e as ferramentas próprios poderão alterar de SAP HANA versão para versão.

É importante que exerce passos e processos de tomada no seu ambiente e com as suas versões HANA e versões. Alguns processos descritos nesta documentação são simplificados para uma melhor compreensão geral e não se destinam a ser utilizado como obter os passos detalhados para handbooks eventual operação. Se pretender criar handbooks operação para as configurações, terá de testar e exercer os seus processos e documentar os processos relacionados com as suas configurações específicas. 


Elevada disponibilidade e recuperação após desastre (DR) são cruciais aspetos da sua SAP HANA fundamentais a ser executado no servidor do Azure (instâncias de grande). É importante trabalhar com o SAP, sua integrador de sistema ou Microsoft corretamente architect e implementar o direita de elevada disponibilidade e estratégias de recuperação após desastre. Também é importante considerar o objetivo de ponto de recuperação (RPO) e o objetivo de tempo de recuperação, que são específicas para o seu ambiente.

Microsoft suporta algumas capacidades de elevada disponibilidade de SAP HANA com instâncias de grande HANA. Estas funcionalidades incluem:

- **Replicação de armazenamento**: capacidade do sistema de armazenamento de replicar todos os dados para outro instância grande HANA carimbo na outra região do Azure. SAP HANA funciona independentemente este método. Esta funcionalidade é o mecanismo de recuperação de desastre predefinido é fornecido para instâncias de grande HANA.
- **Replicação do sistema HANA**: O [replicação de todos os dados no SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html) para um sistema de SAP HANA separado. O objetivo de tempo de recuperação é minimizado através da replicação de dados em intervalos regulares. SAP HANA suporta assíncronos, síncronos modos de memória e síncronos. Modo síncrono é utilizado apenas para os sistemas de SAP HANA que estão dentro do mesmo centro de dados ou inferior a 100 km, à excepção. Com o design de carimbos de instância grande HANA, a replicação do sistema HANA pode ser utilizada para elevada disponibilidade no apenas uma região. Replicação do sistema HANA requer um proxy inverso de terceiros ou o componente de encaminhamento para configurações de recuperação após desastre para outra região do Azure. 
- **Auto-ativação pós-falha do anfitrião**: uma solução de recuperação-falha do local para SAP HANA que é uma alternativa à replicação do sistema HANA. Se o nó principal ficar indisponível, configure um ou mais nós de SAP HANA espera no modo de escalamento horizontal e SAP HANA automaticamente falhar um nó em modo de espera.

SAP HANA no Azure (instâncias de grande) é fornecido em duas regiões do Azure em quatro áreas geopolíticas (E.U.A., da Austrália, Europa e Japão). Duas regiões dentro de uma área geopolítica que alojam os carimbos de data / instância grande HANA estão ligadas a circuitos de rede dedicado separada. Estes são utilizados para replicar os instantâneos de armazenamento para fornecer os métodos de recuperação de desastre. A replicação não for estabelecida por predefinição, mas está configurada para os clientes que ordem a funcionalidade de recuperação após desastre. Replicação de armazenamento está dependente da utilização de instantâneos de armazenamento de instâncias de grande HANA. Não é possível escolher uma região do Azure como uma região DR numa área geopolítica diferentes. 

A tabela seguinte mostra as combinações e atualmente elevados disponibilidade e após desastre recuperação métodos suportados:

| Cenário suportado em instâncias de grande HANA | Opção de elevada disponibilidade | Opção de recuperação de desastre | Comentários |
| --- | --- | --- | --- |
| Nó único | Não está disponível. | Configuração de DR dedicada.<br /> Configuração de Multipurpose DR. | |
| Auto-ativação pós-falha do anfitrião: N + m<br /> incluindo 1 + 1 | Possível com o modo de espera colocar a função do Active Directory.<br /> HANA controla o comutador de função. | Configuração de DR dedicada.<br /> Configuração de Multipurpose DR.<br /> Sincronização de DR utilizando a replicação de armazenamento. | Conjuntos de volume HANA estão ligados a todos os nós (n + m).<br /> Site de DR tem de ter o mesmo número de nós. |
| Replicação do sistema HANA | Possível com a configuração primária ou secundária.<br /> Move secundário para a função primária em caso de ativação pós-falha.<br /> Ativação pós-falha do controlo de replicação do sistema HANA e SO. | Configuração de DR dedicada.<br /> Configuração de Multipurpose DR.<br /> Sincronização de DR utilizando a replicação de armazenamento.<br /> DR utilizando a replicação do sistema HANA ainda não é possível sem componentes de terceiros. | Conjunto separado dos volumes de discos anexados a cada nó.<br /> Apenas os volumes de disco de réplica secundária no site de produção são replicadas para a localização de DR.<br /> É necessário um conjunto de volumes no site de DR. | 

Uma configuração de DR dedicada é onde a unidade de instância grande HANA no site de DR não é utilizada para executar qualquer carga de trabalho ou sistema de não produção. A unidade é passiva e é implementada apenas se for executada uma ativação pós-falha após desastre. No entanto, esta configuração não é uma opção preferencial para muitos clientes.

> [!NOTE]
> [Implementações de SAP HANA MCOD](https://launchpad.support.sap.com/#/notes/1681092) (várias instâncias de HANA numa unidade) como sobrepor cenários trabalho com o HA e DR métodos listados na tabela. Uma exceção é a utilização de replicação do sistema HANA com um cluster de ativação pós-falha automática com base no Pacemaker. Nesse caso só suporta uma instância HANA por unidade. Para [SAP HANA MDC](https://launchpad.support.sap.com/#/notes/2096000) implementações, apenas métodos HA e DR com base no armazenamento funcionam se mais do que um inquilino for implementado. Com um inquilino implementado, todos os métodos listados são válidos.  

Uma configuração de DR multipurpose é onde a unidade de instância grande HANA no site de DR executa uma carga de trabalho de não produção. Em caso de desastre, encerrar o sistema de não produção, monte os conjuntos de armazenamento replicado volume (adicionais) e, em seguida, inicie a instância HANA de produção. Maioria dos clientes que utilizam a funcionalidade de recuperação de desastre instância grande HANA utilizar esta configuração. 


Pode encontrar mais informações sobre a disponibilidade elevada de SAP HANA nos seguintes artigos SAP: 

- [Documento técnico de elevada disponibilidade do SAP HANA](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [Guia de administração do SAP HANA](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [Vídeo de SAP HANA Academy na replicação do sistema de SAP HANA](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [Nota de suporte #1999880 – FAQ na replicação do sistema de SAP HANA de SAP](https://bcs.wdf.sap.corp/sap/support/notes/1999880)
- [SAP suporte Nota 2165547 # – SAP HANA novamente a cópia de segurança e restaurar o ambiente de SAP HANA sistema replicação](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [SAP suporte Nota #1984882 – utilizando SAP HANA replicação do sistema para o Exchange do Hardware com o período de indisponibilidade mínimo/Zero](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="network-considerations-for-disaster-recovery-with-hana-large-instances"></a>Considerações de rede para a recuperação de desastre com instâncias de grande HANA

Para tirar partido da funcionalidade de recuperação após desastre de instâncias de grande HANA, terá de conceber a conectividade de rede para as duas regiões do Azure. Precisará de uma ligação de circuito ExpressRoute do Azure no local na sua região do Azure principal e outra ligação circuito no local para a região de recuperação após desastre. Esta medida abrange uma situação em que há um problema com uma região do Azure, incluindo uma localização de Router de limite de Enterprise do Microsoft (MSEE).

Como uma segunda medida, pode ligar todas as redes virtuais do Azure que se ligam ao SAP HANA no Azure (instâncias de grande) na região de um para um circuito de ExpressRoute liga HANA instâncias grande na outra região. Com esta *cruzada ligar*, os serviços executados num Azure pode ligar uma rede virtual na região 1 para unidades de instância grande HANA por região 2 e o inverso. Esta medida endereços um caso em que apenas uma das localizações MSEE que liga à sua localização no local com o Azure fica offline.

O gráfico seguinte ilustra uma configuração resiliente para cenários de recuperação após desastre:

![Configuração ideal para recuperação após desastre](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)



## <a name="other-requirements-with-hana-large-instances-storage-replication-for-disaster-recovery"></a>Outros requisitos com a replicação de armazenamento de instâncias de grande HANA para recuperação após desastre

Além dos requisitos para uma configuração de recuperação após desastre com instâncias de grande HANA anteriores, tem de:

- Ordem de SAP HANA no Azure (instâncias de grande) SKUs do mesmo tamanho que os SKUs de produção e implementá-las na região de recuperação após desastre. Nas implementações de cliente atual, estas instâncias são utilizadas para executar instâncias HANA de não produção. Estas configurações são referidas como *multipurpose DR setups*.   
- Ordem de armazenamento adicional no site de DR para cada um dos seus SAP HANA no SKUs do Azure (instâncias de grande) que pretende recuperar no site de recuperação após desastre. Comprar armazenamento adicional permite-lhe alocar os volumes de armazenamento. Pode alocar os volumes que são o destino de replicação de armazenamento da sua região do Azure de produção para a recuperação de desastre região do Azure.
- No caso de onde configurou HSR no principal e configurar a replicação de armazenamento baseado no site de DR, tem de comprar armazenamento adicional no site de DR, por isso, os dois principais e os dados de nós secundários obtém replicados para o site de DR.

 

## <a name="backup-and-restore"></a>Cópia de segurança e restauro

Um dos aspetos mais importantes para bases de dados de sistema operativos é protegê-los a partir de eventos catastrófica. A causa destes eventos pode ser qualquer coisa desde perante desastres naturais erros de utilizador simples.

Cópia de segurança das bases de dados, a capacidade de restaurá-lo para qualquer ponto no tempo (tal como alguém eliminar dados críticos), permite que o restauro para um Estado que é o mais parecido possível com a forma como foi antes da interrupção.

Dois tipos de cópias de segurança tem de ser efetuados para obter os melhores resultados:

- As cópias de segurança da base de dados: cópias de segurança completas, incrementais ou diferenciais
- Cópias de segurança de registos de transação

Para além das cópias de segurança de base de dados completo efetuadas ao nível de uma aplicação, pode efetuar cópias de segurança com instantâneos de armazenamento. Instantâneos de armazenamento não substituir cópias de segurança de registo de transações. Cópias de segurança de registo de transações permanecem importantes para restaurar a base de dados até um certo ponto no tempo ou vazio os registos de transações consolidadas já. No entanto, os instantâneos de armazenamento podem acelerar a recuperação ao fornecer rapidamente uma imagem de reencaminhamento de agregação da base de dados. 

SAP HANA no Azure (instâncias de grande) oferece duas opções de cópia de segurança e restauro:

- Do-IT-yourself (DIY). Depois de calcular para garantir que existe espaço suficiente em disco, efetue a base de dados completo e cópias de segurança do registo utilizando um dos seguintes métodos de cópia de segurança de disco. Pode criar cópias de segurança diretamente a volumes anexados às unidades instância grande HANA ou para partilhas de ficheiros de rede (NFS) que são definidas numa máquina virtual do Azure (VM). No caso desta última opção, os clientes configurar uma VM com Linux no Azure, anexar armazenamento do Azure para a VM em partilhem o armazenamento através de um servidor NFS configurado nessa VM. Se efetuar a cópia de segurança em volumes que ligue diretamente às unidades de instância grande HANA, terá de copiar as cópias de segurança para uma conta de armazenamento do Azure (depois de configurar uma VM do Azure que exporta as partilhas NFS baseiam-se no armazenamento do Azure). Também pode utilizar um cofre de cópia de segurança do Azure ou o armazenamento de frio do Azure. 

   Outra opção consiste em utilizar uma ferramenta de proteção de dados de terceiros para armazenar as cópias de segurança após são copiados para uma conta de armazenamento do Azure. A opção de cópia de segurança DIY também poderá ser necessária para dados que precisa para armazenar por períodos mais longos de tempo para fins de auditorias e de conformidade. Em todos os casos, as cópias de segurança são copiadas para as partilhas NFS representadas através de uma VM e o armazenamento do Azure.

- Infraestrutura de cópia de segurança e restaurar a funcionalidade. Também pode utilizar a cópia de segurança e restaurar a funcionalidade que fornece a infraestrutura subjacente de SAP HANA no Azure (instâncias de grande). Esta opção satisfaz a necessidade de cópias de segurança e restauros rápidos. O resto desta secção aborda a funcionalidade de cópia de segurança e restauro é oferecida com instâncias de grande HANA. Esta secção também inclui a cópia de segurança de relação e desastre tem de restaurar a funcionalidade de recuperação oferecidos por instâncias grande HANA.

>   [!NOTE]
>   A tecnologia de instantâneo que é utilizada pela infraestrutura subjacente de instâncias de grande HANA tem uma dependência no instantâneos de SAP HANA. Neste momento, os instantâneos de SAP HANA não funcionam em conjunto com múltiplos inquilinos de contentores de base de dados multi-inquilino de SAP HANA. Se apenas um inquilino for implementado, os instantâneos de SAP HANA funcionam e este método pode ser utilizado.

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Utilizar instantâneos de armazenamento de SAP HANA no Azure (instâncias de grandes)

A infraestrutura de armazenamento subjacente SAP HANA no Azure (instâncias de grande) suporta instantâneos de armazenamento de volumes. Cópia de segurança e restauro de volumes é suportada com as seguintes considerações:

- Em vez de cópias de segurança da base de dados completo, são tirados instantâneos de volume de armazenamento com frequência.
- Quando um instantâneo a acionar através de /hana/data e /hana/shared (inclui /usr/sap) volumes, a tecnologia de instantâneo inicia uma SAP HANA antes de ser executada o instantâneo de armazenamento de instantâneos. Este instantâneo de SAP HANA é o ponto de configuração para restorations eventual registo após a recuperação do instantâneo de armazenamento. Para instantâneo HANA seja concluída com êxito, é necessário uma instância HANA Active Directory.  Cenário HSR, os instantâneos de armazenamento não é suportado no nó secundário atual onde não é possível efetuar o instantâneo HANA.
- Depois do instantâneo de armazenamento foi executado com êxito, é eliminado o instantâneo de SAP HANA.
- Cópias de segurança de registo de transações efetuadas com frequência e são armazenadas no /hana/logbackups volume ou no Azure. Pode acionar o volume de /hana/logbackups que contém as cópias de segurança de registo de transações para criar um instantâneo separadamente. Nesse caso, não terá de executar um instantâneo HANA.
- Se tem de restaurar uma base de dados para um determinado ponto no tempo, solicitar que suporte do Microsoft Azure (para uma falha de produção) ou SAP HANA no restauro de gestão de serviço do Azure para um determinado instantâneo de armazenamento. Um exemplo é um restauro planeado de um sistema de sandbox para o seu estado original.
- O instantâneo de SAP HANA que está incluído no instantâneo de armazenamento é um ponto de deslocamento para aplicar cópias de segurança do registo transações que foram executadas e armazenadas após o armazenamento de instantâneo foi tirado.
- Estas cópias de segurança do registo de transações direcionadas para restaurar a base de dados para um determinado ponto no tempo.

Pode efetuar a filtragem de três classes de volumes de instantâneos de armazenamento:

- Um instantâneo combinado através de dados/hana/e /hana/shared (inclui/usr/sap). Este instantâneo requer a criação de um instantâneo de SAP HANA como preparação para o instantâneo de armazenamento. O instantâneo de SAP HANA certifica-se de que a base de dados está num estado consistente a partir de um ponto de vista do armazenamento. E que para o restauro de processo que é um ponto para definir a segurança no.
- Um instantâneo separado através de hana/logbackups.
- Uma partição do sistema operativo.

Obter os scripts de instantâneo mais recente e a documentação do [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). 

### <a name="storage-snapshot-considerations"></a>Considerações de instantâneos de armazenamento

>[!NOTE]
>Instantâneos de armazenamento consumam o espaço de armazenamento que foi atribuído para as unidades de instância grande HANA. Precisa de considerar os seguintes aspetos de agendamento de instantâneos de armazenamento e o número de instantâneos de armazenamento para manter. 

Os mechanics específicos de instantâneos de armazenamento para SAP HANA no Azure (instâncias de grande) incluem:

- Um instantâneo de armazenamento específico (na altura quando é executada) consome armazenamento reduzida.
- Como as alterações de conteúdo de dados e o conteúdo de dados SAP HANA ficheiros alterados no volume de armazenamento, o instantâneo necessita de armazenar o conteúdo original do bloco, bem como as alterações de dados.
- Como resultado, o instantâneo de armazenamento aumenta de tamanho. O já existe o instantâneo, fica do quanto maior for o instantâneo de armazenamento.
- As mais as alterações efetuadas ao volume de base de dados SAP HANA durante a duração de um instantâneo de armazenamento, a maior o consumo de espaço do instantâneo de armazenamento.

SAP HANA no Azure (instâncias de grande) inclui os tamanhos de volume fixo para os volumes de dados e de registo de SAP HANA. Efetuar instantâneos desses volumes eats no espaço de volume. Terá de determinar quando deve agendar instantâneos de armazenamento. Também precisa de monitorizar o consumo de espaço dos volumes de armazenamento, bem como gerir o número de instantâneos que armazena. Pode desativar os instantâneos de armazenamento quando importar masses de dados ou efetuar outras alterações significativas na base de dados HANA. 


As secções seguintes fornecem informações para efetuar estes instantâneos, incluindo recomendações gerais:

- Apesar do hardware pode suportar 255 instantâneos por volume, em que pretende manter este número também abaixo. Recomendação é 250 ou menos.
- Antes de efetuar os instantâneos de armazenamento, monitorizar e manter o registo do espaço livre.
- Reduzi o número de instantâneos de armazenamento com base no espaço livre. Pode reduzir o número de instantâneos que mantenha ou, pode expandir os volumes. Pode ordenar o armazenamento adicional em unidades de 1 terabyte.
- Durante a atividades como mover dados para SAP HANA com ferramentas de migração de plataforma do SAP (R3load) ou o restauro de bases de dados SAP HANA de cópias de segurança, desative os instantâneos de armazenamento no /hana/data volume. 
- Durante a maior reorganizations das tabelas de SAP HANA, os instantâneos de armazenamento devem ser evitados, se possível.
- Os instantâneos de armazenamento são um pré-requisito para tirar partido do desastre funcionalidades de recuperação de SAP HANA no Azure (instâncias de grande).

### <a name="prerequisites-for-using-self-service-storage-snapshots"></a>Pré-requisitos para utilizar instantâneos de armazenamento de self-service

Para garantir que o script de instantâneo é executada com êxito, certifique-se de que o Perl está instalado no sistema operativo Linux no servidor de instâncias de grande HANA. Perl é previamente instalado na sua unidade de instância grande HANA. Para verificar a versão de Perl, utilize o seguinte comando:

`perl -v`

![A chave pública é copiada por executar este comando](./media/hana-overview-high-availability-disaster-recovery/perl_screen.png)


### <a name="set-up-storage-snapshots"></a>Configurar os instantâneos de armazenamento

Para configurar os instantâneos de armazenamento com instâncias de grande HANA, siga estes passos:
1. Certifique-se de que o Perl está instalado no sistema operativo Linux no servidor de instâncias de grande HANA.
2. Modificar o etc/ssh/ssh\_configuração para adicionar a linha _MACs hmac-sha1_.
3. Crie uma conta de utilizador de cópia de segurança de SAP HANA no nó principal para cada instância de SAP HANA que estiver a executar, se aplicável.
4. Instale o cliente de SAP HANA HDB em todos os servidores de instâncias de grandes dimensões do SAP HANA.
5. No primeiro servidor de SAP HANA grande instâncias de cada região, crie uma chave pública para aceder a infraestrutura de armazenamento subjacente que controla a criação do instantâneo.
6. Copiar os scripts e um ficheiro de configuração da [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) para a localização do **hdbsql** na instalação do SAP HANA.
7. Modificar o *HANABackupDetails.txt* ficheiro conforme necessário para as especificações de cliente adequado.

Obter os scripts de instantâneo mais recente e a documentação do [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). 

### <a name="consideration-for-mcod-scenarios"></a>Considerações para cenários MCOD
Se estiver a executar um [cenário MCOD](https://launchpad.support.sap.com/#/notes/1681092) com várias instâncias de SAP HANA na unidade de instância grande HANA um, tem volumes de armazenamento separada aprovisionados para cada uma das instâncias de SAP HANA. Na versão atual da automatização de instantâneo self-service, não é possível iniciar instantâneos separados em cada sistema de instância HANA ID (SID). A funcionalidade fornece verificações para as instâncias de SAP HANA registadas do servidor no ficheiro de configuração (consulte neste artigo) e executa um instantâneo simultâneo dos volumes de todas as instâncias registados na unidade.
 

### <a name="step-1-install-the-sap-hana-hdb-client"></a>Passo 1: Instalar o cliente de SAP HANA HDB

O sistema de operativo Linux instalado SAP HANA no Azure (instâncias de grande) inclui as pastas e os scripts necessários para executar os instantâneos de armazenamento de SAP HANA para fins de recuperação após desastre e de cópia de segurança. Verifique a existência de versões mais recentes no [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). A versão mais recente dos scripts é 3. Scripts diferentes podem ter diferentes versões de secundárias dentro da mesma versão principal.

>[!IMPORTANT]
>Ao mover a partir da versão 2.1 para a versão 3 dos scripts, tenha em atenção que a estrutura de algumas sintaxe e o ficheiro de configuração foi alterada. Consulte o callouts nas secções específicas. 

É da responsabilidade do cliente para instalar o cliente de SAP HANA HDB das unidades de instância grande HANA enquanto estiver a instalar o SAP HANA.

### <a name="step-2-change-the-etcsshsshconfig"></a>Passo 2: Altere o etc/ssh/ssh\_configuração

Alteração `/etc/ssh/ssh_config` adicionando o _MACs hmac-sha1_ linha conforme mostrado aqui:
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

Para ativar o acesso para as interfaces instantâneos de armazenamento do seu inquilino de instância grande HANA, tem de estabelecer um procedimento de início de sessão através de uma chave pública. No primeiro SAP HANA no servidor do Azure (instâncias de grande) no seu inquilino, crie uma chave pública a utilizar para aceder à infraestrutura de armazenamento. A chave pública garante que uma palavra-passe não é necessário para iniciar sessão para as interfaces instantâneos de armazenamento. Criar uma chave pública também significa que não é necessário manter as credenciais de palavra-passe. No Linux no servidor de SAP HANA grande instâncias, execute o seguinte comando para gerar a chave pública:
```
  ssh-keygen –t dsa –b 1024
```
A nova localização é **_/root/.ssh/id\_dsa.pub**. Não introduza uma palavra-passe real; caso contrário, tem de introduzir a palavra-passe sempre que iniciar sessão. Em vez disso, selecione **Enter** duas vezes para remover o requisito de "Introduza a palavra-passe" para iniciar sessão.

Certifique-se de que a chave pública foi corrigida conforme esperado, alterando as pastas para **/root/.ssh/** e, em seguida, executar o `ls` comando. Se a chave estiver presente, pode copiá-lo executando o seguinte comando:

![A chave pública é copiada por executar este comando](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

Neste momento, contacte SAP HANA na gestão de serviço do Azure e forneça-las com a chave pública. O representante de serviço utiliza a chave pública para registá-lo na infraestrutura de armazenamento subjacente que retirar para o seu inquilino de instância grande HANA.

### <a name="step-4-create-an-sap-hana-user-account"></a>Passo 4: Criar uma conta de utilizador de SAP HANA

Para iniciar a criação de instantâneos de SAP HANA, terá de criar uma conta de utilizador no SAP HANA que podem utilizar os scripts de instantâneos de armazenamento. Crie uma conta de utilizador de SAP HANA SAP HANA Studio para esta finalidade. O utilizador tem de criar sob o SYSTEMDB e não sob a base de dados de SID para MDC. No ambiente de contentor único, o utilizador está configurado com a base de dados do inquilino. Esta conta tem de ter os seguintes privilégios: **Admin de cópia de segurança** e **catálogo leitura**. Neste exemplo, o nome de utilizador é **SCADMIN**. O nome da conta de utilizador criado no HANA Studio diferencia maiúsculas de minúsculas. Certifique-se de que seleciona **não** para exigir ao utilizador alterar a palavra-passe no seu próximo início de sessão.

![Criar um utilizador no HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

Se utilizar MCOD implementações com várias instâncias de SAP HANA numa unidade, terá de repetir este passo para cada instância de SAP HANA.

### <a name="step-5-authorize-the-sap-hana-user-account"></a>Passo 5: Autorizar a conta de utilizador de SAP HANA

Neste passo, está a autorizar a conta de utilizador de SAP HANA que criou, para que os scripts não precisam de envio de palavras-passe no tempo de execução. O comando de SAP HANA `hdbuserstore` permite a criação de uma chave de utilizador de SAP HANA, que é armazenada num ou mais nós de SAP HANA. A chave de utilizador permite ao utilizador acesso SAP HANA sem ter de gerir palavras-passe no processo do script. O processo de script é abordado posteriormente neste artigo.

>[!IMPORTANT]
>Execute o seguinte comando em utilizador os scripts que estejam planeados para ser executada. Caso contrário, o script não funciona corretamente.

Introduza o `hdbuserstore` comando da seguinte forma:

**Para a configuração de HANA não MDC**
```
hdbuserstore set <key> <host>:<3[instance]15> <user> <password>
```

**Para a configuração de MDC HANA**
```
hdbuserstore set <key> <host>:<3[instance]13> <user> <password>
```

No exemplo seguinte, o utilizador é **SCADMIN01**, o nome do anfitrião é **lhanad01**, e é o número de instância **01**:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Se utilizar uma implementação de HANA MCOD com várias instâncias de SAP HANA numa unidade, terá de repetir o passo para cada instância de SAP HANA e o utilizador de cópia de segurança associado na unidade.

Se tiver uma configuração de escalamento horizontal de SAP HANA, terá de gerir todos os scripts de um único servidor. Neste exemplo, a chave de SAP HANA **SCADMIN01** tem de ser alterado para cada anfitrião de uma forma que mostra que o anfitrião está relacionado com a chave. Corrigir a conta de cópia de segurança de SAP HANA com o número de instância de BD HANA. A chave tem de ter privilégios administrativos no anfitrião para a qual está atribuído e o utilizador de cópia de segurança para configurações de escalamento horizontal tem de ter direitos de acesso a todas as instâncias de SAP HANA. Partindo do princípio de três nós de escalamento horizontal tem os nomes **lhanad01**, **lhanad02**, e **lhanad03**, a sequência de comandos tem o seguinte aspeto:

```
hdbuserstore set SCADMIN01 lhanad01:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad02:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad03:30115 SCADMIN <password>
```

### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>Passo 6: Obter os scripts de instantâneo, os instantâneos de configurar e testar a configuração e conectividade

Transferir a versão mais recente dos scripts de [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). Copiar os scripts transferidos e o ficheiro de texto para o directório de trabalho **hdbsql**. Para instalações de HANA atuais, este diretório é o seguinte formato: /hana/shared/D01/exe/linuxx86\_64/hdb. 
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

- Nunca modificar os scripts, a menos que indicado pelo Microsoft Operations.
- Quando lhe for pedido para modificar o script ou um ficheiro de parâmetros, utilize sempre o editor de texto de Linux como "vi" e não um editor de Windows, como o bloco de notas. Com um editor de Windows poderá danificar o formato de ficheiro.
- Utilize sempre os scripts mais recentes. Pode transferir a versão mais recente a partir do GitHub.
- Utilize a mesma versão de scripts para a horizontal.
- Testar os scripts e obter familiarizado com os parâmetros necessários e a saída do script antes de utilizá-los diretamente no sistema de produção.
- Não altere o nome do ponto de montagem do servidor aprovisionado através do Microsoft Operations. Estes scripts dependem destes pontos de montagem padrão para estar disponível para uma execução bem sucedida.


O objetivo das diferentes scripts e ficheiros é o seguinte:

- **Azure\_hana\_backup.pl**: este script está agendado com o utilitário de agendamento de Cron do Linux para executar os instantâneos de armazenamento em dados HANA e volumes partilhados, o volume de logbackups/hana/ou o sistema operativo.
- **Azure\_hana\_replicação\_status.pl**: este script fornece os detalhes básicos em todo o estado de replicação do site de produção para o site de recuperação após desastre. Os monitores de script para se certificar de que a replicação está a decorrer e mostra o tamanho dos itens que estão a ser replicados. Também fornece orientações se uma replicação estiver a demorar demasiado tempo ou se a ligação está inativo.
- **Azure\_hana\_instantâneo\_details.pl**: este script fornece uma lista de detalhes básicos sobre todos os instantâneos, por volume, o que existe no seu ambiente. Este script pode ser executado no servidor primário ou numa unidade servidor na localização de recuperação após desastre. O script fornece as seguintes informações, discriminadas segundo cada volume que contém os instantâneos:
   * O tamanho de instantâneos totais um volume
   * Os detalhes seguintes em cada instantâneo nesse volume: 
      - Nome de instantâneo 
      - Hora de criação 
      - Tamanho do instantâneo
      - Frequência de instantâneo
      - ID de cópia de segurança de HANA associado nesse instantâneo, se forem relevantes
- **Azure\_hana\_instantâneo\_delete.pl**: este script elimina um instantâneo de armazenamento ou um conjunto de instantâneos. Pode utilizar o ID de cópia de segurança de SAP HANA como encontrada no HANA Studio ou o nome de instantâneos de armazenamento. Atualmente, o ID de cópia de segurança só está associado aos instantâneos criados para os volumes de dados/registo/partilhados HANA. Caso contrário, se o ID de instantâneo é introduzido,-procura todos os instantâneos que correspondem o ID de instantâneo introduzido.  
- **testHANAConnection.pl**: este script testa a ligação à instância do SAP HANA e é necessário para configurar os instantâneos de armazenamento.
- **testStorageSnapshotConnection.pl**: este script tem duas finalidades. Em primeiro lugar, assegura que a unidade de instância grande HANA que executa os scripts tem acesso para a máquina virtual de armazenamento atribuído e a interface de instantâneos de armazenamento de instâncias de grande sua HANA. O segundo objetivo consiste em criar um instantâneo temporário para a instância HANA que estiver a testar. Este script deve ser executado para todas as instâncias HANA num servidor para garantir que os scripts de cópia de segurança função conforme esperado.
- **removeTestStorageSnapshot.pl**: este script elimina o instantâneo de teste criado com o script **testStorageSnapshotConnection.pl**.
- **Azure\_hana\_dr\_failover.pl**: este script inicia uma ativação pós-falha de DR para noutra região. O script tem de ser executado na unidade instância grande HANA na região DR ou na unidade que pretende efetuar a ativação pós-falha. Este script deixa de replicação de armazenamento do lado primário para o lado secundária, restaura o instantâneo mais recente nos volumes de DR e fornece o mountpoints para a DR volumes.
- **Azure\_hana\_testar\_dr\_failover.pl**: este script executa uma ativação pós-falha de teste para o site de DR. Ao contrário do azure_hana_dr_failover.pl script, a execução não interromper a replicação de armazenamento do primário para o secundário. Em vez disso, clones dos volumes de armazenamento replicado são criados no lado de DR, e são fornecidos mountpoints dos volumes clonados. 
- **HANABackupCustomerDetails.txt**: este ficheiro é um ficheiro de configuração modificável que terá de modificar para adaptar à sua configuração de SAP HANA. O *HANABackupCustomerDetails.txt* ficheiro é o ficheiro de controlo e a configuração para o script que executa os instantâneos de armazenamento. Ajuste o ficheiro para os fins e a configuração. Recebe o **nome de cópia de segurança de armazenamento** e **endereço de IP de armazenamento** de SAP HANA na gestão de serviço do Azure quando implementar as instâncias. Não é possível modificar a sequência de ordenação ou espaçamento de qualquer uma das variáveis existentes neste ficheiro. Se o fizer, os scripts não são executados corretamente. Além disso, recebe o endereço IP do nó de vertical ou o nó mestre (se escalável) de SAP HANA na gestão de serviço do Azure. Também souber o número de instância HANA obter durante a instalação de SAP HANA. Agora, terá de adicionar um nome de cópia de segurança para o ficheiro de configuração.

Para uma implementação de vertical ou Escalamento horizontal, o ficheiro de configuração deverá ter um aspeto semelhante ao seguinte exemplo depois de preencher o nome do servidor da unidade de instância grande HANA e o endereço IP do servidor. Preencha todos os campos necessários para cada SID de HANA SAP que pretende criar cópias de segurança ou recuperação.

Também pode comente linhas de instâncias que não pretende criar cópias de segurança para um período de tempo, adicionando um "#" à frente de um campo obrigatório. Também não tem de introduzir todos os SAP HANA as instâncias que estão contidas num servidor, se não é necessário para criar cópias de segurança ou recuperação essa instância específica. O formato têm de ser mantido para todos os campos, ou todos os scripts gerem uma mensagem de erro e termina o script. Pode eliminar linhas necessárias adicionais de quaisquer detalhes de informações de SID que não estiver a utilizar após a última instância de SAP HANA em utilização. Todas as linhas tem de ser o preenchidos, comentadas ou eliminadas.

>[!IMPORTANT]
>Alterar a estrutura do ficheiro com a mudança da versão 2.1 para a versão 3. Se pretender utilizar os scripts de versão 3, terá de se adaptar a estrutura do ficheiro de configuração. 


```
HANA Server Name: testing01
HANA Server IP Address: 172.18.18.50
```

Para cada instância que configurar na unidade instância grande HANA ou para a configuração de escalamento horizontal, terá de definir os dados da seguinte forma:

    
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
Para configurações de replicação do sistema HANA e escalável, repita esta configuração em cada um de nós. Esta medida certifica-se de que em casos de falha, as cópias de segurança e a replicação de armazenamento eventual continuarem a trabalhar.   

Depois de colocar todos os dados de configuração para o *HANABackupCustomerDetails.txt* ficheiro, verifique se as configurações estão corretas para os dados da instância HANA. Utilize o script `testHANAConnection.pl`, que é independente de uma configuração de vertical ou Escalamento horizontal de SAP HANA.

```
testHANAConnection.pl
```

Se tiver uma configuração de escalamento horizontal de SAP HANA, certifique-se de que a instância HANA mestre tem acesso a todos os servidores HANA e instâncias necessária. Existem não existem parâmetros para o script de teste, mas tem de adicionar os dados para o *HANABackupCustomerDetails.txt* ficheiro de configuração para o script a executar corretamente. Apenas os códigos de erro de comando shell são devolvidos, pelo que não é possível para o script para erro Verifique todas as instâncias. Ainda assim, o script comentários algumas úteis para que possa verificar.

Para executar o script, introduza o seguinte comando:
```
 ./testHANAConnection.pl
```
Se o script com êxito obtém o estado da instância HANA, é apresentada uma mensagem que a ligação de HANA teve êxito.


É o seguinte passo de teste para verificar a conectividade com o armazenamento com base nos dados colocados no *HANABackupCustomerDetails.txt* configuração do ficheiro e, em seguida, execute um instantâneo de teste. Antes de executar o `azure_hana_backup.pl` script, tem de executar este teste. Se um volume que contém não existem instantâneos, é impossível determinar se o volume está vazio ou se existir uma falha SSH para obter os detalhes de instantâneo. Por este motivo, o script executa dois passos:

- Verifica que a máquina virtual de armazenamento e interfaces do inquilino estão acessíveis para os scripts de instantâneos de execução.
- Cria um instantâneo de teste ou fictício, para cada volume por instância HANA.

Por este motivo, a instância HANA é incluída como um argumento. Se falhar a execução, não é possível fornecer o erro de verificação para a ligação de armazenamento. Mesmo se não houver nenhum erro de verificação, o script fornece sugestões úteis.

1. Execute a sequência de comandos para executar este teste:

   ```
   ssh <StorageUserName>@<StorageIP>
   ```

   O nome de utilizador de armazenamento e o endereço IP de armazenamento foram fornecidos para si no handover da unidade instância grande HANA.

2. Execute o script de teste:
   ```
    ./testStorageSnapshotConnection.pl
   ```

O script tenta iniciar sessão para o armazenamento utilizando a chave pública fornecida nos passos de configuração anteriores e com os dados configurados no *HANABackupCustomerDetails.txt* ficheiro. Se o início de sessão for bem-sucedida, é apresentado o seguinte conteúdo:

```
**********************Checking access to Storage**********************
Storage Access successful!!!!!!!!!!!!!!
```

Se ocorrerem problemas em ligar à consola do armazenamento, o resultado tem este aspeto:

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

Após um bem-sucedida início de sessão para as interfaces de máquina virtual de armazenamento, o script continua com fase 2 e cria um instantâneo de teste. O resultado é apresentado aqui para uma configuração de escalamento horizontal de três nós de SAP HANA:

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

Se o instantâneo de teste foi executado com êxito com o script, pode avançar com a configuração de instantâneos de armazenamento real. Se não for bem sucedida, investigue os problemas antes de prosseguir. O instantâneo de teste deve permanecer em torno até que os instantâneos reais primeiro terminados.


### <a name="step-7-perform-snapshots"></a>Passo 7: Efetuar instantâneos

Quando terminar os passos de preparação, pode começar a configurar a configuração de instantâneos de armazenamento real. O script a ser agendada funciona com configurações de vertical e horizontal de SAP HANA. Para a execução do script de segurança periódica e regular, agende o script utilizando o utilitário de cron. 

Pode criar três tipos de cópias de segurança do instantâneo:
- **HANA**: uma cópia de segurança do instantâneo combinado em que os volumes que contêm dados/hana/e hana/partilhados (que contém /usr/sap bem) estão abrangidas pelo instantâneo coordenado. Um restauro de ficheiro único é possível este instantâneo.
- **Os registos**: uma cópia de segurança do instantâneo do volume de logbackups/hana /. Nenhum instantâneo HANA é acionado para executar este instantâneo de armazenamento. Destina-se este volume de armazenamento para conter as cópias de segurança do registo de transações do SAP HANA. São efetuadas com mais frequência para restringir o crescimento do registo e de impedir a potencial perda de dados. Um restauro de ficheiro único é possível este instantâneo. Não reduzi a frequência em 3 minutos.
- **Arranque**: um instantâneo do volume que contém o número de unidade lógica de arranque (LUN) da instância de grande HANA. Esta cópia de segurança do instantâneo é possível apenas com o tipo I SKUs de HANA grande instâncias. Ficheiro único não é possível efetuar restauros do instantâneo do volume que contém o LUN de arranque.


>[!NOTE]
> A sintaxe de chamada para estes três tipos de instantâneos alterados com a mudança para os scripts de 3 versão, que suporta implementações de MCOD. Não é necessário especificar o SID de HANA de uma instância já. Tem de certificar-se de que as instâncias de SAP HANA de uma unidade são configuradas no ficheiro de configuração *HANABackupCustomerDetails.txt*.

>[!NOTE]
> Quando executar o script pela primeira vez, esta operação poderá mostrar alguns erros inesperados no ambiente de várias SID. Volte a executar o script corrige o problema.



A nova sintaxe de chamada de instantâneos de armazenamento com o script a executar *azure_hana_backup.pl* se parece com isto:

```
HANA backup covering /hana/data and /hana/shared (includes/usr/sap)
./azure_hana_backup.pl hana <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

For /hana/logbackups snapshot
./azure_hana_backup.pl logs <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

For snapshot of the volume storing the boot LUN
./azure_hana_backup.pl boot <HANA Large Instance Type> <snapshot_prefix> <snapshot_frequency> <number of snapshots retained>

```

Os detalhes dos parâmetros são os seguintes: 

- O primeiro parâmetro caracteriza o tipo de cópia de segurança do instantâneo. Os valores permitidos são **hana**, **registos**, e **arranque**. 
- O parâmetro **<HANA Large Instance Type>** é necessário para arranque volume apenas cópias de segurança. Existem dois valores válidos com "TypeI" ou "TypeII" dependentes na unidade de instância grande HANA. Para saber qual é o tipo da unidade, consulte [descrição geral de SAP HANA (instâncias de grandes dimensões) e arquitetura no Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture).  
- O parâmetro **< snapshot_prefix >** é um instantâneo ou uma etiqueta de cópia de segurança para o tipo de instantâneo. Tem duas finalidades: uma é que o utilizador atribuir-lhe um nome, para que saiba quais estes instantâneos são sobre. O objetivo da segundo destina-se o script *azure\_hana\_backup.pl* para determinar o número de instantâneos de armazenamento que são mantidos nessa etiqueta específica. Se agendar duas cópias de instantâneos de armazenamento do mesmo tipo (como **hana**), com duas etiquetas diferentes e definir que devem ser mantidos 30 instantâneos para cada uma, pode ficar com 60 instantâneos de armazenamento dos volumes afetados. Apenas alpha numérica ("A-Z, a-z, 0-9"), um caráter de sublinhado ("_") e travessão ("-") são permitidos carateres. 
- O parâmetro **< snapshot_frequency >** está reservado para futuros desenvolvimentos e não tem qualquer impacto. Defina-o para "3min" ao executar cópias de segurança do tipo **registo**e para "15 min." ao executar outros tipos de cópia de segurança.
- O parâmetro **<number of snapshots retained>** define a retenção instantâneos indiretamente ao definir o número de instantâneos com o mesmo prefixo de instantâneo (etiqueta). Este parâmetro é importante para execuções agendadas através de cron. Se o número de instantâneos com o mesmo snapshot_prefix excede o número especificado por este parâmetro, o instantâneo mais antigo é eliminado antes de executar um novo instantâneo de armazenamento.

No caso de escalamento horizontal, o script faz uma verificação adicional para se certificar de que podem aceder a todos os servidores HANA. O script também verifica todas as instâncias HANA devolvem o estado adequado das instâncias do, antes de que cria um instantâneo de SAP HANA. O instantâneo de SAP HANA é seguido de um instantâneo de armazenamento.

A execução do script `azure_hana_backup.pl` cria o armazenamento de instantâneos no seguinte três fases:

1. Executa um instantâneo de SAP HANA
2. Executa um instantâneo de armazenamento
3. Remove o instantâneo de SAP HANA que foi criado antes da execução do instantâneo de armazenamento

Ao executar o script, chamá-lo a pasta executável do HDB ao qual foi copiado. 

O período de retenção é administrado com o número de instantâneos são submetidos como um parâmetro ao executar o script. A quantidade de tempo que é abrangido pelos instantâneos de armazenamento é uma função do período de execução e o número de instantâneos submetida como um parâmetro ao executar o script. Se o número de instantâneos são mantidos excede o número que são designados como um parâmetro na chamada do script, o instantâneo mais antigo do armazenamento da etiqueta do mesmo é eliminado antes de um novo instantâneo é executado. O número está a conceder como o último parâmetro da chamada é o número pode utilizar para controlar o número de instantâneos são mantidos. Com este número, pode também controlar, indiretamente, o espaço em disco utilizado para instantâneos. 

> [!NOTE]
>Alterar a etiqueta, assim que a contagem começa novamente. Tem de ser strict no etiquetagem, pelo que os instantâneos não são eliminados acidentalmente.

### <a name="snapshot-strategies"></a>Estratégias de instantâneo
A frequência de instantâneos para os diferentes tipos depende se utilizar a funcionalidade de recuperação de desastre instância grande HANA. Esta funcionalidade baseia-se em instantâneos de armazenamento, o que poderão necessitar recomendações especiais para os períodos de frequência e a execução de instantâneos de armazenamento. 

As considerações e recomendações que se seguem, pressuposto é que proceda *não* utilizar a funcionalidade de recuperação de desastre instâncias grande HANA oferece. Em vez disso, pode utilizar os instantâneos de armazenamento para ter cópias de segurança e ser capaz de fornecer a recuperação do ponto no tempo nos últimos 30 dias. Tendo em conta as limitações do número de instantâneos e o espaço, os clientes têm considera os seguintes requisitos:

- O tempo de recuperação para a recuperação do ponto no tempo.
- O espaço utilizado.
- O ponto de recuperação e os objetivos de tempo de recuperação para potencial recuperação de desastres.
- A execução de uma eventual de cópias de segurança de base de dados completo HANA contra discos. Sempre que uma cópia de segurança da base de dados completo contra discos ou **backint** interface é executada, a execução de instantâneos de armazenamento falha. Se planear executar cópias de segurança de base de dados completo por cima de instantâneos de armazenamento, certifique-se de que a execução de instantâneos de armazenamento está desativada durante este período.
- O número de instantâneos por volume (limitado a 250).


Para os clientes que não utilizem a funcionalidade de recuperação após desastre de instâncias de grande HANA, o período de instantâneo é menos frequente. Nestes casos, os clientes executar os instantâneos combinados nos /hana/data e /hana/shared (inclui /usr/sap) em períodos de hora 12 ou 24 horas e mantenha os instantâneos durante um mês. O mesmo acontece com os instantâneos de volume de cópia de segurança de registo. No entanto, a execução de cópias de registo de transação de SAP HANA contra o volume de cópia de segurança do registo de ocorre em 5 minutos para períodos de 15 minutos.

Instantâneos de armazenamento agendada melhor são executados através da utilização de cron. Utilizar o mesmo script para todas as cópias de segurança e desastre e, se modificar as entradas de script para corresponder a vários pedido tempos de cópia de segurança. Estes instantâneos são todos agendados forma diferente no cron, dependendo do seu tempo de execução: horária, 12 horas, diária ou semanal. 

Segue-se um exemplo de uma agenda de cron no etc/crontab:
```
00 1-23 * * * ./azure_hana_backup.pl hana hourlyhana 15min 46
10 00 * * *  ./azure_hana_backup.pl hana dailyhana 15min 28
00,05,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
22 12 * * *  ./azure_hana_backup.pl log dailylogback 3min 28
30 00 * * *  ./azure_hana_backup.pl boot TypeI dailyboot 15min 28
```
No exemplo anterior, é um instantâneo combinado de hora a hora que abrange os volumes que contêm dados/hana/e /hana/shared (inclui/usr/sap) localizações. Utilize este tipo de instantâneos para uma recuperação do ponto no tempo mais rápida nos últimos dois dias. Além disso, não há um instantâneo diário nesses volumes. Por isso, tem dois dias de cobertura por instantâneos horárias, mais de quatro semanas de cobertura por instantâneos diários. Além disso, o volume de cópia de segurança de registo de transação de cópias de segurança diária. Estas cópias de segurança são mantidas durante quatro semanas bem. Como ver na terceira linha de crontab, a cópia de segurança do registo de transação de HANA é agendada para execução a cada 5 minutos. Horas de início das tarefas de cron diferente que execute instantâneos de armazenamento são escalonadas, para que os instantâneos não são executados ao mesmo tempo num determinado ponto no tempo. 

No exemplo seguinte, efetuar um instantâneo combinado que abrange os volumes que contêm as/hana/dados hana/partilhados (incluindo/usr/sap) localizações e numa base horária. Manter estes instantâneos para dois dias. Os instantâneos dos volumes de cópia de segurança de registo de transação são executados numa base de 5 minutos e são mantidos durante 4 horas. Como anteriormente, a cópia de segurança do ficheiro de registo de transações HANA é agendada para execução a cada 5 minutos. O instantâneo do volume de cópia de segurança do registo de transações é executado com um atraso de 2 minutos, depois da cópia de segurança do registo de transação foi iniciado. Esses 2 minutos, a cópia de segurança do registo de transações do SAP HANA deve ser concluída em circunstâncias normais. Como anteriormente, o volume que contém o arranque LUN é feito uma vez por dia por um instantâneo de armazenamento e é mantido por quatro semanas.

```
10 0-23 * * * ./azure_hana_backup.pl hana hourlyhana 15min 48
0,5,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup.pl log logback 3min 48
30 00 * * *  ./azure_hana_backup.pl boot TypeII dailyboot 15min 28
```

O gráfico seguinte ilustra as sequências de tarefas do exemplo anterior, excluindo o LUN de arranque:

![Relação entre as cópias de segurança e instantâneos](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot_updated0921.PNG)

SAP HANA efetua escritas regulares contra o volume de /hana/log documentar as alterações consolidadas na base de dados. Regularmente, SAP HANA escreve um ponto de reposição de volume /hana/data. Conforme especificado no crontab, uma cópia de segurança do registo de transações do SAP HANA é executada a cada 5 minutos. Também pode ver que um instantâneo de SAP HANA é executado devido a acionar um instantâneo de armazenamento combinado sobre os volumes /hana/data e /hana/shared a cada hora. Depois do instantâneo HANA for bem sucedida, o instantâneo de armazenamento combinado é executado. As instruções na crontab, o instantâneo de armazenamento no /hana/logbackup volume é executado de cada 5 minutos, cerca de 2 minutos após a cópia de segurança do registo de transações HANA.

> 

>[!IMPORTANT]
> A utilização de instantâneos de armazenamento para cópias de segurança de SAP HANA é útil apenas quando os instantâneos são efetuados em conjunto com cópias de segurança de registo de transação de SAP HANA. Estas cópias de segurança do registo de transação tem de forma a cobrir os períodos de tempo entre os instantâneos de armazenamento. 

Se tiver configurado um compromisso aos utilizadores de uma recuperação do ponto no tempo de 30 dias, tem de:

- Em casos extremos, aceder a um armazenamento combinado de instantâneos por /hana/data e /hana/shared é 30 dias.
- Ter cópias de segurança do registo transação contíguo que abrangem o tempo entre a quaisquer instantâneos combinado de armazenamento. Por isso, o instantâneo mais antigo do volume de cópia de segurança do registo de transação tem de ser 30 dias. Não é o caso se copiar as cópias de segurança do registo de transações para outra partilha NFS que está localizada no armazenamento do Azure. Nesse caso, poderá solicitar transação registo cópias de segurança antigas do que a partilha NFS.

Para beneficiar de instantâneos de armazenamento e a replicação de armazenamento eventual de cópias de segurança de registo de transações, terá de alterar a localização para os quais o SAP HANA escreve as cópias de segurança do registo de transações. Pode efetuar esta alteração no HANA Studio. Apesar de SAP HANA efetua cópias de segurança segmentos de registo completo automaticamente, deve especificar um intervalo de cópia de segurança do registo ser determinística. Isto é especialmente verdadeiro quando utilizar a opção de recuperação após desastre, porque, normalmente, pretende executar cópias de segurança de registos com um período de determinista. No caso de seguinte, 15 minutos estão definidos como o intervalo de cópia de segurança do registo.

![Agendar cópia de segurança de SAP HANA regista no SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

Também pode escolher as cópias de segurança que são mais frequentes do que 15 em 15 minutos. Uma definição mais frequente, muitas vezes, é utilizada em conjunto com a funcionalidade de recuperação após desastre de instâncias de grande HANA. Alguns clientes efetuar cópias de segurança de registo de transações a cada 5 minutos.  

Se a base de dados nunca foi efetuada cópia de, é o último passo efetuar uma cópia de segurança da base de dados baseada em ficheiros para criar uma única entrada de cópia de segurança que tem de existir no catálogo de cópias de segurança. Caso contrário, SAP HANA não é possível iniciar as cópias de segurança de registo especificado.

![Faça uma cópia de segurança baseada em ficheiros para criar uma única entrada de cópia de segurança](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


Depois de tem foi executados o seu primeiro instantâneos de armazenamento com êxito, pode eliminar o instantâneo de teste foi executado no passo 6. Para tal, execute o script `removeTestStorageSnapshot.pl`:
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


### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>O número e tamanho de instantâneos no volume de disco de monitorização

Num volume de armazenamento, pode monitorizar o número de instantâneos e o consumo de armazenamento dos instantâneos. O `ls` comandos não mostram o directório de instantâneo ou os ficheiros. No entanto, o comando de SO Linux `du` mostra detalhes sobre os instantâneos de armazenamento, porque são armazenados nos volumes a mesmos. O comando pode ser utilizado com as seguintes opções:

- `du –sh .snapshot`: Esta opção fornece um total de todos os instantâneos dentro do diretório de instantâneo.
- `du –sh --max-depth=1`: Esta opção apresenta uma lista de todos os instantâneos são guardados no **.snapshot** pasta e o tamanho de cada instantâneo.
- `du –hc`: Esta opção fornece o tamanho total utilizado por todos os instantâneos.

Utilize estes comandos para se certificar de que os instantâneos que são executados e armazenados não estão a consumir todas as armazenamento nos volumes.

>[!NOTE]
>Os instantâneos do LUN de arranque não estão visíveis com os comandos anteriores.

### <a name="getting-details-of-snapshots"></a>A obter os detalhes de instantâneos
Para obter mais detalhes em instantâneos, também pode utilizar o script `azure_hana_snapshot_details.pl`. Este script pode ser executado em qualquer localização, se existir um servidor ativo na localização de recuperação após desastre. O script fornece o seguinte resultado, discriminado segundo cada volume que contém os instantâneos: 
   * O tamanho de instantâneos totais um volume
   * Os detalhes seguintes em cada instantâneo nesse volume: 
      - Nome de instantâneo 
      - Hora de criação 
      - Tamanho do instantâneo
      - Frequência de instantâneo
      - ID de cópia de segurança de HANA associado nesse instantâneo, se forem relevantes

Segue-se um exemplo da sintaxe de execução do script:

```
./azure_hana_snapshot_details.pl 
```

Porque o script tenta obter o ID de cópia de segurança HANA, tem de ligar à instância do SAP HANA. Esta ligação requer que o ficheiro de configuração *HANABackupCustomerDetails.txt* estar configurada corretamente. Uma saída de dois instantâneos num volume poderá ter o seguinte aspeto:

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


### <a name="file-level-restore-from-a-storage-snapshot"></a>Restauro ao nível dos ficheiros de um instantâneo de armazenamento
Para os tipos de instantâneo **hana** e **registos**, pode aceder aos instantâneos diretamente nos volumes no **.snapshot** diretório. Não há um subdiretório para cada um dos instantâneos. Pode copiar cada ficheiro no Estado em que estava at the point of o instantâneo do que subdiretório para a estrutura de diretórios real. Na versão atual do script, há **não** restaurar script fornecido para o restauro de instantâneo como um Self-Service (embora o restauro de instantâneos pode ser efetuado como parte da recuperação self-service de scripts no site de DR durante a ativação pós-falha). Tem de contactar a equipa de operações do Microsoft ao abrir um pedido de serviço para restaurar um instantâneo pretendido a partir de instantâneos existentes disponíveis.

>[!NOTE]
>Ficheiro único restauro não funciona para instantâneos do arranque LUN independentemente do tipo de unidades de instância grande HANA. O **.snapshot** diretórios não está exposto no LUN de arranque. 


### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Reduzir o número de instantâneos, num servidor

Tal como explicado anteriormente, pode reduzir o número de determinados etiquetas de instantâneos que armazena. Os últimos dois parâmetros do comando para iniciar um instantâneo são a etiqueta e o número de instantâneos que pretenda manter.

```
./azure_hana_backup.pl hana dailyhana 15min 28
```

No exemplo anterior, a etiqueta de instantâneo é **dailyhana** e é o número de instantâneos com esta etiqueta deve ser retida **28**. Como responder para consumo de espaço em disco, pode querer reduzir o número de instantâneos armazenados. É a forma mais fácil de reduzir o número de instantâneos para 15, por exemplo, para executar o script com o último parâmetro definido como **15**:

```
./azure_hana_backup.pl hana dailyhana 15min 15
```

Se executar o script com esta definição, o número de instantâneos, incluindo o instantâneo de armazenamento novo, é 15. 15 mais recentes instantâneos são mantidos e os 15 instantâneos mais antigos são eliminados.

 >[!NOTE]
 > Este script reduz o número de instantâneos só se existirem instantâneos mais do que 1 horas depois. O script não eliminar instantâneos são inferior a 1 horas depois. Estas restrições relacionadas com a funcionalidade de recuperação de desastre opcional fornecida.

Se já não pretender manter um conjunto de instantâneos com a etiqueta de cópia de segurança **hanadaily** nos exemplos de sintaxe, pode executar o script com **0** como o número de retenção. Essa etiqueta de correspondência de todos os instantâneos, em seguida, são removidos. No entanto, a remoção de todos os instantâneos pode afetar as capacidades de funcionalidade de recuperação após desastre de instâncias de grande HANA.

Uma segunda opção para eliminar instantâneos específicos consiste em utilizar o script `azure_hana_snapshot_delete.pl`. Este script foi concebido para eliminar um instantâneo ou um conjunto de instantâneos ou ao utilizar o ID de cópia de segurança HANA como encontrada no HANA Studio ou através do nome de instantâneos em si. Atualmente, o ID de cópia de segurança só está associado aos instantâneos criados para o **hana** tipo de instantâneos. As cópias de segurança do tipo de instantâneos **registos** e **arranque** não efetuar um instantâneo de SAP HANA e, por isso, não existe nenhum ID de cópia de segurança para localizar os instantâneos. Se o nome de instantâneo é introduzido, procura todos os instantâneos em volumes diferentes que corresponde ao nome do instantâneo introduzido. 

Chame o script que tem de especificar o SID da instância HANA utilizando a sintaxe de chamada do script:

```
./azure_hana_snapshot_delete.pl <SID>

```

Execute o script como utilizador **raiz**.

Se selecionar um instantâneo, pode eliminar cada instantâneo individualmente. Primeiro a fornecer o volume que contém o instantâneo e, em seguida, forneça o nome do instantâneo. Se o instantâneo existe esse volume e é mais do que 1 hora antigo, é eliminado. Pode encontrar os nomes de volume e nomes de instantâneo executando o `azure_hana_snapshot_details` script. 

>[!IMPORTANT]
>Se não houver dados que só existe no instantâneo serão eliminados, após o instantâneo é eliminado, que dados são perdidos para sempre.

   

### <a name="recover-to-the-most-recent-hana-snapshot"></a>Recuperar para o instantâneo HANA mais recente

Num cenário de produção pendente, o processo de recuperação a partir de um instantâneo de armazenamento pode ser iniciado como um incidente de cliente com o suporte do Microsoft Azure. É um fim de urgência alta se os dados foram eliminados num sistema de produção e é a única forma para obtê-lo para restaurar a base de dados de produção.

Numa situação diferentes, uma ponto no tempo recuperação poderá ser urgência baixa e planeados com antecedência dias. Pode planear a recuperação com SAP HANA na gestão de serviço do Azure em vez de gerar um sinalizador de alta prioridade. Por exemplo, poderá ser planeamento atualizar o software SAP através da aplicação de um novo pacote de melhoramento. Em seguida, terá de reverter para um instantâneo que representa o estado antes da atualização do pacote de melhoramento.

Antes de enviar o pedido, terá de preparar. O SAP HANA na equipa de gestão de serviço do Azure, em seguida, pode processar o pedido e fornecer os volumes restaurados. Posteriormente, restaure a base de dados HANA com base nos instantâneos. 

O seguinte mostra como preparar o pedido:

>[!NOTE]
>A interface de utilizador pode variar das capturas de ecrã seguintes, consoante a versão de SAP HANA que está a utilizar.

1. Decida qual instantâneo para restaurar. Apenas o volume de dados/hana é restaurado a menos que instruir; caso contrário. 

2. Encerre a instância HANA.

 ![Encerre a instância HANA](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

3. Desmonte os volumes de dados em cada nó de base de dados HANA. Se os volumes de dados ainda são instalados no sistema operativo, o restauro do instantâneo falha.
 ![Desmontar os volumes de dados em cada nó de base de dados HANA](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

4. Abra um pedido de suporte do Azure e inclui instruções sobre o restauro de um instantâneo específico.

 - Durante o restauro: SAP HANA na gestão do serviço Azure poderá pedir que aguardem uma chamada de conferência para Certifique-se coordenação, a verificação e a confirmação de que o instantâneo de armazenamento correta é restaurado. 

 - Após o restauro: SAP HANA na gestão de serviço do Azure notifica-o quando o instantâneo de armazenamento ter sido restaurado.

5. Depois do processo de restauro está concluído, remontar todos os volumes de dados.

 ![Remontar todos os volumes de dados](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

6. Selecione as opções de recuperação SAP HANA Studio, se não automaticamente entram cópias de segurança quando voltar a ligar à base de dados de HANA através de SAP HANA Studio. O exemplo seguinte mostra um restauro para o último instantâneo HANA. Um instantâneo de armazenamento o atacante incorpora um instantâneo HANA. Se restaurar o instantâneo de armazenamento mais recente, deve ser o instantâneo HANA mais recente. (Se restaurar um instantâneo mais antigo do armazenamento, tem de localizar o instantâneo HANA com base na hora que o armazenamento de instantâneo foi tirado.)

 ![Selecione as opções de recuperação SAP HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

7. Selecione **recuperar a base de dados para um instantâneo de cópia de segurança ou de armazenamento de dados específicos**.

 ![A janela de especificar o tipo de recuperação](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

8. Selecione **especificar cópia de segurança sem catálogo**.

 ![A janela de especificar localização de cópia de segurança](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

9. No **tipo de destino** lista, selecione **instantâneo**.

 ![A especificar cópia de segurança a janela de recuperação](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

10. Selecione **concluir** para iniciar o processo de recuperação.

 ![Selecione "Concluir" para iniciar o processo de recuperação](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

11. A base de dados HANA é restaurada e recuperada para o instantâneo HANA que está incluído no instantâneo de armazenamento.

 ![Base de dados HANA é restaurada e recuperada para o instantâneo HANA](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recover-to-the-most-recent-state"></a>Recuperar para o estado mais recente

O seguinte processo restaura o instantâneo HANA que está incluído no instantâneo de armazenamento. Em seguida, restaura as cópias de segurança do registo de transação para o estado mais recente da base de dados antes de restaurar o instantâneo de armazenamento.

>[!IMPORTANT]
>Antes de continuar, certifique-se de que tem uma cadeia completa e contígua de cópias de segurança de registo de transações. Sem estas cópias de segurança, não é possível restaurar o estado atual da base de dados.

1. Conclua os passos 1-6 em [recuperar para o instantâneo HANA mais recente](#recovering-to-the-most-recent-hana-snapshot).

2. Selecione **recuperar a base de dados para o estado mais recente**.

 ![Selecionar "Recuperar a base de dados para o estado mais recente"](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

3. Especifique a localização das cópias de segurança do registo HANA mais recentes. A localização tem de conter todas HANA transação registo cópias de segurança do instantâneo HANA para o estado mais recente.

 ![Especifique a localização das cópias de segurança do registo HANA mais recentes](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

4. Selecione uma cópia de segurança como uma base a partir do qual recuperar a base de dados. Neste exemplo, o instantâneo HANA na captura de ecrã é o instantâneo HANA foi incluído no instantâneo de armazenamento. 

 ![Selecione uma cópia de segurança como uma base a partir do qual recuperar a base de dados](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

5. Limpar o **cópias de segurança do utilize Delta** caixa de verificação se não existem diferenças entre a hora do instantâneo HANA e o estado mais recente.

 ![Desmarque a caixa de verificação "Utilização Delta cópias de segurança", não se existem nenhum deltas](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

6. No ecrã de resumo, selecione **concluir** para iniciar o procedimento de restauro.

 ![Clique em "Concluir" no ecrã de resumo](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recover-to-another-point-in-time"></a>Recuperar para um outro ponto no tempo
Recuperar para um ponto no tempo entre o instantâneo HANA (incluído no instantâneo armazenamento) e outro que é posterior à recuperação de ponto no tempo de instantâneo HANA, execute os seguintes passos:

1. Certifique-se de que tem todas as transações registo cópias de segurança do instantâneo HANA durante o período de tempo que pretende recuperar.
2. Iniciar o procedimento em [recuperar para o estado mais recente](#recovering-to-the-most-recent-state).
3. No passo 2 do procedimento, além de **especificar tipo de recuperação** janela, selecione **recuperar a base de dados para o seguinte ponto no tempo**e, em seguida, especifique o ponto no tempo. 
4. Execute os passos 3 a 6.

### <a name="monitor-the-execution-of-snapshots"></a>Monitorizar a execução de instantâneos

Como utilizar instantâneos de armazenamento de instâncias de grande HANA, também terá de monitorizar a execução do qual estes instantâneos. O script que executa um instantâneo de armazenamento escreve saída para um ficheiro e, em seguida, guarda-a mesma localização que os scripts de Perl. É escrito um ficheiro separado para cada instantâneo de armazenamento. O resultado de cada ficheiro mostra as várias fases que executa o script do instantâneo:

1. Localiza os volumes que tem de criar um instantâneo.
2. Localiza os instantâneos obtidos a partir destes volumes.
3. Elimina o eventual instantâneos existentes para corresponder ao número de instantâneos que especificou.
4. Cria um instantâneo de SAP HANA.
5. Cria o instantâneo de armazenamento de volumes.
6. Elimina o instantâneo de SAP HANA.
7. Muda o nome o instantâneo da mais recente **.0**.

A parte mais importante do ficheiro cab do script identificado é esta peça:
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
Pode ver deste exemplo como o script regista a criação do instantâneo HANA. No caso de escalamento horizontal, este processo é iniciado no nó principal. O nó mestre inicia a criação de instantâneos de SAP HANA em cada um de nós de trabalho síncrona. Em seguida, se obtém o instantâneo de armazenamento. Após a execução bem-sucedida de instantâneos de armazenamento, o instantâneo HANA é eliminado. A eliminação do instantâneo HANA é iniciada a partir do nó principal.


## <a name="disaster-recovery-principles"></a>Princípios de recuperação de desastre
Instâncias de grande HANA oferecem uma funcionalidade de recuperação após desastre entre carimbos de data / instância grande HANA em diferentes regiões do Azure. Por exemplo, se implementar unidades de instância grande HANA na região dos EUA oeste do Azure, pode utilizar as unidades de instância grande HANA na região dos EUA Leste como unidades de recuperação após desastre. Conforme mencionado anteriormente, recuperação após desastre não está configurada automaticamente, porque é necessária a pagar para outra unidade de instância grande HANA na região DR. A configuração de recuperação após desastre funciona para setups vertical, bem como de escalamento horizontal. 

Nos cenários de implementação até ao momento, os clientes utilizam a unidade na região DR para executar os sistemas de não produção que utilizam uma instância HANA instalada. A unidade de instância grande HANA tem de ser do mesmo SKU como o SKU utilizado para fins de produção. A imagem seguinte mostra que a configuração de disco entre a unidade do servidor na região do Azure de produção e a região de recuperação após desastre aspeto:

![Configuração do programa de configuração de DR do disco ponto de vista](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_setup.PNG)

Conforme apresentado no gráfico descrição geral, em seguida, terá de ordenar segundo conjunto de volumes de disco. Os volumes de disco de destino são o mesmo tamanho que os volumes de produção para a instância de produção em unidades de recuperação após desastre. Estes volumes de disco estão associadas a unidade de instância grande HANA de servidor no site de recuperação após desastre. Os volumes seguintes são replicados a partir da região de produção para o site de DR:

- hana/dados
- hana/logbackups 
- /Hana/Shared (inclui/usr/sap)

O volume de /hana/log não é replicado porque não é necessário o registo de transações de SAP HANA da forma que é efetuado o restauro nesses volumes. 

A base da funcionalidade de recuperação após desastre oferecidos é a funcionalidade de replicação de armazenamento fornecida pela infraestrutura instância grande HANA. A funcionalidade que é utilizada no lado de armazenamento não é um fluxo constante de alterações que são replicados de forma assíncrona, como a alteração efetuada para o volume de armazenamento. Em vez disso, é um mecanismo que depende do facto que são criar instantâneos destes volumes regularmente. As diferenças entre um instantâneo já replicado e um novo instantâneo que ainda não foi replicado, em seguida, é transferida para o site de recuperação após desastre para volumes de disco de destino.  Estes instantâneos são armazenados nos volumes e, no caso de uma ativação pós-falha recuperação após desastre, têm de ser restaurado nesses volumes.  

A transferência dos dados do volume completos primeiro deve ter antes da quantidade de dados fica mais pequena do que as diferenças entre instantâneos. Como resultado, os volumes no site de DR contenham todos os instantâneos de volume efetuada no site de produção. Eventualmente, pode utilizar esse sistema DR para obter um estado anterior para recuperar dados perdidos, sem reverter o sistema de produção.

No caso de MCOD as implementações com várias instâncias de SAP HANA independentes numa unidade de instância grande HANA, espera-se que todas as instâncias de SAP HANA são obter armazenamento replicado para o lado de DR.

Nos casos em que utilizam a replicação do sistema de HANA como a funcionalidade de elevada disponibilidade no seu site de produção e utilizar replicação de armazenamento para o site de DR, os volumes de ambos os nós de site primário para a instância de DR são replicados. Tem de comprar armazenamento adicional (tamanho mesmo a partir do nó principal) no site de DR para acomodar a replicação primária e secundária para a DR. 



>[!NOTE]
>A funcionalidade de replicação de armazenamento de instância grande HANA é espelhamento e replicar instantâneos de armazenamento. Se não efetuar instantâneos de armazenamento, tal como apresentado no [cópia de segurança e restauro](#backup-and-restore) secção deste artigo, não pode existir qualquer replicação para o site de recuperação após desastre. Execução de instantâneos de armazenamento é um pré-requisito para a replicação de armazenamento para o site de recuperação após desastre.



## <a name="preparation-of-the-disaster-recovery-scenario"></a>Preparação do cenário de recuperação após desastre
Neste cenário, tem um sistema de produção em execução no instâncias grande HANA numa região do Azure de produção. Para obter os passos que se seguem, vamos assumir que o SID do sistema que HANA é "PRD" e de que tem um sistema de não produção em execução em instâncias de grande HANA na região do Azure de DR. Para esta última opção, vamos assumir que o respetivo SID é "TST." A imagem seguinte mostra esta configuração:

![Início do programa de configuração de DR](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start1.PNG)

Se a instância de servidor já não tem sido ordenados com o conjunto de volume de armazenamento adicional, SAP HANA na gestão de serviço do Azure anexa o conjunto de volumes adicionais como um destino para a réplica de produção para a unidade de instância grande HANA no qual está a executar o TST Instância HANA. Para essa finalidade, tem de fornecer o SID da sua instância HANA de produção. Depois de SAP HANA na gestão de serviço do Azure confirma o anexo desses volumes, terá de montar os volumes para a unidade de instância grande HANA.

![Passo seguinte do programa de configuração DR](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start2.PNG)

O passo seguinte é para que instale a segunda instância de SAP HANA na unidade instância grande HANA na região do Azure de DR, em que executa a instância de TST HANA. A instância de SAP HANA recentemente instalada tem de ter o mesmo SID. Os utilizadores criou tem de ter o mesmo UID e ID de grupo que tenha da instância de produção. Se a instalação foi concluída com êxito, tem de:

- Execute o passo 2 da preparação de instantâneos de armazenamento abordado neste artigo.
- Crie uma chave pública para a unidade de DR da unidade de instância grande HANA se ainda não o ainda feito. Consulte o passo 3 da preparação de instantâneos de armazenamento abordado neste artigo.
- Manter o *HANABackupCustomerDetails.txt* com a nova instância HANA e teste se conectividade para o armazenamento funciona corretamente.  
- Pare a instância de SAP HANA recentemente instalada na unidade instância grande HANA na região do Azure de DR.
- Desmonte estes volumes PRD e contacte o SAP HANA na gestão de serviço do Azure. Os volumes de mensagens em fila não é possível permanecem montados para a unidade porque não pode estar acessíveis à funcionar como destino de replicação de armazenamento.  

![Passo de configuração de DR antes de estabelecer a replicação](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start3.PNG)

A equipa de operações estabelece a relação de replicação entre os volumes PRD na região do Azure de produção e os volumes PRD na região do Azure de DR.

>[!IMPORTANT]
>O volume de /hana/log não é replicado porque não é necessário restaurar a base de dados SAP HANA replicada num estado consistente no site de recuperação após desastre.

Em seguida, configure ou ajustar a agenda de cópia de segurança de instantâneos de armazenamento para aceder ao seu RTO e RPO no caso de desastre. Para minimizar o objetivo de ponto de recuperação, defina os seguintes intervalos de replicação no serviço de instância grande HANA:
- Para os volumes abrangidos pelo instantâneo combinado (instantâneo tipo **hana**), conjunto para replicar a cada 15 minutos para os destinos de volume de armazenamento equivalente no site de recuperação após desastre.
- Para o volume de cópia de segurança de registo de transações (instantâneo tipo **registos**), conjunto para replicar a cada 3 minutos para os destinos de volume de armazenamento equivalente no site de recuperação após desastre.

Para minimizar o objetivo de ponto de recuperação, configure o seguinte:
- Efetuar uma **hana** instantâneos de armazenamento de tipo (consulte "passo 7: efetuar instantâneos") a cada 30 minutos para 1 hora.
- Fazer cópias de segurança de registo de transação de SAP HANA a cada 5 minutos.
- Efetuar uma **registos** escreva a cada 5-15 minutos de instantâneos de armazenamento. Com este período de intervalo, o que poderá alcançar um RPO de cerca de 25 de 15 minutos.

Com esta configuração, a sequência de cópias de segurança de registo de transações, os instantâneos de armazenamento e a replicação da transação HANA é necessário um volume e hana/dados de cópia de segurança de registo e /hana/shared (inclui/usr/sap) aspeto que poderá ter os dados apresentados neste gráfico:

 ![Relação entre um instantâneo de cópia de segurança de registo de transações e um espelho snap num eixo de tempo](./media/hana-overview-high-availability-disaster-recovery/snapmirror.PNG)

Para alcançar um RPO melhor mesmo no caso de recuperação após desastre, pode copiar as cópias de segurança do registo de transações HANA de SAP HANA no Azure (instâncias de grande) para outro região do Azure. Para alcançar esta redução RPO adicional, execute os seguintes passos:

1. Cópia de segurança a transação HANA inicie sessão com uma frequência como possível /hana/logbackups.
2. Utilize rsync para copiar as cópias de segurança do registo de transação para as NFS alojadas em partilha de máquinas virtuais do Azure. As VMs estão em redes virtuais do Azure na região do Azure de produção e nas regiões de DR. Terá de ligar ambas as redes virtuais do Azure para o circuito ligar a instâncias de grande HANA de produção para o Azure. Ver gráficos no [rede considerações para a recuperação de desastre com instâncias de grande HANA](#Network-considerations-for-disaster recovery-with-HANA-Large-Instances) secção. 
3. Manter que as cópias de segurança de registo de transação na região na VM ligadas para o NFS exportados armazenamento.
4. No caso de desastre de ativação pós-falha, complementar as cópias de segurança de registo transação que encontrará no /hana/logbackups volume com mais recentemente executadas cópias de segurança de registo de transação no NFS partilham no site de recuperação após desastre. 
5. Inicie uma cópia de segurança do registo de transações para restaurar a cópia de segurança mais recente que poderão estar guardada através da região DR.

Quando as operações de instância grande HANA confirme a configuração de relação de replicação e iniciar as cópias de segurança de instantâneos de armazenamento execução, começa a replicação de dados.

![Passo de configuração de DR antes de estabelecer a replicação](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start4.PNG)

Avança ser a replicação, os instantâneos nos volumes PRD em regiões do Azure de DR não está a ser restaurado. Apenas são armazenados. Se os volumes são instalados nesse Estado, estes representam o estado em que a desmontar os volumes depois da instância de PRD SAP HANA foi instalada na unidade do servidor na região do Azure de DR. Eles também representam as cópias de segurança de armazenamento que ainda não foram restauradas.

Em caso de uma ativação pós-falha, também pode escolher restaurar um instantâneo mais antigo do armazenamento em vez do instantâneo de armazenamento mais recente.

## <a name="disaster-recovery-failover-procedure"></a>Procedimento de ativação pós-falha de recuperação de desastre
Existem dois cenários a considerar quando efetuar a ativação pós-falha para o site de DR:

- Terá da base de dados SAP HANA para voltar para o estado mais recente dos dados. Neste caso, é um script personalizado com o que pode efetuar a ativação pós-falha sem a necessidade de contactar a Microsoft. No entanto, a reativação pós-falha, terá de trabalhar com a Microsoft.
- Pretende restaurar para um instantâneo de armazenamento que não seja o mais recente instantâneo replicado. Neste caso, terá de trabalhar com a Microsoft. 

>[!NOTE]
>Os seguintes passos tem de ser executado na unidade instância grande HANA, que representa a unidade de DR. 
 
Para restaurar os instantâneos de armazenamento replicado mais recentes, execute os seguintes passos: 

1. Encerre a instância de não produção do HANA na unidade de recuperação após desastre de instâncias de grande HANA que estiver a executar. Isto acontece porque é uma instância de produção HANA dormant pré-instaladas.
2. Certifique-se de que não existem processos de SAP HANA estão em execução. Utilize o seguinte comando para esta verificação: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. O resultado deve mostrar o **hdbdaemon** processos no estado parado e outros processos HANA num Estado em execução ou foi iniciado.
3. Na unidade de instância grande HANA do site de DR, execute o script *azure_hana_dr_failover.pl*. O script é solicitando um SID de HANA SAP para serem restaurados. Quando solicitado, escreva um ou o único SAP HANA SID que foram replicadas e é mantido no *HANABackupCustomerDetails.txt* ficheiro na unidade instância grande HANA no site de DR. 

      Se pretender ter múltiplas instâncias de SAP HANA a ativação pós-falha, terá de executar o script várias vezes. Quando solicitado, escreva SAP HANA SID que pretende efetuar a ativação pós-falha e restaurar. Após a conclusão, o script mostra uma lista de pontos de montagem dos volumes que são adicionados à unidade de instância grande HANA. Esta lista inclui os volumes de DR restaurados.

4. Monte os volumes de recuperação após desastre restaurada utilizando comandos de sistema operativo Linux para a unidade de instância grande HANA no site de recuperação após desastre. 
6. Inicie a instância de produção dormant SAP HANA.
7. Se optar por copiar registos de cópia de segurança de registo de transações para reduzir o tempo RPO, tem de unir as cópias de segurança do registo de transação para o diretório de logbackups recentemente montado DR hana /. Não substitua cópias de segurança existentes. Copie as cópias de segurança mais recentes que não foram replicadas com a replicação mais recente de um instantâneo de armazenamento.
8. Também pode restaurar ficheiros único os instantâneos de tem sido replicados para o volume de /hana/shared/PRD na região do Azure de DR. 

Pode testar a ativação pós-falha de DR, bem como sem afetar a relação de replicação real. Para efetuar uma ativação pós-falha de teste, siga os passos anteriores 1 e 2 e, em seguida, prossiga com o seguinte passo 3.

>[!IMPORTANT]
>Efetue *não* executar quaisquer transações de produção na instância que criou no site de DR durante o processo de **testar uma ativação pós-falha** com o script introduzido no passo 3. Este comando cria um conjunto de volumes que não tenham nenhuma relação para o site primário. Como resultado, a sincronização para o site primário é *não* possíveis. 

Passo 3 para a ativação pós-falha de teste:

Na unidade de instância grande HANA do site de DR, execute o script **azure_hana_test_dr_failover.pl**. Este script é *não* parar a relação de replicação entre o site primário e o site de DR. Em vez disso, este script é clonagem de volumes de armazenamento de DR. Depois do processo de clonagem é concluída com êxito, os volumes clonados são restaurados para o estado do instantâneo mais recente e, em seguida, montados para a unidade de DR. O script é solicitando um SID de HANA SAP para serem restaurados. Escreva um ou o único SAP HANA SID que foram replicadas e é mantido no *HANABackupCustomerDetails.txt* ficheiro na unidade instância grande HANA no site de DR. 

Se pretender ter múltiplas instâncias de SAP HANA para testar, terá de executar o script várias vezes. Quando solicitado, escreva SAP HANA SID da instância que pretende testar para ativação pós-falha. Após a conclusão, o script mostra uma lista de pontos de montagem dos volumes que são adicionados à unidade de instância grande HANA. Esta lista inclui os volumes de DR clonados.

Continue para o passo 4.

   >[!NOTE]
   >Se precisar de efetuar a ativação pós-falha para o site de DR para rescue alguns dados que foi eliminados há horas e tem dos volumes de DR para ser definido como um instantâneo anterior, este procedimento aplica-se. 

4. Encerre a instância de não produção do HANA na unidade de recuperação após desastre de instâncias de grande HANA que estiver a executar. Isto acontece porque é uma instância de produção HANA dormant pré-instaladas.
5. Certifique-se de que não existem processos de SAP HANA estão em execução. Utilize o seguinte comando para esta verificação: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. O resultado deve mostrar o **hdbdaemon** processos num estado parado e outros processos HANA num Estado em execução ou foi iniciado.
6. Determine para que o nome de instantâneo ou o ID de cópia de segurança de SAP HANA pretende que o site de recuperação após desastre restaurado. Em casos de recuperação de desastre real, este instantâneo é, normalmente, o instantâneo mais recente. Se precisar de recuperar dados perdidos, escolha um instantâneo anterior.
7. Contacte o suporte do Azure através de um pedido de suporte de alta prioridade. Pedir o restauro de nesse instantâneo (com o nome e a data do instantâneo) ou o ID de cópia de segurança HANA no site de DR. A predefinição é que o lado de operações restaura/hana/volume de dados apenas. Se pretender ter/hana/logbackups volumes bem, tem de estado de especificamente. *Não restaure o volume de /hana/shared.* Em vez disso, deve escolher ficheiros específicos, como global.ini fora do **.snapshot** diretório e nos seus subdiretórios depois remontar o hana/volume partilhado de PRD. 

   No lado de operações, ocorrem os seguintes passos:

   a. A replicação de instantâneos de volume de produção para os volumes de recuperação após desastre está parada. Este interrupção poderá ter sucedido já se uma falha no site de produção é o motivo que é necessário executar o procedimento de recuperação após desastre.
   
   b. O armazenamento de nome de instantâneo ou instantâneo com o ID de cópia de segurança que escolheu é restaurada nos volumes de recuperação após desastre.
   
   c. Após o restauro, os volumes de recuperação após desastre estão disponíveis para ser montado para as unidades de instância grande HANA na região de recuperação após desastre.
      
8. Monte os volumes de recuperação de desastres para a unidade de instância grande HANA no site de recuperação após desastre. 
9. Inicie a instância de produção dormant SAP HANA.
10. Se optar por copiar registos de cópia de segurança de registo de transações para reduzir o tempo RPO, tem de unir as cópias de segurança do registo de transação para o diretório de logbackups recentemente montado DR hana /. Não substitua cópias de segurança existentes. Copie as cópias de segurança mais recentes que não foram replicadas com a replicação mais recente de um instantâneo de armazenamento.
11. Também pode restaurar ficheiros único os instantâneos de tem sido replicados para o volume de /hana/shared/PRD na região do Azure de DR.

A seguinte sequência de passos envolve a recuperar a instância de produção de SAP HANA com base no instantâneo de armazenamento restaurada e as cópias de segurança de registo de transações que estão disponíveis:

1. Altere a localização de cópia de segurança para **/hana/logbackups** através da utilização de SAP HANA Studio.
   ![Alterar a localização de cópia de segurança para recuperação do DR](./media/hana-overview-high-availability-disaster-recovery/change_backup_location_dr1.png)

2. SAP HANA analisa através as localizações de ficheiros de cópia de segurança e sugere mais recente transação registo cópia de segurança a restaurar. A análise pode demorar alguns minutos até um ecrã de como é apresentado o seguinte: ![lista de cópias de segurança de registo de transação para a recuperação do DR](./media/hana-overview-high-availability-disaster-recovery/backup_list_dr2.PNG)

3. Ajuste algumas das predefinições:

      - Limpar **utilizar cópias de segurança diferencial**.
      - Selecione **inicializar registo área**.

   ![Definir a área de registo de inicializar](./media/hana-overview-high-availability-disaster-recovery/initialize_log_dr3.PNG)

4. Selecione **Concluir**.

   ![Concluir o restauro de DR](./media/hana-overview-high-availability-disaster-recovery/finish_dr4.PNG)

Uma janela de progresso, como mostrado aqui, deve aparecer. Tenha em atenção que o exemplo é de um restauro de recuperação após desastre de uma configuração de SAP HANA de escalamento horizontal de três nós.

![Restaurar progresso](./media/hana-overview-high-availability-disaster-recovery/restore_progress_dr5.PNG)

Se o restauro parecer fique suspensa no **concluir** ecrã e não mostrar o ecrã de progresso, confirme que todas as instâncias de SAP HANA em nós de trabalho estão em execução. Se necessário, inicie manualmente as instâncias de SAP HANA.


### <a name="failback-from-a-dr-to-a-production-site"></a>Reativação pós-falha a partir de um DR para um site de produção
Pode efetuar a reativação a partir de um DR para um site de produção. Vamos ver um cenário em que a ativação pós-falha para o site de recuperação após desastre foi causada por problemas na região do Azure de produção e não pela sua necessidade para recuperar dados perdidos. Tenham sido executados a carga de trabalho de produção do SAP durante o tempo no site de recuperação após desastre. Como são resolvidos os problemas no site de produção, pretende haja reativação para o site de produção. Porque não é possível perder dados, o passo anterior para o site de produção envolve vários passos e fechar cooperação com o SAP HANA na equipa de operações do Azure. Está a funcionar para que possa acionar a equipa de operações a iniciá-la para o site de produção após os problemas são resolvidos.

Esta é a sequência de passos:

1. O SAP HANA na equipa de operações do Azure obtém o acionador para sincronizar os volumes de armazenamento de produção dos volumes de armazenamento de recuperação após desastre, que agora representam o estado de produção. Neste estado, a unidade de instância grande HANA no site de produção é encerrada.
2. O SAP HANA na equipa de operações do Azure monitoriza a replicação e certifica-se de que este é processada antes informando-o.
3. Encerre as aplicações que utilizam a produção HANA instância no site de recuperação após desastre. Em seguida, efetuar uma cópia de segurança do registo de transações HANA. Em seguida, pare a execução das unidades de instância grande HANA no site de recuperação após desastre da instância HANA.
4. Depois da instância HANA em execução na unidade de instância grande HANA no site de recuperação de desastre é encerrada, a equipa de operações manualmente sincroniza os volumes de disco novamente.
5. O SAP HANA na equipa de operações do Azure começa novamente a unidade de instância grande HANA no site de produção e passa-lo ao longo para si. Certifique-se de que a instância de SAP HANA está num Estado de encerramento ao tempo de arranque da unidade instância grande HANA.
6. Efetuar os mesmos passos de restauro de base de dados, tal como fez quando anteriormente efetuar a ativação pós-falha para o site de recuperação após desastre.

### <a name="monitor-disaster-recovery-replication"></a>Monitorizar a replicação de recuperação de desastre

Pode monitorizar o estado do seu progresso de replicação de armazenamento, executar o script `azure_hana_replication_status.pl`. Este script tem de ser executado a partir de uma unidade em execução na localização de recuperação após desastre a funcionar conforme esperado. O script funciona independentemente se a replicação está ativa. O script pode ser executado para cada unidade de instância grande HANA do seu inquilino na localização de recuperação após desastre. Não pode ser utilizado para obter detalhes sobre o volume de arranque.

Chame o script com este comando:
```
./replication_status.pl <HANA SID>
```

O resultado é reduzido, por volume, nas seguintes secções:  

- Estado da ligação
- Atividade de replicação atual
- Mais recente instantâneo replicado 
- Tamanho do instantâneo mais recente
- Tempo de desfasamento atual entre a instantâneos (entre a última replicação de instantâneo concluído e agora)

O estado da ligação é apresentado como **Active Directory** , a menos que a ligação entre localizações estiver em baixo ou há um evento de ativação pós-falha em curso. A atividade de replicação endereços se quaisquer dados está atualmente a ser replicados ou está inativos, ou se outras atividades atualmente estão a ocorrer para a ligação. O último instantâneo replicado só deve ser apresentadas como `snapmirror…`. Em seguida, é apresentado o tamanho do último instantâneo. Por fim, o tempo de atraso é apresentado. O atraso representa a hora a replicação agendada para quando a replicação estiver concluída. Um atraso pode ser maior do que uma hora para a replicação de dados, especialmente na replicação inicial, apesar da replicação foi iniciada. O tempo de desfasamento continua a aumentar até que seja concluída a replicação em curso.

Segue-se um exemplo de saída:

```
hana_data_hm3_mnt00002_t020_dp
-------------------------------------------------
Link Status: Broken-Off
Current Replication Activity: Idle
Latest Snapshot Replicated: snapmirror.c169b434-75c0-11e6-9903-00a098a13ceb_2154095454.2017-04-21_051515
Size of Latest Snapshot Replicated: 244KB
Current Lag Time between snapshots: -   ***Less than 90 minutes is acceptable***
```












