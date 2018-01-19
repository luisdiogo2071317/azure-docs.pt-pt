---
title: "Notas de versão do .NET 2 API do Cofre de chaves | Microsoft Docs"
description: "Os programadores de .NET irão utilizar esta API código para o Cofre de chaves do Azure"
services: key-vault
author: lleonard-msft
manager: mbaldwin
editor: alleonar
ms.assetid: 1cccf21b-5be9-4a49-8145-483b695124ba
ms.service: key-vault
ms.devlang: CSharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/02/2017
ms.author: alleonar
ms.openlocfilehash: a7735f8c1c4332bf2472bc83c0c37baf49019004
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Cofre de chaves do Azure .NET 2.0 - Guia de migração e notas de versão
As informações seguintes ajudam a migrar para a versão 2.0 da biblioteca do Cofre de chaves do Azure para c# e .NET.  Aplicações escritas para versões anteriores tem de atualizar para suportar a versão mais recente.  Estas alterações são necessários para suportar totalmente as funcionalidades novas e melhoradas, tais como **certificados do Cofre de chaves**.

## <a name="key-vault-certificates"></a>Certificados de Cofre de chaves

Certificados de Cofre de chaves gerir x509 certificados e suporta os comportamentos seguintes:  

* Criar certificados através de um processo de criação do Cofre de chaves ou importar certificado existente. Isto inclui ambos autoassinados e autoridade de certificação (CA) gerado certificados.
* Armazenar e gerir x509 em segurança armazenamento de certificados sem interação com o material de chave privada.  
* Defina políticas que direcionam o Cofre de chaves para gerir o ciclo de vida do certificado.  
* Forneça as informações de contacto para eventos de ciclo de vida, tais como avisos de expiração e notificações de renovação.  
* Renove automaticamente os certificados de emissores selecionados (fornecedores de certificados de parceiro X509 do Cofre de chaves e autoridades de certificação). * certificado de suporte do alternativo (não-partner) fornece e autoridades de certificados (não suporta a renovação automática).  

## <a name="net-support"></a>Suporte de .NET

* **O .NET 4.0** não é suportada pela versão 2.0 do biblioteca de .NET de Cofre de chaves do Azure
* **.NET framework 4.5.2** é suportada pela versão 2.0 do biblioteca de .NET de Cofre de chaves do Azure
* **1.4 padrão .NET** é suportada pela versão 2.0 do biblioteca de .NET de Cofre de chaves do Azure

## <a name="namespaces"></a>Espaços de nomes

* O espaço de nomes para **modelos** é alterado de **Microsoft.Azure.KeyVault** para **Microsoft.Azure.KeyVault.Models**.
* O **Microsoft.Azure.KeyVault.Internal** espaço de nomes é ignorado.
* Tem os seguintes espaços de nomes de dependências de SDK do Azure 

    - **Hyak.Common** está agora **Microsoft.Rest**.
    - **Hyak.Common.Internals** está agora **Microsoft.Rest.Serialization**.

## <a name="type-changes"></a>Alterações de tipo

* *Segredo* alterado para *SecretBundle*
* *Dicionário* alterado para *IDictionary*
* *Lista<T>, string []* alterado para *IList<T>*
* *NextList* alterado para *NextPageLink*

## <a name="return-types"></a>Tipos de retorno

* **KeyList** e **SecretList** agora devolve *IPage<T>*  em vez de *ListKeysResponseMessage*
* Gerado **BackupKeyAsync** agora devolve *BackupKeyResult*, que contém *valor* (BLOBs de cópia de segurança). Anteriormente, o método foi encapsulado e apenas o valor devolvido.

## <a name="exceptions"></a>Exceções

* *KeyVaultClientException* é alterado para *KeyVaultErrorException*
* O erro do serviço mudou de *exceção. Erro* para *exceção. Body.Error.Message*.
* Removido informações adicionais da mensagem de erro para **[JsonExtensionData]**.

## <a name="constructors"></a>Construtores

* Em vez de a aceitar uma *HttpClient* como um argumento de construtor, o construtor apenas aceita *HttpClientHandler* ou *DelegatingHandler [-]*.

## <a name="downloaded-packages"></a>Pacotes transferidos

Quando um cliente processa uma dependência do Cofre de chaves, são transferidos os seguintes pacotes:

### <a name="previous-package-list"></a>Lista de pacotes anterior

* `package id="Hyak.Common" version="1.0.2" targetFramework="net45"`
* `package id="Microsoft.Azure.Common" version="2.0.4" targetFramework="net45"`
* `package id="Microsoft.Azure.Common.Dependencies" version="1.0.0" targetFramework="net45"`
* `package id="Microsoft.Azure.KeyVault" version="1.0.0" targetFramework="net45"`
* `package id="Microsoft.Bcl" version="1.1.9" targetFramework="net45"`
* `package id="Microsoft.Bcl.Async" version="1.0.168" targetFramework="net45"`
* `package id="Microsoft.Bcl.Build" version="1.0.14" targetFramework="net45"`
* `package id="Microsoft.Net.Http" version="2.2.22" targetFramework="net45"`

### <a name="current-package-list"></a>Lista de pacote atual

* `package id="Microsoft.Azure.KeyVault" version="2.0.0-preview" targetFramework="net45"`
* `package id="Microsoft.Rest.ClientRuntime" version="2.2.0" targetFramework="net45"`
* `package id="Microsoft.Rest.ClientRuntime.Azure" version="3.2.0" targetFramework="net45"`

## <a name="class-changes"></a>Alterações de classe

* **UnixEpoch** classe foi removida.
* **Base64UrlConverter** classe é mudada para **Base64UrlJsonConverter**.

## <a name="other-changes"></a>Outras alterações

* Foi adicionado suporte para a configuração de política de repetição de operação de KV em falhas transitórias para esta versão da API.

## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet

* Para as operações que devolveram um *cofre*, o tipo de retorno foi uma classe que continha um **cofre** propriedade. O tipo de retorno é agora *cofre*.
* *PermissionsToKeys* e *PermissionsToSecrets* estão agora *Permissions.Keys* e *Permissions.Secrets*
* Aplicam determinadas alterações de tipos de retorno para o controlo-plane bem.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet

* O pacote foi interrompido até **Microsoft.Azure.KeyVault.Extensions** e **Microsoft.Azure.KeyVault.Cryptography** para as operações de criptografia.

