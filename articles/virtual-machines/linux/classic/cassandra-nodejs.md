---
title: Executar um cluster do Cassandra no Linux no Azure do node. js
description: Como executar um cluster do Cassandra no Linux em máquinas de virtuais do Azure a partir de uma aplicação node. js
services: virtual-machines-linux
documentationcenter: nodejs
author: craigshoemaker
manager: routlaw
editor: ''
tags: azure-service-management
ms.assetid: 30de1f29-e97d-492f-ae34-41ec83488de0
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: cshoe
ms.openlocfilehash: b38db71e624d32e7a4a532181a374edb13f13fbf
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54021939"
---
# <a name="run-a-cassandra-cluster-on-linux-in-azure-with-nodejs"></a>Executar um cluster do Cassandra no Linux no Azure com node. js

> [!IMPORTANT]
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [Resource Manager e clássica](../../../resource-manager-deployment-model.md). Este artigo explica como utilizar o modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager. Ver modelos do Resource Manager para [Datastax Enterprise](https://azure.microsoft.com/documentation/templates/datastax) e [Spark cluster e Cassandra no CentOS](https://azure.microsoft.com/documentation/templates/spark-and-cassandra-on-centos/).

## <a name="overview"></a>Descrição geral
Microsoft Azure é uma plataforma de cloud aberta que executa o software de terceiros e da Microsoft. Este software contém a sistemas operacionais, servidores de aplicações, middleware de mensagens, bem como bancos de dados SQL e NoSQL de ambos os modelos comerciais e de código aberto. Criação de serviços resilientes em clouds públicas, incluindo o Azure requer um planejamento cuidadoso e arquitetura deliberada para ambos os servidores de aplicativos como camadas de armazenamento bem. Arquitetura de armazenamento distribuído do Cassandra naturalmente ajuda na criação de sistemas de elevada disponibilidade que são mais tolerante para falhas de cluster. Cassandra é uma base de dados NoSQL mantida pela Apache Software Foundation em cassandra.apache.org de escala da cloud. Cassandra está escrito em Java. Por isso, ele é executado em ambos nas plataformas Windows e Linux.

O foco deste artigo é mostrar a implementação do Cassandra no Ubuntu como um cluster único e os dados de multi center que utiliza máquinas virtuais do Azure e redes virtuais. A implementação do cluster para cargas de trabalho de produção otimizada está fora do escopo deste artigo, porque é necessária configuração de nó com múltiplos discos, design da topologia de anel apropriado e modelação de dados para suportar a replicação necessária, a consistência dos dados, o débito, e requisitos de alta disponibilidade.

Este artigo assume uma abordagem fundamental para mostrar o que está envolvido na criação de cluster do Cassandra, em comparação com o Docker, Chef ou Puppet. Essa abordagem pode fazer a implantação de infra-estrutura muito mais fácil.

## <a name="the-deployment-models"></a>Os modelos de implementação
Sistema de rede do Microsoft Azure permite a implementação de clusters privadas isoladas, o acesso dos quais pode ser restringido a atingir a segurança de rede detalhada. Este artigo é sobre o que mostra a implementação do Cassandra no nível básico, não se concentre em nível de consistência e a estrutura de armazenamento ideal para o débito. Eis a lista de requisitos de rede para o cluster hipotético:

* Sistemas externos não consegue aceder à base de dados Cassandra do dentro ou fora do Azure
* Cluster do Cassandra tem de estar atrás de um balanceador de carga para tráfego de thrift
* Implementar nós de Cassandra em dois grupos em cada Datacenter para um disponibilidade de cluster aprimorado
* Bloquear o cluster assim que apenas o farm de servidores de aplicação tem acesso à base de dados diretamente
* Não existem pontos de extremidade de rede públicos que não seja o SSH
* Cada nó de Cassandra tem um endereço IP interno fixo

Cassandra pode ser implementado numa única região do Azure ou em várias regiões com base na natureza distribuída da carga de trabalho. Pode utilizar um modelo de implementação em várias regiões para servir os utilizadores finais mais próximo para uma determinada geografia através da infraestrutura da mesma Cassandra. Nó incorporado replicação fica assim resolvida do Cassandra da sincronização de vários mestres escreve provenientes de vários centros de dados e apresenta uma exibição consistente dos dados para aplicações. Também pode ajudar a implementação em várias regiões com a redução de risco de falhas de serviço do Azure mais abrangentes. Topologia de replicação e consistência otimizável do Cassandra ajuda a atender a necessidades RPO diversificadas das aplicações.

### <a name="single-region-deployment"></a>Implementação numa única região
Vamos começar com uma implementação numa única região e coletar as lições aprendidas na criação de um modelo de várias regiões. Redes virtuais do Azure é utilizado para criar sub-redes isoladas, para que podem ser cumpridos os requisitos de segurança de rede mencionados acima. O processo descrito na criação da implementação de região única utiliza Ubuntu 14.04 LTS e Cassandra 2.08. No entanto, o processo facilmente pode ser adotado para as outras variantes de Linux. Seguem-se algumas das características sistêmicos da implementação única região.

**Elevada disponibilidade:** Os nós de Cassandra mostrados a figura 1 são implementados para dois conjuntos de disponibilidade, para que os nós são distribuídos entre vários domínios de falha para elevada disponibilidade. VMs anotadas com cada conjunto de disponibilidade é mapeado para 2 domínios de falha. Para gerir não planeada do tempo (por exemplo falhas de hardware ou software), o Azure utiliza o conceito de domínio de falha. O conceito de domínio de atualização (por exemplo, anfitrião ou aplicação de patches de SO convidado/atualização, as atualizações de aplicações) é utilizado para gerir agendada período de indisponibilidade. Veja [recuperação após desastre e elevada disponibilidade para aplicações do Azure](https://msdn.microsoft.com/library/dn251004.aspx) para a função de domínios de atualização e de falha na obtenção de elevada disponibilidade.

![Implementação numa única região](./media/cassandra-nodejs/cassandra-linux1.png)

Figura 1: Implementação numa única região

Tenha em atenção que no momento da redação deste artigo, Azure não permite o mapeamento explícito de um grupo de VMs para um domínio de falha específico; Conseqüentemente, mesmo com o modelo de implementação mostrado na figura 1 é estatisticamente provável que todas as máquinas virtuais podem ser mapeadas para dois domínios de falha em vez de quatro.

**Thrift tráfego de balanceamento de carga:** Bibliotecas de cliente Thrift dentro do servidor web a ligar ao cluster através de um balanceador de carga interno. Isto requer que o processo de adicionar o Balanceador de carga interno para a sub-rede de "dados" (consulte a figura 1) no contexto do serviço em nuvem que aloja o cluster do Cassandra. Assim que o Balanceador de carga interno é definido, cada nó requer o ponto de final com balanceamento de carga para ser adicionado com as anotações de um conjunto com balanceamento de carga com o nome do Balanceador de carga definido anteriormente. Ver [Azure balanceamento de carga interno ](../../../load-balancer/load-balancer-internal-overview.md)para obter mais detalhes.

**Sementes do cluster:** É importante selecionar os maioria de nós de elevada disponibilidade sementes como os novos nós comunicam com nós de semente para detetar a topologia do cluster. Um nó de cada conjunto de disponibilidade é designado como nós de semente para evitar ponto único de falha.

**Fator de replicação e nível de consistência:** Compilação em dados de elevada disponibilidade e durabilidade do Cassandra é caracterizada pelo fator de replicação (RF - número de cópias de cada linha armazenados no cluster) e o nível de consistência (número de réplicas para ser lidos/escritos antes de retornar o resultado para o chamador). Fator de replicação é especificado durante a criação de KEYSPACE (semelhante a uma base de dados relacional), ao passo que o nível de consistência é especificado ao emitir a consulta CRUD. Consulte a documentação do Cassandra em [configurar a consistência dos](https://docs.datastax.com/en/cassandra/3.0/cassandra/dml/dmlConfigConsistency.html) para detalhes de consistência e a fórmula para o cálculo de quórum.

Cassandra suporta dois tipos de modelos de integridade de dados – consistência e a consistência Eventual; o fator de replicação e o nível de consistência em conjunto em determinar se os dados são consistentes, assim como uma operação de gravação é concluída ou eventualmente consistente. Por exemplo, especificar o QUÓRUM como o nível de consistência sempre garante a consistência de dados durante a qualquer nível de consistência, abaixo o número de réplicas de escrita conforme necessário para atingir o QUÓRUM (por exemplo um) resulta em dados que está a ser eventualmente consistente.

O cluster de 8 nós mostrado acima, com um fator de replicação de 3 e o QUÓRUM (2 nós são lidos ou gravados por razões de consistência) de nível de consistência de leitura/escrita, pode sobreviver à perda teórica de até 1 nó por grupo de replicação antes da perceção de início de aplicação a Falha. Esta parte do princípio de que todos os espaços de chave também têm com balanceamento de leitura/escritam pedidos. Seguem-se os parâmetros utilizados para o cluster implementado:

Configuração de cluster do Cassandra de região única:

| Parâmetro de cluster | Valor | Observações |
| --- | --- | --- |
| Número de nós (N) |8 |Número total de nós do cluster |
| Fator de replicação (RF) |3 |Número de réplicas de uma determinada linha |
| Nível de consistência (escrita) |QUORUM[(RF/2) +1) = 2] o resultado da fórmula é arredondado para baixo |Escreve até 2 réplicas antes da resposta é enviada para o chamador; réplica 3º é escrita de forma eventualmente consistente. |
| Nível de consistência (leitura) |QUÓRUM [(RF/2) + 1 = 2] o resultado da fórmula é arredondado para baixo |Lê 2 réplicas antes de enviar a resposta para o chamador. |
| Estratégia de replicação |Consulte NetworkTopologyStrategy [replicação de dados](https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archDataDistributeAbout.html) na documentação do Cassandra para obter mais informações |Compreende a topologia de implementação e coloca réplicas em nós, para que todas as réplicas não chegará no mesmo rack |
| Snitch |Consulte GossipingPropertyFileSnitch [comutadores](https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archSnitchesAbout.html) na documentação do Cassandra para obter mais informações |NetworkTopologyStrategy usa um conceito de snitch para compreender a topologia. GossipingPropertyFileSnitch fornece controle melhor mapear cada nó para o Centro de dados e rack. O cluster utiliza, em seguida, fofocas para propagar essas informações. Isso é muito mais simples na definição de IP dinâmica em relação ao PropertyFileSnitch |

**Considerações do Azure para o Cluster do Cassandra:** Capacidade de máquinas virtuais do Microsoft Azure utiliza o armazenamento de Blobs do Azure para a persistência do disco; O armazenamento do Azure guarda três réplicas de cada disco para uma elevada durabilidade. Isso significa que cada linha de dados inseridos numa tabela Cassandra já é armazenada em três réplicas. Então, consistência de dados já tratámos das, mesmo que o fator de replicação (RF) é 1. O problema principal com fator de replicação que está sendo 1 é que o aplicativo experiências de tempo de inatividade, mesmo se falha um nó único do Cassandra. No entanto, se um nó estiver em baixo para os problemas (por exemplo, hardware, falhas de software do sistema) reconhecidos pelo controlador de malha do Azure, ele fornece um novo nó no seu lugar usando as mesmas unidades de armazenamento. Um novo nó para substituir a antiga de aprovisionamento pode demorar alguns minutos. Da mesma forma para atividades de manutenção planeada, como as alterações ao SO convidado, atualiza o Cassandra e alterações de aplicativo controlador de malha do Azure realiza a implementar atualizações de nós no cluster. Atualizações sem interrupção também poderão demorar baixo alguns nós ao mesmo tempo e, por conseguinte, o cluster poderá experienciar breve período de indisponibilidade para algumas partições. No entanto, os dados não não perdidos devido a redundância de armazenamento do Azure incorporada.

Para sistemas implementados no Azure que não exijam a elevada disponibilidade (por exemplo cerca 99,9 que é equivalente a 8.76 horas/ano; consulte [elevada disponibilidade](http://en.wikipedia.org/wiki/High_availability) para obter detalhes) é capaz de executar com RF = 1 e o nível de consistência = ONE. Para aplicações com requisitos de alta disponibilidade, RF = 3 e o nível de consistência = QUÓRUM tolera indisponíveis de um de nós de uma das réplicas. RF = 1 em implementações tradicionais (por exemplo no local) não é possível utilizar devidos à perda de dados resultante de problemas, como falhas de disco.

## <a name="multi-region-deployment"></a>Implementação em várias regiões
Data center-com suporte para replicação e a consistência modelo do Cassandra descrito acima ajuda com a implementação em várias regiões sem a necessidade de quaisquer ferramentas externas. Isto é diferente dos bancos de dados relacionais tradicionais onde a configuração para o espelhamento de banco de dados para gravações de vários mestres pode ser complexa. Cassandra numa configuração de várias regiões pode ajudar com cenários de utilização, incluindo os cenários:

**Implementação baseados em proximidade:** Aplicações multi-inquilino, com o mapeamento de clara de utilizadores de inquilino-para-região, pode ser se beneficiou por latências baixas do cluster de várias regiões. Por exemplo, um aprendizado sistemas de gestão para instituições de ensino podem implementar um cluster distribuído nas regiões E.U.A. leste e E.U.A. oeste para servir os respectivos Campus para transacional, bem como a análise. Os dados podem ser consistentes localmente no tempo de leituras e escritas e podem ser eventualmente consistentes em ambas as regiões. Existem outros exemplos, como a distribuição de multimédia, e-commerce e qualquer coisa e tudo o que serve o utilizador geo concentrada base é um caso de bom uso para este modelo de implementação.

**Elevada disponibilidade:** Redundância é um fator importante na obtenção de elevada disponibilidade de software e hardware; Ver criar sistemas de nuvem confiável no Microsoft Azure para obter detalhes. No Microsoft Azure, de forma fiável apenas de conseguir verdadeira redundância é ao implementar um cluster de várias regiões. Os aplicativos podem ser implantados em modo ativo-ativo ou ativa-passiva e se uma das regiões estiver desativado, o Gestor de tráfego do Azure pode redirecionar o tráfego para a região de Active Directory. Com a implementação de região única, se a disponibilidade é 99,9, uma implementação em duas regiões pode obter um disponibilidade de lt;99,9999 calculada pela fórmula: (1-(1-0.999) * (1-0.999)) * 100); consulte o artigo acima para obter detalhes.

**Recuperação de desastres:** Se criada adequadamente, cluster do Cassandra de várias regiões, possa suportar a falhas de centro de dados catastrófica. Se uma região está inativo, a aplicação implementada para outras regiões pode começa a servir os utilizadores finais. Como qualquer outras empresas continuidade implementações, a aplicação tem de ser tolerância a falhas para alguma perda de dados resultantes dos dados no pipeline assíncrona. No entanto, Cassandra torna a recuperação muito swifter que o tempo que os processos de recuperação de bases de dados tradicionais. Figura 2 mostra o modelo de implementação em várias regiões típico com oito nós em cada região. Ambas as regiões são imagens espelhadas entre si para a mesma de simetria; designs de mundo real dependem do tipo de carga de trabalho (por exemplo transacional ou analítico), RPO, RTO, consistência dos dados e requisitos de disponibilidade.

![Implementação em várias regiões](./media/cassandra-nodejs/cassandra-linux2.png)

Figura 2: Implementação Cassandra em várias regiões

### <a name="network-integration"></a>Integração da rede
Conjuntos de máquinas virtuais implementadas em redes privadas localizados em duas regiões comunica-se entre si através de um túnel VPN. O túnel VPN liga-se dois gateways de software aprovisionados durante o processo de implementação de rede. Ambas as regiões têm a arquitetura de rede semelhante em termos de sub-redes de "web" e "data"; Redes do Azure permite a criação de sub-redes tantos conforme necessário e aplique ACLs conforme necessário por segurança de rede. Ao conceber a topologia do cluster, inter latência de comunicação do Centro de dados e o impacto econômico da necessidade de tráfego de rede para ser considerado.

### <a name="data-consistency-for-multi-data-center-deployment"></a>Consistência de dados para a implementação de Datacenter multi
Distribuído implementações têm de ser ciente do impacto de topologia de cluster relativamente ao débito e elevada disponibilidade. O nível de consistência e RF precisam de ser selecionada de forma que o quórum não depende da disponibilidade de todos os centros de dados.
Para um sistema que precisa de consistência elevada, um LOCAL_QUORUM para o nível de consistência (para leituras e escritas) certifica-se de que as locais leituras e gravações são satisfeitas a partir do locais nós enquanto os dados são replicados de forma assíncrona para os centros de dados remota. Tabela 2 resume os detalhes de configuração para o cluster de várias regiões, descritos mais tarde a gravação.

**Configuração de cluster do Cassandra de duas regiões**

| Parâmetro de cluster | Valor | Observações |
| --- | --- | --- |
| Número de nós (N) |8 + 8 |Número total de nós do cluster |
| Fator de replicação (RF) |3 |Número de réplicas de uma determinada linha |
| Nível de consistência (escrita) |LOCAL_QUORUM [(sum(RF)/2) +1) = 4] o resultado da fórmula é arredondado para baixo |2 nós é escrito para o Centro de dados primeiro, de forma síncrona; adicionais 2 nós necessários para o quórum é escrito em forma assíncrona para o 2ª Centro de dados. |
| Nível de consistência (leitura) |LOCAL_QUORUM ((RF/2) + 1) = 2, o resultado da fórmula é arredondado para baixo |Os pedidos de leitura são satisfeitos a partir do apenas uma região. 2 nós são Leia antes da resposta é enviada de volta ao cliente. |
| Estratégia de replicação |Consulte NetworkTopologyStrategy [replicação de dados](https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archDataDistributeAbout.html) na documentação do Cassandra para obter mais informações |Compreende a topologia de implementação e coloca réplicas em nós, para que todas as réplicas não chegará no mesmo rack |
| Snitch |Consulte GossipingPropertyFileSnitch [Snitches](https://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archSnitchesAbout.html) na documentação do Cassandra para obter mais informações |NetworkTopologyStrategy usa um conceito de snitch para compreender a topologia. GossipingPropertyFileSnitch fornece controle melhor mapear cada nó para o Centro de dados e rack. O cluster utiliza, em seguida, fofocas para propagar essas informações. Isso é muito mais simples na definição de IP dinâmica em relação ao PropertyFileSnitch |

## <a name="the-software-configuration"></a>A CONFIGURAÇÃO DE SOFTWARE
As seguintes versões de software são usadas durante a implementação:

<table>
<tr><th>Software</th><th>Origem</th><th>Versão</th></tr>
<tr><td>JRE    </td><td>[JRE 8](https://aka.ms/azure-jdks) </td><td>8U5</td></tr>
<tr><td>JNA    </td><td>[JNA](https://github.com/twall/jna) </td><td> 3.2.7</td></tr>
<tr><td>Cassandra</td><td>[O Apache Cassandra 2.0.8](http://www.apache.org/dist/cassandra/)</td><td> 2.0.8</td></tr>
<tr><td>Ubuntu    </td><td>[Microsoft Azure](https://azure.microsoft.com/) </td><td>14.04 LTS</td></tr>
</table>

Para simplificar a implantação, Baixe o software necessário para a área de trabalho. Em seguida, carregue-o para a imagem de modelo do Ubuntu para criar como uma precursora para a implementação do cluster.

Baixe o software de acima para um diretório de download bem conhecidos (por exemplo %TEMP%/downloads no Windows ou ~/Downloads na maioria das distribuições de Linux ou Mac) no computador local.

### <a name="create-ubuntu-vm"></a>CRIAR VM DO UBUNTU
Neste passo do processo, vai criar imagem de Ubuntu com o software de pré-requisitos para que a imagem pode ser reutilizada para o aprovisionamento de vários nós de Cassandra.

#### <a name="step-1-generate-ssh-key-pair"></a>PASSO 1: Gerar o par de chaves SSH
O Azure necessita de uma chave pública PEM ou DER codificado com o tempo de aprovisionamento de X509. Gere um par de chaves públicas/privadas, com as instruções localizadas como pode utilizar o SSH com Linux no Azure. Se planeja usar putty.exe como um cliente SSH no Windows ou Linux, tem de converter PEM codificado por chave privada RSA para o formato PPK puttygen.exe a utilizar. As instruções para que isso podem ser encontradas na página da web acima.

#### <a name="step-2-create-ubuntu-template-vm"></a>PASSO 2: Criar VM do Ubuntu modelo
Para criar o modelo de VM, inicie sessão no portal do Azure e utilize a seguinte sequência: Clique em novo, a COMPUTAÇÃO, a máquina VIRTUAL, a GALERIA de FROM, a UBUNTU, a Ubuntu Server 14.04 LTS e, em seguida, clique na seta à direita. Para obter um tutorial que descreve como criar uma VM do Linux, consulte Criar numa máquina Virtual com Linux em execução.

Introduza as seguintes informações na tela "configuração da Máquina Virtual" #1:

<table>
<tr><th>NOME DO CAMPO              </td><td>       VALOR DO CAMPO               </td><td>         COMENTÁRIOS                </td><tr>
<tr><td>DATA DE LANÇAMENTO DE VERSÃO    </td><td> Selecione uma data no menu pendente</td><td></td><tr>
<tr><td>NOME DA MÁQUINA VIRTUAL    </td><td> modelo de CASS                   </td><td> Este é o nome do anfitrião da VM </td><tr>
<tr><td>ESCALÃO                     </td><td> STANDARD                           </td><td> Deixe a predefinição              </td><tr>
<tr><td>TAMANHO                     </td><td> A1                              </td><td>Selecione a VM com base nas necessidades e/s; para este fim, deixe a predefinição </td><tr>
<tr><td> NOVO NOME DE UTILIZADOR             </td><td> localadmin                       </td><td> "admin" é um nome de utilizador reservados no Ubuntu 12. xx e depois</td><tr>
<tr><td> AUTENTICAÇÃO         </td><td> Clique em caixa de verificação                 </td><td>Verifique se pretende proteger com uma chave SSH </td><tr>
<tr><td> CERTIFICADO             </td><td> nome de ficheiro de certificado de chave pública </td><td> Utilizar a chave pública gerada anteriormente</td><tr>
<tr><td> Nova Palavra-passe    </td><td> palavra-passe segura </td><td> </td><tr>
<tr><td> Confirmar Palavra-passe    </td><td> palavra-passe segura </td><td></td><tr>
</table>

Introduza as seguintes informações na tela "configuração da Máquina Virtual" #2:

<table>
<tr><th>NOME DO CAMPO             </th><th> VALOR DO CAMPO                       </th><th> COMENTÁRIOS                                 </th></tr>
<tr><td> SERVIÇO EM NUVEM    </td><td> Criar um novo serviço cloud    </td><td>Serviço em nuvem é um recursos de computação do contentor como as máquinas virtuais</td></tr>
<tr><td> NOME DE DNS DO SERVIÇO CLOUD    </td><td>ubuntu-template.cloudapp.net    </td><td>Dê um nome de Balanceador de carga agnóstica de máquina</td></tr>
<tr><td> REGIÃO/GRUPO DE AFINIDADE/REDE VIRTUAL </td><td>    EUA Oeste    </td><td> Selecione uma região a partir do qual os aplicativos web aceder ao cluster do Cassandra</td></tr>
<tr><td>CONTA DE ARMAZENAMENTO </td><td>    Utilizar predefinição    </td><td>Utilizar a conta de armazenamento predefinida ou uma conta de armazenamento previamente criados numa região específica</td></tr>
<tr><td>CONJUNTO DE DISPONIBILIDADE </td><td>    Nenhuma </td><td>    Deixe em branco</td></tr>
<tr><td>PONTOS FINAIS    </td><td>Utilizar predefinição </td><td>    Utilizar a configuração predefinida do SSH </td></tr>
</table>

Clique em seta para a direita, deixe as predefinições na tela #3. Clique no botão "verificar" para concluir o processo de aprovisionamento da VM. Após alguns minutos, a VM com o nome "ubuntu-template" deve estar no estado "em execução".

### <a name="install-the-necessary-software"></a>INSTALAR O SOFTWARE NECESSÁRIO
#### <a name="step-1-upload-tarballs"></a>PASSO 1: Carregar tarballs
Utilizar o scp ou pscp, copie o software transferido anteriormente diretório ~/downloads através do formato do comando seguinte:

##### <a name="pscp-server-jre-8u5-linux-x64targz-localadminhk-cas-templatecloudappnethomelocaladmindownloadsserver-jre-8u5-linux-x64targz"></a>pscp server-jre-8u5-linux-x64.tar.gz localadmin@hk-cas-template.cloudapp.net:/home/localadmin/downloads/server-jre-8u5-linux-x64.tar.gz
Repita o comando acima para JRE, bem como para os bits de Cassandra.

#### <a name="step-2-prepare-the-directory-structure-and-extract-the-archives"></a>PASSO 2: Preparar a estrutura de diretórios e extrair os arquivos mortos
Inicie sessão na VM e criar a estrutura de diretório e extrair o software como um Superutilizador utilizando o script de bash abaixo:

```bash
#!/bin/bash
CASS_INSTALL_DIR="/opt/cassandra"
JRE_INSTALL_DIR="/opt/java"
CASS_DATA_DIR="/var/lib/cassandra"
CASS_LOG_DIR="/var/log/cassandra"
DOWNLOADS_DIR="~/downloads"
JRE_TARBALL="server-jre-8u5-linux-x64.tar.gz"
CASS_TARBALL="apache-cassandra-2.0.8-bin.tar.gz"
SVC_USER="localadmin"

RESET_ERROR=1
MKDIR_ERROR=2

reset_installation ()
{
  rm -rf $CASS_INSTALL_DIR 2> /dev/null
  rm -rf $JRE_INSTALL_DIR 2> /dev/null
  rm -rf $CASS_DATA_DIR 2> /dev/null
  rm -rf $CASS_LOG_DIR 2> /dev/null
}
make_dir ()
{
  if [ -z "$1" ]
  then
    echo "make_dir: invalid directory name"
    exit $MKDIR_ERROR
  fi

  if [ -d "$1" ]
  then
    echo "make_dir: directory already exists"
    exit $MKDIR_ERROR
  fi

  mkdir $1 2>/dev/null
  if [ $? != 0 ]
  then
    echo "directory creation failed"
    exit $MKDIR_ERROR
  fi
}

unzip()
{
  if [ $# == 2 ]
  then
    tar xzf $1 -C $2
  else
    echo "archive error"
  fi

}

if [ -n "$1" ]
then
  SVC_USER=$1
fi

reset_installation
make_dir $CASS_INSTALL_DIR
make_dir $JRE_INSTALL_DIR
make_dir $CASS_DATA_DIR
make_dir $CASS_LOG_DIR

#unzip JRE and Cassandra
unzip $HOME/downloads/$JRE_TARBALL $JRE_INSTALL_DIR
unzip $HOME/downloads/$CASS_TARBALL $CASS_INSTALL_DIR

#Change the ownership to the service credentials

chown -R $SVC_USER:$GROUP $CASS_DATA_DIR
chown -R $SVC_USER:$GROUP $CASS_LOG_DIR
echo "edit /etc/profile to add JRE to the PATH"
echo "installation is complete"
```

Se colar esse script na janela de vim, certifique-se remover o retorno de carro ("\r") com o seguinte comando:

    tr -d '\r' <infile.sh >outfile.sh

#### <a name="step-3-edit-etcprofile"></a>Passo 3: Editar etc/perfil
Anexe o seguinte no final:

    JAVA_HOME=/opt/java/jdk1.8.0_05
    CASS_HOME= /opt/cassandra/apache-cassandra-2.0.8
    PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$CASS_HOME/bin
    export JAVA_HOME
    export CASS_HOME
    export PATH

#### <a name="step-4-install-jna-for-production-systems"></a>Passo 4: Instalar JNA para sistemas de produção
Utilize a seguinte sequência de comando: O seguinte comando instala jna 3.2.7.jar e jna plataforma 3.2.7.jar para /usr/share.java directory sudo apt-get instalar libjna-java

Crie links simbólicos no diretório CASS_HOME/lib $ para que o script de inicialização do Cassandra pode encontrar estas jars:

    ln -s /usr/share/java/jna-3.2.7.jar $CASS_HOME/lib/jna.jar

    ln -s /usr/share/java/jna-platform-3.2.7.jar $CASS_HOME/lib/jna-platform.jar

#### <a name="step-5-configure-cassandrayaml"></a>Passo 5: Configurar cassandra.yaml
Edite cassandra.yaml em cada VM para refletir a configuração necessária por todas as máquinas virtuais [é ajustar essa configuração durante o aprovisionamento real]:

<table>
<tr><th>Nome do Campo   </th><th> Valor  </th><th>    Observações </th></tr>
<tr><td>cluster_name </td><td>    "CustomerService"    </td><td> Utilize o nome que reflete a implementação</td></tr>
<tr><td>listen_address    </td><td>[deixe em branco]    </td><td> Eliminar "localhost" </td></tr>
<tr><td>rpc_addres   </td><td>[deixe em branco]    </td><td> Eliminar "localhost" </td></tr>
<tr><td>sementes    </td><td>"10.1.2.4, 10.1.2.6, 10.1.2.8"    </td><td>Lista de todos os endereços IP que são designados como sementes.</td></tr>
<tr><td>endpoint_snitch </td><td> org.apache.cassandra.locator.GossipingPropertyFileSnitch </td><td> Isto é utilizado pelo NetworkTopologyStrateg para inferir o Centro de dados e o suporte da VM</td></tr>
</table>

#### <a name="step-6-capture-the-vm-image"></a>Passo 6: Capturar a imagem VM
Inicie sessão na máquina virtual utilizando o nome de anfitrião (hk-cas-template.cloudapp.net) e a chave privada SSH criado anteriormente. Veja como utilizar o SSH com Linux no Azure para obter detalhes sobre como iniciar sessão com o comando ssh ou putty.exe.

Execute a sequência de ações para capturar a imagem seguinte:

##### <a name="1-deprovision"></a>1. Desaprovisionamento
Utilize o comando "sudo waagent – desaprovisionamento + utilizador" para remover informações específicas de instância de Máquina Virtual. Consulte para [como capturar uma Máquina Virtual Linux](capture-image-classic.md) para utilizar como um modelo mais detalhes sobre o processo de captura de imagem.

##### <a name="2-shut-down-the-vm"></a>2: Encerre a VM
Certifique-se de que a máquina virtual é realçada e clique no link de encerramento da barra de comandos na parte inferior.

##### <a name="3-capture-the-image"></a>3: Capturar a imagem
Certifique-se de que a máquina virtual é realçada e clique no link de captura da barra de comandos na parte inferior. No seguinte ecrã, dê um nome de imagem (por exemplo hk-cas-2-08-ub-14-04-2014071), apropriado descrição da imagem e clique em "verificar" marca para concluir o processo de captura.

Este processo demora alguns segundos e a imagem deve estar disponível na seção de minhas imagens da Galeria de imagens. A VM de origem é eliminado automaticamente depois da imagem é capturada com êxito. 

## <a name="single-region-deployment-process"></a>Processo de implantação de região única
**Passo 1: Criar a rede Virtual** inicie sessão no portal do Azure e criar uma rede virtual (clássica) com os atributos mostrados na tabela seguinte. Ver [criar uma rede virtual (clássico) com o portal do Azure](../../../virtual-network/virtual-networks-create-vnet-classic-pportal.md) para obter passos detalhados do processo.

<table>
<tr><th>Nome de atributo VM</th><th>Valor</th><th>Observações</th></tr>
<tr><td>Nome</td><td>vnet-cass-west-us</td><td></td></tr>
<tr><td>Região</td><td>EUA Oeste</td><td></td></tr>
<tr><td>Servidores DNS</td><td>Nenhuma</td><td>Ignorá-lo como não estamos usando um servidor DNS</td></tr>
<tr><td>Espaço de Endereços</td><td>10.1.0.0/16</td><td></td></tr>
<tr><td>IP inicial</td><td>10.1.0.0</td><td></td></tr>
<tr><td>CIDR </td><td>/16 (65531)</td><td></td></tr>
</table>

Adicione as seguintes sub-redes:

<table>
<tr><th>Nome</th><th>IP inicial</th><th>CIDR</th><th>Observações</th></tr>
<tr><td>web</td><td>10.1.1.0</td><td>/24 (251)</td><td>Sub-rede para a web farm</td></tr>
<tr><td>dados</td><td>10.1.2.0</td><td>/24 (251)</td><td>Sub-rede para os nós de base de dados</td></tr>
</table>

Dados e as sub-redes de Web podem ser protegidas por meio dos grupos de segurança de rede a cobertura de que está fora do escopo deste artigo.

**Passo 2: Aprovisionar máquinas virtuais** utilizando a imagem criada anteriormente, criar as seguintes máquinas virtuais no servidor de cloud "hk-c-svc-Oeste" e ligá-los para as respetivas sub-redes, conforme mostrado abaixo:

<table>
<tr><th>Nome do computador    </th><th>Subrede    </th><th>Endereço IP    </th><th>Conjunto de disponibilidade</th><th>Controlador de domínio/bastidor</th><th>Efetuar o seeding?</th></tr>
<tr><td>HK-c1---oeste    </td><td>dados    </td><td>10.1.2.4    </td><td>hk-c-aset-1    </td><td>DC = WESTUS rack = rack1 </td><td>Sim</td></tr>
<tr><td>HK-c2---oeste    </td><td>dados    </td><td>10.1.2.5    </td><td>hk-c-aset-1    </td><td>DC = WESTUS rack = rack1    </td><td>Não </td></tr>
<tr><td>HK-c3---oeste    </td><td>dados    </td><td>10.1.2.6    </td><td>hk-c-aset-1    </td><td>DC = WESTUS rack = rack2    </td><td>Sim</td></tr>
<tr><td>HK-c4---oeste    </td><td>dados    </td><td>10.1.2.7    </td><td>hk-c-aset-1    </td><td>DC = WESTUS rack = rack2    </td><td>Não </td></tr>
<tr><td>HK-c5---oeste    </td><td>dados    </td><td>10.1.2.8    </td><td>hk-c-aset-2    </td><td>DC = WESTUS rack = rack3    </td><td>Sim</td></tr>
<tr><td>HK-c6---oeste    </td><td>dados    </td><td>10.1.2.9    </td><td>hk-c-aset-2    </td><td>DC = WESTUS rack = rack3    </td><td>Não </td></tr>
<tr><td>HK-c7---oeste    </td><td>dados    </td><td>10.1.2.10    </td><td>hk-c-aset-2    </td><td>DC = WESTUS rack = rack4    </td><td>Sim</td></tr>
<tr><td>HK-c8---oeste    </td><td>dados    </td><td>10.1.2.11    </td><td>hk-c-aset-2    </td><td>DC = WESTUS rack = rack4    </td><td>Não </td></tr>
<tr><td>HK-w1---oeste    </td><td>web    </td><td>10.1.1.4    </td><td>hk-w-aset-1    </td><td>                       </td><td>N/A</td></tr>
<tr><td>HK-w2---oeste    </td><td>web    </td><td>10.1.1.5    </td><td>hk-w-aset-1    </td><td>                       </td><td>N/A</td></tr>
</table>

Criação da lista acima das VMs requer o seguinte processo:

1. Criar um serviço cloud vazio numa região específica
2. Criar uma VM a partir da imagem capturada anteriormente e anexá-lo para a rede virtual criada anteriormente; Repetir isso para todas as VMs
3. Adicionar um balanceador de carga interno para o serviço em nuvem e anexá-lo para a sub-rede de "dados"
4. Para cada VM que criou anteriormente, adicionar um ponto de final de balanceamento de carga para tráfego de thrift, por meio de um conjunto com balanceamento de carga ligado ao balanceador de carga interno criado anteriormente

O processo acima pode ser executado através do portal do Azure; utilizar uma máquina Windows (utilize uma VM no Azure se não tiver acesso a uma máquina Windows), utilize o seguinte script do PowerShell para aprovisionar automaticamente todas as VMs de 8.

**Lista de 1: Script do PowerShell para o aprovisionamento de máquinas virtuais**

```powershell
#Tested with Azure Powershell - November 2014
#This powershell script deployes a number of VMs from an existing image inside an Azure region
#Import your Azure subscription into the current Powershell session before proceeding
#The process: 1. create Azure Storage account, 2. create virtual network, 3.create the VM template, 2. create a list of VMs from the template

#fundamental variables - change these to reflect your subscription
$country="us"; $region="west"; $vnetName = "your_vnet_name";$storageAccount="your_storage_account"
$numVMs=8;$prefix = "hk-cass";$ilbIP="your_ilb_ip"
$subscriptionName = "Azure_subscription_name";
$vmSize="ExtraSmall"; $imageName="your_linux_image_name"
$ilbName="ThriftInternalLB"; $thriftEndPoint="ThriftEndPoint"

#generated variables
$serviceName = "$prefix-svc-$region-$country"; $azureRegion = "$region $country"

$vmNames = @()
for ($i=0; $i -lt $numVMs; $i++)
{
    $vmNames+=("$prefix-vm"+($i+1) + "-$region-$country" );
}

#select an Azure subscription already imported into Powershell session
Select-AzureSubscription -SubscriptionName $subscriptionName -Current
Set-AzureSubscription -SubscriptionName $subscriptionName -CurrentStorageAccountName $storageAccount

#create an empty cloud service
New-AzureService -ServiceName $serviceName -Label "hkcass$region" -Location $azureRegion
Write-Host "Created $serviceName"

$VMList= @()   # stores the list of azure vm configuration objects
#create the list of VMs
foreach($vmName in $vmNames)
{
    $VMList += New-AzureVMConfig -Name $vmName -InstanceSize ExtraSmall -ImageName $imageName |
            Add-AzureProvisioningConfig -Linux -LinuxUser "localadmin" -Password "Local123" |
            Set-AzureSubnet "data"
}

New-AzureVM -ServiceName $serviceName -VNetName $vnetName -VMs $VMList

#Create internal load balancer
Add-AzureInternalLoadBalancer -ServiceName $serviceName -InternalLoadBalancerName $ilbName -SubnetName "data" -StaticVNetIPAddress "$ilbIP"
Write-Host "Created $ilbName"
#Add the thrift endpoint to the internal load balancer for all the VMs
foreach($vmName in $vmNames)
{
    Get-AzureVM -ServiceName $serviceName -Name $vmName |
            Add-AzureEndpoint -Name $thriftEndPoint -LBSetName "ThriftLBSet" -Protocol tcp -LocalPort 9160 -PublicPort 9160 -ProbePort 9160 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ilbName |
            Update-AzureVM

    Write-Host "created $vmName"
}
```

**Passo 3: Configurar o Cassandra em cada VM**

Inicie sessão na VM e execute o seguinte:

* Edite $CASS_HOME/conf/cassandra-rackdc.properties para especificar as propriedades de centro e suporte de dados:
  
       dc =EASTUS, rack =rack1
* Edite cassandra.yaml para configurar nós de semente conforme mostrado a seguir:
  
       Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10"

**Passo 4: Iniciar as VMs e testar o cluster**

Inicie sessão em um de nós (por exemplo hk-c1---oeste) e execute o seguinte comando para ver o estado do cluster:

       nodetool –h 10.1.2.4 –p 7199 status

Deverá ver a exibição semelhante ao seguinte para um cluster de 8 nós:

<table>
<tr><th>Estado</th><th>Endereço    </th><th>Carregar    </th><th>Tokens    </th><th>É o proprietário </th><th>ID de anfitrião    </th><th>Rack</th></tr>
<tr><th>ANULAR    </td><td>10.1.2.4     </td><td>87.81 KB    </td><td>256    </td><td>38.0%    </td><td>GUID (removida)</td><td>rack1</td></tr>
<tr><th>ANULAR    </td><td>10.1.2.5     </td><td>41.08 KB    </td><td>256    </td><td>68.9%    </td><td>GUID (removida)</td><td>rack1</td></tr>
<tr><th>ANULAR    </td><td>10.1.2.6     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (removida)</td><td>rack2</td></tr>
<tr><th>ANULAR    </td><td>10.1.2.7     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (removida)</td><td>rack2</td></tr>
<tr><th>ANULAR    </td><td>10.1.2.8     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (removida)</td><td>rack3</td></tr>
<tr><th>ANULAR    </td><td>10.1.2.9     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (removida)</td><td>rack3</td></tr>
<tr><th>ANULAR    </td><td>10.1.2.10     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (removida)</td><td>rack4</td></tr>
<tr><th>ANULAR    </td><td>10.1.2.11     </td><td>55.29 KB    </td><td>256    </td><td>68.8%    </td><td>GUID (removida)</td><td>rack4</td></tr>
</table>

## <a name="test-the-single-region-cluster"></a>Teste o Cluster de região única
Utilize os seguintes passos para testar o cluster:

1. Utilizar o commandlet do Powershell comando Get-AzureInternalLoadbalancer, obter o endereço IP do Balanceador de carga interno (por exemplo 10.1.2.101). A sintaxe do comando é mostrada abaixo: Get-AzureLoadbalancer – ServiceName "hk-c-svc--e.u.a. Oeste" [apresenta os detalhes do Balanceador de carga interno, juntamente com o respetivo endereço IP de]
2. Inicie sessão no web farm VM (por exemplo hk-w1---oeste) a utilizar o Putty ou ssh
3. Executar $CASS_HOME/bin/cqlsh 10.1.2.101 9160
4. Utilize os seguintes comandos CQL para verificar se o cluster está a funcionar:
   
     Criar KEYSPACE customers_ks com replicação = {"classe": "SimpleStrategy',"replication_factor": 3};   UTILIZAR customers_ks;   Criar tabela Customers(customer_id int PRIMARY KEY, firstname text, lastname text);   Inserir em Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   INSERT INTO o Customers(customer_id, firstname, lastname) VALUES (2, "Joana", "Doe");
   
     SELECIONAR * de clientes;

Deverá ver algo parecido com os seguintes resultados:

<table>
  <tr><th> customer_id </th><th> FirstName </th><th> Apelido </th></tr>
  <tr><td> 1 </td><td> John </td><td> Doe </td></tr>
  <tr><td> 2 </td><td> Joana </td><td> Doe </td></tr>
</table>

Keyspace criada no passo 4 usa SimpleStrategy com um replication_factor de 3. SimpleStrategy é recomendada para dados único center implementações, ao passo que o Centro de NetworkTopologyStrategy para dados de várias implementações. Um replication_factor de 3 fornece tolerância a falhas de nó.

## <a id="tworegion"> </a>Processo de implementação em várias regiões
Aproveite a implementação de região única foi concluída e repita o mesmo processo para a instalação da segunda região. A principal diferença entre a implementação de região única e múltipla é a configuração de túnel VPN para a comunicação entre regiões; começar com a instalação de rede, aprovisionar as VMs e configurar o Cassandra.

### <a name="step-1-create-the-virtual-network-at-the-2nd-region"></a>Passo 1: Criar a rede Virtual na região 2a
Inicie sessão no portal do Azure e criar uma rede Virtual com o programa de atributos na tabela. Ver [configurar uma rede Virtual Cloud-Only no portal do Azure](../../../virtual-network/virtual-networks-create-vnet-classic-pportal.md) para obter passos detalhados do processo.

<table>
<tr><th>Nome do Atributo    </th><th>Valor    </th><th>Observações</th></tr>
<tr><td>Nome    </td><td>vnet-cass-east-us</td><td></td></tr>
<tr><td>Região    </td><td>EUA Leste</td><td></td></tr>
<tr><td>Servidores DNS        </td><td></td><td>Ignorá-lo como não estamos usando um servidor DNS</td></tr>
<tr><td>Configurar uma VPN ponto a site</td><td></td><td>        Ignorar Isto</td></tr>
<tr><td>Configurar uma rede de VPN</td><td></td><td>        Ignorar Isto</td></tr>
<tr><td>Espaço de Endereços    </td><td>10.2.0.0/16</td><td></td></tr>
<tr><td>IP inicial    </td><td>10.2.0.0    </td><td></td></tr>
<tr><td>CIDR    </td><td>/16 (65531)</td><td></td></tr>
</table>

Adicione as seguintes sub-redes:

<table>
<tr><th>Nome    </th><th>IP inicial    </th><th>CIDR    </th><th>Observações</th></tr>
<tr><td>web    </td><td>10.2.1.0    </td><td>/24 (251)    </td><td>Sub-rede para a web farm</td></tr>
<tr><td>dados    </td><td>10.2.2.0    </td><td>/24 (251)    </td><td>Sub-rede para os nós de base de dados</td></tr>
</table>


### <a name="step-2-create-local-networks"></a>Passo 2: Criar redes locais
Uma rede Local no sistema de rede virtual do Azure é um espaço de endereço de proxy que mapeia para um site remoto, incluindo uma nuvem privada ou outra região do Azure. Este espaço de endereços de proxy está vinculado a um gateway remoto para a rede de encaminhamento para os destinos de rede certos. Ver [configurar uma ligação VNet a VNet](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) para obter instruções sobre como estabelecer ligação de VNET a VNET.

Crie duas redes locais pelos seguintes detalhes:

| Nome de Rede | Endereço de Gateway VPN | Espaço de Endereços | Observações |
| --- | --- | --- | --- |
| HK-lnet-Map-to-East-US |23.1.1.1 |10.2.0.0/16 |Ao criar a rede Local, atribua um marcador de posição endereço de gateway. O endereço de real gateway é preenchido, uma vez criado o gateway. Certifique-se de que o espaço de endereços corresponde exatamente a VNET remota respectiva; Neste caso, a VNET criada na região E.U.A. Leste. |
| HK-lnet-Map-to-West-US |23.2.2.2 |10.1.0.0/16 |Ao criar a rede Local, atribua um marcador de posição endereço de gateway. O endereço de real gateway é preenchido, uma vez criado o gateway. Certifique-se de que o espaço de endereços corresponde exatamente a VNET remota respectiva; Neste caso, a VNET criada na região E.U.A. oeste. |

### <a name="step-3-map-local-network-to-the-respective-vnets"></a>Passo 3: Mapear a rede "Local" para as respetivas VNETs
No portal do Azure, selecione cada vnet, clique em "Configurar", marque "Ligar à rede local" e selecione as redes locais pelos seguintes detalhes:

| Rede Virtual | Rede local |
| --- | --- |
| HK-vnet---oeste |HK-lnet-Map-to-East-US |
| hk-vnet-east-us |HK-lnet-Map-to-West-US |

### <a name="step-4-create-gateways-on-vnet1-and-vnet2"></a>Passo 4: Criar Gateways na VNET1 e VNET2
No dashboard das redes virtuais, clique em criar o GATEWAY para acionar o processo de aprovisionamento de gateway de VPN. Após alguns minutos, o dashboard de cada rede virtual deverá apresentar o endereço de gateway real.

### <a name="step-5-update-local-networks-with-the-respective-gateway-addresses"></a>Passo 5: Atualizar redes "Local" com os respectivos endereços de "Gateway"
Edite as redes locais para substituir o endereço IP do gateway de marcador de posição pelo endereço IP real dos gateways apenas aprovisionados. Utilize o seguinte mapeamento:

<table>
<tr><th>Rede local    </th><th>Gateway de Rede Virtual</th></tr>
<tr><td>HK-lnet-Map-to-East-US </td><td>Gateway de hk-vnet---oeste</td></tr>
<tr><td>HK-lnet-Map-to-West-US </td><td>Gateway de hk-vnet-Leste-nós</td></tr>
</table>

### <a name="step-6-update-the-shared-key"></a>Passo 6: Atualizar a chave partilhada
Utilize o seguinte script do Powershell para atualizar a chave de IPSec de cada gateway de VPN [Utilize a chave de apenas porque precisam para ambos os gateways]: Conjunto AzureVNetGatewayKey - VNetName hk-vnet-Leste-nos - LocalNetworkSiteName hk-lnet-map-to-west-us - SharedKey hk-lnet-map-to-east-us de - LocalNetworkSiteName do D9E76BKK conjunto-AzureVNetGatewayKey - VNetName hk-vnet---oeste - SharedKey D9E76BKK

### <a name="step-7-establish-the-vnet-to-vnet-connection"></a>Passo 7: Estabelecer a ligação de VNET a VNET
No portal do Azure, utilize o menu de "DASHBOARD" das redes virtuais para estabelecer a ligação de gateway a gateway. Utilize os itens de menu "Ligar" na barra de ferramentas na parte inferior. Após alguns minutos o dashboard deverá apresentar os detalhes de ligação graficamente.

### <a name="step-8-create-the-virtual-machines-in-region-2"></a>Passo 8: Criar as máquinas virtuais na região #2
Criar a imagem de Ubuntu, conforme descrito na implementação de região #1 ao seguir as mesmas etapas ou copiar o ficheiro VHD da imagem para a conta de armazenamento do Azure localizado na região #2 e criar a imagem. Utilizar esta imagem e crie a seguinte lista de máquinas virtuais num novo serviço cloud hk-c-svc-Leste-nos:

| Nome do computador | Subrede | Endereço IP | Conjunto de disponibilidade | Controlador de domínio/bastidor | Efetuar o seeding? |
| --- | --- | --- | --- | --- | --- |
| hk-c1-east-us |dados |10.2.2.4 |hk-c-aset-1 |DC = EASTUS rack = rack1 |Sim |
| hk-c2-east-us |dados |10.2.2.5 |hk-c-aset-1 |DC = EASTUS rack = rack1 |Não |
| HK-c3-Leste-nos |dados |10.2.2.6 |hk-c-aset-1 |DC = EASTUS rack = rack2 |Sim |
| HK-c5-Leste-nos |dados |10.2.2.8 |hk-c-aset-2 |DC = EASTUS rack = rack3 |Sim |
| HK-c6-Leste-nos |dados |10.2.2.9 |hk-c-aset-2 |DC = EASTUS rack = rack3 |Não |
| HK-c7-Leste-nos |dados |10.2.2.10 |hk-c-aset-2 |DC = EASTUS rack = rack4 |Sim |
| hk-c8-east-us |dados |10.2.2.11 |hk-c-aset-2 |DC = EASTUS rack = rack4 |Não |
| HK-w1-Leste-nos |web |10.2.1.4 |hk-w-aset-1 |N/A |N/A |
| HK-w2-Leste-nos |web |10.2.1.5 |hk-w-aset-1 |N/A |N/A |

Siga as mesmas instruções como a região #1, mas usar o espaço de endereços de 10.2.xxx.xxx.

### <a name="step-9-configure-cassandra-on-each-vm"></a>Passo 9: Configurar o Cassandra em cada VM
Inicie sessão na VM e execute o seguinte:

1. Editar $CASS_HOME/conf/cassandra-rackdc.properties para especificar as propriedades de centro e suporte de dados no formato: dc = EASTUS rack = rack1
2. Edite cassandra.yaml para configurar nós de semente:  Sementes: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10,10.2.2.4,10.2.2.6,10.2.2.8,10.2.2.10"

### <a name="step-10-start-cassandra"></a>Passo 10: Iniciar o Cassandra
Inicie sessão em cada VM e inicie o Cassandra em segundo plano, executando o seguinte comando: $CASS_HOME/bin/cassandra

## <a name="test-the-multi-region-cluster"></a>Teste o Cluster de várias regiões
Já foi implementado Cassandra 16 nós com 8 nós em cada região do Azure. Estes nós estão no mesmo cluster que o nome comum do cluster e a configuração de nó de seed. Utilize o seguinte processo para testar o cluster:

### <a name="step-1-get-the-internal-load-balancer-ip-for-both-the-regions-using-powershell"></a>Passo 1: Obter o IP do Balanceador de carga interno para ambas as regiões com o PowerShell
* Get-AzureInternalLoadbalancer -ServiceName "hk-c-svc-west-us"
* Get-AzureInternalLoadbalancer -ServiceName "hk-c-svc-east-us"
  
    Tenha em atenção os endereços IP (para oeste de exemplo - 10.1.2.101 Leste - 10.2.2.101) apresentado.

### <a name="step-2-execute-the-following-in-the-west-region-after-logging-into-hk-w1-west-us"></a>Passo 2: Execute o seguinte na região Oeste depois de iniciar sessão em hk-w1---oeste
1. Executar $CASS_HOME/bin/cqlsh 10.1.2.101 9160
2. Execute os seguintes comandos CQL:
   
     Criar KEYSPACE customers_ks com replicação = {"classe": "NetworkToplogyStrategy', 'WESTUS': 3, "EASTUS": 3};   UTILIZAR customers_ks;   Criar tabela Customers(customer_id int PRIMARY KEY, firstname text, lastname text);   Inserir em Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   INSERT INTO o Customers(customer_id, firstname, lastname) VALUES (2, "Joana", "Doe");   SELECIONAR * de clientes;

Deverá ver uma exibição como o mostrado abaixo:

| customer_id | FirstName | Apelido |
| --- | --- | --- |
| 1 |John |Doe |
| 2 |Joana |Doe |

### <a name="step-3-execute-the-following-in-the-east-region-after-logging-into-hk-w1-east-us"></a>Passo 3: Execute o seguinte na região Leste depois de iniciar sessão em hk-w1-Leste-nos:
1. Executar $CASS_HOME/bin/cqlsh 10.2.2.101 9160
2. Execute os seguintes comandos CQL:
   
     UTILIZAR customers_ks;   Criar tabela Customers(customer_id int PRIMARY KEY, firstname text, lastname text);   Inserir em Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');   INSERT INTO o Customers(customer_id, firstname, lastname) VALUES (2, "Joana", "Doe");   SELECIONAR * de clientes;

Deverá ver a mesma exibição, conforme se verifica para a região Oeste:

| customer_id | FirstName | Apelido |
| --- | --- | --- |
| 1 |John |Doe |
| 2 |Joana |Doe |

Executar alguns inserções mais e ver que aqueles são replicados para o oeste-na parte do cluster.

## <a name="test-cassandra-cluster-from-nodejs"></a>Cluster do Cassandra de teste do node. js
Utilizar uma das VMs do Linux criado anteriormente na camada "web", executa um script de node. js simples para ler os dados anteriormente inseridos

**Passo 1: Instalar o node. js e o cliente de Cassandra**

1. Instalar node. js e npm
2. Instalar o pacote "cassandra-cliente do nó" utilizando npm
3. Execute o seguinte script na linha de comandos do shell que exibe a cadeia de caracteres do json dos dados recuperados:
    
    ```
    var pooledCon = require('cassandra-client').PooledConnection;
    var ksName = "custsupport_ks";
    var cfName = "customers_cf";
    var hostList = ['internal_loadbalancer_ip:9160'];
    var ksConOptions = { hosts: hostList,
                         keyspace: ksName, use_bigints: false };

    function createKeyspace(callback) {
        var cql = 'CREATE KEYSPACE ' + ksName + ' WITH strategy_class=SimpleStrategy AND strategy_options:replication_factor=1';
        var sysConOptions = { hosts: hostList,
                              keyspace: 'system', use_bigints: false };
        var con = new pooledCon(sysConOptions);
        con.execute(cql,[],function(err) {
            if (err) {
                console.log("Failed to create Keyspace: " + ksName);
                console.log(err);
            }
            else {
                console.log("Created Keyspace: " + ksName);
                callback(ksConOptions, populateCustomerData);
            }
        });
        con.shutdown();
    }

    function createColumnFamily(ksConOptions, callback) {
        var params = ['customers_cf','custid','varint','custname',
                      'text','custaddress','text'];
        var cql = 'CREATE COLUMNFAMILY ? (? ? PRIMARY KEY,? ?, ? ?)';
        var con =  new pooledCon(ksConOptions);
        con.execute(cql,params,function(err) {
            if (err) {
                console.log("Failed to create column family: " + params[0]);
                console.log(err);
            }
            else {
                console.log("Created column family: " + params[0]);
                callback();
            }
        });
        con.shutdown();
    }

    //populate Data
    function populateCustomerData() {
        var params = ['John','Infinity Dr, TX', 1];
        updateCustomer(ksConOptions,params);

        params = ['Tom','Fermat Ln, WA', 2];
        updateCustomer(ksConOptions,params);
    }

    //update also inserts the record if none exists
    function updateCustomer(ksConOptions,params) {
        var cql = 'UPDATE customers_cf SET custname=?,custaddress=? where custid=?';
        var con = new pooledCon(ksConOptions);
        con.execute(cql,params,function(err) {
            if (err) console.log(err);
            else console.log("Inserted customer : " + params[0]);
        });
        con.shutdown();
    }

    //read the two rows inserted above
    function readCustomer(ksConOptions) {
        var cql = 'SELECT * FROM customers_cf WHERE custid IN (1,2)';
        var con = new pooledCon(ksConOptions);
        con.execute(cql,[],function(err,rows) {
            if (err)
                console.log(err);
            else
                for (var i=0; i<rows.length; i++)
                    console.log(JSON.stringify(rows[i]));
            });
        con.shutdown();
    }

    //execute the code
    createKeyspace(createColumnFamily);
    readCustomer(ksConOptions)
    ```

## <a name="conclusion"></a>Conclusão
O Microsoft Azure é uma plataforma flexível que permite a execução de ambos Microsoft, bem como o software de código-fonte aberto, como demonstrado neste exercício. Clusters de elevada disponibilidade Cassandra podem ser implementados num único centro de dados por meio de propagação de nós do cluster em vários domínios de falha. Clusters de Cassandra também podem ser implantados em várias regiões do Azure, geograficamente distantes para sistemas de prova de desastres. Azure e Cassandra em conjunto permite a construção de elevada disponibilidade, altamente dimensionável e serviços de cloud recuperáveis do desastre necessitam, pela internet de hoje, dimensionar serviços.

## <a name="references"></a>Referências
* [http://cassandra.apache.org](http://cassandra.apache.org)
* [http://www.datastax.com](http://www.datastax.com)
* [http://www.nodejs.org](http://www.nodejs.org)
