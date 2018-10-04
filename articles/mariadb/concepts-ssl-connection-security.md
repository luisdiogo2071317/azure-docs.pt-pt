---
title: Conectividade SSL para a base de dados do Azure para MariaDB
description: Informações para configurar a base de dados do Azure para MariaDB e os aplicativos associados corretamente utilizar ligações SSL
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 1d0b27a8fd7e3882a73624fa1b668ac602a85e6b
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48249507"
---
# <a name="ssl-connectivity-in-azure-database-for-mariadb"></a>Conectividade SSL na base de dados do Azure para MariaDB
Oferece suporte a base de dados do Azure para MariaDB, ligar o seu servidor de base de dados para aplicações de cliente, utilizando Secure Sockets Layer (SSL). A imposição de ligações SSL entre o servidor de base de dados e as aplicações de cliente ajuda a proteger contra ataques "man-in-the-middle" ao encriptar o fluxo de dados entre o servidor e a sua aplicação.

## <a name="default-settings"></a>Predefinições
Por predefinição, o serviço de base de dados deve ser configurado para exigir ligações SSL ao ligar a MariaDB.  Recomendamos que evite a desativar a opção de SSL, sempre que possível.

Quando aprovisionar uma nova base de dados do Azure para MariaDB servidor através do portal do Azure e a CLI, imposição de ligações SSL está ativada por predefinição.

Cadeias de ligação para várias linguagens de programação são apresentadas no portal do Azure. Essas cadeias de caracteres de conexão incluem os parâmetros SSL necessários para ligar à base de dados. No portal do Azure, selecione o seu servidor. Sob o **definições** cabeçalho, selecione a **cadeias de ligação**. O parâmetro SSL varia com base no conector, por exemplo "ssl = true" ou "sslmode = exigir" ou "sslmode = necessária" e outras variações.

Para saber como ativar ou desativar a ligação SSL ao desenvolvimento de aplicativo, consulte [como configurar o SSL](howto-configure-ssl.md).

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [regras de firewall do servidor](concepts-firewall-rules.md)
- Saiba como [configurar o SSL](howto-configure-ssl.md).
