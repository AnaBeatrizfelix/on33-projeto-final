
# Análise de Dados de Filmes

## Importação de Bibliotecas
```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from scipy.stats import ttest_ind
```

## Leitura do Arquivo CSV
```python
# Leitura do arquivo CSV, separado por ponto e vírgula
df = pd.read_csv("catalogo_filmes.csv", sep=';')  
print(df.head(15))  # Exibir as primeiras 15 linhas
print(df.shape)  # Quantidades de linhas e colunas
```

## Análise de Dados Nulos
```python
# Contar dados nulos em cada coluna
nulos_por_coluna = df.isnull().sum()
print("Nulos por coluna:
", nulos_por_coluna)

# Contar dados nulos por linhas
nulos_por_linhas = df.isnull().sum(axis=1)
print("Nulos por linha:
", nulos_por_linhas)

# Descrição de dados
print(df.describe())

# Verificar informações
info_df = df.info()
print(info_df)
```

## Renomeação das Colunas
```python
# Renomear as colunas
df.rename(columns={
    'Filmes ': 'Titulos de Filmes', 
    'Ano ': 'Ano de Estreia',
    'Categoria':'Genero', 
    'Pontos': 'Audiencia', 
    'Nacionalidade': 'Origem'
}, inplace=True)

# Salvar no CSV
df.to_csv('catalogo_tratado.csv', index=False)

# Verificando se os nomes foram alterados corretamente
print("Colunas após renomeação:
", df.columns)
```

## Quantidade de Filmes Estrangeiros
```python
# Quantos filmes estrangeiros existem no catálogo
contagem_estrangeiros = df["Origem"].value_counts()

# Gráfico de contagem de filmes por origem
contagem_estrangeiros.plot(kind="bar", edgecolor="blue", color="red")
plt.xlabel("Origem")
plt.ylabel("Quantidade")
plt.title("Quantitativo de Filmes por Origem")
plt.xticks(rotation=45)  # Para melhor visualização
plt.grid(axis='y')
plt.show()
```

## Filme Mais Antigo do Catálogo
```python
# Localizando o filme mais antigo
filme_mais_antigo = df.loc[df['Ano de Estreia'].idxmin()]

# Buscando informações do filme mais antigo
titulo = filme_mais_antigo['Titulos de Filmes']
ano = filme_mais_antigo['Ano de Estreia']

# Gráfico do filme mais antigo
plt.figure(figsize=(8, 5))
plt.bar(titulo, ano, color='lightblue', edgecolor='blue')
plt.xlabel("Título de Filmes")
plt.ylabel("Ano de Estreia")
plt.title("Ano de Estreia do Filme Mais Antigo")
plt.text(titulo, ano + 0.1, str(ano), ha='center', color='black')
plt.grid(axis='y')
plt.show()
```

## Remover Espaços dos Nomes das Colunas
```python
# Remover espaços dos nomes das colunas
df.columns = df.columns.str.strip()
```

## Cálculo da Média do Índice de Audiência por Gênero
```python
# Calcular a média do índice de audiência por gênero
media_audiencia_por_genero = df.groupby('Genero')['Audiencia'].mean().reset_index()

# Plotagem da média do índice de audiência por gênero
plt.figure(figsize=(10, 6))
plt.bar(media_audiencia_por_genero['Genero'], media_audiencia_por_genero['Audiencia'], color='pink', edgecolor='blue')
plt.xlabel("Gênero")
plt.ylabel("Índice de Audiência Média")
plt.title("Índice de Audiência Médio por Gênero")
plt.xticks(rotation=45)
plt.grid(axis='y')
plt.show()
```

## Comparação de Índices de Audiência entre Gêneros
```python
# Amostras
audiencia_acao = df[df['Genero'] == 'Ação']['Audiencia'].dropna()
audiencia_terror = df[df['Genero'] == 'Terror']['Audiencia'].dropna()

# Teste t 
estatistica_t, valor_p = ttest_ind(audiencia_acao, audiencia_terror)

print("Teste T de Índice de Audiência")
print(f"Estatística T: {estatistica_t:.3f}")
print(f"Valor P: {valor_p:.3f}")

# Gráfico da distribuição de audiência
plt.figure(figsize=(10, 6))
sns.histplot(audiencia_acao, color='blue', label='Ação', bins=18, kde=True)
sns.histplot(audiencia_terror, color='red', label='Terror', bins=18, kde=True)

# Rótulo
plt.legend()
plt.title("Distribuição do Índice de Audiência dos Gêneros")
plt.xlabel("Índice de Audiência")
plt.ylabel("Contagem")
plt.grid(axis='y')
plt.show()

# Interpretação do teste t
if valor_p < 0.05:
    print("Rejeitamos a hipótese nula: Há uma diferença significativa entre os índices de audiência.")
else:
    print("Não rejeitamos a hipótese nula: Não há diferença significativa entre os índices de audiência.")
```

## Conclusão
Esta análise permite entender melhor o catálogo de filmes, incluindo a quantidade de filmes estrangeiros, o filme mais antigo e a média de audiência por gênero. O teste t fornece insights sobre diferenças significativas nos índices de audiência entre os gêneros de ação e terror.
