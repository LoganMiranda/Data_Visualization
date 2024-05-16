# Discuta as diferenças entre as plataformas (Spotify, Deezer, Apple Music e Shazam)

<hr>
<br>

```js
const arquivo = await FileAttachment("./data/spotify-2023.csv").csv({typed: true});

arquivo.sort((a,b) => {
   
      return b.streams - a.streams
    
  })

const divWidth = Generators.width(document.querySelector("#ex01"));

import * as vega from "npm:vega";
import * as vegaLite from "npm:vega-lite";
import * as vegaLiteApi from "npm:vega-lite-api";

const vl = vegaLiteApi.register(vega, vegaLite);

const Top2ArtistasMaisFrequentesTop10 = []


const Quantidade_Playlists_Por_Plataforma = {
    spotify: 0,
    deezer: 0,
    apple: 0
};

arquivo.forEach(musica => {
    if (Number.isInteger(musica["in_spotify_playlists"])) {
        Quantidade_Playlists_Por_Plataforma.spotify += musica["in_spotify_playlists"];
    }

    if (Number.isInteger(musica["in_deezer_playlists"])) {
        Quantidade_Playlists_Por_Plataforma.deezer += musica["in_deezer_playlists"];
    }

    if (Number.isInteger(musica["in_apple_playlists"])) {
        Quantidade_Playlists_Por_Plataforma.apple += musica["in_apple_playlists"];
    }
});

const Notas_Musicais_Not_Null = Top2ArtistasMaisFrequentesTop10.filter(row => row.key !== null);



function ex01(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: [
                  {Plataformas: "Spotify", valor:  Quantidade_Playlists_Por_Plataforma.spotify},
                  {Plataformas: "Deezer", valor: Quantidade_Playlists_Por_Plataforma.deezer},
                  {Plataformas: "Apple", valor: Quantidade_Playlists_Por_Plataforma.apple}
                ]
            },
            "mark": {
                "type": "arc",
                "tooltip": true
            },
            "encoding": {
                "theta": {
                  "field": "valor", 
                  "type": "quantitative",
                  "stack":"normalize"
                },
                "color": {
              "field": "Plataformas", 
              "type": "nominal"
              }
            }
        }
    };
}
```

# Musicas nas playlists de cada plataforma
O objetivo era visualizar se alguma plataforma possuia uma quantidade muito grande de músicas em sua Playlists, em comparação com as outras. 

<div id="ex01" class="card">
        <h1>Quantidade de músicas nas Playlist de cada Plataforma</h1>
        <div style="width: 100%; margin-top: 15px;">
            ${ vl.render(ex01(divWidth -115)) }
        </div>
</div>

<br>

# Análise:
Fica explícito que o Spotify é a plataforma que mais possui músicas populares em playlists próprias. Isso pode ser um indicativo que o Spotify tenta introduzir novas músicas aos usuários por meio de playlists, que possuem várias outras novas e diferentes músicas, mas que estão relacionadas à que o usúario gostou, tentando assim apresentar novas músicas com base em uma música mais famosa no momento.


```js

const  Top100 = arquivo.slice(0,100)


const Quantidade_Charts_Por_Plataforma = {
    spotify: 0,
    deezer: 0,
    apple: 0,
    shazam: 0,
};

Top100.forEach(musica => {
    if (Number.isInteger(musica["in_spotify_charts"])) {
        Quantidade_Charts_Por_Plataforma.spotify += musica["in_spotify_charts"];
    }

    if (Number.isInteger(musica["in_deezer_charts"])) {
        Quantidade_Charts_Por_Plataforma.deezer += musica["in_deezer_charts"];
    }

    if (Number.isInteger(musica["in_apple_charts"])) {
        Quantidade_Charts_Por_Plataforma.apple += musica["in_apple_charts"];
    }
    if (Number.isInteger(musica["in_shazam_charts"])) {
        Quantidade_Charts_Por_Plataforma.shazam += musica["in_shazam_charts"];
    }    
});


console.log(Quantidade_Charts_Por_Plataforma)

view(Inputs.table(Top100));
function ex04(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: [
                    { Plataformas: "Spotify", valor: Quantidade_Charts_Por_Plataforma.spotify },
                    { Plataformas: "Deezer", valor: Quantidade_Charts_Por_Plataforma.deezer },
                    { Plataformas: "Apple", valor: Quantidade_Charts_Por_Plataforma.apple },
                    {Plataformas: "Shazam", valor:Quantidade_Charts_Por_Plataforma.shazam},
                ]
            },
            mark: {
                type: "bar"
            },
            encoding: {
                x: {
                    field: "Plataformas", // Usar os nomes das plataformas como valores para o eixo x
                    type: "nominal",
                    title: "Plataformas"
                },
                y: {
                    field: "valor",
                    type: "quantitative",
                    title: "Quantidade de charts"
                }
            }
        }
    };
}


```
<br>
<hr>

# Quantidade de vezes que as músicas do Top100 apareceram no topo (charts) em cada plataforma


<div id="ex04" class="card">
        <h1>Plataformas x Quantidade de charts</h1>
        <div style="width: 100%; margin-top: 15px;">
            ${ vl.render(ex04(divWidth - 175)) }
        </div>
</div>

# Análise
Pode-se inferir que uma musica famosa aparecera muitas vezes no charts da Apple, e para as outras plataformas isso não se mantem. Alem disso, ao contrario da analise anterior, em que spotify é disparado a plataforma que mais possui musicas nas suas playlists, a Apple é, de longe, a que mais possui músicas nos charts. Portanto, a apple busca atrair usúarios, colocando enfase em músicas populares, colocando-as nos charts mais vezes, para que os usuários passem a utilizar mais a plataforma.


