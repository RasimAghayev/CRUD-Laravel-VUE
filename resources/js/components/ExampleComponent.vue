<template>
    <div class="container">
        <form @subit.prevent="submit">
            <div>
                <label for="title">Title</label>
                <input type="text" v-model="form.title">
            </div>
            <div>
                <label for="body">Body</label>
                <input type="text" v-model="form.body">
            </div>
            <input type="submit" value="Submit">
        </form>
        <div>
            <table>
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
import axios from 'axios'
    export default {
        name:'ExampleComponent',
        data:()=>({
            form:{
                title:null,
                body:null
            },
            posts:null
        }),
        methods:{
            submit(){
                axios.post('http://localhost:8000/api/posts',this.form).then(res=>{
                    console.log("res from api",res.data)
                }).catch(err=>{
                    console.log("error",err.response)
                })
            },
            get(){
                axios.get('http://localhost:8000/api/posts').then(res=>{
                    this.posts=res.data
                }).catch(err=>{
                    console.log("error",err.response)
                })
            },
            update_id(id){
                axios.put('http://localhost:8000/api/posts'+id,this.form).then(res=>{
                    console.log("update ",res.data)
                }).catch(err=>{
                    console.log("error",err.response)
                })

            },
            edit_id(id){
                axios.get('http://localhost:8000/api/posts'+id).then(res=>{
                    this.from.title=res.data.title
                    this.from.body=res.data.body
                }).catch(err=>{
                    console.log("error",err.response)
                })

            },
            delete_id(id){
                axios.delete('http://localhost:8000/api/posts'+id).then(res=>{
                    console.log("delete ",res.data)
                    this.get()
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
