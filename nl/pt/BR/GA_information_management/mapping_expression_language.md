---

copyright:
  years: 2016, 2018
lastupdated: "2018-03-26"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Entendendo a linguagem de expressão de mapeamento
{: #mapping_expression}

É possível usar a linguagem de expressão de mapeamento para manipular e combinar dados e para formatar os resultados de quaisquer consultas que você possa executar em seus dados processados. A linguagem de expressão de mapeamento é um subconjunto de [JSONata ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.jsonata.org/index.html){:new_window} e pode ser usada ao definir [mapeamentos](ga_im_definitions.html#resources) ou criar [regras](../information_management/im_rules.html). 

JSONata é uma linguagem leve de consulta e transformação para dados JSON. Uma ferramenta [JSONata Exerciser ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://try.jsonata.org/){:new_window} também está disponível, fornecendo uma maneira rápida e conveniente de experimentar o JSONata.

As informações a seguir mostram os operadores-chaves e as funções que são
atualmente suportadas, com alguns exemplos de como você poderá usá-los. 

## Operadores JSONata
{: #operators}

Todos os [Operadores do JSONata ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.jsonata.org/operators.html){:new_window} são suportados, exceto os operadores a seguir:

- ~ > (encadeamento de função)
- ^ (...) (order-by)
- : = (ligação de variável)

** Nota: ** 
- Use parênteses () para agrupar expressões e para alterar a precedência do operador
- Use aspas simples ou duplas para circundar sequências e nomes de propriedades, por exemplo, $event.object.'ab' 
- Use crases para circundar nomes de propriedades que contêm caracteres especiais, incluindo espaços, por exemplo 
```
{2}{2}.` s y `
```
- Use crases para circundar nomes de propriedade que iniciam com um número, por exemplo
```
"7emperatura"
```

## Funções do JSONata
{: #functions}
As funções do JSONata a seguir são suportadas: 

 - Todas as funções de [Sequência ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.jsonata.org/string-functions.html){:new_window}
 - Todas as funções [Numéricas ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.jsonata.org/numeric-functions.html){:new_window} 	
 - Todas as funções de [Agregação numérica ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.jsonata.org/aggregation-functions.html){:new_window} 
 - Todas as funções [Booleanas ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.jsonata.org/boolean-functions.html){:new_window}  
 - Funções de [Matriz $count e $append ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.jsonata.org/array-functions.html){:new_window} 	

## Estendendo a Linguagem de Expressão

A linguagem de expressão de mapeamento foi estendida para uso com o recurso de
gerenciamento de dados por meio da introdução das variáveis $event, $state e $instance. O
JSON é ligado a essas variáveis antes de a expressão ser avaliada. A tabela a seguir
fornece uma visão geral dessas variáveis, que são definidas para uso em expressões:

Variável                   | Exemplo de JSON de entrada     | Exemplo como uma expressão    | Use para...   
------------- | ------------- | -------------
$event | *{"t": 34.5}*  | $event.t | Selecione uma propriedade de um evento de entrada para usar em uma expressão. 
$state | *{"temperature": 34.5,"humidity": 78 }*  | $state.temperature | Selecione uma propriedade no estado do dispositivo para usar em uma expressão.
$instance | *{"deviceId": "tSensor","typeId": "humiditySensor","metadata": {"temp_adjustment": 50}}*  | $instance.metadata.temp_adjustment | Selecione o atributo de dispositivo ou tipo de dispositivo para uso em uma expressão. 

O exemplo a seguir usa o objeto a seguir como a entrada para um evento:  
```
 {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }

```
O objeto é convertido em uma matriz usando a expressão a seguir:

```
  [ $event.temperature, $event.humidade, $event.pressionar ]
```  
A expressão resulta na geração da saída a seguir:
```
 [
    35,
    72,
    1024
  ]
```

É possível inverter esse exemplo e converter uma matriz da entrada para um objeto. O exemplo a seguir usa a matriz a seguir como a entrada para um evento:
```
{
    "leituras": [
      35,
      72,
      1024
    ]
  }
```
A matriz é convertida para um objeto usando a expressão a seguir:
```
 {0}], "humidade": $event.leituras [ 1 ], "pressão": $event.leituras [ 2 ] }
```
 A expressão resulta na geração da saída a seguir:
  ```
  {
    "temperature": 35,
    "humidity": 72,
    "pressure": 1024
  }
  
 ```
É possível também definir uma expressão que combine o uso dessas variáveis. No exemplo a seguir, uma propriedade *temp_adjustment* é definida nos metadados do dispositivo e é usada para calibrar a leitura do evento. A propriedade é definida em um mapeamento, mas pode ser aplicada a muitos dispositivos. 

```
"propertyMappings" : {
        "tevt" : {
           "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```


O operador dot "." é usado para acesso ao objeto com uma chave literal, por exemplo, $event.object.hh. A expressão no lado esquerdo é restringida a uma propriedade específica, no evento ($event) ou no estado ($state) ou nos metadados ($instance). A expressão no lado direito contém as informações que você pode desejar acessar.

Para obter mais informações sobre o operador dot, veja a seção [Operadores ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.jsonata.org/operators.html){:new_window} da documentação do JSONata.


## Guia do idioma

- Toda [Seleção básica![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.jsonata.org/basic.html){:new_window} é suportada.
- Toda [Seleção complexa![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.jsonata.org/complex.html){:new_window} é suportada, exceto para curingas.
- Expressões condicionais e entre parênteses são suportadas como parte de [Construções de programação![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.jsonata.org/programming.html){:new_window}. As variáveis são suportadas por meio do uso das variáveis $instance, $state e $event como parte da linguagem de mapeamento estendido, que é específica para o gerenciamento de dados do Watson IoT Platform. As variáveis "$" e "$$" que são usadas no JSONata não são suportadas atualmente.

## Construindo saída
É possível especificar como os dados processados são apresentados na saída usando construtores de matriz ou construtores de objeto.

### Construtores de matriz suportados
As matrizes podem ser construídas anexando uma lista separada por vírgula de literais ou expressões em um par de colchetes []. As vírgulas são usadas para separar múltiplas expressões dentro do construtor de matriz, incluindo geração de sequência, por exemplo, *[1, 3-1] = [1,2]*, *[1..3] -> [1,2,3]* e *[1..3, 7..9] -> [1,2,3,7,8,9]*.

### Construtores de objetos suportados
{: #constructors}
É possível construir objetos JSON em sua saída usando um par de chaves {} que contêm valores de chave ou pares separados
por vírgulas, com cada chave e valor separados por dois-pontos, por exemplo, *{key1 : value1, key2:value2}* ou *{"hello" : "world"}*. A chave do objeto deve ser uma sequência.  


## Exemplo: usando matrizes para processar e relatar dados de temperatura
As seções a seguir constroem o exemplo em [Introdução ao gerenciamento de dados usando APIs de REST](ga_im_example.html) para mostrar como você pode usar matrizes para manter uma janela deslizante de leituras de temperatura e calcular a soma ou média atual da leitura contida nessa janela.

Uma janela deslizante armazena dados na ordem de chegada. Em vez de manter todos os
dados já inseridos, janelas deslizantes podem ser configuradas para despejar dados de
modo incremental. Quando uma janela deslizante é preenchida, qualquer inserção futura
resulta na remoção do item de dados mais antigo dessa janela.

O exemplo a seguir mostra uma janela deslizante configurada com uma política de
desocupação baseada em contagem de tamanho 5:
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

O exemplo a seguir mostra como a configuração do arquivo de esquema de interface lógica que é mostrada na etapa 7 do [Guia passo a passo 1](ga_im_index_scenario.html#step4) pode ser modificada incluindo uma matriz chamada *tempReadings*. A matriz é usada para manter uma janela dos últimos 5
valores que são enviados dos dispositivos com relação aos últimos 5 eventos. Se não houver valores armazenados em *tempReadings*, a matriz será configurada como [0] e a próxima leitura recebida será anexada à matriz que cresce até que 5 leituras sejam recebidas. Após o recebimento de 5 leituras, uma nova leitura resulta na remoção da leitura mais antiga da janela. 

**Notas Importantes:** deve-se configurar *tempReadings* como "obrigatório" e como uma matriz por padrão.

```
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "definitions": {},
  "properties": {
      "temperature" : {
          "description" : "latest temperature reading in degrees Celsius",
          "type" : "number",
          "minimum" : -273.15,
          "default" : 0.0
      },
      "tempAverage": {
          "type": "number"
      },
      "tempReadings": {
          "default": [],
          "items": {
              "type": "number"
          },
         "type": "array"
      },
      "tempSum": {
          "type": "number"
      }
  },
   "required" : [
      "tempReadings"
  ],
  "type": "object"
}
```
A seção a seguir mostra um exemplo de como você pode configurar o recurso de mapeamentos
para calcular a leitura de temperatura média e a soma de todas as leituras de
temperatura com base nas leituras que estão contidas na janela deslizante atual:


```
[
   {
       "created": "2017-10-13T09:21:36Z",
       "createdBy": "admin",
       "logicalInterfaceId": "5846ec826522050001db0e12",
       "notificationStrategy": "on-state-change",
       "propertyMappings": {
           "tevt" : {
               "tempAverage": "$average($count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t))",
               "tempReadings": "$count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t)",
               "tempSum": "$sum($count($state.tempReadings)<5?$append($state.tempReadings, $event.t):$append($state.tempReadings[[1..4]], $event.t))"
           }
       },
       "updated": "2017-10-13T10:05:40Z",
       "updatedBy": "a-8x7nmj-9iqt56kfil",
       "version": "active"
   }
]
```
**Note:** a matriz *$state.tempReadings* é recalculada antes de ser usada nas funções $average e $sum. O recálculo da matriz será necessário para assegurar que a matriz contenha os valores atuais quando a expressão *tempAverage* ou *tempSum* for avaliada, porque a ordem das expressões de mapeamento não pode ser controlada.

O exemplo a seguir mostra a temperatura média e a temperatura somada com base em uma janela deslizante de 5 leituras de temperatura:
```
{
    "state": {
        "tempAverage": 18557.6,
        "tempReadings": [
            17591,
            10262,
            25621,
            16676,
            22638
        ],
        "tempSum": 92788
    },
    "timestamp": "2017-10-13T11:07:20Z",
    "updated": "2017-10-13T11:05:40Z"
}
```

## Manipulando incompatibilidades entre a expressão de mapeamento e os dados de entrada

Uma atualização de estado pode falhar quando uma das expressões de mapeamento contém uma referência aos dados de entrada que não está especificada no evento publicado.

Por exemplo, você pode configurar as expressões a seguir:
```
temperature = $event.t 
humidity = $event.hum
```
em que *t* é uma propriedade opcional para o evento.

Se um evento que contém somente dados de umidade for recebido, por exemplo, `{"humidity":22}`, a expressão `temperature = $event.t` falhará ao ser avaliada, porque a propriedade *t* opcional não está especificada no evento de dispositivo publicado.

A atualização de estado falha. A propriedade de estado de umidade não é atualizada, e uma mensagem de erro é publicada no tópico de erro do MQTT para o dispositivo:
```
iot-2/type/ ${ /id/ ${ /err/data
```
Para evitar falhas de atualização de estado por causa de dados opcionais não especificados, é possível usar a função $exists em combinação com um condicional ternária para especificar um valor padrão para propriedades opcionais. O exemplo a seguir define um valor padrão de *0* para a propriedade *t*:

```
" tempEvent: {
      "temperature": "$exists($event.t)?$event.t:0",
      "humidity": "$event.hum"
    }
```

Definindo um valor padrão para a propriedade opcional dessa maneira, a expressão é avaliada com êxito, mesmo quando a propriedade *t* não é especificada no evento de dispositivo publicado.

Portanto, se o evento `{"humidity":22}` for recebido, a atualização de estado será concluída com êxito e o estado do dispositivo será configurado como `{"humidity":22, "temperature":0}`.
