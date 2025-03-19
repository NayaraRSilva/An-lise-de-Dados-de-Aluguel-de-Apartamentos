# Análise de Dados de Aluguel de Apartamentos

Este projeto visa realizar uma análise de dados de aluguel de apartamentos. Utiliza-se a biblioteca `pandas` para manipulação de dados e `seaborn` e `matplotlib` para visualizações gráficas. O código trata de dados relacionados a aluguéis de casas, como preços, áreas, quantidade de quartos, banheiros, entre outros.

## Requisitos

Antes de rodar o código, você precisará instalar as bibliotecas necessárias:

## Passos do Código

### 1. Carregar o arquivo Excel
O arquivo Excel contendo os dados dos apartamentos é carregado para um DataFrame utilizando o pandas.

aluguel_casa = pd.read_excel(r"C:\Users\nayar\Downloads\houses_rent.xlsx")

### 2. Visualizar as primeiras e últimas linhas
Exibimos as 5 primeiras e as 5 últimas linhas do DataFrame para inspecionar os dados iniciais.

print(aluguel_casa.head())
print(aluguel_casa.tail())

### 3. Renomear Colunas
Renomeamos algumas colunas para remover espaços em branco e garantir que todas as colunas tenham nomes consistentes.

aluguel_casa = aluguel_casa.rename(columns={"parking spaces":"parking_spaces", "rent amount": "rent_amount", "property tax": "property_tax", "fire insurance":"fire_insurance"})

### 4. Verificar Tipo de Dados
Exibimos o tipo de dados de cada coluna e o número de entradas não nulas.

print(aluguel_casa.dtypes)
aluguel_casa.info()

### 5. Alterar Tipos de Dados
Alteramos o tipo das colunas rent_amount, property_tax, fire_insurance, e total para float para garantir que sejam tratadas corretamente nas operações.

aluguel_casa = aluguel_casa.astype({"rent_amount":"float", "property_tax":"float", "fire_insurance":"float","total":"float"})

### 6. Exibir todas as colunas
Configura-se o pandas para exibir todas as colunas do DataFrame, para facilitar a inspeção dos dados.

pd.set_option('display.max_columns', None)
print(aluguel_casa.head())

### 7. Descrição Estatística
Geramos uma descrição estatística de algumas colunas selecionadas, mostrando métricas como média, desvio padrão, mínimo, máximo, etc.

print(aluguel_casa[["area", "rooms", "bathroom","parking_spaces","rent_amount","property_tax","fire_insurance","total"]].describe())

### 8. Filtros de Dados
Filtramos os dados de acordo com condições específicas:
Aluguéis com total menor ou igual a 4000.
Aluguéis que aceitam animais.
Aluguéis com 2 ou 3 quartos e aceitam animais.

aluguel_casa = aluguel_casa[(aluguel_casa["total"] <= 4000)]
aluguel_casa = aluguel_casa[(aluguel_casa["total"] <= 4000) & (aluguel_casa["animal"] == "accept")]
aluguel_casa = aluguel_casa[(aluguel_casa["total"] <= 4000) & (aluguel_casa["animal"] == "accept") & ((aluguel_casa["rooms"] == 2) | (aluguel_casa["rooms"] == 3))]


Também utilizamos query() para uma forma alternativa de filtragem:

aluguel_casa = aluguel_casa.query("total <=4000 and animal == 'accept'")
aluguel_casa = aluguel_casa.query("total <=4000 and animal == 'accept' and (rooms == 2 or rooms ==3)")

### 9. Agrupamento de Dados
Agrupamos os dados para calcular a média do valor do aluguel (total), por cidade, por quantidade de quartos e banheiros.

agg_city_price = aluguel_casa.groupby(["city"])["total"].agg("mean").reset_index()
agg_bath_room_price = aluguel_casa.groupby(["bathroom", "rooms"])["total"].agg("mean").reset_index()

### 10. Visualizações Gráficas
Utilizamos seaborn e matplotlib para criar gráficos de barras, histogramas e outros para visualizar os dados.

ax = sns.barplot(data=agg_bath_room_price, x="rooms", y="total", color="blue")
ax.set(xlabel="qtd de quartos", ylabel= "Média do preço")
plt.show()

Gráfico de barras para a média do preço por número de quartos:

ax = sns.barplot(data=agg_bath_room_price, x="rooms", y="total", color="blue")
ax.set(xlabel="qtd de quartos", ylabel= "Média do preço")
plt.show()

Gráfico de barras para a média do preço por número de banheiros:

ax = sns.barplot(data=agg_bath_room_price, x="bathroom", y="total", color="green")
ax.set(xlabel="qtd de banheiros", ylabel= "Média do preço")
plt.show()

Histograma de áreas de apartamentos menores ou iguais a 100 m²:

agg_area = aluguel_casa[["area"]]
agg_area = agg_area.query('area <=100')
sns.histplot(data=agg_area, x='area')
plt.show()

Histograma dos aluguéis por cidade:

sns.histplot(data=aluguel_casa, x="total", hue="city")
plt.show()

### 11. Filtrando Por Cidade
Filtramos dados específicos para a cidade de Porto.

aluguel_casa = aluguel_casa.query("city=='Porto'")
print(aluguel_casa.describe())

### 12. Filtrar Apartamentos por Andar
Contamos a quantidade de apartamentos disponíveis por andar e calculamos a média do preço por andar.

andar = aluguel_casa.groupby("floor").index.nunique().sort_values(ascending=False)
agg_floor_price = aluguel_casa.groupby("floor")["total"].agg("mean").reset_index()

sns.barplot(agg_floor_price, x="floor", y="total", color="blue")
plt.show()

### 13. Filtrando Apartamentos no Andar 16
Filtramos apartamentos localizados no andar 16.

sixteen_floor = aluguel_casa.query("floor == 16")
print(sixteen_floor)

### Conclusão
Este projeto utiliza pandas para limpar e manipular dados de aluguel de apartamentos, utilizando filtros e agrupamentos para obter insights sobre os preços de aluguel. Além disso, emprega visualizações para melhor compreensão dos dados, como gráficos de barras, histogramas e agrupamentos.