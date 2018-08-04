---
title: Autenticação no Data Lake Store com o Azure Active Directory | Documentos da Microsoft
description: Saiba como autenticar com o Data Lake Store com o Azure Active Directory
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 7db48b03a6f34655917c82702c12dbc9bc4c987a
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494182"
---
# <a name="authentication-with-data-lake-store-using-azure-active-directory"></a>Autenticação com o Data Lake Store com o Azure Active Directory

Azure Data Lake Store utiliza o Azure Active Directory para autenticação. Antes de criar um aplicativo que funciona com o Azure Data Lake Store, deve decidir como autenticar a aplicação no Azure Active Directory (Azure AD).

## <a name="authentication-options"></a>Opções de autenticação

* **Autenticação de utilizador final** -credenciais do Azure de um utilizador final são utilizadas para autenticar com o Store de Lake de dados. Solicita a estas credenciais de utilizador da aplicação criada para funcionar com o Data Lake Store. Como resultado, esse mecanismo de autenticação é *interativo* e a aplicação é executada no contexto do utilizador com sessão iniciada. Para obter mais informações e instruções, consulte [autenticação de utilizador final para o Data Lake Store](data-lake-store-end-user-authenticate-using-active-directory.md).

* **Autenticação serviço a serviço** -Utilize esta opção se pretender que um aplicativo para se autenticar no Data Lake Store. Nesses casos, criar uma aplicação do Azure Active Directory (AD) e utilize a chave da aplicação do Azure AD para autenticar no Data Lake Store. Como resultado, esse mecanismo de autenticação é *não interativa*. Para obter mais informações e instruções, consulte [autenticação de serviço a serviço para o Data Lake Store](data-lake-store-service-to-service-authenticate-using-active-directory.md).

A tabela a seguir ilustra como o utilizador final e mecanismos de autenticação de serviço para serviço são suportados para o Data Lake Store. Aqui está como ler a tabela.

* O símbolo de ✔ * indica que a opção de autenticação é suportada e liga a um artigo que demonstra como utilizar a opção de autenticação. 
* O símbolo ✔ indica que a opção de autenticação é suportada. 
* As células vazias indicam que a opção de autenticação não é suportada.


|Utilize esta opção de autenticação com...                   |.NET         |Java     |PowerShell |CLI 2.0 | Python   |REST     |
|:---------------------------------------------|:------------|:--------|:----------|:-------------|:---------|:--------|
|Utilizador final (sem MFA * *)                        |   ✔ |    ✔    |    ✔      |       ✔      |    **[✔ *](data-lake-store-end-user-authenticate-python.md#end-user-authentication-without-multi-factor-authentication)**(preterido)     |    **[✔*](data-lake-store-end-user-authenticate-rest-api.md)**    |
|Utilizador final (com a MFA)                           |    **[✔*](data-lake-store-end-user-authenticate-net-sdk.md)**        |    **[✔*](data-lake-store-end-user-authenticate-java-sdk.md)**     |    ✔      |       **[✔*](data-lake-store-get-started-cli-2.0.md)**      |    **[✔*](data-lake-store-end-user-authenticate-python.md#end-user-authentication-with-multi-factor-authentication)**     |    ✔    |
|Para serviços (utilizando a chave de cliente)         |    **[✔*](data-lake-store-service-to-service-authenticate-net-sdk.md#service-to-service-authentication-with-client-secret)** |    **[✔*](data-lake-store-service-to-service-authenticate-java.md)**    |    ✔      |       ✔      |    **[✔*](data-lake-store-service-to-service-authenticate-python.md#service-to-service-authentication-with-client-secret-for-account-management)**     |    **[✔*](data-lake-store-service-to-service-authenticate-rest-api.md)**    |
|Para serviços (usando o certificado de cliente) |    **[✔*](data-lake-store-service-to-service-authenticate-net-sdk.md#service-to-service-authentication-with-certificate)**        |    ✔    |    ✔      |       ✔      |    ✔     |    ✔    |

<i>* Clique a <b>✔\* </b> símbolo. É uma ligação.</i><br>
<i>* * MFA significa para a autenticação multifator</i>

Ver [cenários de autenticação do Azure Active Directory](../active-directory/develop/authentication-scenarios.md) para obter mais informações sobre como utilizar o Azure Active Directory para autenticação.

## <a name="next-steps"></a>Passos Seguintes

* [Autenticação de utilizador final](data-lake-store-end-user-authenticate-using-active-directory.md)
* [Autenticação serviço a serviço](data-lake-store-service-to-service-authenticate-using-active-directory.md)


