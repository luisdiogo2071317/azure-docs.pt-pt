---
title: Sobre chaves, os segredos e certificados
description: Descrição geral da REST interface e detalhes de programador KV
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: abd1b743-1d58-413f-afc1-d08ebf93828a
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: 2c7dd89d9c2e5d50f2533101499a6e50e52047b3
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="about-keys-secrets-and-certificates"></a>Sobre certificados, chaves e segredos
O Cofre de chaves do Azure permite aos utilizadores armazenar e utilizar as chaves criptográficas dentro do ambiente do Microsoft Azure. O Cofre de chaves suporta vários tipos de chave e algoritmos e permite a utilização de módulos de segurança de Hardware (HSM) para chaves de valor superior. Além disso, o Cofre de chaves permite aos utilizadores armazenar segredos com segurança. Segredos são objetos de octeto tamanho limitado com nenhuma semântica específico. O Cofre de chaves também suporta certificados, que são criados sobre chaves e segredos e adicione uma funcionalidade de renovação automática.

Para obter informações mais gerais sobre o Cofre de chaves do Azure, consulte [que é o Cofre de chaves do Azure?](/azure/key-vault/key-vault-whatis)

**Detalhes gerais do Cofre de chaves**

-   [As normas de suporte](about-keys-secrets-and-certificates.md#BKMK_Standards)
-   [Tipos de dados](about-keys-secrets-and-certificates.md#BKMK_DataTypes)  
-   [Objetos, identificadores e controlo de versões](about-keys-secrets-and-certificates.md#BKMK_ObjId)  

**Sobre chaves**

-   [As chaves e os tipos de chave](about-keys-secrets-and-certificates.md#BKMK_KeyTypes)  
-   [Algoritmos RSA](about-keys-secrets-and-certificates.md#BKMK_RSAAlgorithms)  
-   [Algoritmos de RSA HSM](about-keys-secrets-and-certificates.md#BKMK_RSA-HSMAlgorithms)  
-   [Proteção de criptografia](about-keys-secrets-and-certificates.md#BKMK_Cryptographic)
-   [Operações de chaves](about-keys-secrets-and-certificates.md#BKMK_KeyOperations)  
-   [Atributos de chave](about-keys-secrets-and-certificates.md#BKMK_KeyAttributes)  
-   [Marcas de chave](about-keys-secrets-and-certificates.md#BKMK_Keytags)  

**Sobre os segredos** 

-   [Trabalhar com segredos](about-keys-secrets-and-certificates.md#BKMK_WorkingWithSecrets)  
-   [Atributos secretos](about-keys-secrets-and-certificates.md#BKMK_SecretAttrs)  
-   [Etiquetas secretas](about-keys-secrets-and-certificates.md#BKMK_SecretTags)  
-   [Controlo de acesso secreta](about-keys-secrets-and-certificates.md#BKMK_SecretAccessControl)  

**Sobre os certificados**

-   [Composição de um certificado](#BKMK_CompositionOfCertificate)  
-   [As etiquetas e atributos de certificado](#BKMK_CertificateAttributesAndTags)  
-   [Política de certificado](#BKMK_CertificatePolicy)  
-   [Emissor do certificado](#BKMK_CertificateIssuer)  
-   [Contactos de certificado](#BKMK_CertificateContacts)  
-   [Controlo de acesso de certificado](#BKMK_CertificateAccessControl)  

--

## <a name="key-vault-general-details"></a>Detalhes gerais do Cofre de chaves

As secções seguintes disponibilizam informações gerais aplicáveis entre a implementação do serviço Cofre de chaves do Azure.

###  <a name="BKMK_Standards"></a> As normas de suporte

O JavaScript Object Notation (JSON) e as especificações de objeto de JavaScript assinatura e encriptação (BLOGUE) são informações importantes em segundo plano.  

-   [Chave de Web JSON (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key)  
-   [Encriptação de Web JSON (JWE)](http://tools.ietf.org/html/draft-ietf-jose-json-web-encryption)  
-   [Algoritmos de Web JSON (JWA)](http://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms)  
-   [Assinatura de Web JSON (JWS)](http://tools.ietf.org/html/draft-ietf-jose-json-web-signature)  

### <a name="BKMK_DataTypes"></a> Tipos de dados

Consulte o [especificações de BLOGUE](#BKMK_Standards) para tipos de dados relevantes para as chaves, encriptação e assinatura.  

-   **algoritmo** -um algoritmo de uma operação de chave, por exemplo, RSA1_5 suportado  
-   **valor de ficheiro de encriptação** -octetos de texto, codificados através de Base64URL de cifras  
-   **o valor resumido** -o resultado de um algoritmo de hash codificado através de Base64URL  
-   **tipo de chave** -um dos tipos chaves suportados, por exemplo RSA.  
-   **valor de texto não encriptado** -octetos de texto simples, codificados através de Base64URL  
-   **valor de assinatura** - saída de um algoritmo de assinatura, codificado através de Base64URL  
-   **base64URL** -um Base64URL [RFC4648] codificado valor binário  
-   **Booleano** -true ou false  
-   **Identidade** – uma identidade do Azure Active Directory (AAD).  
-   **IntDate** - um valor decimal de JSON que representa o número de segundos de 1970-01-01T0:0:0Z UTC até que a data/hora especificada UTC. Consulte [RFC3339] para obter detalhes sobre a data/tempo limite em geral e UTC em especial.  

###  <a name="BKMK_ObjId"></a> Objetos, identificadores e controlo de versões

Objetos armazenados no Cofre de chaves do Azure mantém versões sempre que é criada uma nova instância de um objeto e cada versão tem um identificador exclusivo e o URL. Quando um objeto é criado pela primeira vez, é dado um identificador exclusivo de versão e está marcada como a versão atual do objeto. Criação de uma nova instância com o mesmo nome de objeto fornece o novo objeto de um identificador exclusivo de versão e leva-o para se tornar a versão atual.  

Objetos no Cofre de chaves do Azure podem ser resolvidos utilizando o identificador atual ou um identificador de versão específico. Por exemplo, tendo em conta uma chave com o nome "MasterKey", efetuar operações com o identificador atual faz com que o sistema utilizar a versão mais recente disponível. Efetuar operações com o identificador específico da versão faz com que o sistema utilizar essa versão específica do objeto.  

Objetos são identificados de forma exclusiva no Cofre de chaves do Azure com um URL de forma a que não existem dois objetos no sistema, independentemente de geolocalização, tem o mesmo URL. O URL completo para um objeto é denominado o identificador de objeto e é composta por uma parte de prefixo que identifica o Cofre de chaves, o tipo de objeto, um utilizador forneceu o nome do objeto e uma versão de objeto. O nome do objeto é sensível e imutável. Os identificadores que inclua a versão do objeto são referidos como identificadores de Base.  

Para obter mais informações, consulte [autenticação, os pedidos e respostas](authentication-requests-and-responses.md)

Um identificador de objeto tem o seguinte formato geral:  

`https://{keyvault-name}.vault.azure.net/{object-type}/{object-name}/{object-version}`  

Em que:  

|||  
|-|-|  
|`keyvault-name`|O nome para um cofre de chaves no serviço Cofre de chaves do Microsoft Azure.<br /><br /> Os nomes do Cofre de chaves estão selecionados pelo utilizador e são globalmente exclusivos.<br /><br /> Nome do Cofre de chaves tem de ter uma cadeia 3 e 24 carateres comprimento que contém apenas (0-9, a-z, A-Z e -).|  
|`object-type`|O tipo de objeto, "chaves" ou "segredos".|  
|`object-name`|Um `object-name` é um nome de utilizador fornecido e tem de ser exclusivos dentro de um cofre de chaves. O nome tem de ser uma cadeia de 1-127 carateres de comprimento que contém apenas 0-9, a-z, A-Z e -.|  
|`object-version`|Um `object-version` é um gerada pelo sistema, o identificador de cadeia de 32 carateres, opcionalmente, é utilizado para endereçar uma versão de um objecto exclusiva.|  

## <a name="key-vault-keys"></a>Chaves do Cofre de chaves

###  <a name="BKMK_KeyTypes"></a> As chaves e os tipos de chave

As chaves criptográficas no Cofre de chaves do Azure são representadas como objetos JSON Web chave [JWK]. As especificações de JWK/JWA base também são expandidas para permitir tipos de chave exclusivos para a implementação do Cofre de chaves do Azure, por exemplo, a importação de chaves para o Cofre de chaves do Azure utilizando o empacotamento específico do fornecedor (Thales) de HSM para ativar segura transportes de chaves como esse só pode ser utilizados nos HSMs de Cofre de chaves do Azure.  

A versão inicial do Cofre de chaves do Azure suporta apenas; a chaves RSA em versões futuras podem suportar outros tipos de chaves, como de curva elíptica e simétrica.  

-   **RSA**: uma chave RSA de 2048 bits. Esta é uma chave "de forma recuperável", que é processada no software pelo Cofre de chaves, mas é armazenada encriptados em descanso ao utilizar uma chave de sistema que está a ser um HSM. Os clientes podem importar uma chave RSA existente ou pedir que o Cofre de chaves do Azure gerar um.  
-   **RSA HSM**: chave RSA um que é processada num HSM. Chaves RSA de HSM são protegidas dos Universos de segurança do Azure chave de cofre HSM (há um universo de segurança por geografia para manter o isolamento). Os clientes podem importar uma chave RSA, num formulário de forma recuperável ou ao exportar a partir de um dispositivo compatível de HSM, ou pedir que o Cofre de chaves do Azure gerar um. Este tipo de chave adiciona o atributo de T para o JWK obter para transportar o material de chave de HSM.  

     Para obter mais informações sobre limites geográficas, consulte [Microsoft Azure Trust Center](https://azure.microsoft.com/en-us/support/trust-center/privacy/)  

###  <a name="BKMK_RSAAlgorithms"></a> Algoritmos RSA  
 São suportados os seguintes identificadores de algoritmo com chaves RSA no Cofre de chaves do Azure.  

#### <a name="wrapkeyunwrapkey-encryptdecrypt"></a>WRAPKEY/UNWRAPKEY, DE ENCRIPTAÇÃO/DESENCRIPTAÇÃO

-   **RSA1_5** -encriptação de chaves RSAES-PKCS1-V1_5 [RFC3447]  
-   **RSA OAEP** - RSAES utilizando ideal assimétrico encriptação preenchimento (OAEP) [RFC3447] com os parâmetros de predefinição especificados pelo RFC 3447 na secção A.2.1. Os parâmetros de predefinição estão a utilizar uma função de hash SHA-1 e uma função de geração de máscara de MGF1 com SHA-1.  

#### <a name="signverify"></a>VERIFICAR/INÍCIO DE SESSÃO

-   **RS256** - RSASSA-PKCS-v1_5 utilizar SHA-256. O valor de resumo de aplicações fornecido tem de ser calculado com o SHA-256 e tem de ser 32 bytes de comprimento.  
-   **RS384** - RSASSA-PKCS-v1_5 utilizar SHA-384. O valor de resumo de aplicações fornecido tem de ser calculado utilizando SHA-384 e tem de ser 48 bytes de comprimento.  
-   **RS512** - RSASSA-PKCS-v1_5 utilizar SHA-512. O valor de resumo de aplicações fornecido tem de ser calculado utilizando SHA-512 e tem de ser 64 bytes de comprimento.  
-   **RSNULL** -consulte [RFC2437], um caso de utilização especializado para ativar a determinados cenários TLS.  

###  <a name="BKMK_RSA-HSMAlgorithms"></a> Algoritmos de RSA HSM  
São suportados os seguintes identificadores de algoritmo com chaves RSA HSM no Cofre de chaves do Azure.  

### <a name="BKMK_Cryptographic"></a> Proteção de criptografia

Os módulos criptográficos que utiliza o Cofre de chaves do Azure, se o software, ou de HSM são FIPS validada. Não precisa de fazer nada especiais para executar no modo FIPS. Se lhe **criar** ou **importar** chaves como protegida por HSM, estes são garantidos para ser processado no interior de HSMs validados a certificação FIPS 140-2 de nível 2 ou posterior. Se lhe **criar** ou **importar** chaves como protegida por software, em seguida, são processados no interior de módulos criptográficos validado a certificação FIPS 140-2 de nível 1 ou superior. Para obter mais informações, consulte [chaves e os tipos de chave](about-keys-secrets-and-certificates.md#BKMK_KeyTypes).

#### <a name="wrapunwrap-encryptdecrypt"></a>DESENROLAR/MOLDAGEM, DE ENCRIPTAÇÃO/DESENCRIPTAÇÃO

-   **RSA1_5** -RSAES-PKCS1-V1_5 [RFC3447] encriptação de chaves.  
-   **RSA OAEP** - RSAES utilizando ideal assimétrico encriptação preenchimento (OAEP) [RFC3447] com os parâmetros de predefinição especificados pelo RFC 3447 na secção A.2.1. Os parâmetros de predefinição estão a utilizar uma função de hash SHA-1 e uma função de geração de máscara de MGF1 com SHA-1.  

 #### <a name="signverify"></a>VERIFICAR/INÍCIO DE SESSÃO  

-   **RS256** - RSASSA-PKCS-v1_5 utilizar SHA-256. O valor de resumo de aplicações fornecido tem de ser calculado com o SHA-256 e tem de ser 32 bytes de comprimento.  
-   **RS384** - RSASSA-PKCS-v1_5 utilizar SHA-384. O valor de resumo de aplicações fornecido tem de ser calculado utilizando SHA-384 e tem de ser 48 bytes de comprimento.  
-   **RS512** - RSASSA-PKCS-v1_5 utilizar SHA-512. O valor de resumo de aplicações fornecido tem de ser calculado utilizando SHA-512 e tem de ser 64 bytes de comprimento.  
-   RSNULL: Consulte [RFC2437], um caso de utilização especializado para ativar a determinados cenários TLS.  

###  <a name="BKMK_KeyOperations"></a> Operações de chaves

O Cofre de chaves do Azure suporta as seguintes operações nos objetos da chave:  

-   **Criar**: permite que um cliente criar uma chave no Cofre de chaves do Azure. O valor da chave é gerado pelo Cofre de chaves do Azure e armazenado e não é libertado ao cliente. Assimétrico (e no futuro, curva elíptica e Symmetric) chaves podem ser criadas no Cofre de chaves do Azure.  
-   **Importar**: permite que um cliente importar uma chave existente para o Cofre de chaves do Azure. Assimétrico (e no futuro, curva elíptica e Symmetric) chaves podem ser importadas para o Cofre de chaves do Azure utilizando um número de métodos de empacotamento diferente dentro de uma construção JWK.  
-   **Atualização**: permite que um cliente com permissões suficientes para modificar os metadados (atributos de chave) associados uma chave anteriormente armazenada no Cofre de chaves do Azure.  
-   **Eliminar**: permite que um cliente com permissões suficientes para eliminar uma chave do Cofre de chaves do Azure.  
-   **Lista**: permite que um cliente listar todas as chaves num cofre de chave do Azure fornecida.  
-   **Lista de versões**: permite que um cliente listar todas as versões de uma chave especificada num cofre de chaves do Azure fornecida.  
-   **Obter**: permite que um cliente obter as partes de uma chave especificada num cofre de chaves do Azure públicas.  
-   **Cópia de segurança**: exporta uma chave de um formato protegido.  
-   **Restaurar**: importa uma chave anteriormente cópia de segurança.  

Para obter mais informações, consulte [operações de chaves](/rest/api/keyvault/key-operations.md)  

Quando tiver sido criada uma chave no Cofre de chaves do Azure, as operações criptográficas seguintes podem ser efetuadas utilizando a chave:  

-   **Início de sessão e verificar**: estritamente, esta operação é "hash de início de sessão" ou "verificar o hash" como o Cofre de chaves do Azure não suporta o hash do conteúdo como parte da criação de assinatura. As aplicações devem hash os dados de ser assinados localmente e, em seguida, pedir que o hash de sessão do Cofre de chaves do Azure. Verificação de hashes assinadas é suportada como uma operação de conveniência para aplicações que pode não ter acesso a material de chaves [pública]; é recomendado que, para melhor desempenho de aplicações, certifique-se de operações são efetuadas localmente.  
-   **Chave de encriptação / encapsulamento**: uma chave armazenada num cofre de chaves do Azure pode ser utilizada para proteger a chave de outra, normalmente, uma chave de encriptação de conteúdo simétrica (CEK). Quando a chave no Cofre de chaves do Azure é assimétrica, encriptação de chaves é utilizada, por exemplo RSA OAEP e as operações de WRAPKEY/UNWRAPKEY são equivalentes para ENCRIPTAR/DESENCRIPTAR. Quando a chave no Cofre de chaves do Azure é simétrica, é utilizada a chave de moldagem; Por exemplo, AES KW. A operação de WRAPKEY é suportada para efeitos práticos para aplicações que pode não ter acesso a material de chaves [pública]; Recomenda-se que, para melhor desempenho de aplicações, WRAPKEY operações são efetuadas localmente.  
-   **Encriptar e desencriptar**: uma chave armazenada num cofre de chaves do Azure pode ser utilizada para encriptar ou desencriptar um único bloco de dados, o tamanho dos quais é determinado pelo tipo de chave e o algoritmo de encriptação selecionado. A operação de encriptação é fornecida para efeitos práticos para aplicações que pode não ter acesso a material de chaves [pública]; é recomendado que, para melhor desempenho de aplicações, encriptar operações ser executados localmente.  

Enquanto WRAPKEY/UNWRAPKEY utilizando chaves assimétricas possa parecer supérfluas como a operação é equivalente a encriptação/DESENCRIPTAÇÃO, a utilização de operações distintas é considerada importante fornecer semântica e separação de autorização destas operações e consistência Quando os outros tipos de chaves são suportados pelo serviço.  

O Cofre de chaves do Azure não suporta operações de exportação: assim que uma chave é aprovisionada no sistema não é possível extrair ou a material de chaves modificadas.  No entanto, os utilizadores do Cofre de chaves do Azure que pode exigir a respetiva chave para outros casos de utilização ou depois de ter sido eliminado no Cofre de chaves do Azure, podem utilizar as operações de cópia de segurança e RESTAURO para exportar/importar a chave de um formato protegido. Criados pela operação de cópia de segurança de chaves não são utilizáveis fora do Cofre de chaves do Azure. Em alternativa, a operação de importação pode ser utilizada em várias instâncias do Cofre de chaves do Azure.  

Os utilizadores podem restringir qualquer uma das operações criptográficas que suporta o Cofre de chaves do Azure numa base por chave utilizando a propriedade key_ops do objeto JWK.  

Para obter mais informações sobre objetos JWK, consulte [JSON Web chave (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key).  

###  <a name="BKMK_KeyAttributes"></a> Atributos de chave

Para além de material de chaves, os seguintes atributos podem ser especificados. Um pedido do JSON, a palavra-chave de atributos e chavetas, ' {' '}', são necessários, mesmo se existem não existem atributos especificados.  

- *ativado*: predefinição booleana, opcional, é **verdadeiro**. Especifica se a chave está ativado e utilizável para operações de criptografia. O *ativada* atributo é utilizado em conjunto com *nbf* e *exp*. Quando uma operação ocorre entre *nbf* e *exp*, só será possível permitida se *ativada* está definido como **verdadeiro**. Operações fora de *nbf* / *exp* janela não são automaticamente permitidos, exceto para determinados tipos de operação em [condições específicas](about-keys-secrets-and-certificates.md#BKMK_key-date-time-ctrld-ops).
- *NBF*: predefinido opcional, IntDate, está agora. O *nbf* (não antes) atributo identifica o tempo antes que a chave não podem ser utilizada para operações de criptografia, exceto para determinados tipos de operação em [condições específicas](about-keys-secrets-and-certificates.md#BKMK_key-date-time-ctrld-ops). O processamento do *nbf* atributo requer que a data/hora atual tem de ser após ou igual de não-antes de data/hora listada no *nbf* atributo. O Cofre de chaves do Azure pode fornecer para algumas leeway pequeno, normalmente, não mais do que alguns minutos, para contemplar a relógio dissimetrias. O valor tem de ser um número que contém um valor de IntDate.  
- *EXP*: predefinido opcional, IntDate, é "indefinidamente". O *exp* atributo (hora de expiração) identifica a hora de expiração nesta ou após o qual a chave não pode ser utilizada para operação criptográfica, exceto para determinados tipos de operação em [condições específicas](about-keys-secrets-and-certificates.md#BKMK_key-date-time-ctrld-ops). O processamento do *exp* atributo requer que a data/hora atual tem de ser antes da data/hora de expiração indicada no *exp* atributo. O Cofre de chaves do Azure pode fornecer para algumas leeway pequeno, normalmente, não mais do que alguns minutos, para contemplar a relógio dissimetrias. O valor tem de ser um número que contém um valor de IntDate.  

Existem só de leitura os atributos adicionais que estão incluídos em qualquer resposta que inclui os atributos de chave:  

- *criado*: IntDate, opcional. O *criado* atributo indica que esta versão da chave foi criada. Este valor é nulo para chaves criadas antes da adição deste atributo. O valor tem de ser um número que contém um valor de IntDate.  
- *atualizado*: IntDate, opcional. O *atualizado* atributo indica quando esta versão da chave foi atualizada. Este valor é nulo para as chaves que foram atualizadas pela última vez antes da adição deste atributo. O valor tem de ser um número que contém um valor de IntDate.  

Para obter mais informações sobre IntDate e outros tipos de dados, consulte [tipos de dados](about-keys-secrets-and-certificates.md#BKMK_DataTypes)  

#### <a name="BKMK_key-date-time-ctrld-ops"></a> Operações de controlado de data / hora

As chaves não ainda válido e expiradas, esses fora do *nbf* / *exp* janela, irá funcionar para **desencriptar**, **desenrolar** e **verificar** operações (não devolver 403, proibido). A lógica por detrás para utilizar o estado não ainda válido é permitir que uma chave a ser testada antes de utilização em produção. A lógica por detrás para utilizar o estado expirado é permitir que as operações de recuperação de dados que foi criado quando a chave era válida. Além disso, pode desativar o acesso a uma chave através de políticas do Cofre de chaves ou atualizando o *ativada* atributo chave para **falso**.

Para obter mais informações sobre dados de tipos, consulte [tipos de dados](about-keys-secrets-and-certificates.md#BKMK_DataTypes).

Para obter mais informações sobre outros atributos possíveis, consulte o [JSON Web chave (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key).

### <a name="BKMK_Keytags"></a> Marcas de chave

Pode especificar os metadados específicos da aplicação adicionais sob a forma de etiquetas. O Cofre de chaves do Azure suporta até 15 etiquetas, cada um dos quais pode ter um nome de 256 carateres e um valor de 256 carateres.  

>[!Note]
>As etiquetas são legíveis por um emissor se tiverem o *lista* ou *obter* permissão para este tipo de objeto; chaves, os segredos ou certificados.

###  <a name="BKMK_KeyAccessControl"></a> Controlo de acesso de chave

Controlo de acesso para chaves geridas pelo Cofre de chaves é fornecido no nível de um cofre de chaves que age como o contentor de chaves. Há uma política de controlo de acesso para as chaves que é distinta da política de controlo de acesso para segredos no Cofre de chave do mesmo. Os utilizadores podem criar um ou mais cofres para armazenar chaves e são necessários para manter a segmentação adequado do cenário e a gestão de chaves. Controlo de acesso para chaves é independente do controlo de acesso para segredos.  

As seguintes permissões podem ser concedidas, num por utilizador / serviço principal base, na entrada de controlo de acesso de chaves no cofre. Estas permissões rigorosamente espelhar as operações permitidas num objeto de chave:  

-   *criar*: criar novas chaves
-   *obter*: a parte pública de uma chave com os respetivos atributos de leitura
-   *lista*: lista de chaves ou versões de uma chave armazenada num cofre de chaves
-   *Atualizar*: atualizar os atributos para uma chave
-   *eliminar*: eliminar o objeto de chave
-   *início de sessão*: Utilize a chave para assinar digests
-   *Certifique-se*: utilizar a chave para verificar digests
-   *wrapKey*: utilizar a chave para proteger uma chave simétrica
-   *unwrapKey*: Utilize a chave ao desproteger encapsulada chaves simétricas
-   *encriptar*: utilizar a chave para proteger uma sequência de bytes arbitrária
-   *desencriptar*: utilizar a chave para desproteger uma sequência de bytes
-   *importar*: importar uma chave para um cofre de chaves
-   *cópia de segurança*: cópia de segurança de uma chave de um cofre de chaves
-   *Restaurar*: restaure uma cópia de segurança da chave para um cofre de chaves
-   *todos os*: todas as permissões

## <a name="key-vault-secrets"></a>Segredos do Cofre de chaves 

###  <a name="BKMK_WorkingWithSecrets"></a> Trabalhar com segredos

Segredos no Cofre de chaves do Azure são sequências octeto com um tamanho máximo de 25 KBytes cada. O serviço Cofre de chaves do Azure não fornece qualquer semântica para segredos; -apenas aceita os dados, encripta e armazena-os, devolvendo um identificador SECRETO, "id", que pode ser utilizado para obter o segredo numa altura posterior.  

Para dados altamente confidenciais, os clientes devem considerar camadas adicionais de proteção de dados que são armazenados no Cofre de chaves do Azure; Por exemplo através da pré-encriptação de dados através de uma chave de proteção separados.  

O Cofre de chaves do Azure suporta também um campo contentType para segredos. Os clientes podem especificar o tipo de conteúdo, "contentType" de um segredo para ajudar a interpretar os dados secretos quando é obtido. O comprimento máximo deste campo é de 255 carateres. Não existirem valores predefinidos. A utilização de sugeridos for como uma indicação para interpretar os dados secretos. Por exemplo, uma implementação pode armazenar certificados e palavras-passe como segredos, em seguida, utilize este campo para indicar que. Não existirem valores predefinidos.  

###  <a name="BKMK_SecretAttrs"></a> Atributos secretos

Para além de dados secretos, é possível especificar os seguintes atributos:  

- *EXP*: predefinição opcional, IntDate, é **indefinidamente**. O *exp* atributo (hora de expiração) identifica a hora de expiração nesta ou após o qual os dados secretos não pode ser obtido, exceto no [situações específicas](about-keys-secrets-and-certificates.md#BKMK_secret-date-time-ctrld-ops). O processamento do *exp* atributo requer que a data/hora atual tem de ser antes da data/hora de expiração indicada no *exp* atributo. O Cofre de chaves do Azure pode fornecer para algumas leeway pequeno, normalmente, não mais do que alguns minutos, para contemplar a relógio dissimetrias. O valor tem de ser um número que contém um valor de IntDate.  
- *NBF*: predefinição opcional, IntDate, é **agora**. O *nbf* (não antes) atributo identifica o tempo antes do qual os segredo tem não possível recuperar os dados, exceto no [situações específicas](about-keys-secrets-and-certificates.md#BKMK_secret-date-time-ctrld-ops). O processamento do *nbf* atributo requer que a data/hora atual tem de ser após ou igual de não-antes de data/hora listada no *nbf* atributo. O Cofre de chaves do Azure pode fornecer para algumas leeway pequeno, normalmente, não mais do que alguns minutos, para contemplar a relógio dissimetrias. O valor tem de ser um número que contém um valor de IntDate.  
- *ativado*: predefinição booleana, opcional, é **verdadeiro**. Este atributo especifica ou não é possível obter os dados secretos. O atributo enabled é utilizado em conjunto com e *exp* quando uma operação ocorre entre e exp, será apenas possível permitida se estiver ativada está definido como **verdadeiro**. Operações fora de *nbf* e *exp* janela são automaticamente não permitido, exceto na [situações específicas](about-keys-secrets-and-certificates.md#BKMK_secret-date-time-ctrld-ops).  

Existem só de leitura os atributos adicionais que estão incluídos em qualquer resposta que inclui secretos atributos:  

- *criado*: IntDate, opcional. O atributo criado indica que esta versão do segredo do foi criada. Este valor é nulo para segredos criados antes da adição deste atributo. O valor tem de ser um número que contém um valor de IntDate.  
- *atualizado*: IntDate, opcional. O atributo atualizado indica quando esta versão do segredo do foi atualizada. Este valor é nulo para os segredos que foram atualizados pela última vez antes da adição deste atributo. O valor tem de ser um número que contém um valor de IntDate.

#### <a name="BKMK_secret-date-time-ctrld-ops"></a> Operações de controlado de data / hora

Um segredo **obter** operação irá funcionar para segredos expirados e não ainda válidas, fora do *nbf* / *exp* janela. Um segredo ao chamar **obter** operação, para um segredo não ainda válido, pode ser utilizada para fins de teste. A obter (**obter**utilize) um segredo expirado, podem ser utilizados para operações de recuperação.

Para obter mais informações sobre dados de tipos, consulte [tipos de dados](about-keys-secrets-and-certificates.md#BKMK_DataTypes).  

###  <a name="BKMK_SecretAccessControl"></a> Controlo de acesso secreta

Controlo de acesso para segredos geridos no Cofre de chaves do Azure é fornecido no nível de um cofre de chaves que age como o contentor desses segredos. Há uma política de controlo de acesso para os segredos que é distinta da política de controlo de acesso para chaves no Cofre de chave do mesmo. Os utilizadores podem criar um ou mais cofres para armazenar segredos e são necessários para manter a segmentação adequado do cenário e a gestão de segredos. Controlos de acesso para segredos são independentes do controlo de acesso para as chaves.  

As seguintes permissões podem ser utilizadas numa base por principal, na entrada de controlo de acesso de segredos no cofre e rigorosamente espelhar as operações permitidas num objeto secreto:  

-   *definir*: Criar novo segredos  
-   *obter*: ler um segredo  
-   *lista*: lista de versões do segredo armazenados no Cofre de chaves ou segredos  
-   *eliminar*: eliminar o segredo  
-   *todos os*: todas as permissões  

Para obter mais informações sobre como trabalhar com segredos, consulte [operações de segredo](/rest/api/keyvault/secret-operations.md).  

###  <a name="BKMK_SecretTags"></a> Etiquetas secretas  
Pode especificar os metadados específicos da aplicação adicionais sob a forma de etiquetas. O Cofre de chaves do Azure suporta até 15 etiquetas, cada um dos quais pode ter um nome de 256 carateres e um valor de 256 carateres.  

>[!Note]
>As etiquetas são legíveis por um emissor se tiverem o *lista* ou *obter* permissão para este tipo de objeto; chaves, os segredos ou certificados.

## <a name="key-vault-certificates"></a>Certificados de Cofre de chaves

Fornece suporte de certificados do Cofre de chaves para a gestão das suas x509 certificados e os comportamentos seguintes:  

-   Permite que um proprietário de certificado criar um certificado através de um processo de criação do Cofre de chaves ou através da importação de um certificado existente. Isto inclui ambos autoassinados e autoridade de certificação gerados certificados.
-   Permite que um proprietário de certificado do Cofre de chaves implementar o armazenamento seguro e a gestão de X509 certificados sem interação com o material de chave privada.  
-   Permite que um proprietário de certificado criar uma política que direciona o Cofre de chaves para gerir o ciclo de vida de um certificado.  
-   Permite que os proprietários de certificado fornecer informações de contacto para notificação sobre eventos de ciclo de vida de expiração e renovação de certificado.  
-   Suporta a renovação automática com emissores selecionados - parceiro do Cofre de chaves X509 fornecedores de certificado / autoridades de certificados.

>[!Note]
>Não-parcerias fornecedores/autoridades também são permitidas no entanto, não suporta a funcionalidade de renovação automática.

###  <a name="BKMK_CompositionOfCertificate"></a> Composição de um certificado

Quando é criado um certificado do Cofre de chaves, uma chave endereçável e o segredo também são criados com o mesmo nome. A chave de Cofre de chaves permite operações de chaves e o segredo do Cofre de chaves permite a obtenção do valor do certificado como um segredo. Um certificado do Cofre de chaves também contém x509 pública metadados de certificado.  

O identificador e a versão de certificados é semelhante de chaves e segredos. Uma versão específica de uma chave endereçável e o segredo criado com a versão de certificado do Cofre de chaves está disponível a resposta de certificado do Cofre de chaves.
 
![Cetificates são os objetos complexos](media/azure-key-vault.png)

###  <a name="BKMK_CertificateExportableOrNonExportableKey"></a> Chave exportável ou não exportável

Quando é criado um certificado do Cofre de chaves, pode ser obtida do segredo do endereçável com a chave privada no formato PFX ou PEM se a política utilizada para criar o certificado indicado que a chave é exportável. Se a política utilizada para criar o certificado do Cofre de chaves indicado a chave de ser não exportável, em seguida, a chave privada não é uma parte do valor quando obtidos como um segredo.  

A chave endereçável torna-se mais relevante com certificados de KV não exportável. Operações da chave KV endereçável estão mapeadas de *keyusage* campo da política de certificado KV utilizada para criar o certificado de KV.  

São suportados dois tipos de chave – *RSA* ou *RSA HSM* com certificados. Exportável só é permitida com RSA, não é suportado por HSM de RSA.  

###  <a name="BKMK_CertificateAttributesAndTags"></a> As etiquetas e atributos de certificado

Para além dos metadados do certificado, uma chave de endereçável e um segredo endereçável, um certificado do Cofre de chaves também contém as etiquetas e atributos.  

#### <a name="attributes"></a>Atributos

Os atributos de certificado são espelhados nos atributos de chave endereçável e segredo criado quando KV certificado é criado.  

Um certificado do Cofre de chaves tem os seguintes atributos:  

-   *ativado*: predefinição booleana, opcional, é **verdadeiro**. Este atributo pode ser especificado para indicar se os dados de certificados podem ser obtidos como operável como uma chave ou segredo. Isto é utilizado em conjunto com *nbf* e*exp*quando uma operação ocorre entre *nbf* e exp, será apenas possível permitida se estiver ativada está definido como true. Operações fora de *nbf* e*exp*não janela são automaticamente permitidos.  

Existem só de leitura os atributos adicionais que estão incluídos na resposta:

-   *criado*: IntDate: indica quando esta versão do certificado foi criado.  
-   *atualizado*: IntDate: indica quando esta versão do certificado foi atualizada.  
-   *EXP*: IntDate: contém o valor da data de expiração do x509 certificado.  
-   *NBF*: IntDate: contém o valor da data de x509 certificado.  

> [!Note] 
> Se um certificado do Cofre de chaves expirar, é endereçável chave e o segredo tornar-se inoperáveis.  

#### <a name="tags"></a>Etiquetas

 Cliente especificado dicionário de pares de valor de chave, semelhantes para as etiquetas na chaves e segredos.  

 > [!Note]
> As etiquetas são legíveis por um emissor se tiverem o *lista* ou *obter* permissão para este tipo de objeto; chaves, os segredos ou certificados.

###  <a name="BKMK_CertificatePolicy"></a> Política de certificado

Uma política de certificado contém informações sobre como criar e gerir o ciclo de vida de um certificado de KV. Quando um certificado com chave privada é importado para o Cofre de chaves, uma política predefinida é criada através da leitura de x509 certificado.  

Quando um certificado de KV é criado a partir do zero, uma política tem de ser fornecido ao Cofre de chaves para informá-lo sobre como criar esta versão de certificado KV ou a versão de certificado KV seguinte. Quando uma política for estabelecida, não é necessário com sucessivas criar operações criar versões de certificado KV seguintes.  

Não há apenas uma instância de uma política para todas as versões de um certificado de KV.  

Um nível elevado, uma política de certificado contém o seguinte:  

-   Propriedades do certificado X509: contém nome do requerente, os nomes alternativos do requerente etc. utilizado para criar um x509 pedido de certificado.  
-   Propriedades da chaves: contém o tipo de chave, exportável, comprimento de chave e reutilizar os campos de chave. Estes campos instruir sobre como gerar uma chave do Cofre de chaves.  
-   Propriedades secretas: contém propriedades secretas, tais como o tipo de conteúdo do segredo endereçável para gerar o valor secreto, para obter o certificado como um segredo.  
-   Ações de duração: contém ações de duração do certificado KV. Cada ação duração contém:  

     - Acionador: especificado através de dias antes da expiração ou duração percentagem span  

     - Ação: especificação de tipo de ação – *emailContacts* ou *autoRenew*  

-   Emissor: Parâmetros sobre o emissor de certificado a utilizar para emitir x509 certificados.  
-   Atributos de política: contém atributos associados à política  

#### <a name="x509-to-key-vault-usage-mapping"></a>X509 para mapeamento de utilização do Cofre de chaves

A tabela seguinte representa o mapeamento de x509 política de utilização de chave para operações de chaves eficazes de uma chave criada como parte da criação do certificado de um cofre de chaves.

|**X509 sinalizadores de utilização de chave**|**Ops chaves do Cofre de chaves**|**Comportamento predefinido**|
|----------|--------|--------|
|DataEncipherment|encriptar, desencriptar| N/A |
|DecipherOnly|Desencriptar| N/A  |
|Bits DigitalSignature|iniciar sessão, certifique-se| Predefinição de Cofre de chaves sem uma especificação de utilização no momento de criação do certificado | 
|EncipherOnly|encrypt| N/A |
|KeyCertSign|iniciar sessão, certifique-se|N/A|
|KeyEncipherment|wrapKey, unwrapKey| Predefinição de Cofre de chaves sem uma especificação de utilização no momento de criação do certificado | 
|Não rejeição|iniciar sessão, certifique-se| N/A |
|crlsign|iniciar sessão, certifique-se| N/A |

###  <a name="BKMK_CertificateIssuer"></a> Emissor do certificado

Um objeto de certificado do Cofre de chaves contém uma configuração utilizada para comunicar com um fornecedor de emissor do certificado selecionado para certificados de ordem x509.  

-   O Cofre de chaves parceiros com os seguintes fornecedores de emissor de certificados para certificados SSL

|**Nome do fornecedor**|**Localizações**|
|----------|--------|
|DigiCert|Suportado em todas as localizações de serviço Cofre de chaves na nuvem pública e o Azure Government|
|GlobalSign|Suportado em todas as localizações de serviço Cofre de chaves na nuvem pública e o Azure Government|

Antes de um emissor do certificado pode ser criado de um cofre de chaves, pré-requisitos passos 1 e 2 devem ser efetuados com êxito.  

1. Carregar para fornecedores de autoridade (AC) de certificados  

    -   Um administrador da organização tem uma empresa (ex. Contoso) pelo menos um fornecedor de AC.  

2. O administrador cria as credenciais de autor do pedido para o Cofre de chaves para se inscrever (e renovar) certificados SSL  

    -   Fornece a configuração a ser utilizado para criar um objeto de emissor do fornecedor no Cofre de chaves  

Para obter mais informações sobre como criar objetos de emissor a partir do portal de certificados, consulte o [blogue de certificados do Cofre de chave](http://aka.ms/kvcertsblog)  

O Cofre de chaves permite a criação de vários objetos do emissor com a configuração do fornecedor de emissor diferentes. Assim que for criado um objeto de emissor, o nome pode ser referenciado numa ou várias políticas de certificado. Referencia o objeto de emissor dá instruções ao Cofre de chaves para utilizar a configuração conforme especificado no objeto de emissor quando pedir o x509 certificado do fornecedor de AC durante a criação do certificado e a renovação.  

Objetos de emissor são criados no cofre e só podem ser utilizados com certificados KV no cofre do mesmo.  

###  <a name="BKMK_CertificateContacts"></a> Contactos de certificado

Contactos de certificado contêm informações de contacto para enviar notificações acionadas pelos eventos de duração do certificado. As informações de contactos são partilhadas por todos os certificados no Cofre de chaves. É enviada uma notificação para todos os contactos especificados para um evento para qualquer certificado no Cofre de chaves.  

Se a política de um certificado está definida como auto-renovação, em seguida, é enviada uma notificação nos seguintes eventos.  

-   Antes de renovação de certificado
-   Após a renovação de certificado, a indicar se o certificado foi renovado com êxito ou se Ocorreu um erro, a necessidade de renovação manual do certificado.  

 Se a política de um certificado está configurada para ser manualmente renovado (apenas ao e-mail), em seguida, uma notificação é enviada quando for altura para renovar o certificado.  

###  <a name="BKMK_CertificateAccessControl"></a> Controlo de acesso de certificado

 Controlo de acesso para certificados é gerido pelo Cofre de chaves e é fornecido no nível de um cofre de chaves que age como o contentor desses certificados. Há uma política de controlo de acesso para os certificados que é distinta da política de controlo de acesso para chaves e segredos no Cofre de chave do mesmo. Os utilizadores podem criar um ou mais cofres para armazenar certificados e são necessários para manter a segmentação adequado do cenário e a gestão de certificados.  

 As seguintes permissões podem ser utilizadas numa base por principal, na entrada de controlo de acesso de segredos num cofre de chaves e rigorosamente espelhos as operações permitidos num objeto secreto:  

-   *obter*: permite obter a versão atual do certificado ou de qualquer versão de um certificado 
-   *lista*: permite que a lista de certificados atuais ou versões de um certificado  
-   *eliminar*: permite que a eliminação de um certificado, a política e todas as respetivas versões  
-   *criar*: permite criar um certificado do Cofre de chaves.  
-   *importar*: permite que a importação de material de certificado para um certificado do Cofre de chave.  
-   *Atualizar*: permite que a atualização de um certificado.  
-   *manageconnects*: permite a gestão dos contactos de certificado do Cofre de chaves  
-   *getissuers*: permite get de emissores de um certificado  
-   *listissuers*: permite que a lista de emissores de certificados  
-   *setissuers*: permite criar ou atualizar o Cofre de chaves de emissores de certificados  
-   *deleteissuers*: permite eliminar Cofre de chaves de emissores de certificados  
-   *todos os*: concede todas as permissões  

## <a name="additional-information-for-certificates"></a>Informações adicionais de certificados

- [Certificados e políticas](/rest/api/keyvault/certificates-and-policies.md)
- [Emissores de certificados](/rest/api/keyvault/certificate-issuers.md)
- [Contactos de certificado](/rest/api/keyvault/certificate-contacts.md)

## <a name="see-also"></a>Consultar Também

- [Autenticação, os pedidos e respostas](authentication-requests-and-responses.md)
- [Versões do Cofre de chaves](key-vault-versions.md)
- [Guia para programadores do Cofre de chaves](/azure/key-vault/key-vault-developers-guide)
