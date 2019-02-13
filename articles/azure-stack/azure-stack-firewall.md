---
title: Sistemas integrados do Azure firewall de pilha de planeamento para o Azure Stack | Documentos da Microsoft
description: Descreve as considerações de firewall do Azure Stack para implementações de ligada ao Azure Stack Azure com vários nós.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: wfayed
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 023201d221ee5d7ec884c6a760407e8da8340d3f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56207145"
---
# <a name="azure-stack-firewall-integration"></a>Integração da firewall do Azure Stack
Recomenda-se que utilize um dispositivo de firewall para o ajudar a proteger do Azure Stack. Firewalls podem ajudar na defesa contra ataques distribuídos denial-of-service (DDOS), detecção de intrusão e inspeção de conteúdo. No entanto, eles podem também se tornar um gargalo de serviços de armazenamento do Azure, como blobs, tabelas e filas.

 Se for utilizado um modo de implementação desligado, tem de publicar o ponto de extremidade do AD FS. Para obter mais informações, consulte a [artigo de identidade de integração do Centro de dados](azure-stack-integrate-identity.md).

O Azure Resource Manager (administrador), o portal de administrador e pontos de extremidade do Key Vault (administrador) não requerem necessariamente publicação externa. Por exemplo, como um fornecedor de serviços, pode limitar a superfície de ataque ao administrar o Azure Stack de apenas no interior da rede e não a partir da internet.

Para organizações empresariais, a rede externa pode ser a rede empresarial existente. Neste cenário, tem de publicar os pontos de extremidade opere o Azure Stack da rede empresarial.

### <a name="network-address-translation"></a>Tradução de Endereços de Rede
Tradução de endereços de rede (NAT) é o método recomendado para permitir que a máquina de virtual de implementação (DVM) para aceder a recursos externos e a internet durante a implementação, bem como as VMs da consola de recuperação de emergência (ERCS) ou com privilégios ponto final (PEP) durante o registo e resolução de problemas.

NAT também pode ser uma alternativa para endereços IP públicos na rede externa ou VIPs públicas. No entanto, não é recomendado fazê-lo porque limita a experiência de utilizador do inquilino e aumenta a complexidade. Uma opção seria um NAT de um para um que ainda requer um IP público por IP de utilizador no conjunto. Outra opção é um muitos para um NAT que necessita de uma regra NAT por utilizador VIP para todas as portas que um usuário poderia utilizar.

Algumas das desvantagens da utilização de NAT para o VIP público são:
- NAT sobrecarga adiciona ao gerir regras de firewall, uma vez que os utilizadores controlar seus próprios pontos finais e suas próprias regras de publicação na pilha de rede (SDN) definidas por software. Os utilizadores têm de contactar o operador do Azure Stack para obter seus VIPs publicadas e para atualizar a lista de porta.
- Embora a utilização NAT limita a experiência do usuário, dá-controlo total para o operador ao longo de pedidos de publicação.
- Para cenários de nuvem híbrida com o Azure, considere que o Azure não suporta a configuração de um túnel VPN para um ponto final com NAT.

### <a name="ssl-decryption"></a>Desencriptação de SSL
Recomenda-se atualmente para desativar a desencriptação de SSL em todo o tráfego do Azure Stack. Caso seja suportado em atualizações futuras, será fornecida orientação sobre como ativar a desencriptação de SSL para o Azure Stack.

## <a name="edge-firewall-scenario"></a>Cenário de firewall de borda
Uma implementação do edge, do Azure Stack é implementado diretamente por trás do firewall ou o router de limite. Nestes cenários, é suportada para a firewall para estar acima do limite (cenário 1) em que ele oferece suporte a configurações de firewall de ativo-ativo e ativa-passiva ou agir como o dispositivo de limite (cenário com 2) em que ela oferece suporte apenas a firewall de ativo-ativo configuração da entidade confiadora no igual custo múltiplas caminho (ECMP) com o BGP ou o encaminhamento estático para a ativação pós-falha.

Endereços IP encaminháveis públicos são especificados para o conjunto de VIP público da rede externa, no momento da implementação. Num cenário de borda, não é recomendado para utilizar os IPs encaminháveis público em qualquer outra rede por motivos de segurança. Este cenário permite que um utilizador experimentar a experiência de cloud Self-controlado completa como numa nuvem pública, como o Azure.  

![Exemplo de firewall de borda do Azure Stack](./media/azure-stack-firewall/firewallScenarios.png)

## <a name="enterprise-intranet-or-perimeter-network-firewall-scenario"></a>Perímetro de intranet ou rede firewall cenário empresarial
Numa implementação empresarial da intranet ou de perímetro, o Azure Stack é implementado numa firewall a zona de múltiplas ou entre o firewall de borda e a firewall da rede interna, corporativos. Em seguida, o seu tráfego é distribuído entre a rede de perímetro seguro, (ou rede de Perímetro), e zonas não seguros, como descrito abaixo:

- **Zona segura**: Esta é a rede interna, que utiliza endereços IP encaminháveis internos ou empresariais. A rede segura pode ser dividida, ter acesso de saída de internet através de NAT na Firewall e é, normalmente, acessível a partir de qualquer lugar dentro do seu datacenter através da rede interna. Todas as redes do Azure Stack devem residir na zona de segurança, exceto para o conjunto de VIP público da rede externa.
- **Zona de perímetro**. A rede de perímetro é onde externos ou aplicativos, como servidores Web são normalmente implementados de com acesso à internet. Normalmente, é monitorizado por uma firewall para evitar ataques, como DDoS e intrusões (hacking), mas permite que o tráfego de entrada especificado da internet. Apenas o rede externa conjunto de VIP públicos do Azure Stack deve residir na zona de rede de Perímetro.
- **Zona não segura**. Esta é a rede externa, a internet. Ele **není** recomendado para implementar o Azure Stack na zona sem segurança.

![Exemplo de rede de perímetro do Azure Stack](./media/azure-stack-firewall/perimeter-network-scenario.png)

## <a name="learn-more"></a>Saiba mais
Saiba mais sobre [portas e protocolos utilizados pelos pontos de extremidade do Azure Stack](azure-stack-integrate-endpoints.md).

## <a name="next-steps"></a>Passos Seguintes
[Requisitos de PKI da pilha do Azure](azure-stack-pki-certs.md)

