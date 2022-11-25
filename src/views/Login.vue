<template>
    <div class="home">
        <v-app-bar app color="grey darken-3" dark>
            <div class="d-flex align-center">
                <h2 class="ml-2">Internship Assignment</h2>
            </div>

            <v-spacer></v-spacer>
        </v-app-bar>
        <v-container class="d-flex align-center justify-center" style="height: 900px">
            <v-card
                style="width: 500px; height: 600px"
                class="text-center grey darken-3 white--text pa-10"
            >
                <v-alert v-if="error.length != 0" color="red lighten-2">{{
                    error
                }}</v-alert>
                <h2 class="fw-bold my-6 text-uppercase">Log in</h2>

                <p class="pb-6">Please enter your username and password</p>
                <form>
                    <div class="form-group" :class="{ 'has-error': errors.has('email') }">
                        <label class="control-label" for="email">Email</label>

                        <v-text-field
                            v-model="user.email"
                            v-validate="{
                                required: true,
                                regex: /^[a-zA-Z0-9.!#$%&'*+\/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)+$/,
                            }"
                            name="email"
                            type="text"
                            dark
                            outlined
                        ></v-text-field>

                        <p class="text-danger" v-if="errors.has('email')">
                            {{ errors.first("email") }}
                        </p>
                    </div>
                    <div
                        class="form-group"
                        :class="{ 'has-error': errors.has('password') }"
                    >
                        <label class="control-label" for="password">Password</label>

                        <v-text-field
                            v-model="user.password"
                            v-validate="{
                                required: true,
                                min: 8,
                                regex: /^(?=.*?[A-Z])(?=.*?[a-z])(?=.*?[0-9])(?=.*?[#?!@$%^&*-]).*$/,
                            }"
                            name="password"
                            type="password"
                            dark
                            outlined
                        ></v-text-field>
                        <div class="ma-2" v-if="errors.has('password')">
                            {{ errors.first("password") }}
                        </div>
                    </div>
                </form>

                <v-btn
                    v-if="
                        !errors.any() &&
                        user.password.length != 0 &&
                        user.email.length != 0
                    "
                    @click="onSubmit"
                    class="px-5"
                    >Log in</v-btn
                >

                <v-btn v-else disabled class="px-5 mb-2">Log in</v-btn>
                &nbsp;&nbsp;&nbsp;
                <v-btn @click="onSubmit" class="px-5 mb-2"
                    >Log in with given password</v-btn
                >
                &nbsp;&nbsp;&nbsp;<v-btn @click="resetForm" class="px-5">Reset</v-btn>
            </v-card>
        </v-container>
    </div>
</template>

<script>
import Vue from "vue";
import VeeValidate from "vee-validate";
import axios from "axios";
Vue.use(VeeValidate);
export default {
    name: "LoginView",
    data() {
        return {
            user: {
                password: "",
                email: "",
            },
            formSubmitted: false,
            logIn: true,
            error: "",
        };
    },
    watch: {
        someObject: {
            errors(newQuestion) {
                console.log(newQuestion);
                if (newQuestion.length == 0) {
                    this.logIn = false;
                }
                // Note: `newValue` will be equal to `oldValue` here
                // on nested mutations as long as the object itself
                // hasn't been replaced.
            },
            deep: true,
        },
    },
    methods: {
        async onSubmit() {
            await axios
                .post("http://restapi.adequateshop.com/api/authaccount/login", this.user)
                .then((result) => {
                    if (result.data.code == 0) {
                        this.$router.push("/page");
                    } else {
                        this.error = result.data.message;
                    }
                })
                .catch((err) => {
                    console.log(err.response.data.message);
                });
        },
        resetForm() {
            this.user.email = null;
            this.user.password = null;
        },
    },
};
</script>
