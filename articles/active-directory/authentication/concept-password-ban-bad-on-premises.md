---
title: Pré-visualização de proteção de palavra-passe do Azure AD
description: Banir fracas as palavras-passe do Active Directory no local com a pré-visualização de proteção de palavra-passe do Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: b99c1b99fe87c755d6092876ccd598d926289192
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/26/2019
ms.locfileid: "55077835"
---
# <a name="preview-enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Pré-visualização: Impor proteção de palavra-passe do Azure AD para o Windows Server Active Directory

|     |
| --- |
| Proteção de palavra-passe do Azure AD e a lista de palavras-passe banidas personalizadas são funcionalidades de pré-visualização pública do Azure Active Directory. Para obter mais informações sobre pré-visualizações, consulte [termos de utilização suplementares para pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Proteção de palavra-passe do Azure AD é um novo recurso em pré-visualização pública com tecnologia do Azure Active Directory (Azure AD) para melhorar as políticas de palavra-passe de uma organização. A implementação no local de proteção de palavra-passe do Azure AD utiliza os dois global e custom banida listas de palavra-passe armazenadas no Azure AD e executa a mesma verificações no local à medida que muda com base na cloud do Azure AD.

Existem três componentes de software que fazem com a proteção de palavra-passe do Azure AD:

* O serviço de proxy de proteção de palavra-passe do Azure AD é executado em qualquer computador associado a um domínio na floresta do Active Directory atual. Ele reencaminha os pedidos de controladores de domínio para o Azure AD e retorna a resposta do Azure AD para o controlador de domínio.
* O serviço de agente de proteção DC de palavra-passe do Azure AD recebe pedidos de validação da palavra-passe a partir da dll de filtro de palavra-passe do agente DC, processa-las com a política de palavra-passe disponível localmente atual e devolve o resultado (pass\fail). Este serviço é responsável por periodicamente (uma vez por hora) chamando o serviço de proxy de proteção de palavra-passe do Azure AD para obter novas versões da política de palavra-passe. Comunicação para chamadas de e para o serviço de proxy de proteção de palavra-passe do Azure AD é processada através de RPC (chamada de procedimento remoto) sobre TCP. Após a recuperação, as novas políticas são armazenadas numa pasta sysvol, onde pode replicar para outros controladores de domínio. O serviço de agente do controlador de domínio também monitora a pasta sysvol para alterações no caso de outros controladores de domínio escreveram novas políticas de palavra-passe lá, se uma política convenientemente recente já está disponível a verificação do serviço de proxy de proteção de palavra-passe do Azure AD será ignorada.
* A dll de filtro de palavras-passe do agente de controlador de domínio recebe pedidos de validação da palavra-passe a partir do sistema operativo e reencaminha-os para o serviço de proteção DC palavra-passe do Azure AD agente em execução localmente no controlador de domínio.

![Como os componentes de proteção de palavra-passe do Azure AD funcionam em conjunto](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

### <a name="license-requirements"></a>Requisitos de licença

Os benefícios da lista de palavra-passe banidas global aplicam-se a todos os utilizadores do Azure Active Directory (Azure AD).

A lista de palavras-passe banidas personalizado requer licenças do Azure AD básico.

Proteção de palavra-passe do Azure AD para o Windows Server Active Directory requer licenças do Azure AD Premium.

Informações de licenciamento adicionais, incluindo os custos, podem ser encontradas no [do Azure Active Directory preços site](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="download"></a>Transferência

Existem dois programas de instalação necessários para proteção de palavra-passe do Azure AD que pode ser transferido a partir do [Centro de transferências da Microsoft](https://www.microsoft.com/download/details.aspx?id=57071)

## <a name="answers-to-common-questions"></a>Respostas a perguntas comuns

* Sem conectividade internet necessária os controladores de domínio. O computador ou computadores a executar o serviço de proxy de proteção de palavra-passe do Azure AD são apenas máquinas que requerem conectividade à internet.
* Não existem portas de rede estão abertas nos controladores de domínio.
* Sem alterações de esquema do Active Directory são necessárias.
* O software utiliza o contentor do Active Directory existente e os objetos de esquema de serviceConnectionPoint.
* Não há nenhum domínio do Active Directory ou funcional de floresta nível (DFL\FFL) requisito mínimo.
* O software não criam nem necessitam de quaisquer contas em domínios do Active Directory que protege.
* Implementação incremental é suportada com o compromisso que política de palavra-passe só é aplicada onde está instalado o agente de controlador de domínio.
* Recomenda-se para instalar o agente de controlador de domínio em todos os controladores de domínio para garantir a imposição de proteção de palavra-passe. 
* Proteção de palavra-passe do Azure AD não é um motor de aplicação de política em tempo real. Pode haver um atraso no período entre a uma alteração de configuração de política de palavra-passe e o tempo de atingir e é aplicada a todos os controladores de domínio.

## <a name="next-steps"></a>Passos Seguintes

[Implementar proteção de palavras-passe do Azure AD](howto-password-ban-bad-on-premises-deploy.md)
