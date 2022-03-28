<template>
    <nav aria-label="breadcrumb">
        <ol class="breadcrumb" >
            <li class="breadcrumb-item" v-for="li in category" :key=li.id><a href="#">{{ li.name }}</a></li>
        </ol>
    </nav>
</template>

<script>
import axios from 'axios';
export default {
    name: 'Breadcrumb',
    data () {
        return {
            category: []
        }
    },
    mounted() {
        const Authorization = 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiIyMTMuMjMwLjEwMi4zNiIsImlhdCI6MTYzODU1MzY3NywiZXhwIjoxNzM4NTUzNjc3LCJzdWIiOiJzbGFiLmVjb21tZXJjZXNheXQifQ.Qq6OKJaM0GZ6j6Wiq_eIJFSYRuYBn08usQmacUzqb8s'
        let data = {
            "jsonrpc": "2.0",
            "method": "catalog.get",
            "params": {
                "perPage": 10,
                "page": 1,
                "sort": {
                    "type": "num",
                    "field": "id",
                    "order": "desc"
                }
            },
            "id": "1200"
        }
        axios({
        method: 'post',
        url: 'https://api.billz.uz/v2/',
        data,
        headers: {Authorization},

      })
        .then(response => (this.category = response.data.result.data.results));
    },
    methods: {
        showCategory() {
            console.log(this.category)
        }
    }
}
</script>

<style scoped>

</style>