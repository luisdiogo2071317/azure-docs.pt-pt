---
title: Detetar ponto final de gestão de instância gerida da base de dados SQL do Azure | Documentos da Microsoft
description: Saiba como obter a instância gerida da base de dados SQL do Azure ponto final público endereço IP de gestão e verificar a sua proteção de firewall interno
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab
manager: craigg
ms.date: 12/03/2018
ms.openlocfilehash: 99ec559429d66becc20e038e43349f5369afac39
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856258"
---
# <a name="discover-azure-sql-database-managed-instance-management-endpoint"></a>Detetar ponto final de gestão de instância gerida da base de dados SQL do Azure 

## <a name="overview"></a>Descrição geral
Instância gerida do SQL do Azure [virtual cluster](sql-database-managed-instance-connectivity-architecture.md) contém o ponto final de gestão que a Microsoft utiliza para gerir a instância.  

Ponto final de gestão está protegida com firewall interno na verificação de certificado mútua e nível de rede no nível de aplicativo. 

Quando as ligações são iniciadas a partir de dentro da instância gerida (o CLR, o servidor ligado, a cópia de segurança, auditoria log etc.) é apresentada se o tráfego provém de endereço IP de público de ponto final de gestão. Pode limitar o acesso à serviços públicos de instância gerida através da definição de regras de firewall para permitir que apenas este endereço IP.

> [!NOTE]
> Isso não se aplica à configuração de regras de firewall para serviços do Azure que se encontrem na mesma região que a instância gerida que plataforma tem otimização para o tráfego que passa entre os serviços que sejam colocados. 

Este artigo explica como pode obter o ponto final público endereço IP de gestão, bem como para verificar a proteção de firewall interno.

## <a name="finding-management-endpoint-public-ip-address"></a>Encontrar o endereço IP de público de ponto final de gestão
Vamos supor que é anfitrião de MI _mi demo.xxxxxx.database.windows.net_. Execute _nslookup_ usando o nome de anfitrião.

![Resolução de nomes de anfitrião interno](./media/sql-database-managed-instance-management-endpoint/01_find_internal_host.png)

Agora, fazer outra _nslookup_ para nome realçado com removed_.vnet._segment. Obterá o endereço IP público como resultado de executar esse comando.

![Resolução de endereço IP público](./media/sql-database-managed-instance-management-endpoint/02_find_public_ip.png)

## <a name="verifying-managed-instance-built-in-firewall"></a>Verificar o firewall interno da instância gerida
Instância gerida [regras de segurança de entrada obrigatório](sql-database-managed-instance-vnet-configuration.md#mandatory-inbound-security-rules) precisa de portas de gestão 9000, 9003, 1438, 1440, abra o 1452 ser de qualquer origem no grupo de segurança de rede que protege a instância gerida. Embora estas portas estão abertas no nível NSG, estão protegidos no nível de rede por firewall interno.

Para verificar se estas portas, poderia usar qualquer ferramenta de scanner de segurança para testar estas portas. Captura de ecrã seguinte mostra como usar uma dessas ferramentas.

![Verificar o firewall interno](./media/sql-database-managed-instance-management-endpoint/03_verify_firewall.png)
