<template src="./template.html"></template>

<script>
import printTemplate from "./template.js";

export default {
  name: "v-print",
  props: {
    open: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      windowRef: null,
      opened: this.open,
    };
  },
  mounted() {
    if (this.opened) {
      this.openPortal();
    }
  },
  beforeDestroy() {
    if (this.windowRef) {
      this.closePortal();
    }
  },
  watch: {
    open: function(value) {
      if(value){
        this.openPortal();
        this.opened = value;
      }
    },
  },
  methods: {
    openPortal() {
      this.$nextTick(() => this.openWindow());
    },
    openWindow(){
      this.windowRef = window.open("", "", "width=0,height=0");
      this.windowRef.document.write(printTemplate);
      this.windowRef.document.body.appendChild(this.$el);
      this.windowRef.print();
      this.windowRef.addEventListener("afterprint", this.closePortal());
    },
    closePortal() {
      if (this.windowRef) {
        this.windowRef.close();
        this.windowRef = null;
        this.opened = false;
        this.$emit("close");
      }
    }
  }
}
</script>
