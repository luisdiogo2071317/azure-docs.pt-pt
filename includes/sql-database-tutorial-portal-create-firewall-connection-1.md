---
author: MightyPen
ms.service: sql-database
ms.topic: include
ms.date: 12/10/2018
ms.author: genemi
ms.openlocfilehash: ab31ee82e8035fe888fa70b5796aef2c2b2939b2
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53728566"
---
## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/)

## <a name="create-a-blank-sql-database"></a>Criar uma base de dados SQL vazia

Uma base de dados SQL do Azure existe dentro de um conjunto definido de [recursos de computação e armazenamento](../articles/sql-database/sql-database-service-tiers-dtu.md). A base de dados funciona num [grupo de recursos do Azure](../articles/azure-resource-manager/resource-group-overview.md) e uma [servidor lógico de base de dados SQL do Azure](../articles/sql-database/sql-database-features.md).

Siga estes passos para criar uma base de dados SQL vazia.

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.

1. Sobre o **New** página, selecione **bases de dados** > **base de dados SQL**.

   ![create empty-database](../articles/sql-database/media/sql-database-design-first-database/create-empty-database.png)

1. Na **base de dados SQL** painel, escreva ou selecione os seguintes valores:

   | Definição       | Valor sugerido | Descrição |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Nome da base de dados** | *yourDatabase* | Para nomes de base de dados válida, veja [identificadores de bases de dados](/sql/relational-databases/databases/database-identifiers). |
   | **Subscrição** | *yourSubscription*  | Para obter detalhes sobre as suas subscrições, veja [Subscriptions](https://account.windowsazure.com/Subscriptions) (Subscrições). |
   | **Grupo de recursos** | *yourResourceGroup* | Para nomes de grupo de recursos válidos, veja [Naming rules and restrictions](/azure/architecture/best-practices/naming-conventions) (Atribuição de nomes de regras e restrições). |
   | **Selecionar origem** | Base de dados vazia | Especifica que deve ser criada uma base de dados vazia. |

   ![criar base de dados](../articles/sql-database/media/sql-database-design-first-database/create-database.png)

   1. Selecione **servidor** para configurar um servidor da base de dados nova. Em seguida, escreva ou selecione os seguintes valores:

      | Definição       | Valor sugerido | Descrição |
      | ------------ | ------------------ | ------------------------------------------------- |
      | **Nome do servidor** | Qualquer nome globalmente exclusivo | Para nomes de servidores válidos, veja [Naming rules and restrictions](/azure/architecture/best-practices/naming-conventions) (Atribuição de nomes de regras e restrições). |
      | **Início de sessão de administrador do servidor** | Qualquer nome válido | Para nomes de início de sessão válido, veja [identificadores de bases de dados](/sql/relational-databases/databases/database-identifiers).|
      | **Palavra-passe** | Qualquer palavra-passe válida | A palavra-passe tem de ter, pelo menos, 8 carateres e tem de utilizar carateres de três das seguintes categorias: carateres maiúsculos, carateres minúsculos, números e carateres não alfanuméricos. |
      | **Localização** | Nenhuma localização válida | Para obter informações sobre regiões, consulte [regiões do Azure](https://azure.microsoft.com/regions/). |

      Escolha **Selecionar**.

      ![criar servidor de base de dados](../articles/sql-database/media/sql-database-design-first-database/create-database-server.png)

   1. Selecione **escalão de preço** para especificar o escalão de serviço, o número de DTUs e a quantidade de armazenamento. Explore as opções para as DTUs e armazenamento que está disponível para cada escalão de serviço.

      Depois de selecionar o escalão de servidor, o número de DTUs e a quantidade de armazenamento, selecione **aplicar**.

   1. Introduza um **agrupamento** para a base de dados em branco (para este tutorial, utilize o valor predefinido). Para obter mais informações sobre agrupamentos, veja [agrupamentos](/sql/t-sql/statements/collations).

1. Agora que concluiu o **base de dados SQL** formulário, selecione **criar** para criar a base de dados. Este passo pode demorar até um minuto e meio a concluir.

1. Na barra de ferramentas, clique em **Notificações** para monitorizar o processo de implementação.

     ![notificação](../articles/sql-database/media/sql-database-design-first-database/notification.png)

## <a name="create-a-firewall-rule"></a>Criar uma regra de firewall

O serviço de base de dados SQL cria uma firewall ao nível do servidor, para impedir que aplicações e ferramentas externas estabeleçam uma ligação ao servidor ou a quaisquer bases de dados no servidor. Siga estes passos para criar uma [regra de firewall ao nível do servidor de base de dados SQL](../articles/sql-database/sql-database-firewall-configure.md) para o endereço IP do seu cliente. Este processo permite conectividade externa através da firewall de base de dados SQL para o seu endereço IP apenas.

> [!NOTE]
> Base de dados SQL comunica através da porta 1433. Se estiver a tentar ligar a partir de uma rede empresarial, o tráfego de saída através da porta 1433 poderá não ser permitido pela firewall da rede. Se assim for, não é possível ligar ao servidor de base de dados SQL do Azure, a menos que o administrador abra a porta 1433.

1. Depois de concluída a implementação, escolha **bases de dados SQL** no menu do lado esquerdo e, em seguida, selecione *yourDatabase* sobre o **bases de dados SQL** página. O **descrição geral** página para a sua base de dados aberta, que mostra o nome de servidor completamente qualificado (como *yourserver.database.windows.net*) e oferece opções para configuração adicional.

1. Copie o nome de servidor completamente qualificado para utilizar para ligar ao seu servidor e respetivas bases de dados em passos posteriores.

   ![nome do servidor](../articles/sql-database/media/sql-database-design-first-database/server-name.png)

1. Selecione **definir firewall do servidor** na barra de ferramentas. O **definições da Firewall** é aberta a página do servidor de base de dados SQL.

   ![regra de firewall do servidor](../articles/sql-database/media/sql-database-design-first-database/server-firewall-rule.png)

   1. Selecione **Adicionar IP de cliente** na barra de ferramentas para adicionar o seu endereço IP atual a uma nova regra de firewall. Uma regra de firewall consegue abrir a porta 1433 para um único endereço IP ou para um intervalo de endereços IP.

   1. Escolha **Guardar**. É criada uma regra de firewall ao nível do servidor para a sua porta de abertura 1433 do endereço IP atual no servidor lógico.

   1. Selecione **OK** e, em seguida, feche a **definições da Firewall** página.

Seu endereço IP agora pode passar através da firewall e pode agora ligar ao servidor de base de dados SQL e respetivas bases de dados utilizando o SSMS ou outra ferramenta à sua escolha. Certifique-se de que utilizar conta de administrador do servidor que criou anteriormente.

> [!IMPORTANT]
> Por predefinição, o acesso através da firewall de base de dados SQL está ativado para todos os serviços do Azure. Selecione **OFF** nesta página para desativar todos os serviços Azure.
