<h1>Análise de solo com Python</h1>
<h2>Conjunto de dados de recomendação de colheita</h2>
<h3>Contexto geral</h3>
A agricultura de precisão está em alta atualmente. Ajuda os agricultores a tomar decisões informadas sobre a estratégia de cultivo. Aqui, apresento a vocês um conjunto de dados que permitiria aos usuários construir um modelo preditivo para recomendar as safras mais adequadas para crescer em uma determinada fazenda com base em vários parâmetros.

<h3>Contexto do conjunto de dados</h3>
Este conjunto de dados foi construído aumentando os conjuntos de dados de chuva, clima e fertilizantes disponíveis para a Índia.

Campos de dados
N - razão do conteúdo de nitrogênio no solo
P - razão do conteúdo de fósforo no solo
K - razão do teor de potássio no solo
temperatura - temperatura em graus Celsius
umidade - umidade relativa em%
valor ph - ph do solo
precipitação - precipitação em mm




```python
# Importanto as bibliotecas
import pandas as pd

# carregando o conjunto de dados
arquivo = pd.read_csv('soil_data.csv')
arquivo
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>N</th>
      <th>P</th>
      <th>K</th>
      <th>temperature</th>
      <th>humidity</th>
      <th>ph</th>
      <th>rainfall</th>
      <th>label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>90</td>
      <td>42</td>
      <td>43</td>
      <td>20.879744</td>
      <td>82.002744</td>
      <td>6.502985</td>
      <td>202.935536</td>
      <td>rice</td>
    </tr>
    <tr>
      <th>1</th>
      <td>85</td>
      <td>58</td>
      <td>41</td>
      <td>21.770462</td>
      <td>80.319644</td>
      <td>7.038096</td>
      <td>226.655537</td>
      <td>rice</td>
    </tr>
    <tr>
      <th>2</th>
      <td>60</td>
      <td>55</td>
      <td>44</td>
      <td>23.004459</td>
      <td>82.320763</td>
      <td>7.840207</td>
      <td>263.964248</td>
      <td>rice</td>
    </tr>
    <tr>
      <th>3</th>
      <td>74</td>
      <td>35</td>
      <td>40</td>
      <td>26.491096</td>
      <td>80.158363</td>
      <td>6.980401</td>
      <td>242.864034</td>
      <td>rice</td>
    </tr>
    <tr>
      <th>4</th>
      <td>78</td>
      <td>42</td>
      <td>42</td>
      <td>20.130175</td>
      <td>81.604873</td>
      <td>7.628473</td>
      <td>262.717340</td>
      <td>rice</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2195</th>
      <td>107</td>
      <td>34</td>
      <td>32</td>
      <td>26.774637</td>
      <td>66.413269</td>
      <td>6.780064</td>
      <td>177.774507</td>
      <td>coffee</td>
    </tr>
    <tr>
      <th>2196</th>
      <td>99</td>
      <td>15</td>
      <td>27</td>
      <td>27.417112</td>
      <td>56.636362</td>
      <td>6.086922</td>
      <td>127.924610</td>
      <td>coffee</td>
    </tr>
    <tr>
      <th>2197</th>
      <td>118</td>
      <td>33</td>
      <td>30</td>
      <td>24.131797</td>
      <td>67.225123</td>
      <td>6.362608</td>
      <td>173.322839</td>
      <td>coffee</td>
    </tr>
    <tr>
      <th>2198</th>
      <td>117</td>
      <td>32</td>
      <td>34</td>
      <td>26.272418</td>
      <td>52.127394</td>
      <td>6.758793</td>
      <td>127.175293</td>
      <td>coffee</td>
    </tr>
    <tr>
      <th>2199</th>
      <td>104</td>
      <td>18</td>
      <td>30</td>
      <td>23.603016</td>
      <td>60.396475</td>
      <td>6.779833</td>
      <td>140.937041</td>
      <td>coffee</td>
    </tr>
  </tbody>
</table>
<p>2200 rows × 8 columns</p>
</div>




```python
# selecionando os labels existentes na coluna
colunaLabel = arquivo['label'].unique()

# identificando cada label
for rotulo in enumerate(colunaLabel):
    print(rotulo[0],rotulo[1])
```

    0 rice
    1 maize
    2 chickpea
    3 kidneybeans
    4 pigeonpeas
    5 mothbeans
    6 mungbean
    7 blackgram
    8 lentil
    9 pomegranate
    10 banana
    11 mango
    12 grapes
    13 watermelon
    14 muskmelon
    15 apple
    16 orange
    17 papaya
    18 coconut
    19 cotton
    20 jute
    21 coffee
    


```python
# enumerando e substituindo o nome do label por um número para ser tratado em python
for label in enumerate(colunaLabel):
    arquivo['label'] = arquivo['label'].replace(label[1],label[0])
    
arquivo
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>N</th>
      <th>P</th>
      <th>K</th>
      <th>temperature</th>
      <th>humidity</th>
      <th>ph</th>
      <th>rainfall</th>
      <th>label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>90</td>
      <td>42</td>
      <td>43</td>
      <td>20.879744</td>
      <td>82.002744</td>
      <td>6.502985</td>
      <td>202.935536</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>85</td>
      <td>58</td>
      <td>41</td>
      <td>21.770462</td>
      <td>80.319644</td>
      <td>7.038096</td>
      <td>226.655537</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>60</td>
      <td>55</td>
      <td>44</td>
      <td>23.004459</td>
      <td>82.320763</td>
      <td>7.840207</td>
      <td>263.964248</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>74</td>
      <td>35</td>
      <td>40</td>
      <td>26.491096</td>
      <td>80.158363</td>
      <td>6.980401</td>
      <td>242.864034</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>78</td>
      <td>42</td>
      <td>42</td>
      <td>20.130175</td>
      <td>81.604873</td>
      <td>7.628473</td>
      <td>262.717340</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2195</th>
      <td>107</td>
      <td>34</td>
      <td>32</td>
      <td>26.774637</td>
      <td>66.413269</td>
      <td>6.780064</td>
      <td>177.774507</td>
      <td>21</td>
    </tr>
    <tr>
      <th>2196</th>
      <td>99</td>
      <td>15</td>
      <td>27</td>
      <td>27.417112</td>
      <td>56.636362</td>
      <td>6.086922</td>
      <td>127.924610</td>
      <td>21</td>
    </tr>
    <tr>
      <th>2197</th>
      <td>118</td>
      <td>33</td>
      <td>30</td>
      <td>24.131797</td>
      <td>67.225123</td>
      <td>6.362608</td>
      <td>173.322839</td>
      <td>21</td>
    </tr>
    <tr>
      <th>2198</th>
      <td>117</td>
      <td>32</td>
      <td>34</td>
      <td>26.272418</td>
      <td>52.127394</td>
      <td>6.758793</td>
      <td>127.175293</td>
      <td>21</td>
    </tr>
    <tr>
      <th>2199</th>
      <td>104</td>
      <td>18</td>
      <td>30</td>
      <td>23.603016</td>
      <td>60.396475</td>
      <td>6.779833</td>
      <td>140.937041</td>
      <td>21</td>
    </tr>
  </tbody>
</table>
<p>2200 rows × 8 columns</p>
</div>




```python
# separando as variáveis entre preditora e variáveis alvo
y = arquivo['label']
x = arquivo.drop('label', axis=1) ## axis = 0 -> linha || ou axis = 1 -> coluna

```


```python
from sklearn.model_selection import train_test_split

# criando os conjuntos de dados de treino e teste, pois o modelo vai receber alguns dados para treinar  (aprender) 
# e depois a gente vai testar pra ver se está fazendo certo

# passando os dados para a função train_test_split e especificando a porcentagem 30% para teste e 70% (o restante) para treino
x_treino, x_teste, y_treino, y_teste = train_test_split(x, y, test_size = 0.3)
```


```python
y_teste.shape
```




    (660,)




```python
# importando algoritmo que cria árvores de decisão
from sklearn.ensemble import ExtraTreesClassifier

# criação do modelo
modelo = ExtraTreesClassifier(n_estimators=100)
modelo.fit(x_treino, y_treino)
```




    ExtraTreesClassifier()




```python
# imprimindo resultados

resultado = modelo.score(x_teste, y_teste)
print("Acurácia:", resultado)
```

    Acurácia: 0.9939393939393939
    


```python
# selecionando algumas amostras aleatoriamente
y_teste[200:210]
```




    1747    17
    327      3
    1738    17
    2014    20
    2167    21
    1785    17
    377      3
    1751    17
    1765    17
    51       0
    Name: label, dtype: int64




```python
x_teste[200:210]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>N</th>
      <th>P</th>
      <th>K</th>
      <th>temperature</th>
      <th>humidity</th>
      <th>ph</th>
      <th>rainfall</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1747</th>
      <td>34</td>
      <td>65</td>
      <td>48</td>
      <td>41.419684</td>
      <td>90.038631</td>
      <td>6.665025</td>
      <td>199.309643</td>
    </tr>
    <tr>
      <th>327</th>
      <td>24</td>
      <td>80</td>
      <td>22</td>
      <td>16.711706</td>
      <td>19.176514</td>
      <td>5.635994</td>
      <td>96.772858</td>
    </tr>
    <tr>
      <th>1738</th>
      <td>44</td>
      <td>57</td>
      <td>53</td>
      <td>42.304958</td>
      <td>90.514318</td>
      <td>6.931721</td>
      <td>74.876786</td>
    </tr>
    <tr>
      <th>2014</th>
      <td>67</td>
      <td>43</td>
      <td>38</td>
      <td>25.216227</td>
      <td>70.882596</td>
      <td>7.299305</td>
      <td>195.864555</td>
    </tr>
    <tr>
      <th>2167</th>
      <td>89</td>
      <td>28</td>
      <td>33</td>
      <td>26.444141</td>
      <td>53.838762</td>
      <td>6.993236</td>
      <td>175.372331</td>
    </tr>
    <tr>
      <th>1785</th>
      <td>48</td>
      <td>57</td>
      <td>54</td>
      <td>29.023280</td>
      <td>90.203968</td>
      <td>6.617703</td>
      <td>126.806987</td>
    </tr>
    <tr>
      <th>377</th>
      <td>31</td>
      <td>75</td>
      <td>18</td>
      <td>15.467893</td>
      <td>21.437807</td>
      <td>5.824208</td>
      <td>88.887961</td>
    </tr>
    <tr>
      <th>1751</th>
      <td>33</td>
      <td>47</td>
      <td>46</td>
      <td>29.203009</td>
      <td>93.968340</td>
      <td>6.839444</td>
      <td>209.408331</td>
    </tr>
    <tr>
      <th>1765</th>
      <td>40</td>
      <td>64</td>
      <td>47</td>
      <td>32.500375</td>
      <td>93.478888</td>
      <td>6.893509</td>
      <td>71.737595</td>
    </tr>
    <tr>
      <th>51</th>
      <td>76</td>
      <td>60</td>
      <td>39</td>
      <td>20.045414</td>
      <td>80.347756</td>
      <td>6.766240</td>
      <td>208.581016</td>
    </tr>
  </tbody>
</table>
</div>




```python
previsoes = modelo.predict(x_teste[200:210])
```


```python
previsoes
```




    array([17,  3, 17, 20, 21, 17,  3, 17, 17,  0], dtype=int64)




```python

```

<h1>Conclusões</h1>

O algoritmo conseguiu prever com exatidão, baseados nas informações fornecidas nas outras colunas, que as 10 amostras no vetor array([17,  3, 17, 20, 21, 17,  3, 17, 17,  0]) correspondem com a tabela original 
