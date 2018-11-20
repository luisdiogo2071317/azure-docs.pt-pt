---
title: Implementar o IBM Db2 pureScale no Azure
description: Saiba como implementar um [arquitetura de exemplo](ibm-db2-purescale-azure.md) utilizado recentemente para migrar de uma empresa de seu ambiente do IBM Db2 em execução no z/OS para o IBM Db2 pureScale no Azure.
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/09/2018
ms.author: njray
ms.openlocfilehash: 24bdc9867af869437cc0e440a80e5bf4813abbae
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/19/2018
ms.locfileid: "51977683"
---
# <a name="deploy-ibm-db2-purescale-on-azure"></a>Implementar o IBM Db2 pureScale no Azure

Este artigo descreve como implementar um [arquitetura de exemplo](ibm-db2-purescale-azure.md) utilizado recentemente para migrar de uma empresa de seu ambiente do IBM Db2 em execução no z/OS para o IBM Db2 pureScale no Azure.

Para seguir os passos utilizados durante a migração, veja os scripts de instalação na [Db2onAzure](http://aka.ms/db2onazure) repositório no GitHub. Estes scripts baseiam-se na arquitetura utilizada para cargas de trabalho típico, médias de transações online (OLTP) de processamento.

## <a name="get-started"></a>Introdução

Para implementar esta arquitetura, transfira e execute o script de deploy.sh encontrado no [Db2onAzure](http://aka.ms/db2onazure) repositório no GitHub.

O repositório também inclui scripts que pode utilizar para configurar um dashboard da Grafana que pode ser usado para consultar Prometheus, monitorização de código-fonte aberto e sistema de alerta acompanha o Db2.

> [!NOTE]
> O script de deploy.sh no cliente cria as chaves SSH privadas e passa para o modelo de implementação através de HTTPS. Para maior segurança, recomendamos que utilize [do Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) para armazenar segredos, chaves e palavras-passe.

## <a name="how-the-deployment-script-works"></a>Como funciona o script de implementação

O script de deploy.sh cria e configura os recursos do Azure que são utilizados nesta arquitetura. O script pede-lhe a subscrição do Azure e máquinas virtuais utilizadas no ambiente de destino e, em seguida, realiza as seguintes operações:

-   Configura o grupo de recursos, rede virtual e sub-redes no Azure para a instalação.

-   Configura os NSGs e o SSH para o ambiente.

-   Configura vários NICs sobre o GlusterFS e as máquinas de virtuais de pureScale Db2.

-   Cria as máquinas de virtuais de armazenamento GlusterFS.

-   Cria a máquina de virtual da jumpbox.

-   Cria as máquinas de virtuais de pureScale Db2.

-   Cria a máquina virtual de testemunho que pings de pureScale Db2.

-   Cria uma máquina virtual do Windows para utilizar em testes, mas não instala nada nele.

Em seguida, os scripts de implementação configurar o iSCSI rede de armazenamento virtual (vSAN) para armazenamento partilhado no Azure. Neste exemplo, o iSCSI se conecta ao GlusterFS. Esta solução também lhe dá a opção para instalar os destinos iSCSI como um único nó do Windows. (o iSCSI fornece uma interface de armazenamento de blocos partilhado por TCP/IP, que permite que o procedimento de configuração de pureScale Db2 utilizar uma interface de dispositivo para ligar ao armazenamento partilhado.) Para os princípios básicos de GlusterFS, consulte a [arquitetura: tipos de volumes](https://docs.gluster.org/en/latest/Quick-Start-Guide/Architecture/) GlusterFS de introdução ao tópico na introdução.

Os scripts de implantação, execute estes passos gerais:

1.  Configure um cluster de armazenamento partilhado no Azure. GlusterFS é utilizado para configurar o cluster de armazenamento partilhado. Isso envolve a, pelo menos, dois nós do Linux. Para obter detalhes de configuração, consulte [configuração do Red Hat Gluster Storage no Microsoft Azure](https://access.redhat.com/documentation/en-us/red_hat_gluster_storage/3.1/html/deployment_guide_for_public_cloud/chap-documentation-deployment_guide_for_public_cloud-azure-setting_up_rhgs_azure) na documentação do Red Hat Gluster.

2.  Configure uma interface de iSCSI direto nos servidores do Linux de destino para GlusterFS. Para obter detalhes de configuração, consulte [GlusterFS iSCSI](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/) no guia de administração de GlusterFS.

3.  Configure o Iniciador do iSCSI nas máquinas virtuais de Linux que acederá cluster GlusterFS através de destino iSCSI. Para obter detalhes de configuração, consulte a [como configurar um iSCSI Target e iniciador no Linux](https://www.rootusers.com/how-to-configure-an-iscsi-target-and-initiator-in-linux/) na documentação do RootUsers.

4.  Instala GlusterFS como a camada de armazenamento para a interface de iSCSI.

Depois de criar o dispositivo de iSCSI, a etapa final é instalar Db2 pureScale. Como parte da configuração de pureScale Db2, [IBM espectro dimensionamento](https://www.ibm.com/support/knowledgecenter/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0057167.html) (anteriormente conhecido como GPFS) é compilado e instalado no GlusterFS cluster. Este sistema de ficheiros em cluster permite pureScale Db2 partilhar dados entre as várias máquinas virtuais que executam o mecanismo de pureScale Db2. Para obter mais informações, consulte a [IBM espectro dimensionamento](https://www.ibm.com/support/knowledgecenter/en/STXKQY_4.2.0/ibmspectrumscale42_welcome.html) documentação no site da IBM.

## <a name="db2-purescale-response-file"></a>Arquivo de resposta de pureScale de Db2

O repositório do GitHub inclui Db2server.rsp, um arquivo de resposta (.rsp) que permite-lhe gerar um script automatizado para a instalação de pureScale Db2. A tabela seguinte lista as opções de pureScale Db2 que o arquivo de resposta utiliza para a configuração. Pode personalizar o ficheiro de resposta, conforme necessário para o seu ambiente de instalação.

> [!NOTE]
> Um ficheiro de resposta de exemplo, Db2server.rsp, está incluído nos [Db2onAzure](http://aka.ms/db2onazure) repositório no GitHub. Se usar esse arquivo, tem de editá-lo antes de poder funcionar no seu ambiente.

**Opções de ficheiro de resposta de pureScale de Db2**

| Nome do ecrã               | Campo                                        | Valor                                                                                                 |
|---------------------------|----------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Bem-vindo                   |                                              | Nova instalação                                                                                           |
| Escolha um produto          |                                              | Versão de Db2 11.1.3.3. Edições de servidor com pureScale de Db2                                              |
| Configuração             | Diretório                                    | /Data1/OPT/IBM/DB2/V11.1                                                                              |
|                           | Selecione o tipo de instalação                 | Típico                                                                                               |
|                           | Eu concordo com os termos da IBM                     | Assinalado                                                                                               |
| Proprietário da instância            | Instância de existente For de utilizador, nome de utilizador        | Db2sdin1                                                                                              |
| Utilizador bloqueado               | Utilizador existente, nome de utilizador                     | Db2sdfe1                                                                                              |
| Sistema de ficheiros do cluster       | Partilhado o caminho do dispositivo de partição de disco            | /Dev/DM-2                                                                                             |
|                           | Ponto de montagem                                  | / Db2sd\_1804a                                                                                         |
|                           | Partilhado o disco de dados                         | /Dev/DM-1                                                                                             |
|                           | Ponto de montagem (dados)                           | / Db2fs/datafs1                                                                                        |
|                           | Disco para o registo de partilhado                          | /Dev/DM-0                                                                                             |
|                           | Ponto de montagem (registo)                            | / Db2fs/logfs1                                                                                         |
|                           | Db2 Desempatador de serviços de Cluster. Caminho do dispositivo | /Dev/DM-3                                                                                             |
| Lista de anfitriões                 | D1 [eth1], d2 [eth1], cf1 [eth1], cf2 [eth1] |                                                                                                       |
|                           | CF primário preferencial                         | cf1                                                                                                   |
|                           | Preferencial CF secundário                       | cf2                                                                                                   |
| Arquivo de resposta e o resumo | primeira opção                                 | Instale o Db2 Server Edition com a funcionalidade de pureScale IBM Db2 e guardar as minhas configurações num arquivo de resposta |
|                           | Nome de ficheiro de resposta                           | /root/DB2server.rsp                                                                                   |

### <a name="notes-about-this-deployment"></a>Notas sobre esta implementação

-   Os valores para /dev-dm0, /dev-dm1, /dev-dm2 e /dev-dm3 podem alterar após um reinício na máquina virtual onde ocorre a configuração (d0 no script automatizado). Para localizar os valores corretos, pode emitir o comando seguinte antes de concluir o arquivo de resposta no servidor onde vai ser executada a configuração:

```
   [root\@d0 rhel]\# ls -als /dev/mapper
   total 0
   0 drwxr-xr-x 2 root root 140 May 30 11:07 .
   0 drwxr-xr-x 19 root root 4060 May 30 11:31 ..
   0 crw------- 1 root root 10, 236 May 30 11:04 control
   0 lrwxrwxrwx 1 root root 7 May 30 11:07 db2data1 -\> ../dm-1
   0 lrwxrwxrwx 1 root root 7 May 30 11:07 db2log1 -\> ../dm-0
   0 lrwxrwxrwx 1 root root 7 May 30 11:26 db2shared -\> ../dm-2
   0 lrwxrwxrwx 1 root root 7 May 30 11:08 db2tieb -\> ../dm-3
```

-   Os scripts de configuração utilizam aliases para os discos iSCSI para que os nomes reais podem ser encontrados facilmente.

-   Quando o script de configuração é executado num d0, o **/desenvolvimento/dm -\***  valores podem ser diferentes em d1, cf0 e cf1. A configuração de pureScale Db2 não se importa.

## <a name="troubleshooting-and-known-issues"></a>Problemas conhecidos e resolução de problemas

O repositório do GitHub inclui uma Base de dados de conhecimento mantidos pelos autores. Ele apresenta uma lista de possíveis problemas que poderá encontrar e resoluções que pode experimentar. Por exemplo, conhecido podem ocorrer problemas quando:

-   A tentar alcançar o endereço IP do gateway.

-   Compilar GPL.

-   Falha no handshake de segurança entre anfitriões.

-   O instalador de Db2 Deteta um sistema de ficheiros existentes.

-   Instalar manualmente o GPFS.

-   A instalar o Db2 pureScale quando GPFS já foi criado.

-   A remover Db2 pureScale e GPFS.

Para obter mais informações sobre esses e outros problemas conhecidos, consulte o arquivo de kb.md no [Db2onAzure](http://aka.ms/Db2onAzure) repositório.

## <a name="next-steps"></a>Passos Seguintes

-   [GlusterFS iSCSI](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/)

-   [Criação de usuários necessários para uma instalação da funcionalidade de pureScale de Db2](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0055374.html?pos=2)

-   [Db2icrt - criar instância de comando](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.cmd.doc/doc/r0002057.html)

-   [Db2 pureScale solução de dados de Clusters](http://www.ibmbigdatahub.com/blog/db2-purescale-clustered-database-solution-part-1)

-   [Dados do IBM Studio](https://www.ibm.com/developerworks/downloads/im/data/index.html/)

-   [Alliance de modernização de plataforma: IBM Db2 no Azure](https://www.platformmodernization.org/pages/ibmdb2azure.aspx)

-   [Datacenter Virtual do Azure Lift- and -Shift guia](https://azure.microsoft.com/resources/azure-virtual-datacenter-lift-and-shift-guide/)
