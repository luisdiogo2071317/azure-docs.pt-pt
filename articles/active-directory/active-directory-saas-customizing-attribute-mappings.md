---
title: Personalizar os mapeamentos de atributos do Azure AD | Microsoft Docs
description: Identificar os mapeamentos de atributos para aplicações de SaaS no Azure Active Directory como a pode modificá-los para abordar as necessidades de negócio.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 549e0b8c-87ce-4c9b-b487-b7bf0155dc77
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2018
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 565394664ab59ef5186503f708502eacc040321f
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295630"
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Personalização de utilizador aprovisionamento mapeamentos de atributos para aplicações de SaaS no Azure Active Directory
Microsoft Azure AD fornece suporte para o aprovisionamento de utilizadores para aplicações de SaaS de terceiros, tais como o Salesforce, Google Apps e outros. Se tiver utilizadores de aprovisionamento para uma aplicação SaaS de terceiros ativada, o portal do Azure controla os valores de atributo na forma de uma configuração denominada "mapeamento de atributos".

Não há um conjunto de atributos e os mapeamentos de atributos entre objetos de utilizador do Azure AD e objetos de utilizador de cada aplicação SaaS previamente configurado. Algumas aplicações, gerir outros tipos de objetos, além de utilizadores, tais como grupos. <br> 
 Pode personalizar os mapeamentos de atributos de predefinição consoante as suas necessidades de negócio. Isto significa que pode alterar ou eliminar mapeamentos de atributos existente ou criar novos mapeamentos de atributos.
 
## <a name="editing-user-attribute-mappings"></a>Editar mapeamentos de atributos de utilizador

No portal do Azure AD, pode aceder a esta funcionalidade, clicando num **mapeamentos** configuração em **aprovisionamento** no **gerir** secção um  **Aplicação empresarial**.


![Salesforce][5] 

Ao clicar num **mapeamentos** configuração, abre o relacionados **atributo mapeamento** ecrã. Existem mapeamentos de atributos que são necessários para uma aplicação SaaS a funcionar corretamente. Para os atributos necessários, o **eliminar** funcionalidade não está disponível.


![Salesforce][6]  

No exemplo acima, pode ver que o **Username** atributo de um objeto gerido no Salesforce é preenchido com o **userPrincipalName** valor o objeto ligado para o Azure Active Directory.

Pode personalizar existente **mapeamentos de atributos** clicando um mapeamento. Esta ação abre o **Editar atributo** ecrã.

![Salesforce][7]  


### <a name="understanding-attribute-mapping-types"></a>Compreender tipos de mapeamento de atributos
Com mapeamentos de atributos, pode controlar a forma como os atributos são preenchidos numa aplicação SaaS de terceiros. Existem quatro tipos de mapeamento diferentes suportados:

* **Direto** – o atributo de destino é preenchido com o valor de um atributo do objeto ligado no Azure AD.
* **Constante** – o atributo de destino é preenchido com uma cadeia específica que especificou.
* **Expressão** -o atributo de destino é preenchido com base no resultado de uma expressão de tipo de script. 
  Para obter mais informações, consulte [escrever expressões para mapeamentos de atributos no Azure Active Directory](active-directory-saas-writing-expressions-for-attribute-mappings.md).
* **Nenhum** -o atributo de destino for deixado inalterado. No entanto, se o atributo de destino é alguma vez vazio, é preenchido com o valor predefinido que especificar.

Para além destes quatro tipos de mapeamento de atributo básica, os mapeamentos de atributos personalizados suportam o conceito de opcional **predefinido** valor atribuição. A atribuição de valor predefinido garante que um atributo de destino é preenchido com um valor se não existir nenhuma um valor no Azure AD, nem o objeto de destino. A configuração mais comuns é a deixar isto em branco.


### <a name="understanding-attribute-mapping-properties"></a>Noções sobre as propriedades de mapeamento de atributos

Na secção anterior, poderá já foram introduzidos para a propriedade de tipo de mapeamento de atributos.
Para além desta propriedade mapeamentos de atributos também suportam os seguintes atributos:

- **Atributo de origem** -o atributo de utilizador do sistema de origem (exemplo: Azure Active Directory).
- **Atributo de destino** – o atributo de utilizador no sistema de destino (exemplo: ServiceNow).
- **Corresponder objetos utilizando este atributo** – se ou não desta mapeamento deve ser utilizado para identificar exclusivamente os utilizadores entre os sistemas de origem e de destino. Normalmente, isto está definido no atributo userPrincipalName ou correio no Azure AD, que normalmente é mapeado para um campo de nome de utilizador numa aplicação de destino.
- **Correspondência de precedência** – vários atributos de correspondência pode ser definido. Quando existem vários, estes são avaliados por ordem definida por este campo. Assim que for encontrada uma correspondência, não são necessárias mais correspondência de atributos são avaliados.
- **Aplicar este mapeamento**
    - **Sempre** – aplicar desta mapeamento em ambos os criação de utilizador e as ações de atualização
    - **Apenas durante a criação** -aplicar desta mapeamento apenas ações de criação de utilizador


## <a name="editing-group-attribute-mappings"></a>Editar mapeamentos de atributos de grupo

Um número selecionado de aplicações, como o ServiceNow, a caixa e o Google Apps, suporta a capacidade de aprovisionar os objetos de grupo para além dos objetos de utilizador. Objetos de grupo podem conter propriedades de grupo, tais como os nomes a apresentar e aliases, além de membros do grupo de e-mail.

![ServiceNow][8]  

Aprovisionamento de grupo pode ser opcionalmente ativado ou desativado, selecionando o mapeamento de grupo em **mapeamentos**e definição **ativado** para a opção pretendida no **omapeamentodeatributos** ecrã.

Os atributos aprovisionados como parte dos objetos de grupo podem ser personalizados da mesma forma que os objetos de utilizador, descrito anteriormente. 

>[!TIP]
>O aprovisionamento dos objetos de grupo (propriedades e membros) é um conceito distinto do [atribuir grupos](manage-apps/assign-user-or-group-access-portal.md) para uma aplicação. É possível atribuir um grupo a uma aplicação, mas apenas aprovisionar os objetos de utilizador contidos no grupo. O aprovisionamento dos objetos de grupo completo não é necessário para utilizar grupos na atribuições.


## <a name="editing-the-list-of-supported-attributes"></a>Editar a lista de atributos suportados

Os atributos de utilizador suportados para uma determinada aplicação são previamente configurados. APIs de gestão de utilizador a maioria das aplicações não suporta a deteção de esquema, não é capaz de gerar dinamicamente a lista de atributos suportados efetuando chamadas para a aplicação, por conseguinte, o Azure AD que o serviço de fornecimento. 

No entanto, algumas aplicações suportam atributos personalizados. Ordem para o Azure AD que o serviço de fornecimento conseguir ler e escrever os atributos personalizados, é necessário introduzir as respetivas definições para o Azure portal com o **Mostrar opções avançadas** caixa de verificação na parte inferior do  **Mapeamento de atributos** ecrã.

Aplicações e sistemas que suportam a personalização da lista de atributos incluem:

* Salesforce
* ServiceNow
* Workday
* Azure Active Directory
* Active Directory no local (como parte do utilizador Workday aprovisionamento conector)
* As aplicações que suportam [SCIM 2.0](https://tools.ietf.org/html/rfc7643), em que os atributos definidos no [esquema core](https://tools.ietf.org/html/rfc7643) precisam de ser adicionados

>[!NOTE]
>Editar a lista de atributos suportados só é recomendada para os administradores que tem personalizado o esquema das respetivas aplicações e sistemas e tem conhecimento de primeira mão da forma como os respetivos atributos personalizados foram definidos. Por vezes, é necessário familiaridade com as ferramentas APIs e os programadores fornecidas por uma aplicação ou sistema. 

![Editor][9]  

Ao editar a lista de atributos suportados, são fornecidas as seguintes propriedades:

* **Nome** -o nome do sistema do atributo, tal como definido no esquema o objeto de destino. 
* **Tipo** -o tipo de dados que armazena o atributo, tal como definido no esquema o objeto de destino. Isto pode ser um dos seguintes:
   * *Binário* -atributo contém dados binários.
   * *Booleano* -atributo contém um valor VERDADEIRO ou FALSO.
   * *DateTime* -atributo contém uma cadeia de data.
   * *Número inteiro* -atributo contém um número inteiro.
   * *Referência* -atributo contém um ID que faça referência a um valor armazenado no outra tabela na aplicação de destino.
   * *Cadeia* -atributo contém uma cadeia de texto. 
* **Chave primária?** -Se ou não o atributo é definido como um campo de chave primário no esquema o objeto de destino.
* **Necessário?** -Se ou não o atributo é necessário para ser preenchida no sistema ou a aplicação de destino.
* **Valores múltiplos?** -Se ou não o atributo suporta vários valores.
* **Caso exato?** -Se ou não os valores de atributos são avaliados de forma a maiúsculas e minúsculas.
* **Expressão de API** -não utilize, a menos que indicado para o fazer, a documentação de um conector de aprovisionamento específico (por exemplo, Workday).
* **Referenciado atributo de objeto** - se se tratar de um atributo de tipo de referência, em seguida, este menu permite-lhe selecionar a tabela e o atributo da aplicação de destino que contém o valor associado o atributo. Por exemplo, se tiver um atributo com o nome "Departamento", cujo valor armazenado referencia um objeto numa tabela "Departamentos" separado, deverá selecionar "Departments.Name". Tenha em atenção que as tabelas de referência e os campos ID principais suportados para uma determinada aplicação previamente estão configurados e atualmente não podem ser editados utilizando o portal do Azure, mas podem ser editados utilizando o [Graph API](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/synchronization-configure-with-custom-target-attributes).

Para adicionar um novo atributo, desloque até ao final da lista de atributos suportados, preencha os campos acima utilizando as entradas fornecidas e selecione **adicionar atributo**. Selecione **guardar** quando terminar de adicionar atributos. Em seguida, precisa de recarregar a **aprovisionamento** separador para novos atributos fique disponível no editor de mapeamento de atributos.

## <a name="restoring-the-default-attributes-and-attribute-mappings"></a>Restaurar a atributos predefinidos e mapeamentos de atributos

Deve terá início ao longo e repor a sua mapeamentos existentes de volta para o respetivo estado predefinido, pode selecionar o **restaurar mapeamentos de predefinição** caixa de verificação e guardar a configuração. Isto define todos os mapeamentos como se a aplicação apenas tinha foi adicionada ao inquilino do Azure AD da Galeria de aplicações. 

A seleção desta opção irá eficazmente forçar uma sincronização de todos os utilizadores a enquanto o aprovisionamento está em execução. 

>[!IMPORTANT]
>Recomenda-se vivamente que **estado de aprovisionamento** ser definido como **desativar** antes de invocar esta opção.


## <a name="what-you-should-know"></a>O que deve conhecer

* Microsoft Azure AD fornece uma implementação eficaz de um processo de sincronização. Num ambiente inicializado, apenas os objetos que necessitam de atualizações são processados durante um ciclo de sincronização. 

* Atualizar mapeamentos de atributos tem um impacto no desempenho de um ciclo de sincronização. Uma atualização para a configuração de mapeamento de atributos requer que todos os objetos geridos para ser reavaliadas. 

* É uma melhor prática recomendada para manter o número de alterações consecutivas para os mapeamentos de atributos no mínimo.


## <a name="next-steps"></a>Passos seguintes

* [Automatizar utilizador aprovisionamento/desaprovisionamento para aplicações SaaS](active-directory-saas-app-provisioning.md)
* [Escrever expressões para mapeamentos de atributos](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Filtros de âmbito para o aprovisionamento de utilizador](active-directory-saas-scoping-filters.md)
* [Utilizar o SCIM para ativar o aprovisionamento automático de utilizadores e grupos do Azure Active Directory a aplicações](manage-apps/use-scim-to-provision-users-and-groups.md)
* [Lista de tutoriais sobre como integrar aplicações SaaS](active-directory-saas-tutorial-list.md)

<!--Image references-->
[5]: ./media/active-directory-saas-customizing-attribute-mappings/21.png
[6]: ./media/active-directory-saas-customizing-attribute-mappings/22.png
[7]: ./media/active-directory-saas-customizing-attribute-mappings/23.png
[8]: ./media/active-directory-saas-customizing-attribute-mappings/24.png
[9]: ./media/active-directory-saas-customizing-attribute-mappings/25.PNG

