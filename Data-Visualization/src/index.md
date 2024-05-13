# Questão 1 - Existe alguma característica que faz uma música ter mais chance de se tornar popular?

## DataSet Completo

```js
const arquivo = await FileAttachment("./data/spotify-2023.csv").csv({typed: true});

arquivo.sort((a,b) => {
   
      return b.streams - a.streams
    
  })
view(Inputs.table(arquivo));
```



# Top 100 músicas mais ouvidas
Estou fazendo isso para isolar o DataSet e tentar encontrar alguma característica em comum nesse conjunto, que possa fazer com que uma música se torne popular.
Esse será o dataset que será utilizado para responder à questão 1


```js
const divWidth = Generators.width(document.querySelector("#ex01"));

```


```js
const Top100 = arquivo.slice(0, 100);

view(Inputs.table(Top100));


import * as vega from "npm:vega";
import * as vegaLite from "npm:vega-lite";
import * as vegaLiteApi from "npm:vega-lite-api";

const vl = vegaLiteApi.register(vega, vegaLite);

function ex01(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: Top100
            },
            "mark": {
                "type": "bar"
            },
            "encoding": {
                "x": {
                    "field": "key",
                    "type": "nominal"
                },
                "y": {
                    "type": "quantitative",
                    "aggregate": "count",
                    "title": "Number of Musics"
                    
                }
            }
        }
    };
}

function ex02(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: arquivo
            },
            "mark": {
                "type": "bar"
            },
            "encoding": {
                "y": {
                    "field": "bpm",
                    "type": "quantitative",
                    "bin": {"maxbins": 10}
                },
                "x": {
                    "type": "quantitative",
                    "aggregate": "count",
                    "title": "Number of Musics"
                    
                }
            }
        }
    };
}

function ex03(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: Top100
            }, 
            mark: {
                type: "point"
            },
            encoding: {
                y   : {
                    type: "quantitative",
                    field: "energy_%",
                    title: "energy_%"
                },
                x: {
                    type: "quantitative", // You can use "nominal" if y-axis should represent discrete values
                    field: "danceability_%", // Assuming there's a field with the music title
                    title: "Music danceability"
                },
                size: {
                    field: "valence_%", 
                    type: "quantitative"
                }
                }
            }
    };
}

function ex04(divWidth) {
    return {
        spec: {
        "data": {
            values: Top100}, // Use your Top100 dataset
        "transform": [{
            "filter": {"and": [
                {"field": "valence_%", "valid": true}, // Adjust field names
                {"field": "danceability_%", "valid": true} // Adjust field names
            ]}
        }],
        "mark": "rect",
        "width": 300,
        "height": 200,
        "encoding": {
            "x": {
                "bin": {"maxbins": 10},
                "field": "valence_%", // Adjust field name
                "type": "quantitative"
            },
            "y": {
                "bin": {"maxbins": 10},
                "field": "danceability_%", // Adjust field name
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
# Notas musicais(key):
Procurei visualizar melhor a frequencia com que cada valor da tabela "key" aparecia, já que os valores dessa coluna representam a principal nota musical de cada música.
Queria saber, entre as músicas presentes no Top 100, qual nota musical mais aparecia, ou seja, qual é o valor mais frequente da coluna "key" e se o resultado iria trazer alguma discrepância. Não houve uma distância tão grande, mas foi possível notar que o valor "C#" aparece com uma certa frequencia acima dos outros valores do gráfico.
Para facilitar essa visualização de algum padrão foram criados gráficos de barra

<div id="ex01" class="card">
        <h1>Notas musicais mais frequentes</h1>
        <div style="width: 100%; margin-top: 15px;">
            ${ vl.render(ex01(divWidth - 45)) }
        </div>
</div>

## Análise:
Através do gráfico criado acima, podemos perceber que o valor "C#" é o mais frequente com alguma folga, cerca de 50% mais frequente que a segunda e a terceira nota musical que mais aparece nas 100 músicas mais populares do dataset. Isso indica que, se uma música tiver o Dó sustenido(C#), possui maior probabiblidade de se tornar popular


# Visualização da presença de elementos ao vivo

<div id="ex02" class="card">
        <h1>Quantidade de músicas x % de elementos ao vivo</h1>
        <div style="width: 100%; margin-top: 15px;">
            ${ vl.render(ex02(divWidth - 45)) }
        </div>
</div>

## Análise:
O gráfico acima deixa bem evidente que quanto menor o índice de elementos ao vivo, maior a chance da musica estar no Top100. Temos 32 músicas com índice menor que 10% e 39 músicas coom índice menor que 20%. Portanto, temos que 70% das musicas mais populares melhor dizendo, para uma música ter maior probabilidade de ser popular, o ideal é que a quantidade de elementos ao vivo presente na mesma seja um indice menor que 20% de elementos ao vivo.

<div id="ex03" class="card">
        <h1>Exemplo 03</h1>
        <div style="width: 100%; margin-top: 15px;">
            ${ vl.render(ex03(divWidth - 115)) }
        </div>
</div>

<div id="ex04" class="card">
        <h1>Exemplo 04</h1>
        <div style="width: 100%; margin-top: 15px;">
            ${ vl.render(ex04(divWidth - 115)) }
        </div>
</div>
