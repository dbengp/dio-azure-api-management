# dio-azure-api-management 
## API com Azure API Management

### Os conceitos e resumo foram retirados da leitura das documentações oficiais: <https://learn.microsoft.com/pt-br/azure/api-management/api-management-key-concepts> e <https://learn.microsoft.com/pt-br/azure/api-management/import-app-service-as-api>

### O Gerenciamento de API do Azure é um serviço que ajuda as organizações a publicar, gerenciar, proteger e analisar APIs em escala.
- APIs: Representam conjuntos de operações que os desenvolvedores podem acessar. Cada API faz referência a um serviço de backend que a implementa, podendo ser organizada para diversos cenários de consumo.
- Produtos: Servem para expor APIs aos consumidores. Eles podem ser abertos (sem necessidade de assinatura) ou protegidos (exigindo uma chave de assinatura). Os produtos agrupam uma ou mais APIs e podem ser visíveis para grupos específicos de usuários.
- Usuários e Grupos: Os consumidores de API são usuários que podem ser criados ou convidados por administradores do serviço, ou podem se inscrever diretamente no portal do desenvolvedor. Cada usuário pertence a um ou mais grupos e pode se inscrever em produtos que concedem visibilidade às APIs associadas a esses grupos.
- Espaços de Trabalho (Workspaces): Suportam um modelo federado de gerenciamento de API. Isso permite que equipes de desenvolvimento de API descentralizadas gerenciem suas próprias APIs, enquanto uma equipe central de plataforma de API mantém a infraestrutura do Gerenciamento de API.
- Políticas: Permitem que os provedores de API modifiquem o comportamento das APIs através de configurações. As políticas são executadas sequencialmente na solicitação ou na resposta de uma API, possibilitando a aplicação de lógica como limitação de taxa, transformação de dados e autenticação.

### Importando um Serviço de Aplicativo Azure como uma API, permitindo importar facilmente um Serviço de Aplicativo (App Service) existente, transformando-o em uma API gerenciada. O processo envolve os seguintes passos:
- Navegação: No portal do Azure, acesse seu serviço de Gerenciamento de API.
- Adicionar API: No menu esquerdo, dentro da seção "APIs", selecione APIs e, em seguida, + Adicionar API.
- Seleção do Tipo: Escolha o bloco App Service.
- Localização do Serviço de Aplicativo: Selecione Procurar para ver uma lista dos Serviços de Aplicativo disponíveis em sua assinatura.
- Escolha e Confirmação: Selecione o Serviço de Aplicativo desejado e clique em Selecionar.
- Importação de Definição: Se o aplicativo web tiver uma definição OpenAPI associada, o Gerenciamento de API a buscará e importará automaticamente. Caso contrário, ele exporá a API gerando operações curinga para verbos HTTP comuns.
- Sufixo da URL da API: Adicione um sufixo exclusivo para a URL da API. Este sufixo identifica a API na instância do Gerenciamento de API e deve ser único.
- Publicação e Associação a Produto: Para que a API seja publicada e acessível aos desenvolvedores, alterne para a visualização Completa e associe a API a um Produto (por exemplo, o produto "Ilimitado").
- Criação: Selecione Criar para finalizar a importação.

### Importação de um Aplicativo Web com Definição OpenAPI
- Se o seu Aplicativo Web do Azure expõe uma definição OpenAPI, você pode importá-la para o Gerenciamento de API usando o comando az apim api import. Este é o método mais próximo da experiência do portal, que automaticamente detecta e importa definições OpenAPI. Comando básico para importação com OpenAPI:
```
az apim api import \
    --path "/seu-sufixo-api" \
    --resource-group "seu-grupo-de-recursos" \
    --service-name "sua-instancia-apim" \
    --api-id "id-da-api" \
    --display-name "Nome da Sua API" \
    --protocols https \
    --service-url "https://seu-app-service.azurewebsites.net" \
    --specification-format OpenApi \
    --specification-url "https://seu-app-service.azurewebsites.net/swagger/v1/swagger.json" \
    --description "Descrição da sua API"
```
- Explicação dos parâmetros:
  * `--path`: O sufixo da URL da API (ex: `/api-v1`).
  * `--resource-group`: O nome do grupo de recursos onde sua instância do Gerenciamento de API está localizada.
  * `--service-name`: O nome da sua instância do Gerenciamento de API.
  * `--api-id`: Um identificador exclusivo para a API dentro da instância do Gerenciamento de API.
  * `--display-name`: O nome de exibição da API no portal do desenvolvedor.
  * `--protocols`: Protocolos que a API suporta (ex: `https`).
  * `--service-url`: A URL base do seu Aplicativo Web (backend).
  * `--specification-format`: O formato da especificação da API (neste caso, `OpenApi`).
  * `--specification-url`: A URL para o arquivo de definição OpenAPI do seu Aplicativo Web (geralmente `/swagger/v1/swagger.json` ou similar).
  * `--description`: Uma descrição opcional para a API.
- Após importar a API, você também pode associá-la a um produto existente:
```
az apim product api add \
    --product-id "unlimited" \
    --api-id "id-da-api" \
    --resource-group "seu-grupo-de-recursos" \
    --service-name "sua-instancia-apim"
```














