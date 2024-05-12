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
     <div id="ex04" class="card">
        <h1>Exemplo 04</h1>
        <div style="width: 100%; margin-top: 15px;">
            ${ vl.render(ex04(divWidth - 115)) }
        </div>
    </div>
</div>



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
                values: Top100
            }, 
            mark: {
                type: "circle"
            },
            encoding: {
                y   : {
                    type: "quantitative",
                    field: "bpm",
                    title: "Music BPM"
                },
                x: {
                    type: "quantitative", // You can use "nominal" if y-axis should represent discrete values
                    field: "streams", // Assuming there's a field with the music title
                    title: "Music Streams"
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
                type: "circle"
            },
            encoding: {
                y   : {
                    type: "nominal",
                    field: "mode",
                },
                x: {
                    type: "quantitative", // You can use "nominal" if y-axis should represent discrete values
                    field: "streams", // Assuming there's a field with the music title
                    title: "Music Streams"
                }
            }
        }
    };
}

function ex04(divWidth) {
    return {
        spec: {
        "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
        "data": {
            values: Top100}, // Use your Top100 dataset
        "transform": [{
            "filter": {"and": [
                {"field": "bpm", "valid": true}, // Adjust field names
                {"field": "energy_%", "valid": true} // Adjust field names
            ]}
        }],
        "mark": "rect",
        "width": 300,
        "height": 200,
        "encoding": {
            "x": {
                "bin": {"maxbins": 60},
                "field": "bpm", // Adjust field name
                "type": "quantitative"
            },
            "y": {
                "bin": {"maxbins": 40},
                "field": "energy_%", // Adjust field name
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