
<template>
    <div>
        <h1>Song Key Detection</h1>
        <loading :active.sync="isLoading"
            :can-cancel="false"
            :is-full-page="fullPage"
        ></loading>
        
        <label v-if="isLoading">Analyzing...</label>

        <vue-dropzone ref="files" id="dropzone" :options="dropzoneOptions" @vdropzone-complete="afterComplete">
        </vue-dropzone>
        
        <div id="cards" v-for="(item, index) in resultArray" :key="index">
            <Result :result="item"/>
        </div>
    </div>
</template>

<style scoped>
    .resultcard div{
        display: flex;
        justify-content: space-between
    }
</style>

<script>
import axios from "axios";
import Result from './Result.vue';
import vue2Dropzone from 'vue2-dropzone'
import Loading from 'vue-loading-overlay';

import 'vue2-dropzone/dist/vue2Dropzone.min.css'
import 'vue-loading-overlay/dist/vue-loading.css';

export default {
  name: 'upload',
  components: {
    vueDropzone: vue2Dropzone,
    Loading,
    Result
  },
  data: function () {
    return {
        isLoading:false,
        fullPage: true,

        iscomplete:false,
        resultArray: [],
        dropzoneOptions: {
            url: 'http://127.0.0.1:5000/api/upload',
            method: "post",
            thumbnailWidth: 150,
            maxFilesize: 50,
            acceptedFiles: ".mp3",
            headers: { "My-Awesome-Header": "header value" }
        }
    }
  },
  methods: {
    async afterComplete(file) {
        this.isLoading = true;
        await axios.get('http://127.0.0.1:5000/api/get_analysis').then(res => {          
            if (res.status === 200) {
                this.isLoading = false;
                this.iscomplete = true;
                const result = {
                    bpm: '',
                    scale: '',
                    key: '',
                };
                result.bpm = res.data.bpm;
                result.scale = res.data.scale;
                result.key = res.data.key;
                this.resultArray.push(result);
            }
        });
    }
  }
};
</script>
