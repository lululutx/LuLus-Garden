```vue
<html>
  <head>
    <p style="font-size: 20px;color: red;">使用table标签方式将json导出xls文件</p>
    <button onclick='tableToExcel()'>导出</button>
    </head>
    <body>
    <script>  
    const tableToExcel = () => {
    // 要导出的json数据
    const jsonData = [
      {
        name:'路人甲',
        phone:'123456',
        email:'123@123456.com'
      },
      {
        name:'炮灰乙',
        phone:'123456',
        email:'123@123456.com'
      },
      {
        name:'土匪丙',
        phone:'123456',
        email:'123@123456.com'
      },
      {
        name:'流氓丁',
        phone:'123456',
        email:'123@123456.com'
      },
    ]
    // 列标题
    let str = '<tr><td>姓名</td><td>电话</td><td>邮箱</td></tr>';
    // 循环遍历，每行加入tr标签，每个单元格加td标签
    for(let i = 0 ; i < jsonData.length ; i++ ){
      str+='<tr>';
      for(const key in jsonData[i]){
        // 增加\t为了不让表格显示科学计数法或者其他格式
        str+=`<td>${ jsonData[i][key] + '\t'}</td>`;     
      }
      str+='</tr>';
    }
    // Worksheet名
    const worksheet = 'Sheet1'
    const uri = 'data:application/vnd.ms-excel;base64,';

    // 下载的表格模板数据
    const template = `<html xmlns:o="urn:schemas-microsoft-com:office:office" 
        xmlns:x="urn:schemas-microsoft-com:office:excel" 
        xmlns="http://www.w3.org/TR/REC-html40">
        <head><!--[if gte mso 9]><xml><x:ExcelWorkbook><x:ExcelWorksheets><x:ExcelWorksheet>
        <x:Name>${worksheet}</x:Name>
        <x:WorksheetOptions><x:DisplayGridlines/></x:WorksheetOptions></x:ExcelWorksheet>
        </x:ExcelWorksheets></x:ExcelWorkbook></xml><![endif]-->
        </head><body><table>${str}</table></body></html>`;
    // 下载模板
    window.location.href = uri + base64(template);
  };

  // 输出base64编码
  const base64 = s => window.btoa(unescape(encodeURIComponent(s)));
</script>
</body>
  </html>
```
[https://blog.csdn.net/hhzzcc_/article/details/80419396/](https://blog.csdn.net/hhzzcc_/article/details/80419396/)

附：去掉转义字符
```
1.去掉转义字符 s = s.replace(/[\'\"\\\/\b\f\n\r\t]/g, '');

2. 去掉特殊字符 s = s.replace(/[\@\#\$\%\^\&\*\{\}\:\"\L\<\>\? ]/); 
```

