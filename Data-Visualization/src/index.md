# DataSet Completo

```js
const arquivo = await FileAttachment("./data/spotify-2023.csv").csv({typed: true});

arquivo.sort((a,b) => {
   
      return b.streams - a.streams
    
  })
view(Inputs.table(arquivo));
```



# Top 50 músicas mais ouvidas
Estou fazendo isso para isolar o DataSet e tentar encontrar alguma característica em comum nesse conjunto, que possa fazer com que uma música se torne popular.


<div class="grid grid-cols-2">
    <div id="ex01" class="card">
        <h1>Exemplo 01</h1>
        <div style="width: 100%; margin-top: 15px;">
            ${ vl.render(ex01(divWidth - 45)) }
        </div>
    </div>
    <div id="ex02" class="card">
        <h1>Exemplo 02</h1>
        <div style="width: 100%; margin-top: 15px;">
            ${ vl.render(ex02(divWidth - 45)) }
        </div>
    </div>
    <div id="ex03" class="card">
        <h1>Exemplo 03</h1>
        <div style="width: 100%; margin-top: 15px;">
            ${ vl.render(ex03(divWidth - 115)) }
        </div>
    </div>
</div>



```js
const divWidth = Generators.width(document.querySelector("#ex01"));

```


```js
const Top50 = arquivo.slice(0, 100);

view(Inputs.table(Top50));


import * as vega from "npm:vega";
import * as vegaLite from "npm:vega-lite";
import * as vegaLiteApi from "npm:vega-lite-api";

const vl = vegaLiteApi.register(vega, vegaLite);

function ex01(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: Top50
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
                values: Top50
            },
            "mark": {
                "type": "bar"
            },
            "encoding": {
                "x": {
                    "field": "bpm",
                    "type": "nominal",
                    "bin": {
                      "maxbins": 10
                    }
                },
                "y": {
                    "type": "quantitative",
                    "aggregate": "count"
                    
                }
            }
        }
    };
}
```