---
title: "Personalizar as configurações de segurança do SO no Centro de segurança do Azure [pré-visualização] | Microsoft Docs"
description: "Este artigo informa como personalizar as avaliações de centro de segurança"
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
ms.date: 01/08/2018
ms.author: terrylan
ms.openlocfilehash: 2fa63515d290e6700fbe4a90ae509f4635b19f29
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2018
---
# <a name="customizing-os-security-configurations-in-azure-security-center-preview"></a>Personalizar as configurações de segurança do SO no Centro de segurança do Azure [pré-visualização]

Saiba como personalizar as avaliações de SO a configuração de segurança no Centro de segurança do Azure a utilizar estas instruções.

## <a name="what-are-os-security-configurations"></a>Quais são as configurações de segurança do SO?

Configurações de segurança do monitores de centro de segurança do Azure utilizando um conjunto de regras recomendadas mais de 150 para proteger o sistema operativo, incluindo regras relacionadas com a firewalls, auditoria, políticas de palavra-passe e muito mais. Se for encontrada uma máquina ter uma configuração vulnerável, é gerada uma recomendação de segurança.

Personalização das regras pode ajudar as organizações para controlar as opções de configuração são mais adequadas para o respetivo ambiente. Esta funcionalidade permite aos utilizadores definir uma política personalizada de avaliação e aplicá-la em todas as máquinas aplicáveis na subscrição.

> [!NOTE]
> - Personalização do SO a configuração de segurança está atualmente disponível para Windows Server 2008, 2008 R2, 2012, apenas o sistemas de operativos 2012R2.
- A configuração é aplicada a todas as VMs e computadores ligados a todas as áreas de trabalho sob a subscrição selecionada.
- Personalização da configuração de segurança de SO só está disponível no escalão Standard do Centro de segurança.
>
>

Como personalizar as regras de configuração de segurança do SO?

Pode personalizar as regras de configuração de segurança de SO ao ativar e desativar uma regra específica, alterar a definição pretendida para uma regra existente e adicionar uma nova regra com base nos tipos de regra suportados (registo, política de auditoria e política de segurança). Atualmente, a definição pretendida tem de ser um valor exato.

Novas regras tem de estar no mesmo formato e estrutura como outras regras existentes do mesmo tipo.

> [!NOTE]
> Para personalizar as configurações de segurança do SO, tem de ser atribuída a função de proprietário da subscrição, contribuinte de subscrição ou o administrador de segurança.
>
>

## <a name="customize-security-configuration"></a>Personalizar a configuração de segurança

Para personalizar a predefinição de SO de configuração de segurança no Centro de segurança:

1.  Abra o dashboard do **Centro de Segurança**.

2.  No menu principal do Centro de segurança, selecione **política de segurança**.  **Centro de segurança - política de segurança** abre.

3.  Selecione a subscrição que pretende efetuar a personalização para.

    ![](media/security-center-customize-os-security-config/open-security-policy.png)

4. Em **componentes da política**, selecione **configurações de segurança de editar**.

6.  **Configurações de segurança de editar** abre. Siga os passos realçados no ecrã para transferir, editar e carregar o ficheiro modificado.

    ![](media/security-center-customize-os-security-config/blade.png)

  > [!NOTE]
  > Por predefinição, o ficheiro de configuração que transferir está a ser *json* formato. Para obter instruções sobre como modificar este ficheiro, aceda a [personalizar o ficheiro de configuração](#customize-the-configuration-file).
  >

7. Depois de guardar com êxito o ficheiro, a configuração é aplicada a todas as VMs e computadores ligados a todos os workspace(s) sob a subscrição selecionada. Este processo poderá demorar algum tempo, normalmente, alguns minutos, mas pode, poderá demorar mais uma vez que depende o tamanho de infraestrutura. Selecione **guardar** para consolidar a alteração, caso contrário, a política não é armazenada.

    ![](media/security-center-customize-os-security-config/save-successfully.png)

Em qualquer momento, pode repor a configuração atual da política para o estado de política predefinido, selecionando o **repor** opção **as regras de configuração de segurança de SO editar**. Se escolher esta opção, receberá o seguinte alerta pop-up. Selecione **Sim** para confirmar.

![](media/security-center-customize-os-security-config/edit-alert.png)

## <a name="customize-the-configuration-file"></a>Personalizar o ficheiro de configuração

O ficheiro de personalização cada versão suportada do SO tem um conjunto de regras (ruleset). Cada conjunto de regras tem o seu próprio nome e um ID exclusivo, conforme mostrado no exemplo seguinte:

![](media/security-center-customize-os-security-config/custom-file.png)

> [!NOTE]
> Este ficheiro foi editado com o Visual Studio, mas também pode utilizar o bloco de notas, desde que tiver o Visualizador JSON Plug-in instalado.
>
>

Ao editar este ficheiro, pode modificar uma regra ou todos eles. Cada ruleset inclui um *regras* secção que contém as regras, divididas em 3 categorias de regras: registo, a política de auditoria e a política de segurança, como mostrados abaixo:

![](media/security-center-customize-os-security-config/rules-section.png)

Cada categoria tem o seu próprio conjunto de atributos. Para as regras existentes, pode efetuar alterações dos seguintes:

- expectedValue: tipo de dados do campo este atributo tem de corresponder aos valores suportados por cada tipo de regra, por exemplo:

  - baselineRegistryRules: o valor deve corresponder ao [regValueType] (https://msdn.microsoft.com/library/windows/desktop/ms724884 (v=vs.85) definida dessa regra.

  - baselineAuditPolicyRules: o valor deve ser um valor de cadeia, um dos seguintes:

    - Êxito e Falha

    - Êxito

  - baselineSecurityPolicyRules: o valor deve ser um valor de cadeia, um dos seguintes:

    - "Ninguém"

    - Lista de permitidos grupos de utilizadores, por exemplo: "Administradores, operadores de cópia de segurança"

-   Estado: cadeia que pode conter as opções "Enabled" ou "Desativadas". Para esta versão de pré-visualização privada, a cadeia é maiúsculas e minúsculas.

Estes são os campos únicos que podem ser configurados. Se viola o formato de ficheiro ou o tamanho, não poderá guardar a alteração. A seguinte mensagem de erro ocorre quando não é possível processar o ficheiro:

![](media/security-center-customize-os-security-config/invalid-json.png)

Consulte [códigos de erro](#error-codes) para lista de potenciais erros.

Abaixo, pode encontrar exemplos destas regras e os atributos (em negrito) que podem ser alterados:

**Secção de regras:** baselineRegistryRules
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
    "**expectedValue**": "1",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "**state**": "Disabled"

}
```

**Secção de regras:** baselineAuditPolicyRules
```
{
"auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
"ruleId": "37745508-95fb-44ec-ab0f-644ec0b16995",
"originalId": "2ea0de1a-c71d-46c8-8350-a7dd4d447895",
"cceId": "CCE-11001-5",
"ruleName": "Audit Policy: Account Management: Other Account Management Events",
"ruleType": "AuditPolicy",
"**expectedValue**": "Success and Failure",
"severity": "Critical",
"analyzeOperation": "Equals",
"source": "Microsoft",
"**state**": "Enabled"
},
```

**Secções de regras:** baselineSecurityPolicyRules
```
{
"sectionName": "Privilege Rights",
"settingName": "SeIncreaseWorkingSetPrivilege",
"ruleId": "b0ec9d5e-916f-4356-83aa-c23522102b33",
"originalId": "b61bd492-74b0-40f3-909d-36b9bf54e94c",
"cceId": "CCE-10548-6",
"ruleName": "Increase a process working set",
"ruleType": "SecurityPolicy",
"**expectedValue**": "Administrators, Local Service",
"severity": "Warning",
"analyzeOperation": "Equals",
"source": "Microsoft", "**state**": "Enabled"
},
```

Existem algumas regras duplicados para os diferentes tipos de SO. Regras de duplicado com o mesmo 'originalId'.

## <a name="adding-a-new-custom-rule"></a>Adicionar uma nova regra personalizada

Também pode criar uma nova regra. Antes de criar uma nova regra, tenha em atenção as seguintes restrições:

-   Versão do esquema, *baselineId* e *baselineName* não pode ser alterada.

-   Não é possível remover RuleSet.

-   Não é possível adicionar RuleSet.

-   O número máximo de regras permitidos (incluindo as regras predefinidas) é 1000 regras.

Novas regras personalizadas são marcadas com uma nova origem personalizada (! = "Microsoft"). O *ruleId* campo pode ser nulo nem estar vazio. Se estiver vazia, a Microsoft gera um. Se não estiver vazia, tem de ter um GUID válido exclusivo em todas as regras (predefinidos e personalizados). Reveja as restrições abaixo sobre os campos de núcleo:

-   *originalId* -pode ser nulo nem estar vazio. Se *originalId* é não vazio, deve ser um GUID válido.

-   *cceId* -pode ser nulo nem estar vazio. Se *cceId* é não vazio, tem de ser exclusivo.

-   *ruleType* - um dos: registo, AuditPolicy ou SecurityPolicy.

-   *Gravidade* - um dos: desconhecido, crítico, aviso ou informativos.

-   *analyzeOperation* -tem de ser igual a.

-   *auditPolicyId* -tem de ser GUID válido.

-   *regValueType* -tem de ser um dos: Int, longa, cadeia ou MultipleString.

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
**A política de auditoria:**
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

Se o ficheiro de configuração submetido é inválido devido a erros de valores ou formatação, é apresentado um erro de falha. Pode transferir um relatório de csv de erros detalhados para remediar e corrija os erros antes de voltar a submeter um ficheiro de configuração corrigida.

![](media/security-center-customize-os-security-config/invalid-configuration.png)

Exemplo do ficheiro de erros:

![](media/security-center-customize-os-security-config/errors-file.png)

## <a name="error-codes"></a>Códigos de erro

A lista abaixo tem todos os potenciais erros:

| **Erro**                                | **Descrição**                                                                                                                              |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| BaselineConfiguratiohSchemaVersionError  | Foi encontrada a propriedade 'schemaVersion' inválido ou está vazio. O valor tem de ser definido como '{0}'.                                                         |
| BaselineInvalidStringError               | A propriedade '{0}' não pode conter '\\n'.                                                                                                         |
| BaselineNullRuleError                    | Lista de regras de configuração de linha de base contém uma regra com o valor 'null'.                                                                         |
| BaselineRuleCceIdNotUniqueError          | ID de CCE '{0}' não é exclusivo.                                                                                                                  |
| BaselineRuleEmptyProperty                | Propriedade: '{0}' está em falta ou é inválido.                                                                                                       |
| BaselineRuleIdNotInDefault               | A regra tem uma propriedade de origem 'Microsoft', mas não foi encontrada no Microsoft predefinido ruleset.                                                   |
| BaselineRuleIdNotUniqueError             | Id de regra não é exclusivo.                                                                                                                       |
| BaselineRuleInvalidGuid                  | A propriedade '{0}' foi encontrada inválida. O valor não é um Guid válido.                                                                             |
| BaselineRuleInvalidHive                  | Ramo de registo tem de ser LocalMachine.                                                                                                                   |
| BaselineRuleNameNotUniqueError           | O nome da regra não é exclusivo.                                                                                                                 |
| BaselineRuleNotExistInConfigError        | A regra não foi encontrada na configuração de novo. Não é possível eliminar a regra.                                                                     |
| BaselineRuleNotFoundError                | A regra não foi encontrada predefinidas no conjunto de regras de linha de base.                                                                                        |
| BaselineRuleNotInPlace                   | A regra corresponde a uma regra predefinida com tipo {0} e está listada na lista de {1}.                                                                       |
| BaselineRulePropertyTooLong              | A propriedade: '{0}' é demasiado longo. Número máximo permitido de comprimento: {1}.                                                                                        |
| BaselineRuleRegTypeInvalidError          | O valor esperado '{0}' não corresponde o tipo de valor de registo que está definido.                                                              |
| BaselineRulesetAdded                     | Conjunto de regras com o id '{0}' não foi encontrado na configuração predefinida. Não é possível adicionar o conjunto de regras.                                               |
| BaselineRulesetIdMustBeUnique            | O conjunto de regras de linha de base indicado '{0}' tem de ser exclusivo.                                                                                           |
| BaselineRulesetNotFound                  | As regras definidas com o id '{0}' e name '{1}' não foi encontrado na configuração indicada. Não é possível eliminar o conjunto de regras.                                |
| BaselineRuleSourceNotMatch               | Regra com o id '{0}' já está definida.                                                                                                       |
| BaselineRuleTypeDoesntMatch              | O tipo de regra predefinida é '{0}'.                                                                                                              |
| BaselineRuleTypeDoesntMatchError         | O tipo real da regra é: {0}, mas ruleType propriedade é: {1}.                                                                          |
| BaselineRuleUnpermittedChangesError      | São permitidas propriedades de 'expectedValue' e 'state' apenas para ser alterada.                                                                       |
| BaselineTooManyRules                     | As regras personalizadas número máximo permitido é {0} regras. A configuração especificada contém {1} regras. (regras de predefinidas {2} + {3} personalizado regras. |
| ErrorNoConfigurationStatus               | Não foi encontrado nenhum Estado de configuração. Estado o estado de configuração pretendida - 'Default' ou 'Custom'.                                    |
| ErrorNonEmptyRulesetOnDefault            | Estado de configuração está definido como default. Lista de BaselineRulesets tem de ser nulo nem estar vazio.                                                          |
| ErrorNullRulesetsPropertyOnCustom        | O estado de configuração indicado é 'Custom' mas a propriedade 'baselineRulesets' é nulo ou está vazio.                                             |
| ErrorParsingBaselineConfig               | A configuração especificada é inválida. Uma ou mais dos valores definidos tem um valor nulo ou um tipo inválido.                                  |
| ErrorParsingIsDefaultProperty            | O valor indicado 'configurationStatus' '{0}' é inválido. O valor pode ser apenas 'Default' ou 'Custom'.                                         |
| InCompatibleViewVersion                  | Versão de vista: {0} não é suportada neste tipo de área de trabalho.                                                                                   |
| InvalidBaselineConfigurationGeneralError | A configuração de linha base especificada foi encontrada com um ou mais erros de validação de tipo.                                                          |
| ViewConversionError                      | A vista é a versão mais antiga essa área de trabalho suportadas. Falha na conversão de ver: {0}                                                                 |

Se não tiver permissões suficientes, poderá receber um erro de falha geral:

![](media/security-center-customize-os-security-config/general-failure-error.png)

## <a name="next-steps"></a>Passos Seguintes
Este artigo mostrou como personalizar as avaliações de SO a configuração de segurança no Centro de segurança. Para saber mais sobre as regras de configuração e remediação, consulte:

- [Identificadores de configuração comum do Centro de segurança e regras de linha de base](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- Centro de segurança utiliza CCE (Common Configuration enumeração) para atribuir os identificadores exclusivos para as regras de configuração. Visite o [CCE](https://nvd.nist.gov/config/cce/index) site para obter mais informações.
- [Remediar configurações de segurança](security-center-remediate-os-vulnerabilities.md) mostra-lhe como resolver vulnerabilidades quando a configuração do SO não corresponde às regras de configuração de segurança recomendadas.
