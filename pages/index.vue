<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card flat tile class="pa-6">
        <v-form v-model="validData" @submit.prevent="onSubmit">
            <v-card-text>
                <file-input label="Subir archivo:" filetype=".csv,.xls,.xlsx" validated v-model="file" title="Subir archivo o arrastrarlo aquí" icon="paperclip" />
            </v-card-text>
            <v-card-actions class="pa-2">
                <v-spacer />
                <v-row>
                    <v-col class="py-0">
                          <v-progress-linear v-show="loading" v-model="progress" height="25" reactive>
                            <strong>{{ Math.ceil(progress) }}%</strong>
                        </v-progress-linear>
                    </v-col>
                    <v-col class="py-0">
                        <v-btn tile depressed :disabled="!validData" color="primary" type="submit" >Subir</v-btn>
                    </v-col>
                </v-row>                        
            </v-card-actions>
        </v-form>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>


export default {
  data () {
      return {
          file: null,
          validData: true,
          progress: null,
          loading: false
      }
  },
  methods: {
    onSubmit(evt) {
      let data = new FormData()

      data.append('file', this.file)

      this.$axios.post('upload',data, { onUploadProgress: (progressEvent) => { this.progress = Math.round((progressEvent.loaded * 100) / progressEvent.total)}}).then(
        result => {
          console.log(result);
          this.$toast.success('Se subió el archivo correctamente')
          this.file = null;
        },
        error => {
          this.$toast.error('Ocurrió un error, para más información presione F12')
          console.log(error);
      });
    }
  }
}
</script>
