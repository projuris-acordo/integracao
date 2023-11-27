# Integração Projuris Acordos - Documentos e anexos da disputa

> Se você estiver procurando a documentação da API de importação, vá para o [documento principal](../readme.md).

## Enviando um PDF para minuta na disputa

É possível enviar um arquivo PDF para ser assinado como termo de acordo da minuta.
Para enviar o documento PDF como minuta e solicitar ao sistema que faça a gestão das assinaturas, utilize a API do serviço office conforme descrito abaixo:

Verbo da API: `POST`

URL da API: `https://backend.justto.app/api/office/documents/dispute/{disputeId}/sign/external-term`

Note que a URL é formada pelo ID interno da disputa na Projuris Acordos.

Os dados de assinantes e do endereço do arquivo PDF que será utilizado como minuta deve ser enviado no payload da request.

Exemplo de Payload:

```
{
	"url": "https://storage.googleapis.com/justto-app/minha-minuta.pdf",
	"signers": [
		{
			"name": "Lucas Israel",
			"email": "lucas@justto.com.br",
			"documentNumber": "36866231884"
		}
	]
}
```

Pode ser enviado quantos assinantes desejar na lista de assinantes. Sempre respeitando documento (mandatório), email(mandatório) e nome.
