---
title: Atribuir acesso no Azure Cost Management | Microsoft Docs
description: "Atribua acesso a dados de gestão de custos com contas de utilizador que definem os níveis de acesso para as entidades."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/30/2018
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 89639e6d9c06edf110dc8432fbefe1fbfd1298c9
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="assign-access-to-cost-management-data"></a>Atribuir acesso a dados de gestão de custos

O acesso a dados de gestão de custos é fornecido pela gestão de utilizadores ou de entidades. As contas de utilizador do Cloudyn determinam o acesso às *entidades* e às funções administrativas. Existem dois tipos de acesso: administrador e utilizador. A menos que seja modificado pelo utilizador, o acesso de administrador permite a um utilizador utilizar sem restrições todas as funções no portal do Cloudyn, incluindo: gestão de utilizadores, gestão de listas de destinatários e acesso da entidade de raiz a todos os dados de entidade. O acesso de utilizador destina-se aos utilizadores finais para verem e criarem relatórios através do acesso que têm aos dados de entidade.

As entidades são utilizadas para refletir a estrutura hierárquica da sua organização empresarial. Identificam departamentos, divisões e equipas na sua organização no Cloudyn. A hierarquia de entidades ajuda a monitorizar eficazmente os gastos feitos pelas entidades.

Quando registou o seu contrato ou conta do Azure, foi criada uma conta com permissões de administrador no Cloudyn, pelo que pode executar todos os passos neste tutorial. Este tutorial abrange o acesso a dados de gestão de custos, incluindo gestão de utilizadores e de entidades. Saiba como:

> [!div class="checklist"]
> * Criar um utilizador com acesso de administrador
> * Criar um utilizador com acesso de utilizador
> * Criar entidades



## <a name="create-a-user-with-admin-access"></a>Criar um utilizador com acesso de administrador

Embora já tenha acesso de administrador, os seus colegas na organização podem também precisar de ter acesso de administrador. No portal do Cloudyn, clique no símbolo de engrenagem na parte superior direita e selecione **Gestão de Utilizadores**. Clique em **Adicionar Novo Utilizador** para adicionar um novo utilizador.

Introduza as informações necessárias sobre o utilizador. Pode deixar o campo da palavra-passe em branco para que o utilizador possa definir uma nova palavra-passe no primeiro início de sessão. É enviada uma ligação do Cloudyn com informações de início de sessão ao utilizador por e-mail quando seleciona **Notificar utilizador por e-mail**. Escolha as permissões para Permitir Gestão de Utilizadores, para que o utilizador possa criar e modificar outros utilizadores. Gestão de Listas de Destinatários para permitir ao utilizador editar listas de destinatários.

Em **O utilizador tem acesso de administrador**, está selecionada a entidade de raiz da sua organização. Mantenha a raiz selecionada e, em seguida, guarde as informações de utilizador. A seleção da entidade de raiz permite ao utilizador ter permissão de administrador não só para a entidade de raiz na árvore, mas também para todas as entidades que residem abaixo da mesma.  
  ![Adicionar novo utilizador com acesso de administrador](.\media\tutorial-user-access\new-admin-access.png)

## <a name="create-a-user-with-user-access"></a>Criar um utilizador com acesso de utilizador
Os utilizadores típicos que precisem de acesso a dados de gestão de custos, como os dashboards e relatórios, devem ter acesso de utilizador para visualizá-los. Crie um novo utilizador com acesso de utilizador semelhante ao que criou com acesso de administrador, com as seguintes diferenças:

- Desmarque **Permitir Gestão de Utilizadores**, **Permitir Gestão de Listas de Destinatários** e desmarque tudo na lista **O utilizador tem acesso de administrador**.
- Selecione as entidades que o utilizador precisa de aceder na lista **O utilizador tem acesso de utilizador**.
- Também pode permitir o acesso do administrador a entidades específicas, conforme necessário.

![adicionar novo utilizador com acesso de utilizador](.\media\tutorial-user-access\new-user-access.png)

Para ver um vídeo do tutorial sobre como adicionar utilizadores, veja [Adicionar Utilizadores ao Azure Cost Management by Cloudyn](https://youtu.be/Nzn7GLahx30).

## <a name="create-entities"></a>Criar entidades

Quando definir a hierarquia de entidades de custo, é recomendado identificar a estrutura da sua organização.

À medida que cria a árvore, considere a forma como quer ou precisa de ver os seus custos segregados por unidades de negócio, centros de custos, ambientes e departamentos de vendas. A árvore de entidades no Cloudyn é flexível devido à herança de entidades. As subscrições individuais para as suas contas da cloud estão associadas a entidades específicas. Assim, as entidades são multi-inquilino. Pode atribuir o acesso de utilizadores específicos apenas ao respetivo segmento da sua empresa através de entidades. Se o fizer, mantém os dados isolados, mesmo em grandes partes de uma empresa, como subsidiárias. Além disso, o isolamento de dados ajuda na governação.  

Quando registou o seu contrato ou conta do Azure no Cloudyn, os dados dos recursos do Azure, incluindo utilização, desempenho, faturação e dados de etiqueta das suas subscrições, foram copiados para a sua conta do Cloudyn. No entanto, tem de criar manualmente a árvore de entidades. Caso tenha ignorado o registo do Azure Resource Manager, apenas os dados de faturação e alguns relatórios de imobilizado estão disponíveis no portal do Cloudyn.

No portal do Cloudyn, clique em **Definições** na parte superior direita e selecione **Contas da Cloud**. Começa por uma única entidade (raiz) e cria a árvore de entidades na raiz. Segue-se um exemplo de uma hierarquia de entidades que poderá assemelhar-se a muitas organizações de TI após a conclusão da árvore:

![árvore de entidades](.\media\tutorial-user-access\entity-tree.png)

Junto a **Entidades**, clique em **Adicionar Entidade**. Introduza as informações sobre a pessoa ou o departamento que quer adicionar. Os campos **Nome Completo** e **E-mail** não têm de corresponder ao utilizadores existentes. Se quiser ver uma lista de níveis de acesso, procure *Adicionar uma entidade* na ajuda.

![adicionar entidade](.\media\tutorial-user-access\add-entity.png)

Quando tiver terminado, **guarde** a entidade.


Para ver um vídeo do tutorial sobre a criação de uma hierarquia de entidades de custo, veja [Criar uma Hierarquia de Entidades de Custo no Azure Cost Management by Cloudyn](https://youtu.be/dAd9G7u0FmU).

Se for um utilizador do Azure Enterprise Agreement, veja um vídeo do tutorial sobre a associação de contas e subscrições para entidades em [Ligar ao Azure Resource Manager com o Azure Cost Management by Cloudyn](https://youtu.be/oCIwvfBB6kk).

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar um utilizador com acesso de administrador
> * Criar um utilizador com acesso de utilizador
> * Criar entidades

Avance para o tutorial seguinte para aprender a prever despesas através de dados históricos.

> [!div class="nextstepaction"]
> [Prever despesas futuras](tutorial-forecast-spending.md)
