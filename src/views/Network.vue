<template>
  <v-container>
    <v-row>
      <v-col>
        <v-col
          ><v-select
            :items="itemsInput"
            @change="selectedInput"
            label="Input"
          ></v-select
        ></v-col>
      </v-col>
      <v-col
        ><v-select
          v-model="selectOutput"
          :items="itemsOutput"
          label="Output"
          multiple
          chips
        ></v-select
      ></v-col>
    </v-row>
    <v-file-input
      accept=".csv"
      v-model="file"
      color="deep-purple accent-4"
      label="File input"     
      placeholder="Select your files"
      prepend-icon="mdi-paperclip"
      outlined
      :show-size="1000"
      @change="selectedFile"
    >
      <template v-slot:selection="{ index, text }">
        <v-chip v-if="index < 2" color="deep-purple accent-4" dark label small>
          {{ text }}
        </v-chip>

        <span
          v-else-if="index === 2"
          class="text-overline grey--text text--darken-3 mx-2"
        >
          +{{ file.length - 2 }} File(s)
        </span>
      </template>
    </v-file-input>
    <v-row class="d-flex justify-center mb-6">
      <v-btn
        class="ma-2"
        :loading="loading"
        :disabled="loading"
        color="secondary"
        @click="networkAnalysis"
      >
        Start Analysis
      </v-btn>
    </v-row>
  </v-container>
</template>
<script>
export default {
  name: "Nework",
  data() {
    return {
      itemsOutput: ["miRNA", "piRNA", "circRNA"],
      itemsInput: ["Gene", "miRNA", "piRNA", "circRNA"],
      selectOutput: [],
      selectInput: [],
      file: null,
      loading: false,
    };
  },
  methods: {
    selectedInput(item) {
      var index = this.itemsOutput.indexOf(item);
      if (index !== -1) {
        this.itemsOutput.splice(index, 1);
      }
    },
    networkAnalysis() {
      this.loading = true;
    },

    selectedFile() {
      if (!this.file) {
        this.data = "No File Chosen";
      }
      var reader = new FileReader();
      // Use the javascript reader object to load the contents
      // of the file in the v-model prop
      reader.readAsText(this.file);
      reader.onload = () => {
        let nodeFinderList =  reader.result.split(',');
        console.log(nodeFinderList)

      };
      
    },
  },
};
</script>