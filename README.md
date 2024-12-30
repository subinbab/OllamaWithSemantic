# Ollama Chat Integration with .NET 8 Console App

This guide will walk you through integrating and using Ollama models with a .NET 8 console application. We'll use the Microsoft Semantic Kernel library and the Ollama connector to communicate with the `phi3:mini` model.

## Prerequisites

1. Install [Ollama](https://ollama.com/download).
   - Follow the installation instructions for your platform.
   - After installation, test the setup by running the following command in your terminal:
     ```bash
     ollama run phi3:mini
     ```
     This ensures the `phi3:mini` model is installed and ready.

2. Ensure you have .NET SDK 8.0 or later installed.

3. Create a GitHub repository for the project if needed.

## Setting Up the Project

1. **Create a New .NET Console App**
   ```bash
   dotnet new console -n OllamaChatApp
   cd OllamaChatApp
   ```

2. **Add Necessary Packages**
   Add the following packages to your project:
   ```bash
   dotnet add package Microsoft.SemanticKernel --version 1.24.1
   dotnet add package Microsoft.SemanticKernel.Connectors.Ollama --version 1.24.1-alpha
   ```

3. **Disable SKEXP0070 Warning**
   Add the following directive at the top of your `Program.cs` file to suppress the `SKEXP0070` warning:
   ```csharp
   #pragma warning disable SKEXP0070
   ```

## Implementing Chat with Ollama

Update the `Program.cs` file as follows:

```csharp
using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.ChatCompletion;
#pragma warning disable SKEXP0070

var builder = Kernel.CreateBuilder();
builder.AddOllamaChatCompletion("phi3:mini", new Uri("http://localhost:11434"));

var kernel = builder.Build();
var chatService = kernel.GetRequiredService<IChatCompletionService>();

var history = new ChatHistory();
history.AddSystemMessage("You are a helpful assistant.");

while (true)
{
    Console.Write("You: ");
    var userMessage = Console.ReadLine();

    if (string.IsNullOrWhiteSpace(userMessage))
    {
        break;
    }

    history.AddUserMessage(userMessage);

    var response = await chatService.GetChatMessageContentAsync(history);

    Console.WriteLine($"Bot: {response.Content}");

    history.AddMessage(response.Role, response.Content ?? string.Empty);
}
```
