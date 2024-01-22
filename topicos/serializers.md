# Serializers

**aqui vai esta algumas anotações referentes a [documentação oficial do django restframework](https://www-django--rest--framework-org.translate.goog/api-guide/serializers/?_x_tr_sl=auto&_x_tr_tl=pt&_x_tr_hl=pt-BR) é apenas uma forma de fixar alguns conceitos sobre os assuntos.**

## tabela de conteudos:

1. Serializers
2. ModelSerializers
3. HyperLinkModelSerializer

## Serializers

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

### Salvando Instâncias

Na classe serializadora podemos implementar os metodos `create()` e `update()`.

esses dois metodos são chamados automaticamente quando chamamos o metodo `save()` da instâcia da classe serializadora quando ela é valida pelo metodo `is_valid()`. quando passamos apenas um argumento na classe serializadora como um **data** que contem um dicionario python com os dados do modelo, quando verificamos se o serializador é valido, apos usar o metodo `save()` criamos e retornamos uma nova instância do modelo. já quando passamos dois argumentos para a classe serializadora, **instance**, **data**, quando o serializador é valido e usamos o metodo `save()` estamos atualizando a instância passada como argumento.

### Validação

quando estamos desserializando precisamos sempre verificar se o serializador é valido usado o `is_valid()`

podemos criar validações de campos do seu model. basta criar a função em sua subclasse serializadora e criar um metodo que inicie com `validate_<nome do campo>()`. essa função deverá retornar obrigatoriamente um valor validado ou uma exceção do tipo `serializers.ValidationError`.

caso queira personalizar sua validação, você poderá "sobreescrever" a função `validate(data)` em sua subclasse serializadora. no qual data é um dicionario de valores sobre o seu modelo. da mesma forma que a função especifica para um campo especifico, deve-se retornar o dicionario com os valores validados ou uma exeção do tipo `serializers.ValidationError`.

### Lidando com vários objetos aninhados

objetos aninhados em DRF é a forma de tratar os relacionamentos entre os modelos. vou criar um exemplo para ficar mais claro.

temos dois modelos, um `compositor` e `musica`. temos o seguinte relacionamento: o compositor pode escrever varias musicas, porém uma musica só pode ser escrita por um compositor. temos um relacionamento de um para muitos. com isso temos que usar a chave estrangeira em musica que referencie o compositor. temos os modelos dado por:

**models.py**
```python
class Compositor(models.Model):
    nome_compositor = models.CharField(max_lenght=200)

class Musica(models.Model):
    nome_musica = models.CharField(max_lenght=200)
    nome_compositor = models.ForeignKey(Compositor, on_delete=models.CASCADE)
```

então vamos criar os seguintes serializadores:

**serializers.py**
```python
class CompositorSerializer(serializers.Serializer):
    nome_compositor = serializers.CharField(max_lenght=200)

class MusicaSerializer(serializers.Serializer):
    nome_musica = serializers.CharField(max_lenght=200)
    nome_compositor = CompositorSerializer()
```

essa abordagem pode ser atribuida para os três tipos de relacionamentos: `um para um`, `um para muitos` e `muitos para muitos`. no caso de `muitos para muitos` devemos usar o atributo `many=True`.

esse sinalizador `many=True` é usado para serializar varios objetos.

## ModelSerializers

O modelSerializer é uma forma mais pratica de criar serializadores. com ela, o mapeamento dos campos é feito atraves do modelo informado no metadados da classe:

```python
class MusicaSerializer(serializers.ModelSerializer):
    class Meta:
        model = Musica
        fields = [
            'nome_musica',
            'nome_compositor'
        ]
```

o atributo `fields` definem quais serão os campos do modelo que será serializado. já o atributo `exclude` deixa todos os campos menos os definidos nela.

podemos definir um campo de apenas leitura usando o `read_only` ou s quiser definir mais de um campo no modelSerializer no metadados podemos usar o `read_only_fields = `

## HyperLinkModelSerializer