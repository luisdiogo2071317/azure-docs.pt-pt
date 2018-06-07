---
title: Diretrizes para o Azure Marketplace e AppSource publicador | Azure
description: Diretrizes para o Azure Marketplace e AppSource para publicadores de aplicações e serviço
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: jm-aditi-ms
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/05/2018
ms.author: ellacroi
ms.openlocfilehash: 135f934cd6b352dad9e4cea5a14406804f31b66b
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825216"
---
# <a name="guidelines"></a>Orientações  

<!--
## Guidelines for AppSource  
-->
---  

## <a name="guidelines-for-azure-marketplace"></a>Diretrizes para o Azure Marketplace  

### <a name="guidelines-for-creating-a-microsoft-id-to-manage-a-marketplace-account"></a>Diretrizes para criar uma ID da Microsoft para gerir uma conta do marketplace  
Se mais do que uma pessoa necessita de acesso para o mesmo ID Microsoft utilizado para criar a sua conta do marketplace, em seguida, deve seguir estas diretrizes para ajudar a criar uma conta da empresa. 

>[!IMPORTANT]
>Para autorizar vários utilizadores a aceder à sua conta Microsoft Developer Center (Dev Center), a Microsoft recomenda que utilize o Azure Active Directory (Azure AD) para atribuir funções a utilizadores individuais. Cada utilizador têm de aceder a conta ao iniciar sessão com o indivíduo credenciais do Azure AD. Criar o seu ID de Microsoft utilizando um endereço de e-mail num domínio registado para a sua empresa Microsoft sugere que o e-mail não ser atribuídos a uma pessoa. Um exemplo é `windowsapps@fabrikam.com`.  
>*   Para obter mais informações, visite o [problema: ID da Microsoft num Azure AD federado domínio](#issue:-microsoft-id-in-an-azure-ad-federated-domain) secção.  

*   Limitar o acesso para o ID da Microsoft para o menor número possível de programadores. 
*   Configure uma lista de distribuição de e-mail empresarial (ld) que inclui todas as pessoas que têm a aceder à sua conta do Dev Center. Adicione o endereço de e-mail ld às suas informações de segurança. O ld permite que todos os funcionários na lista para receber os códigos de segurança quando solicitado e para gerir as informações de segurança para a Microsoft ID. Se configurar uma lista de distribuição não for viável, em seguida, o proprietário da conta de e-mail individuais tem de estar disponível para aceder e partilhar o código de segurança quando lhe for pedido.  
    *   Por exemplo, o proprietário pedido quando novas informações de segurança são adicionadas para o ID da Microsoft ou quando o ID da Microsoft é acedido a partir de um novo dispositivo.  
*   Adicione um número de telefone da empresa que não necessita de uma extensão e está acessível para os membros da equipa de chave.  
*   Em geral, deve requerer que os programadores a utilizar dispositivos fidedignos para iniciar sessão na sua conta do Dev Center. Todos os membros da equipa de chave devem ter acesso aos dispositivos fidedignos. Utilizar dispositivos fidedignos para aceder ao reduz os requisitos para o envio de códigos de segurança quando alguém está a aceder a conta do Dev Center.  
*   Se forem necessárias para conceder acesso à conta do Dev Center a partir de um computador não fidedigno, deve limitar acesso aos programadores mais do que cinco. Idealmente, os programadores devem aceder à conta de computadores que partilham o mesmo geográfica e a localização de rede.  
*   Frequentemente, reveja e verifique as suas informações de segurança.  
    *   Para ver as informações de segurança, visite a página de definições, localizada em segurança [account.live.com/proofs/Manage](https://account.live.com/proofs/Manage).

A conta do Dev Center deve ser acedida principalmente computadores fidedignos. É extremamente importante que aceder a partir de computadores fidedignos, porque não há um limite ao número de códigos gerados por conta do Dev Center por semana. Utilizar computadores fidedignos permite também que a experiência início de sessão mais segura e consistente. 
*   Para obter mais informações sobre as diretrizes de conta do Dev Center e de segurança adicionais, visite o abrir uma página de conta de programador, localizada em [docs.microsoft.com/windows/uwp/publish/opening-a-developer-account](https://docs.microsoft.com/windows/uwp/publish/opening-a-developer-account). 

---  

#### <a name="issue-microsoft-id-in-an-azure-ad-federated-domain"></a>Problema: ID da Microsoft num domínio federado do Azure AD  
A conta empresarial pode ser federada através do Azure Active Directory (Azure AD). Se tentar criar um ID de Microsoft utilizando um endereço de e-mail da empresa que está Federado com o Azure AD, receberá um erro. Se receber um erro, em seguida, deve verificar com a sua equipa de TI para confirmar a que sua conta é federada através do Azure AD. O e-mail de AD federado do Azure é um problema conhecido e a Microsoft está a trabalhar para resolvê-lo.  
*   Para obter mais informações acerca do AD do Azure, visite a documentação do Azure Active Directory página localizada em [docs.microsoft.com/azure/active-directory](https://docs.microsoft.com/azure/active-directory).

A Microsoft recomenda uma solução. Siga estes passos para criar um novo endereço de e-mail no `outlook.com` domínio e crie uma regra para reencaminhar as suas comunicações.  
1.  Aceda à página de criação de conta e clique em Get uma nova ligação do endereço de correio eletrónico. 
    *   Para se inscrever para o seu ID da Microsoft, visite a página de criação de conta, localizada em [signup.live.com/signup](https://signup.live.com/signup).  
2.  Criar o novo endereço de correio eletrónico e uma palavra-passe. Um novo ID da Microsoft e uma caixa de correio de e-mail no `outlook.com` domínio é criado. Continue o processo de registo até a conta é criada.  

    >[!IMPORTANT]
    >Tem de utilizar uma lista de endereços ou a distribuição de correio eletrónico que é registada como um ID da Microsoft para registar no Dev Center. A Microsoft recomenda que utilize uma lista de distribuição para remover a dependência indivíduos. Se não estiver registada a sua lista de endereços ou a distribuição de correio eletrónico, em seguida, tem de registar agora.    

    >[!Important]
    >Se a qualquer endereço de e-mail está localizado no `Microsoft` da empresa domínio, em seguida, o que não é possível utilizá-la para o registo no Dev Center.  

3.  Depois de criar o ID da Microsoft com o endereço de e-mail do Outlook, iniciar sessão na sua caixa de correio do Outlook. Crie uma regra de reencaminhamento de mensagem de e-mail. O e-mail de reencaminhamento regra deve mover todos os e-mails que são recebidos na caixa de correio da Outlook para o endereço de e-mail no seu domínio que criou para gerir a sua conta do marketplace.  
    *   Para iniciar sessão na sua caixa de correio do Outlook, visite a página de Outlook localizada em [outlook.live.com/owa](https://outlook.live.com/owa).  
    *   Para obter mais informações sobre o reencaminhamento de regras, visite as regras de utilização no Outlook Web App para automaticamente reencaminhar mensagens para outra página de conta, localizado em [support.office.com/article/ Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed](https://support.office.com/article/Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed).  

1.  A regra de reencaminhamento envia e-mail e a comunicações recebidas na conta de e-mail do Outlook para o endereço de e-mail num domínio registado para a sua empresa. O `outlook.com` endereço de e-mail tem de ser utilizado para autenticar no Dev Center e o Portal de parceiros de nuvem.  

## <a name="next-steps"></a>Passos Seguintes
*   Visite o [Azure Marketplace e AppSource publicador guia](./marketplace-publishers-guide.md) página.  
 
---  
