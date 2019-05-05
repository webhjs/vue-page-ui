<template>
	<div class="pager">
		<span class="disabled">{{ totalCount }}条记录</span>
		<a @click.prevent="clickPage(1)" :class="{current:1 == indexPage}">首页</a>
		<a @click.prevent="clickPage(lastPage)" :class="{disabled:lastPage == indexPage}">上一页</a>
		<a v-show="showTop">...</a>
		<a v-for="num in pagers" @click.prevent="clickPage(num)" :class="{current:num == indexPage}" >{{num}}</a>
		<a v-show="showFoot">...</a><a @click.prevent="clickPage(nextPage)" :class="{disabled:nextPage == indexPage}">下一页</a>
		<a @click.prevent="clickPage(pages)" :class="{current:pages == indexPage}">尾页</a>
		<input v-model="num" type="number"/>页<button @click="clickPage(num)">跳转</button>
	</div>
</template>

<script>
	export default{
		props:{
			totalCount:{ //总记录数
				type:Number,
				default:10
			},
			pageLimit:{ //每页显示几条
				type:Number,
				default:2
			},
			indexPage:{ //显示第几页
				type:Number,
				default:1
			},
		},
		data(){
			return {
				num:'',
				showTop:false,
				showFoot:false,
				pagerSize:5//page显示个数
			}
		},
		computed:{
			lastPage(){
				let page = this.indexPage - 1
				if(page < 1 ) page = 1
				return page
			},
			nextPage(){
				let page = this.indexPage + 1
				if(page > this.pages  ) page = this.pages
				return page
			},
			pages(){ //计算页码
				return Math.ceil(this.totalCount/this.pageLimit)
			},
			pagers(){ //页码存储数组
				const arr=[]//存储
				const asideSize = (this.pagerSize-1)/2 //一侧的页码个数
				const page = this.pages //总页码
				let current = this.indexPage //当前页码
				const offset={
					start : current - asideSize, //开始页码
					end : current + asideSize //结束页码
				}
				if(offset.start < 1){
					offset.start = 1
					offset.end = offset.end + (1-offset.start)
				}
				if(offset.end > page){
					offset.end = page
					offset.start = offset.start - (offset.end - page)
				}
				if(offset.start < 1) offset.start = 1
				this.showTop = (offset.start > 1)
				this.showFoot = (offset.end < page)
				for(let i= offset.start;i<=offset.end;i++){
					arr.push(i)
				}
				return arr
			}
		},
		watch:{
			num(e){
				if(e < 1) e=1
				this.num = e
			}
		},
		methods:{
			clickPage(page){
				this.$emit('update:indexPage', parseInt(page))
			}
		}
	}
</script>
	
<style scoped> 
	.pager { padding: 3px; text-align: right; color:#66C;font-size:12px; font-family:Tahoma; user-select: none;}   
	.pager a { margin: 2px; padding:2px 5px; color: #66C; text-decoration: none; border: 1px solid #aad; }   
	.pager a:hover { cursor:pointer;color: #000; border: 1px solid #009; background-color:#DCDCF3; }   
	.pager a.current{ font-weight: bold; margin: 0 2px; padding: 2px 5px; color: #fff; background-color: #66C; border: 1px solid #009; }   
	.pager span.disabled,.pager a.disabled { margin: 0 2px; padding: 2px 5px; color: #CCC; border: 1px solid #DDD; }    
	.pager input {width:40px;margin: 0px 2px -2px 2px; color:#66C; border: 1px solid #DDD; padding:2px; font-size:12px; font-family:Tahoma;text-indent:2px;}
	.pager button {width:50px;margin: 0px 2px -2px 2px;border: 1px solid #DDD; padding:2px; font-size:12px; font-family:Tahoma;background-color:white}
</style>
