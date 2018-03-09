
## <a name="add-the-applications-registration-information-to-your-app"></a>Adicionar informações de registo da aplicação para a sua aplicação

Neste passo, terá de adicionar o ID da aplicação ao seu projeto:

1.  No `ViewController.swift`, substitua a linha que começa com '`let kClientID`' com:
```swift
let kClientID = "[Enter the application Id here]"
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Controlar + clique <code>Info.plist</code> trazer o menu de nível contextual das e, em seguida, clique em: <code>Open As</code> > <code>Source Code</code>
</li>
<li>
Sob o <code>dict</code> nó raiz, adicione o seguinte:
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Enter the application Id here]</string>
            <string>auth</string>
        </array>
    </dict>
</array>
```
