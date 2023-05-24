<script lang="ts">
import ListItem from './ListItem.vue'

    export type ListItemT = {
        title: string
        body: string
        id: number
    }
    
    interface postT extends ListItemT {
        userId: number
    }

    export default {
        components: { ListItem },
        data() {
            return {
                baseApiRoute: "https://jsonplaceholder.typicode.com",
                posts: [],
                searchPosts: [],
                searchString: ''
            };
        },
        methods: {
            fetchApiData() {
                const response = fetch(`${this.baseApiRoute}/posts`).then(response => response.json()).then(data => {
                    this.posts = data
                    this.searchPosts = data
                });
            },
            handleDeletePost(id: number) {
                fetch(`${this.baseApiRoute}/posts/${id}`, {method: "DELETE"}).then(() => {
                    const filteredPosts = this.posts.filter((post) => {
                        return post.id !== id
                    })
                    
                    this.searchPosts = filteredPosts
                    this.posts = filteredPosts
                })
            }
        },
        mounted() {
            this.fetchApiData();
        },
        watch: {
            searchString(newValue, oldValue){
                this.searchPosts = this.posts.filter(post => post.title.includes(newValue))
            }
        }
}
</script>

<template>
    <section>
        <div>
            <form>
                <input type="text" v-model="searchString">
            </form>
            <p>Posts Results</p>
            <ul>
                <ListItem 
                    v-for="post in searchPosts" 
                    :key="post.id" 
                    :title="post.title"
                    :postId="post.id"
                    :body="post.body"
                    @delete="handleDeletePost"
                />
            </ul>
        </div>
    </section>
</template>