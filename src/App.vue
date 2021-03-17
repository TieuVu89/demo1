<template>
  <div>
    <el-row :gutter="15">
      <el-col :span="3">
        <el-input placeholder="IP" v-model="ip"></el-input>
      </el-col>
      <el-col :span="3">
        <div class="form-group">
          <el-select
            v-model="valueAddress"
            placeholder="Address"
            @change="currentSelectAddress"
          >
            <el-option
              v-for="item in Address"
              :key="item.value"
              :value="item.value"
              :label="
                (valueAddress === item.value ? 'Adress: ' : '') + item.text
              "
            >
            </el-option>
          </el-select>
        </div>
      </el-col>
       <el-col :span="5">
           <el-button type="info" @click="fetchData()">GET</el-button>    
       </el-col>
    </el-row>
    <pre style="word-wrap: break-word; white-space: pre-wrap;">{{ data }}</pre>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      data: null,
      ip: "",
      option: "",
      errors: [],
      url: "",
      valueAddress: "",
      Address: [
        {
          text: "1",
          value: "1",
        },
        {
          text: "2",
          value: "2",
        },
        {
          text: "3",
          value: "3",
        },
        {
          text: "4",
          value: "4",
        },
      ],
    };
  },
  methods: {
    fetchData() {
      this.currentSelectAddress()
      axios
        .get(this.url)
        .then((response) => {
          this.data = response.data;
        })
        .catch((e) => {
          this.errors.push(e);
        });
      this.url = "";
    },
    currentSelectAddress() {
      if (this.ip !== "") {
        this.url =
          "http://192.168.10.105:8090/" + this.ip + "/" + this.valueAddress;
      } else {
        alert("vui lòng nhập địa chỉ ip");
      }
    },
  },
};
</script>

<style>
</style>
