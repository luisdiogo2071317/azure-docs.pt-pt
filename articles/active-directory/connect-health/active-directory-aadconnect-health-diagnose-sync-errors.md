---
title: O Azure AD Connect Health - diagnosticar duplicado erros de sincronização de atributo | Microsoft Docs
description: Este documento descreve o processo de diagnóstico de erros de sincronização de atributo duplicado e uma correção de potencial cenários de objeto órfão diretamente a partir do portal do Azure.
services: active-directory
documentationcenter: ''
author: zhiweiw
manager: maheshu
editor: billmath
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: zhiweiw
ms.openlocfilehash: 4a6e0924492c26c9bad4ed0af207ad9764c3cc5c
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34831902"
---
# <a name="diagnose-and-remediate-duplicated-attribute-sync-errors"></a>Diagnosticar e resolver erros de sincronização de atributo duplicado

## <a name="overview"></a>Descrição geral
Colocar um passo farther para realçar erros de sincronização, o Azure Active Directory (Azure AD) Connect Health apresenta remediação self-service. Resolve erros de sincronização de atributo duplicado e corrige os objetos que ficarão órfãos do Azure AD.
A funcionalidade de diagnóstico tem destas vantagens:
- Fornece um procedimento de diagnóstico que restringe-se para baixo de erros de sincronização de atributo duplicado. E fornece correções específicas.
- -Aplica-se uma correção para cenários dedicados do Azure AD para resolver o erro num único passo.
- Nenhuma atualização ou a configuração é necessária para ativar esta funcionalidade.
Para obter mais informações acerca do Azure AD, consulte [resiliência de atributo de sincronização e duplicado identidade](https://aka.ms/dupattributeresdocs).

## <a name="problems"></a>Problemas
### <a name="a-common-scenario"></a>Um cenário comum
Quando **QuarantinedAttributeValueMustBeUnique** e **AttributeValueMustBeUnique** ocorrem erros de sincronização, é comum para ver um **UserPrincipalName** ou **Endereços proxy** conflito no Azure AD. Pode resolver os erros de sincronização, atualizando o objeto de origem em conflito do lado no local. O erro de sincronização será resolvido após a sincronização seguinte. Por exemplo, esta imagem indica que os dois utilizadores têm um conflito do respetivo **UserPrincipalName**. Ambos são **Joe.J@contoso.com**. Os objetos em conflito são colocados em quarentena no Azure AD.

![Diagnosticar cenário comum de erro de sincronização](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixCommonCase.png)

### <a name="orphaned-object-scenario"></a>Cenário de objeto órfão
Ocasionalmente, poderá achar que um utilizador existente perde o **âncora de origem**. Foi efetuada a eliminação de objeto de origem no Active Directory no local. Mas a alteração de sinal de eliminação nunca foi sincronizada com o Azure AD. Esta perda acontece por motivos como problemas de motor de sincronização ou migração de domínio. Quando o mesmo objeto obtém restaurado ou recriado logicamente, um utilizador existente deve ser o utilizador para sincronizar a partir do **âncora de origem**. 

Quando um utilizador existente é um objeto apenas na nuvem, também pode ver o utilizador em conflito sincronizado para o Azure AD. O utilizador não pode ser correspondido sincronizadas para o objeto existente. Não há nenhuma forma direta para remapear o **âncora de origem**. Ver mais informações sobre o [base de dados de conhecimento existente](https://support.microsoft.com/help/2647098). 

Por exemplo, o objeto existente no Azure AD preserva a licença do João. Um objeto sincronizado recentemente com outra **âncora de origem** ocorre num Estado de atributo duplicado no Azure AD. As alterações para Joe no Active Directory no local não serão aplicadas aos utilizadores de original de Joe (objeto existente) no Azure AD.  

![Diagnosticar o cenário de objeto órfão de erro de sincronização](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)

## <a name="diagnostic-and-troubleshooting-steps-in-connect-health"></a>Passos do Connect Health de resolução de problemas e diagnóstico 
A funcionalidade de Diagnostique suporta objetos de utilizador com os seguintes atributos duplicados:

| Nome do atributo | Tipos de erro de sincronização|
| ------------------ | -----------------|
| UserPrincipalName | QuarantinedAttributeValueMustBeUnique ou AttributeValueMustBeUnique | 
| ProxyAddresses | QuarantinedAttributeValueMustBeUnique ou AttributeValueMustBeUnique | 
| sipProxyAddress | AttributeValueMustBeUnique | 
| onPremiseSecurityIdentifier |  AttributeValueMustBeUnique |

>[!IMPORTANT]
> Para aceder a esta funcionalidade, **Administrador Global** permissão, ou **contribuinte** permissão a partir das definições de RBAC, é necessário.
>

Siga os passos do portal do Azure para restringir os detalhes do erro de sincronização e fornecer soluções mais específicas:

![Passos de diagnóstico de erro de sincronização](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixSteps.png)

No portal do Azure, efetuar alguns passos para identificar cenários fixable específicos:  
1.  Verifique o **diagnosticar estado** coluna. O estado é apresentado se houver uma forma possíveis para corrigir um erro de sincronização diretamente a partir do Azure Active Directory. Por outras palavras, um fluxo de resolução de problemas existe que pode restringir o caso de erro e potencialmente corrigi-la.
| Estado | O que significa? |
| ------------------ | -----------------|
| Não iniciado | Ainda não visitada este processo de diagnóstico. Consoante o resultado de diagnóstico, há uma forma possível para corrigir o erro de sincronização diretamente a partir do portal. |
| Correção Manual Obrigatória | O erro não ajustar os critérios de correções disponíveis no portal. Conflitos de qualquer um dos tipos de objeto não são utilizadores, ou que já frequentou os passos de diagnóstico e resolução nenhuma garantia de reparação foi disponível no portal. No caso desta última opção, uma correção do lado no local ainda é uma das soluções. [Saiba mais sobre no local correções](https://support.microsoft.com/help/2647098). | 
| Sincronização Pendente | Foi aplicada uma correção. O portal está a aguardar o próximo ciclo de sincronização para limpar o erro. |
  >[!IMPORTANT]
  > A coluna de estado do diagnóstico irá repor após cada ciclo de sincronização. 
  >

2.  Selecione o **Diagnostique** botão nos detalhes do erro. Irá responder a perguntas alguns e identificar os detalhes do erro de sincronização. Respostas às questões que ajudam a identificar um caso de objeto órfão.

3.  Se um **fechar** botão aparece no final de diagnóstico, há garantia de reparação não rápido disponíveis no portal com base nas suas respostas. Consulte a solução mostrada no último passo. Correções no local ainda são as soluções. Selecione o **fechar** botão. O estado de erro de sincronização atual muda para **Manual correção necessária**. O estado permanece durante o ciclo de sincronização atual.

4.  Depois de é identificado um caso de objeto órfão, pode corrigir os atributos duplicados erros de sincronização diretamente a partir do portal. Para acionar o processo, selecione o **aplicar corrigir** botão. O estado das atualizações de erro de sincronização atual para **pendentes sincronização**.

5.  Após o próximo ciclo de sincronização, o erro deve ser removido da lista.

## <a name="how-to-answer-the-diagnosis-questions"></a>Como responder às perguntas de diagnóstico 
### <a name="does-the-user-exist-in-your-on-premises-active-directory"></a>O utilizador existe no Active Directory no local?

Esta questão tenta identificar o objeto de origem do utilizador do Active Directory no local existente.  
1.  Verifique se o Azure Active Directory tem um objeto com o fornecido **UserPrincipalName**. Se não estiver, responder **não**.
2.  Se existir, verifique se o objeto ainda está em âmbito para sincronização.  
  - Procure no espaço de conector do Azure AD utilizando o DN.
  - Se o objeto se encontra no **adicionar pendente** de estado, responder **não**. O Azure AD Connect não é possível ligar o objeto para o objeto à direita do Azure AD.
  - Se não é possível localizar o objeto, responder **Sim**.

Nestes exemplos, tenta identificar a pergunta se **Joe Jackson** continuará a existir no Active Directory no local.
Para o **cenário comum**, ambos os utilizadores **Joe Johnson** e **Joe Jackson** estão presentes no Active Directory no local. Os objetos em quarentena são dois utilizadores diferentes.

![Diagnosticar cenário comum de erro de sincronização](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixCommonCase.png)

Para o **cenário objeto órfão**, o único utilizador **Joe Johnson** está presente no Active Directory no local:

![Diagnosticar objeto órfão de erro de sincronização * existe utilizador * cenário](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)

### <a name="do-both-of-these-accounts-belong-to-the-same-user"></a>Ambas estas contas pertencer ao mesmo utilizador?
Esta questão verifica um entrada em conflito de utilizador e o objeto de utilizador existente no Azure AD para ver se a que pertencem ao mesmo utilizador.  
1.  O objeto em conflito recentemente está sincronizado com o Azure Active Directory. Compare os atributos de objectos:  
  - Nome a Apresentar
  - Nome do Principal de Utilizador
  - ID de objeto
2.  Se não for possível compare-as do Azure AD, verifique se o Active Directory tem objetos com o fornecido **UserPrincipalNames**. Resposta **não** se encontrar ambas.

No exemplo seguinte, os dois objetos de pertencer ao mesmo utilizador **Joe Johnson**.

![Diagnosticar objeto órfão de erro de sincronização * mesmo utilizador * cenário](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)


## <a name="what-happens-after-the-fix-is-applied-in-the-orphaned-object-scenario"></a>O que acontece depois da correção for aplicada no cenário de objeto órfão
Com base nas respostas às questões anteriores, verá o **aplicar corrigir** botão quando há uma correção do Azure AD. Neste caso, o objeto no local está a sincronizar com um Azure inesperado objecto do AD. Os dois objetos estão mapeados utilizando o **âncora de origem**. O **aplicar corrigir** alteração produz estes passos semelhantes ou:
1. Atualizações a **âncora de origem** para o objeto correto no Azure AD.
2. Elimina o objeto em conflito no Azure AD, se estiver presente.

![Diagnosticar o erro de sincronização após a correção](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixAfterFix.png)

>[!IMPORTANT]
> O **aplicar corrigir** alteração só se aplica a casos de objeto órfão.
>

Depois dos passos anteriores, o utilizador pode aceder ao recurso original, o que é uma hiperligação para um objeto existente. O **diagnosticar estado** valor na vista de lista de atualizações para **sincronização pendente**. O erro de sincronização será resolvido após a sincronização seguinte. Estabelecer a ligação será de estado de funcionamento não mostrar mais o erro de sincronização resolvido na vista de lista.


## <a name="faq"></a>FAQ
**P.** O que acontece se a execução do **aplicar corrigir** falhar?  
**R.** Se falhar a execução, é possível que o Azure AD Connect está em execução um erro de exportação. Atualize a página do portal e tente novamente após a sincronização seguinte. O ciclo de sincronização predefinido é 30 minutos. 


**P.** E se o **objeto existente** deve ser o objeto a eliminar?  
**R.** Se o **objeto existente** deve ser eliminado, o processo não pode envolver uma alteração de **âncora de origem**. Normalmente, pode corrigi-lo do Active Directory no local. 


**P.** Que permissão a um utilizador precisa de aplicar a correção?  
**R.** **Administrador global**, ou **contribuinte** das definições RBAC, tem permissão para aceder ao processo de resolução de problemas e de diagnóstico.


**P.** É necessário que configurar o Azure AD Connect ou atualizar o agente do Azure AD Connect Health para esta funcionalidade?  
**R.** Não, o processo de diagnóstico é uma funcionalidade completa de baseado na nuvem.


**P.** Se o objeto existente for recuperável eliminado, o processo de diagnóstico fará com que o objeto de Active Directory novamente?  
**R.** Não, a correção não atualizar os atributos de objeto que **âncora de origem**.
