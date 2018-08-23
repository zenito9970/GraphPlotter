<template>
  <div>
    <v-btn icon @click.stop="selectFile">
      <v-icon>note_add</v-icon>
    </v-btn>
    <input ref="selectFileBtn" type="file" accept=".csv" style="display:none" @change="onSelected">
  </div>
</template>

<script>
export default {
  name: 'FileSelector',
  methods: {
    selectFile() {
      this.$refs.selectFileBtn.click();
    },
    onSelected(e) {
      this.$emit('fileSelected', e.target.files);
    }
  }
}
</script>

