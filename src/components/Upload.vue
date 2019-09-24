
<template>
    <div>
    <h1>Song Key Detection</h1>

    <form @submit.prevent="sendFile" enctype="multipart/form-data">
        <div v-if="message"
            :class="`message ${error ? 'is-danger' : 'is-success'}`"
        >
            <div class="message-body">{{message}}</div>
        </div>

        <div class="field">
            <div class="file is-boxed is-primary">
                <label class="file-label">
                    <input
                        multiple
                        type="file"
                        ref="files"
                        @change="selectFile"
                        class="file-input"
                    />
                    <span class="file-cta">
                        <span class="file-icon"><i class="fa fa-upload"></i></span>
                        <span>Choose a file...</span>
                    </span>
                    <span v-if="file" class="file-name">{{file.name}}</span>
                </label>
            </div>
        </div>

        <div class="field">
            <div v-for="(file, index) in files" :key="index" 
                :class="`level ${file.invalidMessage && 'has-textdanger'}`"
            >
                        <h1>abc</h1>
                <div class="level-left">
                    <div class="level-item">
                        {{file.name}}
                        <span v-if="file.invalidMessage">&nbsp;- {{file.invalidMessage}}</span>
                    </div>
                </div>
                <div class="level-right">
                    <div class="level-item">
                        <a @click.prevent="files.splice(index,1); uploadFiles.splice(index, 1)" class="delete"></a>
                    </div>
                </div>
            </div>
        </div>

        <div class="field">
            <button class="button is-info">Send</button>
        </div>
    </form>
    
    </div>    
</template>

<script>
import axios from "axios";
import _ from 'lodash';

export default {
  name: "Upload",
  data() {
      return {
          files: [],
          uploadFiles: [],
          message: "",
          error: false
      };
  },
  methods: {
      selectFile() {
          const files = this.$refs.files.files;
          this.files = [
              ...this.files,
              ..._.map(files, file => ({
                  name: file.name,
                  size: file.size,
                  type: file.type,
                  invalidMessage: this.validdate(files)
              }))
          ]
      },

      validdate(file) {

          const MAX_SIZE = 200000;
          const allowedTypes = ["image/mp3", "image/wav"];

          if (file.size > MAX_SIZE) {
              return `Max Size: ${MAX_SIZE/1000} kb`;
          }

          if (!allowedTypes.includes(file.type)) {
              return "Not an audio";
          }

          return "";
      },

      async sendFile() {
          const formData = new FormData();
          _.forEach(this.uploadFiles, file => {
              if (this.validdate(file) == "") {
                  formData.append('files', file);
              }
          });

          try {
              await axios.post('/upload', formData);
              this.message = "File has been uploaded";
              this.files = [];
              this.uploadFiles = [];
          } catch (err) {
              this.message = err.response.data.error;
              this.error = true;
          }
      },
  }
};
</script>