---
title: Recursos de dados de cliente do Azure Key Vault | Documentos da Microsoft
description: Saiba mais sobre os dados do cliente no Cofre de chaves
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 05/22/2018
ms.author: barclayn
ms.openlocfilehash: 1ddc74b1960095509a77d4b3072017847df42d90
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38235538"
---
# <a name="azure-key-vault-customer-data-features"></a>Recursos de dados de cliente do Azure Key Vault

O Azure Key Vault recebe dados de clientes durante a criação ou atualização de cofres, chaves, segredos, certificados e contas de armazenamento gerido. Esses dados de cliente são diretamente visíveis no portal do Azure e através da API REST. Dados dos clientes podem ser editados ou eliminados por atualizar ou eliminar o objeto que contém os dados.

Aceder aos registos de sistema são gerados quando um utilizador ou aplicação acede ao Cofre de chaves. Registos de acesso detalhadas estão disponíveis para clientes que utilizam as informações do Azure.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="identifying-customer-data"></a>Identificar os dados do cliente

As seguintes informações identificam os dados de cliente no Azure Key Vault:

- Políticas de acesso para o Azure Key Vault contenham IDs de objeto que representa os utilizadores, grupos ou aplicações
- Assuntos de certificado podem incluir endereços de e-mail ou outro utilizador ou identificadores organizacionais
- Contactos do certificado podem conter endereços de e-mail do utilizador, nomes ou números de telefone
- Emissores de certificados podem conter endereços de e-mail, nomes, números de telefone, as credenciais da conta e detalhes institucionais
- Etiquetas arbitrárias podem ser aplicadas a objetos no Azure Key Vault. Estes objetos incluem cofres, chaves, segredos, certificados e contas de armazenamento. As etiquetas utilizadas podem conter dados pessoais
- Registos de acesso do Cofre de chaves do Azure contêm os IDs de objeto [UPNs](../active-directory/connect/active-directory-aadconnect-userprincipalname.md)e endereços IP para cada chamada à REST API
- Registos de diagnóstico do Azure Key Vault podem conter IDs de objeto e endereços IP para chamadas da REST API

## <a name="deleting-customer-data"></a>A eliminação de dados do cliente

O mesmo REST APIs, experiência de Portal e SDKs utilizados para criar cofres, chaves, segredos, certificados e contas de armazenamento gerido, também são possível atualizar e eliminar estes objetos.

Eliminação de forma recuperável permite-lhe recuperar dados eliminados durante 90 dias após a eliminação. Ao utilizar a eliminação de forma recuperável, os dados podem ser eliminados permanentemente antes dos 90 dias, período de retenção expira, executando uma operação de remoção. Se o cofre ou a subscrição tiver sido configurada para bloquear remover operações, não é possível eliminar permanentemente os dados até que tenha passado o período de retenção agendado.

## <a name="exporting-customer-data"></a>Exportar dados do cliente

As APIs REST do mesmo, experiência do portal e SDKs que são utilizados para criar cofres, chaves, segredos e certificados e também são contas de armazenamento gerido permitem-lhe ver e exportar esses objetos.

O Azure Key Vault, o registo de acesso é uma funcionalidade opcional que pode ser ativada para gerar registos para cada chamada à REST API. Estes registos serão transferidos para uma conta de armazenamento na sua subscrição em que aplicar a política de retenção que cumpre os requisitos da sua organização.

O Azure Key Vault registos de diagnóstico que contêm dados pessoais podem ser obtidos ao fazer um pedido de exportação no portal de privacidade do utilizador. Este pedido deve ser feito pelo administrador de inquilinos.

## <a name="next-steps"></a>Passos Seguintes

- [Registo do Cofre de Chaves do Azure](key-vault-logging.md)

- [Descrição geral da eliminação de forma recuperável de Cofre de chaves do Azure](key-vault-soft-delete-cli.md)

- [Cofres](https://docs.microsoft.com/rest/api/keyvault/vaults)

- [Operações de chave de Cofre de chaves do Azure](https://docs.microsoft.com/rest/api/keyvault/key-operations)

- [Operações secretas do Cofre de chaves do Azure](https://docs.microsoft.com/rest/api/keyvault/secret-operations)

- [Certificados do Key Vault e as políticas do Azure](https://docs.microsoft.com/rest/api/keyvault/certificates-and-policies)

- [Emissores de certificados](https://docs.microsoft.com/rest/api/keyvault/certificate-issuers)

- [Operações de conta de armazenamento do Azure Key Vault](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations)
