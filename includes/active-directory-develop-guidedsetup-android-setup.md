
## <a name="set-up-your-project"></a>Configurar o projeto

Pretende transferir o projeto do Android Studio este exemplo em vez disso? [Transferir um projeto de](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip)e avance para o [passo da configuração](#create-an-application-express) para configurar o exemplo de código antes de executá-lo.

### <a name="create-a-new-project"></a>Criar um novo projeto 
1.  Abra o Android Studio e, em seguida, selecione **ficheiro** > **novo** > **novo projeto**.
2.  Nome da aplicação e, em seguida, selecione **seguinte**.
3.  Selecione **API 21 ou mais recente (Android 5.0)**e, em seguida, selecione **seguinte**.
4.  Deixe **atividade vazia** como está, selecione **seguinte**e, em seguida, selecione **concluir**.


### <a name="add-msal-to-your-project"></a>Adicionar MSAL ao seu projeto
1.  No Android Studio, selecione **Gradle Scripts** > **gradle (módulo: aplicação)**.
2.  Em **dependências**, cole o seguinte código:

    ```ruby  
    compile ('com.microsoft.identity.client:msal:0.1.+') {
        exclude group: 'com.android.support', module: 'appcompat-v7'
    }
    compile 'com.android.volley:volley:1.0.0'
    ```

<!--start-collapse-->
### <a name="about-this-package"></a>Sobre este pacote

O pacote do código anterior instala a biblioteca de autenticação da Microsoft. MSAL processa a aquisição, colocação em cache e atualizar os tokens de utilizador que são utilizados para aceder o APIs que estão protegidos pelo ponto final do Azure Active Directory v2.
<!--end-collapse-->

## <a name="create-the-application-ui"></a>Criar a IU da aplicação

1. Aceda a **res** > **esquema**e, em seguida, abra **ctivity_main.XML**. 
2. Alterar o esquema de atividade do `android.support.constraint.ConstraintLayout` ou outros para `LinearLayout`.
3. Adicionar o `android:orientation="vertical"` propriedade para o `LinearLayout` nós.
4. Cole o seguinte código para o `LinearLayout` nó, substituindo o conteúdo atual:

    ```xml
    <TextView
        android:text="Welcome, "
        android:textColor="#3f3f3f"
        android:textSize="50px"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="10dp"
        android:layout_marginTop="15dp"
        android:id="@+id/welcome"
        android:visibility="invisible"/>

    <Button
        android:id="@+id/callGraph"
        android:text="Call Microsoft Graph"
        android:textColor="#FFFFFF"
        android:background="#00a1f1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="200dp"
        android:textAllCaps="false" />

    <TextView
        android:text="Getting Graph Data..."
        android:textColor="#3f3f3f"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"
        android:id="@+id/graphData"
        android:visibility="invisible"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dip"
        android:layout_weight="1"
        android:gravity="center|bottom"
        android:orientation="vertical" >

        <Button
            android:text="Sign Out"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="15dp"
            android:textColor="#FFFFFF"
            android:background="#00a1f1"
            android:textAllCaps="false"
            android:id="@+id/clearCache"
            android:visibility="invisible" />
    </LinearLayout>
    ```
