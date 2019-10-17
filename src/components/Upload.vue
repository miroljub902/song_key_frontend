
<template>
    <div>
        <loading :active.sync="isLoading"
            :can-cancel="false"
            :is-full-page="fullPage"
        ></loading>
        
        <label v-if="isLoading">Analyzing...</label>

        <vue-dropzone style="margin-left: 150px; margin-right: 150px" ref="files" id="dropzone" :options="dropzoneOptions" @vdropzone-file-added="fileAdded" @vdropzone-complete="afterComplete">
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
        files: [],
        isLoading:false,
        fullPage: true,

        iscomplete:false,
        resultArray: [],
        dropzoneOptions: {
            url: 'http://ec2-18-191-230-117.us-east-2.compute.amazonaws.com:5000/api/upload',
            method: "post",
            paramName: "files",
            acceptedFiles: ".mp3",
            thumbnailWidth: 150,
            maxFilesize: 50,
            maxFile: 5,
            dictDefaultMessage: "<i class='fa fa-cloud-upload'></i>Drag and Drop audio files here to analyze your music",
        }
    }
  },
  methods: {
    fileAdded(file) {
        console.log(file)
        this.files.push(file)
    },
    async afterComplete(file) {
        this.isLoading = true;
        await axios.get('http://ec2-18-191-230-117.us-east-2.compute.amazonaws.com:5000/api/get_analysis').then(res => {          
            if (res.status === 200) {
                this.isLoading = false;
                this.iscomplete = true;
                const result = {
                    bpm: '',
                    scale: '',
                    key: '',
                };
                result.name = this.files[0].name
                result.bpm = res.data.bpm;
                result.scale = res.data.scale;
                result.key = res.data.key;
                
                this.resultArray.push(result);
                console.log(this.resultArray)
            }
        });
    }
  }
};
</script>
