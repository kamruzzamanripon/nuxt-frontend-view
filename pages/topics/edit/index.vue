<template>
    <div class="container">
        <!--<h2>Edit Post id of {{$route.params.id}}</h2>-->
        <h2>Update Topic Title</h2>

        <form @submit.prevent="update">
            <div class="form-group mt-5">
                <input type="text" class="form-control" v-model="topic.title">
                <small class="form-text text-danger" v-if="errors.title">{{ errors.title[0] }}</small>
            </div>
            <button class="btn btn-outline-success">Update</button>
        </form>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                topic:{
                    title: ''
                }
            }
        },

        async asyncData({$axios, params}){
            const {data} = await $axios.$get(`/topics/${params.id}`)
            return {topic: data}
        },

        methods:{
            async update(){
                await this.$axios.patch(`/topics/${this.$route.params.id}`, {title:this.topic.title})

                //redirect
                this.$router.push('/topics')
            }

        }
    }
</script>
