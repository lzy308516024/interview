<template>
    <div>
        <div class="btn-wrap">
            <button @click="sortByDate">按日期排序</button>
            <button @click="sortByType">按类型排序</button>
        </div>
        <div class="img-wrap">
            <div class="box" v-for="(item,index) in list" :key= "index" >
                <p>名字：<span>{{item.name}} +++ {{item.sort}}</span></p>
                <p>创建日期<span>{{item.create_time}}</span></p>
                <div class="little">
                    <img :src="item.url" alt="" :img-attr="item.id"   draggable="true"  @dragstart="dragstart($event, item.id)" @dragend="dragend" @drop="drop" @dragover.prevent/>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import { data } from './data.js'

export default {
    

    data(){
        return {
         
            list:[],
            start:null,
            end:null,
            flag:false
        }
    },
    mounted(){
     this.list = data
    },
    methods:{
        sortByDate(){
            this.flag =false
            data.sort((a,b) => {
                
                return a.create_time<b.create_time ? 1:-1;
            })
            this.list = data;
        },
        sortByType(){
            this.flag = true
            this.list.sort(function(a,b){
                return a.sort>b.sort?1:-1
            }) 
        },
       
        dragstart (event, data) {
            
            this.start = data
           
            
        },
        dragend (event) {
            event.dataTransfer.clearData()
        },
        drop (event) {
           
           if(this.flag){
             this.end = event.toElement.getAttribute('img-attr');
            
            this.dropData = data
            let obj1 = {id:this.start}
            let obj2= {id:this.end}
            for(var i=0,len = this.list.length;i<len;i++ ){
                if(this.start == this.list[i].id){
                    obj1.url = this.list[i].url
                    obj1.sort = this.list[i].sort
                    obj1.name = this.list[i].name
                }
                if(this.end == this.list[i].id){
                    obj2.url = this.list[i].url
                    obj2.sort = this.list[i].sort
                    obj2.name = this.list[i].name
                }
            }
            this.list.map((item)=>{
                if(item.id == obj1.id){
                    item.url = obj2.url;
                    item.sort = obj2.sort
                    item.name = obj2.name
                }
                 if(item.id == obj2.id){
                    item.url = obj1.url;
                    item.sort = obj1.sort
                    item.name = obj1.name
                }
            })
          

           }
            
        }
       
    }
}
</script>
<style lang="scss">
    * {
        padding: 0;
        margin: 0;
    }
    .btn-wrap {
        display: flex;
        width: 80%;
        margin: 20px auto;
        height: 80px;
        border: 1px solid #eee;
        justify-content: space-around;
        align-items: center;
        button {
            width: 80px;
            height: 40px;
            background: darkcyan;
            color: #fff;
            border: none;
            border-radius: 10px;
            cursor: pointer;
        }
    }
    .img-wrap {
        width: 100%;
        padding:20px;
        display: flex;
        flex-wrap: wrap;
        .box {
            width: 20%;
            height: 300px;
            margin: 2% 2%;
            border:1px solid #ccc;
            .little {
                width:100%;
                max-height: 240px;
                overflow: hidden;
                img{
                    width: 100%;
                }
            }
            p {
                margin: 8px 0;
                font-size: 14px;
                text-align: left;
                text-indent: 2em;
            }
        }
    }
</style>