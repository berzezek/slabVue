<template >
    <div class="container mt-5" v-cloak>
        <Breadcrumb />
        <div class="d-flex ">
            <div class="input-group mb-3 me-3">
                <span class="input-group-text">min</span>
                <input type="text" class="form-control">
            </div>
            <div class="input-group mb-3">
                <span class="input-group-text">max</span>
                <input type="text" class="form-control">
            </div>
        </div>
        <div class="mb-2 d-flex justify-content-center" >
            <button class="btn btn-success me-3" v-on:click="listAll">Всё</button>
            
            <a href="#cardEnd"><button class="btn btn-success" v-on:click="listLess" v-if="listPlus > 0">Меньше</button></a>
        </div>

        <div class="row">
            <div class="col-md-3 mb-3" v-for="product in showProducts" :key="product.ID">
                <b-card class="shadow-lg">
                    <h4 class="text-end">{{ product.name }}</h4>
                    <div class="d-flex">
                        <div class="me-2">
                             <img :src=product.imageUrls[0].url alt="{{ product.name }}" class="mt-3">
                        </div>
                        <div class="ms-3 mt-5 justify-content-end text-end">
                            <h5>{{ numberWithSpaces(product.price) }} UZS</h5>
                        </div>
                        <!-- <div>
                            <b-card-text class="text-left" v-if="product.properties.BRAND">
                                Brand: {{ product.properties.BRAND }}
                            </b-card-text>
                            <b-card-text class="text-left" v-if="product.properties.COLOR">
                                COLOR: {{ product.properties.COLOR }}
                            </b-card-text>
                        </div> -->

                    </div>


                </b-card>

            </div>

        </div>
        <div class="mb-5 d-flex justify-content-center" id="cardEnd">
            <button class="btn btn-success me-3" v-on:click="listAdd">Больше</button>
            <a href="#cardEnd"><button class="btn btn-success" v-on:click="listLess" v-if="listPlus > 0">Меньше</button></a>
        </div>
    </div>
   
</template>

<script>
import axios from 'axios';
import Breadcrumb from '@/components/Breadcrumb/Breadcrumb.vue'

export default {
components: { Breadcrumb },
name: 'Products',
    
    data() {
        return {
            products: [],
            listPlus: 12,
            maxPrice: 0,
            minPrice: Infinity,
            show: "block"
        };
    
    },
    async mounted() {
        const Authorization = 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiIyMTMuMjMwLjEwMi4zNiIsImlhdCI6MTYzODU1MzY3NywiZXhwIjoxNzM4NTUzNjc3LCJzdWIiOiJzbGFiLmVjb21tZXJjZXNheXQifQ.Qq6OKJaM0GZ6j6Wiq_eIJFSYRuYBn08usQmacUzqb8s'
        let data = {
            "jsonrpc": "2.0",
            "method": 'products.get',
            "params": {
                "LastUpdatedDate": "2020-02-21T18:19:25Z",
                "WithProductPhotoOnly":0,
                "IncludeEmptyStocks":0
            },
        }
        await axios({
        method: 'post',
        url: 'https://api.billz.uz/v1/',
        data,
        headers: {Authorization},

      })
        .then(response => (this.products = response.data.result.filter(products => products.price > 1)));
    }, 
    computed: {
        showProducts() {
            return this.products.filter(products => products.price > this.maxPrice).filter(products => products.price < this.minPrice).slice(1, this.listPlus)
        }
    },
    methods: {
        listAdd() {
            this.listPlus += 12;
        },
        listLess() {
            this.listPlus -= 12;
        },
        listAll() {
            console.log(this.products.length);
            this.listPlus = this.products.length;
        },
        getImageUrl(url) {
            return url.replace(/_square/, '')
        },
        numberWithSpaces(number) {
            return number.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ");
        },
        shuffleArray(array) {
            let currentIndex = array.length, temporaryValue, randomIndex;

            while (0 !== currentIndex) {

                randomIndex = Math.floor(Math.random() * currentIndex);
                currentIndex -= 1;

                temporaryValue = array[currentIndex];
                array[currentIndex] = array[randomIndex];
                array[randomIndex] = temporaryValue;
            }
            return array;
        }
    },
}

</script>

<style lang="scss" scoped>
[v-cloak] {
    display: none;
}
</style>
