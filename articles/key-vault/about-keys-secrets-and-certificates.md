---
title: Sobre Chaves, Segredos e Certificados
description: Descrição geral da REST interface e detalhes do programador KV
services: key-vault
documentationcenter: ''
author: BryanLa
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: abd1b743-1d58-413f-afc1-d08ebf93828a
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2018
ms.author: bryanla
ms.openlocfilehash: f36e0e3ddc605d960ed764252308cbf09578832c
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43126147"
---
# <a name="about-keys-secrets-and-certificates"></a>Sobre chaves, segredos e certificados
O Azure Key Vault permite aos utilizadores armazenar e utilizar chaves criptográficas dentro do ambiente do Microsoft Azure. Key Vault suporta vários tipos de chave e algoritmos e permite a utilização de módulos de segurança de Hardware (HSM) para chaves de valor elevado. Além disso, o Key Vault permite aos utilizadores armazenar segredos em segurança. Os segredos são objetos de octeto de tamanho limitado com nenhuma semântica específica. Key Vault também oferece suporte a certificados, que são criados sobre chaves e segredos e adicionar uma funcionalidade de renovação automática.

Para obter mais informações sobre o Azure Key Vault, consulte [o que é o Azure Key Vault?](/azure/key-vault/key-vault-whatis)

**Detalhes gerais do Key Vault**

-   [Oferecer suporte aos padrões](#BKMK_Standards)
-   [Tipos de dados](#BKMK_DataTypes)  
-   [Objetos, identificadores e controle de versão](#BKMK_ObjId)  

**Sobre chaves**

-   [As chaves e os tipos de chaves](#BKMK_KeyTypes)  
-   [Algoritmos RSA](#BKMK_RSAAlgorithms)  
-   [Algoritmos de HSM de RSA](#BKMK_RSA-HSMAlgorithms)  
-   [Proteção criptográfica](#BKMK_Cryptographic)
-   [Operações de chaves](#BKMK_KeyOperations)  
-   [Atributos principais](#BKMK_KeyAttributes)  
-   [Principais etiquetas](#BKMK_Keytags)  

**Sobre segredos** 

-   [Trabalhar com segredos](#BKMK_WorkingWithSecrets)  
-   [Atributos secretos](#BKMK_SecretAttrs)  
-   [Etiquetas secretas](#BKMK_SecretTags)  
-   [Controlo de acesso a segredos](#BKMK_SecretAccessControl)  

**Sobre os certificados**

-   [Composição de um certificado](#BKMK_CompositionOfCertificate)  
-   [Atributos de certificado e as etiquetas](#BKMK_CertificateAttributesAndTags)  
-   [Política de certificado](#BKMK_CertificatePolicy)  
-   [Emissor do certificado](#BKMK_CertificateIssuer)  
-   [Contactos do certificado](#BKMK_CertificateContacts)  
-   [Controlo de acesso de certificado](#BKMK_CertificateAccessControl)  

--

## <a name="key-vault-general-details"></a>Detalhes gerais do Key Vault

As seções a seguir oferecem informações gerais aplicável em toda a implementação do serviço Azure Key Vault.

###  <a name="BKMK_Standards"></a> Oferecer suporte aos padrões

O JavaScript Object Notation (JSON) e especificações de assinatura de objeto de JavaScript e a encriptação (JOSE) são informações gerais importantes.  

-   [Chave do JSON Web (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key)  
-   [Encriptação de Web JSON (JWE)](http://tools.ietf.org/html/draft-ietf-jose-json-web-encryption)  
-   [Algoritmos de Web JSON (JWA)](http://tools.ietf.org/html/draft-ietf-jose-json-web-algorithms)  
-   [Assinatura de Web JSON (JWS)](http://tools.ietf.org/html/draft-ietf-jose-json-web-signature)  

### <a name="BKMK_DataTypes"></a> tipos de dados

Consulte a [especificações de JOSE](#BKMK_Standards) para tipos de dados relevantes para chaves de encriptação e assinatura.  

-   **algoritmo** -um algoritmo suportado para uma operação de chave, por exemplo, RSA1_5  
-   **valor de texto cifrado** -cipher octetos de texto, codificados usando Base64URL  
-   **valor de texto implícita** -a saída de um algoritmo de hash, codificado usando Base64URL  
-   **tipo de chave** -um dos tipos de chave suportados, por exemplo RSA.  
-   **valor de texto não criptografado** -octetos de texto sem formatação, codificados usando Base64URL  
-   **valor de assinatura** - saída de um algoritmo de assinatura, codificado usando Base64URL  
-   **base64URL** -um Base64URL [RFC4648] codificado valor binário  
-   **Booleano** -true ou false  
-   **Identidade** – um identidade do Azure Active Directory (AAD).  
-   **IntDate** – um valor decimal de JSON que representa o número de segundos de 1970-01-01T0:0:0Z UTC até a data/hora especificada UTC. Veja [RFC3339] para obter detalhes sobre a data/horas em geral e a UTC em particular.  

###  <a name="BKMK_ObjId"></a> Objetos, identificadores e controle de versão

Objetos armazenados no Azure Key Vault mantém versões sempre que uma nova instância de um objeto é criada e cada versão tem um identificador exclusivo e um URL. Quando um objeto é criado, ele é fornecido um identificador de versão exclusivo e está marcado como a versão atual do objeto. Criação de uma nova instância com o mesmo nome de objeto fornece o novo objeto de um identificador de versão exclusivo e faz com que ele se tornar a versão atual.  

Objetos no Azure Key Vault podem ser resolvidos utilizando o identificador atual ou um identificador de versão específico. Por exemplo, dada uma chave com o nome "MasterKey", realizar operações com o identificador atual faz com que o sistema para utilizar a versão mais recente disponível. Realizar operações com o identificador de versão específico, faz com que o sistema para utilizar essa versão específica do objeto.  

Objetos são identificados exclusivamente no Azure Key Vault através de um URL que não existem dois objetos no sistema, independentemente da localização geográfica, tem o mesmo URL. O URL completo para um objeto é denominado o identificador de objeto e consiste numa parte de prefixo que identifica o Cofre de chaves, o tipo de objeto, um utilizador forneceu o nome do objeto e uma versão de objeto. O nome do objeto é imutável e maiúsculas de minúsculas. Identificadores de que não incluem a versão de objeto são referidas como identificadores de Base.  

Para obter mais informações, consulte [autenticação, os pedidos e respostas](authentication-requests-and-responses.md)

Um identificador de objeto tem o seguinte formato geral:  

`https://{keyvault-name}.vault.azure.net/{object-type}/{object-name}/{object-version}`  

Em que:  

|||  
|-|-|  
|`keyvault-name`|O nome para um cofre de chaves no serviço do Microsoft Azure Key Vault.<br /><br /> Nomes de Cofre de chaves são selecionados pelo usuário e são globalmente exclusivos.<br /><br /> O nome do Cofre de Chaves tem de ser uma cadeia com 3 a 24 carateres de comprimento que contenha apenas (0-9, a-z, A-Z e -).|  
|`object-type`|O tipo de objeto, "chaves" ou "segredos".|  
|`object-name`|Um `object-name` é um nome de utilizador fornecido para e tem de ser exclusivo dentro de um cofre de chaves. O nome tem de ser uma cadeia de caracteres 1 127 carateres de comprimento e conter apenas 0-9, a-z, A-Z e -.|  
|`object-version`|Um `object-version` é gerada pelo sistema, identificador de cadeia de caracteres de 32, opcionalmente, é usado para tratar de uma versão exclusiva de um objeto.|  

## <a name="key-vault-keys"></a>Chaves do Cofre de chaves

###  <a name="BKMK_KeyTypes"></a> As chaves e os tipos de chaves

As chaves criptográficas no Azure Key Vault são representadas como objetos de chave do JSON Web [JWK]. As especificações de JWK/JWA bases também são expandidas para permitir os tipos de chave de exclusivos para a implementação do Azure Key Vault, por exemplo, a importação de chaves para o Azure Key Vault com o empacotamento específico do fornecedor (Thales) de HSM para ativar o transporte seguro de chaves, esse só pode ser utilizados nos HSMs do Azure Key Vault.  

- **As chaves "Soft"**: uma chave processados no software pelo Key Vault, mas é encriptada em descanso ao utilizar uma chave de sistema que está num HSM. Os clientes podem importar uma chave RSA ou EC existente ou solicitar que o Azure Key Vault gerar um.
- **Chaves de "Disco rígidas"**: processados de uma chave num HSM (módulo de Hardware de segurança). Estas chaves são protegidas em um dos mundos de segurança de HSM de Cofre de chave de Azure (há um universo de segurança por geografia para manter o isolamento). Os clientes podem importar uma chave RSA ou EC, na forma de forma recuperável ou ao exportar a partir de um dispositivo HSM compatível, ou peça que o Azure Key Vault gerar um. Este tipo de chave adiciona o atributo de T para o JWK obter para transportar o material de chave HSM.

     Para obter mais informações sobre dos limites geográficos, consulte [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/privacy/)  

O Azure Key Vault suporta chaves RSA e criptografia de curva elíptica apenas; versões futuras podem suportar, tal como outros tipos de chave simétrica.

-   **EC**: chave de criptografia de curva elíptica "Soft".
-   **EC HSM**: chave de criptografia de curva elíptica "Rígido".
-   **RSA**: chave RSA "Soft".
-   **RSA HSM**: chave RSA "Fixas".

O Azure Key Vault suporta chaves RSA de 2048, 3072 e 4096 de tamanhos e as chaves de criptografia de curva elíptica de tipo p-256, p-384, p-521 e P-256_K.

### <a name="BKMK_Cryptographic"></a> Proteção criptográfica

Os módulos criptográficos que utiliza o Azure Key Vault, se o HSM ou software, são FIPS validada. Não precisa de fazer nada de especial para executar no modo FIPS. Se **crie** ou **importar** chaves como protegida por HSM, é garantido que ser processado dentro de HSMs validados FIPS 140-2 nível 2 ou superior. Se **crie** ou **importar** chaves como protegida por software, em seguida, são processados no interior de módulos criptográficos validados FIPS 140-2 de nível 1 ou superior. Para obter mais informações, consulte [chaves e os tipos de chave](#BKMK_KeyTypes).

###  <a name="BKMK_ECAlgorithms"></a> Algoritmos EC
 Os identificadores de algoritmo seguintes são suportados com chaves EC e EC HSM no Azure Key Vault. 

#### <a name="signverify"></a>VERIFICAR/INÍCIO DE SESSÃO

-   **ES256** - digests ECDSA para SHA-256 e chaves criadas com a curva p-256. Esse algoritmo é descrito em [RFC7518].
-   **ES256K** - digests ECDSA para SHA-256 e chaves criadas com curva P-256_K. Esse algoritmo está pendente de padronização.
-   **ES384** - digests ECDSA para SHA-384 e chaves criadas com a curva p-384. Esse algoritmo é descrito em [RFC7518].
-   **ES512** - digests ECDSA para SHA-512 e chaves criadas com a curva p-521. Esse algoritmo é descrito em [RFC7518].

###  <a name="BKMK_RSAAlgorithms"></a> Algoritmos RSA  
 Os identificadores de algoritmo seguintes são suportados com chaves RSA e RSA HSM no Azure Key Vault.  

#### <a name="wrapkeyunwrapkey-encryptdecrypt"></a>WRAPKEY/UNWRAPKEY, ENCRIPTAÇÃO/DESENCRIPTAÇÃO.

-   **RSA1_5** -encriptação de chave RSAES-PKCS1-V1_5 [RFC3447]  
-   **RSA OAEP** - RSAES usando ideal assimétrica encriptação de preenchimento (OAEP) [RFC3447], com os parâmetros de padrão especificados pelo RFC 3447 na secção A.2.1. Esses parâmetros de predefinição estão a utilizar uma função de hash de SHA-1 e uma função de geração de máscara de MGF1 com SHA-1.  

#### <a name="signverify"></a>VERIFICAR/INÍCIO DE SESSÃO

-   **RS256** - RSASSA-PKCS-v1_5 com SHA-256. O valor de texto implícita de aplicação fornecido deve ser calculado utilizando SHA-256 e tem de ser 32 bytes de comprimento.  
-   **RS384** - RSASSA-PKCS-v1_5, SHA-384 a utilizar. O valor de texto implícita de aplicação fornecido deve ser calculado utilizando SHA-384 e tem de ser 48 bytes de comprimento.  
-   **RS512** - RSASSA-PKCS-v1_5 usando SHA-512. O valor de texto implícita de aplicação fornecido deve ser calculado utilizando SHA-512 e tem de ser 64 bytes de comprimento.  
-   **RSNULL** -veja [RFC2437], um caso de utilização especializado para permitir que determinados cenários TLS.  

###  <a name="BKMK_KeyOperations"></a> Operações de chaves

O Azure Key Vault suporta as seguintes operações nos objetos da chave:  

-   **Criar**: permite que um cliente criar uma chave no Cofre de chaves do Azure. O valor da chave é gerado pelo Azure Key Vault e armazenado e não é liberada para o cliente. Assimétrica (e no futuro, criptografia de curva elíptica e Symmetric) as chaves podem ser criadas no Azure Key Vault.  
-   **Importar**: permite que um cliente importar uma chave existente para o Azure Key Vault. Assimétrica (e no futuro, criptografia de curva elíptica e Symmetric) as chaves podem ser importadas para o Azure Key Vault com uma série de métodos de empacotamento diferentes dentro de uma construção JWK.  
-   **Atualização**: permite que um cliente com permissões suficientes para modificar os metadados (atributos de chaves) associados uma chave armazenada anteriormente no Azure Key Vault.  
-   **Eliminar**: permite que um cliente com permissões suficientes para eliminar uma chave do Cofre de chaves do Azure.  
-   **Lista**: permite que um cliente listar todas as chaves no Azure Key Vault determinado.  
-   **Lista de versões**: permite que um cliente listar todas as versões de uma determinada chave no Azure Key Vault determinado.  
-   **Obter**: permite que um cliente obter as partes públicas de uma determinada chave num Azure Key Vault.  
-   **Cópia de segurança**: exporta uma chave num formato protegido.  
-   **Restaurar**: importa uma chave anteriormente cópia de segurança.  

Para obter mais informações, consulte [operações na referência de API de REST do Cofre de chaves da chave](/rest/api/keyvault).  

Quando tiver sido criada uma chave no Cofre de chaves do Azure, as seguintes operações de criptografia podem ser realizadas usando a chave:  

-   **Início de sessão e verifique se**: estritamente, esta operação é "hash de início de sessão" ou "verificar o hash" como o Azure Key Vault não suporta a hash de conteúdo como parte da criação de assinatura. Aplicativos devem discutir os dados para ser iniciada localmente e, em seguida, solicitar esse início de sessão do Azure Key Vault o hash. Verificação de hashes assinados é suportada como uma operação de conveniência para as aplicações que pode não ter acesso ao material essencial [pública]; nós recomendamos que, para melhor desempenho de aplicações, certifique-se de que as operações são executadas localmente.  
-   **Encriptação de chave / encapsulamento**: uma chave armazenada no Azure Key Vault pode ser utilizada para proteger a outra chave, normalmente, uma chave de encriptação de conteúdo simétrica (CEK). Quando a chave no Azure Key Vault é assimétrica, criptografia de chave é utilizada, por exemplo RSA OAEP e as operações de WRAPKEY/UNWRAPKEY são equivalentes a encriptação/DESENCRIPTAÇÃO. Quando a chave no Azure Key Vault é simétrica, é utilizada a chave de encapsulamento de aplicações; Por exemplo, AES KW. A operação de WRAPKEY é suportada como uma conveniência para as aplicações que pode não ter acesso ao material essencial [pública]; Recomenda-se que, para melhor desempenho de aplicações, WRAPKEY operações são executadas localmente.  
-   **Encriptar e desencriptar**: uma chave armazenada no Azure Key Vault pode ser utilizada para encriptar ou desencriptar um único bloco de dados, o tamanho dos quais é determinado pelo tipo de chave e algoritmo de encriptação selecionado. A operação de encriptação é fornecida para sua comodidade, para aplicações que pode não ter acesso ao material essencial [pública]; é recomendado que, para melhor desempenho de aplicações, encriptar operações ser efetuada localmente.  

Embora pareça supérflua WRAPKEY/UNWRAPKEY com chaves assimétricas como a operação é equivalente a encriptação/DESENCRIPTAÇÃO, a utilização de operações distintas é considerada importante para fornecer semântica e a separação de autorização destas operações e consistência Quando os outros tipos de chave são suportados pelo serviço.  

O Azure Key Vault não suporta operações de exportação: assim que uma chave é aprovisionada no sistema não é possível extrair ou modificado de seu material de chave.  No entanto, os utilizadores do Azure Key Vault, que possam necessitar das chaves para outros casos de utilização ou assim que tiver sido eliminada no Azure Key Vault, podem utilizar as operações de cópia de segurança e RESTAURO para exportar/importar a chave num formato protegido. Criados pela operação de cópia de segurança de chaves não são utilizáveis fora do Azure Key Vault. Em alternativa, a operação de importação pode ser utilizada em várias instâncias do Azure Key Vault.  

Os utilizadores podem restringir qualquer uma das operações criptográficas que suporte o Azure Key Vault numa base por chave usando a propriedade key_ops do objeto JWK.  

Para obter mais informações sobre objetos JWK, consulte [chave de Web JSON (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key).  

###  <a name="BKMK_KeyAttributes"></a> Atributos principais

Para além do material de chave, podem ser especificados os seguintes atributos. Num pedido do JSON, a palavra-chave de atributos e chavetas, ' {' '}', são necessários, mesmo se não houver nenhum atributo especificado.  

- *ativada*: booleana, opcional, a predefinição é **true**. Especifica se a chave é ativado e pode ser utilizada para operações criptográficas. O *habilitado* atributo é utilizado em conjunto com *nbf* e *exp*. Quando uma operação ocorre entre *nbf* e *exp*, apenas irá ser permitida se *ativada* está definido como **verdadeiro**. Operações de fora a *nbf* / *exp* janela não são automaticamente permitidas, exceto para determinados tipos de operação em [condições particulares](#BKMK_key-date-time-ctrld-ops).
- *NBF*: predefinido opcional, de IntDate, está agora. O *nbf* (não antes) atributo identifica o tempo antes do qual a chave não pode ser utilizada para operações de criptografia, exceto para determinados tipos de operação sob [condições particulares](#BKMK_key-date-time-ctrld-ops). O processamento do *nbf* atributo requer que a data/hora atual tem de ser posterior ou igual a não-antes de data/hora listada na *nbf* atributo. O Azure Key Vault pode fornecer para alguns pequenos leeway, normalmente, não mais do que alguns minutos, para levar em conta relógio distorção. O valor tem de ser um número que contém um valor de IntDate.  
- *EXP*: padrão de IntDate, opcional, é "sempre". O *exp* atributo (hora de expiração) identifica a hora de expiração em ou depois que a chave não deve ser utilizado para operação criptográfica, exceto para determinados tipos de operação em [particulares condições](#BKMK_key-date-time-ctrld-ops). O processamento do *exp* atributo requer que a data/hora atual tem de ser antes da data/hora de expiração listados na *exp* atributo. O Azure Key Vault pode fornecer para alguns pequenos leeway, normalmente, não mais do que alguns minutos, para levar em conta relógio distorção. O valor tem de ser um número que contém um valor de IntDate.  

Existem atributos adicionais de só de leitura que estão incluídos em qualquer resposta que inclui os principais atributos:  

- *criado*: IntDate, opcional. O *criado* atributo indica que esta versão da chave foi criada. Este valor é nulo para chaves criadas antes da adição deste atributo. O valor tem de ser um número que contém um valor de IntDate.  
- *atualizado*: IntDate, opcional. O *atualizado* atributo indica que a esta versão da chave foi atualizada. Este valor é nulo para as chaves que foram atualizadas pela última vez antes da adição deste atributo. O valor tem de ser um número que contém um valor de IntDate.  

Para obter mais informações sobre IntDate e outros tipos de dados, consulte [tipos de dados](#BKMK_DataTypes)  

#### <a name="BKMK_key-date-time-ctrld-ops"></a> Operações de controlado de data e hora

Chaves expiradas e não, mas válido, essas fora do *nbf* / *exp* janela, funcionará para **desencriptar**, **anular a moldagem** e **verificar** operações (não retornará 403, proibido). A lógica para utilizar o estado não ainda válido é permitir que uma chave a serem testados antes do uso de produção. A lógica para utilizar o estado expirado é para permitir operações de recuperação nos dados que foi criadas quando a chave era válida. Além disso, pode desativar o acesso a uma chave através de políticas de Key Vault ou ao atualizar o *habilitado* atributo-chave para **falso**.

Para obter mais informações sobre dados de tipos, consulte [tipos de dados](#BKMK_DataTypes).

Para obter mais informações sobre outros atributos possíveis, consulte a [chave de Web JSON (JWK)](http://tools.ietf.org/html/draft-ietf-jose-json-web-key).

### <a name="BKMK_Keytags"></a> Principais etiquetas

Pode especificar os metadados de específicos de aplicativo adicionais na forma de etiquetas. O Azure Key Vault suporta até 15 etiquetas, cada um dos quais pode ter um nome de 256 carateres e um valor de 256 carateres.  

>[!Note]
>As etiquetas são legíveis por um chamador se tiverem o *lista* ou *obter* permissão para esse tipo de objeto; chaves, segredos ou certificados.

###  <a name="BKMK_KeyAccessControl"></a> Controlo de acesso a chaves

Controlo de acesso para as chaves geridas pelo Cofre de chaves é fornecido no nível de um cofre de chaves que age como o contentor de chaves. Existe uma política de controlo de acesso para as chaves que distingue a política de controlo de acesso para segredos no Cofre de chaves do mesmo. Os utilizadores podem criar cofres de um ou mais para armazenar chaves e são necessários para manter a segmentação apropriado do cenário e a gestão de chaves. Controlo de acesso para chaves é independente do controlo de acesso para segredos.  

As seguintes permissões podem ser concedidas, num por utilizador / serviço principal base, em que a entrada de controle de acesso de chaves no cofre. Estas permissões espelham de perto as operações permitidas num objeto de chave:  

-   *criar*: criar chaves novas
-   *obter*: ler a parte pública de uma chave, juntamente com seus atributos
-   *lista*: listar as chaves ou versões de uma chave armazenada num cofre de chaves
-   *Atualizar*: Atualize os atributos de uma chave
-   *eliminar*: eliminar o objeto de chave
-   *início de sessão*: Utilize a chave para assinar digests
-   *Certifique-se de*: utilizar a chave para verificar os resumos de
-   *wrapKey*: utilizar a chave para proteger uma chave simétrica
-   *unwrapKey*: Utilize a chave para desproteger encapsulada chaves simétricas
-   *encriptar*: utilizar a chave para proteger uma seqüência arbitrária de bytes
-   *desencriptar*: utilizar a chave para desproteger uma seqüência de bytes
-   *importar*: importar uma chave para um cofre de chaves
-   *cópia de segurança*: cópia de segurança de uma chave no Cofre de chaves
-   *Restaurar*: restaurar uma cópia de segurança da chave para um cofre de chaves
-   *todos os*: todas as permissões

## <a name="key-vault-secrets"></a>Segredos do Key Vault 

###  <a name="BKMK_WorkingWithSecrets"></a> Trabalhar com segredos

Os segredos no Cofre de chaves do Azure são seqüências de octeto com um tamanho máximo de 25 mil bytes, cada. O serviço Azure Key Vault não fornece qualquer semântica para segredos; ele simplesmente aceita os dados, encripta e armazena-lo, retornando um identificador SECRETO, o "id", que pode ser usada para recuperar o segredo num momento posterior.  

Para dados altamente confidenciais, os clientes devem considerar camadas adicionais de proteção para dados armazenados no Azure Key Vault; Por exemplo ao previamente encriptar dados com uma chave de proteção separados.  

O Azure Key Vault também oferece suporte a um campo contentType para segredos. Os clientes podem especificar o tipo de conteúdo, "contentType", de um segredo para ajudar a interpretar os dados secretos quando ela é recuperada. O comprimento máximo deste campo é 255 carateres. Não há nenhum valor predefinido. A utilização de sugerida é como uma sugestão para interpretar os dados secretos. Por exemplo, uma implementação pode armazenar palavras-passe e certificados como segredos, em seguida, utilize este campo para indicar que. Não há nenhum valor predefinido.  

###  <a name="BKMK_SecretAttrs"></a> Atributos secretos

Além dos dados secretos, podem ser especificados os seguintes atributos:  

- *EXP*: a predefinição de IntDate, opcional, é **para sempre**. O *exp* atributo (hora de expiração) identifica a hora de expiração em ou depois que os dados secretos devem não ser recuperados, exceto nas [situações específicas](#BKMK_secret-date-time-ctrld-ops). Este campo é para **informativa** fins apenas que ela informa aos usuários do serviço do Cofre de chaves que não pode ser utilizado um segredo específico. O valor tem de ser um número que contém um valor de IntDate.   
- *NBF*: a predefinição de IntDate, opcional, é **agora**. O *nbf* (não antes) atributo identifica o tempo antes do qual os dados secretos não devem ser obtidos, exceto nas [situações específicas](#BKMK_secret-date-time-ctrld-ops). Este campo é para **informativa** apenas a fins. O valor tem de ser um número que contém um valor de IntDate. 
- *ativada*: booleana, opcional, a predefinição é **true**. Esse atributo Especifica se é ou não podem ser obtidos dados secretos. O atributo habilitado é utilizado em conjunto com e *exp* quando uma operação ocorre entre e exp, será apenas ser permitida se ativada é definido como **verdadeiro**. Operações de fora a *nbf* e *exp* janela são automaticamente não permitido, exceto na [situações específicas](#BKMK_secret-date-time-ctrld-ops).  

Existem atributos adicionais de só de leitura que estão incluídos em qualquer resposta que inclui atributos secretos:  

- *criado*: IntDate, opcional. O atributo criado indica que esta versão do segredo do foi criada. Este valor é nulo para segredos criados antes da adição deste atributo. O valor tem de ser um número que contém um valor de IntDate.  
- *atualizado*: IntDate, opcional. O atributo atualizado indica que a esta versão do segredo do foi atualizada. Este valor é nulo para segredos que foram atualizados pela última vez antes da adição deste atributo. O valor tem de ser um número que contém um valor de IntDate.

#### <a name="BKMK_secret-date-time-ctrld-ops"></a> Operações de controlado de data e hora

Um segredo **Obtenha** operação funcionará para segredos não ainda válido e expirados, fora a *nbf* / *exp* janela. Chamar um segredo **obter** operação, para um segredo não ainda válido, pode ser utilizada para fins de teste. A obter (**obter**ing) um segredo expirado, pode ser utilizado para operações de recuperação.

Para obter mais informações sobre dados de tipos, consulte [tipos de dados](#BKMK_DataTypes).  

###  <a name="BKMK_SecretAccessControl"></a> Controlo de acesso a segredos

Controlo de acesso para segredos geridos no Azure Key Vault é fornecido no nível de um cofre de chaves que age como o contentor desses segredos. Existe uma política de controlo de acesso para segredos que distingue a política de controlo de acesso para as chaves no Cofre de chaves do mesmo. Os utilizadores podem criar cofres de um ou mais para armazenar segredos e são necessários para manter a segmentação apropriado do cenário e a gestão de segredos. Controlos de acesso para segredos são independentes de controlo de acesso para as chaves.  

As seguintes permissões podem ser utilizadas, numa base por principal, na entrada de controle de acesso a segredos no cofre e espelham de perto as operações permitidas num objeto secreto:  

-   *definir*: criar novos segredos  
-   *obter*: ler um segredo  
-   *lista*: listar os segredos ou versões de um segredo armazenados num cofre de chave  
-   *eliminar*: eliminar o segredo  
-   *todos os*: todas as permissões  

Para obter mais informações sobre como trabalhar com segredos, consulte [operações de segredo na referência de API de REST do Cofre de chaves](/rest/api/keyvault).  

###  <a name="BKMK_SecretTags"></a> Etiquetas secretas  
Pode especificar os metadados de específicos de aplicativo adicionais na forma de etiquetas. O Azure Key Vault suporta até 15 etiquetas, cada um dos quais pode ter um nome de 256 carateres e um valor de 256 carateres.  

>[!Note]
>As etiquetas são legíveis por um chamador se tiverem o *lista* ou *obter* permissão para esse tipo de objeto; chaves, segredos ou certificados.

## <a name="key-vault-certificates"></a>Certificados de Cofre de chaves

Suporte de certificados do Key Vault fornece para gestão do seu x509 certificados e os comportamentos seguintes:  

-   Permite que um proprietário de certificado criar um certificado por meio de um processo de criação do Cofre de chaves ou a importação de um certificado existente. Isto inclui ambos autoassinados e certificados gerados de autoridade de certificação.
-   Permite que um proprietário de certificado do Key Vault implementar o armazenamento seguro e gestão de X509 certificados, sem interação com o material de chave privada.  
-   Permite que um proprietário de certificado criar uma política que direciona o Key Vault para gerir o ciclo de vida de um certificado.  
-   Permite que os proprietários de certificado fornecer informações de contato para notificações sobre eventos de ciclo de vida de expiração e renovação de certificado.  
-   Suporta a renovação automática com emissores selecionados - Key Vault parceiro X509 fornecedores de certificado / autoridades de certificado.

>[!Note]
>Fornecedores/autoridades de uma parceria não também são permitidas mas não suporta a funcionalidade de renovação automática.

###  <a name="BKMK_CompositionOfCertificate"></a> Composição de um certificado

Quando é criado um certificado do Key Vault, uma chave endereçável e o segredo também são criados com o mesmo nome. A chave do Key Vault permite operações-chave e o segredo do Key Vault permite a obtenção do valor do certificado como um segredo. Um certificado do Key Vault também contém x509 pública metadados de certificado.  

O identificador e a versão de certificados é semelhante de chaves e segredos. Uma versão específica de uma chave endereçável e o segredo criado com a versão de certificado do Key Vault está disponível na resposta de certificado do Key Vault.
 
![Certificados são objetos complexos](media/azure-key-vault.png)

###  <a name="BKMK_CertificateExportableOrNonExportableKey"></a> Chave exportável ou não exportável

Quando é criado um certificado do Key Vault, que pode ser obtida do segredo endereçável com a chave privada no formato PFX ou PEM se a política utilizada para criar o certificado indicado que a chave é exportável. Se a política utilizada para criar o certificado do Key Vault indicado a chave para ser não exportável, em seguida, a chave privada não é uma parte do valor quando recuperados como um segredo.  

A chave endereçável torna-se mais relevante com certificados de KV não exportável. Operações da chave KV endereçável estão mapeadas a partir *keyusage* campo da política de certificado KV utilizada para criar o certificado de KV.  

São suportados dois tipos de chave – *RSA* ou *RSA HSM* com certificados. Exportável só é permitida com RSA, não são suportada por RSA HSM.  

###  <a name="BKMK_CertificateAttributesAndTags"></a> Atributos de certificado e as etiquetas

Além de metadados do certificado, uma chave de endereçável e um segredo endereçável, um certificado do Key Vault também contém atributos e as etiquetas.  

#### <a name="attributes"></a>Atributos

Os atributos de certificado são espelhados nos atributos dos endereçável chave e segredo criado quando o certificado de KV é criado.  

Um certificado do Key Vault tem os seguintes atributos:  

-   *ativada*: booleana, opcional, a predefinição é **true**. Esse atributo pode ser especificado para indicar se os dados do certificado podem ser obtidos como operável como uma chave ou segredo. Isto é utilizado em conjunto com *nbf* e *exp* quando uma operação ocorre entre *nbf* e *exp*, só irá ser permitida se ativada está definido como true. Operações de fora a *nbf* e *exp* não janela são automaticamente permitidas.  

Existem atributos adicionais de só de leitura que estão incluídos na resposta:

-   *criado*: IntDate: indica que esta versão do certificado foi criada.  
-   *atualizado*: IntDate: indica que a esta versão do certificado foi atualizada.  
-   *EXP*: IntDate: contém o valor da data de expiração de x509 certificado.  
-   *NBF*: IntDate: contém o valor da data de x509 certificado.  

> [!Note] 
> Se um certificado do Key Vault expira, seja endereçável chave e segredo tornar-se inoperáveis.  

#### <a name="tags"></a>Etiquetas

 Cliente especificado dicionário de pares chave-valor, semelhantes às etiquetas em chaves e segredos.  

 > [!Note]
> As etiquetas são legíveis por um chamador se tiverem o *lista* ou *obter* permissão para esse tipo de objeto; chaves, segredos ou certificados.

###  <a name="BKMK_CertificatePolicy"></a> Política de certificado

Uma política de certificado contém informações sobre como criar e gerir o ciclo de vida de um certificado de KV. Quando um certificado com chave privada é importado para o Cofre de chaves, é criada uma política de predefinição lendo o x509 certificado.  

Quando um certificado de KV é criado a partir do zero, uma política tem de ser fornecido para o Key Vault a informá-lo sobre como criar esta versão de certificado KV ou a próxima versão do certificado de KV. Quando uma política for estabelecida, não é necessário com sucessivas operações para criar versões de certificado KV seguintes de criação.  

Há apenas uma instância de uma política para todas as versões de um certificado de KV.  

Num alto nível, uma política de certificado contém o seguinte:  

-   Propriedades de certificado X509: contém o nome de assunto, os nomes alternativos do requerente etc. utilizado para criar uma x509 pedido de certificado.  
-   Propriedades da chave: contém o tipo de chave exportável, comprimento da chave e reutilizar os campos de chave. Estes campos instruir sobre como gerar uma chave do Cofre de chaves.  
-   Propriedades do segredo: contém as propriedades do segredo, como o tipo de conteúdo de segredo endereçável para gerar o valor secreto, para obter o certificado como um segredo.  
-   Ações de tempo de vida: contém ações de tempo de vida para o certificado de KV. Cada ação de tempo de vida contém:  

     - Acionador: especificado por meio de dias antes da expiração ou a duração de percentagem de span  

     - Ação: especificar o tipo de ação – *emailContacts* ou *renovação automática*  

-   Emissor: Parâmetros sobre o emissor de certificado a utilizar para emitir x509 certificados.  
-   Atributos de política: contém atributos associados à política  

#### <a name="x509-to-key-vault-usage-mapping"></a>X509 para mapeamento de utilização do Cofre de chaves

A tabela seguinte representa o mapeamento do x509 de política de utilização de chave para operações de chaves em vigor a partir de uma chave criada como parte de uma criação de certificados do Key Vault.

|**X509 sinalizadores de utilização de chave**|**Operações de chave do Key Vault**|**Comportamento predefinido**|
|----------|--------|--------|
|DataEncipherment|encriptar, desencriptar| N/A |
|DecipherOnly|desencriptar| N/A  |
|Bits DigitalSignature|iniciar sessão, certifique-se| Padrão de Cofre de chaves sem uma especificação de utilização no momento de criação do certificado | 
|EncipherOnly|encrypt| N/A |
|KeyCertSign|iniciar sessão, certifique-se|N/A|
|KeyEncipherment|wrapKey, unwrapKey| Padrão de Cofre de chaves sem uma especificação de utilização no momento de criação do certificado | 
|Não-repúdio|iniciar sessão, certifique-se| N/A |
|crlsign|iniciar sessão, certifique-se| N/A |

###  <a name="BKMK_CertificateIssuer"></a> Emissor do certificado

Um objeto de certificado do Key Vault mantém uma configuração utilizada para comunicar com um provedor de emissor do certificado selecionado para certificados x509 de ordem.  

-   Para parceiros do Key Vault com seguintes fornecedores de emissor de certificados para certificados SSL

|**Nome do fornecedor**|**Localizações**|
|----------|--------|
|DigiCert|Suportado em todas as localizações de serviço do Cofre de chaves na cloud pública e o Azure Government|
|GlobalSign|Suportado em todas as localizações de serviço do Cofre de chaves na cloud pública e o Azure Government|

Antes de um emissor do certificado pode ser criado no Cofre de chaves, passos de pré-requisitos seguintes 1 e 2 devem ser realizados com êxito.  

1. Carregar para fornecedores de autoridade de certificado  

    -   Um administrador da organização deve integrar a empresa (ex. Contoso) com, pelo menos, um fornecedor de AC.  

2. O administrador cria as credenciais de autor do pedido para o Key Vault inscrever (e renovar) certificados SSL  

    -   Fornece a configuração a ser utilizado para criar um objeto de emissor do fornecedor no Cofre de chaves  

Para obter mais informações sobre como criar objetos de emissor a partir do portal de certificados, consulte o [blog de certificados do Key Vault](http://aka.ms/kvcertsblog)  

Key Vault permite a criação de vários objetos de emissor com a configuração do fornecedor de emissor diferentes. Depois de criar um objeto de emissor, seu nome pode ser referenciado numa ou várias políticas de certificado. Referencia o objeto de emissor instrui o Key Vault para utilizar a configuração conforme especificado no objeto de emissor ao pedir o x509 certificado do fornecedor de AC durante a criação do certificado e a renovação.  

Objetos de emissor são criados no cofre e só podem ser utilizados com certificados de KV no mesmo cofre.  

###  <a name="BKMK_CertificateContacts"></a> Contactos do certificado

Contactos do certificado contêm informações de contacto para enviar notificações acionadas por eventos de ciclo de vida do certificado. As informações de contatos são partilhadas por todos os certificados no Cofre de chaves. É enviada uma notificação para todos os contatos especificados para um evento para qualquer certificado no Cofre de chaves.  

Se a política de um certificado estiver definida para renovação automática, em seguida, é enviada uma notificação sobre os eventos seguintes.  

-   Antes de renovação de certificado
-   Após a renovação de certificado, que indica se o certificado foi renovado com êxito ou se tiver ocorrido um erro, que requerem a renovação manual do certificado.  

 Se a política de um certificado é definida manualmente a ser renovado (apenas ao e-mail), em seguida, uma notificação é enviada quando está na altura de renovar o certificado.  

###  <a name="BKMK_CertificateAccessControl"></a> Controlo de acesso de certificado

 Controlo de acesso para certificados é gerenciado pelo Cofre de chaves e é fornecido no nível de um cofre de chaves que age como o contêiner desses certificados. Existe uma política de controlo de acesso para os certificados que distingue a política de controlo de acesso de chaves e segredos no Cofre de chaves do mesmo. Os utilizadores podem criar cofres de um ou mais para armazenar certificados e são necessários para manter a segmentação apropriado do cenário e a gestão de certificados.  

 As seguintes permissões podem ser utilizadas numa base por principal, na entrada de controle de acesso a segredos num cofre de chaves e estreitamente espelhos as operações permitidas num objeto secreto:  

-   *obter*: permite que o get de qualquer versão de um certificado ou a versão atual do certificado 
-   *lista*: permite que a lista dos certificados atuais ou versões de um certificado  
-   *eliminar*: permite que a eliminação de um certificado, a política e todas as suas versões  
-   *criar*: permite criar um certificado do Key Vault.  
-   *importar*: permite que a importação de material de certificado para um certificado do Cofre de chave.  
-   *Atualizar*: permite que a atualização de um certificado.  
-   *managecontacts*: permite a gestão de contactos do certificado de Key Vault  
-   *getissuers*: permite que o get de emissores de um certificado  
-   *listissuers*: permite que a lista de emissores de certificados  
-   *setissuers*: permite criar ou atualizar de emissores de certificados do Key Vault  
-   *deleteissuers*: permite que a eliminação de emissores de certificados do Key Vault  
-   *todos os*: concede todas as permissões  

Para obter mais informações, consulte a [operações na referência de API de REST do Cofre de chaves de certificado](/rest/api/keyvault). 

## <a name="see-also"></a>Consultar Também

- [Autenticação, os pedidos e respostas](authentication-requests-and-responses.md)
- [Versões do Key Vault](key-vault-versions.md)
- [Guia do programador do Cofre de chaves](/azure/key-vault/key-vault-developers-guide)
