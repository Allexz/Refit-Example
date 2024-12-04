# REFIT - Implementação em C# 
  
### Definição  ###
É uma biblioteca para **.NET** desenvolvida para facilitar o consumo de **API'S REST (Http Client)** fornecendo interfaces que abstraem a comunicação com serviços HTTP possibilitando a escrita de 
código mais conciso(limpo) e de baixo acomplamento.
É necessário adicionar o PACKAGE à solução como segue: 
```dotnet add package Refit --version 8.0.0```  
Para consumir a API do GITHUB é necessário criar seu próprio PERSONAL ACCESS TOKEN. 
---
Implementação do recurso, utilizei a classe PROGRAM: 
```
//Materializacao do objeto para utilizacao de seus valores na configuracao do HttpClient
GitHubSettings settingValue = builder.Configuration.GetSection("GitHubSettings").Get<GitHubSettings>() !;

builder.Services.AddRefitClient<IGitHubApi>(new RefitSettings
{
    //Definicao de propriedades da serializacao
    ContentSerializer = new NewtonsoftJsonContentSerializer(new JsonSerializerSettings
    {
        ContractResolver = new CamelCasePropertyNamesContractResolver(),
        NullValueHandling = NullValueHandling.Ignore
    })
}) .ConfigureHttpClient((setting, client) =>
    {
        //Configuracao do HttpClient
        client.BaseAddress = new Uri(settingValue.BaseAddress);
        client.DefaultRequestHeaders.UserAgent.ParseAdd(settingValue.AppName);
        client.DefaultRequestHeaders.Add("Authorization", settingValue.AccessToken);
        client.DefaultRequestHeaders.Add("X-GitHub-Api-Version", settingValue.GitHubHeader);
        client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/vnd.github.v3+json"));

    });
```
