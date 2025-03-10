---
sites_supported:
  - mlb
---

# Relatório de dinheiro em conta (Settlement Report)

Contém todos os eventos de pagamento (aprovados, devolvidos, em mediação e estornos) que afetam o saldo de sua conta no Mercado Pago no período solicitado.


## Como são utilizados?

Podem ser gerados manualmente (indicando um intervalo específico de datas) ou de forma programada.

Consulte o [glossário dos relatórios](https://www.mercadopago.com.br/ajuda/glosario-reporte-conciliacion_2140).

### Uso manual

#### 1.Para gerar um relatório:
Faça o POST para a API especificando as datas de início e término da seguinte forma:

```php
<?php

// SDK Version >= 0.5.0

$mp = new MP("ACCESS_TOKEN");

$request = array(
        "uri" => "/v1/account/settlement_report",
        "data" => array(
            "begin_date" => "2015-05-01T00:00:00Z",
            "end_date" => "2015-06-01T00:00:00Z"
        )
    );

$mp->post($request);

?>
```

Você receberá um `HTTP STATUS 202 (Accepted)` como resposta e o relatório será gerado de forma assíncrona.

#### 2. Busca:
Para verificar se o relatório foi gerado, você terá que fazer uma consulta à API da seguinte forma:


```php
<?php

// SDK Version >= 0.5.0

$mp = new MP("ACCESS_TOKEN");

$request = array(
        "uri" => "/v1/account/settlement_report/list"
    );

$mp->get($request);

?>
```

Você receberá como resposta:

```json
[
    {
        "id": 1234,
        "user_id": 130379930,
        "begin_date": "2015-05-01T00:00:00Z",
        "end_date": "2015-06-01T00:00:00Z",
        "file_name": "SR-TEST-1234567890123.csv",
        "created_from": "manual",
        "date_created": "2016-01-20T10:07:53.000-04:00"
    },
    {
    	...
    }
]
```

#### 3. Download
Utilizando o atributo `file_name`, você poderá fazer o download do relatório na URL a seguir:

	`https://api.mercadopago.com/v1/account/settlement_report/:file_name?access_token=ACCESS_TOKEN`


### Uso programado

Os relatórios de conciliação também podem ser gerados automaticamente.

#### 1. Para gerar um relatório:

Programe para gerar um relatório automaticamente utilizando a frequência no recurso de configuração. Atualize o atributo `scheduled` na configuração para `true`:


	`POST` /v1/account/settlement_report/schedule

Interrompa a geração automática do relatório. Atualize o atributo `scheduled` na configuração para `false`:

	`DELETE` /v1/account/settlement_report/schedule

#### 2. Download

Faça o download do arquivo especificado:

	`GET` /v1/account/settlement_report/:file_name
