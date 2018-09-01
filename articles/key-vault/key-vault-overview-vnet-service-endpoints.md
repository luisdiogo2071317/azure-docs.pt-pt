---
ms.assetid: ''
title: Cofre de chaves de pontos finais de serviço VNET para o Azure | Documentos da Microsoft
description: Descrição geral de pontos finais de serviço de rede Virtual para o Cofre de chaves
services: key-vault
author: amitbapat
ms.author: ambapat
manager: mbaldwin
ms.date: 08/31/2018
ms.service: key-vault
ms.workload: identity
ms.topic: article
ms.openlocfilehash: fd3a16841e9d3c9b8bfd9b0f53c42ad1e08b3d80
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43345077"
---
# <a name="virtual-network-service-endpoints-for-azure-key-vault"></a>Pontos finais de serviço de rede virtual para o Azure Key Vault

O virtual rede pontos finais de serviço para o Key Vault permite-lhe restringir o acesso à rede Virtual especificado e/ou a uma lista de intervalos de endereços IPv4 (protocolo IP versão 4). Qualquer autor da chamada a ligar ao seu Cofre de chaves de fora essas origens de acesso será negado. Se cliente tiver optado por participar para permitir que os "Serviços Microsoft fidedignos", como o Office 365 Exchange Online, SharePoint Online do Office 365, Azure computação, o Azure Resource Manager, etc. de cópia de segurança do Azure, ligações a partir desses serviços permitirá através da firewall. É claro, esses chamadores ainda tem de apresentar um token do AAD válido e deve ter permissões de (configuradas como políticas de acesso) para efetuar a operação pedida. Conheça os detalhes mais técnicos [pontos finais de serviço de rede Virtual](../virtual-network/virtual-network-service-endpoints-overview.md).

## <a name="usage-scenarios"></a>Cenários de utilização

Pode configurar [Key Vault firewalls e redes virtuais](key-vault-network-security.md) para negar acesso para o tráfego de todas as redes (incluindo o tráfego de Internet) por predefinição. O acesso pode ser concedido para tráfego de redes virtuais do Azure específicas e/ou a internet pública, endereço IP, intervalos, que lhe permite criar um limite de rede segura para as suas aplicações.

> [!NOTE]
> Firewalls de Key Vault e regras de rede virtual só se aplicam ao Cofre de chaves [plano de dados](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control). Operações de plano de controlo do Cofre de chaves (por exemplo, o Cofre de chaves, criar, eliminar, operações, definir políticas de acesso, a definição de firewalls e regras de rede virtual de modificar) não são afetadas por firewalls e regras de rede virtual.

Por exemplo,
* Se estiver a utilizar o Key Vault para armazenar chaves de encriptação, segredos da aplicação, certificados e pretende bloquear o acesso ao seu Cofre de chaves a partir da internet pública.
* Pretende bloquear o acesso ao seu Cofre de chaves para que apenas seu aplicativo ou uma lista curta de hosts designados possam ligar ao seu Cofre de chaves
* Tem uma aplicação em execução na sua rede virtual do Azure (VNET) e permanece bloqueada nesta VNET para todo o tráfego de entrada e saído. Seu aplicativo ainda precisa de ligar ao Cofre de chaves para obter segredos ou certificados ou utilizar as chaves criptográficas.

## <a name="configure-key-vault-firewalls-and-virtual-networks"></a>Configurar o Cofre de chaves firewalls e redes virtuais

Aqui estão as etapas necessárias para configurar firewalls e redes virtuais. Estes passos permanecem iguais, independentemente de qual interface (PowerShell, CLI, o portal do Azure) irá utilizar para configurar a firewall e regras de rede virtual.
1. Opcional mas altamente recomendado: ative [registo de Cofre de chaves](key-vault-logging.md) para ver os registos de acesso detalhadas. Isto irá ajudá-lo no diagnóstico ao firewalls e regras de rede virtual impedem o acesso a um cofre de chaves.
2. Ativar "pontos finais de serviço para o key vault" para o destino rede ou redes virtuais e sub-redes
3. Definir regras de rede virtual para um cofre de chaves restringir o acesso ao Cofre de chaves de rede ou redes virtuais específicos, sub-redes e intervalos de endereços IPv4 e de firewalls.
4. Se este Cofre de chaves tem de ser acessível para todos os serviços Microsoft fidedignos, terá de ativar a opção para permitir que os "Serviços de Azure fidedigno" ligar ao Cofre de chaves.

Consulte a [configurar o Azure Key Vault Firewalls e redes virtuais](key-vault-network-security.md) para obter instruções passo a passo detalhadas.

> [!IMPORTANT]
> Assim que as regras de firewall estão em vigor, todos os Key Vault [plano de dados](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) operações só podem ser executadas quando o chamador pedidos provêm da rede ou redes virtuais permitidas ou intervalos de endereços IPV4. Isso também se aplica a aceder ao Cofre de chaves a partir do portal do Azure. Enquanto um usuário pode browser para um cofre de chaves a partir do portal do Azure, eles talvez não consiga lista chaves/segredos/certificados se os respetivos computadores cliente não está na lista de permitidos. Isso também afeta o 'selecionador de Cofre de chave' por outros serviços do Azure. Os utilizadores podem ser capazes de ver a lista de cofres de chaves, mas não listar as chaves, se as regras de firewall impedem os respetivos computadores cliente.


> [!NOTE]
> * São permitidas um máximo 127 as regras de VNET e 127 regras de IPv4. 
> * Intervalos de endereços pequenos com "/ 31" ou "/ 32" prefixo tamanhos não são suportados. Estes intervalos devem ser configurados com regras de endereços IP individuais.
> * Regras de rede IP só são permitidas para endereços IP públicos. Intervalos de endereços IP reservados para redes privadas (conforme definido em RFC 1918) não são permitidos em regras de IP. Redes privadas incluem endereços que comecem com * 10.* *, * 172.16. * *, e * 192.168. * *. 
> * Apenas endereços IPv4 são suportados neste momento.

## <a name="trusted-services"></a>Serviços confiáveis
Aqui está uma lista de serviços confiáveis que têm permissão para aceder um cofre de chaves, se a opção "Permitir fidedigna services" está ativada.

|Serviço de confiança|Cenários de utilização|
| --- | --- |
|Serviço de implementação de Máquinas Virtuais do Azure|[Implementar certificados para VMs a partir do Cofre de chaves geridas pelo cliente](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)|
|Serviço de implementação de modelos do Azure Resource Manager|[Transmitir valores seguros durante a implementação](../azure-resource-manager/resource-manager-keyvault-parameter.md)|
|Azure Disk Encryption para serviço encriptação de volumes|Permitir o acesso à chave do BitLocker (Windows VM) ou a frase de acesso do DM (Linux VM) e a chave de encriptação de chave durante a implementação de VM para ativar [Azure Disk Encryption](../security/azure-security-disk-encryption.md)|
|Azure Backup|Permitir cópia de segurança e restauro de relevantes chaves e segredos durante a cópia de segurança de VM do Azure, utilizando [cópia de segurança do Azure](../backup/backup-introduction-to-azure-backup.md)|
|O Exchange Online e SharePoint Online|Permitir o acesso à chave do cliente para a encriptação de serviço com [chave de cliente](https://support.office.com/en-us/article/Controlling-your-data-in-Office-365-using-Customer-Key-f2cd475a-e592-46cf-80a3-1bfb0fa17697).|
|Azure Information Protection|Permitir o acesso à chave de inquilino para [do Azure Information Protection.](https://docs.microsoft.com/azure/information-protection/what-is-information-protection)|
|Serviços Aplicacionais|[Implementar o certificado de aplicação Web do Azure através do Key Vault](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)|
|SQL do Azure|[Encriptação de dados transparente com suporte de traga a sua própria chave para a base de dados do Azure SQL e o armazém de dados](../sql-database/transparent-data-encryption-byok-azure-sql.md?view=sql-server-2017&viewFallbackFrom=azuresqldb-current)|
|Storage do Azure|[Encriptação do serviço de armazenamento a utilizar chaves geridas pelo cliente no Azure Key Vault](../storage/common/storage-service-encryption-customer-managed-keys.md)|
|Azure Data Lake Store|[Encriptação de dados no Azure Data Lake Store](../data-lake-store/data-lake-store-encryption.md) com o cliente gerido chave|



> [!NOTE]
> As políticas de acesso do Cofre de chaves relevantes tem de ser definidas para permitir que os serviços correspondentes obter acesso ao Cofre de chaves.

## <a name="next-steps"></a>Passos Seguintes

* [Proteger o seu cofre de chaves](key-vault-secure-your-key-vault.md)
* [Configurar o Cofre de chaves do Azure Firewalls e redes virtuais](key-vault-network-security.md)