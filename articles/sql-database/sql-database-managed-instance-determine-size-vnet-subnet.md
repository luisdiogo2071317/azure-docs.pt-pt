---
title: O Azure SQL Database Managed Instance determinar o tamanho de VNet/sub-rede | Documentos da Microsoft
description: Este tópico descreve como calcular o tamanho da sub-rede em que as instâncias de geridas da base de dados do Azure SQL serão implementadas.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 12/12/2018
ms.openlocfilehash: 97bb7ac79dc96d7f66ae330f5d6a603ca600f648
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54043381"
---
# <a name="determine-vnet-subnet-size-for-azure-sql-database-managed-instance"></a>Determinar o tamanho da sub-rede de VNet de instância gerida da base de dados SQL do Azure

Instância de gerida de base de dados de SQL do Azure tem de ser implementada dentro do Azure [rede virtual (VNet)](../virtual-network/virtual-networks-overview.md).

O número de instâncias geridas que podem ser implementadas na sub-rede da VNet depende do tamanho da sub-rede (intervalo de sub-rede).

Quando cria uma instância gerida, o Azure aloca um número de máquinas virtuais, consoante o escalão selecionado durante o aprovisionamento. Uma vez que estas máquinas virtuais estão associadas a sua sub-rede, necessitam de endereços IP. Para garantir a elevada disponibilidade durante operações normais e de manutenção do serviço, o Azure pode alocar máquinas virtuais adicionais. Como resultado, o número de endereços IP necessários numa sub-rede é maior do que o número de instâncias geridas nessa sub-rede.

Por design, uma instância gerida necessita de um mínimo de 16 endereços IP numa sub-rede e pode utilizar até 256 endereços IP. Como resultado, pode utilizar máscaras de sub-rede /28 para /24 quando definir os intervalos IP de sub-rede.

> [!IMPORTANT]
> Tamanho de sub-rede com 16 endereços IP é o mínimo absoluto com potencial de limitado para a outra instância gerida de ampliação. Ao escolher sub-rede com o prefixo /27 ou abaixo é altamente recomendado.

## <a name="determine-subnet-size"></a>Determinar o tamanho da sub-rede

Se planeia implementar várias instâncias geridas dentro da sub-rede e precisa para otimizar o tamanho da sub-rede, utilize estes parâmetros para formar um cálculo:

- O Azure utiliza cinco endereços IP na sub-rede para as suas próprias necessidades
- Cada instância de fins gerais tem dois endereços
- Cada uma delas críticas para a empresa precisa quatro endereços

**Exemplo**: Planeia ter três para fins gerais e duas empresas instâncias da críticos geridos. Que significa que terá de 5 + 3 * 2 + 2 * 4 = 19 endereços IP. Como intervalos de IP estão definidos na potência de 2, precisa do intervalo IP de 32 (2 ^ 5) endereços IP. Por conseguinte, terá de reservar a sub-rede com a máscara de sub-rede de/27.

> [!IMPORTANT]
> Cálculo exibido acima se tornará obsoleto com ainda mais melhorias.

## <a name="next-steps"></a>Passos Seguintes

- Para uma descrição geral, consulte [o que é uma instância gerida](sql-database-managed-instance.md).
- Saiba mais sobre [arquitetura de conectividade para a instância gerida](sql-database-managed-instance-connectivity-architecture.md).
- Veja como [criar VNet onde irá implementar instâncias geridas](sql-database-managed-instance-create-vnet-subnet.md)
- Para problemas DNS, consulte [configurar um DNS personalizado](sql-database-managed-instance-custom-dns.md)
