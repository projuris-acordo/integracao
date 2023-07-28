# Integração JUSTTO - Importação de novos processos

> Se você estiver procurando a documentação do webhook, vá para a [documentação de webhook padrão](webhook-padrao/README.md).

## Importação

A importação de novos processos no sistema JUSTTO pode ocorrer de duas maneiras. Por API ou envio de mensagens JMS.
Nas duas abordagens, o conteúdo utilizado é o mesmo. Um JSON com os processos.

A representação do domínio de payload das APIs estão disponíveis no diagrama de classe:

![Diagrama de classe de payloads da API de integração](INTEGRA%C3%87%C3%83O-Proposta%20Padr%C3%A3o.png?raw=true "Classes do payload")

## Postman

As APIs disponíveis para realizar a importação e consultas de disputas estão disponíveis publicamente no endereço https://documenter.getpostman.com/view/5391983/SzKPW1rv

## Configurações do servidor JMS

- **Provedor JMS:** [ActiveMQ na versão 5.15.9](http://activemq.apache.org/activemq-5159-release.html "ActiveMQ na versão 5.15.9")
- **Tipo de destino para importação:** fila (queue)
- **URL de conexão:** `tcp://activemq.justto.com.br:61616` ver parâmetros adicionais de configurações da conexão direto [na documentação do ActiveMQ](https://activemq.apache.org/connection-configuration-uri "na documentação do ActiveMQ")
- **Usuário de acesso:** ````
- **Senha de acesso:** ````
- **Fila para envio do LoteProcessoDTO:** `importacao-lote-processo-prod`

## Ambiente HTTP para acesso a API

API para salvar o lote de processos: POST `https://api.justto.app/api/justto-import`

## Autenticação na API

Para utilizar a API, precisa se identificar no sistema.
O processo consiste em 2 etapas:
1 - Enviar um login e senha para gerar o token HTTP que irá ser utilizado no header das próximas requisições (Authorization)
2 - Selecionar qual workspace irá ser utilizada

### Login - Authorization para listar workspaces

Para realizar o login, enviar usuário e senha para obter o token temporário.

API: `POST https://api.justto.app/api/accounts/token`

Payload:

```json
{
  "email": "lucas@justto.com.br",
  "password": "minhaSenha"
}
```

Retorno desta chamada será um JSON contendo o token para requisitar as workspaces deste usuário. Como no exemplo abaixo

```json
{
  "token": "Bearer eyJhbGciOiJIUzUxMiJ9.qdGkiOiIyOTQiLCJzdWIiOiJsdWNhc0BqdXN0dG8uY29tLmJyIiwiUEVSU09OU19JRFNfS0VZIjoiMzk0MTEsOTE3MzgsMTA4MDY3LDEyMTE2OCIsImV4cCI6MTU4MTUxOTQ1MX0.0whyvARSlRFblH2-Vege70EGOmEdKFHNMIYbvSF8FHi4t-4_ndT9xB_iSHsOvnb79QEZ3-HGat5y0_"
}
```

### Listando workspaces

Um usuário pode participar de várias workspaces, antes de interagir, precisa definir qual é a workspace que irá realizar as ações.
Todas as ações são realizadas dentro de uma workspace.
Para listar as workspaces do usuário:

API: `GET https://api.justto.app/api/workspaces/my?loadStrategy=[true|false]`

Parâmetros opcionais:
1 - loadStrategy: Se true, carrega a lista de estratégias disponíveis na workspace

Header HTTP: `Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.qdGkiOiIyOTQiLCJzdWIiOiJsdWNhc0BqdXN0dG8uY29tLmJyIiwiUEVSU09OU19JRFNfS0VZIjoiMzk0MTEsOTE3MzgsMTA4MDY3LDEyMTE2OCIsImV4cCI6MTU4MTUxOTQ1MX0.0whyvARSlRFblH2-Vege70EGOmEdKFHNMIYbvSF8FHi4t-4_ndT9xB_iSHsOvnb79QEZ3-HGat5y0_`

Muito importante. Note que o Authorization precisa enviar o token Bearer que é retornado no login (passo anterior).

Exemplo de retorno:

```json
[
  {
    "id": 348,
    "person": {
      "id": 39411,
      "accountId": null,
      "name": "Lucas Israel",
      "alias": null,
      "type": "NATURAL",
      "documentNumber": null,
      "locales": [],
      "emails": [
        {
          "id": 2818,
          "createAt": {
            "dateTime": "2019-04-23T21:26:01Z"
          },
          "updateAt": {
            "dateTime": null
          },
          "archived": false,
          "enriched": false,
          "isValid": true,
          "isMain": true,
          "ranking": null,
          "source": null,
          "address": "lucas@justto.com.br"
        }
      ],
      "phones": [
        {
          "id": 216114,
          "createAt": {
            "dateTime": "2020-01-10T17:55:37Z"
          },
          "updateAt": {
            "dateTime": null
          },
          "archived": false,
          "enriched": false,
          "isValid": true,
          "isMain": true,
          "ranking": null,
          "source": null,
          "number": "(12) 98826-1043",
          "isMobile": false,
          "service": null
        }
      ],
      "oabs": [],
      "bankAccounts": [
        {
          "createdAt": "2019-12-20T13:40:12.928Z",
          "updatedAt": null,
          "id": 1021,
          "personId": null,
          "name": "Teste Sprint 17",
          "document": "CPF",
          "email": "lucas@justto.com.br",
          "number": "8901",
          "agency": "0987",
          "bank": "422 - BANCO SAFRA",
          "type": "SAVING"
        }
      ],
      "alerts": [],
      "properties": {
        "checkout.oabNumber": "",
        "checkout.email": "lucas@justto.com.br",
        "checkout.documentNumber": "36866231884"
      }
    },
    "workspace": {
      "id": 110,
      "subDomain": "lucasisrael",
      "name": "JUSTTO Advogados",
      "teamName": "Equipe JUSTTO",
      "status": "READY",
      "type": "LAW_FIRM"
    },
    "profile": "ADMINISTRATOR",
    "strategys": [
      {
        "id": 30,
        "name": "INDENIZATÓRIA SEM OFERTA DE VALOR",
        "types": ["PAYMENT"],
        "triggerType": "ENGAGEMENT"
      },
      {
        "id": 19,
        "name": "INDENIZATÓRIO - (JEC, CÍVEL)",
        "types": ["PAYMENT"],
        "triggerType": "ENGAGEMENT"
      }
    ]
  }
]
```

Para cada objeto retornado, é um acesso em uma workspace que o usuário possui.
O nome da Workspace (já conhecido pelo usuário da JUSTTO. O que aparece para selecionar quando faz o login), é o atributo `workspace.teamName`
Na API, para identificar uma workspace, o que precisa ser enviado é `workspace.subDomain`

### Listando usuários da workspaces

Permite listar todos os usuários de uma workspace.

Documentação no POSTMAN: https://documenter.getpostman.com/view/5391983/SzKPW1rv#172ea14c-6bbe-4c8a-8303-6df7a6177197

Endereo para obter lista de usuários:
API `GET https://api.justto.app/api/workspaces/members/vm`

Header HTTP:`Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.qdGkiOiIyOTQiLCJzdWIiOiJsdWNhc0BqdXN0dG8uY29tLmJyIiwiUEVSU09OU19JRFNfS0VZIjoiMzk0MTEsOTE3MzgsMTA4MDY3LDEyMTE2OCIsImV4cCI6MTU4MTUxOTQ1MX0.0whyvARSlRFblH2-Vege70EGOmEdKFHNMIYbvSF8FHi4t-4_ndT9xB_iSHsOvnb79QEZ3-HGat5y0_`

Header HTTP: `Workspace: c7082f65f08c475f9dd69b3eeaec6f3b`

Note que o Authorization é o token Bearer obtido no login (passo 1) e a Workspace é o atributo `workspace.subDomain` da workspace selecionada para uso.

O retorno é um payload listando os membros da da equipe e seus respectivos papeis na equipe.

**Note** que esta API é paginada. Se não enviar o tamanho da paginação, o padrão será 20 itens por páginas.
Para carregar todos de uma única vez, utilize o parâmetro `?size=999` na URL. Exemplo: `GET https://api.justto.app/api/workspaces/members/vm?size=999`

Para exibir os membros arquivados, utilize o parâmetro: showArchived=true
Exemplo: `GET https://api.justto.app/api/workspaces/members/vm?showArchived=true`

Exemplo de retorno:

```json
{
  "content": [
    {
      "id": 4297,
      "person": {
        "id": 683287,
        "name": "Deivid Barbosa",
        "type": "NATURAL",
        "documentNumber": "CPF",
        "emails": [
          {
            "id": 679104,
            "source": "CLIENT",
            "address": "deivid@justto.com.br"
          }
        ],
        "phones": [],
        "oabs": [],
        "bankAccounts": [],
        "alerts": [],
        "properties": {}
      },
      "workspace": {
        "id": 583,
        "keyAccountId": 294,
        "subDomain": "1f0446fcd90a43859e780e812025df5a"
      },
      "profile": "ADMINISTRATOR",
      "accountEmail": "deivid@justto.com.br"
    },
    {
      "id": 4300,
      "person": {
        "id": 683325,
        "name": "Lucas Israel GMAIL",
        "type": "NATURAL",
        "documentNumber": "CPF",
        "emails": [
          {
            "id": 679240,
            "source": "CLIENT",
            "address": "emaildolucas@gmail.com"
          }
        ],
        "phones": [],
        "oabs": [],
        "bankAccounts": [],
        "alerts": [],
        "properties": {}
      },
      "workspace": {
        "id": 583,
        "keyAccountId": 294,
        "subDomain": "1f0446fcd90a43859e780e812025df5a"
      },
      "profile": "NEGOTIATOR",
      "accountEmail": "emaildolucas@gmail.com",
      "archived": false
    }
  ],
  "pageable": {
    "sort": {
      "sorted": false,
      "unsorted": true,
      "empty": true
    },
    "offset": 0,
    "pageNumber": 0,
    "pageSize": 20,
    "unpaged": false,
    "paged": true
  },
  "last": true,
  "totalPages": 1,
  "totalElements": 4,
  "size": 20,
  "number": 0,
  "sort": {
    "sorted": false,
    "unsorted": true,
    "empty": true
  },
  "first": true,
  "numberOfElements": 4,
  "empty": false
}
```

### Obtendo lista de estratégias da workspace

Existem estratégias padrões do sistema (disponíveis para todas as workspaces) e existem estratégias privadas para a workspace.
Esta última, somente é visivel para a workspace que é sua proprietária.
Para listar as estratégias padrões + da workspace, precisa estar autenticado.

Endereço para obter lista de estratégias
API: `GET https://api.justto.app/api/workspaces/strategies`

Header HTTP: `Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.qdGkiOiIyOTQiLCJzdWIiOiJsdWNhc0BqdXN0dG8uY29tLmJyIiwiUEVSU09OU19JRFNfS0VZIjoiMzk0MTEsOTE3MzgsMTA4MDY3LDEyMTE2OCIsImV4cCI6MTU4MTUxOTQ1MX0.0whyvARSlRFblH2-Vege70EGOmEdKFHNMIYbvSF8FHi4t-4_ndT9xB_iSHsOvnb79QEZ3-HGat5y0_`

Header HTTP: `Workspace: c7082f65f08c475f9dd69b3eeaec6f3b`

Note que o Authorization é o token Bearer obtido no login (passo 1) e a Workspace é o atributo `workspace.subDomain` da workspace selecionada para uso.

**OBS**: No objeto LoteProcessoDTO existe um atributo chamado nomeUsuario, do tipo alfanumérico. Este atributo deve ser informado com o subDomain da workspace.
No caso de requisições não HTTP (ex: filas), a única maneira de saber a qual workspace pertence os processos do lote é atraves desta informação.

## Filtrando disputas cadastradas na workspace

URL: GET https://api.justto.app/api/disputes/filter

### Filtros disponíveis

Todos os filtros são do tipo Query Params

### De paginações e ordenações

- page -> Indica a página que se deseja exibir (inteiro. ex: 0,1,2)
- size -> Indica o tamanho da página. A quantidade de conteúdo da página (inteiro. ex: 20,50,100)
- sort -> Define ordenações do filtro. Precisa informar o nome do campo e o tipo de ordenação. Pode ser informado mais de um.
  Exemplo: sort=id,desc&sort=favorite,desc

Exemplo completo de uma URL válida para listar os primeiros 20 registros ordenando de forma decrescente pelo ID:
https://api.justto.app/api/disputes/filter?page=0&size=20&sort=id,desc

### De negociadores

É possível filtrar somente disputas que pertence a um determinado negociador. O parâmetro do filtro é o ID do negociador no sistema.

Exemplo de URL buscando disputas do negociador com ID 1000
https://api.justto.app/api/disputes/filter?persons=45442

### De prescrições do sistema (auxilio rápido ao que é importante)

Existem várias prescrições para filtro de disputas que auxiliam na tomada de decisões. São elas:

- HAS_ANSWER -> Filtra somente disputas que possuem respostas (recebeu mensagem da parte contrária)
- COUNTERPROPOSAL_UP_TO_20 -> Filtra disputas com contrapropostas até 20% da alçada máxima
- COUNTERPROPOSAL_OVER_20 -> Filtra disputas com contrapropostas acima de 20% da alçada máxima
- ONLY_VISUALIZED -> Filtra disputas cuja parte contrára recebeu email e visualizou o email
- UNSETTLED_WITH_MESSAGES -> Filtra disputas encerradas como PERDIDAS e que receberam novas mensagens após serem marcadas como PERDIDAS
- PENDING -> Filtra disputas que estão PENDENTES. Que não foram possíveis encontrar dados para realizar o engajamento
  Exemplo de URL: https://api.justto.app/api/disputes/filter?prescriptions=PENDING

### De campanhas pelo nome

Permite filtrar campanhas pelo nome das campanhas.
Nome do parâmetro: campaigns
Exemplo de URL:
https://api.justto.app/api/disputes/filter?campaigns=SUBMARINO%20VIAGENS%20LTDA.&campaigns=SUBMARINO%2031.01.2020%20-

Note que pode ser enviado mais de uma campanha

### Por data fim da negociação

Permite filtrar disputas com intervalo de datas limites para negociar.

Intervalo de datas deve ser informado nos parâmetros iniciais e finais.
expirationDateStart e expirationDateEnd
Deve ser inviado no formato padrão de datas do sistema JUSTTO, que é em UTC, no formato "yyyy-MM-dd'T'HH:mm:ss'Z'"

Exemplo de URL: https://api.justto.app/api/disputes/filter?expirationDateStart=2020-02-14T00:00:00Z&expirationDateEnd=2020-02-15T23:59:59Z

### Por data da importação da disputa

Permite filtrar disputas com intervalo de datas que foram importadas no sistema

Intervalo de datas deve ser informado nos parâmetros iniciais e finais.
importingDateStart e importingDateEnd
Deve ser inviado no formato padrão de datas do sistema JUSTTO, que é em UTC, no formato "yyyy-MM-dd'T'HH:mm:ss'Z'"

Exemplo de URL: https://api.justto.app/api/disputes/filter?importingDateStart=2020-02-14T00:00:00Z&importingDateEnd=2020-02-15T23:59:59Z

### Por status da disputa

Permite filtrar pelo status atual da disputa.
O nome do parâmetro é "status".

Exemplo de URL: https://api.justto.app/api/disputes/filter?status=ENRICHED

As disputas podem estar nos seguintes estados:

- IMPORTED -> Status inicial. Assim que é importada, fica neste status.
- ENRICHED -> Indica que já foi enriquecida
- ENGAGEMENT -> Indica que agendou mensagens para as partes e está enviando conforme agendamento
- RUNNING -> Infica que engajou a parte e iniciou a negociação
- PENDING -> Infica que não conseguiu gerar mensagens para engajar a parte. Geralmente por falta de contatos válidos
- ACCEPTED -> Infica que a parte aceitou a proposta da disputa
- CHECKOUT -> Infica que a parte informou os dados da conta bancária para depósito
- EXPIRED -> Indica disputa que expirou. Que atingiu a data fim de negociação
- SETTLED -> Infica disputa que foi ganha. Negociador precisa informar este status
- UNSETTLED -> Infica disputa que foi perdida. Negociador precisa informar este status
- REFUSED -> Indica disputa que foi negada pela parte

### Por status da disputa

Permite filtrar pelo nome do réu.
O nome do parâmetro é "respondentNames".
Exemplo de URL: https://api.justto.app/api/disputes/filter?status=ENRICHED&prescriptions=PENDING&page=0&size=20&sort=id,desc&sort=favorite,desc&respondentNames=AEROLINEAS%20ARGENTINAS%20SA

### Composição de filtros

Todos os filtros podem ser utilizados em conjunto. Todos podem ser utilizados mais de uma vez.
Exemplo de URL com mais de um filtro: https://api.justto.app/api/disputes/filter?status=ENRICHED&status=PENDING&status=IMPORTED&status=ENGAGEMENT&status=RUNNING&campaigns=SUBMARINO%20VIAGENS%20LTDA.&prescriptions=PENDING&page=0&size=20&sort=id,desc&sort=favorite,desc&respondentNames=AEROLINEAS%20ARGENTINAS%20SA&importingDateStart=2020-02-14T00:00:00Z&importingDateEnd=2020-02-14T23:59:59Z&

### Busca Global por termos

Permite realizar uma busca por termo genérico, que pode ser o ID da disputa, número do processo, nomes das partes, etc.
URL: GET https://api.justto.app/api/disputes/search?term=?
Exemplos:

- Por número de processos: https://api.justto.app/api/disputes/search?term=0002582-62.2020.8.17.8201
- Por ID do processo: https://api.justto.app/api/disputes/search?term=24567
- Por nome: https://api.justto.app/api/disputes/search?term=Lucas%20Israel

## Listando NOTAS do negociador em uma disputa

Permite listar as notas do negociador em uma disputa.
URL: GET https://api.justto.app/api/disputes/{dispute-id}/occurrences/type/NOTE

## Listando COMUNICAÇÕES em uma disputa

Permite listar todas as comunicações de uma disputa. Tanto o envio de mensagem do negociador para a parte ou advogado quando o recebimento de mensagens de todas as partes.
Tudo fica centralizado, independente do meio de comunicação (SMS, Whatsapp, email).
Também lista qualquer contraproposta realizada nos sistemas da JUSTTO.

URL: https://api.justto.app/api/disputes/{dispute-id}/occurrences/type/INTERACTION?size=20&sort=createdAt,desc&sort=id,desc

## Documentação dos tipos de dados e suas obrigatoriedades

Documenta os tipos de dados e suas respectivas funções. Estes "Domain Transfer Objects" estão representados no diagrama do arquivo "INTEGRAÇÃO-Proposta Padrão.png"

### LoteProcessoDTO

Representa um lote de processos

| **Atributo**               | **Mandatório** | **Tipo**                           | **Descrição**                                                                                                                                                                                                                                                                               |
| -------------------------- | -------------- | ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| observacao                 | Não            | Alfanumérico (100.000)             | Campo texto livre para entrada de observações. As observações adicionadas neste campo, serão adicionadas como "NOTAS" da disputa dentro da plataforma JUSTTO                                                                                                                                |
| alcadaMaxima               | Sim            | Inteiro (valor máximo: 2147483647) | O valor da alçada máxima que será aplicada para todos os processos do lote que não tenha especificado individualmente este atributo. Este valor é dado em centavos de reais. ex: R$ 2,43 => deve ser representado como 243                                                                  |
| documentoUsuario           | Sim            | Alfanumérico (20)                  | O CPF/CNPJ do usuário que está solicitando a criação/envio do lote de processos para ser negociado                                                                                                                                                                                          |
| emailUsuario               | Sim            | Alfanumérico (150)                 | O email principal do usuário que está solicitando a criação/envio do lote de processos para ser negociado. Este email será utilizado para identificar o usuário no Sistema JUSTTO                                                                                                           |
| accountEmails              | Não            | Array de String                    | Lista de emails a serem distribuidos o lote de disputas                                                                                                                                                                                                                                     |
| nomeUsuario                | Sim            | Alfanumérico (250)                 | O nome do usuário que está solicitando a criação/envio do lote de processos para ser negociado. Utilizar o subDomain da workspace para definir qual é a workspace que irá receber as disputas                                                                                               |
| intimado                   | Sim            | Alfanumérico (250)                 | O nome do intimado no processo                                                                                                                                                                                                                                                              |
| documentoIntimado          | Sim            | Alfanumérico (250)                 | O documento CPF/CNPJ do intimado no processo                                                                                                                                                                                                                                                |
| processos                  | Sim            | Array de ProcessoDTO               | Lista contendo processos a serem enviados para a JUSTTO                                                                                                                                                                                                                                     |
| estrategia                 | Não            | Numérico                           | ID da estratégia de engajamento utilizada na plataforma da JUSTTO                                                                                                                                                                                                                           |
| campanha                   | Não            | Alfanumérico                       | Nome da campanha que irá agrupar todos os processos do lote                                                                                                                                                                                                                                 |
| percentualPrimeiraProposta | Não            | Inteiro (entre 0 e 100)            | Representa o percentual da primeira proposta calculado a partir da alçada máxima. Somente aplicável aos casos onde não é informado o valor da primeira proposta. Quando não informado, o valor padrão é de 60%, ou seja, a primeira proposta irá com 60% do valor da alçada máxima definida |

### ConfiguracaoImportacao

Representa as configurações de comportamento que o sistema deve respeitar para a importação do lote de processos

| **Atributo**                         | **Mandatório** | **Tipo** | **Descrição**                                                                                                                                                                                                                                                                                                                                                                                                               |
| ------------------------------------ | -------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| contactarAutorQuandoSemAdvogado      | Não            | boleano  | Flag indicando se o sistema deve contactar o autor do processo quando não houver ou não encontrar o advogado representante do autor. Quando true, para os processos sem advogados constituídos, o sistema irá enviar mensagem para o autor. Padrão é false, ou seja, nunca contactar o autor do processo.                                                                                                                   |
| contactarAutorQuandoAdvogadoSemDados | Não            | boleano  | Flag indicando se o sistema deve contactar o autor do processo quando o advogado representante do autor não possuir dados para ser contactado (telefone ou email). Quando true, para os processos que possuam advogados constituídos, mas o sistema não encontrar dados de contatos (email/telefone) do advogado, o sistema irá enviar mensagem para o autor. Padrão é false, ou seja, nunca contactar o autor do processo. |
| agendarMsgHorarioComercial           | Não            | boleano  | Flag indicando se o sistema irá enviar mensagens automáticas somente dentro do horário comercial. Padrão é true, ou seja, envia mensagens envia mensagens somente de segunda a sexta entre as 8 e 18 horas                                                                                                                                                                                                                  |
| ignorarEnriquecimentoAutomatico      | Não            | boleano  | Flag indicando se o sistema irá realizar enriquecimento do processo e de seus participantes. Padrão é false, ou seja, sistema irá enriquecer com o máximo de informações que puder                                                                                                                                                                                                                                          |
| naoPermiteContaPoupanca              | Não            | boleano  | Flag indicando que os processos não devem permitir que o autor ou o advogado informem uma conta do tipo poupança para o depósito do valor do acordo, se houver acordo                                                                                                                                                                                                                                                       |

### ProcessoDTO

Representa um processo que será cadastrado no sistema JUSTTO como disputa a ser negociada com as partes.

| **Atributo**     | **Mandatório** | **Tipo**           | **Descrição**                                                                                                                                                                                                     |
| ---------------- | -------------- | ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| numero           | Sim            | Alfanumérico (250) | Representa o número de identificação única do processo no TJ                                                                                                                                                      |
| foro             | Não            | Alfanumérico (250) | Nome do foro do processo                                                                                                                                                                                          |
| vara             | Não            | Alfanumérico (250) | Nome da vara do processo                                                                                                                                                                                          |
| classe           | Não            | Alfanumérico (250) | A classe do processo                                                                                                                                                                                              |
| valor            | Sim            | Numérico Real      | Valor monetário da ação representada. Exemplo: R$ 1500,00 deve ser representado como 1500.00                                                                                                                      |
| alcadaMaxima     | Não            | Numérico Real      | Valor monetário da alçada máxima para negociação. Quando não informado este atributo, irá considerar o atributo alçada máxima do Lote. Exemplo: R$ 1500,00 deve ser representado como 1500.00                     |
| assunto          | Não            | Alfanumérico (250) | Assunto do processo                                                                                                                                                                                               |
| comptetencia     | Não            | Alfanumérico (250) | Comptência do processo                                                                                                                                                                                            |
| partes           | Sim            | Array de ParteDTO  | Lista contendo as partes do processo                                                                                                                                                                              |
| dataLimite       | Não            | Date               | Objeto de data no formato "yyyy-MM-dd'T'HH:mm:ss'Z'" indicando a data limite de negociação                                                                                                                        |
| codigoExterno    | Não            | Alfanumérico (50)  | Representa a identificação do processo no sistema externo, para referências futuras                                                                                                                               |
| observacao       | Não            | Alfanumérico (500) | Qualquer observação ou anotação que queira importar anexado no processo para ficar disponível para o negociador na plataforma                                                                                     |
| primeiraProposta | Não            | Numérico Real      | Valor monetário da primeira proposta. Quando não informado este atributo, irá considerar o percentual da primeira proposta informado no lote de processos. Exemplo: R$ 1500,00 deve ser representado como 1500.00 |

### ParteDTO

Representa o participante de uma disputa que será cadastrado no sistema JUSTTO.

| **Atributo**    | **Mandatório**       | **Tipo**             | **Descrição**                                                                                                                                                                                                                                                                                                                 |
| --------------- | -------------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| nome            | Não                  | Alfanumérico (250)   | Nome completo da parte                                                                                                                                                                                                                                                                                                        |
| documentoFiscal | Sim                  | Alfanumérico (20)    | CPF ou CNPJ da parte                                                                                                                                                                                                                                                                                                          |
| documentoCivil  | Não                  | Alfanumérico (20)    | RG da parte, quando disponível                                                                                                                                                                                                                                                                                                |
| principal       | Não - padrão é false | boolean (true/false) | Flag indicando se a parte é a principal para o tipo de participação no processo. Exemplo: No caso de processos com mais de um réu, precisamos ter certeza que o principal é a parte que está iniciando a negociação. Sempre que possível, iremos inferir com base no documentoFiscal comparando com o cnpj do LoteProcessoDTO |
| emails          | Não                  | Array de EmailDTO    | Lista de emails os endereços de emails da parte                                                                                                                                                                                                                                                                               |
| tipo            | Sim                  | TipoParticipacao     | Enum informando o tipo de participação da parte no processo                                                                                                                                                                                                                                                                   |
| perfil          | Sim                  | Perfil               | Enum informando o perfil da parte no processo                                                                                                                                                                                                                                                                                 |
| principal       | Sim                  | Boolean              | Flag inficando se é a parte principal para o tipo de participação informado                                                                                                                                                                                                                                                   |
| enderecos       | Não                  | Array de EnderecoDTO | Lista de enrereços da parte                                                                                                                                                                                                                                                                                                   |
| telefones       | Não                  | Array de TelefoneDTO | Lista de telefones da parte                                                                                                                                                                                                                                                                                                   |

### EmailDTO

Representa um email. Este objeto foi criado apartado para permitir que associemos mais atributos, como a fonte de dados do email.

| **Atributo** | **Mandatório** | **Tipo**     | **Descrição**                                                                                                                                                                |
| ------------ | -------------- | ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| endereco     | Sim            | String (150) | O endereço de email válido. Deve ser apenas um email, não a lista de emails. Quando houver mais de um, criar várias instiancias do objeto EmailDTO com o respectivo endereço |

### EnderecoDTO

Representa um endereço físico da parte associada no processo.

| **Atributo** | **Mandatório** | **Tipo**           | **Descrição**                                                                                       |
| ------------ | -------------- | ------------------ | --------------------------------------------------------------------------------------------------- |
| logradouro   | Não            | Alfanumérico (250) | Logradouro do endereço da parte. Aqui utilizado para descrever o nome da rua,avenida, viela, etc... |
| numero       | Não            | Alfanumérico (25)  | Descreve o número ou o motivo de não ter número do endereço                                         |
| complemento  | Não            | Alfanumérico (250) | Descreve qualquer complemento que auxilie no endereço                                               |
| bairro       | Não            | Alfanumérico (250) | Nome do bairro do endereço                                                                          |
| cidade       | Não            | Alfanumérico (250) | Nome da cidade do endereço                                                                          |
| cep          | Não            | Alfanumérico (25)  | CEP do endereço                                                                                     |
| estado       | Não            | Estado             | O estado (Unidade Federativa) do endereço                                                           |
| tipo         | Sim            | TipoEndereco       | Indicador do tipo de endereço                                                                       |

### TelefoneDTO

Representa o telefone da parte

| **Atributo** | **Mandatório**       | **Tipo**              | **Descrição**                                                                                                                                    |
| ------------ | -------------------- | --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| numero       | Sim                  | Alfanumérico (15)     | Número do teleefone com o DDD. No caso de número internacional, incluir o código do país                                                         |
| mobile       | Não - padrão é false | booleano (true/false) | Flag indicando se o telefone é um número de telefone móvel. Fundamental para iniciarmos verificação do número com whatsapp ou com provedores SMS |
