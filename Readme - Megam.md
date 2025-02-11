# API de Listagem de Robôs de MegaMan

Este é um projeto de API construído em .NET, com o objetivo de fornecer uma lista de robôs de MegaMan em formato JSON. A API tem como principal função disponibilizar os dados dos robôs de MegaMan para sistemas que precisam consumir essa informação.

## Estrutura do JSON

A API retorna os dados dos robôs no seguinte formato:

```json
{
  "Id": 1,
  "Code": "DLN/DRN-003",
  "Name": "Cutman",
  "HP": 150,
  "Picture": "https://vignette.wikia.nocookie.net/megaman/images/2/22/Cutman.png"
}
```

## Tecnologias Usadas

O projeto utiliza as seguintes tecnologias:

- **.NET Core 3.1** para o desenvolvimento da API.
- **Entity Framework Core** para interação com o banco de dados.
- **Newtonsoft.Json** para manipulação de dados JSON.

### Dependências

- [Microsoft.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore) - Versão 3.1.8
- [Microsoft.EntityFrameworkCore.Design](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Design) - Versão 3.1.8
- [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer) - Versão 3.1.8
- [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json) - Versão 12.0.2

## Estrutura do Projeto

O projeto segue a estrutura padrão de um aplicativo .NET Core Web API com o SDK:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>netcoreapp3.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore" Version="3.1.8" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.1.8">
      <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
      <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="3.1.8" />
    <PackageReference Include="Newtonsoft.Json" Version="12.0.2" />
  </ItemGroup>
</Project>
```

## Endpoints

A API possui os seguintes endpoints:

### 1. **GET /api/v1/robots**

Este endpoint retorna a lista de todos os robôs disponíveis.

- **URL**: `/api/v1/robots`
- **Método**: GET
- **Resposta de Sucesso**:
    - **Código de Status**: 200 OK
    - **Corpo da Resposta**: Lista de objetos JSON com informações dos robôs.

Exemplo de resposta:

```json
[
  {
    "Id": 1,
    "Code": "DLN/DRN-003",
    "Name": "Cutman",
    "HP": 150,
    "Picture": "https://vignette.wikia.nocookie.net/megaman/images/2/22/Cutman.png"
  },
  ...
]
```

### 2. **GET /api/v1/robots/{id}**

Este endpoint retorna os dados de um robô específico com base no `id`.

- **URL**: `/api/v1/robots/{id}`
- **Método**: GET
- **Parâmetros**: 
    - `id` (int): ID do robô a ser retornado.
- **Resposta de Sucesso**:
    - **Código de Status**: 200 OK
    - **Corpo da Resposta**: Objeto JSON com as informações do robô.

Exemplo de resposta:

```json
{
  "Id": 1,
  "Code": "DLN/DRN-003",
  "Name": "Cutman",
  "HP": 150,
  "Picture": "https://vignette.wikia.nocookie.net/megaman/images/2/22/Cutman.png"
}
```

- **Resposta de Erro**:
    - **Código de Status**: 404 Not Found
    - **Corpo da Resposta**:

```json
{
  "message": "Nenhum robô encontrado"
}
```

### 3. **POST /api/v1/robots**

Este endpoint permite o envio de dados para adicionar um novo robô.

- **URL**: `/api/v1/robots`
- **Método**: POST
- **Resposta de Sucesso**:
    - **Código de Status**: 200 OK
    - **Corpo da Resposta**: Confirmação de sucesso.

## Estrutura de Código

A API é composta pela classe `RobotsController`, que define as rotas e lida com as requisições.

```csharp
[ApiController]
[Route("api/v1/robots")]
public class RobotsController : ControllerBase
{
    private readonly IRobotServices _services;

    public RobotsController(IRobotServices services)
    {
        _services = services;
    }

    // GET api/robots
    [HttpGet]
    public ActionResult<IEnumerable<RobotReadDTO>> GetAllRobots()
    {
        var robotItems = _services.SearchAll();
        return Ok(robotItems);
    }

    // GET api/v1/robots/{id}
    [HttpGet]
    [Route("{id:int}")]
    public object GetCommandById([FromRoute] int id)
    {
        var robot = _services.SearchById(id);
        
        if (robot != null)
            return Ok(robot);
        
        return NotFound(new { message = "Nenhum robô encontrado" });
    }

    // POST api/v1/robots
    [HttpPost]
    public ActionResult RobotSend()
    {
        return Ok();
    }
}
```

## Instruções de Execução

Para rodar o projeto, siga as etapas abaixo:

1. Clone o repositório:
    ```bash
    git clone <url_do_repositorio>
    ```

2. Navegue até a pasta do projeto:
    ```bash
    cd <nome_da_pasta>
    ```

3. Restaure os pacotes NuGet:
    ```bash
    dotnet restore
    ```

4. Execute o projeto:
    ```bash
    dotnet run
    ```

5. A API estará rodando na URL padrão `http://localhost:5000`.

## Contribuindo

Sinta-se à vontade para contribuir com melhorias no código ou na documentação. Se encontrar algum erro ou tiver sugestões de melhoria, abra uma **issue** ou envie um **pull request**.

## Licença

Este projeto está licenciado sob a [MIT License](https://opensource.org/licenses/MIT).
```