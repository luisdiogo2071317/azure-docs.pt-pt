---
title: Testar módulos do Terraform no Azure com o Terratest
description: Saiba como utilizar o Terratest para testar os seus módulos do Terraform.
services: terraform
ms.service: terraform
keywords: terraform, devops, conta de armazenamento, azure, terratest, teste de unidades, teste de integração
author: JunyiYi
manager: jeconnoc
ms.author: junyi
ms.topic: tutorial
ms.date: 10/19/2018
ms.openlocfilehash: 7feee063c7b311934f7d157a9dff62d803a041b0
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638716"
---
# <a name="test-terraform-modules-in-azure-using-terratest"></a>Testar módulos do Terraform no Azure com o Terratest

Os módulos do Terraform são utilizados para criar componentes reutilizáveis, compostos e testáveis. Incorporam o encapsulamento no mundo da “infraestrutura como código”.

Tal como acontece com outros componentes de software, a garantia de qualidade desempenha um papel importante nos módulos do Terraform. Infelizmente, não há muita documentação disponível que explique como criar testes de unidades e de integração nos módulos do Terraform. Este tutorial apresenta uma infraestrutura de testes e boas práticas que adotámos quando criámos os [módulos do Terraform do Azure](https://registry.terraform.io/browse?provider=azurerm).

Depois de considerar todas as mais populares infraestruturas de testes, escolhemos o [Terratest](https://github.com/gruntwork-io/terratest). O Terratest é implementado como uma biblioteca de Go. Oferece uma coleção de funções de programa auxiliar e padrões para tarefas de testes comuns de infraestruturas, como fazer pedidos HTTP e SSH a uma máquina virtual específica. Algumas das principais vantagens do Terratest incluem:

- **Programas auxiliares práticos para verificar a infraestrutura.** Esta funcionalidade é útil para quando quer verificar a infraestrutura real no ambiente real.
- **Estrutura de pastas claramente organizada.** Os casos de teste são organizados claramente e seguem a [estrutura de pastas do módulo do Terraform padrão](https://www.terraform.io/docs/modules/create.html#standard-module-structure).
- **Todos os casos de teste são escritos em Go.** Uma vez que a maioria dos programadores de Terraform já são programadores de Go, a utilização do Terratest elimina a necessidade de aprender outra linguagem de programação. Além disso, as únicas dependências necessárias para executar casos de teste no Terratest são o Go e o Terraform.
- **Esta infraestrutura é altamente expansível.** Não é difícil expandir funções adicionais por cima do Terratest, como funcionalidades específicas do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Este guia prático não depende de nenhuma plataforma; pode ser executado no Windows, no Linux ou no MacOS. Antes de avançar, instale o software seguinte:

- **A linguagem de programação Go**: os casos de teste do Terraform são escritos em [Go](https://golang.org/dl/).
- **dep**: [dep](https://github.com/golang/dep#installation) é uma ferramenta de gestão de dependências para Go.
- **CLI do Azure**: a [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) é uma ferramenta de linha de comandos para gerir recursos do Azure. (O Terraform suporta a autenticação no Azure através de um Principal de Serviço ou [da CLI do Azure](https://www.terraform.io/docs/providers/azurerm/authenticating_via_azure_cli.html).)
- **mage**: vamos utilizar o [executável mage](https://github.com/magefile/mage/releases) para aprender a simplificar a execução dos casos do Terratest. 

## <a name="create-a-static-webpage-module"></a>Criar um módulo de página Web estática

Neste tutorial, vai criar um módulo o Terraform que aprovisiona uma página Web mediante o carregamento de um ficheiro HTML individual para o blob do armazenamento do Azure. Este módulo vai permitir que utilizadores de todo o mundo acedam a esta página Web através de um URL que o módulo devolve.

> [!NOTE]
> Todos os ficheiros descritos nesta secção devem ser criados em [GOPATH](https://github.com/golang/go/wiki/SettingGOPATH).

Primeiro, crie uma pasta nova com o nome `staticwebpage` na pasta `src` de GoPath. A estrutura de pastas geral deste tutorial é descrita abaixo. (Os ficheiros marcados com um asterisco `(*)` são o destaque principal nesta secção.)

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 main.tf      (*)
   ├ 📄 outputs.tf   (*)
   └ 📄 variables.tf (*)
```

O módulo de página Web estática aceita três entradas, que são declaradas em `./variables.tf`:

```hcl
variable "location" {
  description = "The Azure region in which all resources will be created."
}

variable "website_name" {
  description = "The website name which will be used to create a bunch of related resources in Azure."
}

variable "html_path" {
  description = "The file path of the static homepage HTML in your local filesystem."
  default     = "index.html"
}
```

Conforme mencionado acima, este módulo também produz um URL declarado em `./outputs.tf`:

```hcl
output "homepage_url" {
  value = "${azurerm_storage_blob.homepage.url}"
}
```

Isto leva-nos para a lógica principal do módulo. No total, vão ser aprovisionados quatro recursos:
- Um grupo de recursos cujo nome é a entrada `website_name` anexada por `-staging-rg`.
- Uma conta de armazenamento cujo nome é a entrada `website_name` anexada por `data001`. Contudo, para cumprir as limitações de nome da conta de armazenamento, o módulo remove todos os carateres especiais e escreve o nome todo em letras minúsculas.
- Um contentor com um nome fixo, `wwwroot`, criado na conta de armazenamento acima.
- Um ficheiro HTML individual lido a partir da entrada `html_path` e carregado para `wwwroot/index.html`.

A lógica do módulo da página Web estática é implementado em `./main.tf`:

```hcl
resource "azurerm_resource_group" "main" {
  name     = "${var.website_name}-staging-rg"
  location = "${var.location}"
}

resource "azurerm_storage_account" "main" {
  name                     = "${lower(replace(var.website_name, "/[[:^alnum:]]/", ""))}data001"
  resource_group_name      = "${azurerm_resource_group.main.name}"
  location                 = "${azurerm_resource_group.main.location}"
  account_tier             = "Standard"
  account_replication_type = "LRS"
}

resource "azurerm_storage_container" "main" {
  name                  = "wwwroot"
  resource_group_name   = "${azurerm_resource_group.main.name}"
  storage_account_name  = "${azurerm_storage_account.main.name}"
  container_access_type = "blob"
}

resource "azurerm_storage_blob" "homepage" {
  name                   = "index.html"
  resource_group_name    = "${azurerm_resource_group.main.name}"
  storage_account_name   = "${azurerm_storage_account.main.name}"
  storage_container_name = "${azurerm_storage_container.main.name}"
  source                 = "${var.html_path}"
  type                   = "block"
  content_type           = "text/html"
}
```

### <a name="unit-test"></a>Teste de unidades

O Terratest é uma ferramenta geralmente concebida para testes de integração, o que significa que aprovisiona recursos reais num ambiente real. Por vezes, alguns trabalhos podem tornar-se excecionalmente grandes, sobretudo se tiver de aprovisionar dezenas de recursos. A lógica da convenção de nomenclatura da conta de armazenamento descrita na secção anterior é um bom exemplo, no sentido em que não temos realmente de aprovisionar nenhum recurso, apenas queremos ter a certeza de que a lógica da convenção está correta.

Graças à flexibilidade do Terratest, podemos fazê-lo facilmente com os testes de unidades. Os testes de unidades são casos de testes em execução no local (embora o acesso à Internet continue a ser preciso); mediante a mera execução dos comandos `terraform init` e `terraform plan`, os casos de testes de unidades analisam a saída de `terraform plan` e procuram os valores dos atributos que vão ser comparados.

O resto desta secção descreve como utilizar o Terratest para implementar um teste de unidades para confirmar que a lógica da convenção de nomenclatura está correta. Só estamos interessados nos ficheiros marcados com asterisco `(*)`.

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html                (*)
   │   │       └ 📄 main.tf                   (*)
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go (*)
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

Primeiro, um ficheiro HTML vazio `./test/fixtures/storage-account-name/empty.html` é apenas um marcador de posição.

O ficheiro `./test/fixtures/storage-account-name/main.tf` é o esqueleto do caso de teste. Aceita uma entrada `website_name`, que também é a entrada dos testes de unidades. A respetiva lógica é apresentada aqui:

```hcl
variable "website_name" {
  description = "The name of your static website."
}

module "staticwebpage" {
  source       = "../../../"
  location     = "West US"
  website_name = "${var.website_name}"
  html_path    = "empty.html"
}
```

Por fim, o componente principal é a implementação dos testes de unidades: `./test/storage_account_name_unit_test.go`

Se for programador de Go, reconhecerá que corresponde à assinatura de uma função de teste clássica do Go, ao aceitar um argumento do tipo `*testing.T`.

No corpo do teste de unidades, temos cinco casos no total definidos na variável `testCases` (a chave é a entrada e o valor é a saída esperada). Em cada caso de teste de unidades, começamos por executar `terraform init`, visando a pasta “test fixture” (`./test/fixtures/storage-account-name/`). 

Depois, um comando `terraform plan` com a entrada do caso de teste específico (veja a definição `website_name` em `tfOptions`) guarda o resultado em `./test/fixtures/storage-account-name/terraform.tfplan` (que não está listado na estrutura de pastas geral).

Em seguida, este ficheiro de resultado é convertido numa estrutura legível por código através do analisador de planos oficial do Terraform.

Agora, vamos procurar os atributos que nos interessam (neste caso, `name` de `azurerm_storage_account`) e compará-los com a saída esperada.

```go
package test

import (
    "os"
    "path"
    "testing"

    "github.com/gruntwork-io/terratest/modules/terraform"
    terraformCore "github.com/hashicorp/terraform/terraform"
)

func TestUT_StorageAccountName(t *testing.T) {
    t.Parallel()

    // Test cases for storage account name conversion logic
    testCases := map[string]string{
        "TestWebsiteName": "testwebsitenamedata001",
        "ALLCAPS":         "allcapsdata001",
        "S_p-e(c)i.a_l":   "specialdata001",
        "A1phaNum321":     "a1phanum321data001",
        "E5e-y7h_ng":      "e5ey7hngdata001",
    }

    for input, expected := range testCases {
        // Specify test case folder and "-var" options
        tfOptions := &terraform.Options{
            TerraformDir: "./fixtures/storage-account-name",
            Vars: map[string]interface{}{
                "website_name": input,
            },
        }

        // Terraform init and plan only
        tfPlanOutput := "terraform.tfplan"
        terraform.Init(t, tfOptions)
        terraform.RunTerraformCommand(t, tfOptions, terraform.FormatArgs(tfOptions.Vars, "plan", "-out="+tfPlanOutput)...)

        // Read and parse the plan output
        f, err := os.Open(path.Join(tfOptions.TerraformDir, tfPlanOutput))
        if err != nil {
            t.Fatal(err)
        }
        defer f.Close()
        plan, err := terraformCore.ReadPlan(f)
        if err != nil {
            t.Fatal(err)
        }

        // Validate the test result
        for _, mod := range plan.Diff.Modules {
            if len(mod.Path) == 2 && mod.Path[0] == "root" && mod.Path[1] == "staticwebpage" {
                actual := mod.Resources["azurerm_storage_account.main"].Attributes["name"].New
                if actual != expected {
                    t.Fatalf("Expect %v, but found %v", expected, actual)
                }
            }
        }
    }
}
```

Para executar os testes de unidades, tem de realizar os passos seguintes na linha de comandos.

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in test cases
GoPath/src/staticwebpage$ cd test
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ az login    # Required when no service principal environment variables present
GoPath/src/staticwebpage/test$ go test -run TestUT_StorageAccountName
```

Verá o resultado do teste do Go tradicional ao fim de, aproximadamente, um minuto.

### <a name="integration-test"></a>Teste de integração

Contrariamente aos testes de unidades, os testes de integração são necessários para aprovisionar recursos num ambiente real tendo em conta a perspetiva completa. O Terratest consegue fazê-lo de forma eficaz. Uma vez que a boa prática do módulo do Terraform também recomenda a pasta `examples` que contém alguns exemplos completos, porque não testar simplesmente esses exemplos como testes de integração? Nesta secção, vamos debruçar-nos em três ficheiros, cada um marcado com um asterisco `(*)`.

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html              (*)
   │       └ 📄 main.tf                 (*)
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go (*)
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

Vamos começar com os exemplos. É criada uma pasta de exemplos nova, denominada `hello-world/`, na pasta `./examples/`. Aí, fornecemos uma página HTML simples para carregamento, `./examples/hello-world/index.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Hello World</title>
</head>
<body>
    <h1>Hi, Terraform Module</h1>
    <p>This is a sample web page to demostrate Terratest.</p>
</body>
</html>
```

O exemplo do Terraform `./examples/hello-world/main.tf` é semelhante ao que foi mostrado no teste de unidades, apenas com uma diferença significativa - também imprime o URL do HTML carregado com o nome `homepage`.

```hcl
variable "website_name" {
  description = "The name of your static website."
  default     = "Hello-World"
}

module "staticwebpage" {
  source       = "../../"
  location     = "West US"
  website_name = "${var.website_name}"
}

output "homepage" {
  value = "${module.staticwebpage.homepage_url}"
}
```

O Terratest e a função de teste clássica do Go voltam a aparecer no ficheiro de teste de integração `./test/hello_world_example_test.go`.

Ao contrário dos testes de unidades, os testes de integração criam recursos reais no Azure, motivo pelo qual tem de ter cuidado para evitar conflitos de nomes. (Tenha particular atenção a alguns nomes globalmente exclusivos, como o nome da conta de armazenamento). Consequentemente, o primeiro passo da lógica de teste é gerar um `websiteName` aleatorizado com a função `UniqueId()` fornecida pelo TerraTest. Esta função gera um nome aleatório que contém letras minúsculas, maiúsculas ou números. `tfOptions` faz todos os comandos do Terraform que visam a pasta `./examples/hello-world/` e também confirma que `website_name` está definido como o `websiteName` aleatorizado.

Em seguida, são executados `terraform init`, `terraform apply` e `terraform output`, um a um. Utilizámos outra função de programa auxiliar, `HttpGetWithCustomValidation()`, que o Terratest forneceu, para confirmar que o HTML é carregado para o URL `homepage` de saída que `terraform output` devolveu mediante a comparação do código de estado Get do HTTP com `200` e procurar algumas palavras-chave no conteúdo do HTML. Por fim, é “prometido” que se tira partido da funcionalidade `defer` do Go para executar `terraform destroy`.

```go
package test

import (
    "fmt"
    "strings"
    "testing"

    "github.com/gruntwork-io/terratest/modules/http-helper"
    "github.com/gruntwork-io/terratest/modules/random"
    "github.com/gruntwork-io/terratest/modules/terraform"
)

func TestIT_HelloWorldExample(t *testing.T) {
    t.Parallel()

    // Generate a random website name to prevent naming conflict
    uniqueID := random.UniqueId()
    websiteName := fmt.Sprintf("Hello-World-%s", uniqueID)

    // Specify test case folder and "-var" options
    tfOptions := &terraform.Options{
        TerraformDir: "../examples/hello-world",
        Vars: map[string]interface{}{
            "website_name": websiteName,
        },
    }

    // Terraform init, apply, output and destroy
    defer terraform.Destroy(t, tfOptions)
    terraform.InitAndApply(t, tfOptions)
    homepage := terraform.Output(t, tfOptions, "homepage")

    // Validate the provisioned webpage
    http_helper.HttpGetWithCustomValidation(t, homepage, func(status int, content string) bool {
        return status == 200 &&
            strings.Contains(content, "Hi, Terraform Module") &&
            strings.Contains(content, "This is a sample web page to demostrate Terratest.")
    })
}
```

Para executar os testes de integração, tem de realizar os passos seguintes na linha de comandos.

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in test cases
GoPath/src/staticwebpage$ cd test
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ az login    # Required when no service principal environment variables present
GoPath/src/staticwebpage/test$ go test -run TestIT_HelloWorldExample
```

Verá o resultado do teste do Go tradicional ao fim de, aproximadamente, dois minutos. Claro que também pode executar os testes de unidades e de integração mediante a execução de:

```shell
GoPath/src/staticwebpage/test$ go fmt
GoPath/src/staticwebpage/test$ go test
```

Como pode ver, os testes de integração demoram muito mais tempo do que os de unidades (dois minutos para um caso de integração e um minuto para cinco casos de unidades). No entanto, optar por utilizar testes de unidades ou tirar partido dos testes de integração continua a ser uma decisão sua. Geralmente, nós preferimos utilizar os testes de unidades para lógicas complexas mediante a utilização de funções HCL e utilizar os testes de integração numa perspetiva completa do utilizador.

## <a name="use-mage-to-simplify-running-terratest-cases"></a>Utilizar mage para simplificar a execução de casos do Terratest 

Como vimos, a execução de casos de testes na shell não é uma tarefa fácil, porque é preciso navegar para diretórios diferentes e executar diversos comandos. Por esse motivo, introduzimos o sistema de compilação no nosso projeto. Nesta secção, utilizamos um mage de sistema de compilação do Go para levar a cabo a tarefa.

A única coisa de que o mage precisa é de `magefile.go` no diretório de raiz do projeto (marcado com `(+)` na imagem abaixo).

```
 📁 GoPath/src/staticwebpage
   ├ 📁 examples
   │   └ 📁 hello-world
   │       ├ 📄 index.html
   │       └ 📄 main.tf
   ├ 📁 test
   │   ├ 📁 fixtures
   │   │   └ 📁 storage-account-name
   │   │       ├ 📄 empty.html
   │   │       └ 📄 main.tf
   │   ├ 📄 hello_world_example_test.go
   │   └ 📄 storage_account_name_unit_test.go
   ├ 📄 magefile.go (+)
   ├ 📄 main.tf
   ├ 📄 outputs.tf
   └ 📄 variables.tf
```

Eis um exemplo de `./magefile.go`. Neste script de compilação, escrito em Go, implementámos cinco passos de compilação:
- `Clean`: este passo remove todos os ficheiros gerados/temporários durante as execuções de testes.
- `Format`: este passo executa `terraform fmt` e `go fmt` para formatar a base do código.
- `Unit`: este passo executa todos os testes de unidades (mediante a utilização da convenção de nomes `TestUT_*`) na pasta `./test/`.
- `Integration`: semelhante a `Unit`, mas em vez de testes de unidades, executa os testes de integração (`TestIT_*`).
- `Full`: este passo executa `Clean`, `Format`, `Unit', and ` e Integration`, sequencialmente.

```go
// +build mage

// Build script to format and run tests of a Terraform module project.
package main

import (
    "fmt"
    "os"
    "path/filepath"

    "github.com/magefile/mage/mg"
    "github.com/magefile/mage/sh"
)

// Default target when execute `mage` in shell
var Default = Full

// A build step that runs Clean, Format, Unit and Integration in sequence
func Full() {
    mg.Deps(Unit)
    mg.Deps(Integration)
}

// A build step that runs unit tests
func Unit() error {
    mg.Deps(Clean)
    mg.Deps(Format)
    fmt.Println("Running unit tests...")
    return sh.RunV("go", "test", "./test/", "-run", "TestUT_", "-v")
}

// A build step that runs integration tests
func Integration() error {
    mg.Deps(Clean)
    mg.Deps(Format)
    fmt.Println("Running integration tests...")
    return sh.RunV("go", "test", "./test/", "-run", "TestIT_", "-v")
}

// A build step that formats both Terraform code and Go code
func Format() error {
    fmt.Println("Formatting...")
    if err := sh.RunV("terraform", "fmt", "."); err != nil {
        return err
    }
    return sh.RunV("go", "fmt", "./test/")
}

// A build step that removes temporary build/test files
func Clean() error {
    fmt.Println("Cleaning...")
    return filepath.Walk(".", func(path string, info os.FileInfo, err error) error {
        if err != nil {
            return err
        }
        if info.IsDir() && info.Name() == "vendor" {
            return filepath.SkipDir
        }
        if info.IsDir() && info.Name() == ".terraform" {
            os.RemoveAll(path)
            fmt.Printf("Removed \"%v\"\n", path)
            return filepath.SkipDir
        }
        if !info.IsDir() && (info.Name() == "terraform.tfstate" ||
            info.Name() == "terraform.tfplan" ||
            info.Name() == "terraform.tfstate.backup") {
            os.Remove(path)
            fmt.Printf("Removed \"%v\"\n", path)
        }
        return nil
    })
}
```

Tal como nos passos de execução anteriores, pode utilizar os comandos seguintes para executar um conjunto de testes completo:

```shell
$ cd [Your GoPath]/src/staticwebpage
GoPath/src/staticwebpage$ dep init    # Run only once for this folder
GoPath/src/staticwebpage$ dep ensure  # Required to run if you imported new packages in magefile or test cases
GoPath/src/staticwebpage$ go fmt      # Only requied when you change the magefile
GoPath/src/staticwebpage$ az login    # Required when no service principal environment variables present
GoPath/src/staticwebpage$ mage
```

Pode substituir à vontade a última linha de comandos por qualquer passo do mage, como, por exemplo, `mage unit` ou `mage clean`. Agora, talvez esteja a pensar que ainda há muitas linhas de comandos aqui e que pode ser boa ideia incorporar os comandos `dep`, bem como `az login`, no magefile. Mas ainda não mostramos o código aqui. Outro passo da utilização do mage é que os passos podem ser partilhados mediante a utilização do sistema de pacotes do Go. Assim, os magefiles em todos os seus módulos podem ser simplificados ao simplesmente referenciar uma implementação comum e declarar dependências (`mg.Deps()`).

> [!NOTE]
> **Opção: definir variáveis de ambiente do principal de serviço para executar testes de aceitação**
> 
> Em vez de executar `az login` antes dos testes, pode definir as variáveis de ambiente do principal de serviço para realizar a autenticação do Azure. O Terraform publica [uma lista de nomes de variáveis de ambiente](https://www.terraform.io/docs/providers/azurerm/index.html#testing). (Só são necessárias as quatro primeiras variáveis de ambiente.) O Terraform também publica instruções detalhadas que explicam como [obter o valor destas variáveis de ambiente.](https://www.terraform.io/docs/providers/azurerm/authenticating_via_service_principal.html)

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre o Terratest, veja a [respetiva página do GitHub](https://github.com/gruntwork-io/terratest). Pode encontrar algumas informações úteis sobre o mage na [página do GitHub](https://github.com/magefile/mage) e [na home page](https://magefile.org/) do mage.
