---
title: Personalizar mapeamentos de atributos de AD do Azure | Documentos da Microsoft
description: Saiba quais mapeamentos de atributos para aplicações de SaaS no Azure Active Directory são como pode modificá-los para atender as suas necessidades de negócio.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/09/2018
ms.author: celested
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: c1926849c8ec63b4240d951e46b1341f31f7c5bd
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56170351"
---
# <a name="customizing-user-provisioning-attribute-mappings-for-saas-applications-in-azure-active-directory"></a>Personalizar mapeamentos de atributos de Provisionamento para aplicações de SaaS no Azure Active Directory de usuários
Microsoft Azure AD fornece suporte para aprovisionamento de utilizadores para aplicações de SaaS de terceiros, como o Salesforce, Google Apps e outros. Se tiver de aprovisionamento de utilizadores para uma aplicação SaaS de terceiros ativada, o portal do Azure controla os valores de atributo na forma de mapeamentos de atributos.

Existe um conjunto pré-configuradas de atributos e mapeamentos de atributos entre objetos de utilizador do Azure AD e os objetos de utilizador de cada aplicação de SaaS. Algumas aplicações, gerir outros tipos de objetos, além de utilizadores, tais como grupos. <br> 
 Pode personalizar os mapeamentos de atributos padrão, de acordo com suas necessidades empresariais. Isso significa que, pode alterar ou eliminar mapeamentos de atributos existentes ou criar novos de mapeamentos de atributos.
 
## <a name="editing-user-attribute-mappings"></a>Editar mapeamentos de atributos de utilizador

No portal do Azure AD, pode aceder a esta funcionalidade ao clicar num **mapeamentos** configuração em **aprovisionamento** no **gerir** secção de um  **Aplicação empresarial**.


![Salesforce](./media/customize-application-attributes/21.png) 

Clicar num **mapeamentos** configuração, é aberto o relacionados **mapeamento do atributo** ecrã. Existem mapeamentos de atributos que são necessários para uma aplicação SaaS para funcionar corretamente. Para os atributos necessários, o **eliminar** funcionalidade não está disponível.


![Salesforce](./media/customize-application-attributes/22.png)

No exemplo acima, pode ver que o **nome de utilizador** atributo de um objeto gerido no Salesforce é preenchido com o **userPrincipalName** valor do objeto ligado para o Azure Active Directory.

Pode personalizar a existente **mapeamentos de atributos** ao clicar num mapeamento. Esta ação abre o **Editar atributo** ecrã.

![Salesforce](./media/customize-application-attributes/23.png)


### <a name="understanding-attribute-mapping-types"></a>Compreender os tipos de mapeamento do atributo
Com mapeamentos de atributos, controla a forma como os atributos são preenchidos numa aplicação SaaS de terceiros. Existem quatro tipos de mapeamento diferentes suportados:

* **Direto** – o atributo de destino é preenchido com o valor de um atributo do objeto ligado no Azure AD.
* **Constante** – o atributo de destino é preenchido com uma cadeia específica que especificou.
* **Expressão** -o atributo de destino é preenchido com base no resultado de uma expressão de tipo de script. 
  Para obter mais informações, consulte [escrever expressões para mapeamentos de atributos no Azure Active Directory](functions-for-customizing-application-data.md).
* **Nenhum** -o atributo de destino é deixado sem modificações. No entanto, se o atributo de destino está sempre vazio, é preenchida com o valor predefinido que especificar.

Para além desses quatro tipos básicos, mapeamentos de atributos personalizados suportam o conceito de opcional **predefinição** atribuição de valor. A atribuição de valor de predefinição garante que um atributo de destino é preenchido com um valor se não houver nenhuma um valor no Azure AD nem no objeto de destino. A configuração mais comuns é deixar em branco.


### <a name="understanding-attribute-mapping-properties"></a>Noções básicas sobre as propriedades de mapeamento do atributo

Na secção anterior, já foram introduzidas para o tipo de mapeamento do atributo de propriedade.
Para além desta propriedade, mapeamentos de atributos também suportam os seguintes atributos:

- **Atributo de origem** -o atributo de utilizador do sistema de origem (exemplo: Azure Active Directory).
- **Atributo de destino** – o atributo de utilizador no sistema de destino (exemplo: ServiceNow).
- **Correspondência de objetos utilizando este atributo** – independentemente desse mapeamento deve ser utilizado para identificar exclusivamente os utilizadores entre os sistemas de origem e de destino. Normalmente, isso é definido no atributo userPrincipalName ou email no Azure AD, que normalmente é mapeado para um campo de nome de utilizador num aplicativo de destino.
- **Precedência de correspondência** – o várias correspondência de atributos pode ser definida. Quando houver múltiplos, eles são avaliados na ordem definida por este campo. Assim que uma correspondência for encontrada, não são necessárias mais correspondência de atributos são avaliados.
- **Aplicar este mapeamento**
    - **Sempre** – aplicar este mapeamento em ambos os criação de utilizador e ações de atualização
    - **Apenas durante a criação** -aplicar este mapeamento apenas nas ações de criação do utilizador


## <a name="editing-group-attribute-mappings"></a>Editar mapeamentos de atributos de grupo

Um número selecionado de aplicativos, como o ServiceNow, a caixa e o Google Apps, suporta a capacidade de provisionar objetos de grupo, além de objetos de utilizador. Objetos de grupo podem conter propriedades do grupo, como os nomes a apresentar e aliases, além de membros do grupo de e-mail.

![ServiceNow](./media/customize-application-attributes/24.png)

Aprovisionamento do grupo pode ser opcionalmente ativado ou desativado, selecionando o mapeamento de grupo sob **mapeamentos**e a definição **ativado** para a opção pretendida no **mapeamento do atributo** ecrã.

Os atributos aprovisionados como parte dos objetos de grupo podem ser personalizados da mesma forma como os objetos de utilizador, descrito anteriormente. 

>[!TIP]
>O aprovisionamento de objetos de grupo (propriedades e membros) é um conceito distinto da [atribuir grupos](assign-user-or-group-access-portal.md) a uma aplicação. É possível atribuir um grupo a uma aplicação, mas apenas aprovisionar os objetos de utilizador contidos no grupo. Aprovisionamento de objetos de grupo completo não é necessário para utilizar grupos na atribuições.


## <a name="editing-the-list-of-supported-attributes"></a>Editar a lista de atributos suportados

Os atributos de utilizador suportados para um determinado aplicativo estão pré-configuradas. APIs de gestão de utilizador a maioria das aplicações não suportam a deteção de esquema, portanto o Azure AD que o serviço de aprovisionamento não é capaz de gerar dinamicamente a lista de atributos suportados por meio de chamadas para a aplicação. 

No entanto, algumas aplicações suportam atributos personalizados. Por ordem para o serviço de aprovisionamento do AD do Azure para poder ler e escrever atributos personalizados, é necessário introduzir as respetivas definições no portal do Azure com o **Mostrar opções avançadas** caixa de verificação na parte inferior do  **Mapeamento do atributo** ecrã.

Aplicações e sistemas que suportam a personalização da lista de atributos incluem:

* Salesforce
* ServiceNow
* Dia de trabalho
* O Azure Active Directory ([atributos de padrão do Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity) e extensões de diretório personalizados são suportadas)
* As aplicações que suportam [SCIM 2.0](https://tools.ietf.org/html/rfc7643), em que os atributos definidos na [esquema central](https://tools.ietf.org/html/rfc7643) precisam de ser adicionados

>[!NOTE]
>Editar a lista de atributos suportados apenas é recomendada para os administradores que tem personalizado o esquema de seus aplicativos e sistemas e tenham conhecimento de primeira mão de como seus atributos personalizados foram definidos. Às vezes, é necessário estar familiarizado com as APIs e ferramentas de programação fornecido por um aplicativo ou sistema. 

![Editor](./media/customize-application-attributes/25.png) 

Ao editar a lista de atributos suportados, são fornecidas as seguintes propriedades:

* **Nome** -o nome do sistema do atributo, tal como definido no esquema do objeto de destino. 
* **Tipo de** -o tipo de dados que armazena o atributo, tal como definido no esquema do objeto de destino. Isto pode ser um dos seguintes:
   * *Binário* -atributo contém dados binários.
   * *Booleano* -atributo contém um valor de VERDADEIRO ou FALSO.
   * *DateTime* -atributo contém uma cadeia de data.
   * *Número inteiro* -atributo contém um número inteiro.
   * *Referência* -atributo contém um ID que faz referência a um valor armazenado em outra tabela na aplicação de destino.
   * *Cadeia de caracteres* -atributo contém uma cadeia de texto. 
* **Chave primária?** -Ou não o atributo é definido como um campo de chave primária no esquema do objeto de destino.
* **Necessário?** -Ou não o atributo é necessário para ser preenchido em sistemas ou aplicativos de destino.
* **Valores múltiplos?** -Ou não o atributo oferece suporte a vários valores.
* **Maiúsculas ou minúsculas?** -Ou não os valores de atributos são avaliados de uma forma de maiúsculas e minúsculas.
* **Expressão de API** -não utilizar, exceto se instruído a fazer isso, a documentação para um conector de aprovisionamento específico (por exemplo, o dia de trabalho).
* **Referenciado atributo de objeto** - se se tratar de um atributo de tipo de referência, em seguida, esse menu permite-lhe selecionar a tabela e o atributo no aplicativo de destino que contém o valor associado com o atributo. Por exemplo, se tiver um atributo com o nome "Departamento", cujo valor armazenado referencia um objeto numa tabela de "Departamentos" separada, deve selecionar "Departments.Name". Tenha em atenção que as tabelas de referência e os campos de ID principal suportados para um determinado aplicativo pré-configuradas e atualmente não podem ser editados com o portal do Azure, mas podem ser editados utilizando o [Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-configure-with-custom-target-attributes).

Para adicionar um novo atributo, desloque-se para o final da lista de atributos suportados, preencha os campos acima usando entradas fornecidas e selecione **adicionar atributo**. Selecione **guardar** quando concluir a adição de atributos. Em seguida, terá de recarregar o **aprovisionamento** separador para os novos atributos fiquem disponíveis no editor de mapeamento do atributo.

## <a name="restoring-the-default-attributes-and-attribute-mappings"></a>Restaurar a atributos predefinidos e mapeamentos de atributos

Caso precise começar novamente e reposição seus mapeamentos existentes de volta para o seu estado padrão, pode selecionar o **restaurar os mapeamentos predefinidos** caixa de verificação e guardar a configuração. Isso define todos os mapeamentos, como se o aplicativo simplesmente tinha sido adicionado ao seu inquilino do Azure AD da Galeria de aplicações. 

A seleção desta opção efetivamente forçará uma nova sincronização de todos os utilizadores enquanto o serviço de aprovisionamento está em execução. 

>[!IMPORTANT]
>É altamente recomendável que **estado de aprovisionamento** definida como **desativar** antes de invocar esta opção.


## <a name="what-you-should-know"></a>O que deve saber

* Microsoft Azure AD fornece uma implementação eficiente de um processo de sincronização. Num ambiente inicializado, apenas os objetos que precisam de atualizações são processados durante um ciclo de sincronização. 

* A atualizar mapeamentos de atributo tem um impacto no desempenho de um ciclo de sincronização. Uma atualização para a configuração de mapeamento do atributo necessitar de todos os objetos geridos ser reavaliadas. 

* É uma prática recomendada para manter o número de alterações consecutivas para seus mapeamentos de atributos no mínimo.


## <a name="next-steps"></a>Passos Seguintes

* [Automatizar o utilizador aprovisionamento/desaprovisionamento às aplicações SaaS](user-provisioning.md)
* [Escrever expressões para mapeamentos de atributos](functions-for-customizing-application-data.md)
* [Filtros de âmbito para o aprovisionamento do utilizador](define-conditional-rules-for-provisioning-user-accounts.md)
* [Utilizar o SCIM para ativar o aprovisionamento automático de utilizadores e grupos do Azure Active Directory a aplicações](use-scim-to-provision-users-and-groups.md)
* [Lista de tutoriais sobre como integrar aplicações SaaS](../saas-apps/tutorial-list.md)


