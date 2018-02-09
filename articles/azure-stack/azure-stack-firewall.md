---
title: Firewall de pilha Azure planeamento de pilha do Azure integrado sistemas | Microsoft Docs
description: "Descreve as considerações de firewall de pilha do Azure implementações de vários nós ligado do Azure de pilha do Azure."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 919618c0779d47f0add02d5e7d3ab9ab4b5bdd10
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="azure-stack-firewall-integration"></a>Integração de firewall de pilha do Azure
É recomendado que utilize um dispositivo de firewall para o ajudar a proteger pilha do Azure. Embora as firewalls podem ajudar com coisas como distribuídas denial of service (DDoS distribuídos), deteção de intrusão e e ataques inspeção de conteúdo, pode também ficarem um estrangulamento do débito para serviços de armazenamento do Azure como blobs, tabelas e filas.

Com base no modelo de identidade do Azure Active Directory (Azure AD) ou Windows Server Active Directory os serviços de Federação (AD FS), poderá ser necessário para publicar o ponto final do AD FS. Se for utilizado um modo de implementação de desligado, tem de publicar o ponto final do AD FS. Para obter mais informações, consulte o [artigo de identidade de integração do Centro de dados](azure-stack-integrate-identity.md).

O Azure Resource Manager (administrador), o portal de administrador e a pontos finais do Cofre de chaves (administrador) não exige necessariamente de publicação externa. Por exemplo, como um fornecedor de serviços, pode querer limitar a superfície de ataque e administrar apenas a pilha de Azure a partir de dentro da sua rede e não a partir da internet.

Para organizações de empresa, a rede externa pode ser a rede empresarial existente. Neste cenário, é necessário publicar os pontos finais para operar pilha do Azure a partir da rede empresarial.

### <a name="network-address-translation"></a>Tradução de Endereços de Rede
Tradução de endereços de rede (NAT) é o método recomendado para permitir que a máquina virtual de implementação (DVM) para aceder os recursos externos e a internet durante a implementação, bem como as VMs de consola de recuperação de emergência (ERCS) ou com privilégios ponto final (PEP) durante a registo e a resolução de problemas.

NAT também pode ser uma alternativa para endereços IP públicos no VIPs públicas ou de rede externa. No entanto, não é recomendada para fazê-lo porque limita a experiência de utilizador de inquilino e aumenta a complexidade. As duas opções seria um NAT 1:1, que ainda necessita de um IP público por IP de utilizador no conjunto ou muitos: 1 NAT que necessita de uma regra NAT por utilizador VIP que contém as associações de todas as portas de que um utilizador pode utilizar.

Algumas das downsides da utilização de NAT para o VIP público são:
- NAT sobrecarga adiciona ao gerir regras de firewall porque dos utilizadores para controlar os seus próprios pontos finais e as suas próprias regras de publicação na pilha de rede (SDN) definidas por software. Os utilizadores tem de contactar o operador de pilha do Azure para obter os VIPs publicadas e para atualizar a lista de portas.
- Embora a utilização NAT limita a experiência de utilizador, fornece controlo total para o operador através de pedidos de publicação.
- Para cenários de nuvem híbrida com o Azure, considere que o Azure não suporta a configuração de um túnel VPN para um ponto final com NAT.

### <a name="ssl-decryption"></a>Desencriptação de SSL
Recomenda-se atualmente para desativar a desencriptação de SSL em todo o tráfego de pilha do Azure. Caso seja suportado nas atualizações futuras, será fornecida orientação sobre como ativar a desencriptação de SSL para a pilha do Azure.

## <a name="edge-firewall-scenario"></a>Cenário de firewall de limite
Uma implementação de limite, pilha do Azure é implementada diretamente atrás da firewall ou o router de limite. Nestes cenários, é suportada para a firewall para estar acima do limite ou a agir como o dispositivo de limite se suporta igual custo várias caminho (ECMP) com o BGP ou encaminhamento estático.

Normalmente, os endereços IP encaminháveis públicos especificados para o conjunto VIP público da rede externa no momento da implementação. Num cenário de limite, não se recomenda utilizar IPs encaminháveis públicos em qualquer outra rede por motivos de segurança. Este cenário permite que um utilizador experimentar a experiência de nuvem automática controlada completa como uma nuvem pública, como o Azure.  

![Exemplo de firewall de limite de pilha do Azure](.\media\azure-stack-firewall\edge-firewall-scenario.png)

## <a name="enterprise-intranet-or-perimeter-network-firewall-scenario"></a>Intranet ou perímetro rede firewall cenário empresarial
Implementação empresarial da intranet ou de perímetro, pilha do Azure é implementada uma firewall a zona várias ou entre firewall de limite e a firewall da rede empresarial interna. Em seguida, o respetivo tráfego é distribuído entre a rede de perímetro segura, (ou DMZ) e zonas não seguros, como descrito abaixo:

- **Zona segura**: Esta é a rede interna que utiliza endereços IP encaminháveis internos ou da empresariais. A rede segura pode ser dividida, tem acesso de saída à internet através de NAT na Firewall e normalmente acessível a partir em qualquer lugar no seu centro de dados através da rede interna. Todas as redes de pilha do Azure devem residir na zona segura, exceto para o conjunto de VIP público da rede externa.
- **Zona de perímetro**. Rede de perímetro é onde externo ou internet com aplicações, como servidores Web, normalmente, são implementados. Normalmente, é monitorizado por uma firewall para evitar ataques como DDoS e intrusões (pirataria), permitindo ainda que o tráfego de entrada especificado através da internet. Apenas o rede externa público conjunto VIP da pilha do Azure deve residir na zona de rede de Perímetro.
- **Zona não segura**. Esta é a rede externa, a internet. - **Não é** recomendado para implementar a pilha do Azure na zona não segura.

![Exemplo de rede de perímetro de pilha do Azure](.\media\azure-stack-firewall\perimeter-network-scenario.png)

## <a name="learn-more"></a>Saiba mais
Saiba mais sobre [portas e protocolos utilizados pelos pontos finais de pilha do Azure](azure-stack-integrate-endpoints.md).

## <a name="next-steps"></a>Passos Seguintes
[Requisitos de PKI de pilha do Azure](azure-stack-pki-certs.md)

