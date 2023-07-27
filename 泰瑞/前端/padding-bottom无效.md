![image.png](https://cdn.nlark.com/yuque/0/2023/png/26798000/1684148405229-109702c5-1877-493b-be60-d1f16e1627a1.png#averageHue=%23eeecec&clientId=u0579283b-32dc-4&from=paste&height=961&id=uccd6415f&originHeight=961&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=260598&status=done&style=none&taskId=ubef48d75-4950-442b-893a-3b83771f905&title=&width=1920)![image.png](https://cdn.nlark.com/yuque/0/2023/png/26798000/1684148419752-aaf2b4cd-07c9-411e-ba80-c2bc8616dba2.png#averageHue=%23eeecec&clientId=u0579283b-32dc-4&from=paste&height=961&id=uf3ec1189&originHeight=961&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=260049&status=done&style=none&taskId=uc76db923-1d52-460b-84bf-bde03a9352c&title=&width=1920)


**H:3 这里必须是auto不能是100%**
```vue
.aside {
    width: 20%;
    height: auto;   
    border: solid 1px red;
    display: flex;
    flex-direction: column;
    padding-top: 80px;
    padding-bottom: 80px;
    font-size: 50px;
    color: aliceblue;


    .part {
      height: 33%;
      flex: 1;
      width: 100%;
      border: solid 1px red;


    }
  }
```
