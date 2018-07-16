---
title: Descrição geral de redefinição de senha de autoatendimento de AD do Azure | Documentos da Microsoft
description: O que significam de reposição de palavra-passe self-service podem do Azure AD para a sua organização?
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: e28918fe9e26221738fe234ad41923c58a6ac260
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049026"
---
# <a name="azure-ad-self-service-password-reset-for-the-it-professional"></a>O Azure AD self-service reposição palavra-passe para profissionais de TI

Com a reposição de palavra-passe self-service do Azure Active Directory (Azure AD) (SSPR), os utilizadores podem repor as palavras-passe no seu próprio quando e onde precisam. Ao mesmo tempo, os administradores podem controlar como obtém a reposição de palavra-passe de um utilizador. Os utilizadores já não é necessário chamar um suporte técnico para repor a palavra-passe. Azure AD SSPR inclui:

* **Alteração de palavra-passe self-service**: O utilizador sabe a palavra-passe, mas quiser alterá-lo para algo novo.
* **Reposição de palavra-passe self-service**: O utilizador não conseguir iniciar sessão e pretende repor a palavra-passe utilizando um ou mais dos seguintes métodos de autenticação validados:
   * Envie uma mensagem de texto para um telefone celular validado.
   * Fazer uma chamada telefónica para um telefone móvel ou office validado.
   * Envie um e-mail para uma conta de e-mail secundário validados.
   * Responda às perguntas de segurança.
* **Desbloqueio de conta de self-service**: O utilizador não conseguir iniciar sessão com a palavra-passe e foi bloqueado. O usuário deseja desbloquear a conta sem intervenção do administrador através de métodos de autenticação deles.

> [!VIDEO https://www.youtube.com/embed/hc97Yx5PJiM]

## <a name="why-choose-azure-ad-sspr"></a>Por quê escolher o Azure AD SSPR

Azure AD SSPR ajuda-o a:

* **Reduzir os custos** como help desk assistido redefinições de senha, normalmente, conta para 20% das chamadas de suporte de uma organização de TI. 
* **Melhorar as experiências de utilizador final** e **reduzir o volume de suporte técnico** ao dar aos utilizadores finais a capacidade de resolver seus próprios problemas de palavra-passe. Não é necessário para chamar um suporte técnico ou abra um pedido de suporte.
* **Unidade mobilidade** como os utilizadores podem repor as palavras-passe onde quer que estejam.
* **Manter o controlo** da política de segurança. Os administradores podem continuar a impor as políticas que eles já possuem.

Se estiver pronto, pode começar a utilizar com o Azure AD SSPR com o nosso [documentação de orientação de início rápido](quickstart-sspr.md). Rapidamente pode dar aos seus utilizadores a capacidade de repor a palavra-passe.

## <a name="azure-ad-sspr-availability"></a>Disponibilidade do Azure AD SSPR

Azure AD SSPR está disponível em três escalões consoante a sua subscrição:

* **Azure AD gratuito**: os administradores apenas na Cloud podem repor a palavra-passe.
* **Azure AD básico** ou qualquer **subscrição do Office 365 paga**: os utilizadores apenas na Cloud podem repor a palavra-passe.
* **O Azure AD Premium**: qualquer utilizador ou administrador, incluindo autenticação apenas na cloud, federada, pass-through ou os utilizadores sincronizado de hash de palavra-passe, pode repor a palavra-passe. Palavras-passe no local requerem a repetição de escrita de palavra-passe esteja ativada.

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>O Azure AD preços, SLA, atualizações e Roteiro

Podem encontrar mais detalhes sobre licenciamento, preços e planos futuros nos seguintes sites:

* [O Azure AD, os detalhes dos preços](https://azure.microsoft.com/pricing/details/active-directory/)
* [Preços do Office 365](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [Contratos de nível de serviço do Azure](https://azure.microsoft.com/support/legal/sla/)
* [Contrato de nível de serviço do Microsoft Online Services](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [Atualizações do Azure](https://azure.microsoft.com/updates/)
* [Mapa do Azure](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Passos Seguintes

* Está pronto para começar a utilizar o SSPR? [Configurar a palavra-passe self-service do Azure AD repor](quickstart-sspr.md).
* Planear uma implementação com êxito da SSPR aos seus utilizadores usando a orientação encontrada no nosso [guia de implementação](howto-sspr-deployment.md).
* [Reponha ou altere a palavra-passe](../user-help/active-directory-passwords-update-your-own-password.md).
* [Registe-se na reposição personalizada de palavras-passe](../user-help/active-directory-passwords-reset-register.md).
