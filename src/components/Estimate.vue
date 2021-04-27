<template>
  <v-container>
    <v-row class="text-center">
      <v-col class="mt-12 mb-12">
        <h1 class="display-3 font-weight-bold mt-12 mb-3">
          ファイルを推論する
        </h1>
      </v-col>
    </v-row>
    <v-col
      class="mb-5 mt-12"
      cols="12"
    >
      <v-file-input
        show-size
        counter
        accept="text/csv"
        label="File input"
        @change="getFileContent"
      />

      <v-btn
        class="mt-4 mb-4"
        x-large
        color="primary"
        :loading="processing"
        :desabled="processing"
        @click="upload_estimate_server"
        block
      >
       submit
      </v-btn>
      ※2~3分程度かかる可能性があります
    </v-col>

  </v-container>
</template>

<script>
  export default {
    name: 'Estimate',
    data (){
      return {
        content:"None",
        processing:false,
        filename:"None"
      }
    },
mounted() {
        this.axios.get("https://0b23852dc7fe.ngrok.io/wakeup_test"
        ).then(() => {
        }).catch(() => {
            alert("back end server is off-line \n please contact @Hasshi7965");
        });
    },
    methods: {
      getFileContent (file) {
        try {
          this.content = file
          this.filename = file.name
        } catch (e) {
          alert(e)
        }
      },
      upload_estimate_server(){
        this.processing=true
        let formData = new FormData();
        formData.append('test_csv', this.content);                
        let config = {
                    headers: {
                        'content-type': 'multipart/form-data'
                    }
                };
        this.axios.post('https://0b23852dc7fe.ngrok.io/estimate', formData, config)
          .then((response) => {
            let blob = new Blob([response.data], { type: 'text/csv' })
            let link = document.createElement('a')
            link.href = window.URL.createObjectURL(blob)
            link.download = "estimate_" + this.filename
            link.click()
          })
          .catch((e) => {
            alert(e);
          })
          .finally(() => {
            this.processing=false
          });
      }
    }
  }
</script>
