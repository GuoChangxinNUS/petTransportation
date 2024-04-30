<script setup>
import axios from "axios";
import { HmacSHA1, enc } from "crypto-js";
import { reactive, ref, onMounted, watch } from "vue";
// 替换为你自己的阿里云物联网平台 Access Key 和 Access Secret
const accessKey = "";
const accessSecret = "";
//StartTime为当前时间前一小时，EndTime为当前时间
var EndTime = new Date().getTime();
var StartTime = EndTime - 3600000;
var requestParams = {
  //请求参数
  Action: "QueryDevicePropertyData",
  // 其他接口参数...
  IotInstanceId: "",
  ProductKey: "",
  DeviceName: "",
  StartTime: StartTime,
  EndTime: EndTime,
  Asc: 1,
  PageSize: 50,
  Identifier: "",
};
const sendRequest = async (params) => {
  //发送历史数据请求
  const requestParams = {
    ...params,
    Format: "JSON",
    Version: "2018-01-20",
    AccessKeyId: accessKey,
    SignatureMethod: "HMAC-SHA1",
    SignatureVersion: "1.0",
    SignatureNonce: generateRandomString(),
    Timestamp: new Date().toISOString(),
    RegionId: "cn-shanghai",
  };
  const signedParams = signRequest(requestParams, accessSecret);

  const requestPath = buildRequestPath(signedParams);
  const url = `https://iot.cn-shanghai.aliyuncs.com/${requestPath}`;

  try {
    const response = await axios.get(url);
    return response.data;
  } catch (error) {
    throw error;
  }
};
const generateRandomString = () => {
  //生成随机字符串
  return Math.random().toString(36).substr(2);
};
const signRequest = (params, accessSecret) => {
  //拼接签名
  const paramList = Object.keys(params)
    .sort()
    .map(
      (key) => `${encodeURIComponent(key)}=${encodeURIComponent(params[key])}`
    );
  const stringToSign = `GET&%2F&${encodeURIComponent(paramList.join("&"))}`;
  const signature = HmacSHA1(stringToSign, `${accessSecret}&`).toString(
    enc.Base64
  );
  return {
    ...params,
    Signature: signature,
  };
};
const buildRequestPath = (params) => {
  //构建请求路径
  const queryList = [];
  for (const key in params) {
    if (params.hasOwnProperty(key)) {
      queryList.push(`${key}=${encodeURIComponent(params[key])}`);
    }
  }
  return `?${queryList.join("&")}`;
};
const convertTo24HourFormat = (time) => {
  // 将毫秒值时间戳转换为24进制时间补足两位
  const date = new Date(time);
  const hour = date.getHours();
  const minute = date.getMinutes();
  const second = date.getSeconds();
  return `${hour < 10 ? "0" + hour : hour}:${
    minute < 10 ? "0" + minute : minute
  }:${second < 10 ? "0" + second : second}`;
};
const sendRequestAndChangeData = async (requestParams) => {
  //发送请求异步函数,递归调用，直到NextValid为false
  const response = await sendRequest(requestParams);
  console.log(response.Data);
};
onMounted(() => {
  sendRequestAndChangeData(requestParams);
});
</script>