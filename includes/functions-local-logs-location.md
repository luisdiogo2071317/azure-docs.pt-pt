Quando o anfitrião de funções é executada localmente, escreve os registos para o seguinte caminho:

```
<DefaultTempDirectory>\LogFiles\Application\Functions
```

No Windows, `<DefaultTempDirectory>` primeiro valor encontrado o USERPROFILE TMP, TEMP, variáveis de ambiente ou diretório do Windows.
No MacOS ou Linux, `<DefaultTempDirectory>` é a variável de ambiente de TMPDIR.

[!Note]
Quando o anfitrião de funções é iniciado, substituirá a estrutura de ficheiros existentes no diretório.