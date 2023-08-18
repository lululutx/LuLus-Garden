```js
    handleRemove(file) { 
      let index = this.$refs.upload.uploadFiles.findIndex( 
        (item) => item.uid === file.uid 
      ); 
      this.$refs.upload.uploadFiles.splice(index, 1); 
    }, 

```
