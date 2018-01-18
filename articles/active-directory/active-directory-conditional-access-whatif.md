---
title: "Azure Active Directory e se ferramenta de acesso condicional - pré-visualização | Microsoft Docs"
description: "Saiba como pode testar a configuração das suas políticas de acesso condicional do Azure Active Directory."
services: active-directory
keywords: "acesso condicional para aplicações, o acesso condicional com o Azure AD, o acesso seguro aos recursos da empresa, as políticas de acesso condicional"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/21/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: de6b3dcd77132154e583d7333983d6745c4aa3bd
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="azure-active-directory-conditional-access-what-if-tool---preview"></a>Azure Active Directory e se ferramenta de acesso condicional - preview

[Acesso condicional](active-directory-conditional-access-azure-portal.md) é uma funcionalidade do Azure Active Directory (Azure AD) que lhe permite controlar como autorizado os utilizadores acedam as aplicações na nuvem. Como pode saber o que pode esperar formulário as políticas de acesso condicional no seu ambiente? Para responder a esta questão, pode utilizar o **acesso condicional e se a ferramenta**.

Este artigo explica como pode utilizar esta ferramenta para testar as suas políticas de acesso condicional.

## <a name="what-it-is"></a>O que é

O **condicional aceder a que se ferramenta política** permite-lhe compreender o impacto das políticas de acesso condicional no seu ambiente. Em vez de teste impulsionar as suas políticas, executando vários inícios de sessão manualmente, esta ferramenta permite-lhe avaliar um simulada início de sessão de um utilizador. A simulação calcula o impacto neste início de sessão tem as suas políticas e gera um relatório de simulação. O relatório não listar apenas política de acesso aplicada políticas de acesso, mas também [políticas clássicas](active-directory-conditional-access-migration.md#classic-policies) caso existam.    

O que se ferramentas também fornece uma forma rápida determine as políticas que se aplicam a um utilizador específico. Pode utilizar as informações do utilizador, por exemplo, se for necessário resolver um problema.  

## <a name="how-it-works"></a>Como funciona

No **acesso condicional e se a ferramenta**, terá primeiro de configurar as definições de início de sessão no cenário de que pretende simular. Estas definições incluem:

- O utilizador que pretende testar 

- As aplicações em nuvem, que o utilizador seria a tentar aceder

- As condições sob as qual acesso para o configura nuvem é efetuada a aplicações
     
Como passo seguinte, pode iniciar uma simulação executar que avalia as suas definições. Apenas as políticas que são ativadas fazem parte de uma edição de avaliação executar.


Quando terminar a avaliação, a ferramenta gera um relatório das políticas afetados.


## <a name="running-the-tool"></a>Executar a ferramenta

Pode encontrar o **e se** ferramenta o  **[acesso condicional - políticas](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies)**  página no portal do Azure.

Para iniciar a ferramenta, na barra de ferramentas por cima da lista de políticas, clique em **e se**.

![E se](./media/active-directory-conditional-access-whatif/01.png)

Antes de poder executar uma edição de avaliação, tem de configurar as definições.

## <a name="settings"></a>Definições

Esta secção fornece informações sobre as definições de simulação executar.

![E se](./media/active-directory-conditional-access-whatif/02.png)


### <a name="user"></a>Utilizador

Só pode selecionar um utilizador. Este é o único campo obrigatório.

### <a name="cloud-apps"></a>Aplicações na cloud

A predefinição para esta definição é **todas as aplicações em nuvem**. A predefinição executa uma edição de avaliação de todas as políticas disponíveis no seu ambiente. Pode restringir o âmbito para as políticas que afetam as aplicações na nuvem específico.


### <a name="ip-address"></a>Endereço IP

O endereço IP é um único endereço IPv4 para imitar o [condição localização](active-directory-conditional-access-azure-portal.md#locations). O endereço representa a Internet com o endereço do dispositivo utilizado pelo seu utilizador para iniciar sessão. Pode verificar se o endereço IP de um dispositivo ao, por exemplo, ao navegar para [que é o meu endereço IP](https://whatismyipaddress.com).    

### <a name="device-platforms"></a>Plataformas de dispositivos

Esta definição mimics o [condição de plataformas de dispositivo](active-directory-conditional-access-azure-portal.md#device-platforms) e representa o equivalente do **todas as plataformas (não suportado, incluindo)**. 
### <a name="client-apps"></a>Aplicações do cliente

Esta definição mimics o [condição de aplicações de cliente](active-directory-conditional-access-azure-portal.md#client-apps).
Por predefinição, esta definição faz com que uma edição de avaliação de todas as políticas ter **Browser** ou **clientes de ambiente de trabalho e aplicações móveis** o individualmente ou ambos selecionado. Também Deteta as políticas que impõem **Exchange ActiveSync (EAS)**. Pode reduzir esta definição ao selecionar:

- **Browser** para avaliar todas as políticas de ter, pelo menos, **Browser** selecionado. 

- **As aplicações móveis e os clientes de ambiente de trabalho** para avaliar todas as políticas de ter, pelo menos, **clientes de ambiente de trabalho e aplicações móveis** selecionado. 


### <a name="sign-in-risk"></a>Risco de início de sessão

Esta definição mimics o [condição de início de sessão risco](active-directory-conditional-access-azure-portal.md#sign-in-risk).   


## <a name="evaluation"></a>Avaliação 

Inicie uma edição de avaliação, clicando em **e se**. O resultado da avaliação disponibiliza um relatório que é composta por: 

![E se](./media/active-directory-conditional-access-whatif/03.png)

- Um indicador se clássico políticas existem no seu ambiente
- Políticas que se aplicam aos seus utilizadores
- Políticas que não se aplicam aos seus utilizadores


Se [políticas clássicas](active-directory-conditional-access-migration.md#classic-policies) existem para as aplicações em nuvem selecionado, é apresentado um indicador para si. Ao clicar em indicador, são redirecionados para a página de políticas clássico. Na página políticas clássica, pode migrar uma política de clássica ou basta desativá-lo. Poderá regressar à sua resultado da avaliação fechando nesta página.

Na lista de políticas que se aplicam para o utilizador selecionado, também pode encontrar uma lista de [conceder controlos](active-directory-conditional-access-controls.md#grant-controls) e [sessão](active-directory-conditional-access-controls.md#session-controls) controlos de utilizador deve satisfazer.

A lista de políticas que não se aplicam aos seus utilizadores, pode e também encontrar motivos por que motivo não aplicam estas políticas. Para cada política listada, o motivo representa a primeira condição que não foi cumprida. Um motivo possível para uma política que não é aplicada é uma política desativada porque não são avaliadas mais.   



## <a name="next-steps"></a>Passos Seguintes

- Se quiser saber como configurar uma política de acesso condicional, consulte [introdução ao acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Se estiver pronto para configurar políticas de acesso condicional para o seu ambiente, consulte o [melhores práticas para acesso condicional no Azure Active Directory](active-directory-conditional-access-best-practices.md). 

- Se pretender migrar clássicas políticas, consulte [migrar políticas clássicas no portal do Azure](active-directory-conditional-access-migration.md)  
