---
title: O que é o cofre de chave do Azure? | Microsoft Docs
description: O Cofre de Chaves do Azure ajuda a salvaguardar as chaves criptográficas e os segredos utilizados pelas aplicações em cloud e pelos serviços. Ao utilizar o cofre de chave do Azure, os clientes podem encriptar chaves e segredos (tal como chaves de autenticação, chaves de conta de armazenamento, chaves de encriptação de dados, ficheiros .PFX e palavras-passe) utilizando as teclas que estejam protegidas por módulos de segurança de hardware (HSMs).
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e759df6f-0638-43b1-98ed-30b3913f9b82
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: barclayn
ms.openlocfilehash: 58bc3a582db23a48eedaaf67df7d20da9c42ded4
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45603282"
---
# <a name="what-is-azure-key-vault"></a>O que é o cofre de chave do Azure?

O Azure Key Vault ajuda a resolver os seguintes problemas
- **Gestão de segredos** -Azure Key Vault pode ser utilizado para armazenar de forma segura e totalmente controlar o acesso aos tokens, palavras-passe, certificados, chaves de API e outros segredos
- **A gestão de chaves** -Azure Key Vault também pode ser utilizado como uma solução de gestão de chaves. O Azure Key Vault torna mais fácil criar e controlar as chaves de encriptação utilizadas para encriptar os seus dados. 
- **Gestão de certificados** - Azure Key Vault também é um serviço que lhe permite facilmente aprovisionar, gerir e implementar certificados de Secure Sockets Layer/Transport Layer Security (SSL/TLS) públicos e privados para utilização com o Azure e ligadas interno recursos. 
- **Módulos de hardware de segurança** -os segredos e as chaves podem ser protegidas por software ou FIPS 140-2 nível 2 valida HSMs

## <a name="basic-concepts"></a>Conceitos básicos

O Azure Key Vault é uma ferramenta para armazenar e aceder a segredos em segurança. Um segredo é tudo cujo acesso deseja controlar rigidamente, como chaves de API, palavras-passe ou certificados. R **cofre** é grupo lógico de segredos aqui estão alguns termos-chave:
- **Inquilino**: um inquilino é a organização que possui e gere uma instância específica de serviços cloud da Microsoft. É utilizado mais frequentemente de forma exata para fazer referência ao conjunto de serviços do Azure e do Office 365 para uma organização.
- **Proprietário do Key Vault**: um proprietário do cofre pode criar um cofre de chaves e obter acesso total e controlo sobre o mesmo. O proprietário do cofre também pode configurar uma auditoria ao registo de quem acede a segredos e chaves. Os administradores podem controlar o ciclo de vida das chaves. Podem implementar uma nova versão da chave, fazer uma cópia de segurança e executar tarefas relacionadas.
- **Consumidor do cofre**: um consumidor do cofre pode efetuar ações nos recursos dentro do cofre de chaves quando o proprietário do cofre lhe concede acesso. As ações disponíveis dependem das permissões concedidas.
- **Recurso**: um recurso é um item gerível que está disponível através do Azure. Alguns recursos comuns são uma máquina virtual, conta de armazenamento, aplicação web, base de dados e rede virtual, mas existem muitos mais.
- **Grupo de recursos**: um grupo de recursos é um contentor que mantém recursos relacionados para uma solução do Azure. O grupo de recursos pode incluir todos os recursos para a solução ou apenas os recursos que pretende gerir como um grupo. Decida como pretende atribuir recursos a grupos de recursos com base no que é mais adequado para a sua organização.
- **Principal de serviço** -um Principal de serviço pode ser visto como uma credencial para a sua aplicação.
- **[Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md)**: o Azure AD é o serviço do Active Directory para um inquilino. Cada diretório tem um ou mais domínios. Um diretório pode ter várias subscrições associadas, mas apenas um inquilino. 
- **ID do inquilino do Azure**: um ID do inquilino é uma forma exclusiva para identificar uma instância do Azure AD numa subscrição do Azure.
- **Gerido identidades para recursos do Azure**: Azure Key Vault oferece uma forma de armazenar em segurança as credenciais e outras chaves e segredos, mas o seu código precisa para autenticar para o Key Vault para recuperá-los. Utilizar uma identidade gerida faz a resolver esse problema mais simples, fornecendo serviços do Azure uma identidade gerida automaticamente no Azure AD. Pode utilizar esta identidade para autenticar no Key Vault ou em qualquer serviço que suporta a autenticação Azure AD, sem ser necessário ter credenciais no seu código. Para obter mais informações, veja a imagem abaixo e o [geridos identidades para descrição geral de recursos do Azure](../active-directory/managed-identities-azure-resources/overview.md).

    ![Diagrama de como geridos identidades para funciona de recursos do Azure](./media/key-vault-whatis/msi.png)

## <a name="key-vault-roles"></a>Funções do Key Vault

Utilize a tabela seguinte para melhor compreender como a chave de cofre pode ajudar a satisfazer as necessidades dos programadores e dos administradores de segurança.

| Função | Declaração do problema | Resolvido pelo cofre de chave do Azure |
| --- | --- | --- |
| Programador de uma aplicação do Azure |"Quero criar uma aplicação para o Azure que utilize chaves para assinatura e encriptação, mas quero que essas chaves sejam externas à minha aplicação, para que a solução seja adequada a uma aplicação que é distribuída geograficamente. <br/><br/>Quero que as chaves e os segredos estejam protegidos, sem que tenha de escrever eu próprio o código. E também quero que seja fácil utilizar as chaves e os segredos a partir das minhas aplicações, com um desempenho ótimo”. |As chaves √ são armazenadas num cofre e invocadas pelo URI quando necessário.<br/><br/> √ As chaves são salvaguardadas pelo Azure, utilizando os algoritmos de norma da indústria, os comprimentos de chave e os módulos de segurança de hardware.<br/><br/> √ As chaves são processadas nos HSMs que residem no mesmos datacenters do Azure que as aplicações. Este método proporciona mais fiabilidade e latência reduzida do que as chaves que residem numa localização separada, como no local. |
| Programador de software como um serviço (SaaS) |"Não quero a responsabilidade ou um potencial encargo para chaves e segredos dos inquilino dos meus clientes. <br/><br/>Quero que os clientes sejam proprietários e efetuem a gestão das suas próprias chaves para que eu me possa concentrar no que sei fazer de melhor, ou seja, fornecer as principais funcionalidades de software." |Os clientes √ podem importar as suas próprias chaves para o Azure e geri-las. Se uma aplicação SaaS tiver de fazer operações de criptografia com as chaves dos respetivos clientes, o Cofre de Chaves faz essas operações em nome da aplicação. A aplicação não consegue ver as chaves dos clientes. |
| Responsável pela segurança (CSO) |"Quero saber se as nossas aplicações estão em conformidade com os HSMs FIPS 140-2 de nível 2 para uma gestão de chaves segura. <br/><br/>Pretendo certificar-me de que a minha organização está a controlar o ciclo de vida das chave e pode monitorizar a utilização das mesmas. <br/><br/>E embora utilizemos vários serviços e recursos do Azure, pretendo gerir as chaves a partir de uma única localização no Azure." |Os HSMs √ têm a certificação FIPS 140-2 de nível 2 validada.<br/><br/>A chave do cofre √ foi concebida para que a Microsoft não consulte ou extraia as suas chaves.<br/><br/>√ A utilização da chave é registada em tempo quase real.<br/><br/>√ O cofre fornece uma interface única, independentemente de quantos cofres tiver no Azure, de quantas regiões suporta e quais as aplicações em que os utiliza. |

Qualquer pessoa com uma subscrição do Azure pode criar e utilizar cofres de chave. Apesar do Key Vault beneficiar os programadores e os administradores de segurança, pode ser implementado e gerido pelo administrador de uma organização que gere outros serviços do Azure para uma organização. Por exemplo, este administrador pode iniciar sessão com uma subscrição do Azure, criar um cofre para a organização na qual pretende armazenar chaves e, em seguida, ser responsável por tarefas operacionais como:

* Criar ou importar uma chave ou segredo
* Revogar ou eliminar uma chave ou segredo
* Autorizar aos utilizadores ou aplicações o acesso à chave de cofre para que possam gerir ou utilizar as suas chaves e segredos
* Configurar a utilização da chave (por exemplo, iniciar sessão ou encriptar)
* Monitorizar a utilização da chave

Este administrador iria fornecer URIs aos programadores para os contactar a partir das suas aplicações e iria fornecer também ao respetivo administrador de segurança as informações de utilização da chave de registo. 

![Descrição geral do cofre de chave do Azure][1]

Os programadores também podem gerir as chaves diretamente, com APIs. Para obter mais informações, consulte o [manual do programador da chave](key-vault-developers-guide.md).

## <a name="next-steps"></a>Passos Seguintes

Para consultar um tutorial de introdução para um administrador, veja [Introdução ao Azure Key Vault](key-vault-get-started.md).

Para obter mais informações sobre o registo de utilização do Key Vault, veja [Registo do Azure Key Vault](key-vault-logging.md).

Para obter mais informações sobre a utilização de chaves e segredos com o Azure Key Vault, veja [Sobre chaves, segredos e certificados](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx).

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
O Cofre de Chaves do Azure chave está disponível na maior parte das regiões. Para obter mais informações, consulte a [página de preços do Cofre de Chaves](https://azure.microsoft.com/pricing/details/key-vault/).
