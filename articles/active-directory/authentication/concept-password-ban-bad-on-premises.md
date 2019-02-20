---
title: Pré-visualização de proteção de palavra-passe do Azure AD
description: Banir fracas as palavras-passe do Active Directory no local com a pré-visualização de proteção de palavra-passe do Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/18/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1beae186f6eb276b9aa302d3d51f0ba8688e591
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/19/2019
ms.locfileid: "56415753"
---
# <a name="preview-enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Pré-visualização: Impor proteção de palavra-passe do Azure AD para o Windows Server Active Directory

|     |
| --- |
| Proteção de palavra-passe do Azure AD e a lista de palavras-passe banidas personalizadas são funcionalidades de pré-visualização pública do Azure Active Directory. Para obter mais informações sobre pré-visualizações, consulte [termos de utilização suplementares para pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Proteção de palavra-passe do Azure AD é um novo recurso em pré-visualização pública com tecnologia do Azure Active Directory (Azure AD) para melhorar as políticas de palavra-passe de uma organização. A implementação no local de proteção de palavra-passe do Azure AD utiliza os dois global e custom banida listas de palavra-passe armazenadas no Azure AD e executa a mesma verificações no local à medida que muda com base na cloud do Azure AD.

## <a name="design-principles"></a>Princípios de conceção

A proteção de palavra-passe do AD do Azure para o Active Directory foi concebida com os seguintes princípios em mente:

* Controladores de domínio nunca são necessárias para comunicar diretamente com a Internet
* Não existem novas portas de rede estão abertas nos controladores de domínio.
* Sem alterações de esquema do Active Directory são necessárias. O software utiliza o contentor do Active Directory existente e os objetos de esquema de serviceConnectionPoint.
* É necessário o nível funcional de floresta ou de domínio do Active Directory não mínimo (DFL\FFL).
* O software não criam nem necessitam de quaisquer contas em domínios do Active Directory que protege.
* Palavras-passe de texto não encriptado do utilizador nunca deixam o controlador de domínio (se, durante as operações de validação de palavra-passe ou em qualquer outra altura).
* Implementação incremental é suportada com o compromisso que política de palavra-passe só é aplicada onde está instalado o agente de controlador de domínio.
* Recomenda-se para instalar o agente de controlador de domínio em todos os controladores de domínio para garantir a imposição de segurança de proteção de palavra-passe onipresente.

## <a name="architectural-diagram"></a>Diagrama da arquitetura

É importante ter um entendimento do design subjacente e conceitos funcionais antes de implementar a proteção de palavra-passe do Azure AD no ambiente do Active Directory no local. O diagrama seguinte mostra como os componentes de proteção de palavra-passe do Azure AD funcionam em conjunto:

![Como os componentes de proteção de palavra-passe do Azure AD funcionam em conjunto](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

O diagrama acima mostra os três componentes de software básica que compõem a proteção de palavra-passe do Azure AD:

* O serviço de Proxy de proteção de palavra-passe do Azure AD é executado em qualquer computador associado a um domínio na floresta do Active Directory atual. Seu objetivo principal é reencaminhar pedidos de transferência da política de palavra-passe de controladores de domínio para o Azure AD e retornar a resposta do Azure AD para o controlador de domínio.
* A dll de filtro de palavra-passe de agente do Azure AD palavra-passe proteção DC recebe pedidos de validação da palavra-passe de utilizador do sistema operacional e reencaminha-os para o serviço de agente do Azure AD palavra-passe proteção DC em execução localmente no controlador de domínio.
* O serviço de agente do Azure AD palavra-passe proteção DC recebe pedidos de validação da palavra-passe a partir da dll de filtro de palavra-passe do agente DC, processa-las com a política de palavra-passe (disponível localmente) atual e devolve o resultado (pass\fail).

## <a name="theory-of-operations"></a>Teoria de operações

Portanto, tendo em conta os princípios de design e o diagrama acima, como proteção de palavra-passe do Azure AD, na verdade, funciona?

Cada serviço de Proxy de proteção de palavra-passe do Azure AD anuncia o próprio para controladores de domínio na floresta através da criação de um objeto de serviceConnectionPoint no Active Directory.

Cada serviço de agente do Azure AD palavra-passe proteção DC cria também um objeto de serviceConnectionPoint no Active Directory. No entanto é utilizado principalmente para geração de relatórios e diagnósticos.

O serviço de agente do Azure AD palavra-passe DC de proteção é responsável por iniciar a transferência de uma nova política de palavra-passe do Azure AD. A primeira etapa é localizar um serviço de Proxy de proteção de palavra-passe do Azure AD através da consulta da floresta para objetos de serviceConnectionPoint de proxy. Assim que for encontrado um serviço de proxy disponível, um pedido de transferência de política de palavra-passe é enviado do serviço de agente do controlador de domínio para o serviço de proxy, que por sua vez envia-os para o Azure AD e, em seguida, devolve a resposta para o serviço de agente do controlador de domínio. Depois de receber uma nova política de palavra-passe do Azure AD, o serviço do agente DC armazena a política numa pasta dedicada na raiz da sua partilha de sysvol do domínio. O serviço de agente do controlador de domínio também monitora esta pasta no caso de políticas mais recentes replicar de outros serviços do agente de controlador de domínio no domínio.

O serviço de agente do Azure AD palavra-passe proteção DC sempre irá pedir uma nova política no arranque do serviço. Depois do serviço do agente DC é iniciado, ele será periodicamente (uma vez por hora) Verifique a idade da política atual disponível localmente; Se a política atual for mais antiga do que uma hora o serviço do agente DC irá pedir uma nova política do Azure AD, tal como descrito acima, caso contrário, o agente de DC irá continuar a utilizar a política atual.

O serviço de agente de controlador de domínio de proteção de palavra-passe do Azure AD se comunica com o serviço de Proxy de proteção de palavra-passe do Azure AD através de RPC (chamada de procedimento remoto) sobre TCP. O serviço de Proxy escuta para essas chamadas de qualquer porta RPC dinâmica ou estática (conforme configurado).

O agente do Azure AD palavra-passe DC de proteção nunca escuta numa porta de rede disponível e o serviço de Proxy nunca tenta chamar o serviço de agente do controlador de domínio.

O serviço de Proxy de proteção de palavra-passe do Azure AD está sem monitoração de estado; ele nunca caches de políticas ou qualquer outro Estado transferido a partir do Azure.

O serviço de agente do Azure AD palavra-passe DC proteção só avaliará a palavra-passe de um utilizador através da política de palavra-passe localmente disponível mais recente. Se nenhuma política de palavra-passe está disponível no controlador de domínio local, a palavra-passe serão aceites automaticamente e uma mensagem de registo de eventos será registada para avisar o administrador.

Proteção de palavra-passe do AD do Azure não é um motor de aplicação de política em tempo real. Pode haver um atraso no período entre a uma alteração de configuração de política de palavra-passe é feito no Azure AD e o tempo de atingir e é aplicada a todos os controladores de domínio.

## <a name="foresttenant-binding-for-azure-ad-password-protection"></a>Enlace de Forest\tenant para proteção de palavra-passe do Azure AD

Implementação de proteção de palavra-passe do Azure AD numa floresta do Active Directory requer o registo de floresta do Active Directory e quaisquer serviços implementados do Proxy de proteção de palavra-passe do Azure AD, com o Azure AD. Esses registos (floresta e proxies) estão associados uma específicas do inquilino do Azure AD, que é identificado implicitamente por meio das credenciais utilizadas durante o registo. Sempre que uma política de palavra-passe de proteção de palavra-passe do Azure AD é transferida, é sempre específico a este inquilino (ou seja, que política será sempre uma combinação do Microsoft global banida lista de palavra-passe e a lista de palavras-passe banidas personalizado por inquilino). Não é suportada para configurar proxies ou de diferentes domínios numa floresta para ser vinculado ao Azure AD diferentes inquilinos.

## <a name="license-requirements"></a>Requisitos de licença

Os benefícios da lista de palavra-passe banidas global aplicam-se a todos os utilizadores do Azure Active Directory (Azure AD).

A lista de palavras-passe banidas personalizado requer licenças do Azure AD básico.

A proteção de palavra-passe do AD do Azure para o Windows Server Active Directory requer licenças do Azure AD Premium.

Informações de licenciamento adicionais, incluindo os custos, podem ser encontradas no [do Azure Active Directory preços site](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="download"></a>Transferência

Os dois necessário programas de instalação do agente para a proteção de palavra-passe do Azure AD que pode ser transferida a partir do [Centro de transferências da Microsoft](https://www.microsoft.com/download/details.aspx?id=57071)

## <a name="next-steps"></a>Passos Seguintes

[Implementar proteção de palavras-passe do Azure AD](howto-password-ban-bad-on-premises-deploy.md)
