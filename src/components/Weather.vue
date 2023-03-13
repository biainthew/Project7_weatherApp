<template>
    <div class="container">
        <div class="content">
            <v-btn
                v-if="show"
                color="red-darken-4"
                elevation="2"
                @click="closeBox()"
            >
                Cancel
            </v-btn>
            <v-btn
                v-else
                color="indigo-darken-4"
                elevation="2"
                @click="openBox()"
            >
                Start
            </v-btn>
            <div class="input-city">
                <v-text-field
                    v-if="inputing"
                    shaped
                    outlined
                    clearable
                    variant="solo"
                    class="custom-label-color"
                    label="Input your city"
                    placeholder="Enter your city"
                    ref="weatherInput"
                    @click:append="clearText()"
                    append-inner-icon="mdi-magnify"
                    @keyup.enter="getAPI()"
                    v-model="inputText"
                >
                </v-text-field>
            </div>
            <WeatherInfo :dataAPI="dataAPI" :inputText="inputText"/>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    import WeatherInfo from './WeatherInfo.vue'
    export default {
        name: 'Weather',
        components: { WeatherInfo },
        data(){
            return {
                show: false,
                inputing: false,
                inputText: '',
                key: "c4e7b399b485192946342267e9f1dfa9",
                rules: {
                    name: [val => (val || '').length > 0 || 'This field is required']
                },
                dataAPI: null
            };
        },
        methods: {
            closeBox(){
                this.inputing = false
                this.show = !this.show
            },
            openBox(){
                this.inputing = true
                this.show = !this.show
            },
            clearText(){
                this.inputText = ''
                this.$refs.weatherInput.focus()
            },
            showToastMessage(){
                const app = document.getElementById('app')
                const toast = document.createElement('div')
                toast.classList.add('toast')
                toast.style.animation = ''
                toast.innerHTML = `
                    <img class="toast-icon" src="https://img.icons8.com/external-sbts2018-outline-color-sbts2018/58/000000/external-alert-emergencies-set-sbts2018-outline-color-sbts2018.png"/>
                    <div class="toast-message">
                        <h3>Danger</h3>
                        <h5>Please check the city name</h5>
                    </div>
                `
                app.appendChild(toast)
                
                setTimeout(()=>{
                    app.removeChild(toast)
                },3000)
            },
            getAPI(){
                if(this.inputText == ''){
                    this.showToastMessage()
                    this.$refs.weatherInput.focus();
                }else{
                    try{
                        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.inputText}&appid=48d2a880b7b07ffbd9b975451c25b12a&units=metric`)
                        .then(response => {
                            console.log(response)
                            this.dataAPI = response.data;
                            this.inputText = ''
                        })
                        .catch(() => {
                            this.showToastMessage()
                            this.inputText = ''
                        })
                    }
                    catch(error){
                        if(error.response){
                            console.log(error.response.data);
                            console.log(error.response.status);
                            console.log(error.response.headers);
                        } else if (error.request) {
                            console.log(error.request);
                        }
                        else {
                            console.log('Error', error.message);
                        }
                    }
                }
            }
        }
    }
</script>

<style>
    .down-arrow{
        fill: #fff;
    }
    .custom-label-color input {
        color: #000000 !important;
    }
    .input-city {
        margin-top: 50px;
        text-align: center;
    }
    .container {
        margin: 0 auto;
        position: relative;
    }
    .content {
        margin: 0 auto;
        width: 700px;
        text-align: center;
    }

    .toast{
        background: #ff4949;
        position: absolute;
        top: 20px;
        right: 10px;
        border-radius: 15px;
        height: 60px;
        width: 250px;
        display: flex;
        align-items: center;
        animation: slideIn ease 1.5s, fadeOut linear 1s 3s;
        box-shadow: -1px 2px 10px 0px rgba(0, 0, 0, .75);
    }
    .toast-message{
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        color: #fff;
    }
    .toast-icon{
        height: 30px;
        padding: 0 10px;
    }
    .v-field__append-inner{
        cursor: pointer;
    }

    @keyframes slideIn {
        from {
            right: -40px;
            opacity: 0;
        }
        to {
            right: 10px;
            opacity: 1;
        }
    }
    @keyframes fadeOut {
        to{
            opacity: 0;
        }
    }
</style>