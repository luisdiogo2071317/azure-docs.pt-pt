---
title: Base de dados do Azure para as regras de firewall do servidor de PostgreSQL
description: Este artigo descreve as regras de firewall para a base de dados do Azure para o servidor de PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 8a3f5d9fa8f1c36d8468c38f7dda803d3ca1d832
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="azure-database-for-postgresql-server-firewall-rules"></a>Base de dados do Azure para as regras de firewall do servidor de PostgreSQL
Base de dados do Azure para a firewall do servidor de PostgreSQL impede que todos os acessos ao seu servidor de base de dados até especificar quais os computadores que tem permissão. A firewall concede acesso ao servidor com base no endereço IP de origem de cada pedido.
Para configurar a firewall, crie as regras de firewall que especificam intervalos de endereços IP aceitáveis. Pode criar regras de firewall ao nível do servidor.

**Regras de firewall:** estas regras permitem que os clientes aceder à sua base de dados do Azure completa para o servidor de PostgreSQL, ou seja, todas as bases de dados no mesmo servidor lógico. Regras de firewall ao nível do servidor podem ser configuradas ao utilizar o portal do Azure ou utilizar comandos da CLI do Azure. Para criar regras de firewall ao nível do servidor, tem de ser o proprietário da subscrição ou um contribuinte da subscrição.

## <a name="firewall-overview"></a>Descrição geral das firewalls
Por predefinição, todos os acessos de base de dados na base de dados do Azure para o servidor de PostgreSQL está bloqueado pela firewall. Para começar a utilizar o servidor a partir de outro computador, tem de especificar uma ou mais regras de firewall ao nível do servidor para ativar o acesso ao seu servidor. Utilize as regras de firewall para especificar que IP intervalos de endereços da Internet para permitir. Acesso para o site do portal do Azure em si não é afetado pelas regras de firewall.
Tentativas de ligação provenientes da Internet e o Azure tem primeiro de passar através da firewall antes que podem aceder a base de dados PostgreSQL, conforme mostrado no diagrama seguinte:

![Fluxo de exemplo de como funciona a firewall](media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a>Ligar a partir da Internet
Regras de firewall ao nível do servidor aplicam-se a todas as bases de dados a mesma base de dados do Azure para o servidor de PostgreSQL. Se o endereço IP do pedido estiver dentro dos intervalos especificados nas regras de firewall ao nível do servidor, é concedida ligação.
Se o endereço IP do pedido não está dentro dos intervalos especificados em qualquer uma das regras de firewall ao nível do servidor, o pedido de ligação falha.
Por exemplo, se a sua aplicação estabelece ligação com o controlador JDBC PostgreSQL, pode encontrar este erro ao tentar ligar quando a firewall está a bloquear a ligação.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: FATAL: nenhum grupo de proteção\_hba.conf entrada para o anfitrião "123.45.67.890", o utilizador "adminuser", base de dados "postgresql", SSL

## <a name="connecting-from-azure"></a>Ligar a partir do Azure
Para permitir que as aplicações do Azure ligar à base de dados do Azure para o servidor de PostgreSQL, ligações das Azure tem de estar ativadas. Por exemplo, para alojar uma aplicação de Web Apps do Azure ou uma aplicação que é executado numa VM do Azure ou para ligar a partir de um gateway de gestão de dados do Azure Data Factory. Os recursos não precisam de estar na mesma rede Virtual (VNet) ou grupo de recursos para a regra de firewall para permitir essas ligações. Quando uma aplicação do Azure tenta ligar ao servidor de base de dados, a firewall verifica se as ligações do Azure são permitidas. Existem dois métodos para ativar estes tipos de ligações. Uma definição de firewall com o endereço de início e de fim igual a 0.0.0.0 indica que estas ligações são permitidas. Em alternativa, pode definir o **permitir o acesso aos serviços do Azure** opção para **ON** no portal do **segurança de ligação** painel e prima **guardar**. Se a tentativa de ligação não for permitida, o pedido não atingir a base de dados do Azure para o servidor de PostgreSQL.

> [!IMPORTANT]
> Esta opção configura a firewall para permitir todas as ligações a partir do Azure, incluindo ligações de subscrições de outros clientes. Quando seleciona esta opção, certifique-se de que as permissões de início de sessão e de utilizador limitam o acesso a utilizadores autorizados apenas.
> 

![Configurar a permitir o acesso aos serviços do Azure no portal](media/concepts-firewall-rules/allow-azure-services.png)

## <a name="programmatically-managing-firewall-rules"></a>Gerir regras de firewall programaticamente
Para além de portal do Azure, as regras de firewall podem ser geridas através de programação utilizando a CLI do Azure.
Consulte também [criar e gerir a base de dados do Azure PostgreSQL das regras de firewall ao utilizar a CLI do Azure](howto-manage-firewall-using-cli.md)

## <a name="troubleshooting-the-database-server-firewall"></a>Resolução de problemas da firewall do servidor de base de dados
Quando o acesso à base de dados do Microsoft Azure para o serviço de servidor PostgreSQL não comportar-se como esperado, considere os seguintes pontos:

* **Alterações à lista de permissões não efetuou efeito ainda:** poderão existir como sucederia com um atraso de cinco minutos para as alterações à base de dados do Azure para a configuração da firewall do servidor de PostgreSQL entrem em vigor.

* **O início de sessão não está autorizado ou foi utilizada uma palavra-passe incorreta:** se um início de sessão não tem permissões na base de dados do Azure para o servidor de PostgreSQL ou a palavra-passe utilizada está incorreta, a ligação à base de dados do Azure para o servidor de PostgreSQL é negada. Criar uma definição de firewall apenas fornece aos clientes uma oportunidade para tentar ligar ao seu servidor; cada cliente ainda tem de fornecer as credenciais de segurança necessário.

Por exemplo, utilizando um cliente JDBC, poderá aparecer o erro seguinte.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: FATAL: Falha na autenticação de palavra-passe para o utilizador "yourusername"

* **Endereço IP dinâmico:** se tiver uma ligação à Internet com endereçamento IP dinâmico e estiver a ter dificuldades em passar a firewall, pode experimentar uma das soluções seguintes.

* Peça ao seu fornecedor de serviços Internet (ISP) para o intervalo de endereços IP atribuído aos computadores cliente que acedem a base de dados do Azure para o servidor de PostgreSQL e, em seguida, adicione o intervalo de endereços IP como uma regra de firewall.

* Obter estático endereçamento IP em vez disso, para os computadores cliente e, em seguida, adicione o endereço IP estático como uma regra de firewall.

## <a name="next-steps"></a>Passos Seguintes
Para artigos sobre a criação de regras de firewall ao nível do servidor e o nível de base de dados, consulte:
* [Criar e gerir a base de dados do Azure para as regras de firewall de PostgreSQL no portal do Azure](howto-manage-firewall-using-portal.md)
* [Criar e gerir a base de dados do Azure PostgreSQL das regras de firewall ao utilizar a CLI do Azure](howto-manage-firewall-using-cli.md)
