```vue
 
      this.formItem_popup.dateStart = new Date( 
        new Date(val.dataTime).getTime() - 1000 * 60 * 60 * 24 * 30 
      ).format("yyyy-MM-dd"); 

```
