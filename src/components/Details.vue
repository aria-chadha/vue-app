<template>
    <div class="details">
        <v-container class="d-flex align-center justify-center mt-3" v-if="loading">
            <h1>Loading...</h1>
        </v-container>
        <v-row v-else class="d-flex align-center justify-center mt-3">
            <div>
                <v-col>
                    <v-card elevation="2" max-width="900" class="mx-auto" outlined>
                        <v-list-item>
                            <v-list-item-content>
                                <div class="text-overline mb-2">
                                    {{ product.type }}
                                </div>
                                <v-list-item-title class="text-h6 mb-4">
                                    {{ product.name }}
                                </v-list-item-title>
                                <v-list-item-subtitle>{{
                                    product.description
                                }}</v-list-item-subtitle>
                            </v-list-item-content>
                            <div class="text-h6">
                                <span class="pr-6"> Rs. {{ product.price }} </span>
                            </div>
                        </v-list-item>
                        <div class="d-flex align-center justify-center mb-6 pt-4">
                            <img
                                :src="product.image"
                                style="border-radius: 10px"
                                alt="Product Image"
                            />
                        </div>
                    </v-card>
                </v-col>
            </div>
        </v-row>
        <v-row
            ><v-col class="d-flex align-center justify-center mt-3"
                ><v-btn @click="goBack" class="ma-2" color="purple lighten-4">
                    <v-icon dark left> mdi-arrow-left </v-icon>Back
                </v-btn>
            </v-col></v-row
        >
    </div>
</template>
<script>
import axios from "axios";

export default {
    name: "DetailS",
    props: ["id"],
    data() {
        return {
            product: {},
            loading: true,
        };
    },
    methods: {
        async getProduct() {
            await axios
                .get("https://61922b19aeab5c0017105dfb.mockapi.io/product/" + this.id)
                .then((result) => {
                    this.product = result.data;
                    this.loading = false;
                })
                .catch((err) => {
                    console.log(err.response.data.message);
                });
        },
        goBack() {
            console.log("Going Back");
            this.$emit("details", false);
        },
    },
    beforeCreate() {
        this.loading = true;
    },
    mounted() {
        this.getProduct();
    },
};
</script>
