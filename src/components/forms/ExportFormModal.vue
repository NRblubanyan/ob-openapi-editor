<template>
  <div id="export-form-container">
    <b-modal id="export-modal" title="Export settings">
      <template v-slot:modal-footer="{ ok, cancel }">
        <b-button
          size="sm"
          variant="primary"
          @click="
            ok();
            exportFile();
            exportModalOpened();
          "
        >
          Save As
        </b-button>
        <b-button
          size="sm"
          variant="danger"
          @click="
            cancel();
            exportModalOpened();
          "
        >
          Cancel
        </b-button>
      </template>

      <b-form>
        <b-form-group
          id="export-form-filename"
          label="File name:*"
          label-for="export-input-filename"
        >
          <b-form-input
            id="export-input-filename"
            v-model="exportForm.filename"
            placeholder="Enter file name"
            required
          >
          </b-form-input>
        </b-form-group>
      </b-form>
      <p v-if="$store.state.currentFile">
        You have
        <strong>{{ $store.state.currentFile.fileName }}</strong> selected, this
        is the file that will be exported
      </p>
      <p>**Will save as .json</p>
    </b-modal>
  </div>
</template>

<script>
export default {
  data() {
    return {
      exportForm: {
        OASDescription: null,
        OASTitle: null,
        OASVersion: null,
        filename: null
      }
    };
  },
  methods: {
    exportFile() {
      for (let property in this.exportForm) {
        if (!this.exportForm[property]) {
          this.exportForm[property] = "left blank";
        }
      }
      let exportObj = {
        type: "exportFile",
        filename: this.exportForm.filename,
        info: {
          description: "Description placeholder",
          title: "Title placeholder",
          version: "Version placeholder"
        }
      };

      this.$store.commit(exportObj);
    },
    exportModalOpened() {
      this.$store.commit("toggleExportModal");
    }
  },
  watch: {
    "$store.state.exportModalOpened"() {
      for (let property in this.exportForm) {
        this.exportForm[property] = null;
      }
    }
  }
};
</script>

<style></style>
