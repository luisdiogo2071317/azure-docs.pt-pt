---
title: Pré-visualização de proteção de palavra-passe do Azure AD
description: Banir fracas as palavras-passe no local do Active Directory utilizando a pré-visualização de proteção de palavra-passe do Azure AD
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 0819a947229e633331253923654de56638a6c76a
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296114"
---
# <a name="preview-enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Pré-visualização: Impor proteção de palavra-passe do Azure AD para o Windows Server Active Directory

|     |
| --- |
| Proteção de palavra-passe do Azure AD e a lista de palavras-passe banned personalizadas são funcionalidades de pré-visualização pública do Azure Active Directory. Para mais informações sobre pré-visualizações, consulte [suplementares os termos de utilização para a pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Proteção de palavra-passe do Azure AD é uma nova funcionalidade em pré-visualização pública com tecnologia do Azure Active Directory (Azure AD) para melhorar as políticas de palavra-passe de uma organização. A implementação no local de proteção de palavra-passe do Azure AD utiliza ambos os o global e personalizada banned listas de palavra-passe armazenadas no Azure AD e efetua a mesma verifica no local como alterações de baseado na nuvem do Azure AD.

Existem três componentes de software que compõem a proteção de palavra-passe do Azure AD:

* O serviço de proxy de proteção de palavra-passe do Azure AD é executado em qualquer computador associado a um domínio na floresta do Active Directory atual. Este encaminha os pedidos de controladores de domínio para o Azure AD e devolve a resposta do Azure AD para o controlador de domínio.
* O serviço de agente de proteção DC de palavra-passe do Azure AD recebe pedidos de validação da palavra-passe a partir da dll de filtro de palavra-passe de agente do DC, processa-los utilizando a política de palavra-passe disponíveis localmente atual e devolve o resultado (pass\fail). Este serviço é responsável por periodicamente (uma vez por hora) ao chamar o serviço de proxy de proteção do Azure AD palavra-passe para obter novas versões da política de palavra-passe. Comunicação para chamadas de e para o serviço de proxy de proteção do Azure AD palavra-passe é processada através de RPC (chamada de procedimento remoto) através de TCP. Após a obtenção, as novas políticas são armazenadas numa pasta sysvol onde pode replicar para outros controladores de domínio. O serviço do agente DC também monitoriza a pasta sysvol para as alterações no caso de outros controladores de domínio de escrever novas políticas de palavra-passe não existe, se uma política de deverá recente já está disponível a verificação do serviço de proxy de proteção de palavra-passe do Azure AD será ignorada.
* A dll de filtro de palavras-passe do agente DC recebe pedidos de validação de palavra-passe do sistema operativo e reencaminha-os para o serviço agente do Azure AD palavra-passe proteção DC executados localmente no controlador de domínio.

![A forma como os componentes de proteção de palavra-passe do Azure AD funcionam em conjunto](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

## <a name="requirements"></a>Requisitos

* Todas as máquinas em que os componentes de proteção de palavra-passe do Azure AD estão instalados, incluindo controladores de domínio têm de ser o Windows Server 2012 ou posterior.
* Conectividade de rede tem de existir entre, pelo menos, um controlador de domínio em cada domínio e pelo menos um servidor que aloja o serviço de proxy de proteção de palavra-passe do Azure AD.
* Qualquer domínio do Active Directory com o DC software do serviço de agente tem de utilizar DFSR para replicação de sysvol.
* Uma conta de administrador global para registar o serviço de proxy de proteção de palavra-passe do Azure AD com o Azure AD.
* Uma conta com privilégios de administrador de domínio do Active Directory no domínio de raiz de floresta.

### <a name="license-requirements"></a>Requisitos de licença

As vantagens da lista de palavra-passe de banned global aplicam-se a todos os utilizadores do Azure Active Directory (Azure AD).

A lista de palavras-passe banned personalizadas requer licenças básico do Azure AD.

Proteção de palavra-passe do Azure AD para o Windows Server Active Directory requer licenças do Azure AD Premium. 

Informações de licenciamento adicionais, incluindo os custos, podem ser encontradas no [Azure Active Directory preços site](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="download"></a>Transferência

Existem dois programas de instalação necessários para a proteção de palavra-passe do Azure AD que pode ser transferido a partir de [Centro de transferências da Microsoft](https://www.microsoft.com/download/details.aspx?id=57071)

## <a name="answers-to-common-questions"></a>Respostas a perguntas comuns

* Sem conectividade internet necessária os controladores de domínio. As máquinas que executa o serviço de proxy de proteção de palavra-passe do Azure AD são as máquinas apenas necessidade de conectividade à internet.
* Não existem portas de rede estão abertas em controladores de domínio.
* Não existem alterações de esquema do Active Directory são necessárias.
   * O software utiliza o contentor do Active Directory existente e serviceConnectionPoint objetos de esquema.
* Não há nenhum domínio do Active Directory ou floresta funcional nível (DFL\FFL) mínimos.
* O software não crie ou necessitam de quaisquer contas em domínios do Active Directory que protege.
* Implementação incremental é suportada com o compromisso de que a política de palavra-passe é imposta apenas onde está instalado o agente de controlador de domínio.
* Proteção de palavra-passe do Azure AD não é um motor de aplicação de política em tempo real. Pode haver um atraso de tempo entre a uma alteração de configuração de política de palavra-passe e a hora é atinge e é aplicada a todos os controladores de domínio.

## <a name="next-steps"></a>Passos Seguintes

[Implementar proteção de palavras-passe do Azure AD](howto-password-ban-bad-on-premises.md)