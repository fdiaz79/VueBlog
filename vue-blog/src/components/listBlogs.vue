<template>
    <div v-theme:column="'narrow'" id="show-blogs">
        <h1> List Blog Titles</h1>
        <input type="text" v-model="search" placeholder="search box" />
        <div v-for="blog in filteredBlogs" class="single-blog" :key="blog.id" >
            <router-link v-bind:to="'/blog/' + blog.id">
                <h2 v-rainbow>{{blog.title | to-uppercase}} </h2>
            </router-link>
        </div>    
    </div>
</template>

<script>
    import searchMixin from '../mixins/searchMixin';
    export default {
    
        data() {
            return {
                blogs: [],
                search:''
            }
        },
        methods: {
        
        },
        created() {
            this.$http.get('https://vueblog-df35c.firebaseio.com/posts.json').then(function(data) {
                return data.json();
            }).then(function(data) {
                var blogsArray = [];
                for (let key in data){
                    data[key].id = key;
                    blogsArray.push(data[key]);
                }
                this.blogs = blogsArray;
            });
        },
        computed: {
            
        },
        // filters locally registered. To be used only in this component
        filters: {
            'to-uppercase': function(value) {
                return value.toUpperCase();
            }
        },
        // directives locally registered. To be used only in this component
        directives: {
            'rainbow': {
                bind(el,binding,vnode) {
                    el.style.color='#'+Math.random().toString().slice(2,8)
                }
            }
        },
        mixins: [searchMixin]
    };
</script>

<style>
    #show-blogs{
        max-width: 800px;
        margin: 0 auto;
    }
    .single-blog{
        padding: 20px;
        margin: 20px 0;
        box-sizing: border-box;
        background: #eee;
    }
</style>
