<template>
    <v-app style="background: #ebdef0 !important; font-family: Helvetica Neue"
        ><body>
            <v-main>
                <router-view></router-view>
            </v-main>
        </body>
    </v-app>
</template>

<script>
export default {
    name: "App",
    data: () => ({
        //
    }),
};
</script>
