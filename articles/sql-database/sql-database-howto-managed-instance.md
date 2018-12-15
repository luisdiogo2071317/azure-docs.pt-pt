---
title: Como configurar a instância gerida da base de dados SQL do Azure | Documentos da Microsoft
description: Saiba como configurar e gerir a instância gerida da base de dados SQL do Azure.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 12/14/2018
ms.openlocfilehash: 10a33ac09b5cfa588bef88e6c1587d67036b1aef
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2018
ms.locfileid: "53440493"
---
# <a name="how-to-use-managed-instance"></a>Como utilizar a instância gerida

Nesta seção encontrará várias guias, scripts e explicação que pode ajudar a gerir e configurar a sua base de dados de SQL do Azure - instância gerida.

## <a name="migration"></a>Migração

- [Migrar para a instância gerida](sql-database-managed-instance-migrate.md) – Saiba mais sobre o processo de migração recomendado e ferramentas para a migração para a instância gerida.

- [Migrar TDE cert para a instância gerida](sql-database-managed-instance-migrate-tde-certificate.md) – se a sua base de dados do SQL Server estiver protegido por encriptação de dados transparente (TDE), seria necessário migrar o certificado de instância gerida, pode utilizar para desencriptar a cópia de segurança que pretende restaurar no Azure.

## <a name="network-configuration"></a>Configuração da rede

- [Determinar o tamanho da sub-rede de instância gerida de](sql-database-managed-instance-determine-size-vnet-subnet.md) – instância gerida é colocada no dedica a sub-rede que não pode ser redimensionado depois de adicionar os recursos no interior. Portanto, seria necessário calcular o intervalo IP de endereços seriam necessário para a sub-rede dependendo do número e tipos de instâncias que pretende implementar na sub-rede.
- [Criar nova VNet e sub-rede da instância gerida](sql-database-managed-instance-create-vnet-subnet.md) – VNet do Azure e a sub-rede onde pretende implementar as instâncias geridas devem ser configurados de acordo com o [descritos aqui de requisitos de rede](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Neste guia, encontrará a forma mais fácil para criar a nova VNet e sub-rede configurada corretamente para as instâncias geridas.
- [Configurar a VNet existente e a sub-rede da instância gerida](sql-database-managed-instance-configure-vnet-subnet.md) – se quiser configurar a sua VNet existente e a sub-rede para implementar as instâncias de gerida dentro, aqui pode encontrar o script que verifica o [requisitos de rede](sql-database-managed-instance-connectivity-architecture.md#network-requirements) e marca configura a sua sub-rede, de acordo com os requisitos.
- [Configurar DNS personalizado](sql-database-managed-instance-custom-dns.md) – tem de configurar o DNS personalizado para aceder a recursos externos sobre os domínios personalizados da sua instância gerida através do servidor ligado de perfis de correio de db.
- [Configuração de rede sincronizada](sql-database-managed-instance-sync-network-configuration.md) -isso pode acontecer que embora [integrado a sua aplicação com uma rede Virtual do Azure](../app-service/web-sites-integrate-with-vnet.md), pode&#39;t estabelecer ligação à instância gerida. É uma coisa que pode tentar atualizar a configuração de rede para o seu plano de serviço.
- [Encontrar o endereço IP do ponto final de gestão](sql-database-managed-instance-find-management-endpoint-ip-address.md) – instância gerida utiliza o ponto final público gestão-apenas para fins. Pode determinar o endereço IP do ponto final de gestão a utilizar o script descrito aqui.
- [Verificar a proteção de firewall interno](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md) – instância gerida está protegida com firewall interno que permite o tráfego apenas nas portas necessárias. Pode verificar e verifique se as regras de firewall interno usando o script descrito neste guia.
- [Ligar aplicações](sql-database-managed-instance-connect-app.md) – instância gerida é colocada na sua própria VNet privada do Azure com o endereço IP privado. Saiba mais sobre os padrões diferentes para ligar as aplicações à sua instância gerida.

## <a name="feature-configuration"></a>Configuração de funcionalidades

- [Replicação transacional](replication-with-sql-database-managed-instance.md) permite-lhe replicar os dados entre instâncias geridas ou do servidor de SQL no local para a instância gerida e vice-versa. Encontre mais informações como utilizar e configurar a replicação de transação neste guia.
- [Configurar a deteção de ameaças](sql-database-managed-instance-threat-detection.md) – [deteção de ameaças](sql-database-threat-detection-overview.md) é um recurso de base de dados do Azure SQL interno que detecta vários ataques potenciais, como Injeção de SQL ou o acesso a partir de localizações suspeitas. Este guia pode saber como ativar e configurar [deteção de ameaças](sql-database-threat-detection-overview.md) para a instância gerida.

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [guias de procedimentos na base de dados única](sql-database-howto-single-database.md)