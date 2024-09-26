<template>
    <section class="jumbotron">
        <h3 class="jumbotron-heading">Search Github Users</h3>
        <div>
            <input 
                type="text" 
                placeholder="enter the name you search" 
                v-model="keyWord" 
                @keyup.enter="searchUsers" 
            />
            &nbsp;
            <button @click="searchUsers">Search</button>
         </div>
    </section>
</template>

<script>
import axios from 'axios';

export default {
    name:'Search',
    data() {
        return {
            keyWord:''
        }
    },
    methods:{
        searchUsers(){
            this.$bus.$emit('updateListData',{isFirst:false,isLoading:true,errMag:'',users:[]})
            axios.get(`https://api.github.com/search/users?q=${this.keyWord}`).then(
                res=>{
                    console.log('请求成功了！',res.data.items);
                    this.$bus.$emit('updateListData',{isLoading:false,errMag:'',users:res.data.items})
                },
                err=>{
                    console.log('请求失败了！',err.message);
                    this.$bus.$emit('updateListData',{isLoading:false,errMag:err.message,users:[]})
                }
            )
        }
    }
}
</script>