# Integração Projuris Acordos - Webhook padrão da Projuris Acordos

> Se você estiver procurando a documentação da API de importação, vá para o [documento principal](../readme.md).

Você pode configurar sua workspace para enviar a movimentação de todas as disputas em um [webhooks](https://pt.wikipedia.org/wiki/Webhook) seu.
Isto permite que sua aplicação (exemplo: seu ERP jurídico) receba uma atualização de todas. Qualquer movimentação nos dados da disputa, enviaremos pra você.
Você escolhe o que é interessante pra você processar com os dados que chegam, assim tem a flexibilidade para implementar as regras de negócios que melhor atenda a sua equipe.

## Diagrama de classes representando o que você irá receber
![Diagrama de classe de payloads que enviaremos no seu webhook](Objeto-do-webhook-padrao.png?raw=true "Classes do payload do webhook")

# Classes
Documenta as classes que enviamos no webhook

### DisputeVM
Representa uma disputa no sistema da Projuris Acordos

| **Atributo**                      | **Mandatório** | **Tipo**                                            | **Descrição**                                                                                                                                                                                                                                                                                                    |
| --------------------------------- | -------------- | --------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| archived                          | Sim            | Booleano                                            | Indica quando a disputa foi arquivada/removida                                                                                                                                                                                                                                                                   |
| alwaysContactParty                | Sim            | Booleano                                            | Indica quando a disputa deve contatar a parte do processo                                                                                                                                                                                                                                                        |
| code                              | Sim            | Alfanumérico (255)                                  | Código único da disputa, normalmente utilizado com o número do processo júridico                                                                                                                                                                                                                                 |
| contactPartyWhenInvalidLawyer     | Sim            | Booleano                                            | Indica quando deve contatar a parte caso não tenha contato do advogado                                                                                                                                                                                                                                           |
| contactPartyWhenNoLawyer          | Sim            | Booleano                                            | Indica quando deve contatar a parte caso não localize o advogado                                                                                                                                                                                                                                                 |
| courtHearingDate                  | Não            | Data hora                                           | Data da audiência                                                                                                                                                                                                                                                                                                |
| denySavingDeposit                 | Sim            | Booleano                                            | Não aceita deposito em conta poupança                                                                                                                                                                                                                                                                            |
| description                       | Não            | Alfanumérico                                        | Descrição da disputa. Também é utilizdo para descrever obrigações de fazer (OBF)                                                                                                                                                                                                                                 |
| disputeDealDate                   | Não            | Data hora                                           | Data em que a disputa teve a proposta aceita (data do acordo)                                                                                                                                                                                                                                                    |
| disputeDealValue                  | Não            | Numérico                                            | Valor do acordo                                                                                                                                                                                                                                                                                                  |
| denySavingDeposit                 | Sim            | Booleano                                            | Negar pagamento do acordo em conta Poupança                                                                                                                                                                                                                                                                      |
| disputeNextToExpire               | Sim            | Booleano                                            | Quando verdadeiro, indica que restam menos de 4 dias para a data limite de negociação                                                                                                                                                                                                                            |
| disputeUpperRange                 | Não            | Numérico                                            | Valor máximo para realizar um acordo. A alçada máxima.                                                                                                                                                                                                                                                           |
| expirationDate                    | Não            | Data hora                                           | Prazo limite para negociar a disputa                                                                                                                                                                                                                                                                             |
| externalId                        | Não            | Alfanumérico                                        | Chave única do usuário. Em caso de integração entre sistemas, utilizar o ID do sistema externo. Pode ser importado com qualquer código para facilitar a localização em outros sistemas                                                                                                                           |
| awaitingAnalysis                  | Sim            | Booleano                                            | Proposta aguardando analise da empresa. Ocorre quando recebe uma proposta próximo da alçada máxima e o negociador/mediador responsável solicita a majoração da alçada para a empresa. Esta flag é apenas um indicador para auxiliar na localização de casos que estão aguardando resposta de majoração da alçada |
| firstClaimant                     | Não            | Alfanumérico                                        | Nome da primeira parte contrária da disputa                                                                                                                                                                                                                                                                      |
| firstClaimantDocumentNumber       | Não            | Numérico                                            | CPF/CNPJ da primeira parte contrária da disputa                                                                                                                                                                                                                                                                  |
| firstClaimantStatus               | Sim            | ONLINE/OFFLINE                                      | Indica se a parte contrária está interagindo com a plataforma no exato momento                                                                                                                                                                                                                                   |
| firstClaimantLawyer               | Não            | Alfanumérico                                        | Nome do primeiro advogado da parte contrária da disputa                                                                                                                                                                                                                                                          |
| firstClaimantLawyerDocumentNumber | Não            | Numérico                                            | CPF/CNPJ do primeiro advogado da parte contrária da disputa                                                                                                                                                                                                                                                      |
| firstClaimantLawyerOab            | Não            | Alfanumérico                                        | Inscrição na OAB do primeiro advogado da parte contrária da disputa                                                                                                                                                                                                                                              |
| firstClaimantLawyerStatus         | Sim            | ONLINE/OFFLINE                                      | Indica se o advogado da parte contrária está interagindo com a plataforma no exato momento                                                                                                                                                                                                                       |
| hasDocument                       | Sim            | Booleano                                            | Indica se existe minuta gerada para formalizar o acordo. Quando criar a minuta, esta flag já irá vir true, indicando que existe um documento mesmo que este ainda não foi pra assinatura. O status de assinatura do documento está disponível no atributo signStatus                                             |
| hasOBFInStrategy                  | Sim            | Boolean                                             | Indica se a estratégia da disputa possui 'Obrigação de Fazer'                                                                                                                                                                                                                                                    |
| id                                | Sim            | Numérico                                            | Identificação interna única da disputa na Projuris Acordos                                                                                                                                                                                                                                                                 |
| isMy                              | Sim            | Booleano                                            | Indica se a disputa pertence ao usuário logado. Só é utilizado quando consulta a disputa via API. Ao consultar via API, se o usuário que fizer a requisição for o negociador/mediador da disputa, esta flag irá vir true                                                                                         |
| lastCounterOfferName              | Não            | Alfanumérico                                        | Nome do responsável por realizar a última contra proposta na disputa                                                                                                                                                                                                                                             |
| lastCounterOfferValue             | Não            | Numérico                                            | Valor da última contra proposta realizada na disputa                                                                                                                                                                                                                                                             |
| lastOfferName                     | Não            | Alfanumérico                                        | Nome do responsável por realizar a última proposta na disputa                                                                                                                                                                                                                                                    |
| lastOfferValue                    | Não            | Numérico                                            | Valor da última proposta realizada na disputa                                                                                                                                                                                                                                                                    |
| lastOfferRoleId                   | Sim            | Numérico                                            | Identificador único da parte que realizou a última proposta na disputa                                                                                                                                                                                                                                           |
| lastVisualizationByNegotiator     | Não            | Data hora                                           | Momento em que o negociador/mediador visualizou a disputa pela última vez                                                                                                                                                                                                                                        |
| mainRespondent                    | Sim            | Alfanumérico                                        | Nome do réu da disputa                                                                                                                                                                                                                                                                                           |
| manualStrategy                    | Sim            | Booleano                                            | Se verdadeiro, o agendamento de mensagens automáticas para as partes da disputa não ocorrerá                                                                                                                                                                                                                     |
| materialDamage                    | Não            | Numérico                                            | Valor em danos materiais do processo                                                                                                                                                                                                                                                                             |
| moralDamage                       | Não            | Numérico                                            | Valor em danos morais do processo                                                                                                                                                                                                                                                                                |
| paused                            | Sim            | Booleano                                            | Se verdadeiro, indica que a disputa está com as negociações pausada. Isto impede qualquer movimentação da disputa na plataforma. Também impede que a parte contrária faça qualquer ação além de enviar mensagens na disputa                                                                                      |
| properties                        | Não            | Mapa                                                | Informações adicionais da disputa e processo. É um mapa (chave/valor) que contém informações adicionais da disputa capturadas a partir dos tribunais. Cada tribunal tem um padrão e disponibiliza alguma informação diferente. Coletamos todas disponíveis e armazenamos como chave/valor na disputa             |
| provisionedValue                  | Não            | Numérico                                            | Valor provisionado pela empresa na disputa                                                                                                                                                                                                                                                                       |
| requestedValue                    | Não            | Numérico                                            | Valor pedido no processo                                                                                                                                                                                                                                                                                         |
| strategyId                        | Sim            | Numérico                                            | Indentificador interno da estratégia utilizada na disputa. A extratégia define a régua de mensagens automáticas além dos objetos negociados na disputa (ex: vai negociar cobrança ou indenizatório? Vai negociar obrigações de fazer?)                                                                           |
| strategyName                      | Sim            | Alfanumérico                                        | Nome da estratégia da disputa. A extratégia define a régua de mensagens automáticas além dos objetos negociados na disputa (ex: vai negociar cobrança ou indenizatório? Vai negociar obrigações de fazer?)                                                                                                       |
| tagsIds                           | Não            | Lista numérica                                      | Lista de IDs das etiquetas da disputa. O negociador/mediador podem associar/desassociar várias etiquetas para se organizar. Este atributo descreve uma lista de IDs com as etiquetas que foram criadas e estão associadas na disputa                                                                             |
| updateAt                          | Não            | Data hora                                           | Data hora da última atualização da disputa                                                                                                                                                                                                                                                                       |
| visualized                        | Sim            | Booleano                                            | Se falso, indica que existem interações não visualizadas na disputa pelo negociador/mediador. Se true, indica que após receber a última mensagem, a disputa já foi visualizada (conceito parecido com o email)                                                                                                   |
| signStatus                        | Não            | [SignStatus](SignStatus)                            | Indica o estado da minuta quando enviada para assinatura. Quando não estiver definido, indica que o documento ainda não foi enviado (está em fase de edição, por exemplo). Os valores deste enum estão documentados logo abaixo                                                                                  |
| firstClaimantAlerts               | Não            | Lista do enum [DisputeAlertType](#DisputeAlertType) | Lista de alertas na parte contrária da disputa. Os valores deste enum estão documentados logo abaixo.                                                                                                                                                                                                            |
| firstClaimantLawyerAlerts         | Não            | Lista do enum [DisputeAlertType](#DisputeAlertType) | Lista de alertas no advogado da parte contrária da disputa. Os valores deste enum estão documentados logo abaixo.                                                                                                                                                                                                |
| status                            | Sim            | [DisputeStatus](#DisputeStatus)                     | Status que a disputa se encontra. Os valores deste enum estão documentados logo abaixo                                                                                                                                                                                                                           |
| conclusion                        | Não            | [DisputeConclusionDto](#DisputeConclusionDto)       | Detalhes da conclusão da disputa. Está documentado logo abaixo                                                                                                                                                                                                                                                   |
| bankAccounts                      | Não            | Lista de [BankAccountDto](#BankAccountDto)          | Lista de conta bancárias associadas na disputa para depósito do acordo. Está documentado logo abaixo                                                                                                                                                                                                             |
| lastReceivedMessage               | Não            | [DisputeInteractionDto](#DisputeInteractionDto)     | A última mensagem recebida na disputa                                                                                                                                                                                                                                                                            |
| lastNegotiatorAccess              | Não            | [DisputeInteractionDto](#DisputeInteractionDto)     | O último acesso realizado no portal de negociações                                                                                                                                                                                                                                                               |
| lastOutboundInteraction           | Não            | [DisputeInteractionDto](#DisputeInteractionDto)     | A última mensagem enviada para a parte ou advogado da parte. A última interação de saída                                                                                                                                                                                                                         |
| lastInboundInteraction            | Não            | [DisputeInteractionDto](#DisputeInteractionDto)     | A interação de entrada. Exemplos: mensagem recebida, click num link enviado, leitura de uma mensagem, acesso ao portal de negociação, etc... Qualquer interação que a parte faça com o negociador                                                                                                                |
| firstInteraction                  | Não            | [DisputeInteractionDto](#DisputeInteractionDto)     | A primeira interação da disputa                                                                                                                                                                                                                                                                                  |
| lastInteraction                   | Não            | [DisputeInteractionDto](#DisputeInteractionDto)     | A última interação da disputa                                                                                                                                                                                                                                                                                    |
| campaign                          | Sim            | [CampaignDto](#CampaignDto)                         | A campanha associada na disputa                                                                                                                                                                                                                                                                                  |
| workspace                         | Sim            | [WorkspaceDto](#WorkspaceDto)                       | A workspace associada na disputa                                                                                                                                                                                                                                                                                  |


### DisputeConclusionDto
Representa a conclusão de uma disputa.
Interessante documentar que a conclusão ocorre quando o negociador/mediador marca como perdida ou a disputa é aceita (a proposta é aceita pela parte contrária)
| **Atributo**   | **Mandatório** | **Tipo**                        | **Descrição**                                                                                                                                                |
| -------------- | -------------- | ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| description    | Sim            | Alfanumérico                    | Descrião da conclusão                                                                                                                                        |
| conclusionDate | Sim            | Data hora                       | A data e hora que a disputa foi concluída                                                                                                                    |
| reasons        | Não            | Lista alfanumérica              | Lista de motivos da conclusão. No caso de disputa perdida, por exemplo, aqui o negociador envia quais são os motivos pelo quais a disputa não foi frutífera. |
| status         | Sim            | [DisputeStatus](#DisputeStatus) | Status que a disputa se encontra no momento que foi concluída. Os valores deste enum estão documentados logo abaixo                                          |



### BankAccountDto
Contas bancárias associadas na disputa para realizar o depósito do valor acordado.
Interessante documentar que ter contas bancárias associadas não representa que a disputa teve acordo. Muitos casos, os dados bancários são associados enquanto ocorre a negociação
| **Atributo** | **Mandatório** | **Tipo**                            | **Descrição**                                                                       |
| ------------ | -------------- | ----------------------------------- | ----------------------------------------------------------------------------------- |
| id           | Sim            | Numérico                            | Indica o id interno da conta bancária nos cadastros da Projuris Acordos                       |
| personID     | Sim            | Numérico                            | Indica o id da pessoa que possui a conta bancária associada nos cadastros da Projuris Acordos |
| name         | Sim            | Alfanumérico                        | Nome da pessoa para depósito                                                        |
| document     | Sim            | Alfanumérico                        | CPF/CNPJ para realizar o depósito                                                   |
| email        | Sim            | Alfanumérico                        | Email do responsável pela conta                                                     |
| number       | Sim            | Alfanumérico                        | Número da conta bancária                                                            |
| agency       | Sim            | Alfanumérico                        | Número da agência bancária                                                          |
| bank         | Sim            | Alfanumérico                        | Nome do banco                                                                       |
| type         | Sim            | [BankAccountType](#BankAccountType) | Tipo da conta. Os valores deste enum estão documentados logo abaixo                 |

### DisputeInteractionDto
Representa uma interação na disputa
| **Atributo** | **Mandatório** | **Tipo**                                                    | **Descrição**                                                                                                                                                                     |
| ------------ | -------------- | ----------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id           | Sim            | Numérico                                                    | Identificação única da interação no sistema da Projuris Acordos                                                                                                                             |
| createdAt    | Sim            | Data hora                                                   | Representa a data e hora que foi criado                                                                                                                                           |
| properties   | Não            | Mapa chave/valor                                            | Representa propriedades da interação. São metadados adicionais da interação. As chaves disponíveis estão documentadas no enum [InteractionPropertyType](#InteractionPropertyType) |
| message      | Não            | [InteractionCommunicationDto](#InteractionCommunicationDto) | Representa a mensagem recebida ou enviada na interação, no caso de interação do tipo COMMUNICATION                                                                                |

### InteractionCommunicationDto
Representa uma interação na disputa
| **Atributo**      | **Mandatório** | **Tipo**                                                  | **Descrição**                                                                                                                                                                        |
| ----------------- | -------------- | --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| id                | Sim            | Numérico                                                  | Identificação única da interação no sistema da Projuris Acordos                                                                                                                                |
| createdAt         | Sim            | Data hora                                                 | Representa a data e hora que foi criado                                                                                                                                              |
| parameters        | Sim            | Mapa chave/valor                                          | Possui valores de metadados referentes a mensagem recebida ou enviada. Os metadados disponíveis estão documentados na enum [CommunicationParameterType](#CommunicationParameterType) |
| receiver          | Sim            | Alfanumérico                                              | Informa o endereo de quem vai receber a mensagem. Ex: No caso do email, o destinatário. No caso de uma mensagem WhatsApp o número que vai receber a mensagem.                        |
| sender            | Sim            | Alfanumérico                                              | Informa o endereo de quem mandou a mensagem. Ex: No caso de um email, que enviou o email.                                                                                            |
| resume            | Sim            | Alfanumérico                                              | Resumo da mensagem enviada                                                                                                                                                           |
| scheduledTime     | Não            | Data hora                                                 | Data e hora que a mensagem foi agendada para ser enviada                                                                                                                             |
| title             | Não            | Alfanumérico                                              | Assunto da mensagem enviado no email                                                                                                                                                 |
| trackerToken      | Sim            | Alfanumérico                                              | Identificação única da mensagem fornecida pelos prvedores (email e WhatsApp) que facilita o rastreio da mensagem                                                                     |
| contentType       | Sim            | [ContentType](#ContentType)                               | Identifica o tipo de conteúdo da mensagem. Os valores deste enum estão documentados logo abaixo                                                                                      |
| status            | Sim            | [CommunicationMessageStatus](#CommunicationMessageStatus) | Identifica status atual da mensagem. Os valores deste enum estão documentados logo abaixo                                                                                            |
| communicationType | Sim            | [CommunicationType](#CommunicationType)                   | Identifica o canal de comunicação utilizado. Os valores deste enum estão documentados logo abaixo                                                                                    |



### CampaignDto
Representa a campanha da disputa
| **Atributo**                  | **Mandatório** | **Tipo**     | **Descrição**                                                                                                                                                                                                                                     |
| ----------------------------- | -------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                            | Sim            | Numérico     | Identificação única da interação no sistema da Projuris Acordos                                                                                                                                                                                             |
| createdAt                     | Sim            | Data hora    | Representa a data e hora que foi criado                                                                                                                                                                                                           |
| alwaysContactParty            | Sim            | Booleano     | Indica que a Projuris Acordos sempre deve contatar o autor. Mesmo se tiver advogado, a Projuris Acordos irá agendar mensagens automáticas para serem enviadas para a parte contrária                                                                                  |
| businessHoursEngagement       | Sim            | Booleano     | Indica todas as mensagens automáticas serão agendadas para envio dentro de horário comercial. (De segunda a quinta das 8-19hs. Sexta das 8-18hs)                                                                                                  |
| contactPartyWhenInvalidLawyer | Sim            | Booleano     | Indica que a Projuris Acordos pode contatar a parte contrária quando seu advogado não possuir dados de contato e não conseguirmos contato com ele. Ex: em disputas onde o advogado está sem email/telefone, a plataforma irá agendar mensagens para o autor |
| contactPartyWhenNoLawyer      | Sim            | Booleano     | Indica que a Projuris Acordos pode contatar a parte contrária quando não tiver advogados na disputa                                                                                                                                                         |
| deadline                      | Sim            | Data hora    | A data limite que a plataforma irá negociar as disputas. Pode ser sobrescrito em cada disputa da campanha.                                                                                                                                        |
| denySavingDeposit             | Sim            | Boolean      | Indica que a Projuris Acordos não deve aceitar conta poupança para depósito de acordos da campanha                                                                                                                                                          |
| initialOfferPercentage        | Sim            | Numérico     | Indica o percentual da primeira proposta referente a alçada máxima                                                                                                                                                                                |
| name                          | Sim            | Alfanumérico | Indica o da campanha                                                                                                                                                                                                                              |
| skipEnrichment                | Sim            | Booleano     | Flag indica se quem importou solicitou que a Projuris Acordos enriqueça os dados automaticamente                                                                                                                                                            |


### WorkspaceDto
Representa a workspace da disputa
| **Atributo**                  | **Mandatório** | **Tipo**     | **Descrição**                                                                                                                                                                                                                                     |
| ----------------------------- | -------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                            | Sim            | Numérico     | Identificação única da interação no sistema da Projuris Acordos                                                                                                                                                                                             |
| name                          | Sim            | Alfanumérico | Indica o nome da workspace                                                                                                                                                                                                                        |
| teamName                      | Sim            | Alfanumérico | Indica o nome to time da workspace                                                                                                                                                                                                                |
| subDomain                     | Sim            | Alfanumérico | Indica o código único para o domínio da workspace                                                                                                                                                                                                 |
| status                        | Sim            | [WoskapceStatus](#WoskapceStatus) | Identifica status atual da workspace. Os valores deste enum estão documentados logo abaixo                                                                                                                                   |
| createdAt                     | Sim            | Data hora    | Representa a data e hora que foi criado                                                                                                                                                                                                           |


### PhoneDto
Representa um cadastro de telefone no sistema Projuris Acordos
| **Atributo** | **Mandatório** | **Tipo**     | **Descrição**                                         |
| ------------ | -------------- | ------------ | ----------------------------------------------------- |
| id           | Sim            | Numérico     | Identificação única da interação no sistema da Projuris Acordos |
| createdAt    | Sim            | Data hora    | Representa a data e hora que foi criado               |
| number       | Sim            | Alfanumérico | Representa o número de telefone                       |
| isMobile     | Sim            | Booleano     | Indica se o número é um número de celular ou fixo     |

### EmailDto
Representa um cadastro de email no sistema Projuris Acordos
| **Atributo** | **Mandatório** | **Tipo**     | **Descrição**                                         |
| ------------ | -------------- | ------------ | ----------------------------------------------------- |
| id           | Sim            | Numérico     | Identificação única da interação no sistema da Projuris Acordos |
| createdAt    | Sim            | Data hora    | Representa a data e hora que foi criado               |
| address      | Sim            | Alfanumérico | Representa o endereo de email                         |

### OabDto
Representa um cadastro de OAB no sistema Projuris Acordos
| **Atributo** | **Mandatório** | **Tipo**                          | **Descrição**                                         |
| ------------ | -------------- | --------------------------------- | ----------------------------------------------------- |
| id           | Sim            | Numérico                          | Identificação única da interação no sistema da Projuris Acordos |
| createdAt    | Sim            | Data hora                         | Representa a data e hora que foi criado               |
| number       | Sim            | Alfanumérico                      | Representa o número da inscrição da OAB               |
| state        | Sim            | [BrazilianState](#BrazilianState) | Representa o estado (UF) da OAB - A seccional         |


### DisputeRoleDto
Representa um participante da disputa
| **Atributo**       | **Mandatório** | **Tipo**                                     | **Descrição**                                                                                                                                   |
| ------------------ | -------------- | -------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| id                 | Sim            | Numérico                                     | Identificação única da interação no sistema da Projuris Acordos                                                                                           |
| createdAt          | Sim            | Data hora                                    | Representa a data e hora que foi criado                                                                                                         |
| birthday           | Não            | Data                                         | Representa data de nascimento da pessoa                                                                                                         |
| dead               | Sim            | Boolean                                      | Flag indicando possível óbito do participante                                                                                                   |
| disputeId          | Sim            | Numérico                                     | Identificação única da disputa na Projuris Acordos                                                                                                        |
| documentNumber     | Não            | Alfanumérico                                 | CPF/CNPJ do participante da disputa                                                                                                             |
| name               | Sim            | Alfanumérico                                 | Nome do participante da disputa                                                                                                                 |
| namesake           | Sim            | Boolean                                      | Indica se o participante é um homônimo. Quando possuir um CPF, esta flag deixa de ser útil                                                      |
| online             | Sim            | Boolean                                      | Indica se o participante está online no portal de negociações da Projuris Acordos                                                                         |
| personId           | Sim            | Numérico                                     | Indica a identificação única do cadastro da pessoa na workspace. Note que uma mesma pessoa pode pertencer a mais de uma disputa                 |
| roleNameLawyer     | Sim            | Boolean                                      | Flag indicando se o participante é um advogado. Isto significa que possui a role associada como LAWYER. Utilitário.                             |
| roleNameNegotiator | Sim            | Boolean                                      | Flag indicando se o participante é o negociador/mediador na disputa. Isto significa que possui a role associada como NEGOTIATOR. Utilitário.    |
| roleNameParty      | Sim            | Boolean                                      | Flag indicando se o participante é uma das partes da disputa (réu ou autor). Isto significa que possui a role associada como PARTY. Utilitário. |
| roles              | Sim            | Lista de [DisputeRoleName](#DisputeRoleName) | Lista contendo os papeis do participante na disputa                                                                                             |
| personType         | Sim            | [PersonType](#PersonType)                    | Indica se é pessoa física ou jurídica                                                                                                           |
| party              | Sim            | [DisputeParty](#DisputeParty)                | Indica a polaridade do participante (polo passivo ou ativo)                                                                                     |
| bankAccounts       | Sim            | Lista de [BankAccountDto](#BankAccountDto)   | Lista de contas bancárias cadastradas no participante da disputa                                                                                |
| phones             | Sim            | Lista de [PhoneDto](#PhoneDto)               | Lista de telefones cadastrados no participante da disputa                                                                                       |
| emails             | Sim            | Lista de [EmailDto](#EmailDto)               | Lista de emails cadastrados no participante da disputa                                                                                          |
| OabDto             | Sim            | Lista de [OabDto](#OabDto)                   | Lista de OABs cadastradas no participante da disputa                                                                                            |


# ENUM
Aqui vamos registrar todas as enums utilizadas no sistema. Também tentaremos dar exemplos de quais cenários são utilizadas.

### SignStatus
Enum representa o status de assinatura da minuta gerada. Quando existe documento gerado, mas não possui um status definido, significa que o documento está sendo redigido e ainda não foi enviado para nenhum assinante.
| **valor** | **Descrição**                                                                                                      |
| --------- | ------------------------------------------------------------------------------------------------------------------ |
| PENDING   | Indica que o documento foi enviado para assinatura. Está pendente de assinatura. Os assinantes ainda não assinaram |
| SIGNED    | Indica que todos os assinantes configurados no documento já assinaram. Processo de assinatura está concluído       |
| SIGNING   | Indica que pelo menos um dos assinantes já assinou o documento, mas que ainda está faltando assinaturas            |
| EXPIRED   | Indica que o tempo para realizar assinatura do documento foi excedido.                                             |
| CANCELED  | Indica que o documento teve a assinatura cancelada. Documento não está mais disponível.                            |

### DisputeStatus
Enum representa o status da disputa na plataforma da Projuris Acordos.
| **valor**       | **Descrição**                                                                                                                                                                                                                                                 |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IMPORTED        | Status temporário da disputa. A disputa só fica neste status quando acaba de ser importada. É o primeiro status da disputa na Projuris Acordos.                                                                                                                         |
| ENRICHED        | Indica que a disputa foi enriquecida pelo sistema da Projuris Acordos.                                                                                                                                                                                                  |
| ENGAGEMENT      | Indica que a disputa agendou mensagens automáticas para convidar as partes para negociar                                                                                                                                                                      |
| RUNNING         | Indica que a disputa entrou em negociação. Isto acontece quando ocorre uma interação da parte na disputa.                                                                                                                                                     |
| PENDING         | Indica que o sistema tentou agendar mensagens para convidar as partes para negociar, mas não conseguiu. A disputa está pendente de análise manual. Isto ocorre quando não temos dados de contatos disponíveis, por exemplo.                                   |
| ACCEPTED        | Indica que a parte contrária aceitou o valor e condições propostas pelo negociador/mediador. A disputa está apta para formalização, onde a minuta do acordo será gerada. Disputas neste status não possuem dados bancários para depósito do acordo.           |
| CHECKOUT        | Indica que a parte contrária aceitou o valor e condições propostas pelo negociador/mediador. A disputa está apta para formalização, onde a minuta do acordo será gerada. Neste status, a disputa já possui dados bancários associados para acordo.            |
| EXPIRED         | Indica que a disputa atingiu o tempo limite para negociar na plataforma. O sistema move as disputas para este status automaticamente quando é atingido o tempo limite de negociaão                                                                            |
| SETTLED         | Indica que o negociador/mediador sinalizou que a disputa foi finalizada como ganha. Isto depende de cada operação, mas na maioria dos casos, só ocorre após protocoloar a minuta assinada.                                                                    |
| UNSETTLED       | Indica que o negociador/mediador sinalizou que a disputa foi finalizada como perdida. Neste caso, a negociação é infrutífera.                                                                                                                                 |
| PRE_NEGOTIATION | Indica que a disputa ainda não entrou em negociação proque detectou alguma anomalia pré configurada. Geralmente, um processo que já tem sentença ou alguma configuração definida pela empresa (ex: não quero negociar com o advogado com uma determinada OAB) |
| CANCELED        | Indica que a disputa teve a negociação cancelada. Este status ocorre após uma disputa entrar em pré negociação e o negociador/mediador decidir que não está apta para ser negociada.                                                                          |

### DisputeAlertType
Enum que representa o tipo de alerta em um participante da disuta
| **valor**       | **Descrição**                                                                                                                                        |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| NAMESAKE        | Alerta indica que a parte é um homônimo e não possui um CPF. O sistema não será capaz de enriquecer os dados de contato da parte                     |
| VEXATIOUS_PARTY | Indica que a parte é um possível litigante de má fé ou advogado "fidelizado". São pessoas que possuem um grande números de disputas associadas a ela |

### DisputeParty
Enum que representa a polaridade de um participante da disputa
| **valor**  | **Descrição**                                                                                                                               |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| RESPONDENT | Participante da disputa atua no polo passivo (réu)                                                                                          |
| CLAIMANT   | Participante da disputa atua no polo ativo (parte contrária)                                                                                |
| UNKNOWN    | Sistema da Projuris Acordos não conseguiu definir a polaridade da parte. Isto acontece porque alguns tribunais não fornecem estes dados para consulta |

### PersonType
Enum que representa o tipo de pessoa
| **valor** | **Descrição**              |
| --------- | -------------------------- |
| NATURAL   | Representa pessoa física   |
| LEGAL     | Representa pessoa jurídica |


### DisputeRoleName
Enum que representa o papel
| **valor**  | **Descrição**                                                                             |
| ---------- | ----------------------------------------------------------------------------------------- |
| PARTY      | Indica que o participante da disputa está com papel de parte (autor ou réu) na disputa    |
| LAWYER     | Indica que o participante da disputa está com o papel de advogado na disputa              |
| NEGOTIATOR | Indica que o participante da disputa está com o papel de negociador/negociador na disputa |


### Direction
Enum que representa a direção de uma interação na disputa
| **valor** | **Descrição**                                                                        |
| --------- | ------------------------------------------------------------------------------------ |
| INBOUND   | Indica que é uma interação de entrada. Ex: recebimento de uma nova mensagem WhatsApp |
| OUTBOUND  | Indica que é uma interação de saída. Ex: envio de um email                           |


### ContetType
Enum que representa o tipo de conteúdo de uma mensagem
| **valor** | **Descrição**                       |
| --------- | ----------------------------------- |
| TEXT      | Indica um conteúdo tipo texto plano |
| HTML      | Indica um conteúdo tipo HTML        |
| JSON      | Indica um conteúdo tipo JSON        |
| XML       | Indica um conteúdo tipo XML         |


### CommunicationMessageStatus
Enum que representa o status de uma mensagem no sistema da Projuris Acordos
| **valor**         | **Descrição**                                                                                                                                                                                                                                                                                                |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| WAITING           | Indica que a mensagem está aguardando o horário de envio. Ocorre em mensagens que são agendadas para serem enviadas.                                                                                                                                                                                         |
| PROCESSED         | Indica que a mensagem foi enviada. Este status é específico para mensagens automáticas, que são agendadas (ficam em WAITING) e quando chegam o horário do agendamento são enviadas. Também é o status de mensagens recebidas pelo sistema.                                                                   |
| PROCESSED_BY_USER | Indica que a mensagem foi enviada manualmente. Um negociador/mediador enviou uma mensagem manualmente para um determinado email/telefone                                                                                                                                                                     |
| FAILED            | Indica que a Projuris Acordos recebeu um erro ao tentar realizar o envio da mensagem. Geralmente ocorre devido a emails/números inválidos. Após o envio (PROCESSED), o provedor de emails ou o facebook(whatsapp) nos informa que o endereço é inválido, então a mensagem muda para o status de falha.                 |
| CANCELED          | Indica que a mensagem foi cancelada. Isto acontece com mensagens agendadas. Quando a negociaão é iniciada, as mensagens de convite de negociação que estão agendadas são todas canceladas                                                                                                                    |
| RETRYING          | Status temporário. Indica que o sistema da Projuris Acordos está tentando reenviar uma mensagem que entrou em falha. Isto normalmente acontece com serviços do WhatsApp, onde alguns números estão cadastrados sem o nono dígito - Quando detectado pelo facebook, nós tentamos reenviar a mensagem com o nono dígito. |

### InteractionType
Enum que representa os tipos de interações possíveis
| **valor**                | **Descrição**                                                                                                                                                                                                 |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| COMMUNICATION            | Indica que a interação foi uma mensagem (ex: envio/recebimento de um email)                                                                                                                                   |
| VISUALIZATION            | Indica que a interação foi a leitura de uma mensagem (ex: leitura de um email ou de uma mensagem no WhatsApp)                                                                                                 |
| NEGOTIATOR_ACCESS        | Indica que a interação foi um acesso no portal de negociação                                                                                                                                                  |
| NEGOTIATOR_PROPOSAL      | Indica que a interação foi uma proposta no portal de negociação                                                                                                                                               |
| NEGOTIATOR_COUNTERPROSAL | Indica que a interação foi uma contra proposta no portal de negociação                                                                                                                                        |
| MANUAL_PROPOSAL          | Indica que a interação foi uma proposta manual, onde o negociador/mediador informou manualmente a contraproposta na parte. Normalmente ocorre quando o negociador liga para a parte e conversa com ela        |
| MANUAL_COUNTERPROPOSAL   | Indica que a interação foi uma contra proposta manual, onde o negociador/mediador informou manualmente a contraproposta na parte. Normalmente ocorre quando o negociador liga para a parte e conversa com ela |
| NEGOTIATOR_ACCEPTED      | Indica que a interação foi o aceite de uma proposta no portal de negociações                                                                                                                                  |
| NEGOTIATOR_CHECKOUT      | Indica que a interação foi o cadastro de contas bancárias para depósito do acordo no portal de negociação                                                                                                     |
| CLICK                    | Indica que a interação foi um click em algum link enviado para a parte                                                                                                                                        |
| SCHEDULER                | Indica que a interação está agendada. Neste momento, ainda não é considerada uma interação, mas uma mensagem que será enviada no futuro                                                                       |
| ATTACHMENT               | Indica que a interação foi o envio de um anexo na disputa. Ex: parte enviou um documento pelo portal de negociações                                                                                           |
| NPS                      | Indica que a interação foi o de uma avaliação NPS. A avaliação NPS é realizada pelo portal de negociação e avalia o atendimento do negociador/mediador na disputa                                             |


### CommunicationType
Enum que representa os tipos de comunicações possíveis
| **valor**          | **Descrição**                                                              |
| ------------------ | -------------------------------------------------------------------------- |
| EMAIL              | Indica que a comunicação foi um email                                      |
| SMS                | Indica que a comunicação foi um SMS                                        |
| WHATSAPP           | Indica que a comunicação foi um WhatsApp                                   |
| NEGOTIATOR_MESSAGE | Indica que a comunicação foi uma mensagem no chat do portal de negociações |


### InteractionPropertyType
Enum que representa os metadados disponíveis de uma interação
| **valor**      | **Descrição**                                                                                     |
| -------------- | ------------------------------------------------------------------------------------------------- |
| LINK           | Indica que o valor é um link contendo um endereço na internet                                     |
| VALUE          | Indica um valor monetário                                                                         |
| DEVICE         | Indica dados do dispositivo que fez a interação                                                   |
| PERSON_NAME    | Indica o nome da pessoa que fez a interação                                                       |
| NOTE           | Indica notas/comentários/observações realizadas na interação                                      |
| BANK_INFO      | Indica um sumário com os dados bancários informados na interação                                  |
| USER           | Indica o usuário que fez a interação                                                              |
| DOCUMENT_ID    | Indica o id do documento referenciado na interação. É gerado quando envia um anexo na disputa     |
| ROLE_ID        | Indica o id do participante da disputa [DisputeRoleDto](#DisputeRoleDto) que realizou a interação |
| PERSON_EMAIL   | Indica o email de quem realizou a interação                                                       |
| NPS_COMMENT    | Indica o comentário realizado numa avaliação NPS                                                  |
| NPS_REASONS    | Indica os motivos informados na avaliação NPS                                                     |
| NPS_STARS      | Indica a quantidade de estrelas enviadas na avaliação NPS. Varia de 1-5                           |
| NPS_REPLY      | Indica a réplica ou resposta do negociador/mediador numa avaliação NPS                            |
| NPS_REPLY_DATE | Indica a data hora que a avaliação NPS foi respondida                                             |


### CommunicationParameterType
Enum que representa os metadados disponíveis em uma mensagem enviada ou recebida
| **valor**        | **Descrição**                                                                 |
| ---------------- | ----------------------------------------------------------------------------- |
| FAILED_SEND      | Detalhes da falha na entrega da mensagem para o destinatário                  |
| SEND_DATE        | Data da confirmação do envio                                                  |
| CC               | Endereço em cópia da mensagem                                                 |
| CCO              | Endereço em cópia da mensagem                                                 |
| RECEIVER_NAME    | Nome do destinatário                                                          |
| SENDER_NAME      | Nome do remetente (informado pelo servidor de emails e WhatsApp)              |
| SENDER_EMAIL     | Endereço alternativo informado pelo servidor de email                         |
| READ_DATE        | Data da leitura da mensagem. Informado pelo servidor de email e pelo WhatsApp |
| CLICKED_DATE     | Data de click na mensagem. Informado pelo servidor de email e pelo WhatsApp   |
| RECEIVER         | Endereço que irá receber a mensagem                                           |
| SENDER           | Iendereço que está enviando a mensagem                                        |
| RECEIVER_DATE    | Data de confirmação de recebimento da mensagem                                |
| EMAIL_ACCOUNT_ID | ID da conta de emails que enviou a mensagem (enviado pelo provedor de emails) |
| MESSAGE_ID       | ID da mensagem na caixa de entrada (provedor de emails)                       |
| SUBJECT          | Assunto da mensagem                                                           |
| ATTACHMENT_COUNT | Indica a quantidade de anexos que vieram na mensagem                          |
| IN_REPPLY_TO     | Indica se a mensagem é uma resposta de outra mensagem                         |


### BrazilianState
Enum que representa os estados da OAB (seccionais).
Esta enum não documentaremos a descrição por ser semântico.
| **valor** |
| --------- |
| AC        |
| AL        |
| AP        |
| AM        |
| BA        |
| CE        |
| DF        |
| ES        |
| GO        |
| MA        |
| MT        |
| MS        |
| MG        |
| PA        |
| PB        |
| PR        |
| PE        |
| PI        |
| RJ        |
| RN        |
| RS        |
| RO        |
| RR        |
| SC        |
| SP        |
| SE        |
| TO        |

### BankAccountType
Enum representa o tipo de conta bancária.
| **valor** | **Descrição**                                                                                                      |
| --------- | ------------------------------------------------------------------------------------------------------------------ |
| SAVINGS   | Indica que a conta bancária é do tipo Conta Poupança                                                               |
| CHECKING  | Indica que a conta bancária é do tipo Conta Corrente                                                               |

### WoskapceStatus
Enum que representa o estado da Workspace
| **valor** | **Descrição**                                                                                                      |
| --------- | ------------------------------------------------------------------------------------------------------------------ |
| CREATING  | Indica que a Workspace está sendo configurada                                                                      |
| READY     | Indica que a Workspace está ativa                                                                                  |
| DISABLED  | Indica que a Workspace está desativada                                                                             |

### OccurrenceDTO


| **Atributo**      | **Mandatório** | **Tipo**                                         | **Descrição**                                                              |
|-------------------|----------------|--------------------------------------------------|----------------------------------------------------------------------------|
| archived          | Sim            | Booleano                                         | Indica quando a ocorrência foi arquivada/removida                          |
| disputeId         | Sim            | Númerico                                         | Indica o número da disputa na plataforma                                   |
| description       | Sim            | Alfanumérico                                     | Descrição da ocorrência, trazendo informações sobre o evento               |
| type              | Sim            | [DisputeOccurrenceType](#DisputeOccurrenceType)  | Enum para indicar o tipo da ocorrência                                     |
| executionDateTime | Sim            | Data Hora                                        | Representa o momento em que o evento foi gerado na plataforma              |
| status            | Sim            | [DisputeStatus](#DisputeStatus)                  | Representa o status da disputa                                             |
| properties        | Não            | Mapa                                             | Informações adicionais da ocorrência, representa por um mapa (chave/valor) |
| interaction       | Não            | [DisputeInteractionDto](#DisputeInteractionDto)  | Representa o objeto contendo informações da interação                      |

### DisputeOccurrenceType

| **Valor**   | **Descrição**                                |
|-------------|----------------------------------------------|
| INCIDENT    | Indica ocorrências que são do tipo incidente |
| INTERACTION | Indica ocorrências que são do tipo interação |
| NOTE        | Indica ocorrências provenientes de notas     |
| LOG         | Indica ocorrências que são do tipo log       |
| SUMMARY     | Indica ocorrências que são do tipo resumo    |
| ACTION      | Indica ocorrências que são do tipo ação      |
