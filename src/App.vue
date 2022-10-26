<template>
  <div>
    <el-button type="primary" @click="visible = !visible">open/offf</el-button>
    <FormDialogVue v-model="visible" />
  </div>
</template>

<script>
import FormDialogVue from './views/FormDialog.vue';

export default {
  components: { FormDialogVue },
  data() {
    return {
      visible: false,
    };
  },
};
</script>
