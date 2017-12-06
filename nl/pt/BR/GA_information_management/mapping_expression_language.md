---

copyright:
  years: 2016, 2017
lastupdated: "2017-11-09"

---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Entendendo a linguagem de expressão de mapeamento
{: #mapping_expression}

É possível usar a linguagem de expressão de mapeamento para manipular e combinar dados e para formatar os resultados de quaisquer consultas que você possa executar em seus dados processados. A linguagem de expressão de mapeamento é um subconjunto de [JSONata ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.jsonata.org/index.html){:new_window} e pode ser usada ao definir [mapeamentos](ga_im_definitions.html#definitions_resources). JSONata é uma linguagem leve de consulta e transformação para dados JSON.

As informações a seguir mostram os operadores-chaves e as funções que são
atualmente suportadas, com alguns exemplos de como você poderá usá-los. 

## Operadores JSONata suportados
{: #operators}

O seguinte subconjunto de operadores JSONata é suportado: 

Tipo de operador                   | operadores suportados     | Notas   
------------- | ------------- | -------------
Aritmético | *+* - / * % | O operador% retorna o resto.
Comparação | < <= > >= != = | O operador de igualdade é =, como é em JSONata.
Booleano | *em*, *e* | As constantes booleanas são *true* ou *false*.
Ternário condicional | ? | O operador ? avalia uma das duas expressões alternativas com base no resultado de uma condição de teste. 
O operador usa o seguinte formato *expression* ? *value_if_true* : *value_if_false*
Seqüência de caracteres | & | O operador & associa os valores de sequência dos operandos a uma única sequência resultante.
Gerador de sequência | .. | Cria uma matriz de números inteiros crescentes, começando com
o número no LHS e terminando com o número no RHS, por exemplo, [1..4] ->
[1,2,3,4]. Os operandos devem ser avaliados como número inteiro. O gerador de sequência só pode ser usado dentro de um construtor de matriz [ ].
Outro | . | O operador ponto é usado para acesso do objeto com uma chave literal, por exemplo, $event.object.hh. *
e:* A expressão do lado esquerdo é restrita a uma propriedade específica, no evento ($event), no estado ($state) ou nos metadados ($instance).

**Notas:** 
- Use parênteses () para agrupar expressões e para alterar a precedência do operador
- Use aspas simples para delimitar os nomes das propriedades que contêm espaços, por exemplo, $event.object.'a b' 

## Estendendo a linguagem de expressão de mapeamento

A linguagem de expressão de mapeamento foi estendida para uso com o recurso de
gerenciamento de dados por meio da introdução das variáveis $event, $state e $instance. O
JSON é ligado a essas variáveis antes de a expressão ser avaliada. A tabela a seguir
fornece uma visão geral dessas variáveis, que são definidas para uso em expressões:

Variável                   | Exemplo de JSON de entrada     | Exemplo como uma expressão    | Use para...   
------------- | ------------- | -------------
$event | *{"t": 34.5}*  | $event.t | Selecione uma propriedade de um evento de entrada para usar em uma expressão. 
$state | *{"temperature": 34.5,"humidity": 78 }*  | $state.temperature | Selecione uma propriedade no estado do dispositivo para usar em uma expressão.
$instance | *{"deviceId": "TemperatureSensor1","typeId": "EnvSensor1","metadata": {"temp_adjustment": 50}}*  | $instance.metadata.temp_adjustment | Selecione o atributo de dispositivo ou tipo de dispositivo para uso em uma expressão. 

É possível também definir uma expressão que combine o uso dessas variáveis. No exemplo a seguir, uma propriedade *temp_adjustment* é definida nos metadados do dispositivo e usada para calibrar a leitura de eventos. A propriedade é definida em um mapeamento, mas pode ser aplicada a muitos dispositivos. 

```
"propertyMappings" : {
        "tevt" : {
           "temperature" : "$event.t + $instance.metadata.temp_adjustment"
        }
     },
     
```

## Funções JSONata suportadas
{: #functions}
O seguinte subconjunto de funções JSONata é suportado: 

Tipo de função                   |Função                   | Descrição | Exemplo
------------- | ------------- | ------------- 
Seqüência de caracteres | $substring(string, start_index[, length]) | Subsequência de sequência, por exemplo, *$substring("Hello World", 3, 5) => "lo Wo"*. 
Seqüência de caracteres | $string(arg) | Converte o argumento em um valor de sequência, por exemplo, *$string(2) => "2"*.
Numérico | $number(arg) | Converte o argumento em um valor numérico se possível, por exemplo, *$number(2) => 2*.
Numérico | $sum(array) | Retorna a soma aritmética de uma matriz de números, por exemplo, *([1,2,3]) = 6*.
Numérico | $average(array) | Retorna o valor médio de uma matriz de números, por exemplo, *([1,2,3]) = 2.0*.
Booleano | $exists(expression) | Retorna *true* se a propriedade na expressão existir; *false*, caso contrário.
Array | $count(array) | Retorna o número de itens no parâmetro de matriz, por exemplo, *([1,2,3,4]) = 4*. Se o parâmetro de matriz não for uma matriz, mas sim um valor de outro tipo JSON, o parâmetro será tratado como uma matriz singleton contendo esse valor e essa função retornará *1*.
Array | $append(array1, array2) | Retorna uma matriz contendo os valores em *array1*, seguido pelos valores em *array2*, Por exemplo, *([1,2], [3,4]) = [1,2,3,4]*. Se um dos parâmetros não for uma matriz, ele será tratado como matriz singleton que contém esse valor, por exemplo, *$append("Hello", "World") => ["Hello", "World"]*.

## Matrizes
Use matrizes JSON para colocar uma coleção de valores em uma ordem especificada. As
matrizes associam cada valor na matriz a um índice ou posição. Para resolver valores
individuais em uma matriz, deve-se especificar o índice usando colchetes após o nome do
campo da matriz. Se os colchetes contiverem um número ou uma expressão avaliada como
número, o número representará o índice do valor a ser selecionado. Uma matriz de números
também pode ser usada como um índice, por exemplo, *["a","b","c"][[1,2]] ->
["b", "c"]*. A matriz *[1,2]* é usada como índice que identifica
quais valores selecionar da matriz *["a","b","c"]*. 

Os índices têm deslocamento zero, de modo que o primeiro valor em uma matriz
*arr* é *arr[0]*, por exemplo,
*[1,2,3][0] -> 1*. Caso não seja um número inteiro, ele será
arredondado para um número inteiro, por exemplo, *[1,2,3][0.9] -> [1]*.

Use índices negativos a serem contados do final da matriz, por exemplo, *[1,2,3][-1] -> 3*. 

Se o índice especificado exceder o tamanho da matriz, nada será selecionado.

## Construindo saída
É possível especificar como os dados processados são apresentados na saída usando
construtores de matriz ou construtores de objeto.

### Construtores de matriz suportados
As matrizes podem ser construídas colocando uma lista separada por vírgula de
literais ou expressões em um par de colchetes [ ]. São usadas vírgulas para separar múltiplas expressões dentro do construtor de matriz, por exemplo, *[1, 3-1] = [1,2]*.

### Construtores de objeto suportados
{: #constructors}
É possível construir objetos JSON em sua saída usando um par de chaves {} que contém
valores ou pares de chaves separados por vírgulas, com cada chave e valor separados por
dois-pontos, por exemplo, *{key1 : value1, key2:value2}* ou *{"hello"
: "world"}*. A chave do objeto deve ser uma sequência.  


## Exemplo: usando matrizes para processar e relatar dados de temperatura
As seções a seguir se baseiam no exemplo em
[Introdução ao gerenciamento de dados](ga_im_example.html) para mostrar
como você pode usar matrizes para manter uma janela deslizante de leituras de
temperatura e calcular a soma ou média atual da leitura contida nessa janela. 

Uma janela deslizante armazena dados na ordem de chegada. Em vez de manter todos os
dados já inseridos, janelas deslizantes podem ser configuradas para despejar dados de
modo incremental. Quando uma janela deslizante é preenchida, qualquer inserção futura
resulta na remoção do item de dados mais antigo dessa janela.

O exemplo a seguir mostra uma janela deslizante configurada com uma política de
desocupação baseada em contagem de tamanho 5:
```
() -> (1) -> (2, 1) -> (3, 2, 1) -> (4, 3, 2, 1) -> (5, 4, 3, 2, 1) -> (6, 5, 4, 3, 2) -> (7, 6, 5, 4, 3) -> ...
```

O exemplo a seguir mostra como a configuração do arquivo de esquema de interface
lógica que é mostrada na etapa 7 do [Guia
passo a passo](ga_im_index_scenario.html#step4) pode ser modificado incluindo uma matriz chamada
*tempReadings*. A matriz é usada para manter uma janela dos últimos 5
valores que são enviados dos dispositivos com relação aos últimos 5 eventos. Se não
houver valores armazenados em *tempReadings*, a matriz será configurada como
[0] e a próxima leitura recebida será anexada à matriz que cresce até 5 leituras serem
recebidas. Após o recebimento de 5 leituras, uma nova leitura resulta na remoção da leitura mais antiga da janela. 

**Notas importantes:** deve-se configurar *tempReadings* como "necessário" e como uma matriz por padrão. 

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
**Nota:** a matriz *$state.tempReadings* é recalculada
antes de ser usada nas funções $average e $sum. O recálculo da matriz é necessário para
assegurar que a matriz contenha os valores atuais quando a expressão
*tempAverage* ou *tempSum* é avaliada, porque a ordem das
expressões de mapeamento não pode ser controlada.
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

