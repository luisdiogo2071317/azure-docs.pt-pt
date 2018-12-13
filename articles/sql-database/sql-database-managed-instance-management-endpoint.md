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
ms.date: 12/04/2018
ms.openlocfilehash: fc04422f5e32a77fff42dc07e2441e84c9b15866
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53271883"
---
# <a name="discover-azure-sql-database-managed-instance-management-endpoint"></a>Detetar ponto final de gestão de instância gerida da base de dados SQL do Azure

O Azure SQL Database Managed Instance [virtual cluster](sql-database-managed-instance-connectivity-architecture.md) contém um ponto final de gestão que a Microsoft utiliza para gerir a instância gerida. O ponto final de gestão está protegido com firewall interno na verificação de certificado mútua e nível de rede no nível de aplicativo.

Quando as ligações sejam iniciadas a partir de dentro da instância gerida (cópia de segurança, registo de auditoria), parece que origina o tráfego do endereço IP de público de ponto final de gestão. Pode limitar o acesso à serviços públicos de instância gerida através da definição de regras de firewall para permitir apenas o endereço de IP de instância gerida.

> [!NOTE]
> Isso não se aplica à configuração de regras de firewall para serviços do Azure que estão na mesma região que a instância gerida, como a plataforma do Azure tem uma otimização para o tráfego que passa entre os serviços que sejam colocados.

Este artigo explica como pode obter o ponto final público endereço IP de gestão, bem como para verificar a proteção de firewall interno.

## <a name="finding-the-management-endpoint-public-ip-address"></a>Encontrar o endereço IP público do gestão ponto final

Vamos supor que o anfitrião da instância gerida é `mi-demo.xxxxxx.database.windows.net`. Executar `nslookup` usando o nome de anfitrião.

![Resolução de nomes de anfitrião interno](./media/sql-database-managed-instance-management-endpoint/01_find_internal_host.png)

Agora, fazer outra `nslookup` para remover o nome realçado a `.vnet.` segmento. Obterá o endereço IP público como resultado de executar esse comando.

![Resolução de endereço IP público](./media/sql-database-managed-instance-management-endpoint/02_find_public_ip.png)

## <a name="verifying-the-managed-instance-built-in-firewall"></a>A verificar o firewall interno da instância gerida

A instância gerida [regras de segurança de entrada obrigatória](sql-database-managed-instance-vnet-configuration.md#mandatory-inbound-security-rules) precisa de portas de gestão 9000, 9003, 1438, 1440, 1452 ser abrir a partir do **qualquer origem** em segurança de rede grupo (NSG) que protege a gerida Instância. Embora estas portas estão abertas no nível do NSG, estão protegidos ao nível da rede, o firewall interno.

Para verificar se estas portas, utilize qualquer ferramenta de scanner de segurança para testar estas portas. Captura de ecrã seguinte mostra como usar uma dessas ferramentas.

![Verificar o firewall interno](./media/sql-database-managed-instance-management-endpoint/03_verify_firewall.png)

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre as instâncias geridas e a conectividade, consulte [arquitetura de conectividade de instância de geridas da base de dados do SQL Azure](sql-database-managed-instance-connectivity-architecture.md).