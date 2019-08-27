<template>
    <div>
        <div class="btn-wrap">
            <button @click="sortByDate">按日期排序</button>
            <button @click="sortByType">按类型排序</button>
        </div>
        <div class="img-wrap">
            <div class="box" v-for="(item,index) in list" :key= "index">
                <div class="little">
                    <img :src="item.url" alt="">
                </div>
                
                <p>名字：<span>{{item.name}} +++ {{item.sort}}</span></p>
                <p>创建日期<span>{{item.create_time}}</span></p>
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
            list1:null
        }
    },
    mounted(){
     this.list = data
    },
    methods:{
        sortByDate(){
            data.sort((a,b) => {
                
                return a.create_time<b.create_time ? 1:-1;
            })
            this.list = data;
        },
        sortByType(){
            
            this.list.sort(function(a,b){
                return a.sort>b.sort?1:-1
            }) 
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