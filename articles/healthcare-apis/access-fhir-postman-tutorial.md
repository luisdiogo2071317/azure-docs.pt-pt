---
title: Servidor FHIR postman no tutorial do Azure – API do Azure para FHIR
description: Este artigo descreve como aceder a uma API de FHIR com o Postman.
services: healthcare-apis
ms.service: healthcare-apis
ms.topic: tutorial
ms.reviewer: dseven
ms.author: mihansen
author: hansenms
ms.date: 02/07/2019
ms.openlocfilehash: 7009fa175e19ad484c83ec3a988d6c33b67fefcd
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823442"
---
# <a name="tutorial-access-fhir-api-with-postman"></a>Tutorial: Aceder à API de FHIR com o Postman

Uma aplicação cliente teria de aceder a uma API de FHIR por meio de um [REST API](https://www.hl7.org/fhir/http.html). Também poderá interagir diretamente com o servidor FHIR à medida que cria aplicativos, por exemplo, para fins de depuração. Neste tutorial, vamos descrever as etapas necessárias para usar [Postman](https://www.getpostman.com/) para aceder a um servidor FHIR. Postman é uma ferramenta frequentemente utilizada para depuração durante a criação de aplicativos que acessem a APIs.

## <a name="prerequisites"></a>Pré-requisitos

- Um ponto de extremidade FHIR no Azure. Pode configurar essa opção usando a API gerida do Azure para FHIR ou o servidor de Open FHIR de origem para o Azure. Configurar a API do Azure geridos para utilizar o FHIR [portal do Azure](fhir-paas-portal-quickstart.md), [PowerShell](fhir-paas-powershell-quickstart.md), ou [da CLI do Azure](fhir-paas-cli-quickstart.md).
- Postman instalado. Pode obtê-lo de [https://www.getpostman.com](https://www.getpostman.com)

## <a name="fhir-server-and-authentication-details"></a>Detalhes do servidor e autenticação FHIR

Para utilizar o Postman, terá de saber os detalhes seguintes:

- Sua FHIR URL do servidor, por exemplo, `https://MYFHIRSERVICE.azurewebsites.net` ou `https://MYACCOUNT.azurehealthcareapis.com`
- O fornecedor de identidade `Authority` para o seu servidor FHIR, por exemplo, `https://login.microsoftonline.com/{TENANT-ID}`
- Configurada `audience`. Para a API do Azure para FHIR trata `https://azurehealthcareapis.com` e para o servidor FHIR de código-fonte aberto para o Azure, seria definida na [registo de aplicação de recurso do Azure AD](register-resource-azure-ad-client-app.md).
- O `client_id` (ou ID da aplicação) da [aplicação cliente](register-confidential-azure-ad-client-app.md) que irá utilizar para aceder ao serviço FHIR.
- O `client_secret` (ou o segredo de aplicação) da aplicação cliente.

Por fim, deve verificar que `https://www.getpostman.com/oauth2/callback` é um URL de resposta registado para a sua aplicação de cliente.

## <a name="connect-to-fhir-server"></a>Ligar ao servidor FHIR

Utilizar o Postman, fazer uma `GET` pedido para `https://fhir-server-url/metadata`:

![Declaração de capacidade de metadados do postman](media/tutorial-postman/postman-metadata.png)

Neste exemplo, é o URL do servidor FHIR `https://fhirdocsmsft.azurewebsites.net` e a instrução de capacidade do servidor está disponível em `https://fhirdocsmsft.azurewebsites.net/metadata`. Ponto de extremidade deve ser acessível, sem autenticação.

Se tentar aceder aos recursos restritos, deve, em seguida, uma resposta "Falha na autenticação de":

![Falha na autenticação](media/tutorial-postman/postman-authentication-failed.png)

## <a name="obtaining-an-access-token"></a>Obter um token de acesso

Para obter um token de acesso válido, selecione "Autorização" e escolha o tipo "OAuth 2.0":

![Definir o OAuth 2.0](media/tutorial-postman/postman-select-oauth2.png)

Acertos "obter novo Token de acesso" e uma caixa de diálogo é exibida:

![Pedir novo Token de acesso](media/tutorial-postman/postman-request-token.png)

Precisará de alguns detalhes:
| Campo                 | Valor de Exemplo                                                                                                   | Comentário                    |
|-----------------------|-----------------------------------------------------------------------------------------------------------------|----------------------------|
| Nome do token            | MYTOKEN                                                                                                         | Um nome que escolher          |
| Tipo de Concessão            | Código de autorização                                                                                              |                            |
| Url de chamada de retorno          | `https://www.getpostman.com/oauth2/callback`                                                                      |                            |
| Auth URL              | `https://login.microsoftonline.com/{TENANT-ID}/oauth2/authorize?resource=<audience>` | `audience` é `https://azurehealthcareapis.com` para a API do Azure para FHIR e `https://MYFHIRSERVICE.azurewebsites.net` para o servidor de OSS FHIR |
| URL de Token de acesso      | `https://login.microsoftonline.com/{TENANT ID}/oauth2/token`                                                      |                            |
| ID de Cliente             | `XXXXXXXX-XXX-XXXX-XXXX-XXXXXXXXXXXX`                                                                            | ID da aplicação             |
| Segredo do Cliente         | `XXXXXXXX`                                                                                                        | Chave secreta do cliente          |
| Estado                 | `1234`                                                                                                            |                            |
| Autenticação de Cliente | Enviar credenciais de cliente no corpo                                                                                 |                 

Acertos "pedir Token" e vai ser orientado ao longo o fluxo de autenticação do Active Directory do Azure e um token será retornado para o Postman. Caso se depare com problemas, abra a consola do Postman (de item de menu "Visualização -> Mostrar Postman consola").

Desloque para baixo na tela token devolvida e clique em "Utilizar Token":

![Utilizar Token](media/tutorial-postman/postman-use-token.png)

O token deve agora ser preenchido no campo "Token de acesso" e pode selecionar tokens a partir de "Tokens disponíveis". Se "Enviar" novamente para repetir a `Patient` pesquisa de recursos, deve obter um estado `200 OK`:

![200 OK](media/tutorial-postman/postman-200-OK.png)

Neste caso, não há nenhum pacientes na base de dados e a pesquisa está vazia.

Se inspecionar o token de acesso com uma ferramenta como [ https://jwt.ms ](https://jwt.ms), deverá ver o conteúdo, como:

```json
{
  "aud": "https://azurehealthcareapis.com",
  "iss": "https://sts.windows.net/{TENANT-ID}/",
  "iat": 1545343803,
  "nbf": 1545343803,
  "exp": 1545347703,
  "acr": "1",
  "aio": "AUQAu/8JXXXXXXXXXdQxcxn1eis459j70Kf9DwcUjlKY3I2G/9aOnSbw==",
  "amr": [
    "pwd"
  ],
  "appid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "oid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "appidacr": "1",

  ...// Truncated
}
```

Na solução de problemas de situações, a validar que tem o público-alvo correto (`aud` afirmação) é um bom lugar para começar. A API gerida do Azure para utilizações FHIR [IDs de objeto de identidade](find-identity-object-ids.md) para restringir o acesso ao serviço. Certifique-se de que `oid` afirmação do token contém um ID de objeto da lista de IDs de objeto permitidos.

## <a name="inserting-a-patient"></a>Inserir um paciente

Agora que tem um token de acesso válido. Pode inserir um novo paciente. Mude para o método "POST" e adicione o seguinte documento JSON no corpo do pedido:

[!code-json[](samples/sample-patient.json)]

Atingir o "Enviar", e verá que o paciente é criado com êxito:

![Paciente criado](media/tutorial-postman/postman-patient-created.png)

Se repetir a pesquisa dos pacientes, deverá ver o registo dos pacientes:

![Paciente criado](media/tutorial-postman/postman-patient-found.png)

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, que tenha acedido uma API de FHIR com o postman. Leia sobre as funcionalidades de API suportadas no servidor de Microsoft FHIR para o Azure.
 
>[!div class="nextstepaction"]
>[Funcionalidades FHIR suportadas](fhir-features-supported.md)