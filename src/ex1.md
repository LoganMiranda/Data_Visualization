# Questão 1 - Existe alguma característica que faz uma música ter mais chance de se tornar popular?

<br>
<hr>

## DataSet Completo


```js
const arquivo = await FileAttachment("./data/spotify-2023.csv").csv({typed: true});

arquivo.sort((a,b) => {
   
      return b.streams - a.streams
    
  })
view(Inputs.table(arquivo));

const divWidth = Generators.width(document.querySelector("#ex01"));

import * as vega from "npm:vega";
import * as vegaLite from "npm:vega-lite";
import * as vegaLiteApi from "npm:vega-lite-api";

const vl = vegaLiteApi.register(vega, vegaLite);

const Not_Null_Key = arquivo.filter(row => row.key !== null);

function ex01(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: Not_Null_Key
            },
            "mark": {
                "type": "bar"
            },
            "encoding": {
                "x": {
                    "field": "key",
                    "type": "nominal",
                    "title": "notas musicais"
                },
                "y": {
                    "type": "quantitative",
                    "aggregate": "count",
                    "title": "Quantidade de músicas"
                    
                }
            }
        }
    };
}
```
<hr>

# Notas musicais (key):
Procuramos visualizar melhor a frequencia com que cada valor da tabela "key" aparecia, já que os valores dessa coluna representam a principal nota musical de cada música.
Queríamos saber, entre as músicas presentes na base de dados, qual nota musical mais aparecia, ou seja, qual é o valor mais frequente da coluna "key" e se o resultado iria trazer alguma discrepância. 
Para facilitar essa visualização de algum padrão foram criados gráficos de barra

<div id="ex01" class="card">
        <h1>Notas musicais mais frequentes</h1>
        <div style="width: 100%; margin-top: 15px;">
            ${ vl.render(ex01(divWidth - 45)) }
        </div>
</div>

<br>
<hr>

# Análise:
Através do gráfico criado acima, podemos perceber que não houve uma distância tão grande, mas foi possível notar que o valor "C#" aparece com uma certa frequencia acima dos outros valores do gráfico, seguida por "G" e "G#". O valor "C#" é cerca de 50% mais frequente que a segunda e a terceira nota musical que mais aparecem no dataset. Isso indica que, se uma música tiver o Dó sustenido(C#), possui maior probabiblidade de se tornar popular

<br>

# Justificativa de visualização
Foi utilizado gráfico de barras, para facilitar a comparação dos dados, já que os dados são de duas categorias diferentes. Os dados do eixo X, "notas musicais", são do tipo catégorico, enquanto que os do eixo Y são do tipo quantitativo. O marcador visual é a barra e o canal visual é a cor aplicada na barra que, nesse caso, é o azul.

```js

function ex02(divWidth) {
    return {
        spec: {
        "data": {
            values: arquivo  },
        "transform": [{
            "filter": {"and": [
                {"field": "danceability_%", "valid": true}, 
                {"field": "energy_%", "valid": true} 
            ]}
        }],
        "mark": "rect",
        "width": 300,
        "height": 200,
        "encoding": {
            "y": {
                "bin": {"maxbins": 10},
                "field": "danceability_%", 
                "title": "dançabilidade",
                "type": "quantitative"
            },
            "x": {
                "bin": {"maxbins": 10},
                "field": "energy_%", 
                "title": "energia%",
                "type": "quantitative"
            },
            "color": {
                "aggregate": "count",
                "type": "quantitative"
            }
        },
        "config": {
            "view": {
                "stroke": "transparent"
            }
        }
    }
  };
}

```
<br>
<hr>

# Dançabilidade e Energia
Procuramos encontrar uma correlação de dois elementos musicais que, quando aliados em uma determinada faixa, possam concentrar uma maior quantidade de músicas. Dessa maneira, se uma música possuir a combinação desses dois elementos musicais, teria mais chance de se tornar popular.

<div id="ex02" class="card">
        <h1>Danceability x Energy</h1>
        <div style="width: 100%; margin-top: 15px;">
            ${ vl.render(ex02(divWidth -15)) }
        </div>
</div>

# Análise
Pode-se perceber que há uma concentração maior do número de músicas presentes na base de dados, quando a música possui uma faixa de danceability entre 50 e 90%, há uma condensação ainda maior na específica faixa de 70 a 80%. Aliada a uma taxa de energy entre 50 e 90%. Portanto, para uma música ter mais chance de se tornar popular, seria interessante possuir um ritmo dançante e com uma boa energia também, conferindo, provavelmente, um aspecto mais animado a música

<br>

# Justificativa de visualização
Optamos por utilizar um histograma heatmap, para visualizarmos melhor a distribuição dos elementos musicais "Danceability" e "Energy" do DataSet. Portanto, estavamos buscando identificar a faixa de "danceability" e "energy" mais frequentes. O marcador visual utilizado nesse gráfico são as células, que possui como canal visual diferentes cores, em que quanto mais escura, ou seja, quanto mais forte é a cor, há maior frequência desses valores no conjunto de dados.

```js

function ex04(divWidth) {
    
    return {
        spec: {
            width: divWidth,
            data: {
                values: arquivo
            },
            "mark": {
                "type": "point"
            },
            "encoding": {
                "x": {
                    "field": "liveness_%",
                    "type": "quantitative",
                    "title": "elementos ao vivo%"

                },
                "y": {
                    "field": "speechiness_%",
                    "type": "quantitative",
                    "title": "palavras diferentes%"

                }   
            }
        }
    };
}
```
<br>
<hr>

# Palavras diferentes e elementos ao vivo

<div id="ex04" class="card">
        <h1>Speechiness x Liveness</h1>
        <div style="width: 100%; margin-top: 15px;">
            ${ vl.render(ex04(divWidth - 175)) }
        </div>
</div>

# Análise
Pode-se perceber que há uma extrema concentração na parte baixa e à esquerda do gráfico mostrado. O eixo y apresenta valores na parte baixa, pois a esmagadora maioria das músicas possui poucas palavras diferentes ditas, há uma condensação onde as músicas possuem menos de 20 palavras diferentes utilizadas. Aliado a isso, temos que os elementos que conferem a sensação "ao vivo", não devem ser maiores que 40%, pois existem pouquissímos valores à metade direita do eixo X. Logo, uma música com poucas palavras diferentes na sua composição, ou seja, que repete mais palavras, provavelmente se torna mais fácil de ser memorizada e cantada. Além disso, o público prefere ouvir músicas que remetem menos a sensação "ao vivo".

<br>

# Justificativa de visualização
Optamos por utilizar um Scatterplot para tentarmos encontrar alguma correlação entre os elementos "Speechiness" e "Liveness" presentes em cada música. O marcador visual são os pontos no gráfico, compostos pelos dois atributos citados acima. Por outro lado, o canal visual utilizado foi a cor azul.
