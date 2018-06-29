---
title: Integridade da infraestrutura do Azure
description: Este artigo aborda a integridade da infraestrutura do Azure.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 18d7fab30c0bbaa5292fe5d3003b7f2309b34d3b
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102523"
---
# <a name="integrity-of-azure-infrastructure"></a>Integridade da infraestrutura do Azure   

## <a name="software-installation"></a>Instalação de software
Todos os componentes na pilha de software que estão instalados no ambiente do Azure estão personalizadas incorporadas seguindo o processo de Security Development Lifecycle (SDL) da Microsoft. Todos os componentes de software (incluindo as imagens de SO e a SQL Database) são implementados como parte do processo de alteração e gestão de versões. O sistema operativo que é executado em todos os nós é uma versão personalizada do Windows Server 2008 ou Windows Server 2012. A versão exata é escolhida por FC, de acordo com a função-intends para o SO reproduzir. Além disso, o sistema operativo anfitrião não permite a instalação de quaisquer componentes de software não autorizado.

Alguns componentes do Microsoft Azure (por exemplo, RDFE, Portal do programador, etc.) são implementadas como clientes do Azure numa VM do convidado em execução no SO convidado.

## <a name="virus-scans-on-builds"></a>Análises de vírus no compilações
Compilações do componente (incluindo o SO) de software do Azure tem de passar por uma análise de vírus com a ferramenta de software antivírus do Microsoft Endpoint Protection (MEP). Cada análise de vírus irá criar um registo no diretório de compilação associadas com detalhes sobre o que foi verificado e os resultados da análise. A análise de vírus faz parte do código fonte compilação para cada componente no Azure. Código não será movido para produção sem ter um vírus limpo e com êxito de análise. Se existirem problemas indicação em contrário, a compilação está fixa e será, em seguida, vá para as equipas de segurança dentro do Microsoft Security para identificar em que o código de "não autorizados" introduzido a compilação.

## <a name="closedlocked-environment"></a>Ambiente fechado/bloqueado
Por predefinição, nós de infraestrutura do Azure e VMs de convidado não têm as contas de utilizador criadas nos mesmos. Além disso, as contas de administrador do Windows predefinida também estão desativadas. Os administradores do Microsoft Azure em direto suporte (WALS) podem – com a autenticação adequada –, inicie sessão destas máquinas e administrar a rede de produção do Azure para emergência reparações.

## <a name="microsoft-azure-sql-database-authentication"></a>Autenticação de base de dados do SQL Server do Microsoft Azure
Tal como acontece com a implementação do SQL Server, gestão de contas de utilizador tem de ser controlado rigorosamente. Base de dados do Microsoft Azure SQL Server só suporta a autenticação do SQL Server. Utilizador de contas com palavras-passe seguras e configurada com específicos direitos devem ser utilizados, bem como para complementar o modelo de segurança de dados do cliente.

## <a name="firewallacls-between-msft-corpnet-and-microsoft-azure-cluster"></a>Firewall/ACLs entre MSFT CorpNet e Cluster do Microsoft Azure
ACLs/Firewall entre a plataforma de serviço e a rede empresarial MS proteger a base de dados do Microsoft Azure SQL contra acesso não autorizado internas. Além disso, apenas os utilizadores de intervalos de endereços IP da Microsoft CorpNet podem aceder o ponto final de gestão do WinFabric plataforma.

## <a name="firewallacls-between-nodes-in-an-azure-sql-db-cluster"></a>Firewall/ACLs entre nós num cluster BD SQL do Azure
Como uma proteção adicional, como parte da defesa-na profundidade-estratégia, foi implementada ACLs/Firewall entre nós num cluster de BD SQL do Microsoft Azure. Todas as comunicações dentro do cluster de plataforma WinFabric, bem como a todo o código em execução é fidedigno.

## <a name="custom-mas-watchdogs"></a>MAs personalizadas (Watchdogs)
Base de dados de SQL do Microsoft Azure emprega MAs personalizadas denominadas watchdogs para monitorizar o estado de funcionamento do cluster BD SQL do Microsoft Azure.

## <a name="web-protocols"></a>Protocolos de Web

### <a name="role-instance-monitoring-and-restart"></a>Monitorização de instância de função e reinício
Azure garante que todas as em execução funções (web para a Internet, ou funções de trabalho de processamento de back-end) implementadas estão sujeitos a um Estado de funcionamento para garantir que efetivamente de monitorização e fornecimento de forma eficiente os serviços em que tiver sido aprovisionadas. Em caso de uma função fica danificada, por uma falha crítica da aplicação que está a ser problema de configuração subjacente ou alojada numa instância de função próprio, FC do Microsoft Azure irá detetar o problema na instância de função e iniciar um Estado as medidas corretivas .

### <a name="compute-connectivity"></a>Conectividade de computação
Azure garante que a aplicação/serviço implementado é acessível através de protocolos padrão baseada na web. Para a Internet web virtual as instâncias de função terão Conectividade Internet externa e esteja acessíveis diretamente por utilizadores de web. Instâncias de função do processamento de back-end worker virtual tem conectividade de Internet externa, mas não pode ser acedidas diretamente por um utilizador da web externo, para proteger a confidencialidade e a integridade das operações que executar as funções de trabalho em nome do instâncias virtuais da função web acessível publicamente.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre o que faz Microsoft para proteger a infraestrutura do Azure, consulte:

- [Instalações do Azure, no local e a segurança física](azure-physical-security.md)
- [Disponibilidade da infraestrutura do Azure](azure-infrastructure-availability.md)
- [Componentes de sistema de informações do Azure e limites](azure-infrastructure-components.md)
- [Arquitetura de rede do Azure](azure-infrastructure-network.md)
- [Rede de produção do Azure](azure-production-network.md)
- [Funcionalidades de segurança da base de dados de SQL do Microsoft Azure](azure-infrastructure-sql.md)
- [Gestão e operações de produção do Azure](azure-infrastructure-operations.md)
- [Monitorização da infraestrutura do Azure](azure-infrastructure-monitoring.md)
- [Proteção de dados de cliente no Azure](azure-protection-of-customer-data.md)
