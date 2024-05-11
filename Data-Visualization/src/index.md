# DataSet

```js
const arquivo = await FileAttachment("./data/spotify-2023.csv").csv({typed: true});

view(Inputs.table(arquivo));

MostStreamedSongs = {
    const most_streamed_songs = arquivo.map((musica) => musica["streams"])
    arquivo.sort((a, b) => a.streams - b.streams);
    return 
}



MostStreamedSongs = {
    const most_streamed_songs = arquivo.map(musica => ({
     Time: x.Time,
     Partidas:0
   }));

   campeonatoBrasileiroFull.forEach(jogo => {
     let time = contagemDePartidas.find(x => x.Time === jogo["mandante"]);
     time.Partidas++;
     time = contagemDePartidas.find(x => x.Time === jogo["visitante"]);
     time.Partidas++;
   });
   return contagemDePartidas.sort((a,b) => {
     if (a.Partidas === b.Partidas)
       return a.Time.localeCompare(b.Time);
     else
       return b.Partidas - a.Partidas;
   });
}




times = {
  let temp = campeonatoBrasileiroFull.map((jogo) => jogo["mandante"]);
  let array = Array(...new Set(temp));
  return array.map(time => ({ Time: time }))
```