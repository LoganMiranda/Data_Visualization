# Questão 2 -  O conjunto das top 10 músicas e dos top 10 artistas varia muito se considerarmos apenas musicas lançadas no mesmo ano?


```js
const arquivo = await FileAttachment("./data/spotify-2023.csv").csv({typed: true});

import * as vega from "npm:vega";
import * as vegaLite from "npm:vega-lite";
import * as vegaLiteApi from "npm:vega-lite-api";

const divWidth = Generators.width(document.querySelector("#top10ArtistasEntreOsTop10"));

const vl = vegaLiteApi.register(vega, vegaLite);


```




Para responder a essa pergunta, primeiramente precisamos de uma visualizacão do top 10 músicas e 10 artistas entre todos os anos:

## Top 10 músicas entre todos os anos

```js
  arquivo.sort((a,b) => {
   
      return b.streams - a.streams
    
  })

  const dezMusicasMaisPopulares = arquivo.slice(0,10)


  view(Inputs.table(dezMusicasMaisPopulares))

```

Para visualizarmos as 10 músicas mas populares entre todos os anos, ordenamos todas a músicas pelo número de reproduções, do maior para o menor, e selecionamos as 10 primeiras ocorrências.

## Top 10 artistas entre todos os anos

```js

  function getStreamsPorArtista(listaMusicas){
    const streamsPorArtista = listaMusicas.reduce((acc, linha_csv) => { 
    const nomes_artistas = linha_csv["artist(s)_name"].split(",")
    nomes_artistas.forEach((nome_artista) => {
      nome_artista = nome_artista.trim()
      if (acc[nome_artista] === undefined){
        acc[nome_artista] = linha_csv.streams
      }else{
        acc[nome_artista] += linha_csv.streams
      }
    })
    return acc
    }, {})
    return streamsPorArtista
  }

  const streamsPorArtista = getStreamsPorArtista(arquivo)


  function getDezArtistasMaisPopulares(streamsPorArtista){
    const dezArtistasMaisPopulares = Object.entries(streamsPorArtista).sort((a,b) => {
    return  b[1] - a[1]
  }).slice(0, 10).map((a) => {
    return {
      "nome": a[0],
      "streams": a[1]
    }
  })

  return dezArtistasMaisPopulares
  }

  const dezArtistasMaisPopulares = getDezArtistasMaisPopulares(streamsPorArtista)

  view(Inputs.table(dezArtistasMaisPopulares))
```
Para visualizar os top 10 artistas de todos os tempos, primeiro foram agregadas as streams das músicas que cada artista fazia parte. Após agregado, os artistas foram ordenados pelo número de streams totais da história e os 10 primeiros foram selecionados.

```js
  const musicasEmCadaAno = arquivo.reduce((acc, linha_csv) => {
    const ano_musica = linha_csv.released_year
    if (acc[ano_musica] === undefined){
      acc[ano_musica] = [linha_csv]
    }else {
      acc[ano_musica].push(linha_csv)
      acc[ano_musica].sort((a,b) => {
        return b.streams - a.streams
      })
    }
    return acc
  }, {})
  


  const topDezMusicasEmCadaAno = Object.entries(musicasEmCadaAno).reduce((acc, anoEMusicas) => {
    const ano = anoEMusicas[0]
    if(acc[ano] === undefined){
      acc[ano] = []
    }
    const musicas = anoEMusicas[1].sort((a,b) => {
      b.streams - a.streams
    }).slice(0,10)

    acc[ano].push(...musicas)

    return acc
  }, {})



  const topDezArtistasEmCadaAno = Object.entries(musicasEmCadaAno).reduce((acc, anoEMusicas) => {
    const ano = anoEMusicas[0]
    const musicas = anoEMusicas[1]
    const streamsPorArtistaNoAno = getStreamsPorArtista(musicas)
    const dezArtistasMaisPopularesNoAno = getDezArtistasMaisPopulares(streamsPorArtistaNoAno)
    acc[ano] = dezArtistasMaisPopularesNoAno

    return acc
  }, {})

  // Musicas em cada ano e separadas por mes, ordenadas da mais popular para a menos popular
  const musicasEmCadaAnoEMes = Object.entries(musicasEmCadaAno).reduce((acc, anoEMusicas) => {
    const ano = anoEMusicas[0]
    const musicas = anoEMusicas[1]

    acc[ano] = musicas.reduce((acc, musica) => {
      const mes_lancamento = musica.released_month
      if(acc[mes_lancamento] === undefined){
        acc[mes_lancamento] = [musica]
      }else{
        acc[mes_lancamento].push(musica)
        acc[mes_lancamento].sort((a,b) => {
          return b.streams - a.streams
        })
      }
      return acc
    }, {})
    
    return acc
  }, {})

  

  console.log({
    "🚀 ~ dezMusicasMaisPopulares:": dezMusicasMaisPopulares,
    "🚀 ~ streamsPorArtista": streamsPorArtista,
    "🚀 ~ dezArtistasMaisPopulares:": dezArtistasMaisPopulares,
    "🚀 ~ musicasEmCadaAno:": musicasEmCadaAno,
    "🚀 ~ topDezMusicasEmCadaAno:": topDezMusicasEmCadaAno,
    "🚀 ~ topDezArtistasEmCadaAno:": topDezArtistasEmCadaAno,
    "🚀 ~  musicasEmCadaAnoEMes:": musicasEmCadaAnoEMes
  })


```

## É frequente um artista aparecer muitas vezes no top 10 em diferentes anos?
Procuramos descobrir se um artista aparece muitas vezes no top 10 ao longo dos anos ou se é algo isolado. Para isso, primeiro agregamos os top 10 artistas em cada um dos anos e, com isso, fomos capazes de observar que somente a partir de 2011 temos pelo menos 10 artistas distintos para compor o top 10. Após isso, contamos quantas vezes cada artista esteve presente no top 10 de 2011 para frente.

```js

  const topDezArtistasQueMaisAparecemNoTopDezDeCadaAnoAPartirDeDoisMilEOnze = Object.entries(Object.entries(topDezArtistasEmCadaAno).filter((anoEMusicas) =>  anoEMusicas[0]>=2011).reduce((acc, anoEMusicas)=>{
    
    const musicas = anoEMusicas[1]

    musicas.forEach((musica) => {
    const nomeArtista = musica.nome

      if (acc[nomeArtista] === undefined){
        acc[nomeArtista] = 1
      }else{
        acc[nomeArtista] += 1
      }
    })
    return acc
  }, {})).reduce((acc, nomeEQuantidade)=>{
    acc.push(
      {
        "nome": nomeEQuantidade[0],
        "quantidade":nomeEQuantidade[1]
      }
    )
    return acc
  },[]).sort((a,b)=>{
    return b.quantidade - a.quantidade
  }).slice(0,10)


function top10ArtistasEntreOsTop10(divWidth) {
    return {
        spec: {
            width: divWidth,
            data: {
                values: topDezArtistasQueMaisAparecemNoTopDezDeCadaAnoAPartirDeDoisMilEOnze
            },
               
            "mark": {
                "type": "bar", tooltip: true
            },
            "encoding": {
                "x": {
                    "field": "nome",
                    "type": "nominal",
                    "title": "Nome artista",
                    axis:{
                      labelAngle: -45
                    }
                },
                "y": {
                    "type": "quantitative",
                    "field": "quantidade",
                    "title": "Ocorrências no top 10"
                    
                }
            }
        }
    };
}

```



<div id="top10ArtistasEntreOsTop10" class="card">
        <h1>Artistas mais frequentes no Top 10 de cada ano a partir de 2011, inclusive</h1>
        <div style="width: 100%; margin-top: 15px;">
            ${ vl.render(top10ArtistasEntreOsTop10(divWidth -45)) }
        </div>
</div>


# Análise
Podemos inferir que, entre os artistas mais ouvidos, esses são os mais populares. De modo que há uma tendência maior das músicas lançadas por esses artistas estarem no Top10 do ano do lançamento da música. Também é possível notar que "The Weekend" e "Bad Bunny" estão na frente dos outros artistas, e o próximo passo, portanto, é buscarmos alguma relaçao das músicas desses 2 artistas para entendermos melhor

<br>

# Justificativa de visualização
Foi utilizado gráfico de barras, para facilitar a comparação dos dados, já que os dados são de duas categorias diferentes. Os dados do eixo X, "nome do artista", são do tipo catégorico, enquanto que os do eixo Y são do tipo quantitativo e representam a contagem de vezes que oa artista do eixo x esteve no Top10 desde 2011. O marcador visual é a barra e o canal visual é a cor aplicada na barra que, nesse caso, é o azul.

```js
const quantidadeDeStreamsDoTop10DeCadaAno = Object.entries(Object.entries(topDezArtistasEmCadaAno).filter((anoEMusicas) =>  anoEMusicas[0]>=2011).reduce((acc, anoEMusicas)=>{
    
    const musicas = anoEMusicas[1]
    const ano = anoEMusicas[0]

    musicas.forEach((musica) => {
    const nomeArtista = musica.nome
    const streamsArtista = musica.streams

      if (acc[nomeArtista] === undefined){
        acc[ano] = streamsArtista
      }else{
        acc[ano] += streamsArtista
      }
    })
    return acc
  }, {})).reduce((acc, anoEStreams)=>{
    acc.push(
      {
        "ano": anoEStreams[0],
        "streams":anoEStreams[1]
      }
    )
    return acc
  },[]).sort((a,b)=>{
    return a.ano - b.ano
  })


function quantidadeDeStreamsDoTop10DeCadaAnoGraphInterativo(divWidth) {
  return {
    "spec": {
      "description": "Quantidade de streams do top 10 de cada ano.",
      "data": { "values": quantidadeDeStreamsDoTop10DeCadaAno },
      "encoding": {
        "x": { "field": "ano", "type": "temporal", "title": "Ano" },
        "y": { "field": "streams", "type": "quantitative", "title": "Quantidade de Streams" }
      },
      "layer": [{
        "mark": { "type": "line" }
      }, {
        "params": [{
          "name": "hover",
          "select": { "type": "point", "on": "pointerover", "clear": "pointerout" }
        }],
        "mark": { "type": "circle", "tooltip": true },
        "encoding": {
          "opacity": {
            "condition": { "param": "hover", "value": 1 },
            "value": 0
          },
          "size": {
            "condition": { "param": "hover", "value": 48 },
            "value": 100
          },
        
        }
      }],
      "width": divWidth
    }
  };
}
```
<br>
<hr>

# Qual é a quantidade de artistas presentes nas músicas do "The Weekdn", quem mais aparece no Top10 ao longo dos anos, e do "Bad Bunny", segundo colocado ?

```js
const Top2ArtistasMaisFrequentesTop10 = []
console.log("🚀 ~ Top2ArtistasMaisFrequentesTop10:", Top2ArtistasMaisFrequentesTop10)

arquivo.forEach(musica => {
    if (musica['artist(s)_name'].includes('The Weeknd') || musica['artist(s)_name'].includes('Bad Bunny')) {
        // Adiciona a música ao array filtrado
        Top2ArtistasMaisFrequentesTop10.push(musica);
    }
});

const contagemPorNumeroDeArtistas = Object.entries(Top2ArtistasMaisFrequentesTop10.reduce((acc, musica) =>{
  if(acc[musica.artist_count] === undefined){
    acc[musica.artist_count] = 0
  }
  acc[musica.artist_count] += 1

  console.log("🚀 ~ contagemPorNumeroDeArtistas ~ acc:", acc)
  return acc
  
},{})).reduce((acc, qtdArtistasEQtdMusicas) => {
  console.log("🚀 ~ contagemPorNumeroDeArtistas ~ qtdArtistasEQtdMusicas:", qtdArtistasEQtdMusicas)
  
  acc.push({
    "quantidade_artistas":qtdArtistasEQtdMusicas[0],
    "quantidade_musicas": qtdArtistasEQtdMusicas[1]
  })
  return acc
}, [])


const Notas_Musicais_Not_Null = Top2ArtistasMaisFrequentesTop10.filter(row => row.key !== null);


console.log(contagemPorNumeroDeArtistas)


function ex01(divWidth) {
  return {
    spec: {
      width: divWidth,
      data: {
        values: contagemPorNumeroDeArtistas
      },
      "encoding": {
        "theta": { "field": "quantidade_musicas", "type": "quantitative", "stack":true},
        "color": { "field": "quantidade_artistas", "type": "nominal" },
      },
      "mark": {
        "tooltip": true
      },
      "layer": [{
        "mark": { "type": "arc", "outerRadius": 80, "tooltip": true }
      }, {
        "mark": { "type": "text", "radius": 90,"tooltip": true, fontSize: 16 },
        "encoding": {
          "text": { "field": "quantidade_musicas", "type": "quantitative"  }
        }
      }],
    }
  };
}

```
<div id="ex01" class="card">
        <h1>Artistas por música</h1>
        <div style="width: 100%; margin-top: 15px;">
            ${ vl.render(ex01(divWidth - 115)) }
        </div>
</div>

<br>

# Análise
 Pode-se perceber que o número de músicas Portanto, se uma música tiver mais de 2 participantes, dificilmente estará no Top10 ao longo dos anos, possui probabiblidade menor que 10%.

<br>

 # Justificativa de visualização
 Utilizamos o gráfico de pizza, pois queríamos representar a parte de um todo. Queríamos representar para todas as músicas que os artistas "The Weeknd" e "Bad Bunny" aparecem, a quantidade de músicas que tem 1, 2 ou 3 cantores participantes. Logo, o marcador visual é o total da área do gráfico, que são todas as músicas que esses dois cantores possuem participação e, os canais visuais são as diferentes cores utilizadas, que representam uma quantidade diferente de cantores presentes nas músicas.


<hr>
<br>

## Como é o comportamento da quantidade total de streams de todo o top 10 artistas ao longo dos anos?
Pensamos em observar o comportamento da quatidade de streams totais de um top 10 de um ano, para descobrir se havia alguma tendência de comportamento ao longo dos anos. Para isso, separamos as top 10 músicas de cada ano a partir de 2011

<div id="quantidadeDeStreamsDoTop10DeCadaAnoGraphInterativo" class="card">
        <h1>Quantidades de streams em cada top 10 de artistas ao longo dos anos</h1>
        <div style="width: 100%; margin-top: 15px;">
            ${ vl.render(quantidadeDeStreamsDoTop10DeCadaAnoGraphInterativo(divWidth -15)) }
        </div>
</div>

### Análise
A partir da análise do gráfico gerado, é possível observar que houve uma queda expressiva na popularidade das músicas entre o top 10 de 2017 e o top 10 de 2018, com uma diferença de mais de 1 bilhão de streams totais. Essa queda pode se dar pela popularidade da plataforma Spotify, que pode ter caído nesse ano. Além disso, pudemos observar que durante a pandemia da covid-19 houve um aumento expressivo de cerca de 1 bilhão de streams a mais em 2021 do que em 2020, número esse que foi caindo nos anos seguintes a pandemia, chegando a menos de 1 bilhão de streams ao final de julho de 2023(limite do dataset).

### Justificativa da visualização
Escolhemos um gráfico de linha para sermos capazes de observar alguma possível tendência sobre esses números. Queríamos ver se o spotify tinha uma tendência de ir crescendo ou diminuindo, em termos de quantidade de strams em cada Top10 por ano. Os marcadores visuais utilizados foram as linhas, interligadas por pontos. Já o canal visual utilizado foi a cor azul.