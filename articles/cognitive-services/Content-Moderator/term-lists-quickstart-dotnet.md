---
title: Moderada com listas de termos personalizado no Azure Content Moderator | Documentos da Microsoft
description: Como a moderadas com termo personalizado apresenta uma lista com o SDK de moderador de conteúdos do Azure para .NET.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/11/2018
ms.author: sajagtap
ms.openlocfilehash: 2ae080518a9ad78552a8ec173e7f4d70085c7a6b
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44022645"
---
# <a name="moderate-with-custom-term-lists-in-net"></a>Moderar com listas de termos personalizado no .NET

A lista global de padrão de termos no Azure Content Moderator é suficiente para as necessidades de moderação mais conteúdas. No entanto, poderá ter de o ecrã para termos que são específicos para sua organização. Por exemplo, pode querer nomes de concorrência de etiquetas para examinar detalhadamente. 

Pode utilizar o [conteúdo de moderador de SDK para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) para criar listas personalizadas de termos para utilizar com a API de moderação de texto.

> [!NOTE]
> Existe um limite máximo de **apresenta uma lista de 5 termo** cada lista para **não pode exceder 10 000 termos**.
>

Este artigo fornece informações e exemplos de código para ajudá-lo a começar a utilizar o SDK de moderador de conteúdo para o .NET para:
- Crie uma lista.
- Adicione termos a uma lista.
- Termos de tela contra os termos numa lista.
- Elimine termos de uma lista.
- Elimine uma lista.
- Edite informações da lista.
- Atualize o índice para que as alterações à lista estão incluídas numa nova análise.

Este artigo pressupõe que já está familiarizado com o Visual Studio e c#.

## <a name="sign-up-for-content-moderator-services"></a>Inscreva-se para os serviços do Content Moderator

Antes de poder utilizar os serviços de Content Moderator através da API REST ou o SDK, precisa de uma chave de subscrição.

No Dashboard de moderador de conteúdo, pode encontrar a chave de subscrição **configurações** > **credenciais** > **API**  >  **Avaliação Ocp-Apim-Subscription-Key**. Para obter mais informações, consulte [descrição geral](overview.md).

## <a name="create-your-visual-studio-project"></a>Criar o seu projeto do Visual Studio

1. Adicionar um novo **aplicação de consola (.NET Framework)** projeto à sua solução.

1. Nomeie o projeto **TermLists**. Selecione este projeto como o projeto de arranque único para a solução.

1. Adicionar uma referência para o **ModeratorHelper** projeto assembly que criou no [guia de introdução do Content Moderator cliente auxiliar](content-moderator-helper-quickstart-dotnet.md).

### <a name="install-required-packages"></a>Instalar pacotes necessários

Instale os seguintes pacotes de NuGet para o projeto de TermLists:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Microsoft.Rest.ClientRuntime.Azure
- Newtonsoft

### <a name="update-the-programs-using-statements"></a>O programa de atualização usando instruções

Modifique o programa usando instruções.

    using System;
    using System.Threading;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;

### <a name="add-private-properties"></a>Adicionar propriedades privadas

Adicione as seguintes propriedades privadas ao espaço de nomes TermLists, classe Program.

    /// <summary>
    /// The language of the terms in the term lists.
    /// </summary>
    private const string lang = "eng";

    /// <summary>
    /// The minimum amount of time, in milliseconds, to wait between calls
    /// to the Content Moderator APIs.
    /// </summary>
    private const int throttleRate = 3000;

    /// <summary>
    /// The number of minutes to delay after updating the search index before
    /// performing image match operations against a the list.
    /// </summary>
    private const double latencyDelay = 0.5;

## <a name="create-a-term-list"></a>Criar uma lista de termos

Crie uma lista de termo com **ContentModeratorClient.ListManagementTermLists.Create**. O primeiro parâmetro para **criar** é uma cadeia que contém um tipo MIME, o que deve ser "application/json". Para obter mais informações, consulte a [referência da API](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f). O segundo parâmetro é um **corpo** objeto que contém um nome e descrição para a lista de novo termo.

Adicione a seguinte definição de método ao espaço de nomes TermLists, classe Program.

> [!NOTE]
> A chave de serviço do Content Moderator tem um pedidos por segundo limite de taxa (RPS) e, se exceder o limite, o SDK lançará uma exceção com um código de 429 erro. 
>
> Uma chave de escalão gratuito tem um limite de taxa de um RPS.

    /// <summary>
    /// Creates a new term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <returns>The term list ID.</returns>
    static string CreateTermList (ContentModeratorClient client)
    {
        Console.WriteLine("Creating term list.");

        Body body = new Body("Term list name", "Term list description");
        TermList list = client.ListManagementTermLists.Create("application/json", body);
        if (false == list.Id.HasValue)
        {
            throw new Exception("TermList.Id value missing.");
        }
        else
        {
            string list_id = list.Id.Value.ToString();
            Console.WriteLine("Term list created. ID: {0}.", list_id);
            Thread.Sleep(throttleRate);
            return list_id;
        }
    }

## <a name="update-term-list-name-and-description"></a>Atualizar o termo lista nome e descrição

Atualizar as informações da lista com o termo **ContentModeratorClient.ListManagementTermLists.Update**. O primeiro parâmetro para **atualização** é o ID de lista de termo. O segundo parâmetro é um tipo MIME, o que deve ser "application/json". Para obter mais informações, consulte a [referência da API](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f685). O terceiro parâmetro é um **corpo** objeto, que contém o novo nome e descrição.

Adicione a seguinte definição de método ao espaço de nomes TermLists, classe Program.

    /// <summary>
    /// Update the information for the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to update.</param>
    /// <param name="name">The new name for the term list.</param>
    /// <param name="description">The new description for the term list.</param>
    static void UpdateTermList (ContentModeratorClient client, string list_id, string name = null, string description = null)
    {
        Console.WriteLine("Updating information for term list with ID {0}.", list_id);
        Body body = new Body(name, description);
        client.ListManagementTermLists.Update(list_id, "application/json", body);
        Thread.Sleep(throttleRate);
    }

## <a name="add-a-term-to-a-term-list"></a>Adicionar um termo para uma lista de termos

Adicione a seguinte definição de método ao espaço de nomes TermLists, classe Program.

    /// <summary>
    /// Add a term to the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to update.</param>
    /// <param name="term">The term to add to the term list.</param>
    static void AddTerm (ContentModeratorClient client, string list_id, string term)
    {
        Console.WriteLine("Adding term \"{0}\" to term list with ID {1}.", term, list_id);
        client.ListManagementTerm.AddTerm(list_id, term, lang);
        Thread.Sleep(throttleRate);
    }

## <a name="get-all-terms-in-a-term-list"></a>Obter todos os termos de uma lista de termo

Adicione a seguinte definição de método ao espaço de nomes TermLists, classe Program.

    /// <summary>
    /// Get all terms in the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to get all terms.</param>
    static void GetAllTerms(ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Getting terms in term list with ID {0}.", list_id);
        Terms terms = client.ListManagementTerm.GetAllTerms(list_id, lang);
        TermsData data = terms.Data;
        foreach (TermsInList term in data.Terms)
        {
            Console.WriteLine(term.Term);
        }
        Thread.Sleep(throttleRate);
    }

## <a name="add-code-to-refresh-the-search-index"></a>Adicionar código para atualizar o índice de pesquisa

Depois de efetuar alterações a uma lista de termo, atualiza seu índice de pesquisa para que as alterações a serem incluídos na próxima vez que utilize a lista de termo em texto do ecrã. Isto é semelhante à forma como um mecanismo de pesquisa no ambiente de trabalho (se ativada) ou um mecanismo de pesquisa web continuamente atualiza seu índice para incluir novos ficheiros ou páginas.

Atualizar um índice de pesquisa da lista de termo com **ContentModeratorClient.ListManagementTermLists.RefreshIndexMethod**.

Adicione a seguinte definição de método ao espaço de nomes TermLists, classe Program.

    /// <summary>
    /// Refresh the search index for the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to refresh.</param>
    static void RefreshSearchIndex (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Refreshing search index for term list with ID {0}.", list_id);
        client.ListManagementTermLists.RefreshIndexMethod(list_id, lang);
        Thread.Sleep((int)(latencyDelay * 60 * 1000));
    }

## <a name="screen-text-using-a-term-list"></a>Texto de tela usando uma lista de termos

Ecrã o texto usando uma lista de termo com **ContentModeratorClient.TextModeration.ScreenText**, que utiliza os seguintes parâmetros.

- O idioma dos termos na lista de termo.
- Um tipo MIME, que pode ser "text/html", "texto/xml", "text/markdown" ou "text/plain".
- O texto a tela.
- Um valor booleano. Defina este campo para **true** a autocorreção o texto antes do bloqueio-lo.
- Um valor booleano. Defina este campo para **true** para detetar informações de identificação pessoal (PII) no texto.
- O ID de lista de termo.

Para obter mais informações, consulte a [referência da API](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f).

**ScreenText** retorna um **ecrã** objeto, que tem uma **termos** esse Content Moderator detetada no bloqueio de termos de propriedade que apresenta uma lista de qualquer um. Observe que, se o Content Moderator não detetou quaisquer termos durante o bloqueio, o **termos** vlastnost má hodnotu **nulo**.

Adicione a seguinte definição de método ao espaço de nomes TermLists, classe Program.

    /// <summary>
    /// Screen the indicated text for terms in the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to use to screen the text.</param>
    /// <param name="text">The text to screen.</param>
    static void ScreenText (ContentModeratorClient client, string list_id, string text)
    {
        Console.WriteLine("Screening text: \"{0}\" using term list with ID {1}.", text, list_id);
        Screen screen = client.TextModeration.ScreenText(lang, "text/plain", text, false, false, list_id);
        if (null == screen.Terms)
        {
            Console.WriteLine("No terms from the term list were detected in the text.");
        }
        else
        {
            foreach (DetectedTerms term in screen.Terms)
            {
                Console.WriteLine(String.Format("Found term: \"{0}\" from list ID {1} at index {2}.", term.Term, term.ListId, term.Index));
            }
        }
        read.Sleep(throttleRate);
    }

## <a name="delete-terms-and-lists"></a>Eliminar termos e listas

A eliminar um termo ou uma lista é simples. Utilize o SDK para efetuar as seguintes tarefas:

- Elimine um termo. (**ContentModeratorClient.ListManagementTerm.DeleteTerm**)
- Elimine todas as condições numa lista sem a eliminação da lista. (**ContentModeratorClient.ListManagementTerm.DeleteAllTerms**)
- Elimine uma lista e todos os respetivos conteúdos. (**ContentModeratorClient.ListManagementTermLists.Delete**)

### <a name="delete-a-term"></a>Eliminar um termo

Adicione a seguinte definição de método ao espaço de nomes TermLists, classe Program.

    /// <summary>
    /// Delete a term from the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to delete the term.</param>
    /// <param name="term">The term to delete.</param>
    static void DeleteTerm (ContentModeratorClient client, string list_id, string term)
    {
        Console.WriteLine("Removed term \"{0}\" from term list with ID {1}.", term, list_id);
        client.ListManagementTerm.DeleteTerm(list_id, term, lang);
        Thread.Sleep(throttleRate);
    }

### <a name="delete-all-terms-in-a-term-list"></a>Eliminar todos os termos de uma lista de termo

Adicione a seguinte definição de método ao espaço de nomes TermLists, classe Program.

    /// <summary>
    /// Delete all terms from the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to delete all terms.</param>
    static void DeleteAllTerms (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Removing all terms from term list with ID {0}.", list_id);
        client.ListManagementTerm.DeleteAllTerms(list_id, lang);
        Thread.Sleep(throttleRate);
    }

### <a name="delete-a-term-list"></a>Eliminar uma lista de termo

Adicione a seguinte definição de método ao espaço de nomes TermLists, classe Program.

    /// <summary>
    /// Delete the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to delete.</param>
    static void DeleteTermList (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Deleting term list with ID {0}.", list_id);
        client.ListManagementTermLists.Delete(list_id);
        Thread.Sleep(throttleRate);
    }

## <a name="putting-it-all-together"></a>Juntando as peças

Adicionar a **Main** definição de método ao espaço de nomes TermLists, classe de programa. Por fim, feche a classe de programa e o espaço de nomes de TermLists.

    static void Main(string[] args)
    {
        using (var client = Clients.NewClient())
        {
            string list_id = CreateTermList(client);

            UpdateTermList(client, list_id, "name", "description");
            AddTerm(client, list_id, "term1");
            AddTerm(client, list_id, "term2");

            GetAllTerms(client, list_id);

            // Always remember to refresh the search index of your list
            RefreshSearchIndex(client, list_id);

            string text = "This text contains the terms \"term1\" and \"term2\".";
            ScreenText(client, list_id, text);

            DeleteTerm(client, list_id, "term1");

            // Always remember to refresh the search index of your list
            RefreshSearchIndex(client, list_id);

            text = "This text contains the terms \"term1\" and \"term2\".";
            ScreenText(client, list_id, text);

            DeleteAllTerms(client, list_id);
            DeleteTermList(client, list_id);

            Console.WriteLine("Press ENTER to close the application.");
            Console.ReadLine();
        }
    }

## <a name="run-the-application-to-see-the-output"></a>Executar a aplicação para ver a saída

A saída será nas seguintes linhas, mas os dados podem variar.

    Creating term list.
    Term list created. ID: 252.
    Updating information for term list with ID 252.
    
    Adding term "term1" to term list with ID 252.
    Adding term "term2" to term list with ID 252.
    
    Getting terms in term list with ID 252.
    term1
    term2
    
    Refreshing search index for term list with ID 252.
    
    Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
    Found term: "term1" from list ID 252 at index 32.
    Found term: "term2" from list ID 252 at index 46.
    
    Removed term "term1" from term list with ID 252.
    
    Refreshing search index for term list with ID 252.
    
    Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
    Found term: "term2" from list ID 252 at index 46.
    
    Removing all terms from term list with ID 252.
    Deleting term list with ID 252.
    Press ENTER to close the application.
    
## <a name="next-steps"></a>Passos Seguintes

Obter o [SDK de .NET do conteúdo moderador](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) e o [solução do Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para esse e outros guias de introdução do Content Moderator para .NET e começar a trabalhar com sua integração.
