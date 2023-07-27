| border | 是否带有纵向边框 | boolean | — | false |
| --- | --- | --- | --- | --- |

```vue


>>>.el-table::before {
  height: 0px;
}


>>>.el-table tr::before {
  height: 0px;
}


>>>.el-table td.el-table__cell, .el-table th.el-table__cell.is-leaf {
  border-bottom: none ;
}
>>>.el-table .el-table__cell.is-center{
  border-bottom: none ;
}
>>>.el-table .el-table__cell.is-leaf{
  border-bottom: none ;
}
```
