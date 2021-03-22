<template>
  <div style=" font-size: 15px; width:800px; margin: 0 auto;">
    <img src="@/assets/cmccs.png" alt="" height="auto" width="300px" style="display: block; margin: 0 auto">
      <el-row :gutter="25" style="border: 1px dashed green; padding: 20px;">
        <h2 class="title-form">CMC SOC ACTION</h2>
        <el-col :span="10">
          <label style="color:green; margin-bottom: 15px; display: block; font-family: 'Open Sans', sans-serif;">IP Address:</label>
          <el-input placeholder="VD: 172.24.240.117" v-model="ip"></el-input>
        </el-col>
        <el-col :span="10">
          <label  style="color:green; margin-bottom: 15px; display: block; font-family: 'Open Sans', sans-serif;">Action:</label>
          <div class="form-group">
            <el-select
              v-model="valueAddress"
              placeholder="Action"
          
              size="large"
            >
              <el-option
                v-for="item in Address"
                :key="item.text"
                :value="item.value"
                :label="item.text
                "
              >
              </el-option>
            </el-select>
          </div>
        </el-col>
        <el-col :span="1">
          <br><br>
            <el-button  type="success" round @click="getData()" style="background-color: green; border-color:green;">GET</el-button>    
        </el-col>
      </el-row>
      <el-row :gutter="15"  class="result" v-if="isShow">
        <!-- <label>Result:</label> -->
        <h3>IP : {{ ip }} Action: {{ valueAddress }}</h3>
        <el-col class="jsonText">
          <pre style="word-wrap: break-word; white-space: pre-wrap;">{{ data }}
          </pre>
        </el-col>
      </el-row>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      isShow: false,
      data: null,
      ip: "",
      option: "",
      errors: [],
      url: "",
      valueAddress: "",
      Address: [
        {
          text: "1: Port Admin Status changed.",
          value: "1",
        },
        {
          text: "2: Block MAC address.",
          value: "2",
        },
        {
          text: "3: Block IP address.",
          value: "3",
        },
        {
          text: "4: Reboot device.",
          value: "4",
        },
      ],
    };
  },
  methods: {
    getData(){
      var ipv4format = /^(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/;
      //CHECK this.valueIP có phải là IPV4 không
      if (this.ip.match(ipv4format)){
        if(this.valueAddress != ''){
        // this.type = 'IPV4'
        // console.log(this.valueAddress)
         this.url =
           "http://192.168.10.105:8090/" + this.ip + "/" + this.valueAddress;
          this.fetchData();
        }else{
          alert("Bạn chưa chọn Action")
        }
      } else {
        alert("Bạn nhập 1 địa chỉ IP không hợp lệ!")  
      }
    },

    fetchData() {
      axios
        .get(this.url)
        .then((response) => {
          this.data = response.data;
          this.isShow=true;
        })
        .catch((e) => {
          this.errors.push(e);
        });
      this.url = "";
    },
  },
};
</script>

<style>
.jsonText{
  color: green;
  font-size: 18px;
}
.app{
  background-color: #000;
}
.el-input__inner {
    -webkit-appearance: none;
    background-color: #fff0;
    background-image: none;
    border-radius: 46px;
    border: 1px solid green;
    box-sizing: border-box;
    color: green;
    display: inline-block;
    font-size: inherit;
    height: 35px;
    line-height: 35px;
    outline: 0;
    padding: 0 15px;
    transition: border-color .2s cubic-bezier(.645,.045,.355,1);
    width: 100%;
}
.el-select {
    display: block;
    position: relative;
}
.el-select-dropdown__item{
  color:green;
  font-family: 'Open Sans', sans-serif;
}
.el-select-dropdown{
background-color: rgba(0,0,0,0.9);
border: 1px solid green;
}
.title-form{
  font-size: 20px;
  font-weight: bold;
  color: green;
  text-align: center;
  display: block;
  font-family: 'Open Sans', sans-serif;
  margin-top: 0;
}
.result h3{
  font-size: 16px;
  font-weight: bold;
  color: green;
  font-family: 'Open Sans', sans-serif;
  margin-top: 0;
  text-align: left;
  padding: 20px 0;
}
.el-select-dropdown__item.selected {
  color: green;
}
.el-select .el-input.is-focus .el-input__inner {
    border-color: green;
}
.el-input__inner:focus {
    border-color: green;
}
.el-select .el-input__inner:focus {
    border-color: green;
}
</style>
