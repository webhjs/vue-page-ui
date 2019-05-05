<template>
	<div class="pages-warp">
      <span>共&nbsp;{{totalNum}}&nbsp;条</span>  
      <span class="page">
         <button class="last" :disabled="pageIndex==1" @click="selectPage(pageIndex-1)"><i class="fa fa-angle-left fa-lg"></i></button>
         <span v-show="showLast">...</span>
         <span v-for="item in pagelist" :class="{'onIndex':pageIndex==item}" v-text="item" @click="selectPage(item)"></span>
         <span v-show="showNext">...</span>
         <button class="next" :disabled="pageIndex==total" @click="selectPage(pageIndex+1)"><i class="fa fa-angle-right fa-lg"></i></button>
      </span>
      <span class="select" @click.stop="changPage();onPagesEvent()" ref="pages_parent">
         <div>&nbsp;{{ limitPage }}&nbsp;条/页&nbsp;&nbsp;<i class="fa fa-lg" :class="isPageShow?'fa-angle-up':'fa-angle-down'"></i></div>
         <ul class="option" :class="{'hidden':isPageShow}" ref="pages">
            <li v-for="(item,index) in limitList" v-html="item.name" @click="selectLimit(item)"></li>
         </ul>
      </span>
      <span class="jump">跳转至&nbsp;<input v-model="jump" type="number" min="1" :max="total" class="res"/>&nbsp;页</span>
   </div>
</template>

<script>
   export default{
      props:{
         totalNum:{
            type:Number,
            default:0
         }, //总条数
         limitPage:{
            type:Number,
            default:5
         }, //每页几条
         pageIndex:{
            type:Number,
            default:1
         } //当前页码
      },
      data(){
         return {
            "isPageShow":true,
            "limitList":[
               { 
                  "name": "5&nbsp;条/页",
                  "value":5
               },
               {
                  "name": "15&nbsp;条/页",
                  "value":15
               },
               {
                  "name": "25&nbsp;条/页",
                  "value":25
               }
            ],
            showLast:false,
            showNext:false,
            pageContol:7,
            jump:null, //跳转页面
         }
      },
      watch:{
         'jump':function(newVal,oldVal){
            if(newVal>this.total || newVal<1) return
            this.$emit('update:pageIndex', parseInt(newVal))
         }
      },
      computed:{
         pagelist(){
            let star,end,arr=[]
            
            if(this.total <= this.pageContol){
               star = 1
               end = this.total
               for(let i=star;i<=end;i++){
                  arr.push(i);
               }
               return arr;
            }
            if(this.pageIndex-this.floor>1){
               this.showLast = true
               star = this.pageIndex-this.floor
            }else{
               this.showLast = false
               star = 1
            }
            if(this.pageIndex+this.floor<this.total){
               this.showNext = true
               end = this.pageIndex+this.floor
            }else{
               this.showNext = false
               end = this.total
            }
            
            for(let i=star;i<=end;i++){
               arr.push(i);
            }
            return arr;
         },
         floor(){
            return Math.floor(this.pageContol/2)
         },
         total(){
            return Math.ceil(this.totalNum/this.limitPage)
         }
      },
      methods:{
         changPage(){
            this.isPageShow = !this.isPageShow
         },
         selectLimit(item){
            if(item.value == this.limitPage) return
            this.$emit('update:limitPage', item.value)
         },
         selectPage(item){
            if(item == this.pageIndex) return
            this.$emit('update:pageIndex', item)
         },
         onPagesEvent(){
            let docScroll = document.documentElement.scrollTop;
            let winHeight = window.innerHeight + docScroll;
            let paParHeight = this.$refs.pages_parent.offsetHeight;
            let paCliHeight = this.$refs.pages.offsetHeight;
            let docHeight = this.$refs.pages_parent.offsetTop+paCliHeight+paParHeight;
            if( docHeight > winHeight ){
               this.$refs.pages.style.top = -128+'px';
            }else{
               this.$refs.pages.style.top = 38+'px';
            }
         }
      },
      mounted(){
         let timer = null;
         window.onresize = () => { 
            if(timer){
               clearInterval(timer);
            }
            timer = setTimeout(this.onPagesEvent(),300);
         };
         window.onscroll = () => { 
            if(timer){
               clearInterval(timer);
            }
            timer = setTimeout(this.onPagesEvent(),300);
         }
      }
   }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
   button{
      background:none;
      border:none;
      cursor:pointer;
   }
   .hidden{
      visibility: hidden;
   }
   .pages-warp{
      text-align:center;
      padding:20px 0;
      color:#7c869b;
      user-select:none; 
   }
   .onIndex{
      color:#56a5f7;
      font-size:20px;
      text-decoration:underline;
   }
   .last i,.next i{
      font-size:24px;
      padding:0 10px;
   }
   button:disabled{ 
      cursor:not-allowed;
   }
   .select{
      margin:0 20px;
      display:inline-block;
      height:30px;
      line-height:30px;
      width:100px;
      border:1px solid #ccc;
      border-radius:5px;
      position:relative;
      cursor:pointer;
   }
   .select .option{
      display:inline-block;
      width:100px;
      position:absolute;
      will-change:top;
      top:38px;
      //top:-128px;
      left:0;
      background:white;
      box-shadow:	2px 0px 2px 1px #ccc; 
      border-radius:5px;
      line-height:40px;
   }
   .select ul.option li:hover{
      background:#eee;
   }
   .page span{
      cursor:pointer;
      margin:0 5px;
      padding:0 5px;
   }
   .jump input{
      display:inline-block;
      height:25px;
      width:60px;
      border:1px solid #ccc;
      border-radius:5px;
      margin:0 5px;
      text-indent:8px;
   }
</style>
