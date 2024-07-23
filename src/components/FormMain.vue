<template>
    <div class="container">
        <div class="row mt-5">
            <div class="col-4">
                <div class="mb-3">
                    <input type="text" class="form-control" id="exampleFormControlInput1" placeholder="id instance" v-model="idInstance">
                </div>
                <div class="mb-3">
                    <input type="text" class="form-control" id="exampleFormControlInput1" placeholder="api token instance" v-model="apiToken">
                </div>
                <div class="mb-3">
                    <button type="button" class="btn btn-primary  w-100" @click="getSettings">Get settings</button>
                </div>
                <div class="mb-3">
                    <button type="button" class="btn btn-primary  w-100" @click = "getStateInstance">Get state instance</button>
                </div>
                <div class="mb-3">
                    <input type="text" class="form-control" id="exampleFormControlInput1" placeholder="phone" v-model="phone">
                </div>
                <div class="mb-3">
                    <textarea class="form-control" id="exampleFormControlTextarea1" rows="3" placeholder="message" v-model="message"></textarea>
                </div>
                <div class="mb-3">
                    <button type="button" class="btn btn-primary  w-100" @click="sendMessage">Send message</button>
                </div>
                <div class="mb-3">
                    <input type="text" class="form-control" id="exampleFormControlInput1" placeholder="phone" v-model="phone2">
                </div>
                <div class="mb-3">
                    <input type="text" class="form-control" id="exampleFormControlInput1" placeholder="file url" v-model="fileUrl">
                </div>
                <div class="mb-3">
                    <button type="button" class="btn btn-primary  w-100" @click="sendfile">Send file by url</button>
                </div>
            </div>
            <div class="col-8">
                <div class="mb-3">
                    <label for="exampleFormControlTextarea1" class="form-label">Ответ</label>
                    <textarea class="form-control" id="exampleFormControlTextarea1" rows="20" v-model = "response"></textarea>
                </div>
            </div>
        </div>
    </div>
</template> 

<script setup>
import {ref } from "vue";
import axios from "axios";

const idInstance = ref("");
const apiToken = ref("");
const response = ref("");
const phone = ref("");
const message = ref("");
const phone2 = ref("");
const fileUrl = ref("");
const url = process.env.VUE_APP_GREEN_API_URL

const greenApiGet = (api) => {
    return axios.get(url + '/waInstance' + idInstance.value + '/' + api + '/' + apiToken.value)    
}

const greenApiPost = (api, params) => {
    return axios.post(url + '/waInstance' + idInstance.value + '/' + api + '/' + apiToken.value, params)    
}

const getSettings = () => {
    greenApiGet('getSettings').then((resp) => {
        response.value = JSON.stringify(resp.data)
    })       
}

const getStateInstance = () => {
    greenApiGet('getStateInstance').then((resp) => {
        response.value = JSON.stringify(resp.data)
    })       
}

const sendMessage = () => {
    let params = {
        "chatId": phone.value + "@c.us",
        "message": message.value
    }

    greenApiPost('sendMessage', params).then((resp) => {
        response.value = JSON.stringify(resp.data)
    })       
}

const sendfile = () => {
    let params = {
        "chatId": phone2.value + "@c.us",
        "urlFile": fileUrl.value,
        "fileName": fileUrl.value.split('/').pop(),
    }

    greenApiPost('sendFileByUrl', params).then((resp) => {
        response.value = JSON.stringify(resp.data)
    })       
}
</script>


