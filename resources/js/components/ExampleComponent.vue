<template>
    <div class="container">
        <form @submit.prevent="submit">
            <div>
                <label for="title">Title</label>
                <input type="text" v-model="form.title">
            </div>
            <div>
                <label for="body">Body</label>
                <input type="text" v-model="form.body">
            </div>
            <input v-if="idToUPload===null" type="submit" value="Submit">
            <input v-else type="submit" value="Submit">
        </form>
        <div>
            <table v-if="posts">
                <tr>
                    <th>Title</th>
                    <th>Body</th>
                    <th>Action</th>
                </tr>
                <tr v-for="post in posts" :key="post.id">
                    <td>{{post.title}}</td>
                    <td>{{post.body}}</td>
                    <td>
                        <button @click="edit_id(post.id)">Edit</button>
                        <button @click="delete_id(post.id)">Delete</button>
                    </td>
                </tr>
            </table>
        </div>
    </div>
</template>

<script>
const url='http://localhost:8000'
import axios from 'axios'
    export default {
        data:()=>({
          form:{
                title:null,
                body:null
          },
          posts:null,
          idToUPload:null
        }),
        methods:{
            submit(){
              if(this.idToUPload===null){
                console.log(this.form);
                axios.post(url+'/api/posts', this.form).then(res=>{
                  console.log("res form api",res.data)
                  this.form.title=null
                  this.form.body=null
                  this.get()
                }).catch(err=>{
                  console.log("error",err.response)
                })
              }else{
                this.update_id(this.idToUPload)
              }
            },
            get(){
                axios.get(url+'/api/posts').then(res=>{
                    this.posts=res.data
                }).catch(err=>{
                    console.log("error",err.response)
                })
            },
            update_id(id){
                axios.put(url+'/api/posts/'+id, this.form).then(res=>{
                    console.log("update ",res.data)
                }).catch(err=>{
                    console.log("error",err.response)
                })
            },
            edit_id(id){
                axios.get(url+'/api/posts/'+id).then(res=>{
                  console.log(res)
                  this.form.title=res.data.title
                  this.form.body=res.data.body
                  this.idToUPload=res.data.id
                }).catch(err=>{
                    console.log("error",err.response)
                })
            },
            delete_id(id){
                axios.delete(url+'/api/posts/'+id).then(res=>{
                    console.log("delete ",res.data)
                    this.get()
                  this.idToUPload=null
                }).catch(err=>{
                    console.log("error",err.response)
                })

            }
        },
        created(){
            this.get()
        },
        mounted() {
            console.log('Mounted')
        }
    }
</script>
<style lang="scss" scoped>
table,tr,th,td{
  border:1px solid #dde0e3;
}
</style>