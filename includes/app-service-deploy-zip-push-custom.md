## <a name="deployment-customization"></a>Personalização da implementação

O processo de implementação parte do princípio de que o ficheiro. zip que push contém uma aplicação preparada para execução. Por predefinição, são executadas sem personalizações. Pode ativar os processos de compilação que obter com a integração contínua adicionando o seguinte para as definições da aplicação:

    SCM_DO_BUILD_DURING_DEPLOYMENT=true 

Quando utilizar a implementação de push. zip, esta definição é **falso** por predefinição. A predefinição é **verdadeiro** para implementações de integração contínua. Quando definido como **verdadeiro**, as definições de implementação são utilizadas durante a implementação. Estas definições podem ser definidas como as definições de aplicação ou num `.deployment` ficheiro de configuração localizado na raiz do ficheiro zip. Para obter mais informações, consulte [repositório e definições relacionadas com a implementação](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings) de referência de implementação.