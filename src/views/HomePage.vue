<script>
import InfoTable from "@/components/InfoTable.vue";
import axios from "@/axios";

export default {
  name: "HomePage",
  components: {
    InfoTable,
  },
  data() {
    return {
      msg: [],
    };
  },
  methods: {
    updateInfo() {
      axios.get("/get_info").then((res) => {
        this.msg = res.data;
      });
    },
  },
  beforeMount() {
    this.updateInfo();
    setInterval(this.updateInfo, 2000);
  },
};
</script>

<template>
  <div class="main">
    <template v-for="i in msg" :key="i.id">
      <InfoTable :msg="i" class="item" />
    </template>
  </div>
</template>

<style scoped>
.main {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  padding-bottom: 20px;
}

.item {
  margin-top: 20px;
  width: 80%;
  max-width: 800px;
  min-width: 300px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  padding: 20px;
  transition: all 0.3s;
}
</style>
