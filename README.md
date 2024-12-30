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
using Microsoft.SemanticKernel.Connectors.AI.Ollama;

class Program
{
    static async Task Main(string[] args)
    {
        Console.WriteLine("Starting chat with Ollama model...");

        // Initialize Semantic Kernel
        var kernel = new KernelBuilder().WithOllamaChatModel("phi3:mini").Build();

        // Start the conversation
        while (true)
        {
            Console.Write("You: ");
            string userInput = Console.ReadLine();

            if (string.IsNullOrWhiteSpace(userInput) || userInput.Equals("exit", StringComparison.OrdinalIgnoreCase))
            {
                Console.WriteLine("Exiting chat. Goodbye!");
                break;
            }

            var response = await kernel.GetResponseAsync(userInput);
            Console.WriteLine($"Ollama: {response}");
        }
    }
}
```

## Running the Application

1. Build and run the application:
   ```bash
   dotnet run
   ```

2. Interact with the `phi3:mini` model by entering text prompts. Type `exit` to end the chat.

## GitHub README Content

### Ollama Chat Integration

Integrate Ollama models with .NET 8 Console App using the Semantic Kernel.

#### Prerequisites
- Install [Ollama](https://ollama.com/download).
- .NET SDK 8.0 or later.

#### Setup
```bash
dotnet new console -n OllamaChatApp
cd OllamaChatApp

dotnet add package Microsoft.SemanticKernel --version 1.24.1
dotnet add package Microsoft.SemanticKernel.Connectors.Ollama --version 1.24.1-alpha
```

#### Program.cs
```csharp
#pragma warning disable SKEXP0070

using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.Connectors.AI.Ollama;

class Program
{
    static async Task Main(string[] args)
    {
        var kernel = new KernelBuilder().WithOllamaChatModel("phi3:mini").Build();

        while (true)
        {
            Console.Write("You: ");
            string userInput = Console.ReadLine();

            if (string.IsNullOrWhiteSpace(userInput) || userInput.Equals("exit", StringComparison.OrdinalIgnoreCase))
                break;

            var response = await kernel.GetResponseAsync(userInput);
            Console.WriteLine($"Ollama: {response}");
        }
    }
}
```

#### Run the App
```bash
dotnet run
```

