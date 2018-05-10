---
title: Gerir os dados de utilizador no Azure AD B2C | Microsoft Docs
description: Saiba como eliminar ou exportar dados de utilizador no Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 05/06/2018
ms.author: davidmu
ms.openlocfilehash: 414221c3e4942801b5792835d520ec936c8c4bbb
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="manage-user-data-in-azure-ad-b2c"></a>Gerir os dados de utilizador no Azure AD B2C

 Este artigo fornece informações sobre como pode gerir os dados de utilizador no Azure Active Directory (AD) B2C, utilizando as operações que forneceu o [Active Directory Graph API do Azure](https://msdn.microsoft.com/en-us/library/azure/ad/graph/api/api-catalog). Gestão de dados de utilizador inclui a capacidade de eliminar dados ou exportar dados de registos de auditoria.

[!INCLUDE [gdpr-intro-sentence.md](../../includes/gdpr-intro-sentence.md)]

## <a name="delete-user-data"></a>Eliminar dados de utilizador

Dados de utilizador são armazenados no diretório do Azure AD B2C e nos registos de auditoria. Todos os dados de auditoria do utilizador são mantidas de 30 dias a retenção de dados no Azure AD B2C. Se pretender eliminar dados de utilizador dentro do que 30 dias, pode utilizar o [eliminar um utilizador](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#DeleteUser) operação. Uma operação de eliminação não é necessária para cada um dos inquilinos do Azure AD B2C onde os dados podem residir. 

Todos os utilizadores no Azure AD B2C é atribuído um ID de objeto. O ID de objeto fornece um identificador inequívoca para que possa utilizar para eliminar dados de utilizador no Azure AD B2C.  Consoante a arquitetura, o ID de objeto pode ser um identificador de correlação útil através de outros serviços como o departamento de marketing financeiro, e bases de dados de gestão de relação de cliente.  

É a forma mais exata para obter o ID de objeto para um utilizador para obtê-lo como parte de um journey de autenticação com o Azure AD B2C.  Se é recebido um pedido válido para dados de um utilizador através de outros métodos, um processo offline, por exemplo, uma pesquisa por um suporte ao cliente agente, poderá ser necessário para localizar o utilizador e tenha em atenção o ID de objeto associado. 

O exemplo seguinte mostra um fluxo de eliminação de dados:

1. O utilizador inicia sessão e seleciona **eliminar os meus dados**.
2. A aplicação oferece uma opção para eliminar os dados dentro de uma secção Administração da aplicação.
3. A aplicação força uma autenticação para o Azure AD B2C. O Azure AD B2C fornece um token com o ID de objeto do utilizador para a aplicação. 
4. O token é recebido, a aplicação e o objeto que ID é utilizado para eliminar os dados do utilizador através de uma chamada para o Azure AD Graph API. Azure AD Graph API elimina os dados de utilizador e devolve um código de estado 200 OK.
5. A aplicação orquestra eliminação de dados de utilizador noutros sistemas organizacional, conforme necessário, utilizando o ID de objeto ou outros identificadores.
6. A aplicação confirma que a eliminação de dados e fornece passos para o utilizador.

## <a name="export-customer-data"></a>Exportar dados de cliente

O processo para exportar dados de cliente do Azure AD B2C é semelhante ao processo de eliminação.

Dados de utilizador do Azure AD B2C estão limitados a:

- **Os dados armazenados no Azure Active Directory** -dados podem ser obtidos num journey de utilizador de autenticação do Azure AD B2C com o ID de objeto ou qualquer nome de início de sessão, tais como o e-mail ou nome de utilizador.  
- **Relatório de eventos de auditoria específicas do utilizador** -Data é indexado com o ID de objeto.

No seguinte exemplo de um fluxo de dados de exportação, passos descritos como a ser efetuadas pela aplicação também podem ser efetuados por um processo de back-end ou por um utilizador com uma função de administrador no diretório:

1. O utilizador inicia sessão na aplicação. O Azure AD B2C impõe a autenticação com a autorização multifator se for necessário.
2. A aplicação utiliza as credenciais de utilizador para chamar uma operação de AD Graph API do Azure para obter os atributos de utilizador. O AD Graph API do Azure fornece os dados de atributo no formato JSON. Consoante o esquema, o conteúdo de token de id pode ser definido para incluir todos os dados pessoais sobre um utilizador.
3. A aplicação obtém a atividade de auditoria do utilizador final. Azure AD Graph API fornece os dados do evento para a aplicação.
4. a aplicação agrega os dados e torna a mesma disponível para o utilizador.

## <a name="next-steps"></a>Passos Seguintes

- Saiba como gerir a forma como os utilizadores podem aceder a sua aplicação no [gerir o acesso de utilizador](manage-user-access.md)




