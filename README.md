# vue-page-ui
this is the two page-ui components
# .sync 在2.3中作为语法糖写法
>作用:在子组件vue-server-ui中用this.$emit('update:value', newVal)触发父组件value的更新
## page 组件
>新分页类组件
### 父组件调用方式：
#### script代码:
>```import Page from '@/components/page'``` 
><br>```components:{Page}```</br>
#### template代码:
>**```<page class="page"	:totalNum.sync="totalNum" :limitPage.sync="limitPage" :pageIndex.sync="pageIndex"></page>```**
>>子组件依赖三个值分别是:totalNum:传入的记录总条数、limitPage:传入了每页显示条数、pageIndex:当前页码。
>>>备注：totalNum、limitPage、pageIndex 这两个值limitPage、pageIndex可以被子组件更改
---
## paging 组件
>老分页类组件
### 父组件调用方式：
#### script代码:
>```import Paging from '@/components/paging'``` 
><br>```components:{Paging}```</br>
#### template代码:
>**```<paging class="page" :totalCount.sync="totalCount" :pageLimit.sync="pageLimit" :indexPage.sync="indexPage"></paging>```**
>>子组件依赖三个值分别是:totalCount:传入的记录总条数、pageLimit:传入了每页显示条数、indexPage:当前页码。
>>>备注：totalCount、pageLimit、indexPage 子组件只对indexPage做操作
