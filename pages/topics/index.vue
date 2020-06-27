<template>
    <div class="container">
        <h2>Latest Topics</h2>

        <div class="bg-light mt-5 mb-5" v-for="(topic, index) in topics" :key="index">
            <h2>
                <nuxt-link
                :to="{ name:'topics-id', params:{id: topic.id }}"
                >{{ topic.title }}</nuxt-link>

            </h2>

            <div v-if="authenticated">
                <div v-if="user.id === topic.user.id">
                    <nuxt-link :to="{ name:'topics-edit', params:{id: topic.id }}">
                        <button class="btn btn-outline-success fa fa-edit fa-2x pull-right"></button>
                    </nuxt-link>

                    <button class="btn btn-outline-danger fa fa-trash fa-2x pull-right" @click="deleteTopic(topic.id)"></button>
                </div>
            </div>

            <p class="text-muted">{{ topic.created_at }} by {{ topic.user.name }}</p>

            <div class="ml-5 content" v-for="(content,index) in topic.posts" :key="index">
                {{ content.body }}
                <p class="text-muted">{{ content.created_at }} by {{ content.user.name }}</p>

                <!--Add Likes Button-->
                <div class="btn btn-outline-primary fa fa-thumbs-up ml-5 mb-2" @click="likePost(topic.id, content)">
                    <span class="badge">{{ content.like_count }}</span>
                </div>
            </div>
        </div>

        <nav>
            <ul class="pagination justify-content-center">
                <li class="page-item" v-for="(key, value) in links">
                    <a href="#" class="page-link" @click="loadMore(key)">{{ value }}</a>
                </li>
            </ul>
        </nav>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                topics: [],
                links: []
            }
        },

        async asyncData({$axios}){
            let {data, links} = await $axios.$get('/topics')
            //console.log(links)
            return {
                topics:data,
                links: links
            }
        },

        methods:{
            async loadMore(key){
                let {data} = await this.$axios.$get(key)
                return this.topics = {...this.topics, ...data}
            },

            async deleteTopic(id){
                await this.$axios.$delete(`/topics/${id}`)
                this.$router.push('/')
            },

            async likePost(topicId, content){
                const userFromVuex = this.$store.getters["auth/user"];
                if(userFromVuex){

                    //can't like your own post
                    if(userFromVuex.id === content.user.id){
                        alert('You can\'t like your own post')
                    }

                    //if User have already Liked
                    if(content.users){
                        if(content.users.some(user=>user.id === userFromVuex.id)){
                            alert('You have already liked this post')
                        }else{
                            await this.$axios.$post(`/topics/${topicId}/posts/${content.id}/likes`)
                            let {data, links} = await this.$axios.$get('/topics')
                            this.topics = data
                            this.links = links
                        }
                    }
                } else{
                    alert('please Log in')
                    this.$router.push('/login')
                }
            }
        }
    }
</script>

<style scoped>
    .bg-light.mt-5.mb-5 {
        padding: 20px;
    }
</style>
