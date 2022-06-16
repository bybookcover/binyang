# 使用nanoid 自动生成id序列



```
import {nanoid} from 'nanoid'
export default {
  name: 'MyHeader',
  methods:{
    add(e){
      const todoObj = {id:nanoid(),title:e.target.value,done:false}
    }
  }
```
