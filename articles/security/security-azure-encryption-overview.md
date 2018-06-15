---
title: Descrição geral da encriptação do Azure | Microsoft Docs
description: Saiba mais sobre as várias opções de encriptação no Azure
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: barclayn
ms.openlocfilehash: 00c8b30b5351b7a6e4388b186fab70e3a3357ef4
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34366312"
---
# <a name="azure-encryption-overview"></a>Descrição geral da encriptação do Azure

Este artigo fornece uma descrição geral de como a encriptação é utilizada no Microsoft Azure. Aborda as principais áreas de encriptação, incluindo a encriptação de inativos, de encriptação em trânsito e gestão de chaves com o Cofre de chaves do Azure. Cada secção inclui ligações para informações mais detalhadas.

## <a name="encryption-of-data-at-rest"></a>Encriptação de dados inativos

Dados Inativos incluem informações que residem no armazenamento persistente num suporte físico, em qualquer formato digital. O suporte de dados pode incluir os ficheiros no suporte de dados torção ou optical, os dados arquivados e cópias de segurança de dados. Microsoft Azure oferece uma variedade de soluções de armazenamento de dados para satisfazer as necessidades diferentes, incluindo ficheiros, discos, BLOBs e o table storage. A Microsoft também fornece a encriptação para proteger [SQL Database do Azure](../sql-database/sql-database-technical-overview.md), [Azure Cosmos DB](../cosmos-db/introduction.md)e o Azure Data Lake.

Está disponível para os serviços de encriptação de dados Inativos entre o software como um serviço (SaaS), a plataforma como um serviço (PaaS) e a infraestrutura como um em nuvem modelos de serviço (IaaS). Este artigo resume e fornece recursos para ajudar a utilizar as opções de encriptação do Azure.

Para ver um debate mais detalhado da forma como os dados Inativos são encriptados no Azure, consulte [encriptação em Rest da Azure dados](azure-security-encryption-atrest.md).

## <a name="azure-encryption-models"></a>Modelos de encriptação do Azure

Azure suporta vários modelos de encriptação, incluindo encriptação do lado do servidor que utiliza o serviço gerido chaves, chaves gerida pelo cliente no Cofre de chaves ou chaves gerida pelo cliente no hardware controlados pelo cliente. Encriptação do lado do cliente, pode gerir e armazenar as chaves no local ou noutra Proteja a localização.

### <a name="client-side-encryption"></a>Encriptação do lado do cliente

Encriptação do lado do cliente é efetuada fora do Azure. Inclui:

- Dados são encriptados por uma aplicação que está a executar no Centro de dados do cliente ou por uma aplicação de serviço.
- Dados que já são encriptados quando recebido pelo Azure.

Com a encriptação do lado do cliente, os fornecedores de serviços em nuvem não tem acesso às chaves de encriptação e não é possível desencriptar estes dados. Manter o controlo total das chaves.

### <a name="server-side-encryption"></a>Encriptação do lado do servidor

Os três modelos de encriptação do lado do servidor oferecem características de gestão de chaves diferentes, o que pode escolher, de acordo com os requisitos:

- **O serviço gerido chaves**: disponibiliza uma combinação de controlo e a conveniência baixo overhead.

- **Gerida pelo cliente chaves**: oferece controlo sobre as chaves, incluindo o suporte de Bring Your Own chaves (BYOK), ou permite-lhe gerar novos.

- **Chaves de serviço gerido no hardware de cliente controlada**: permite-lhe gerir chaves no seu repositório proprietária, fora do controlo da Microsoft. Esta característica denomina-anfitrião sua própria chave (HYOK). No entanto, a configuração é complexa e serviços mais do Azure não suportam este modelo.

### <a name="azure-disk-encryption"></a>Encriptação de disco do Azure

Pode proteger máquinas virtuais Windows e Linux utilizando [encriptação de disco do Azure](azure-security-disk-encryption.md), que utiliza [BitLocker do Windows](https://technet.microsoft.com/library/cc766295(v=ws.10).aspx) tecnologia e Linux [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) para proteger ambos discos de sistema operativo e os discos de dados com a encriptação de volume completo.

Encriptação de chaves e segredos são salvaguardados no seu [subscrição do Cofre de chaves do Azure](../key-vault/key-vault-whatis.md). Ao utilizar o serviço de cópia de segurança do Azure, pode criar cópias de segurança e restaurar encriptadas máquinas de virtuais (VMs) que utilizam a configuração de chave de encriptação de chaves (KEK).

### <a name="azure-storage-service-encryption"></a>Encriptação do Serviço de Armazenamento do Azure

Dados Inativos no armazenamento de Blobs do Azure e partilhas de ficheiros do Azure podem ser encriptados em cenários de lado do servidor e do lado do cliente.

[Encriptação de serviço de armazenamento do Azure (SSE)](../storage/common/storage-service-encryption.md) automaticamente pode encriptar dados antes de esta está armazenada e automaticamente desencripta os dados quando é possível obter. O processo é completamente transparente para os utilizadores. Encriptação do serviço de armazenamento utiliza 256 bits [encriptação Advanced Encryption Standard (AES)](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), que é um das cifras de bloco maior disponíveis. AES processa encriptação, desencriptação e gestão de chaves transparente.

### <a name="client-side-encryption-of-azure-blobs"></a>Encriptação do lado do cliente de blobs do Azure

Pode efetuar do lado do cliente de várias formas de blobs de encriptação do Azure.

Pode utilizar a biblioteca de clientes de armazenamento do Azure para o pacote NuGet do .NET para encriptar os dados dentro das suas aplicações cliente antes de carregá-lo para o armazenamento do Azure.

Para obter mais informações sobre e transferir a biblioteca de clientes de armazenamento do Azure para o pacote NuGet do .NET, consulte [armazenamento do Microsoft Azure 8.3.0](https://www.nuget.org/packages/WindowsAzure.Storage).

Quando utilizar a encriptação do lado do cliente com o Cofre de chaves, os dados são encriptados utilizando uma única simétrica conteúdo encriptação de chave (CEK) que é gerado pelo cliente do Storage do Azure SDK. O CEK é encriptado utilizando uma chave de encriptação de chaves (KEK), que pode ser uma chave simétrica ou um par de chaves assimétricas. Pode geri-lo localmente ou armazene-o no Cofre de chaves. Os dados encriptados, em seguida, são carregados para o Storage do Azure.

Para obter mais informações sobre a encriptação do lado do cliente com o Cofre de chaves e começar a utilizar com instruções sobre como proceder, consulte o artigo [Tutorial: encriptar e desencriptar blobs no armazenamento do Azure utilizando o Cofre de chaves](../storage/storage-encrypt-decrypt-blobs-key-vault.md).

Por fim, também pode utilizar a biblioteca de clientes de armazenamento do Azure para Java para efetuar a encriptação do lado do cliente antes de carregar dados para o armazenamento do Azure e para desencriptar os dados quando transferir para o cliente. Esta biblioteca também suporta a integração com [Cofre de chaves](https://azure.microsoft.com/services/key-vault/) para gestão de chaves de conta de armazenamento.

### <a name="encryption-of-data-at-rest-with-azure-sql-database"></a>Encriptação de dados Inativos com a SQL Database do Azure

[Base de dados SQL do Azure](../sql-database/sql-database-technical-overview.md) é um serviço de base de dados relacional para fins gerais no Azure que suporta estruturas de dados relacionais, JSON, geográfico e XML. Base de dados SQL suporta tanto a encriptação do lado do servidor através da funcionalidade de encriptação de dados transparente (TDE), como a encriptação do lado do cliente através da funcionalidade sempre encriptado.

#### <a name="transparent-data-encryption"></a>Encriptação de Dados Transparente

[TDE](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) é utilizado para encriptar [do SQL Server](https://www.microsoft.com/sql-server/sql-server-2016), [SQL Database do Azure](../sql-database/sql-database-technical-overview.md), e [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) ficheiros de dados em tempo real, utilizando uma chave de encriptação da base de dados (DEK) , que é armazenado no registo de arranque da base de dados de disponibilidade durante a recuperação.

TDE protege os ficheiros de registo e dados, utilizando AES e Data encriptação Encryption Standard (3DES) algoritmos de encriptação. Encriptação do ficheiro de base de dados é efetuada ao nível da página. As páginas numa base de dados encriptado são encriptadas antes de que são escritos em disco e são desencriptados quando leem na memória. TDE está agora ativada por predefinição nas bases de dados SQL do Azure criados recentemente.

#### <a name="always-encrypted-feature"></a>Sempre encriptado funcionalidade

Com o [sempre encriptados](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) funcionalidade no SQL do Azure pode encriptar dados dentro de aplicações de cliente antes de armazenar no SQL Database do Azure. Também pode ativar a delegação da administração de base de dados no local a terceiros e manter a separação entre a quem o proprietário e pode ver os dados e a quem geri-lo, mas não devem ter acesso ao mesmo.

#### <a name="cell-level-or-column-level-encryption"></a>Encriptação de nível de células ou ao nível da coluna

Base de dados SQL do Azure, pode aplicar encriptação simétrica a uma coluna de dados, utilizando Transact-SQL. Esta abordagem é chamada [célula ao nível de encriptação ou a encriptação de nível de coluna (CLE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/encrypt-a-column-of-data), porque pode ser utilizado para encriptar colunas específicas ou mesmo específicas células de dados com diferentes chaves de encriptação. Se o fizer, dá-lhe mais granular capacidade de encriptação que TDE, que encripta os dados nas páginas.

CLE tem funções incorporadas que pode utilizar para encriptar os dados através da utilização de chaves simétricas ou assimétricas, a chave pública de um certificado ou uma frase de acesso utilizando 3DES.

### <a name="cosmos-db-database-encryption"></a>Encriptação de base de dados do cosmos DB

[BD do Azure do Cosmos](../cosmos-db/database-encryption-at-rest.md) é base de dados da Microsoft, globalmente distribuído, com vários modelo. Dados de utilizador que são armazenados na base de dados do Cosmos no armazenamento não volátil (unidades de estado sólido) são encriptados por predefinição. Não existem nenhum controlos para ativá-la ou desativar. Encriptação de Inativos é implementada através da utilização de uma série de tecnologias de segurança, incluindo sistemas de armazenamento de chaves segura, redes encriptados e APIs de criptografia. Chaves de encriptação são geridas pela Microsoft e rodam por internas diretrizes da Microsoft.

### <a name="at-rest-encryption-in-data-lake"></a>Encriptação em rest no Data Lake

[Azure Data Lake](../data-lake-store/data-lake-store-encryption.md) é um repositório de toda a empresa de cada tipo de dados recolhidos num único local antes de qualquer definição formal de esquema ou requisitos. Data Lake Store suporta "por predefinição," a encriptação transparente de dados inativos, que é configurado durante a criação da sua conta. Por predefinição, o Azure Data Lake Store gere as chaves para si, mas tem a opção para geri-los por si.

São utilizados três tipos de chaves de encriptação e desencriptação de dados: a chave de encriptação mestra (MEK), a chave de encriptação de dados (DEK) e a chave de encriptação de bloco (BEK). O MEK é utilizado para encriptar o DEK, que é armazenado no suporte de dados persistente, e o BEK é derivado do DEK e o bloco de dados. Se estiver a gerir as suas próprias chaves, pode rodar o MEK.

## <a name="encryption-of-data-in-transit"></a>Encriptação de dados em trânsito

O Azure oferece vários mecanismos para manter os dados privada são transmitidos de uma localização para outra.

### <a name="tlsssl-encryption-in-azure"></a>Encriptação de TLS/SSL no Azure

A Microsoft utiliza o [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) protocolo (TLS) para proteger dados quando é estiverem em deslocação entre os serviços de nuvem e os clientes. Os datacenters da Microsoft negociar uma ligação de TLS com sistemas de cliente que se ligam aos serviços do Azure. TLS fornece autenticação forte, privacidade de mensagens e integridade (Ativando a deteção de adulteração de mensagem, interception e falsificação), interoperabilidade, flexibilidade do algoritmo e facilidade de implementação e utilização.

[Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) protege ligações entre sistemas de cliente e cloud services da Microsoft dos clientes ao chaves exclusivas. Ligações também utilizam os comprimentos de chave de encriptação de 2,048-bit com base em RSA. Esta combinação torna difícil alguém para dados de acesso e intercept que se encontram em trânsito.

### <a name="azure-storage-transactions"></a>Transações de armazenamento do Azure

Quando o utilizador interage com o Storage do Azure através do portal do Azure, todas as transações ocorrer por HTTPS. Também pode utilizar a API de REST do Storage através de HTTPS para interagir com o Storage do Azure. Pode impor a utilização de HTTPS quando chamar as APIs REST para objetos de acesso em contas de armazenamento, permitindo a transferência segura necessárias para a conta de armazenamento.

Assinaturas de acesso partilhado ([SAS](../storage/storage-dotnet-shared-access-signature-part-1.md)), que pode ser utilizado para delegar o acesso a objetos de armazenamento do Azure, que incluem uma opção para especificar que apenas o protocolo HTTPS pode ser utilizado quando utiliza assinaturas de acesso partilhado. Esta abordagem garante que qualquer pessoa que envia ligações com SAS tokens utiliza o protocolo correto.

[O SMB 3.0](https://technet.microsoft.com/library/dn551363(v=ws.11).aspx#BKMK_SMBEncryption), que utilizado para aceder a ficheiros do Azure partilhas, suporta a encriptação e está disponível no Windows Server 2012 R2, Windows 8, Windows 8.1 e Windows 10. -Permite o acesso por várias regiões e até mesmo acesso no ambiente de trabalho.

A encriptação do lado do cliente encripta os dados antes de ser enviada para a instância de armazenamento do Azure, para que são encriptado conforme que circula na rede.

### <a name="smb-encryption-over-azure-virtual-networks"></a>Encriptação SMB através de redes virtuais do Azure 

Ao utilizar [SMB 3.0](https://support.microsoft.com/help/2709568/new-smb-3-0-features-in-the-windows-server-2012-file-server) em VMs que estejam a executar o Windows Server 2012 ou posterior, pode efetuar dados transferências proteger ao encriptar dados em trânsito através de redes virtuais do Azure. Através da encriptação de dados, ajudar a proteger contra adulteração e ataques de eavesdropping. Os administradores podem ativar a encriptação SMB para todo o servidor, ou partilhas apenas específicas.

Por predefinição, depois de encriptação SMB está ativada para uma partilha ou o servidor, são permitidos apenas os clientes do SMB 3.0 para aceder às partilhas de encriptados.

## <a name="in-transit-encryption-in-vms"></a>Encriptação em trânsito em VMs

Dados em trânsito para, de e entre as VMs que estejam a executar o Windows são encriptados de diversas formas, dependendo da natureza da ligação.

### <a name="rdp-sessions"></a>Sessões RDP

Pode ligar e iniciar sessão para uma VM utilizando o [protocolo RDP (Remote Desktop Protocol)](https://msdn.microsoft.com/library/aa383015(v=vs.85).aspx) de um computador cliente Windows ou de um Mac com um cliente RDP instalado. Dados em trânsito através da rede em sessões RDP podem ser protegidos por TLS.

Também pode utilizar o ambiente de trabalho remoto para ligar a uma VM com Linux no Azure.

### <a name="secure-access-to-linux-vms-with-ssh"></a>Proteger o acesso a VMs com Linux com SSH

Para a gestão remota, pode utilizar [Secure Shell](../virtual-machines/linux/ssh-from-windows.md) (SSH) para estabelecer a ligação para VMs com Linux em execução no Azure. SSH é um protocolo de ligação encriptada que permite que os inícios de sessão seguros através de ligações não segura. É o protocolo de ligação predefinido para VMs com Linux alojados no Azure. Ao utilizar chaves SSH para a autenticação, eliminar a necessidade de palavras-passe iniciar sessão. SSH utiliza um par de chaves públicas/privadas (encriptação assimétrico) para autenticação.

## <a name="azure-vpn-encryption"></a>Encriptação de VPN do Azure

Pode ligar ao Azure através de uma rede privada virtual que cria um túnel seguro para proteger a privacidade dos dados que está a ser enviados através da rede.

### <a name="azure-vpn-gateways"></a>Gateways de VPN do Azure

Pode utilizar um [gateway de VPN do Azure](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md) para enviar tráfego encriptado entre a rede virtual e a sua localização no local através de uma ligação pública, ou enviar o tráfego entre redes virtuais.

Utilize VPNs de site para site [IPsec](https://en.wikipedia.org/wiki/IPsec) para a encriptação de transporte. Gateways de VPN do Azure utilizam um conjunto de propostas de predefinição. Pode configurar gateways de VPN do Azure para utilizar uma política personalizada do IPsec/IKE com algoritmos criptográficos específicos e chave força da codificação, em vez de define a política predefinida do Azure.

### <a name="point-to-site-vpns"></a>VPNs ponto a site

VPNs ponto a site permitem ao cliente individual acesso de computadores a uma rede virtual do Azure. [Secure Socket Tunneling Protocol (SSTP)](https://technet.microsoft.com/library/2007.06.cableguy.aspx) é utilizado para criar o túnel VPN. -Pode atravessar as firewalls (o túnel aparece como uma ligação HTTPS). Pode utilizar a sua própria autoridade de certificação de raiz de infraestrutura de chaves públicas (PKI) interna (AC) para conetividade ponto a site.

Pode configurar uma ligação de VPN ponto a site a uma rede virtual com o portal do Azure com a autenticação de certificados ou o PowerShell.

Para saber mais sobre ligações de VPN ponto a site para redes virtuais do Azure, consulte:

[Configurar uma ligação de ponto a site para uma rede virtual, utilizando a autenticação de certificação: portal do Azure](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md) 

[Configurar uma ligação de ponto a site para uma rede virtual, utilizando a autenticação de certificado: PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="site-to-site-vpns"></a>VPNs de site a site 

Pode utilizar uma ligação de gateway VPN de site a site para ligar a sua rede no local a uma rede virtual do Azure através de um túnel VPN IPsec/IKE (IKEv1 ou IKEv2). Este tipo de ligação requer um dispositivo VPN no local que tenha um endereço IP público destinados atribuído.

Pode configurar uma ligação de VPN de site para site a uma rede virtual, utilizando o portal do Azure, o PowerShell ou a CLI do Azure.

Para obter mais informações, consulte:

[Criar uma ligação site a site no portal do Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

[Criar uma ligação site a site no PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

[Criar uma rede virtual com uma ligação de VPN de site para site ao utilizar a CLI](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="in-transit-encryption-in-data-lake"></a>Encriptação em trânsito no Data Lake

Os dados em trânsito (também denominados dados em movimento) também são sempre encriptados no Data Lake Store. Para além de encriptação de dados antes de armazenar no suporte de dados persistente, os dados também sempre protegidos em trânsito através de HTTPS. O HTTPS é o único protocolo que as interfaces REST do Data Lake Store suportam.

Para saber mais sobre a encriptação de dados em trânsito no Data Lake, veja [encriptação de dados no Data Lake Store](../data-lake-store/data-lake-store-encryption.md).

## <a name="key-management-with-key-vault"></a>Gestão de chaves com o Cofre de chaves

Sem proteção adequada e a gestão de chaves, a encriptação é composta inútil. O Cofre de chaves é a solução recomendadas pela Microsoft para gerir e controlar o acesso às chaves de encriptação utilizada pelos serviços de nuvem. Podem ser atribuídas permissões para chaves de acesso a serviços ou utilizadores através de contas do Azure Active Directory.

Tal não invalida possa Cofre de chaves organizações da necessidade de configurar, aplicar o patch e manter os módulos de segurança de hardware (HSMs) e o software de gestão de chaves. Quando utiliza o Cofre de chaves, manter o controlo. Microsoft nunca vê as chaves e as aplicações não tiverem acesso direto aos mesmos. Também pode importar ou gerar chaves nos HSMs.

## <a name="next-steps"></a>Passos Seguintes

- [Descrição geral da segurança do Azure](security-get-started-overview.md)
- [Descrição geral de segurança de rede do Azure](security-network-overview.md)
- [Descrição geral de segurança da base de dados do Azure](azure-database-security-overview.md)
- [Descrição geral de segurança de máquinas virtuais do Azure](security-virtual-machines-overview.md)
- [Encriptação de dados inativa](azure-security-encryption-atrest.md)
- [Melhores práticas de segurança e encriptação de dados](azure-security-data-encryption-best-practices.md)
