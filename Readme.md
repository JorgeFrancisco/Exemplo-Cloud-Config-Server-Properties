📌 Spring Cloud Config - Centralizando Configurações (Java 8+)

Em um exemplo anterior apresentei boas práticas com @ConfigurationProperties no Spring Boot (Java 17+), neste, abordo um pouco sobre Spring Cloud Config


🚀 O que é o Spring Cloud Config?

O Spring Cloud Config fornece um servidor central de configuração, geralmente integrado a um repositório Git, permitindo que aplicações cliente carreguem suas configurações de forma remota, padronizada e segura.


✅ Benefícios:

- Centralização das configurações
- Separação entre código e configuração
- Suporte a múltiplos ambientes (dev, test, prod)
- Suporte à criptografia de propriedades sensíveis ({cipher})

🚀 Vamos pra prática?

📁 Crie um repo (Azure, Gitlab, GitHub, etc) só para os arquivos properties (ou yml) das aplicações com a seguinte estrutura de pastas (SE ESTÁ LENDO ESTE README NO GIT, JÁ ESTÀ NO REPO DOS PROPERTIES):

├── properties/
│   └── NOME_DA_APLICACAO
│         └── PROFILE

Onde NOME_DA_APLICACAO deve ser IGUAL ao que está no properties da aplicação em spring.application.name. E PROFILE deve ser o profile do properties de acordo com o ambiente: dev, hml (staging) ou prod

Para este exemplo ficará assim:

├── properties/
│   └── cloudconfigpropclient
│         └── development
│         └── staging
│         └── production

🛠️ Copie o properties da aplicação para dentro da pasta de acordo com os valores para cada profile (ambiente):

├── properties/
│   └── cloudconfigpropclient
│         └── development
│               └── application.properties
│         └── staging
│         └── production

🔑 Para criptografar dados sensíveis sem precisar enviar arquivo nenhum, guarde também a chave pública (.pem) da aplicação na pasta de cada ambiente, com o nome gerado pelo Config Server (NOME_DA_APLICACAO-client-key-PROFILE.pem):

├── properties/
│   └── cloudconfigpropclient
│         └── development
│               └── application.properties
│               └── cloudconfigpropclient-client-key-development.pem

Assim, basta informar a aplicação e o ambiente no endpoint /certificate/encrypt do Config Server. A chave pública não é segredo e pode ficar no Git. O Config Server só carrega como properties os arquivos application*.properties (ou yml), então o .pem não aparece na configuração da aplicação.

⚙️ Execute o exemplo (projeto Exemplo-Cloud-Config-Server) do Cloud Config Server localmente, e faça o teste no browser (ou Postman):

http://localhost:8888/cloudconfigpropserver/config/cloudconfigpropclient/development

🧠 Deve retornar os valores do properties que adiciononou na pasta

⚙️ Execute o exemplo (projeto Exemplo-Cloud-Config-Client) do Cloud Config Client localmente, e faça o teste no browser (ou Postman):

http://localhost:8080/api/config

🧠 Dever retornar o response do controler com os valores lidos no properties

🧵 Conclusão

Spring Cloud Config é uma ferramenta poderosa para centralizar, versionar e proteger configurações em ambientes distribuídos. Com suporte a criptografia, você consegue manter dados sensíveis seguros mesmo com repositórios públicos.

Para projetos GitOps ou ambientes mais avançados, considere:

- Sealed Secrets
- External Secrets Operator
- Spring Cloud Vault
