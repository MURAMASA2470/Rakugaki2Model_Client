<template>
    <v-layout row v-if="models">
        <v-flex xs6 class="box">
            <v-img :src="'./output/images/'+models[index]['file']+'.jpg'" width="500"></v-img>
        </v-flex>
        <v-flex xs6 class="box">
            <Viwer :modelName="'./output/models/'+models[index]['file']+'.obj'"></Viwer>
        </v-flex>
    </v-layout>
</template>

<style>
    .box {
        width: 500px;
    }
</style>

<script>
import Viwer from "~/components/viewer.vue"
import axios from 'axios'

export default {
    data() {
        return {
            selectedModel: null,
            index: null,
            models: null
        }
    },
    mounted() {
        this.index = 0
        axios.get('/api/').then(
        (models) => {
            this.models = models.data.reverse()
            console.log(this.models)
        }
        ).catch((e) => {
        console.log(e)
        })
    },
    methods: {

    },
    components: {
        Viwer
    }
}
</script>