<template src="./template.html"></template>

<script>
export default {
  name: "v-print-slot",
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
      this.windowRef.document.write(`<!DOCTYPE html><html><head>
      <style>
      *{color:#fff;}

      @media print{
        body {-webkit-print-color-adjust: exact;}

        @page {size: landscape;}
        *{color:#000}

        .header{
          background-color: #a0d8ea;
          color: #08486c;
          font-weight: bold;
          padding: 8px;

        }
        .header-principal {
          width:100%;
          line-height:36px;
          font-size: 16px;
          margin-bottom: 8px;
          text-align: center;
          text-transform: uppercase;
          color: #1c6594;
          border: 1px solid #1c6594;
          box-sizing:border-box;
        }
        .info-box {
          display: flex;
          flex-flow: row;
          flex-wrap: wrap;
          justify-content: space-between;
          color:#343a40;
          padding:8px 0;
        }
        .info-item{
          flex: 1 0 33%;
          padding: 0 8px;
        }
        .info-subtitle{
          border-bottom: 1px solid #08486c;
          width: 100%;
          padding: 8px;
        }
        .f-100{
          flex: 0 0 100%;
        }
        .f-50{
          flex: 1 0 50%;
        }
      </style></head>
      <body></body></html>`);

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
