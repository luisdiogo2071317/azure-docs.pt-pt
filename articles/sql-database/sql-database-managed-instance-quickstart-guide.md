---
title: Início rápido - instância gerida de base de dados SQL do Azure | Documentos da Microsoft
description: Saiba como começar a utilizar rapidamente com o Azure SQL Database - instância gerida
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 77deed43c106a451d3de768989233c749e1280e1
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55468177"
---
# <a name="getting-started-with-azure-sql-database-managed-instance"></a>Introdução à instância gerida da base de dados SQL do Azure

[O Azure SQL Database Managed Instance](sql-database-managed-instance-index.yml) é totalmente gerido de PaaS de versão do SQL Server alojado na cloud do Azure e colocada na sua própria VNet com o endereço IP privado. Nesta seção, aprenderá como configurar rapidamente e criar instância gerida e migrar as suas bases de dados.

## <a name="quickstart-overview"></a>Descrição geral do guia de introdução

Nesta secção, irá ver uma descrição geral dos artigos disponíveis que podem ajudar a começar rapidamente instâncias geridas. A maneira mais fácil de criar a primeira instância gerida é usar [do portal do Azure](sql-database-managed-instance-get-started.md) ambiente de rede onde pode configurar os parâmetros necessários (nome de utilizador/palavra-passe, número de núcleos, ao armazenamento máximo) e criar automaticamente do Azure sem precisar de saber sobre o funcionamento em rede detalhes e requisitos de infraestrutura. Apenas Certifique-se de que tem um [tipo de subscrição](sql-database-managed-instance-resource-limits.md#supported-subscription-types) que tem permissão para criar a instância.

Se tem sua própria rede que pretende utilizar ou se pretender personalizar a rede, consulte como [configurar o ambiente de rede](#configure-network-environment) para a instância gerida.

Ao criar a sua instância gerida, terá de ligar à instância utilizando uma das abordagens seguintes:

* Crie [Máquina Virtual do Azure](sql-database-managed-instance-configure-vm.md) com instalado o SQL Server Management Studio e outras aplicações que podem ser utilizadas para aceder à sua instância gerida numa sub-rede na mesma VNet onde a sua instância gerida está colocada. VM não pode estar na mesma sub-rede com as instâncias geridas.
* Configurar [ligaçãopontoasitede](sql-database-managed-instance-configure-p2s.md) no seu computador que irá permitir-lhe associar o seu computador para a VNet onde a instância gerida é colocada e utilizar a instância gerida de qualquer outro SQL Server na sua rede.

Como alternativa, pode utilizar o expressroute ou de ligação site a site da rede local, mas essas abordagens estão fora do âmbito dos inícios Rápidos seguintes.

Quando cria uma instância gerida e configurar o acesso, pode começar a migrar as bases de dados colocados no SQL Server no local ou VMs do Azure. Tenha em atenção que a migração irá falhar se tiver algumas funcionalidades não suportadas na base de dados origem que pretende migrar. Para evitar falhas e verificar a compatibilidade, pode instalar [Assistente de migração de dados (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) que irá analisar seus bancos de dados no SQL Server e encontrar qualquer problema que poderia bloquear a migração para a instância gerida, tais como a existência de FileStream ou vários ficheiros de registo. Se resolver estes problemas, as bases de dados estão prontos para ir para a instância gerida. [Base de dados de Assistente de experimentação](https://blogs.msdn.microsoft.com/datamigration/2018/08/06/release-database-experimentation-assistant-dea-v2-6/) é outra ferramenta útil que pode gravar sua carga de trabalho no SQL Server e repetição-lo na instância gerida para que possa determinar estarão disponíveis para ser problemas de desempenho, se efetuar a migração para a instância gerida.

Assim que tiver a certeza de que pode migrar a base de dados para a instância gerida, pode utilizar [restaurar nativo](sql-database-managed-instance-get-started-restore.md) funcionalidade que permite-lhe criar uma cópia de segurança da base de dados com o comando do Transact-SQL, carregue-o para um armazenamento de Blobs do Azure e RESTAURE a base de dados do armazenamento de Blobs com o comando do Transact-SQL.

Estes inícios rápidos permitem-lhe configurar, criar e colocar as bases de dados em suas instâncias geridas rapidamente. Em alguns cenários, precisaria personalizar ou automatizar a implantação de instância gerida e o ambiente de rede necessário. Estes cenários serão descritos abaixo.

## <a name="customizing-network-environment"></a>Personalizar o ambiente de rede

Embora a VNet/sub-rede podem ser configurada automaticamente quando a instância é criada usando [do portal do Azure](sql-database-managed-instance-get-started.md), talvez seja bom para criá-lo antes de começar a criar instâncias geridas porque pode configurar os parâmetros da VNet e a sub-rede. A maneira mais fácil para criar e configurar o ambiente de rede é usar [implementação de recursos do Azure](sql-database-managed-instance-create-vnet-subnet.md) modelo que irá criar e configurar a rede e a sub-rede onde a instância será colocada. Basta pressionar o Azure Resource Manager botão implementar e preencher o formulário com parâmetros. Como alternativa, pode utilizar [script do PowerShell](https://www.powershellmagazine.com/2018/07/23/configuring-azure-environment-to-set-up-azure-sql-database-managed-instance-preview/) para automatizar a criação da rede.

Se já tiver uma VNet e sub-rede onde pretende implementar a sua instância gerida, terá de certificar-se de que a sua VNet e sub-rede satisfazem [requisitos de rede](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Deve usá-la [script do PowerShell para verificar se a sub-rede está configurada corretamente](sql-database-managed-instance-configure-vnet-subnet.md). Este script irá validar a sua rede e reportar os problemas – não apenas ele dirá o que deve ser alterado e também oferecem a para fazer as alterações necessárias na sua VNet/subrede. Execute este script, se não quiser configurar a sua VNet/sub-rede manualmente, e também deve executá-lo Depois de qualquer grandes Reconfigurações de sua infraestrutura de rede. Se pretender criar e configurar a sua própria rede ler [documentação de instância gerida](sql-database-managed-instance-connectivity-architecture.md) e [este guia](https://medium.com/azure-sqldb-managed-instance/the-ultimate-guide-for-creating-and-configuring-azure-sql-managed-instance-environment-91ff58c0be01).

## <a name="automating-creation-of-managed-instance"></a>Automatizar a criação de instância gerida

 Se não tiver criado o ambiente de rede conforme descrito no passo anterior, o portal do Azure pode fazê-lo para – o único inconveniente é o fato de que ele irá configurá-lo com alguns parâmetros de predefinição que não é possível alterar mais tarde. Como alternativa, pode utilizar [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/), [PowerShell com o modelo do Resource Manager](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md), ou [da CLI do Azure](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/11/14/create-azure-sql-managed-instance-using-azure-cli/).

## <a name="migrating-to-managed-instance-with-minimal-downtime"></a>Migrar para a instância gerida com o tempo de inatividade mínimo

Artigos nestes inícios rápidos permitem-lhe configurar a instância gerida e mover as bases de dados rapidamente. No entanto, com o restauro nativo, que terá de aguardar que as bases de dados ser restaurado, que faria com que algum período de inatividade do seu aplicativo especialmente se a base de dados é maior. Se estiver a mover a base de dados de produção, provavelmente terá uma melhor forma de migrar, que irá garantir que o período de indisponibilidade mínimo de migração. [Serviço de migração de dados](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance?toc=/azure/sql-database/toc.json) é um serviço de migração que pode migrar a sua base de dados com o período de indisponibilidade mínimo ao incrementalmente enviar as alterações efetuadas na base de dados de origem para uma base de dados que está a restaurar para a instância gerida. Dessa forma, pode alternar rapidamente seu aplicativo de origem para a base de dados de destino com o período de indisponibilidade mínimo.

## <a name="next-steps"></a>Passos Seguintes

* Encontrar um [lista de alto nível das funcionalidades suportadas na instância gerida aqui](sql-database-features.md) e [detalhes e problemas conhecidos aqui](sql-database-managed-instance-transact-sql-information.md). 
* Saiba mais sobre [características técnicas de instância gerida](sql-database-managed-instance-resource-limits.md#instance-level-resource-limits). 
* Encontre mais avançadas tutoriais [como secção](sql-database-howto-managed-instance.md). 
