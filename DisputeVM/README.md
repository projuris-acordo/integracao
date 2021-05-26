### DisputeVM
Representa uma disputa

|**Atributo**|**Mandatório**|**Tipo**|**Descrição**|
| ------------| ------------ | ------------ | ------------ |
|archived|Sim|Booleano|Indica quando a disputa foi arquivada/removida|
|alwaysContactParty|Sim|Booleano|Indica quando a disputa deve contatar a parte do processo|
|code|Sim|Alfanumérico (255)|Código único da disputa, normalmente utilizado com o número do processo júridico|
|contactPartyWhenInvalidLawyer|Sim|Booleano|Indica quando deve contatar a parte caso não tenha contato do advogado|
|contactPartyWhenNoLawyer|Sim|Booleano|Indica quando deve contatar a parte caso não localize o advogado|
|courtHearingDate|Não|Data hora|Data da audiência|
|denySavingDeposit|Sim|Booleano|Não aceita deposito em conta poupança|
|description|Sim|Alfanumérico| ??? Descrição da disputa
|disputeDealDate|Não|Data hora|Data em que o acordo foi aceito|
|disputeDealValue|Não|Numérico|Valor do acordo|
|denySavingDeposit|Sim|Booleano|Acordo não aceita deposito em conta Poupança|
|disputeNextToExpire|Sim|Booleano|Quando verdadeiro, indica que restam menos de 4 dias para a data limite do acordo|
|disputeUpperRange|Não|Numérico|Valor máximo para realizar um acordo|
|expirationDate|Não|Data hora|Prazo limite para realizar um acordo|
|externalId|Não|Alfanumérico|Chave única do usuário em caso de integração entre sistemas|
|awaitingAnalysis|Sim|Booleano|Proposta aguardando analise da empresa|
|firstClaimant|Não|Alfanumérico|Nome da primeira parte contrária da disputa|
|firstClaimantDocumentNumber|Não|Numérico|CPF/CNPJ da primeira parte contrária da disputa|
|firstClaimantStatus|Sim|ONLINE/OFFLINE|Indica se a parte contrária está interagindo com a plataforma no exato momento|
|firstClaimantLawyer|Não|Alfanumérico|Nome do primeiro advogado da parte contrária da disputa|
|firstClaimantLawyerDocumentNumber|Não|Numérico|CPF/CNPJ do primeiro advogado da parte contrária da disputa|
|firstClaimantLawyerOab|Não|Alfanumérico|Inscrição na OAB do primeiro advogado da parte contrária da disputa|
|firstClaimantLawyerStatus|Sim|ONLINE/OFFLINE|Indica se o advogado da parte contrária está interagindo com a plataforma no exato momento|
|hasDocument|Sim|Booleano|Indica se existe minuta gerada para formalizar o acordo|
|hasOBFInStrategy|Sim|Boolean|Indica se a estratégia da disputa é do tipo 'Obrigação de Fazer'|
|id|Sim|Numérico|Chave interna única da disputa|
|isMy|Sim|Booleano|Indica se a disputa pertence ao usuário logado|
|lastCounterOfferName|Não|Alfanumérico|Nome do responsável por realizar a última contra proposta na disputa|
|lastCounterOfferValue|Não|Numérico|Valor da última contra proposta realizada na disputa|
|lastOfferName|Não|Alfanumérico|Nome do responsável por realizar a última proposta na disputa|
|lastOfferValue|Não|Numérico|Valor da última proposta realizada na disputa|
|lastOfferRoleId|Sim|Numérico|Identificador único da parte que realizou a última proposta na disputa|
|lastVisualizationByNegotiator|Não|Data hora|Momento em que o negociador visualizou a disputa pela última vez|
|mainRespondent|Sim|Alfanumérico|Nome do réu da disputa|
|manualStrategy|Sim|Booleano|Se verdadeiro, bloqueia agendamento de mensagens automáticas para as partes da disputa|
|materialDamage|Não|Numérico|Valor em danos materiais do processo judicial|
|moralDamage|Não|Numérico|Valor em danos morais do processo judicial|
|paused|Sim|Booleano|Se verdadeiro, indica que a disputa está pausada|
|properties|Não|Mapa|Informações adicionais da disputa e processo|
|provisionedValue|?|Numérico| ??? Saldo aprovisionado
|recalculateProposal|?|Booleano| ???
|requestedValue|?|Numérico| ???
|strategyId|Sim|Numérico|Indentificador interno da estratégia da disputa|
|strategyName|Sim|Alfanumérico|Nome da estratégia da disputa|
|tagsIds|Não|Lista numérica|Lista de IDs das etiquetas da disputa|
|updateAt|Não|Data hora|Data hora da última atualização da disputa|
|visualized|Sim|Booleano|Se falso, indica que existem interações não visualizadas na disputa pelo negociador|
|phase|Sim|Alfabetico| ??? "Fase" da disputa 
|signStatus|Não|Alfabetico|Indica o estado da minuta quando enviada para assinatura|
|firstClaimantAlerts|Não|Alfabetico|Indica se existe homônimo (NAMESAKE) ou litigante de má fé (VEXATIOUS_PARTY) na disputa