---
title: Descrição Geral do Azure Key Vault
description: O Azure Key Vault é um serviço cloud que funciona como um arquivo de segredos seguro.
services: key-vault
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 34af20ee-3fa7-4f28-9d98-6168b1759764
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.date: 07/17/2018
ms.author: barclayn
ms.openlocfilehash: 2cda30c85ce8a8dc9b7a6c0134b7cabc58b842a4
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39115325"
---
# <a name="what-is-azure-key-vault"></a>O que é o cofre de chave do Azure?

O Azure Key Vault é um serviço cloud que funciona como um arquivo de segredos seguro.

Para que as aplicações funcionem, é preciso ter palavras-passe, cadeias de ligação e outras informações. É essencial garantir que essas informações estão disponíveis, mas armazenadas em segurança. É aí que o Azure Key Vault pode ajudar. O Azure Key Vault pode ajudá-lo a arquivar e gerir, de forma segura, os segredos das aplicações.

O Key Vault permite-lhe criar vários contentores seguros, chamados “cofres”. Estes cofres são apoiados por módulos de segurança de hardware (HSMs). Os cofres centralizam o armazenamento dos segredos das aplicações, o que ajuda a reduzir as possibilidades de perda acidental de informações de segurança. Os cofres de chaves também controlam e registam o acesso a tudo o que está armazenado nos mesmos. O Azure Key Vault pode processar pedidos e renovações de certificados Transport Layer Security (TLS), proporcionando as funcionalidades inerentes a uma solução de gestão de ciclo de vida de certificados robusta.

 O Azure Key Vault foi concebido para suportar chaves e segredos de aplicações. O Key Vault não se destina a ser utilizado como arquivo para palavras-passe de utilizadores.

## <a name="why-use-azure-key-vault"></a>Porquê utilizar o Azure Key Vault?

### <a name="centralize-application-secrets"></a>Centralizar os segredos das aplicações

Ao centralizar o armazenamento dos segredos das aplicações no Azure Key Vault, pode controlar a distribuição dos mesmos. Desta forma, está a reduzir grandemente as possibilidades de haver fugas de segredos acidentais. Se o Key Vault for utilizado, os programadores de aplicações já não precisam de armazenar informações de segurança nas aplicações. Assim, a necessidade de incluir essas informações como parte do código é eliminada. Por exemplo, uma aplicação poderá ter de se ligar a uma base de dados. Em vez de armazenar a cadeia de ligação nos códigos da aplicação, vai armazená-los de forma segura no Key Vault.

As suas aplicações podem aceder em segurança às informações de que precisam através de URIs que lhes permitem obter versões específicas de um segredo depois de a chave ou o segredo da aplicação ser armazenado no Azure Key Vault. Isto acontece sem que seja necessário escrever código personalizado para proteger qualquer uma das informações dos segredos.

### <a name="securely-store-secrets-and-keys"></a>Guarde os segredos e chaves em segurança

Os segredos e as chaves são salvaguardadas pelo Azure, utilizando os algoritmos de norma da indústria, os comprimentos de chave e os módulos de segurança de hardware (HSMs). Os HSMs têm validação da norma federal norte-americana Federal Information Processing Standards (FIPS) 140-2 Nível 2.

O acesso a um cofre de chaves requer autenticação e autorização adequadas antes de um “chamador” (utilizador ou aplicação) poder obter acesso. A autenticação estabelece a identidade do chamador, ao passo que a autorização determina as operações que aquele pode fazer.

A autenticação é feita através do Azure Active Directory. A autorização pode ser feita através do controlo de acesso baseado em funções (RBAC) ou de uma política de acesso do Key Vault. O RBAC é utilizado para lidar com a gestão dos cofres e a política de acesso do Key Vault é utilizada para o acesso aos dados armazenados nos cofres.

O Azure Key Vaults pode ser protegido por HSMs de software ou hardware. Em casos em que seja preciso mais segurança, pode importar ou gerar chaves nos módulos de segurança de hardware (HSMs) que nunca deixam o limite do HSM. A Microsoft utiliza os módulos de hardware de segurança da Thales. Pode utilizar ferramentas da Thales para mover uma chave do seu HSM para o Azure Key Vault.

Por fim, o Azure Key Vault foi concebido para que a Microsoft não veja ou extraia os seus dados.

### <a name="monitor-access-and-use"></a>Monitorizar o acesso e a utilização

Depois de criar alguns cofres de chaves, vai querer monitorizar como e quando é que as chaves e segredos são acedidos. Para tal, pode ativar o registo no Key Vault. Pode configurar o Azure Key Vault para:

- Arquivar numa conta de armazenamento.
- Transmitir em fluxo para um hub de eventos.
- Enviar os registos para o Log Analytics.

Tem controlo sobre os seus registos, pode protegê-los ao restringir o acesso e também pode eliminar aqueles de que já não precisa.

### <a name="simplified-administration-of-application-secrets"></a>Administração simplificada de segredos de aplicações

Quando armazena dados valiosos, tem de executar vários passos. As informações de segurança têm de estar protegidas, têm de seguir um ciclo de vida e têm de ter elevada disponibilidade. O Azure Key Vault simplifica bastante tudo isto ao:

- Eliminar a necessidade de conhecimentos de HSMs por parte dos utilizadores da sua organização.
- Aumentar verticalmente com pouca antecedência, para satisfazer os picos de utilização da sua organização.
- Replicar os conteúdos do seu cofre de chaves numa região e para uma região secundária. Isto garante elevada disponibilidade e faz com que o administrador não tenha de fazer nada para acionar a ativação pós-falha.
- Disponibilizar opções de administração padrão do Azure através do portal, da CLI do Azure e do PowerShell.
- Automatizar determinadas tarefas relativas a certificados que comprar junto de ACs públicas, como tarefas de inscrição e renovação.

Além disso, o Azure Key Vault permite-lhe segregar segredos das aplicações. As aplicações só podem aceder ao cofre ao qual têm autorização e estão limitadas à realização de determinadas operações. Pode criar um cofre de chaves do Azure por aplicação e restringir os segredos armazenados num cofre a uma aplicação e a uma equipa de programadores específica.

### <a name="integrate-with-other-azure-services"></a>Integrar noutros serviços do Azure

Enquanto arquivo seguro no Azure, o Key Vault tem sido utilizado para simplificar cenários como [Azure Disk Encryption](../security/azure-security-disk-encryption.md), a funcionalidade [Always Encrypted]( https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) no SQL Server e no Azure SQL e [Aplicações Web do Azure]( https://docs.microsoft.com/azure/app-service/web-sites-purchase-ssl-web-site). O próprio Key Vault pode ser integrado em contas de armazenamento, em hubs de eventos e no Log Analytics.

## <a name="next-steps"></a>Passos seguintes

- [Quickstart: Create an Azure Key Vault using the CLI](quick-create-cli.md) (Início Rápido: Criar um Cofre de Chaves do Azure com a CLI do Azure)
- [Configure an Azure web application to read a secret from Key vault](tutorial-web-application-keyvault.md) (Configurar uma aplicação Web do Azure para ler segredos em cofres de segurança)