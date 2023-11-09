
```
npm i json-loader
```


``` vue
//vue.config.js
  configureWebpack: {
    module: {
      rules: [{
        test: /\.geojson$/,
        loader: 'json-loader'
      }]
    },
  },
```