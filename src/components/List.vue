<script setup lang="ts">
    import ListItem, {ListItemT} from './ListItem.vue';
    import { onMounted, ref, watchEffect } from 'vue';
    
    interface postT extends ListItemT {
        userId: number
    }
    
    const baseApiRoute = "https://jsonplaceholder.typicode.com"
    let postsData = ref<postT[]>([])
    let searchData = ref<postT[] | null>([])
    let fetchError = ref<boolean>(false)
    let search = ref<string>('')

    const fetchApiData = () => {
        try {
            fetch(`${baseApiRoute}/posts`).then(response => response.json()).then(responseData => {
                postsData.value = responseData
                searchData.value = responseData
            })
        } catch {
            fetchError.value = true
        }
    }

    const deletePost = async (id: number) => {
        return await fetch(`${baseApiRoute}/posts/${id}`, {method: "DELETE"})    
    }

    const handleDeletePost = async (e: number) => {
        const deleteRequest = await deletePost(e);

        if(!deleteRequest?.ok) {
            alert("We couldn't delete this post try again")
            return
        }
        
        postsData.value = postsData.value.filter((postData) => {
            return postData.id !== e
        })
    }

    watchEffect(() => {
        searchData.value = postsData.value.filter((postData) => {
            return postData.title.includes(search.value)
        })
    })

    onMounted(() => {
        fetchApiData()
    })
</script>

<template>
    <section>
        <div>
            <form>
                <input type="text" v-model="search">
            </form>
        </div>
        <div v-if="!fetchError">
            <ul v-if="searchData && searchData.length > 0">
            <ListItem 
                v-for="post in searchData" 
                :key="post.id" 
                :title="post.title"
                :body="post.body"
                :id="post.id"
                @delete="handleDeletePost"
                />
            </ul>
            <div v-else>
                <p>There're no results, try with a different search value</p>
            </div>
        </div>
        <div v-else>
            <p>Woops there was an error Loading the Data</p>
        </div>
    </section>
</template>