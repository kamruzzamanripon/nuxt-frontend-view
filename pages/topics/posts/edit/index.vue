<template>
    <div class="container">
        <!--<h2>Edit Post id of {{$route.params.id}}</h2>-->
        <h2>Update Post</h2>

        <form @submit.prevent="update">
            <div class="form-group mt-5">
                <textarea rows="5" class="form-control" v-model="post.body"></textarea>
                <small class="form-text text-danger" v-if="errors.body">{{ errors.body[0] }}</small>
            </div>
            <button class="btn btn-outline-success">Update</button>
        </form>
        <p class="mt-5 btn btn-outline-warning">
            <nuxt-link to="/topics">Back to Topics</nuxt-link>
        </p>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                post:{
                    body: ''
                }
            }
        },

        async asyncData({$axios, params}){
            const {data} = await $axios.$get(`/topics/${params.id}/posts/${params.body}`)
            return {post: data}
        },

        methods:{
            async update(){
                await this.$axios.patch(`/topics/${this.$route.params.id}/posts/${this.$route.params.body}`, {body:this.post.body})

                //redirect
                this.$router.push('/topics')
            }

        }
    }
</script>
