---
title: Gerir inscrições de dispositivos utilizando SDKs Service de aprovisionamento do dispositivo do Azure | Microsoft Docs
description: Como gerir inscrições de dispositivos no IoT Hub dispositivo aprovisionamento de serviço com os SDKs de serviço
author: yzhong94
ms.author: yizhon
ms.date: 04/04/18
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: arjmands
ms.openlocfilehash: 9a68d928c70e1e233f6de7df13441a1f688f456a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629852"
---
# <a name="how-to-manage-device-enrollments-with-azure-device-provisioning-service-sdks"></a>Como gerir inscrições de dispositivos com SDKs Service de aprovisionamento do dispositivo do Azure
A *inscrição de dispositivos* cria um registo de um único dispositivo ou um grupo de dispositivos que poderão, a determinada altura registar com o serviço de aprovisionamento de dispositivos. O registo de inscrição contém a configuração pretendida inicial para os dispositivos como parte do que a inscrição, incluindo o hub IoT pretendido. Este artigo mostra como gerir inscrições de dispositivos para o seu serviço aprovisionamento através da programação com os SDKs IoT do Azure aprovisionamento de serviço.  Os SDKs estão disponíveis no GitHub no mesmo repositório como SDKs IoT do Azure.

## <a name="prerequisites"></a>Pré-requisitos
* Obter a cadeia de ligação da sua instância do serviço de aprovisionamento de dispositivos.
* Obter o dispositivo artefactos de segurança para o [mecanismo de atestado](concepts-security.md#attestation-mechanism) utilizado:
    * [**Trusted Platform Module (TPM)**](/azure/iot-dps/concepts-security#trusted-platform-module):
        * Inscrição individuais: ID de registo e a chave de endossamento TPM de um dispositivo físico ou de simulador de TPM.
        * Grupo de inscrição não é aplicável a atestado TPM.
    * [**X.509**](/azure/iot-dps/concepts-security):
        * Inscrição individuais: O [certificado folha](/azure/iot-dps/concepts-security#leaf-certificate) do dispositivo físico ou do SDK [REPARTIR](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) emulador.
        * Grupo de inscrição: O [certificado de AC/root](/azure/iot-dps/concepts-security#root-certificate) ou [intermediária](/azure/iot-dps/concepts-security#intermediate-certificate), utilizada para produzir o certificado de dispositivo num dispositivo físico.  Também pode ser gerado pelo emulador REPARTIR SDK.
* Chamadas de API exatas poderão ser diferentes devido a diferenças de idioma. Reveja os exemplos fornecidos no GitHub para obter mais detalhes:
   * [Exemplos de cliente do serviço de aprovisionamento de Java](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-samples)
   * [Exemplos de aprovisionamento de cliente do serviço do node.js](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service/samples)
   * [Exemplos de cliente do serviço de aprovisionamento de .NET](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/provisioning/service/samples)

## <a name="create-a-device-enrollment"></a>Criar uma inscrição de dispositivos
Existem duas formas de poder inscrever os seus dispositivos com o serviço de aprovisionamento:

* Um **grupo inscrição** é uma entrada para um grupo de dispositivos que partilham um mecanismo de atestado comuns dos certificados x. 509, assinado pelo [certificado de raiz](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) ou [intermediária ](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate). Recomendamos que utilize um grupo de inscrição para um grande número de dispositivos que partilham uma configuração desejada inicial ou para dispositivos que todos os mesmo inquilino. Tenha em atenção que só podem inscrever dispositivos que utilizam o mecanismo de atestado de x. 509 como *grupos inscrição*. 

    Pode criar um grupo de inscrição com os SDKs seguir este fluxo de trabalho:

    1. Para o grupo de inscrição, o mecanismo de atestado utiliza o certificado de raiz de x. 509.  Chamar a API do SDK do serviço ```X509Attestation.createFromRootCertificate``` com o certificado de raiz para criar atestado para inscrição.  O certificado de raiz de x. 509 é fornecido num ficheiro PEM ou como uma cadeia.
    1. Crie um novo ```EnrollmentGroup``` variável utilizando o ```attestation``` criado e um único ```enrollmentGroupId```.  Opcionalmente, pode definir parâmetros, como ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    2. Chamar a API do SDK do serviço ```createOrUpdateEnrollmentGroup``` na sua aplicação de back-end com ```EnrollmentGroup``` para criar um grupo de inscrição.

* Um **inscrição individuais** é uma entrada para um único dispositivo pode registar. As inscrições individuais podem utilizar certificados x. 509 ou tokens SAS (a partir de um TPM físico ou virtual) como mecanismos de atestado. Recomendamos que utilize inscrições individuais para os dispositivos que requerem configurações iniciais exclusivas ou para dispositivos que só podem utilizar os tokens SAS através do TPM ou virtual TPM como o mecanismo de atestado. As inscrições individuais podem ter o ID de dispositivo do hub IoT pretendido especificado.

    Pode criar um registo individual com os SDKs seguir este fluxo de trabalho:
    
    1. Escolha o ```attestation``` mecanismo, o que pode ser TPM ou os x. 509.
        1. **TPM**: utilizar a chave de endossamento a partir de um dispositivo físico ou de TPM simulador como entrada, pode chamar a API do serviço SDK ```TpmAttestation``` criar atestado para inscrição. 
        2. **X. 509**: utilizar o certificado de cliente como a entrada, pode chamar a API do serviço SDK ```X509Attestation.createFromClientCertificate``` criar atestado para inscrição.
    2. Crie um novo ```IndividualEnrollment``` variável com a utilização de ```attestation``` criado e um único ```registrationId``` como entrada, o que está no seu dispositivo ou gerado a partir do simulador do TPM.  Opcionalmente, pode definir parâmetros, como ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    3. Chamar a API do SDK do serviço ```createOrUpdateIndividualEnrollment``` na sua aplicação de back-end com ```IndividualEnrollment``` para criar um registo individual.

Depois de ter criado com êxito de inscrição, o serviço de aprovisionamento de dispositivos devolve um resultado de inscrição. Este fluxo de trabalho é demonstrado nos exemplos [mencionado anteriormente](#prerequisites).

## <a name="update-an-enrollment-entry"></a>Atualizar uma entrada de inscrição

Depois de criar uma entrada de inscrição, poderá pretender atualizar a inscrição.  Potenciais cenários incluem a atualização da propriedade pretendida, ao atualizar o método de atestado ou revogar o acesso do dispositivo.  Existem diferentes APIs para a inscrição individuais e inscrição de grupo, mas sem distinção para o mecanismo de atestado.

Pode atualizar uma entrada de inscrição, seguindo este fluxo de trabalho:
* **Inscrição individual**:
    1. Obter a inscrição mais recente primeiro com o serviço de API do SDK do serviço de aprovisionamento ```getIndividualEnrollment```.
    2. Modifique o parâmetro da mais recente inscrição conforme necessário. 
    3. Utilizar a inscrição mais recente, chamar a API do SDK do serviço ```createOrUpdateIndividualEnrollment``` para atualizar a sua entrada de inscrição.
* **Inscrição de grupo**:
    1. Obter a inscrição mais recente primeiro com o serviço de API do SDK do serviço de aprovisionamento ```getEnrollmentGroup```.
    2. Modifique o parâmetro da mais recente inscrição conforme necessário.
    3. Utilizar a inscrição mais recente, chamar a API do SDK do serviço ```createOrUpdateEnrollmentGroup``` para atualizar a sua entrada de inscrição.

Este fluxo de trabalho é demonstrado nos exemplos [mencionado anteriormente](#prerequisites).

## <a name="remove-an-enrollment-entry"></a>Remover uma entrada de inscrição

* **Inscrição individuais** pode ser eliminado ao chamar a API do SDK do serviço ```deleteIndividualEnrollment``` utilizando ```registrationId```.
* **Inscrição de grupo** pode ser eliminado ao chamar a API do SDK do serviço ```deleteEnrollmentGroup``` utilizando ```enrollmentGroupId```.

Este fluxo de trabalho é demonstrado nos exemplos [mencionado anteriormente](#prerequisites).

## <a name="bulk-operation-on-individual-enrollments"></a>Operação em massa no inscrições individuais

Pode efetuar a operação em massa para criar, atualizar ou remover vários inscrições individuais de seguir este fluxo de trabalho:

1. Criar uma variável que contém vários ```IndividualEnrollment```.  Implementação desta variável é diferente para cada idioma.  Reveja o exemplo de operação em massa no GitHub para obter mais detalhes.
2. Chamar a API do SDK do serviço ```runBulkOperation``` com um ```BulkOperationMode``` para da operação desejada e a variável para inscrições individuais. São suportados quatro modos: criar, atualizar, updateIfMatchEtag e eliminar.

Após ter efetuado com êxito uma operação, o serviço de aprovisionamento de dispositivos iria devolver um resultado de operação em massa.

Este fluxo de trabalho é demonstrado nos exemplos [mencionado anteriormente](#prerequisites).
