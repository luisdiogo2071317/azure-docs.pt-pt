---
title: "Como efetuar uma prova de posse x. 509 para certificados de AC com o serviço de aprovisionamento de dispositivos do Azure IoT Hub | Microsoft Docs"
description: "Como verificar os certificados da AC de x. 509 com o serviço DPS"
services: iot-dps
keywords: 
author: JimacoMS
ms.author: v-jamebr
ms.date: 02/26/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 07fe5f975e59c10fcd716db6585e2ae0fefc90e4
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2018
---
# <a name="how-to-do-proof-of-possession-for-x509-ca-certificates-with-your-device-provisioning-service"></a>Como efetuar uma prova de posse x. 509 para certificados de AC com o serviço de aprovisionamento de dispositivos

Um verificado x. 509 autoridade de certificação (CA) o certificado é um certificado de AC que foi carregado e registado para o aprovisionamento de serviço e tornou-se através de uma prova de posse com o serviço. 

Prova de posse envolve os seguintes passos:
1. Obtenha um código de verificação exclusivo gerado pelo serviço de aprovisionamento para o certificado de AC de x. 509. Pode fazê-lo a partir do portal do Azure.
2. Criar um certificado de verificação de x. 509 com o código de verificação como o assunto e assinar o certificado com a chave privada associada do seu certificado da AC de x. 509.
3. Carregue o certificado de verificação assinado para o serviço. O serviço valida o certificado de verificação utilizando a seção pública do certificado da AC ser verificado, comprovar assim que estiver na posse da chave privada do certificado de AC.

Certificados verificados desempenham um papel importante quando utilizar grupos de inscrição. Verificação da propriedade do certificado fornece uma camada adicional de segurança, garantindo que /carregador do certificado estiver na posse da chave privada do certificado. Verificação impede que um ator malicioso o tráfego de extrair um certificado intermédio e utilizar esse certificado para criar um grupo de inscrição no seu próprio serviço de aprovisionamento de interceção assumir eficazmente os seus dispositivos. Através de comprovar a propriedade de raiz ou um certificado intermediária na cadeia de certificados, a comprovar que tem permissão para gerar os certificados de folha para os dispositivos que irão registar como parte desse grupo de inscrição. Para este motivo, a raiz ou intermediária configurado num grupo de inscrição tem de ser um certificado verificado ou tem o roll até um certificado na cadeia de certificados um dispositivo verificado apresenta quando autentica com o serviço. Para mais informações sobre grupos de inscrição, consulte o artigo [certificados x. 509](concepts-security.md#x509-certificates) e [controlar o acesso de dispositivo para o serviço de aprovisionamento com certificados x. 509](concepts-security.md#controlling-device-access-to-the-provisioning-service-with-x509-certificates).

## <a name="register-the-public-part-of-an-x509-certificate-and-get-a-verification-code"></a>Registar a parte pública de um certificado x. 509 e obter um código de verificação

Para registar um certificado de AC com o serviço de aprovisionamento e obter um código de verificação que pode utilizar durante a prova de posse, siga estes passos. 

1. No portal do Azure, navegue para o serviço de aprovisionamento e abra **certificados** no menu esquerdo. 
2. Clique em **adicionar** para adicionar um novo certificado.
3. Introduza um nome a apresentar amigável para o seu certificado. Procure o ficheiro. cer ou. pem que representa a parte pública do certificado x. 509. Clique em **Carregar**.
4. Depois de obter uma notificação que o certificado é carregado com êxito, clique em **guardar**.

    ![Carregar certificado](./media/how-to-verify-certificates/add-new-cert.png)  

   O certificado irá apresentar o **Explorador de certificado** lista. Tenha em atenção que o **estado** deste certificado está *Unverified*.

5. Clique no certificado que adicionou no passo anterior.

6. No **detalhes do certificado**, clique em **gerar código de verificação**.

7. O serviço de aprovisionamento cria um **código de verificação** que pode utilizar para validar a propriedade de certificado. Copie o código para a sua área de transferência. 

   ![Verificar o certificado](./media/how-to-verify-certificates/verify-cert.png)  

## <a name="digitally-sign-the-verification-code-to-create-a-verification-certificate"></a>Assinar digitalmente o código de verificação para criar um certificado de verificação

Agora, tem de se inscrever o *código de verificação* com a chave privada associada com o certificado x. 509 AC, o que gera uma assinatura. Isto é conhecido como [prova de posse](https://tools.ietf.org/html/rfc5280#section-3.1) e resultados de um certificado de verificação assinado.

A Microsoft fornece ferramentas e exemplos que podem ajudar a criar um certificado assinado de verificação: 

- O **SDK do Azure IoT Hub C** fornece PowerShell (Windows) e scripts de Bash (Linux) para ajudar a criar certificados de AC e folha para desenvolvimento e efetuar uma prova de posse utilizando um código de verificação. Pode transferir o [ficheiros](https://github.com/Azure/azure-iot-sdk-c/tree/master/tools/CACertificates) relevantes para o sistema para uma pasta de trabalho e siga as instruções no [Leia-me certificados de AC de gerir](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) para efetuar uma prova de posse num certificado de AC. 
- O **Azure IoT Hub c# SDK** contém o [exemplo de verificação de certificado do grupo](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/provisioning/service/samples/GroupCertificateVerificationSample), que pode utilizar para efetuar uma prova de posse.
- Pode seguir os passos a [scripts do PowerShell para gerir os certificados x. 509 assinado para AC](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-security-x509-create-certificates) artigo na documentação do IoT Hub, especificamente o script mencionado na secção intitulada [prova de posse do seu Certificado da AC de x. 509](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-security-x509-create-certificates#signverificationcode).
 
> [!IMPORTANT]
> Para além de efetuar uma prova de posse, os scripts do PowerShell e Bash citou anteriormente também permitem-lhe criar certificados de folha que podem ser utilizados para autenticar e aprovisionar dispositivos, certificados intermediários e certificados de raiz. Estes certificados devem ser utilizados para desenvolvimento apenas. Nunca deve ser utilizadas num ambiente de produção. 

Os scripts do PowerShell e Bash fornecidos na documentação e SDKs dependem [OpenSSL](https://www.openssl.org/). É também possível utilizar OpenSSL ou outras ferramentas de terceiros para o ajudar a fazer uma prova de posse. Para obter mais informações sobre ferramentas fornecida com os SDKs, consulte [como utilizar ferramentas fornecidas nos SDKs](how-to-use-sdk-tools.md). 


## <a name="upload-the-signed-verification-certificate"></a>Carregue o certificado de verificação assinado

1. Carregar a assinatura resultante como um certificado de verificação ao seu serviço no portal de aprovisionamento. No **detalhes do certificado** no portal do Azure, utilize o _Explorador de ficheiros_ ícone junto ao **ficheiro. pem ou. cer de certificado de verificação** campo para carregar assinada certificado de verificação do sistema.

2. Depois do certificado é carregado com êxito, clique em **verifique**. O **estado** das alterações de certificado para  **_Verified_**  no **certificado Explorer** lista. Clique em **atualizar** se não atualizar automaticamente.

   ![Carregar a verificação de certificado](./media/how-to-verify-certificates/upload-cert-verification.png)  

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre como utilizar o portal para criar um grupo de inscrição, consulte [gerir inscrições de dispositivos com o portal do Azure](how-to-manage-enrollments.md).
- Para saber mais sobre como utilizar os SDKs do serviço para criar um grupo de inscrição, consulte [gerir inscrições de dispositivos com o serviço SDKs](how-to-manage-enrollments-sdks.md).










