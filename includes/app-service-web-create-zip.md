## <a name="create-a-project-zip-file"></a>Criar um ficheiro ZIP do projeto

Certifique-se de que ainda está no diretório de raiz do projeto de exemplo. Crie um arquivo ZIP de tudo no seu projeto. O comando seguinte utiliza a ferramenta predefinida no seu terminal:

```
# Bash
zip -r myAppFiles.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath myAppFiles.zip
``` 

Mais tarde, carregue este ficheiro ZIP para o Azure e implemente-o no Serviço de Aplicações.