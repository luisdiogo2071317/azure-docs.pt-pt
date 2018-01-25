---
title: "Personalizar as configurações de segurança do SO no Centro de segurança do Azure (pré-visualização) | Microsoft Docs"
description: "Este artigo demonstra como personalizar as avaliações de centro de segurança"
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/16/2018
ms.author: terrylan
ms.openlocfilehash: d42dd4ba150a28109d6bb3f7c2281d07b21a366e
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="customize-os-security-configurations-in-azure-security-center-preview"></a>Personalizar as configurações de segurança do SO no Centro de segurança do Azure (pré-visualização)

Esta explicação passo a passo demonstra como personalizar as avaliações de configuração do SO segurança no Centro de segurança do Azure.

## <a name="what-are-os-security-configurations"></a>Quais são as configurações de segurança do SO?

Configurações de segurança de monitores de centro de segurança do Azure através da aplicação de um conjunto de regras recomendadas mais de 150 para proteger o sistema operativo, incluindo regras relacionadas com a firewalls, auditoria, políticas de palavra-passe e muito mais. Se for encontrada uma máquina ter uma configuração vulnerável, o Centro de segurança gera uma recomendação de segurança.

Ao personalizar as regras, as organizações podem controlar as opções de configuração são mais adequadas para o respetivo ambiente. Pode definir uma política personalizada de avaliação e, em seguida, aplicam-se em todas as máquinas aplicáveis na subscrição.

> [!NOTE]
> - Atualmente, a personalização da configuração de segurança do SO está disponível para Windows Server 2008, 2008 R2, 2012 e 2012 R2 de versões apenas nos sistemas operativos.
> - A configuração aplica-se a todas as VMs e computadores que estejam ligados a todas as áreas de trabalho sob a subscrição selecionada.
> - Personalização de configuração de segurança do SO não está disponível apenas na camada padrão de centro de segurança.
>
>

Pode personalizar as regras de configuração de segurança de SO por ativar e desativar uma regra específica, alterar a definição pretendida para uma regra existente ou adicionar uma nova regra com base nos tipos de regra suportados (registo, política de auditoria e política de segurança). Atualmente, a definição pretendida tem de ser um valor exato.

Novas regras tem de estar no mesmo formato e estrutura como outras regras existentes do mesmo tipo.

> [!NOTE]
> Para personalizar as configurações de segurança do SO, tem de ser atribuída a função de *proprietário da subscrição*, *contribuinte de subscrição*, ou *administrador de segurança*.
>
>

## <a name="customize-the-default-os-security-configuration"></a>Personalizar a configuração de segurança de SO predefinida

Para personalizar a configuração de segurança de SO predefinida no Centro de segurança, efetue o seguinte:

1.  Abra o dashboard **Centro de Segurança**.

2.  No painel esquerdo, selecione **política de segurança**.  
    O **Centro de segurança - política de segurança** é aberta a janela.

    ![Lista de políticas de segurança](media/security-center-customize-os-security-config/open-security-policy.png)

3.  Selecione a subscrição que pretende efetuar a personalização para.

4. Em **componentes da política**, selecione **configurações de segurança de editar**.  
    O **configurações de segurança de editar** é aberta a janela. 
    
    ![A janela "Editar configurações de segurança"](media/security-center-customize-os-security-config/blade.png)

5. No painel da direita, siga os passos para transferir, editar e carregar o ficheiro modificado.

   > [!NOTE]
   > Por predefinição, o ficheiro de configuração que transferir está a ser *json* formato. Para obter instruções sobre como modificar este ficheiro, aceda a [personalizar o ficheiro de configuração](#customize-the-configuration-file).
   >

   Depois de ter guardado com êxito o ficheiro, a configuração é aplicada a todas as VMs e computadores que estejam ligados a todas as áreas de trabalho sob a subscrição. O processo normalmente demora alguns minutos, mas pode demorar mais tempo, dependendo do tamanho de infraestrutura. 

6. Para confirmar a alteração, selecione **guardar**. Caso contrário, a política não está armazenada.

    ![O botão Guardar](media/security-center-customize-os-security-config/save-successfully.png)

Em qualquer momento, só pode repor a configuração atual da política no estado predefinido. Para tal, no **as regras de configuração de segurança de editar o SO** janela, selecione **repor**. Confirme esta opção, selecionando **Sim** na janela de pop-up de confirmação.

![A janela de confirmação de reposição](media/security-center-customize-os-security-config/edit-alert.png)

## <a name="customize-the-configuration-file"></a>Personalizar o ficheiro de configuração

No ficheiro de personalização, cada versão suportada do SO tem um conjunto de regras ou ruleset. Cada ruleset tem o seu próprio nome e um ID exclusivo, conforme mostrado no exemplo seguinte:

![O nome de ruleset e o ID](media/security-center-customize-os-security-config/custom-file.png)

> [!NOTE]
> Este ficheiro de exemplo foi editado no Visual Studio, mas também pode utilizar o bloco de notas se tiver o plug-in de Visualizador de JSON instalado.
>
>

Ao editar o ficheiro de personalização, pode modificar uma regra ou todos eles. Cada ruleset inclui um *regras* secção que é separada nas três categorias: registo, a política de auditoria e a política de segurança, conforme mostrado aqui:

![Três categorias de ruleset](media/security-center-customize-os-security-config/rules-section.png)

Cada categoria tem o seu próprio conjunto de atributos. Pode alterar os seguintes atributos:

- **expectedValue**: tipo de dados do campo este atributo tem de corresponder aos valores suportados por *tipo de regra*, por exemplo:

  - **baselineRegistryRules**: O valor deve corresponder a [regValueType](https://msdn.microsoft.com/library/windows/desktop/ms724884) que está definida nessa regra.

  - **baselineAuditPolicyRules**: Utilize um dos seguintes valores de cadeia:

    - *Êxito e falha*

    - *Êxito*

  - **baselineSecurityPolicyRules**: Utilize um dos seguintes valores de cadeia:

    - *Nenhum*

    - Lista de permitidos grupos de utilizadores, por exemplo: *administradores*, *operadores de cópia de segurança*

-   **estado**: A cadeia pode conter as opções *desativado* ou *ativado*. Para esta versão de pré-visualização privada, a cadeia é maiúsculas e minúsculas.

Estes são os campos únicos que podem ser configurados. Se viola o formato de ficheiro ou o tamanho, não poderá guardar a alteração. A seguinte mensagem de erro ocorre quando não é possível processar o ficheiro:

![Mensagem de erro de configuração de segurança](media/security-center-customize-os-security-config/invalid-json.png)

Para obter uma lista de outros erros de potenciais, consulte [códigos de erro](#error-codes).

As secções seguintes três contém exemplos das regras anteriores. O *expectedValue* e *estado* atributos podem ser alterados.

**baselineRegistryRules**
```
    {
    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\LanManServer\\\\Parameters",
    "valueName": "restrictnullsessaccess",
    "ruleId": "f9020046-6340-451d-9548-3c45d765d06d",
    "originalId": "0f319931-aa36-4313-9320-86311c0fa623",
    "cceId": "CCE-10940-5",
    "ruleName": "Network access: Restrict anonymous access to Named Pipes and
    Shares",
    "ruleType": "Registry",
    "expectedValue": "1",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Disabled"

    }
```

**baselineAuditPolicyRules**
```
    {
    "auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
    "ruleId": "37745508-95fb-44ec-ab0f-644ec0b16995",
    "originalId": "2ea0de1a-c71d-46c8-8350-a7dd4d447895",
    "cceId": "CCE-11001-5",
    "ruleName": "Audit Policy: Account Management: Other Account Management Events",
    "ruleType": "AuditPolicy",
    "expectedValue": "Success and Failure",
    "severity": "Critical",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Enabled"
    }
```

**baselineSecurityPolicyRules**
```
    {
    "sectionName": "Privilege Rights",
    "settingName": "SeIncreaseWorkingSetPrivilege",
    "ruleId": "b0ec9d5e-916f-4356-83aa-c23522102b33",
    "originalId": "b61bd492-74b0-40f3-909d-36b9bf54e94c",
    "cceId": "CCE-10548-6",
    "ruleName": "Increase a process working set",
    "ruleType": "SecurityPolicy",
    "expectedValue": "Administrators, Local Service",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Enabled"
    }
```

Algumas regras estão duplicadas para os diferentes tipos de SO. Regras duplicadas com o mesmo *originalId* atributo.

## <a name="create-custom-rules"></a>Criar regras personalizadas

Também pode criar novas regras. Antes de criar uma nova regra, tenha em atenção as seguintes restrições:

-   Versão do esquema, *baselineId* e *baselineName* não pode ser alterada.

-   Não é possível remover RuleSet.

-   Não é possível adicionar RuleSet.

-   O número máximo de regras permitidos (incluindo as regras predefinidas) é 1000.

Novas regras personalizadas são marcadas com uma nova origem personalizada (! = "Microsoft"). O *ruleId* campo pode ser nulo nem estar vazio. Se estiver vazia, a Microsoft gera um. Se não estiver vazia, tem de ter um GUID válido que seja exclusivo em todas as regras (predefinidos e personalizados). Reveja as seguintes restrições para os campos de núcleo:

-   **originalId**: pode ser nulo nem estar vazio. Se *originalId* é não vazio, deve ser um GUID válido.

-   **cceId**: pode ser nulo nem estar vazio. Se *cceId* é não vazio, tem de ser exclusivo.

-   **ruleType**: (selecionar um) registo, AuditPolicy ou SecurityPolicy.

-   **Gravidade**: (selecionar um) desconhecido, crítico, aviso ou informativos.

-   **analyzeOperation**: tem de ser *é igual a*.

-   **auditPolicyId**: tem de ser um GUID válido.

-   **regValueType**: (selecionar um) Int, longa, cadeia ou MultipleString.

> [!NOTE]
> Ramo de registo tem de ser *LocalMachine*.
>
>

Exemplo de uma nova regra personalizada:

**Registo**:
```
    {
    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\Netlogon\\\\MyKeyName",
    "valueName": "MyValueName",
    "originalId": "",
    "cceId": "",
    "ruleName": "My new registry rule”, "baselineRuleType": "Registry",
    "expectedValue": "123", "severity": "Critical",
    "analyzeOperation": "Equals",
    "source": "MyCustomSource",
    "state": "Enabled"
    }
```
**Política de segurança**:
```
   {
   "sectionName": "Privilege Rights",
   "settingName": "SeDenyBatchLogonRight",
   "originalId": "",
   "cceId": "",
   "ruleName": "My new security policy rule", "baselineRuleType":
   "SecurityPolicy",
   "expectedValue": "Guests",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```
**A política de auditoria**:
```
   {
   "auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
   "originalId": "",
   "cceId": "",
   "ruleName": " My new audit policy rule ", "baselineRuleType": "AuditPolicy",
   "expectedValue": " Failure",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```

## <a name="file-upload-failures"></a>Falhas de carregamento de ficheiros

Se o ficheiro de configuração submetido é inválido devido a erros de valores ou formatação, é apresentado um erro de falha. Pode transferir um relatório de erros detalhados. csv para remediar e corrigir os erros antes de submeter um ficheiro de configuração corrigida.

![Mensagem de erro "ação de guardar falhou"](media/security-center-customize-os-security-config/invalid-configuration.png)

Exemplo de um ficheiro de erro:

![Exemplo de ficheiro de erro](media/security-center-customize-os-security-config/errors-file.png)

## <a name="error-codes"></a>Códigos de erro

Todos os potenciais erros estão listados na seguinte tabela:

| **Erro**                                | **Descrição**                                                                                                                              |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| BaselineConfiguratiohSchemaVersionError  | A propriedade *schemaVersion* encontrada inválido ou está vazio. O valor deve ser definido como *{0}*.                                                         |
| BaselineInvalidStringError               | A propriedade *{0}* não pode conter  *\\ n* .                                                                                                         |
| BaselineNullRuleError                    | A lista de regras de configuração de linha de base contém uma regra com o valor *nulo*.                                                                         |
| BaselineRuleCceIdNotUniqueError          | O ID de CCE *{0}* não é exclusivo.                                                                                                                  |
| BaselineRuleEmptyProperty                | A propriedade *{0}* está em falta ou é inválido.                                                                                                       |
| BaselineRuleIdNotInDefault               | A regra tem uma propriedade de origem *Microsoft* mas não foi encontrada no ruleset de predefinição Microsoft.                                                   |
| BaselineRuleIdNotUniqueError             | O Id de regra não é exclusivo.                                                                                                                       |
| BaselineRuleInvalidGuid                  | A propriedade *{0}* encontrada inválido. O valor não é um GUID válido.                                                                             |
| BaselineRuleInvalidHive                  | O ramo de registo tem de ser LocalMachine.                                                                                                                   |
| BaselineRuleNameNotUniqueError           | O nome da regra não é exclusivo.                                                                                                                 |
| BaselineRuleNotExistInConfigError        | A regra não foi encontrada na configuração de novo. Não é possível eliminar a regra.                                                                     |
| BaselineRuleNotFoundError                | A regra não foi encontrada no predefinido ruleset de linha de base.                                                                                        |
| BaselineRuleNotInPlace                   | A regra corresponde a uma regra predefinida com tipo {0} e está listada na lista de {1}.                                                                       |
| BaselineRulePropertyTooLong              | A propriedade *{0}* é demasiado longo. Número máximo permitido de comprimento: {1}.                                                                                        |
| BaselineRuleRegTypeInvalidError          | O valor esperado *{0}* não corresponde ao tipo de valor de registo que está definido.                                                              |
| BaselineRulesetAdded                     | Ruleset com o ID *{0}* não foi encontrado na configuração predefinida. Não é possível adicionar o ruleset.                                               |
| BaselineRulesetIdMustBeUnique            | A linha base especificada ruleset *{0}* tem de ser exclusivo.                                                                                           |
| BaselineRulesetNotFound                  | Ruleset com o id *{0}* e nome *{1}* não foi encontrado na configuração indicada. Não é possível eliminar o ruleset.                                |
| BaselineRuleSourceNotMatch               | A regra com o ID *{0}* já está definido.                                                                                                       |
| BaselineRuleTypeDoesntMatch              | O tipo de regra predefinida é *{0}*.                                                                                                              |
| BaselineRuleTypeDoesntMatchError         | O tipo real da regra é *{0}*, mas o *ruleType* propriedade é *{1}*.                                                                          |
| BaselineRuleUnpermittedChangesError      | Apenas *expectedValue* e *estado* são permitidas propriedades de ser alteradas.                                                                       |
| BaselineTooManyRules                     | O número máximo de regras personalizadas permitidas é {0} regras. A configuração indicada contém regras {1}, regras de predefinidas {2} e regras de {3} personalizado. |
| ErrorNoConfigurationStatus               | Não foi encontrado nenhum Estado de configuração. Indicar o estado de configuração pretendida: *predefinido* ou *personalizada*.                                    |
| ErrorNonEmptyRulesetOnDefault            | O estado da configuração é definido como predefinido. O *BaselineRulesets* lista tem de ser nulo nem estar vazio.                                                          |
| ErrorNullRulesetsPropertyOnCustom        | O estado de configuração indicado é *personalizada* , mas a *baselineRulesets* propriedade é nulo ou está vazio.                                             |
| ErrorParsingBaselineConfig               | A configuração especificada é inválida. Um ou mais dos valores definidos tem um valor nulo ou um tipo inválido.                                  |
| ErrorParsingIsDefaultProperty            | O dado *configurationStatus* valor *{0}* é inválido. O valor só pode ser *predefinido* ou *personalizada*.                                         |
| InCompatibleViewVersion                  | A versão de vista *{0}* é *não* suportado neste tipo de área de trabalho.                                                                                   |
| InvalidBaselineConfigurationGeneralError | A configuração de linha base especificada foi encontrada com um ou mais erros de validação de tipo.                                                          |
| ViewConversionError                      | A vista é uma versão mais antiga que suporta a área de trabalho. Falha na conversão de ver: {0}.                                                                 |

Se não tiver permissões suficientes, poderá obter um erro de falha geral, conforme mostrado aqui:

![Mensagem de erro "ação de guardar falhou"](media/security-center-customize-os-security-config/general-failure-error.png)

## <a name="next-steps"></a>Passos Seguintes
Este artigo abordados como personalizar as avaliações de configuração do SO segurança no Centro de segurança. Para saber mais sobre as regras de configuração e remediação, consulte:

- [Identificadores de configuração comuns do Centro de segurança e regras de linha de base](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- Centro de segurança utiliza enumeração comuns de configuração (de CCE) para atribuir os identificadores exclusivos para as regras de configuração. Para obter mais informações, consulte [CCE](https://nvd.nist.gov/config/cce/index).
- Para resolver vulnerabilidades quando a configuração do SO não corresponde as regras de configuração de segurança recomendadas, consulte [remediar configurações de segurança](security-center-remediate-os-vulnerabilities.md).
