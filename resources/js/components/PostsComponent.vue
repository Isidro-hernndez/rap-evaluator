<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">


                <div class="card mb-3 mt-3" v-for="item in items">
                    <div class="card-header" v-bind:href="item.slug" v-text="item.title"></div>

                    <div class="card-body">
                        <p class="card-text" v-text="item.body"></p>
                    </div>
                </div>

                <infinite-loading @infinite="infiniteHandler"></infinite-loading>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                items: [],
                page: 0,

            };

            
        },
        methods: {
                infiniteHandler($state){
                    //conectar al servidor
                    this.page++;
                    let url = 'http://rap.isidro.dev/api/posts?page=' + this.page;
                    

                    axios.get(url)
                    .then(response => {
                        let posts = response.data.data;

                        if(posts.lenght){
                            this.items = this.items.concat(posts);
                            $state.loaded()
                        }else{
                            $state.complete();
                        }
                    })
                }
        }
    }
</script>
