# Serializers

**aqui vai esta algumas anotações referentes a [documentação oficial do django restframework](https://www-django--rest--framework-org.translate.goog/api-guide/serializers/?_x_tr_sl=auto&_x_tr_tl=pt&_x_tr_hl=pt-BR) é apenas uma forma de fixar alguns conceitos sobre os assuntos.**

basicamente um serializer é um elemento que transforma consultas do banco e instância de modelos do django em dados nativo do python para poderem ser transformados facilmente em `JSON` ou `XML`.

para usarmos o serializador temos que importa-los:

```python
from rest_framework import serializers
```

a forma como declaramos um serializador é bastante similar a criação de um model, uma class form...

```python
class ModelSerializer(serializers.Serializer):
    campo_do_model = serializers.CharField(max_length=200)
    ...
```

ou seja, esse será um serializer de um tal modelo, quando quisermos serializar ou deserializar dados desse modelo precisamos passa-ló para essa classe.

para finalizar o processo, precisamos renderizar os dados obtidos pelo `.data` da instâcia do serializer em `JSON`. para isso usamos:

```python
from rest_framework import JSONRenderer

json = JSONRenderer.render(instaciadoserializer.data)
```

```
vale resaltar a diferença entre o `instanciadoserializer.data` e da variavel `json`. o instanciadoserializer.data possui os dados do modelo serializado e esta em formato de algum dado em python como dicionario, vetores... já o json tambem possui os dados do modelo serializado porém está no formato JSON.
```

## Salvando Instâncias

Na classe serializadora podemos implementar os metodos `create()` e `update()`.

esses dois metodos são chamados automaticamente quando chamamos o metodo `save()` da instâcia da classe serializadora quando ela é valida pelo metodo `is_valid()`. quando passamos apenas um argumento na classe serializadora como um **data** que contem um dicionario python com os dados do modelo, quando verificamos se o serializador é valido, apos usar o metodo `save()` criamos e retornamos uma nova instância do modelo. já quando passamos dois argumentos para a classe serializadora, **instance**, **data**, quando o serializador é valido e usamos o metodo `save()` estamos atualizando a instância passada como argumento.

## Validação

quando estamos desserializando precisamos sempre verificar se o serializador é valido usado o `is_valid()`

podemos criar validações de campos do seu model. basta criar a função em sua subclasse serializadora e criar um metodo que inicie com `validate_<nome do campo>()`. essa função deverá retornar obrigatoriamente um valor validado ou uma exceção do tipo `serializers.ValidationError`.

caso queira personalizar sua validação, você poderá "sobreescrever" a função `validate(data)` em sua subclasse serializadora. no qual data é um dicionario de valores sobre o seu modelo. da mesma forma que a função especifica para um campo especifico, deve-se retornar o dicionario com os valores validados ou uma exeção do tipo `serializers.ValidationError`.

## Lidando com vários objetos aninhados