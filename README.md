## Documentação para o Sistema de Recomendação de Filmes

### Visão Geral

Este projeto implementa um **Sistema de Recomendação de Filmes** utilizando **Machine Learning**. O algoritmo é baseado em dados de avaliações de usuários e informações de filmes, recomendando títulos similares com base nas preferências de cada usuário. Sistemas de recomendação, como este, são amplamente usados por plataformas como **Netflix** e **Amazon Prime Video** para melhorar a experiência do usuário, oferecendo sugestões personalizadas.

### Como baixar e rodar o projeto

1. **Clone o repositório**:
   ```
   git clone https://github.com/usuario/recomendacao-filmes.git
   ```

2. **Navegue até a pasta do projeto**:
   ```
   cd recomendacao-filmes
   ```

3. **Instale as dependências** utilizando o gerenciador de pacotes **pip**:
   ```
   pip install -r requirements.txt
   ```

4. **Baixe os arquivos de dados** utilizados no projeto:
   - Filmes: [Movies Metadata](https://www.kaggle.com/code/alyssonbispopereira/recomenda-o-de-filmes-ptbr/data)
   - Avaliações: `ratings.csv`

   Coloque esses arquivos no diretório raiz do projeto.

### Bibliotecas utilizadas

As principais bibliotecas que você precisará instalar estão listadas no arquivo `requirements.txt`. As mais importantes incluem:

- **Pandas**: para manipulação e análise dos dados.
- **Numpy**: para operações matemáticas e funções de array.
- **Scikit-learn**: para implementação do algoritmo de recomendação (KNN).
- **Scipy**: para trabalhar com matrizes esparsas (sparse matrices).

### Estrutura dos Dados

Os dados utilizados neste projeto vêm de um conjunto de filmes e avaliações:

- **movies_metadata.csv**: contém informações sobre os filmes, como título, idioma e número de avaliações.
- **ratings.csv**: contém as avaliações dos usuários para diversos filmes.

### Etapas de Implementação

#### 1. Pré-processamento dos dados
Primeiro, os arquivos CSV são carregados e as colunas irrelevantes são descartadas. Os valores nulos são removidos para garantir que apenas dados completos sejam usados no modelo.

```python
import pandas as pd
filmes = pd.read_csv('movies_metadata.csv')
filmes = filmes[['id', 'original_title', 'original_language', 'vote_count']].dropna()

avaliacoes = pd.read_csv('ratings.csv')
avaliacoes = avaliacoes[['userId', 'movieId', 'rating']].drop_duplicates()
```

#### 2. Criação da Matriz Esparsa

A matriz esparsa (`csr_matrix`) é criada para economizar espaço de memória, considerando que a maioria dos valores serão zeros (ou seja, filmes não avaliados por muitos usuários).

```python
from scipy.sparse import csr_matrix
filmes_pivot = avaliacoes_e_filmes.pivot_table(columns="ID_USUARIO", index="TITULO")
filmes_sparse = csr_matrix(filmes_pivot.fillna(0))
```

#### 3. Implementação do Algoritmo KNN

Utilizamos o **Nearest Neighbors (KNN)** para encontrar filmes similares com base nas avaliações de usuários. Este algoritmo mede a distância entre as características dos filmes e encontra os mais próximos.

```python
from sklearn.neighbors import NearestNeighbors
modelo = NearestNeighbors(algorithm='brute')
modelo.fit(filmes_sparse)
```

#### 4. Geração de Recomendações

Após o treinamento do modelo, podemos gerar recomendações para qualquer filme. Por exemplo, se quisermos recomendações para o filme **"127 Hours"**:

```python
distancia, sugestoes = modelo.kneighbors(filmes_pivot.filter(items=['127 Hours']))
for i in range(len(sugestoes)):
    print(filmes_pivot.index[sugestoes[i]])
```

### Importância do Sistema de Recomendação

Sistemas de recomendação são essenciais para melhorar a experiência do usuário em plataformas de streaming ou qualquer serviço que ofereça uma grande quantidade de opções. Ao fornecer sugestões personalizadas, o algoritmo ajuda a aumentar o engajamento e a satisfação do usuário.

Empresas como **Netflix**, **Amazon**, e **Spotify** utilizam algoritmos semelhantes para sugerir novos filmes, séries ou músicas, com base no histórico de consumo dos usuários. Esse tipo de sistema também pode ser aplicado a e-commerce, recomendando produtos de acordo com os interesses do cliente.

### Executando o Projeto

Após configurar o ambiente e garantir que os dados estão disponíveis:

1. **Execute o script Python**:
   ```
   python recomendacao_filmes.py
   ```

2. **Adicione um filme para receber recomendações**:
   Edite o código ou insira diretamente o título de um filme na linha:
   ```python
   distancia, sugestoes = modelo.kneighbors(filmes_pivot.filter(items=['FILME']))
   ```

3. **Visualize as sugestões**. O sistema retornará os filmes mais similares com base no histórico de avaliações dos usuários.

Agora, o sistema estará pronto para fornecer recomendações personalizadas com base nos dados fornecidos.
