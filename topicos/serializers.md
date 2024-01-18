# Serializers

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

