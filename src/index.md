
## DataSet Completo

```js
const arquivo = await FileAttachment("./data/spotify-2023.csv").csv({typed: true});

arquivo.sort((a,b) => {
   
      return b.streams - a.streams
    
  })
view(Inputs.table(arquivo));
```


