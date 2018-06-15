---
title: Serviços de segurança do Azure e as tecnologias | Microsoft Docs
description: O artigo fornece uma lista organizada de tecnologias e serviços de segurança do Azure.
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: a5a7f60a-97e2-49b4-a8c5-7c010ff27ef8
ms.service: security
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2018
ms.author: barclayn
ms.openlocfilehash: e52cee2cb642de6e54270c597e6ed99f7162d0ed
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34641463"
---
# <a name="security-services-and-technologies-available-on-azure"></a>Serviços de segurança e as tecnologias disponíveis no Azure

No nosso discussões com clientes do Azure atuais e futuros, iremos estiver a muitas vezes, mais frequentes "tem uma lista de todos os serviços relacionados com a segurança e as tecnologias que o Azure tem para oferecer?"

Quando avaliar as opções do fornecedor de serviço em nuvem, é útil ter estas informações. Por isso, fornecemos esta lista para ajudar a começar.

Ao longo do tempo, esta lista irá alterar e aumentar, tal como sucede do Azure. Certifique-se verificar esta página regularmente para se manter atualizado no nosso tecnologias e serviços relacionados com segurança.

## <a name="general-azure-security"></a>Segurança do Azure geral
|Serviço|Descrição|
|--------|--------|
|[Azure&nbsp;segurança&nbsp;Center](../security-center/security-center-intro.md)| Uma solução de proteção de carga de trabalho do cloud fornece gestão de segurança e proteção avançada contra ameaças nas cargas de trabalho do híbridos na nuvem.|
|[Cofre de Chaves do Azure](../key-vault/key-vault-overview.md)| Um arquivo de segredos segura para as palavras-passe, as cadeias de ligação e outras informações que necessárias para manter as suas aplicações a funcionar. |
|[Log Analytics](../log-analytics/log-analytics-overview.md)|Um serviço de monitorização que recolhe telemetria e outros dados e fornece um motor de idioma e análise de consulta para fornecer informações operacionais para as aplicações e recursos. Pode ser usada individualmente ou com outros serviços, tais como o Centro de segurança. |
|[Laboratórios de desenvolvimento/teste do Azure](../devtest-lab/devtest-lab-overview.md)|Um serviço que ajuda os programadores e testers rapidamente criar ambientes no Azure ao minimizando as perdas e controlar o custo.  |

<!---|[Azure&nbsp;Disk&nbsp;Encryption](azure-security-disk-encryption-overview.md)| THIS WILL GO TO THE NEW OVERVIEW TOPIC MEGHAN STEWART IS WRITING|--->

## <a name="storage-security"></a>Segurança de armazenamento
|Serviço|Descrição|
|------|--------|
| [Azure&nbsp;armazenamento&nbsp;serviço&nbsp;encriptação](../storage/common/storage-service-encryption.md)|Uma funcionalidade de segurança que encripta automaticamente os dados no armazenamento do Azure.   |
|[StorSimple encriptados armazenamento híbrida](../storsimple/storsimple-ova-overview.md)| Uma solução de armazenamento integrada que gere as tarefas de armazenamento entre dispositivos no local e de armazenamento na nuvem do Azure.|
|[Encriptação do lado do cliente do Azure](../storage/common/storage-client-side-encryption.md)| Uma solução de encriptação do lado do cliente que encripta os dados dentro de aplicações de cliente antes de carregar para o armazenamento do Azure; também desencripta os dados durante a transferência. |
| [Assinaturas de acesso partilhado do Storage do Azure](../storage/common/storage-dotnet-shared-access-signature-part-1.md)|Uma assinatura de acesso partilhado fornece acesso delegado a recursos na sua conta de armazenamento.  |
|[Chaves de conta de armazenamento do Azure](../storage/common/storage-create-storage-account.md)| Um método de controlo de acesso para o armazenamento do Azure que é utilizado para autenticação quando a conta do storage é acedida. |
|[Partilhas de ficheiros do Azure com a encriptação SMB 3.0](../storage/files/storage-files-introduction.md)|Uma tecnologia de segurança de rede que ativa a encriptação de rede automática para o protocolo de partilha de ficheiros Server Message Block (SMB). |
|[Análise de armazenamento do Azure](https://docs.microsoft.com/en-us/rest/api/storageservices/Storage-Analytics)| Uma tecnologia de registo e a geração de métricas para os dados na sua conta do storage. |

<!------>

## <a name="database-security"></a>Segurança de bases de dados
|Serviço|Descrição|
|------|--------|
| [Azure&nbsp;SQL&nbsp;Firewall](../sql-database/sql-database-firewall-configure.md)|Uma funcionalidade de controlo de acesso do rede protege contra ataques baseados na rede para a base de dados. |
|[Azure&nbsp;SQL&nbsp;célula&nbsp;de nível de encriptação](https://blogs.msdn.microsoft.com/sqlsecurity/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database/)| Uma tecnologia de segurança da base de dados que fornece a encriptação de um nível de granular.  |
| [Azure&nbsp;SQL&nbsp;encriptação da ligação](../sql-database/sql-database-control-access.md)|Para fornecer segurança, a Base de Dados SQL controla o acesso com regras de firewall que limitam a conectividade por endereço IP, mecanismos de autenticação que exigem que os utilizadores provem a sua identidade e mecanismos de autorização que limitam os utilizadores a ações e dados específicos. |
| [SQL do Azure sempre encriptação](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017)|Protege os dados confidenciais, como números de cartão de crédito ou números de identificação national (por exemplo, E.U.A. números de segurança social), armazenados nas bases de dados SQL Database do Azure ou SQL Server.  |
| [Azure&nbsp;SQL&nbsp;encriptação transparente de dados](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql?view=azuresqldb-current)| Uma funcionalidade de segurança da base de dados que encripta o armazenamento de uma base de dados completa. |
| [Base de dados SQL do Azure de auditoria](../sql-database/sql-database-auditing.md)|Uma base de dados de auditoria funcionalidade que controla os eventos de base de dados e escreve-los para uma auditoria iniciar sessão na sua conta do storage do Azure.  |


## <a name="identity-and-access-management"></a>Gestão de identidades e acessos
|Serviço|Descrição|
|------|--------|
| [Azure&nbsp;função&nbsp;com base&nbsp;controlo de acesso](../active-directory/role-based-access-control-configure.md)|Uma funcionalidade de controlo de acesso foi concebida para permitir que os utilizadores acedam apenas os recursos são necessárias para acesso com base nas respetivas funções dentro da organização.  |
| [Azure Active Directory](../active-directory/active-directory-whatis.md)|Um repositório de autenticação baseados na nuvem que suporta vários serviços de gestão de identidade no Azure e um diretório de multi-inquilino, baseado na nuvem.  |
| [Azure Active Directory B2C](../active-directory-b2c/active-directory-b2c-overview.md)|Um serviço de gestão de identidade que permite o controlo sobre como os clientes a inscrição, início de sessão e gerir os seus perfis quando utilizar aplicações baseadas no Azure.   |
| [Serviços de domínio do Active Directory do Azure](../active-directory-domain-services/active-directory-ds-overview.md)| Uma versão baseada na nuvem e gerida dos serviços de domínio do Active Directory. |
| [Multi-factor Authentication do Azure](../active-directory/authentication/multi-factor-authentication.md)| Aprovisionamento de segurança que são implementadas várias formas diferentes de autenticação e verificação antes de permitir o acesso a informações protegidas. |

## <a name="backup-and-disaster-recovery"></a>Cópia de segurança e recuperação após desastre
|Serviço|Descrição|
|------|--------|
| [Azure&nbsp;cópia de segurança](../backup/backup-introduction-to-azure-backup.md)| Um serviço baseado no Azure utilizado para criar cópias de segurança e restaurar dados em nuvem do Azure. |
| [Azure&nbsp;Site&nbsp;recuperação](../site-recovery/site-recovery-overview.md)|Um serviço online que replica cargas de trabalho que executem físicos e máquinas virtuais (VMs) de um site primário para uma localização secundária para ativar a recuperação dos serviços após uma falha. |

## <a name="networking"></a>Redes
|Serviço|Descrição|
|------|--------|
| [Rede&nbsp;segurança&nbsp;grupos](../virtual-network/virtual-networks-nsg.md)| Uma funcionalidade de controlo de acesso baseado na rede utilizando uma 5 cadeias de identificação para permitir ou negar decisões.  |
| [Gateway VPN do Azure](../vpn-gateway/vpn-gateway-about-vpngateways.md)| Um dispositivo de rede utilizado como locais de um ponto final de VPN para permitir o acesso às redes virtuais do Azure.  |
| [Gateway de aplicação do Azure](../application-gateway/application-gateway-introduction.md)|Uma aplicação web avançadas carregar balanceador que pode encaminhar com base no URL e efetuar a descarga de SSL. |
| [Azure Load Balancer](../load-balancer/load-balancer-overview.md)|Um balanceador de carga de rede do TCP/UDP aplicação. |
| [ExpressRoute do Azure](../expressroute/expressroute-introduction.md)| Ligue a uma WAN dedicada entre redes no local e redes virtuais do Azure. |
| [Gestor de Tráfego do Azure](../traffic-manager/traffic-manager-overview.md)| Um balanceador de carga DNS global.|
| [Proxy de aplicações do Azure](../active-directory/active-directory-application-proxy-get-started.md)| Uma autenticação de front-end utilizado para proteger o acesso remoto para aplicações web alojadas no local. |