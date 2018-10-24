---
title: Principais funcionalidades e conceitos no Azure Stack | Documentos da Microsoft
description: Saiba mais sobre as principais funcionalidades e conceitos no Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 09ca32b7-0e81-4a27-a6cc-0ba90441d097
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/23/2018
ms.author: jeffgilb
ms.reviewer: ''
ms.openlocfilehash: 38120b2be2ab7789946a4ad2fe688954e6212189
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49959031"
---
# <a name="key-features-and-concepts-in-azure-stack"></a>Principais funcionalidades e conceitos no Azure Stack
Se estiver familiarizado com o Microsoft Azure Stack, esses termos e as descrições de recursos poderão ser útil.

## <a name="personas"></a>Pessoas fictícias
Existem duas variedades de utilizadores para o Microsoft Azure Stack, o operador da nuvem (fornecedor) e de inquilino (consumidor).

* R **operador da nuvem** pode configurar o Azure Stack e gerir ofertas, planos, serviços, quotas e preços para fornecer recursos para os seus inquilinos.  Operadores da nuvem também capacidade de gerir e respondem a alertas.  
* R **inquilino** (também referido como um utilizador) consome serviços de que o administrador da nuvem oferece. Os inquilinos podem aprovisionar, monitorizar e gerir os serviços subscreveu, tais como aplicações Web, armazenamento e máquinas virtuais.

## <a name="portal"></a>Portal
Os principais métodos de interação com o Microsoft Azure Stack são o portal de administrador, o portal de utilizador e o PowerShell.

Os portais do Azure Stack são apoiados cada por instâncias separadas do Gestor de recursos do Azure.  Um operador da cloud utiliza o portal de administrador para gerir o Azure Stack e para fazer coisas como criar ofertas de inquilino.  O portal de utilizador (também referido como o portal de inquilinos) fornece uma experiência self-service para consumo de recursos de nuvem, como máquinas virtuais, contas de armazenamento e aplicações Web. Para obter mais informações, consulte [utilizar os portais de administrador e de utilizador do Azure Stack](azure-stack-manage-portals.md).

## <a name="identity"></a>Identidade 
O Azure Stack utiliza o Azure Active Directory (AAD) ou serviços de Federação do Active Directory (AD FS) como um fornecedor de identidade.  

### <a name="azure-active-directory"></a>Azure Active Directory
O Azure Active Directory é o fornecedor de identidade baseada na cloud, multi-inquilino da Microsoft.  A maioria dos cenários híbridos, os utilizam o Azure Active Directory como o repositório de identidades.

### <a name="active-directory-federation-services"></a>Serviços de Federação do Active Directory
Pode optar por usar serviços de Federação do Active Directory (AD FS) para implementações desconectadas do Azure Stack.  O Azure Stack, fornecedores de recursos e outras aplicações funcionam quase da mesma forma com o AD FS, tal como com o Azure Active Directory. O Azure Stack inclui a sua própria instância do AD FS e o Active Directory e uma API de gráfico do Active Directory. Kit de desenvolvimento do Azure Stack suporta os seguintes cenários do AD FS:

- Inicie sessão para a implementação com o AD FS.
- Criar uma máquina virtual com segredos no Key Vault
- Criar um cofre para armazenar/acesso a segredos
- Criar funções RBAC personalizadas na subscrição
- Atribuir utilizadores a funções RBAC em recursos
- Criar funções RBAC de todo o sistema através do PowerShell do Azure
- Inicie sessão como um utilizador através do PowerShell do Azure
- Criar serviço principais de usá-los para iniciar sessão no Azure PowerShell


## <a name="regions-services-plans-offers-and-subscriptions"></a>Regiões, serviços, planos, ofertas e subscrições
No Azure Stack, os serviços são entregues para inquilinos que utilizam as regiões, subscrições, ofertas e planos. Os inquilinos podem subscrever várias ofertas. Ofertas podem ter um ou mais planos e planos podem ter um ou mais serviços.

![](media/azure-stack-key-features/image4.png)

Hierarquia de exemplo das subscrições de um inquilino para as ofertas, cada uma com diferentes planos e serviços.

### <a name="regions"></a>Regiões
Regiões de pilha do Azure são um elemento básico de dimensionamento e gestão. Uma organização possa ter várias regiões com recursos disponíveis em cada região. Regiões podem também ter ofertas de serviço diferentes disponíveis. No Azure Stack Development Kit, é suportada apenas uma única região e automaticamente o nome *local*.

### <a name="services"></a>Serviços
Microsoft Azure Stack permite que os fornecedores fornecer uma ampla variedade de serviços e aplicações, como máquinas virtuais, do SQL Server bases de dados, SharePoint, Exchange e muito mais.

### <a name="plans"></a>Planos
Planos consistem em agrupamentos de um ou mais serviços. Como um fornecedor, criar planos para oferecer aos seus inquilinos. Por sua vez, os seus inquilinos subscrevem as suas ofertas para utilizar os planos e os serviços incluídos.

Cada serviço adicionado a um plano pode ser configurado com as definições de quota para o ajudar a gerir a capacidade de nuvem. As quotas podem incluir restrições, como limites VM, RAM e da CPU e são aplicadas por subscrição do utilizador. As quotas podem ser diferenciadas por localização. Por exemplo, um plano de serviços de computação de região A poderia ter uma quota de duas máquinas virtuais, 4 GB de RAM e 10 núcleos de CPU.

Ao criar uma oferta, o administrador de serviços pode incluir uma **plano base**. Estes planos bases estão incluídos por predefinição, quando um inquilino subscreve essa oferta. Quando um utilizador subscrever (e a subscrição é criada), o utilizador tem acesso a todos os fornecedores de recursos especificado nesses planos base (com as quotas de correspondentes).

O administrador de serviços também pode incluir **planos de suplementos** numa oferta. Planos de suplementos não estão incluídos por predefinição na subscrição. Planos de suplementos são planos adicionais (cotas) disponíveis numa oferta que um proprietário da subscrição pode adicionar a suas assinaturas.

### <a name="offers"></a>Ofertas
As ofertas são grupos de um ou mais planos que os fornecedores apresentam aos inquilinos para comprarem (subscrever). Por exemplo, oferecem Alpha pode conter o plano uma contendo um conjunto de serviços de computação e o plano B, que contém um conjunto de serviços de armazenamento e rede.

Uma oferta inclui um conjunto de planos base e os administradores de serviço podem criar planos de suplementos que os inquilinos podem adicionar à sua subscrição.

### <a name="subscriptions"></a>Subscrições
Uma subscrição é como os inquilinos comprar suas ofertas. Uma subscrição é uma combinação de um inquilino com uma oferta. Um inquilino pode ter subscrições para várias ofertas. Cada subscrição se aplica a apenas uma oferta. Subscrições de um inquilino determinam quais os planos/serviços que podem aceder.

As assinaturas ajudam a fornecedores de organizar e aceder a recursos na cloud e serviços.

Para o administrador, uma subscrição do fornecedor predefinida é criada durante a implementação. Esta subscrição pode ser utilizada para gerir o Azure Stack, ainda mais implementar fornecedores de recursos e criar planos e ofertas para os inquilinos. Não deve ser usado para executar aplicações e cargas de trabalho do cliente. A partir da versão 1804, duas subscrições adicionais complementam a subscrição do fornecedor predefinido; uma subscrição de medição e uma subscrição de consumo. Essas adições de facilitam a separar a gestão da infraestrutura básica, fornecedores de recursos adicionais e cargas de trabalho.  

## <a name="azure-resource-manager"></a>Azure Resource Manager
Ao utilizar o Azure Resource Manager, pode trabalhar com os seus recursos de infraestrutura num modelo declarativo, com base no modelo.   Ele fornece uma interface única que pode utilizar para implementar e gerir os componentes da solução. Para informações completas e orientações, veja a [descrição geral do Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

### <a name="resource-groups"></a>Grupos de recursos
Grupos de recursos são coleções de recursos, serviços e aplicações — e cada recurso tem um tipo, como máquinas virtuais, redes virtuais, IPs público, contas de armazenamento e Web sites. Cada recurso tem de estar num grupo de recursos e para que os grupos de recursos ajudam a logicamente organizar recursos, como por carga de trabalho ou localização.  No Microsoft Azure Stack, recursos, tais como os planos e ofertas também são geridos em grupos de recursos.

Ao contrário [Azure](../azure-resource-manager/resource-group-move-resources.md), não é possível mover recursos entre grupos de recursos. Quando visualizar as propriedades de um recurso ou grupo de recursos no portal de administração do Azure Stack, o *mover* botão está desativado e indisponível. 
 
### <a name="azure-resource-manager-templates"></a>Modelos do Azure Resource Manager
Com o Azure Resource Manager, pode criar um modelo (no formato JSON) que define a implementação e configuração da sua aplicação. Este modelo é conhecido como um modelo Azure Resource Manager e fornece uma forma declarativa de definir a implementação. Ao utilizar um modelo, pode implementar repetidamente a aplicação durante todo o ciclo de vida da mesma e ter a confiança de que os recursos são implementados num estado consistente.

## <a name="resource-providers-rps"></a>Fornecedores de recursos (RPs)
Fornecedores de recursos são serviços da web que formam a base para todos os IaaS baseada no Azure e serviços PaaS. O Azure Resource Manager conta com RPs diferentes para fornecer acesso aos serviços.

Existem quatro RPs fundamentais: rede, armazenamento, computação e o Cofre de chaves. Cada um desses RPs ajuda-o a configurar e controlar os respetivos recursos. Os administradores de serviços também podem adicionar novos fornecedores de recursos personalizado.

### <a name="compute-rp"></a>Computação RP
O fornecedor de recursos de computação (CRP) permite que os inquilinos do Azure Stack criar suas próprias máquinas virtuais. O CRP inclui a capacidade de criar máquinas virtuais, bem como as extensões de Máquina Virtual. O serviço de extensão de Máquina Virtual ajuda a fornecer recursos de IaaS para máquinas virtuais Windows e Linux.  Por exemplo, pode utilizar o CRP para Aprovisionar uma máquina virtual do Linux e executar scripts de Bash durante a implementação para configurar a VM.

### <a name="network-rp"></a>Rede RP
O fornecedor de recursos de rede (NRP) oferece uma série de recursos definida pelo Software (SDN) e Virtualização de função de rede (NFV) para a nuvem privada.  Pode utilizar o NRP para criar recursos como software carga balanceadores, IPs públicos, grupos de segurança, redes virtuais.

### <a name="storage-rp"></a>Armazenamento RP
A RP de armazenamento fornece quatro serviços consistentes do Azure de armazenamento: blob, tabela, fila e gestão de contas. Ele também oferece um serviço de administração do armazenamento na cloud para facilitar a administração de fornecedor de serviço dos serviços de armazenamento do Azure consistente. O armazenamento do Azure fornece a flexibilidade para armazenar e obter grandes quantidades de dados não estruturados, como documentos e ficheiros de multimédia com Blobs do Azure, e NoSQL estruturado com base em dados com as tabelas do Azure. Para obter mais informações sobre o armazenamento do Azure, consulte [introdução ao armazenamento do Microsoft Azure](../storage/common/storage-introduction.md).

#### <a name="blob-storage"></a>Armazenamento de blobs
Armazenamento de BLOBs armazena qualquer conjunto de dados. Um blob pode ser qualquer tipo de texto ou de dados binários, tal como um documento, um ficheiro multimédia ou o instalador da aplicação. O Table storage armazena conjuntos de dados estruturados. O Table Storage é um arquivo de dados do atributo chave NoSQL, que permite um rápido desenvolvimento e um acesso rápido às grandes quantidades de dados. Armazenamento de filas fornece mensagens fiáveis para processamento de fluxo de trabalho e para a comunicação entre os componentes dos serviços cloud.

Cada blob está organizado num contêiner. Os contentores também fornecem uma forma útil de atribuir políticas de segurança a grupos de objetos. Uma conta de armazenamento pode conter qualquer número de contentores e um contentor pode conter qualquer número de blobs, até ao limite de 500 TB de capacidade da conta de armazenamento. O Blob Storage oferece três tipos de blobs: blobs de blocos, blobs de acréscimo e blobs de página (discos). Os blobs de blocos estão otimizados para transmitir em fluxo e armazenar os objetos da nuvem e são uma boa opção para armazenar documentos, ficheiros multimédia, cópias de segurança, etc. Os blobs de acréscimo são semelhantes aos blobs de blocos, mas estão otimizados para as operações de acréscimo. Um blob de acréscimo apenas pode ser atualizado adicionando um novo bloco ao final. Os blobs de acréscimo são uma boa opção para cenários como o registo, onde novos dados têm de ser escritos apenas no final do blob. Blobs de páginas são otimizados para representar discos IaaS e oferecendo suporte a aleatório escreve e pode ser até 1 TB. Um disco IaaS anexado a uma rede de Virtual Machines do Azure é um VHD armazenado como um blob de páginas.

#### <a name="table-storage"></a>Table Storage
O Table storage é o arquivo de chaves/atributos NoSQL da Microsoft – tem um design sem esquemas, que o diferencia das tradicionais bases de dados relacionais. Uma vez que os esquemas de falta de arquivos de dados, é fácil adaptar os seus dados conforme as necessidades da sua aplicação evoluem. O Table Storage é fácil de utilizar para que os programadores possam criar aplicações rapidamente. O Table Storage é um arquivo de atributo chave, o que significa que cada valor numa tabela é armazenado com um nome de propriedade escrito. O nome da propriedade pode ser utilizado para filtrar e especificar critérios de seleção. Uma coleção de propriedades e os respetivos valores compõem uma entidade. Desde esquemas de falta de armazenamento de tabela, duas entidades na mesma tabela podem conter diferentes coleções de propriedades e essas propriedades podem ser de diferentes tipos. Pode utilizar o Table Storage para armazenar conjuntos de dados flexíveis, tais como os dados do utilizador para Web Apps, livros de endereços, informações do dispositivo e qualquer outro tipo de metadados necessários para o seu serviço. Pode armazenar quantas entidades pretender numa tabela e uma conta do Storage pode conter quantas tabelas pretender, até ao limite de capacidade da conta do Storage.

#### <a name="queue-storage"></a>Armazenamento de filas
O armazenamento de Filas do Azure fornece um serviço de mensagens na nuvem entre componentes da aplicação. Ao conceber aplicações para o dimensionamento, os componentes da aplicação, muitas vezes, são desacoplados para um dimensionamento independente. O Armazenamento de filas fornece um serviço de mensagens assíncrono para uma comunicação entre os componentes da aplicação, quer estejam a ser executados na nuvem, no ambiente de trabalho, num servidor no local ou num dispositivo móvel. O Armazenamento de filas também suporta a gestão das tarefas assíncronas e a criação de fluxos de trabalho do processo.

### <a name="keyvault"></a>KeyVault
A RP de Cofre de chaves fornece gestão e auditoria de segredos, tais como palavras-passe e certificados. Por exemplo, um inquilino pode utilizar a RP de Cofre de chaves para fornecer palavras-passe de administrador ou chaves durante a implementação de VM.

## <a name="high-availability-for-azure-stack"></a>Elevada disponibilidade para o Azure Stack
*Aplica-se a: Azure Stack 1802 ou versões superiores*

Para assegurar elevada disponibilidade de um sistema de produção de várias VMS no Azure, as VMs são colocadas num conjunto de disponibilidade que se propaga-las em vários domínios de falha e domínios de atualização. Dessa forma, [as VMs implementadas em conjuntos de disponibilidade](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) estão fisicamente isoladas umas das outras em racks de servidor separadas para permitir para resiliência de falha, conforme mostrado no diagrama seguinte:

  ![Disponibilidade elevada de pilha do Azure](media/azure-stack-key-features/high-availability.png)

### <a name="availability-sets-in-azure-stack"></a>Conjuntos de disponibilidade no Azure Stack
Enquanto a infraestrutura do Azure Stack já está a ser resiliente a falhas, a tecnologia subjacente (clustering de ativação pós-falha) ainda incorre num período de indisponibilidade para as VMs num servidor físico afetado se houver uma falha de hardware. O Azure Stack oferece suporte a ter um conjunto de disponibilidade com um máximo de três domínios de falha para ser consistente com o Azure.

- **Domínios de falha**. Serão fisicamente isoladas umas das outras VMs colocadas num conjunto de disponibilidade ao propagá-los de forma mais uniforme possível através de vários domínios de falha (nós do Azure Stack). Se houver uma falha de hardware, VMs a partir do domínio de falha com falha irão ser reiniciadas em outros domínios de falha, mas, se possível, mantidas em domínios de falha de outras VMs no mesmo conjunto de disponibilidade. Quando o hardware estiver online novamente, as VMs vão ser reequilibradas para manter uma elevada disponibilidade. 
 
- **Domínios de atualização**. Domínios de atualização são outro conceito do Azure que fornece elevada disponibilidade em conjuntos de disponibilidade. Um domínio de atualização é um grupo lógico de hardware subjacente que pode entrar em manutenção ao mesmo tempo. VMs localizadas no mesmo domínio de atualização serão reiniciadas em conjunto durante a manutenção planeada. Como os inquilinos cria VMs num conjunto de disponibilidade, a plataforma Azure distribui automaticamente as VMs por estes domínios de atualização. No Azure Stack, as VMs estão no ar migrados entre os outros anfitriões no cluster online antes do anfitrião subjacente é atualizado. Uma vez que não existe nenhum tempo de inatividade de inquilino durante uma atualização de anfitrião, a funcionalidade do domínio de atualização no Azure Stack só existe para compatibilidade com o modelo com o Azure. 

### <a name="upgrade-scenarios"></a>Cenários de atualização 
VMs nos conjuntos de disponibilidade que foram criados antes da versão do Azure Stack 1802 recebem um número predefinido de domínios de falha e de atualização (1 e 1, respectivamente). Para alcançar a elevada disponibilidade para VMs nestes conjuntos de disponibilidade já existente, tem primeiro de eliminar as VMs existentes e, em seguida, implementá-los novamente para uma novo conjunto de disponibilidade com as contagens de domínio de falha e de atualização corretas conforme descrito em [alteração a conjunto de disponibilidade para uma VM do Windows](https://docs.microsoft.com/azure/virtual-machines/windows/change-availability-set). 

Para conjuntos de dimensionamento de máquina virtual, um conjunto de disponibilidade é criado internamente com um padrão de falhas domínio e de atualização de contagem de domínios (3 e 5, respectivamente). Conta do quaisquer conjuntos de dimensionamento de máquina virtual criados antes da atualização 1802 será colocada num conjunto de disponibilidade com o domínio de falha e de atualização predefinido (1 e 1, respectivamente). Para atualizar estas instâncias do conjunto de dimensionamento máquina virtual para alcançar a propagação mais recente, aumente horizontalmente os conjuntos de dimensionamento de máquina virtual, o número de instâncias que estavam presentes antes da atualização 1802 e, em seguida, eliminar as instâncias mais antigas dos conjuntos de dimensionamento de máquina virtual. 

## <a name="role-based-access-control-rbac"></a>Controlo de acesso (RBAC) baseado em funções
Pode utilizar o RBAC para conceder acesso de sistema para os utilizadores autorizados, grupos e serviços ao atribuir-lhes funções numa subscrição, grupo de recursos ou ao nível de recursos individuais. Cada função define o nível de acesso que um utilizador, grupo ou serviço tem sobre recursos do Microsoft Azure Stack.

RBAC do Azure tem três funções básicas que se aplicam a todos os tipos de recursos: proprietário, Contribuidor e leitor. O proprietário tem acesso total a todos os recursos, incluindo o direito de delegar o acesso a outras pessoas. Contribuidor pode criar e gerir todos os tipos de recursos do Azure, mas não é possível conceder acesso a outras pessoas. Leitor só pode ver os recursos do Azure existentes. O restante das funções do RBAC do Azure permitem a gestão de recursos específicos do Azure. Por exemplo, a função de contribuinte de Máquina Virtual permite a criação e gestão de máquinas virtuais, mas não permite a gestão de rede virtual ou a sub-rede que a máquina virtual se liga a.

## <a name="usage-data"></a>Dados de utilização
Microsoft Azure Stack recolhe e agrega dados de utilização em todos os fornecedores de recursos e transmite-lo para o Azure para processamento pelo Azure commerce. Os dados de utilização recolhidos no Azure Stack podem ser visualizados por meio de uma API REST. Existe uma API de inquilino do Azure consistente, bem como fornecedor e APIs do fornecedor de delegado para obter dados de utilização em todas as subscrições de inquilino. Estes dados podem ser utilizados para integrar com uma ferramenta externa ou o serviço de faturação ou estorno. Assim que a utilização tiver sido processada pelo Azure commerce, podem ser visualizado no portal de faturação do Azure.

## <a name="next-steps"></a>Passos Seguintes
[Noções básicas de administração](azure-stack-manage-basics.md)

