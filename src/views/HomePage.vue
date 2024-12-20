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
      filter: {},
      filter_list: {},
      r_msg: [],
    };
  },
  methods: {
    updateInfo() {
      axios.get("/get_info").then((res) => {
        this.msg = res.data;
        // 获取所有可能的location筛选条件
        this.filter_list = { location: [] };
        for (let i of this.msg) {
          if (!this.filter_list.location.includes(i.location)) {
            this.filter_list.location.push(i.location);
          }
        }
      });
    },
    handleClick(type, key) {
      this.filter[type] = key;
      this.r_msg = this.msg.filter((i) => {
        return (
          (this.filter.location === undefined ||
            i.location === this.filter.location) &&
          (this.filter.type === undefined || i.type === this.filter.type)
        );
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
    <div class="title">
      <h1>预警信息</h1>
    </div>
    <div class="info">
      <!-- 筛选 -->
      <div>
        <a-dropdown>
          <a-button> {{ filter.location || "地点" }} </a-button>
          <template #overlay>
            <a-menu @click="handleClick('location', $event.key)">
              <a-menu-item
                v-for="i in filter_list.location"
                :key="i"
                :value="i"
                >{{ i }}</a-menu-item
              >
            </a-menu>
          </template>
        </a-dropdown>
      </div>
      <template v-for="i in msg" :key="i.id">
        <InfoTable :msg="i" class="item" />
      </template>
    </div>
  </div>
</template>

<style scoped>
.main {
  position: relative;
  width: 100%;
}

.info {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-bottom: 20px;
  position: relative;
  top: 60px;
}

.title {
  line-height: 60px;
  height: 60px;
  z-index: 2;
  font-size: 1.3em;
  font-weight: bold;
  position: fixed;
  background-color: #e3e3e3;
  width: 100%;
  padding-left: 30px;
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
