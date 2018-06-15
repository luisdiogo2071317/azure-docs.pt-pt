---
title: Descrição geral de reposição do Azure AD personalizada de palavra-passe | Microsoft Docs
description: Que efetue de reposição de palavra-passe self-service podem do Azure AD para a sua organização?
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: 77d26d860462b1a82315bf1385bccf62047e3da3
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
ms.locfileid: "33866673"
---
# <a name="azure-ad-self-service-password-reset-for-the-it-professional"></a>Azure AD self-service reposição palavra-passe para profissionais de TI

Com a reposição de palavra-passe self-service do Azure Active Directory (Azure AD) (SSPR), os utilizadores podem repor as palavras-passe nos seus próprios quando e onde precisam. Ao mesmo tempo, os administradores podem controlar como obtém a reposição de palavra-passe de um utilizador. Os utilizadores já não necessitam de um técnico apenas para repor a palavra-passe. Azure AD SSPR inclui:

* **Alteração de palavra-passe self-service**: O utilizador sabe a palavra-passe, mas pretende alterá-lo para algo de novo.
* **Reposição de palavra-passe self-service**: O utilizador não é possível iniciar sessão e pretende repor a palavra-passe através da utilização de um ou mais dos seguintes métodos de autenticação validados:
   * Envie uma mensagem de texto para um telemóvel validado.
   * Efetue uma chamada telefónica um telefone do escritório ou mobile validado.
   * Envie um e-mail para uma conta de e-mail secundário validado.
   * Responda às perguntas de segurança.
* **Desbloqueio de conta personalizada**: O utilizador não é possível iniciar sessão com a palavra-passe e for bloqueado. O utilizador pretende desbloquear a conta sem a intervenção do administrador ao utilizar os métodos de autenticação.

> [!VIDEO https://www.youtube.com/embed/hc97Yx5PJiM]

## <a name="why-choose-azure-ad-sspr"></a>Por que motivo escolhê do Azure AD SSPR

Azure AD SSPR ajuda-o a:

* **Reduzir os custos** como ajuda palavra-passe de assistido do suporte técnico repõe normalmente de conta para 20% de chamadas de suporte de uma organização de TI. 
* **Melhorar as experiências de utilizador final** e **reduzir o volume de suporte técnico de ajuda** , concedendo aos utilizadores finais a capacidade para resolver problemas as suas próprias palavras-passe. Não é necessário para um técnico ou abra um pedido de suporte.
* **Unidade mobilidade** como os utilizadores podem repor as palavras-passe de onde quer que estão.
* **Manter o controlo** da política de segurança. Os administradores podem continuar para impor políticas que têm atualmente.

Se estiver pronto, pode começar com o Azure AD SSPR com o nosso [orientações de início rápido](quickstart-sspr.md). Rapidamente pode conceder aos seus utilizadores a capacidade de repor as respetivas palavras-passe.

## <a name="azure-ad-sspr-availability"></a>Disponibilidade do Azure AD SSPR

Está disponível em três camadas, dependendo da sua subscrição do Azure AD SSPR:

* **Azure AD livres**: os administradores só de nuvem podem repor as suas próprias palavras-passe.
* **Azure AD Basic** ou qualquer **subscrição do Office 365 paga**: os utilizadores de apenas na nuvem podem repor as respetivas palavras-passe.
* **O Azure AD Premium**: qualquer utilizador ou administrador, incluindo autenticação apenas na nuvem, federada, pass-through ou utilizadores de sincronização de hash de palavra-passe, pode repor as suas próprias palavras-passe. Palavras-passe no local requerem a repetição de escrita de palavras-passe para ser ativada.

## <a name="azure-ad-pricing-sla-updates-and-roadmap"></a>Do Azure AD preços, SLA, atualizações e informações gerais

Podem encontrar mais detalhes sobre o licenciamento, preço e planos futuros nos seguintes sites:

* [Azure AD detalhes de preços](https://azure.microsoft.com/pricing/details/active-directory/)
* [Preços do Office 365](https://products.office.com/compare-all-microsoft-office-products?tab=2)
* [Contratos de nível de serviço do Azure](https://azure.microsoft.com/support/legal/sla/)
* [Contrato de nível de serviço para o Microsoft Online Services](http://go.microsoft.com/fwlink/?LinkID=272026&clcid=0x409)
* [Atualizações do Azure](https://azure.microsoft.com/updates/)
* [Mapa do Azure](https://www.microsoft.com/cloud-platform/roadmap-recently-available)

## <a name="next-steps"></a>Passos Seguintes

* Está pronto para começar a utilizar SSPR? [Configurar a palavra-passe self-service do Azure AD repor](quickstart-sspr.md).
* Planear uma implementação de SSPR com êxito aos seus utilizadores utilizando as orientações encontrada no nosso [guia de implementação](howto-sspr-deployment.md).
* [Reponha ou altere a palavra-passe](../active-directory-passwords-update-your-own-password.md).
* [Registe-se na reposição personalizada de palavras-passe](../active-directory-passwords-reset-register.md).
