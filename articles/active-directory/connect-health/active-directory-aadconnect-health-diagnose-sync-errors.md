---
title: O Azure AD Connect Health - diagnosticar duplicado erros de sincronização de atributo | Microsoft Docs
description: Este documento descreve o processo de Diagnostique de erros de sincronização de atributo duplicado e corrija possível dos cenários de objeto órfão diretamente a partir do portal do Azure.
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
ms.openlocfilehash: 0a345fd3443fb33d6f5912c8a04677091e20ecc8
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="duplicate-attribute-sync-errors-diagnosis-and-remediation"></a>Diagnóstico de erros de sincronização de atributos e remediação de duplicação 

## <a name="overview"></a>Descrição geral
Colocar um passo adicional de realce erros de sincronização, o Azure Active Directory Connect Health está a introduzir uma experiência de remediação de self-service para resolver erros de sincronização de atributo duplicado e corrigir objetos isolados do Azure AD. A principal vantagem da funcionalidade de diagnóstico:
- Forneça o procedimento de diagnóstico para restringir a cenários de erro de sincronização de atributo duplicado e indicar resoluções específicas
- Aplicar a correção para cenários dedicados do Azure AD para resolver o erro de clique único
- Nenhuma atualização ou a configuração é necessária para ativar esta funcionalidade.
Leia mais detalhes sobre o Azure AD [duplicado resiliência](https://aka.ms/dupattributeresdocs).

## <a name="problems"></a>Problemas
### <a name="common-scenario"></a>Cenário comum
Quando **QuarantinedAttributeValueMustBeUnique** e **AttributeValueMustBeUnique** ocorrem erros de sincronização, é comum para ver os conflitos de nome Principal de utilizador ou endereços de Proxy no Azure AD. Pode resolver os erros de sincronização, atualizando o objeto de origem em conflito no lado do local. O erro de sincronização será resolvido após a sincronização seguinte. Por exemplo, a imagem abaixo indica que dois utilizadores estão a ter conflito do respetivo UserPrincipalName como *Joe.J@contoso.com*. Os objetos em conflito são colocados em quarentena no Azure AD. 

![Diagnosticar cenário comum de erro de sincronização](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixCommonCase.png)

### <a name="orphaned-object-scenario"></a>Cenário de objeto órfão
Ocasionalmente, poderá considerar que um utilizador existente perde a âncora de origem. A eliminação de objeto de origem foi efetuada no local do AD, mas a alteração de sinal de eliminação nunca foi sincronizada com o Azure AD. Pode acontecer devido a razões, tais como a migração de domínio ou de problema do motor de sincronização. Ao mesmo objeto foi restaurado ou recriado, o utilizador logicamente existente deve ser o utilizador para sincronizar a partir de âncora de origem. Para o utilizador existente como único objeto de nuvem, também pode ver o utilizador em conflito sincronizada com o Azure AD e não pode ser correspondido sincronizadas para o objeto existente. Não há nenhuma forma direta para remapear a âncora de origem. Leia mais sobre o [KB existente](https://support.microsoft.com/help/2647098). Por exemplo, o objeto existente no Azure AD preserva a licença do João. Ocorreu um objeto sincronizado recentemente com uma âncora de origem diferente no estado de atributo duplicado no Azure AD. As alterações de Joe no local no AD não poderá ser aplicada ao utilizador original de Joe (objeto existente) no Azure AD.  

![Diagnosticar o cenário de objeto órfão de erro de sincronização](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)

## <a name="diagnostic-and-troubleshooting-steps-in-connect-health"></a>Passos do Connect Health de resolução de problemas e diagnóstico 
Diagnosticar funcionalidade suporta objetos de utilizador com os seguintes atributos duplicados:

| Nome do Atributo | Tipos de erro de sincronização|
| ------------------ | -----------------|
| UserPrincipalName | QuarantinedAttributeValueMustBeUnique ou AttributeValueMustBeUnique | 
| ProxyAddresses | QuarantinedAttributeValueMustBeUnique ou AttributeValueMustBeUnique | 
| sipProxyAddress | AttributeValueMustBeUnique | 
| onPremiseSecurityIdentifier |  AttributeValueMustBeUnique |

>[!IMPORTANT]
> Requer **Administrador Global** permissão ou **contribuinte** das definições do RBAC para poder aceder a esta funcionalidade. 
>

Os seguintes passos no portal do Azure, poderá restringir os detalhes do erro de sincronização e fornecer soluções mais específicas:

![Diagnosticar sincronização erro diagnosticar passos](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixSteps.png)

No portal do Azure, poderá efetuar alguns passos para identificar cenários fixable específicos:  
1.  Na coluna Estado diagnostique o estado irá mostrar se existe um potencial fluxos de resolução de problemas para restringir o caso de erro e corrija potencialmente diretamente a partir do Azure Active Directory.
| Estado | O que significa? |
| ------------------ | -----------------|
| Não iniciado | Não visitaram este processo de diagnóstico. Depende no resultado diagnóstico, não há potencialmente uma forma para corrigir o erro de sincronização do portal diretamente. |
| Correção Manual Obrigatória | O erro não se ajusta os critérios de correção disponível no portal. As maiúsculas e minúsculas podem ser (1) em conflito objeto tipos não são utilizadores (2) que já ocorreu através dos passos de diagnóstico e não corrigir disponível no portal de resolução. Neste caso, a correção do lado do local continuarão a estar uma das soluções. [Saiba mais sobre no local com garantia de reparação](https://support.microsoft.com/help/2647098) | 
| Sincronização Pendente | Correção foi aplicada. A aguardar o próximo ciclo de sincronização para limpar o erro. |
>[!IMPORTANT]
> A coluna de estado do diagnóstico será reposta após cada ciclo de sincronização. 
>

2.  Ao clicar em **Diagnostique** botão no painel de detalhes do erro, tem de responder às perguntas alguns e identificar os detalhes do erro de sincronização. As respostas a perguntas ajudará a identificar as maiúsculas e minúsculas do cenário de objeto órfão. 

3.  Se existir um **fechar** botão no final de diagnóstico, significa que não há nenhuma garantia de reparação rápido disponíveis no portal baseado nas respostas indicadas. Consulte a solução mostrada no último passo. Correção no local continuarão a estar as soluções. Depois de clicar no botão fechar, irá encontrar o estado de erro de sincronização atual passará a ser **Manual correção necessária**. O estado irão manter durante o ciclo de sincronização atual.

4.  Uma vez identificado o caso de objeto órfão, poderá corrigir os atributos duplicados erros de sincronização diretamente a partir do portal. Clique em de **aplicar corrigir** botão para acionar o processo. O estado de erro de sincronização atual será atualizada para ser **pendentes sincronização**.

5.  Após o seguinte ciclo de sincronização, o erro deve ser removido da lista.

## <a name="how-to-answer-the-diagnosis-questions"></a>Como responder às perguntas de diagnóstico 
### <a name="does-the-user-exist-in-your-on-premises-active-directory"></a>O utilizador existe no Active Directory no local?

A pergunta está a tentar identificar o objeto de origem de um utilizador existente do Active Directory no local.  
1.  Verifique se o Active Directory tem um objeto com o UserPrincipalName fornecido. Se não, responder não.
2.  Em caso Afirmativo, verifique se o objeto ainda está no âmbito para Sincronização.  
  - Procure no Azure AD Connect Space com o DN.
  - Se é possível localizar o objeto com o **adicionar pendente** de estado, responder não. Azure AD Connect não é possível ligar o objeto para o objeto à direita do AD.
  - Se o objeto não foi encontrado, responder Sim.

> Obter o diagrama a seguir, por exemplo, a pergunta está a tentar identificar se *Joe Jackson* ainda existem no Active Directory no local.
Para **cenário comum**, ambos os utilizador *Joe Johnson* e *Joe Jackson* estará presente no Active Directory no local. Os objetos em quarentena são dois utilizadores diferentes.

![Diagnosticar cenário comum de erro de sincronização](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixCommonCase.png)

> Para **cenário objeto órfão**, apenas único utilizador – *Joe Johnson* estará presente do Active Directory no local:

![Diagnosticar o cenário de objeto órfão de erro de sincronização](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)

### <a name="do-both-these-accounts-belong-to-the-same-user"></a>Ambas as contas pertencem ao mesmo utilizador?
A pergunta está a verificar o utilizador em conflito de entrada e o objeto de utilizador existente no Azure AD para ver se a que pertencem ao mesmo utilizador.  
1.  Objeto em conflito recentemente está sincronizado com o Azure Active Directory. Comparar o objeto a partir do respetivo:  
  - Nome a apresentar
  - Nome do Principal de Utilizador
  - ID de objeto
2.  Se não tiver compará-los, certifique-se do que Active Directory tem objetos com o UserPrincipalNames fornecido. Se ambos forem encontradas, responder não.  

> No caso de abaixo, os dois objetos pertence ao mesmo utilizador *Joe Johnson*.

![Diagnosticar o cenário de objeto órfão de erro de sincronização](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixOrphanedCase.png)


## <a name="what-happened-after-fix-is-applied-for-orphaned-object-scenario"></a>O que aconteceu após a correção for aplicada para o cenário de objeto órfão
Com base nas respostas de perguntas geradas, será capaz de ver **aplicar corrigir** botão quando há uma correção do Azure AD. Neste caso, o objeto local no está a sincronizar com um Azure inesperado objecto do AD. Os dois objetos estão mapeados com o "âncora de origem". A alteração de aplicar irá executar os passos, tais como:
- Atualize a âncora de origem para o objeto correto no Azure AD.
- Elimine o objeto em conflito no Azure AD, se o apresente.

![Diagnosticar o erro de sincronização após a correção](./media/active-directory-aadconnect-health-sync-iidfix/IIdFixAfterFix.png)

>[!IMPORTANT]
> A alteração aplicar corrigir só se aplicará aos cenários de objeto órfão.
>

Depois dos passos acima, o utilizador será capaz de aceder ao recurso original, o que é a ligação para o objeto existente. O **diagnosticar estado** valor na vista de lista será atualizada para ser **sincronização pendente**. Erro de sincronização será resolvido após a sincronização seguinte. Ligar estado de funcionamento não apresentará erro de sincronização resolvido da vista de lista já. 


## <a name="faq"></a>FAQ
 -  O que aconteceu caso de falha de execução das situações?  
Se falhar a execução, é possível do Azure AD Connect está a executar o erro de exportação no momento. Atualize a página do portal e tente novamente após a sincronização seguinte. Ciclo de sincronização a predefinição é 30 minutos. 

 -  E se o **objeto existente** deve ser o objeto a eliminar?  
Se o objeto existente deve ser eliminado neste caso, o processo não envolve a alteração da âncora de origem. Deve ser capaz de corrigir do seu local AD.  

 -  O que é a permissão do utilizador poder aplicar a correção?  
Administrador global ou de contribuinte a partir das definições do RBAC terá permissão para aceder ao processo de resolução de problemas e de diagnóstico.

 -  Tem configuração AAD Connect ou atualizar o agente do Azure AD Connect Health para esta funcionalidade?  
Não, o processo de diagnóstico é uma funcionalidade completa de baseado na nuvem.

 -  Se o objeto existente for recuperável eliminado, o processo de Diagnostique restauram o objeto ser novamente Active Directory?  
Não, a correção não irá atualizar o atributo de objeto que não seja a âncora de origem. 
