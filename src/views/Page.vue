<template>
    <div class="list">
        <v-app-bar app color="grey darken-3" dark>
            <v-app-bar-title><h2 class="ml-2">Arushita Chadha</h2></v-app-bar-title>
            <v-spacer></v-spacer>
            <form @submit="searchProducts">
                <v-text-field
                    v-model="searchTerm"
                    hide-details
                    label="Search"
                    placeholder="Search"
                    filled
                    rounded
                    dense
                    single-line
                    append-icon="mdi-magnify"
                    class="shrink mr-4"
                >
                </v-text-field>
            </form>
        </v-app-bar>
        <v-container class="d-flex align-center justify-center mt-3" v-if="loading">
            <h1>Loading...</h1>
        </v-container>
        <v-container v-else-if="!details">
            <v-row class="d-flex align-center justify-center mt-3">
                <div v-if="!searched">
                    <v-col> <h1>Products</h1></v-col>
                </div>
                <div v-else>
                    <v-col>
                        <h1>{{ products.length }} Products Found</h1></v-col
                    >
                </div>
            </v-row>
            <v-row
                v-if="products.length != 0"
                class="d-flex align-center justify-center mt-3"
            >
                <div v-for="product in products" :key="product.id">
                    <v-col>
                        <v-card
                            elevation="2"
                            class="mx-auto"
                            max-width="400"
                            max-height="400"
                            outlined
                        >
                            <v-list-item three-line>
                                <v-list-item-content>
                                    <div class="text-overline mb-4">
                                        {{ product.type }}
                                    </div>
                                    <v-list-item-title class="text-h6 mb-1">
                                        {{ product.name }}
                                    </v-list-item-title>
                                    <v-list-item-subtitle>{{
                                        product.description
                                    }}</v-list-item-subtitle>
                                </v-list-item-content>

                                <img
                                    :src="product.image"
                                    width="80"
                                    height="80"
                                    alt="Product Image"
                                />
                            </v-list-item>
                            <v-list-item>
                                <v-list-item-content>
                                    <v-list-item-title class="mb-1">
                                        <span class="pr-6">
                                            Price: Rs. {{ product.price }}
                                        </span>
                                        <v-btn
                                            outlined
                                            rounded
                                            text
                                            @click="getDetails(product.id)"
                                        >
                                            Details</v-btn
                                        >
                                    </v-list-item-title>
                                </v-list-item-content>
                            </v-list-item>
                        </v-card>
                    </v-col>
                </div>
            </v-row>
            <v-row v-else class="d-flex align-center justify-center mt-1">
                <div>
                    <v-col> <p>No products matched your search, try again!</p></v-col>
                </div>
            </v-row>
            <v-row v-if="searched" class="d-flex align-center justify-center mt-1">
                <div>
                    <v-col>
                        <v-btn
                            @click="$router.go()"
                            class="ma-2"
                            color="purple lighten-4"
                        >
                            <v-icon dark left> mdi-arrow-left </v-icon>Back
                        </v-btn></v-col
                    >
                </div>
            </v-row>
        </v-container>
        <v-container v-else>
            <DetailS :id="id" @details="getBack" />
        </v-container>
    </div>
</template>
<script>
import axios from "axios";
import DetailS from "../components/Details.vue";

export default {
    name: "PageView",
    components: { DetailS },
    data() {
        return {
            loading: true,
            products: [],
            searchTerm: "",
            searched: false,
            details: false,
        };
    },
    methods: {
        getBack(value) {
            this.details = value;
        },
        async getProducts() {
            await axios
                .get("https://61922b19aeab5c0017105dfb.mockapi.io/product")
                .then((result) => {
                    this.products = result.data;
                    this.loading = false;
                })
                .catch((err) => {
                    console.log(err.response.data.message);
                });
        },
        async searchProducts() {
            let url =
                "https://61922b19aeab5c0017105dfb.mockapi.io/product?name=" +
                this.searchTerm;
            await axios
                .get(url)

                .then((result) => {
                    this.products = result.data;
                    this.searched = true;
                })
                .catch((err) => {
                    console.log(err.response.data.message);
                });
        },
        async getDetails(index) {
            this.id = index;
            this.details = true;
        },
    },
    beforeCreate() {
        this.loading = true;
    },
    mounted() {
        this.getProducts();
    },
};
</script>
