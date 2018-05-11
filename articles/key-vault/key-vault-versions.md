---
title: Versões do Cofre de chaves
description: As várias versões do Cofre de chaves do Azure
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e8622dcc-59a3-4f4b-9f63-cd2232515a65
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: beb73be66f36ccf95fe27d4d8128106cd12722a8
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="key-vault-versions"></a>Versões do Cofre de chaves

## <a name="2016-10-01---managed-storage-account-keys"></a>2016-10-01 - chaves de conta de armazenamento gerido

Summer 2017 - funcionalidade de chaves de conta de armazenamento adicionada mais fácil integração com o Storage do Azure. Consulte o tópico de descrição geral para obter mais informações, [descrição geral de chaves de conta de armazenamento gerido](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-storage-keys).

## <a name="2016-10-01---soft-delete"></a>2016-10-01-eliminar de forma recuperável

Summer 2017 - funcionalidade de eliminação de forma recuperável adicionada melhorada para proteção de dados dos seus cofres de chaves e o Cofre de chaves objetos. Consulte o tópico de descrição geral para obter mais informações, [descrição geral da eliminação de forma recuperável](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete).

## <a name="2015-06-01---certificate-management"></a>2015-06-01 - gestão de certificados

Gestão de certificados é adicionada como uma funcionalidade para a versão GA 2015-06-01 no 26 de Setembro de 2016.

## <a name="2015-06-01---general-availability"></a>2015-06-01 - disponibilidade geral

Disponibilidade geral versão 2015-06-01, anunciado no 24 de Junho de 2015.

As seguintes alterações foram efetuadas nesta versão:

- Eliminar uma chave - "utilizar" campo removido.
- Obter informações sobre uma chave - "utilizar" campo removido.
- Importar uma chave para um cofre - "utilizar" campo removido.
- Restaurar uma chave - "utilizar" campo removido.
- Alteradas "RSA_OAEP" para "RSA-OAEP" para algoritmos RSA. Consulte [sobre certificados, chaves e segredos](about-keys-secrets-and-certificates.md).

## <a name="2015-02-01-preview"></a>2015-02-01-preview 

Segundo pré-visualização versão 2015-02-01-preview, anunciados no 20 de Abril de 2015. Para obter mais informações, consulte [atualização da API de REST](http://blogs.technet.com/b/kv/archive/2015/04/20/empty-3.aspx) blogue.

Foram atualizadas as seguintes tarefas:

- Liste as chaves num cofre - paginação adicionado suporte para a operação.
- Lista as versões de uma chave - adicionar operação de lista as versões de uma chave.
- Lista os segredos num cofre - suporte de paginação adicionado.
- Versões do segredo de lista - Adicionar operação de lista as versões do segredo.
- Todas as operações - Adicionar carimbos criar/atualizar atributos.
- Criar um segredo - adicionar o tipo de conteúdo a segredos.
- Criar uma chave - adicionar etiquetas como informações opcionais.
- Criar um segredo - adicionar etiquetas como informações opcionais.
- Atualizar uma chave - adicionar etiquetas como informações opcionais.
- Atualizar um segredo - adicionar etiquetas como informações opcionais.
- Alterar o tamanho máximo de segredos de 10 mil para KBytes 25. Ver, [sobre certificados, chaves e segredos](about-keys-secrets-and-certificates.md).

## <a name="2014-12-08-preview"></a>pré-visualização 2014-12-08

Primeiro pré-visualização versão pré-visualização 2014-12-08-, anunciados no 8 de Janeiro de 2015.

## <a name="see-also"></a>Consulte também
- [Sobre chaves, segredos e certificados](about-keys-secrets-and-certificates.md)
