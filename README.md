# Visualização de Diagrama Entidade Relação (DER)

descarregar (graphviz)[https://graphviz.org/download/]

instalação dos modulos:
```
pip install pygraphviz
pip install pydot
pip install django-extensions
```


no ficheiro settings.py:

```Python
# settings.py
INSTALLED_APPS += ['django_extensions']

GRAPH_MODELS = {
  'all_applications': True,
  'group_models': True,
}
```

para gerar uma imagem do DER deve executar os seguintes comandos na consola:
```bash
> python manage.py graph_models -a -o myapp_models.dot
> dot -Tpng myapp_models.dot -o output.png
```



# Exemplos


### Família-Morada (1:1)
```python

class Familia(models.Model):
    nome = models.CharField(max_length=100)

    def __str__(self):
        return self.nome


class Morada(models.Model):
    rua = models.CharField(max_length=100)
    localidade = models.CharField(max_length=100)
    familia = models.OneToOneField(Familia,
                                   on_delete=models.CASCADE,
                                   related_name='morada')

```
<img width="180" alt="familia-morada" src="https://github.com/ULHT-PW/diagrama-entidade-relacao/assets/42048382/ce3645b9-aecb-4857-bd94-96a23f996dc0">

### Aluno-Disciplina (N:N)
```python
class Aluno(models.Model):
    nome = models.CharField(max_length=100)

    def __str__(self):
        return self.nome

class Disciplina(models.Model):
    nome = models.CharField(max_length=100)
    alunos = models.ManyToManyField(Aluno,
                              related_name='disciplinas')

    def __str__(self):
        return self.nome
```

<img width="150" alt="disciplina-aluno" src="https://github.com/ULHT-PW/diagrama-entidade-relacao/assets/42048382/17f66397-7b55-4092-990d-ca943bbf9bd8">

### Escritor-Livro (1:N)
```python
class Escritor(models.Model):
    nome = models.CharField(max_length=100)

    def __str__(self):
        return self.nome

class Livro(models.Model):
    titulo = models.CharField(max_length=100)
    escritor = models.ForeignKey(Escritor, on_delete=models.CASCADE,
                                     related_name='livros')

    def __str__(self):
        return self.titulo
```


<img width="159" alt="livro-escritor" src="https://github.com/ULHT-PW/diagrama-entidade-relacao/assets/42048382/30bb8ae2-5ec7-4193-835c-d3951d8ff73f">

