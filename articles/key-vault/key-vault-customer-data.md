---
title: Funcionalidades de dados de cliente de Cofre de chaves do Azure | Microsoft Docs
description: Saiba mais sobre os dados de cliente no Cofre de chaves
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
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34637366"
---
# <a name="azure-key-vault-customer-data-features"></a>Funcionalidades de dados de cliente de Cofre de chaves do Azure

O Cofre de chaves do Azure recebe dados de cliente durante a criação ou atualização de cofres, chaves, segredos, certificados e contas de armazenamento gerido. Estes dados de cliente são diretamente visíveis no portal do Azure e através da API REST. Dados de cliente podem ser editados ou eliminados por atualizar ou eliminar o objeto que contém os dados.

Os registos de acesso do sistema são gerados quando um utilizador ou aplicação acede ao Cofre de chaves. Acesso de detalhado estão disponíveis registos para clientes que utilizam as informações do Azure.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="identifying-customer-data"></a>Identificar dados de cliente

As seguintes informações identificam os dados de cliente no Cofre de chaves do Azure:

- Políticas de acesso para o Cofre de chaves do Azure contenham IDs de objeto que representa os utilizadores, grupos ou aplicações
- Assuntos de certificado podem incluir endereços de e-mail ou outro utilizador ou identificadores organizacionais
- Contactos do certificado podem conter endereços de e-mail do utilizador, nomes ou números de telefone
- Emissores de certificados podem conter endereços de correio eletrónico, nomes, números de telefone, as credenciais da conta e detalhes organizacionais
- Etiquetas arbitrários podem ser aplicadas a objetos no Cofre de chaves do Azure. Tais objetos incluem cofres, chaves, segredos, certificados e contas de armazenamento. As etiquetas utilizadas podem conter dados pessoais
- Os registos de acesso do Cofre de chaves do Azure conter IDs de objeto, [UPNs](../active-directory/connect/active-directory-aadconnect-userprincipalname.md)e endereços IP para cada chamada de REST API
- Os registos de diagnóstico Cofre de chaves do Azure podem conter IDs de objeto e endereços IP para chamadas da REST API

## <a name="deleting-customer-data"></a>Eliminar dados de cliente

O mesmo REST APIs, experiência de Portal e SDKs utilizados para criar cofres, chaves, segredos, certificados e contas de armazenamento gerido, também são capazes de atualizar e eliminar estes objetos.

Eliminação de forma recuperável permite-lhe recuperar dados eliminados 90 dias após a eliminação. Quando utilizar a eliminação de forma recuperável, os dados podem ser eliminados permanentemente antes dos 90 dias, período de retenção expira, efetuando uma operação de remoção. Se o cofre ou uma subscrição tiver sido configurada para bloquear remover operações, não é possível eliminar permanentemente os dados até ter passado o período de retenção agendado.

## <a name="exporting-customer-data"></a>Exportar dados de cliente

O mesmo APIs REST, experiência do portal e SDKs que são utilizados para criar chaves, cofres, certificados e segredos e também são contas de armazenamento gerido permitem-lhe ver e exportar esses objetos.

Registo de acesso é uma funcionalidade opcional que pode ser ativada para o Cofre de chaves do Azure gera registos para cada chamada de REST API. Estes registos serão transferidos para uma conta de armazenamento na sua subscrição onde aplicar a política de retenção que cumpra os requisitos da sua organização.

Registos diagnóstico do Azure do Cofre de chaves, que contém os dados pessoais podem ser obtidos ao efetuar um pedido de exportação no portal de privacidade do utilizador. Este pedido têm de ser efetuado pelo administrador do inquilino.

## <a name="next-steps"></a>Passos Seguintes

- [Registo do Cofre de Chaves do Azure](key-vault-logging.md)

- [Descrição geral da eliminação de forma recuperável Cofre de chaves do Azure](key-vault-soft-delete-cli.md)

- [cofres](https://docs.microsoft.com/rest/api/keyvault/vaults)

- [Operações de chaves de Cofre de chaves do Azure](https://docs.microsoft.com/rest/api/keyvault/key-operations)

- [Operações de segredo Cofre de chaves do Azure](https://docs.microsoft.com/rest/api/keyvault/secret-operations)

- [Políticas e certificados de Cofre de chaves do Azure](https://docs.microsoft.com/rest/api/keyvault/certificates-and-policies)

- [Emissores de certificados](https://docs.microsoft.com/rest/api/keyvault/certificate-issuers)

- [Operações de conta de armazenamento de Cofre de chaves do Azure](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations)
