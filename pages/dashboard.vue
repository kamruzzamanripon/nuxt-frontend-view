<template>
    <div class="container col-md-6 mt-5">
        <h2>User DashBoard</h2>
        <hr>
        <h3>Create a new topics</h3>
        <br>
        <form @submit.prevent="create">
            <div class="form-group">
                <label>Topic Title</label>
                <input v-model="form.title" type="text" class="form-control"  placeholder="Enter Title" autofocus>
                <small class="form-text text-danger" v-if="errors.title">{{ errors.title[0]}}</small>
            </div>
            <div class="form-group">
                <label >Body</label>
                <textarea class="form-control" rows="5" v-model="form.body"></textarea>
                <small class="form-text text-danger" v-if="errors.body">{{ errors.body[0]}}</small>
            </div>

            <button type="submit" class="btn btn-primary">Create</button>
        </form>
        <br>
        <p>Don't have an Account? <nuxt-link to="/register">Register</nuxt-link></p>
    </div>
</template>

<script>
    export default {
        middleware:['auth'],
        data(){
            return {
                form: {
                    title:'',
                    body:''
                }
            }
        },

        methods:{
            async create(){
                await this.$axios.$post('/topics', this.form)
                this.$router.push('/')
            }
        }
    }
</script>
