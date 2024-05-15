# Questão 2 -  O conjunto das top 10 músicas e dos top 10 artistas varia muito se considerarmos apenas musicas lançadas no mesmo ano?


```js
const arquivo = await FileAttachment("./data/spotify-2023.csv").csv({typed: true});


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

  

  console.log({
    "🚀 ~ dezMusicasMaisPopulares:": dezMusicasMaisPopulares,
    "🚀 ~ streamsPorArtista": streamsPorArtista,
    "🚀 ~ dezArtistasMaisPopulares:": dezArtistasMaisPopulares,
    "🚀 ~ musicasEmCadaAno:": musicasEmCadaAno,
    "🚀 ~ topDezMusicasEmCadaAno ~ topDezMusicasEmCadaAno:": topDezMusicasEmCadaAno,
    "🚀 ~ topDezArtistasEmCadaAno": topDezArtistasEmCadaAno,
  })


```
