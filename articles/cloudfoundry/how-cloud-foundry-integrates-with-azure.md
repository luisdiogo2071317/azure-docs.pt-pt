---
title: Como nuvem Foundry integra-se com o Azure | Microsoft Docs
description: Descreve como serviços de nuvem Foundry pode utlize do Azure para melhorar a experiência de Enterprice
services: virtual-machines-linux
documentationcenter: ''
author: ningk
manager: jeconnoc
editor: ''
tags: Cloud-Foundry
ms.assetid: 00c76c49-3738-494b-b70d-344d8efc0853
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2018
ms.author: ningk
ms.openlocfilehash: 1f4afbe1849210c55c392d014449224f2fe97b04
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655465"
---
# <a name="integrate-cloud-foundry-with-azure"></a>Integrar Foundry de nuvem com o Azure

[Nuvem Foundry](https://docs.cloudfoundry.org/) é uma plataforma de PaaS em execução em cima da plataforma de IaaS de fornecedores de nuvem. Oferece a experiência de implementação de aplicação consistente através de fornecedores de nuvem. Além disso, também pode integrar com vários serviços do Azure, com o nível de enterprise HA, escalabilidade e reduções de custos.
Existem [6 subsistemas de nuvem Foundry](https://docs.cloudfoundry.org/concepts/architecture/), que pode ser flexivelmente escala online, incluindo: encaminhamento, a autenticação, gestão de ciclo de vida de aplicações, serviço de gestão, monitorização e de mensagens. Para cada um dos subsistemas, pode configurar a nuvem Foundry utilizar correspondent serviço do Azure. 

![Nuvem Foundry na arquitetura de integração do Azure](media/CFOnAzureEcosystem-colored.png)

## <a name="1-high-availability-and-scalability"></a>1. Elevada disponibilidade e escalabilidade
### <a name="managed-disk"></a>Disco gerido
Bosh utiliza a Azure CPI (Interface do fornecedor de nuvem) para criar o disco e rotinas de eliminação. Por predefinição, são utilizados discos não geridos. Requer o cliente manualmente criar contas de armazenamento, e em seguida, configure as contas de nos ficheiros de manifesto CF. Isto acontece devido à limitação do número de discos por conta de armazenamento.
Agora [geridos disco](https://azure.microsoft.com/services/managed-disks/) estiver disponível, oferece um armazenamento de disco gerido segura e fiável para máquinas virtuais. Cliente já não precisam de lidar com a conta de armazenamento para escala e HA. Azure dispõe discos automaticamente. Se se trata de uma nova ou uma implementação existente, o Azure CPI irá processar a criação ou migração de disco gerido durante uma implementação de CF. É suportada com PCF 1.11. Também pode explorar o Foundry de nuvem de open source [orientações sobre o disco geridos](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/managed-disks) para referência. 
### <a name="availability-zone-"></a>Zona de disponibilidade *
Como uma plataforma de aplicação nativa na nuvem, Foundry de nuvem é concebida com [quatro nível de elevada disponibilidade](https://docs.pivotal.io/pivotalcf/2-1/concepts/high-availability.html). Enquanto os primeiros três níveis de falhas de software podem ser processados pelo sistema CF próprio, tolerância a falhas plataforma é fornecida pelos fornecedores de nuvem. Os componentes de CF chave devem ser protegidos com a plataforma de um fornecedor de nuvem solução HA. Isto inclui GoRouters, Diego Brains, CF mosaicos de base de dados e o serviço. Por predefinição, [do conjunto de disponibilidade do Azure](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/deploy-cloudfoundry-with-availability-sets) é utilizado para tolerância a falhas entre clusters num centro de dados.
É boa novo, [zona de disponibilidade do Azure](https://docs.microsoft.com/azure/availability-zones/az-overview ) é libertado agora, levam a tolerância a falhas ao nível seguinte, com redundância de latência baixa em todos os centros de dados.
Zona de disponibilidade do Azure distribui HA colocando a um conjunto de VMs em centros de dados 2 +, cada conjunto de VMs são redundante para outros conjuntos. Se uma zona estiver desativado, os outros conjuntos são ainda em direto, isolada de desastre.
> [!NOTE] 
> Zona de disponibilidade do Azure não é oferecida para todas as regiões ainda, verifique a versão mais recente [anúncio para ver a lista de regiões suportadas](https://docs.microsoft.com/azure/availability-zones/az-overview). Para abrir Foundry de nuvem de origem, verifique [zona de disponibilidade do Azure para obter orientações de nuvem Foundry open source para](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/availability-zone).

## <a name="2-network-routing"></a>2. Encaminhamento de rede
Por predefinição, o Balanceador de carga básico do Azure é utilizado para receber pedidos de aplicações e API CF, reenviando-os para o Gorouters. Componentes de CF como Diego "-brain", MySQL, ERT também pode utilizar o Balanceador de carga para equilibrar o tráfego para HA. Além disso, o Azure oferece um conjunto de soluções de balanceamento de carga completamente gerido. Se estiver à procura para a terminação de TLS ("descarga de SSL") ou pelo processamento de camada de aplicação de pedidos HTTP/HTTPS, considere o Gateway de aplicação. Para elevada disponibilidade e na camada 4 de balanceamento de carga de escalabilidade, considere o Balanceador de carga padrão.
### <a name="azure-application-gateway-"></a>Gateway de aplicação do Azure *
[Gateway de aplicação do Azure](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) oferece capacidades, incluindo SSL descarregar, ponto a ponto SSL, Firewall de aplicações Web, afinidade de sessão com base em cookies e muito mais de balanceamento de carga de camada 7 vários. Pode [configurar o Gateway de aplicação no abra Foundry de nuvem de origem](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/application-gateway). Para PCF, verifique o [notas de versão PCF 2.1](https://docs.pivotal.io/pivotalcf/2-1/pcf-release-notes/opsmanager-rn.html#azure-application-gateway) para teste POC.

### <a name="azure-standard-load-balancer-"></a>Balanceador de carga Standard do Azure *
Balanceador de carga do Azure é um balanceador de carga de camada 4. É utilizado para distribuir o tráfego entre instâncias dos serviços de um conjunto com balanceamento de carga. A versão padrão fornece [funcionalidades avançadas](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) sobre a versão básica. Por exemplo, 1. O limite máximo do conjunto de back-end é gerado de 100 para VMs de 1000.  2. Os pontos finais agora suportam vários conjuntos de disponibilidade em vez de conjunto de disponibilidade único.  3. Funcionalidades adicionais, como portas HA, dados de monitorização mais rico, etc. Se estiver a mover a zona de disponibilidade do Azure, o Balanceador de carga padrão é necessário. Para uma nova implementação, recomendamos que comece com Balanceador de carga do Azure Standard. 

## <a name="3-authentication"></a>3. Autenticação 
[Nuvem Foundry conta de utilizador e a autenticação](https://docs.cloudfoundry.org/concepts/architecture/uaa.html) é o serviço de gestão de identidade central para CF e os respetivos vários componentes. [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) é baseada na cloud, multi-inquilino diretório e identidade do serviço de gestão da Microsoft. Por predefinição, UAA é utilizado para autenticação de Foundry de nuvem. Como uma opção avançada, UAA também suporta Azure AD como um arquivo de utilizador externo. Os utilizadores do AD do Azure podem aceder a Foundry de nuvem utilizando a identidade LDAP, sem uma conta de Foundry de nuvem. Siga estes passos para [configurar o Azure AD para UAA no PCF](http://docs.pivotal.io/p-identity/1-6/azure/index.html).

## <a name="4-data-storage-for-cloud-foundry-runtime-system"></a>4. Armazenamento de dados para o sistema de tempo de execução do Cloud Foundry
Nuvem Foundry oferece extensibilidade excelente utilizar blobstore do Azure ou serviços do Azure. o MySQL/PostgreSQL para armazenamento do sistema de runtime de aplicação.
### <a name="azure-blobstore-for-cloud-foundry-cloud-controller-blobstore"></a>Blobstore do Azure para o controlador de nuvem do Cloud Foundry blobstore
O controlador de nuvem blobstore é um arquivo de dados críticos para buildpacks, droplets, pacotes e agrupamentos de recursos. Por predefinição, o servidor NFS é utilizado para blobstore de controlador de nuvem. Para evitar ponto único de falha, utilize o Blob Storage do Azure como armazenamento externo. Veja o [documentação de nuvem Foundry](https://docs.cloudfoundry.org/deploying/common/cc-blobstore-config.html) de segundo plano, e [opções no Pivotal nuvem Foundry](https://docs.pivotal.io/pivotalcf/2-0/customizing/azure.html).

### <a name="mysqlpostgresql-as-cloud-foundry-elastic-run-time-database-"></a>Base de dados do tempo de execução de MySQL/PostgreSQL como nuvem Foundry elástico *
Tempo de execução elástico CF requer duas bases de dados do sistema principais:
#### <a name="ccdb"></a>CCDB 
A base de dados do controlador de nuvem.  Controlador de nuvem fornece pontos finais da REST API para os clientes aceder ao sistema. CCDB armazena as tabelas para organizações do, espaços, serviços, as funções de utilizador e mais para o controlador de nuvem.
#### <a name="uaadb"></a>UAADB 
A base de dados para a conta de utilizador e a autenticação. Armazena a autenticação de utilizador relacionadas com os dados, por exemplo encriptados nomes de utilizador e palavras-passe.

Por predefinição, uma base de dados do sistema local (MySQL) pode ser utilizado. Para HA e escala, tire partido do Azure MySQL gerido ou serviços PostgreSQL. Segue-se a instrução de [ativar Azure MySQL/PostgreSQL CCDB, UAADB e outras bases de dados do sistema com abra Foundry de nuvem de origem](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/configure-cf-external-databases-using-azure-mysql-postgres-service).

## <a name="5-open-service-broker"></a>5. Mediador de serviço aberto
Mediador de serviço do Azure oferece uma interface consistente para gerir o acesso da aplicação aos serviços do Azure. A nova [aberta do Service Broker para o projeto do Azure](https://github.com/Azure/open-service-broker-azure) fornece uma forma simple e única para fornecer serviços de aplicações em nuvem Foundry, OpenShift e Kubernetes. Consulte o [Azure aberta do Service Broker para mosaico PCF](https://network.pivotal.io/products/azure-open-service-broker-pcf/) para obter instruções de implementação no PCF.

## <a name="6-metrics-and-logging"></a>6. Métricas e registo
Nozzle de análise de registos do Azure é um componente de Foundry de nuvem, que reencaminha as métricas do [nuvem Foundry loggregator firehose](https://docs.cloudfoundry.org/loggregator/architecture.html) para [Log Analytics do Azure](https://azure.microsoft.com/services/log-analytics/). Com o Nozzle, pode recolher, ver e analisar as métricas de desempenho e estado de funcionamento de sistema do CF em múltiplas implementações.
Clique em [aqui](https://docs.microsoft.com/azure/cloudfoundry/cloudfoundry-oms-nozzle) para saber como implementar o Nozzle de análise do registo do Azure no Open Source e ambiente Pivotal Foundry de nuvem e, em seguida, aceder os dados a partir da consola de análise de registos do Azure OMS. 
> [!NOTE]
> A partir da PCF 2.0, métricas de estado de funcionamento BOSH para VMs são reencaminhadas para o Loggregator Firehose por predefinição e estão integradas na consola de análise de registos do Azure OMS.

## <a name="7-cost-saving"></a>7. Custo guardar
### <a name="cost-saving-for-devtest-environments"></a>Custo guardar para ambientes de desenvolvimento/teste
#### <a name="b-series-"></a>Série B: *
Apesar de série F e D VM foram normalmente recomendado para o ambiente de produção Pivotal Foundry de nuvem, o novo "burstable" [B-série](https://azure.microsoft.com/blog/introducing-b-series-our-new-burstable-vm-size/) proporciona novas opções. As VMs burstable B-série são ideais para cargas de trabalho que não tem o desempenho total da CPU continuamente, como servidores web, desenvolvimento de bases de dados pequenas e ambientes de teste. Estas cargas de trabalho normalmente têm requisitos de desempenho burstable. É $0.012/ hora (B1) em comparação com $ 0,05/hora (F1), consulte a lista completa de [tamanhos de VM](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-general) e [os preços](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) para obter mais detalhes. 
#### <a name="managed-standard-disk"></a>Disco gerido de padrão: 
Os discos Premium foram recomendados para desempenho fiável na produção.  Com [geridos disco](https://azure.microsoft.com/services/managed-disks/), armazenamento standard pode também fornecer fiabilidade semelhante, com um desempenho diferentes. Para a carga de trabalho que não é sensível de desempenho, como o ambiente não críticos ou de programador/teste, discos padrão geridos oferecem uma opção alternativa com custo mais baixo.  
### <a name="cost-saving-in-general"></a>Custo guardar em geral 
#### <a name="significant-vm-cost-saving-with-reserved-instances"></a>Custo VM significativas guardar com instâncias reservadas: 
Hoje que todas as VMs de CF são faturadas com preços de "a pedido", apesar dos ambientes, normalmente, manter cópias de segurança indefinidamente. Agora pode reservar capacidade de VM num termo de 1 ou 3 anos e obter descontos de 45 65%. Descontos são aplicados no sistema de faturação, sem alterações ao seu ambiente. Para obter mais informações, consulte [reservado como funciona a instâncias](https://azure.microsoft.com/pricing/reserved-vm-instances/). 
#### <a name="managed-premium-disk-with-smaller-sizes"></a>Gerido Premium disco com o tamanho mais pequeno: 
Gerido discos suporte disco menor, por exemplo, P4(32 GB) e P6(64 GB) para discos standard e premium. Se tiver pequenas cargas de trabalho, pode poupar custos quando migrar do discos standard premium para os discos premium gerido.
#### <a name="utilizing-azure-first-party-services"></a>Utilizar primeira intervenientes dos serviços do Azure: 
Tirar partido do serviço de terceiros primeiro do Azure irá reduzir a administração de longa duração de custos, além de HA e fiabilidade mencionado em secções. 

Pivotal foi iniciada uma [pequenos requisitos de espaço ERT](https://docs.pivotal.io/pivotalcf/2-0/customizing/small-footprint.html) para clientes PCF, os componentes estão colocalizados no apenas 4 VMs, até 2500 instâncias de aplicações em execução. A versão de avaliação já está disponível através de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry).

## <a name="next-steps"></a>Próximos Passos
Funcionalidades de integração do Azure pela primeira vez estão disponíveis com [abra Foundry de nuvem de origem](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs/advanced/), antes de ficarem disponível no Pivotal Foundry de nuvem. Funcionalidades marcado com * ainda não estão disponíveis através de PCF. Nuvem Foundry integração com a pilha do Azure não é abrangida neste documento:.
Para suporte PCF das funcionalidades marcados com *, ou integração de nuvem Foundry com pilha do Azure, contacte o seu Gestor de conta Pivotal e da Microsoft para o estado mais recente. 

