---
title: Configurar a autenticação mútua de TLS - serviço de aplicações do Azure
description: Saiba como configurar a sua aplicação web para utilizar a autenticação de certificado de cliente no TLS.
services: app-service
documentationcenter: ''
author: naziml
manager: erikre
editor: jimbe
ms.assetid: cd1d15d3-2d9e-4502-9f11-a306dac4453a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2016
ms.author: naziml
ms.custom: seodec18
ms.openlocfilehash: f08e8f60f0e23cce9546e45dcf7b249d38224736
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2018
ms.locfileid: "53252886"
---
# <a name="how-to-configure-tls-mutual-authentication-for-web-app"></a>Como Configurar a Autenticação Mútua de TLS para a Aplicação Web
## <a name="overview"></a>Descrição geral
Pode restringir o acesso à sua aplicação web do Azure, permitindo que diferentes tipos de autenticação para o mesmo. É uma forma de fazê-lo autenticar com um certificado de cliente quando a solicitação é sobre TLS/SSL. Esse mecanismo é chamado de autenticação mútua de TLS ou autenticação e este artigo apresenta em pormenor como configurar a sua aplicação web para utilizar a autenticação de certificado de cliente de certificado de cliente.

> **Nota:** Se acessar seu site por HTTP e HTTPS não, não receberá qualquer certificado de cliente. Portanto, se seu aplicativo exigir certificados de cliente que não deve permitir pedidos à sua aplicação através de HTTP.
> 
> 

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="configure-web-app-for-client-certificate-authentication"></a>Configurar aplicação Web para autenticação de certificados de cliente
Para configurar a aplicação web para exigir certificados de cliente, terá de adicionar a definição de site clientCertEnabled para a sua aplicação web e defini-lo como true. Esta definição também é possível configurar no portal do Azure, no painel de certificados de SSL.

Pode utilizar o [ARMClient ferramenta](https://github.com/projectkudu/ARMClient) torna mais fácil criar a chamada à REST API. Depois de iniciar sessão com a ferramenta, precisará emitir o comando seguinte:

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose

Substituir tudo no {} com informações para a web app e a criação de um arquivo chamado enableclientcert.json com o JSON seguinte conteúdo:

    {
        "location": "My Web App Location",
        "properties": {
            "clientCertEnabled": true
        }
    }

Certifique-se alterar o valor de "local" para onde quer que a sua aplicação web está localizada por exemplo, e.u.a. Centro-Norte ou oeste dos EUA etc.

Também pode utilizar https://resources.azure.com inverter os `clientCertEnabled` propriedade `true`.

> **Nota:** Se executar o ARMClient a partir do Powershell, terá de escape o \@ símbolo para o ficheiro JSON com uma escala de back-".
> 
> 

## <a name="accessing-the-client-certificate-from-your-web-app"></a>Acessando o certificado de cliente a partir da sua aplicação Web
Se estiver usando o ASP.NET e configurar a sua aplicação para utilizar a autenticação de certificado de cliente, o certificado vai estar disponível através da **HttpRequest.ClientCertificate** propriedade. Para outras pilhas de aplicação, o certificado de cliente estarão disponível na sua aplicação através de um valor de codificação base64 no cabeçalho de pedido "X-ARR-ClientCert". Seu aplicativo pode criar um certificado a partir deste valor e, em seguida, utilizá-la para fins de autenticação e autorização em seu aplicativo.

## <a name="special-considerations-for-certificate-validation"></a>Considerações especiais para validação do certificado
O certificado de cliente que é enviado para o aplicativo não passa pela nenhuma validação da plataforma de aplicações Web do Azure. A validar este certificado é da responsabilidade da aplicação web. Aqui está o código ASP.NET de exemplo que valida as propriedades do certificado para fins de autenticação.

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;

                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;

                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want to test if the certificate chains to a Trusted Root Authority you can uncomment the code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }
