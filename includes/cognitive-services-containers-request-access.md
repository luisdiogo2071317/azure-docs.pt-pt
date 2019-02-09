---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/24/2019
ms.openlocfilehash: d5a6692aceedb6dc7fad7a434a433882cdbea015
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984910"
---
Primeiro tem de concluir e submeter o [formulário de pedido de contentores de imagem digitalizada dos serviços cognitivos](https://aka.ms/VisionContainersPreview) para pedir acesso ao contentor. O formulário solicita as informações sobre si, sua empresa e o cenário de utilizador que vai utilizar o contentor. Depois de submeter, a equipe de serviços cognitivos do Azure analisa o formulário para se certificar de que cumpre os critérios para acesso ao registo de contentor privado.

> [!IMPORTANT]
> Tem de utilizar um endereço de e-mail associado à conta de (Azure AD) de uma conta Microsoft (MSA) ou o Azure Active Directory no formulário.

Se o pedido for aprovado, em seguida, receber um e-mail com instruções que descrevem como obter as suas credenciais e aceder ao registo de contentor privado.

## <a name="log-in-to-the-private-container-registry"></a>Inicie sessão particular registo de contentor

Existem várias formas de autenticar com o registo de contentor privado para contentores de serviços cognitivos, mas o método recomendado na linha de comando é utilizando o [CLI do Docker](https://docs.docker.com/engine/reference/commandline/cli/).

Utilize o [início de sessão do docker](https://docs.docker.com/engine/reference/commandline/login/) comando, conforme mostrado no exemplo a seguir, para iniciar sessão no `containerpreview.azurecr.io`, o registo de contentor privado para contentores de serviços cognitivos. Substitua *\<nome de utilizador\>* com o nome de utilizador e *\<palavra-passe\>* com a palavra-passe que forneceu as credenciais que recebeu do Azure Equipa de serviços cognitiva.

```
docker login containerpreview.azurecr.io -u <username> -p <password>
```

Se proteger as suas credenciais de um arquivo de texto, poderá concatenar os conteúdos de texto de ficheiros, com o `cat` comando, para o `docker login` comando conforme mostrado no exemplo a seguir. Substitua *\<passwordFile\>* com o caminho e nome do ficheiro de texto que contém a palavra-passe e *\<username\>* com o nome de utilizador fornecido as credenciais.

```
cat <passwordFile> | docker login containerpreview.azurecr.io -u <username> --password-stdin
```

