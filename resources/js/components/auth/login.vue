<script setup>
import { reactive, ref } from 'vue'
import {useRouter} from 'vue-router'

const router = useRouter()

let form = reactive({
    email: '',
    password : '',
})

let error = ref('')
const login = async () => {
    await axios.post('/api/login', form)
        .then(response => {
            // console.log(response)
            if (response.data.success) {
                localStorage.setItem('token', response.data.data.token)
                router.push('/admin/home')
            } else {
                error.value = response.data.message;
             }

        })
}
</script>

<template>
    <div class="login">
       <div class="formLogin">
        <p class="text-danger" v-if="error">{{ error }}</p>
         <form @submit.prevent="login">
                <input type="email" placeholder="Enter your Email" v-model="form.email" />
                <br>
                <input type="password" placeholder="Enter your Password" v-model="form.password"/>
                <br>
                <input type="submit" value="Login" class="submit"/>
            </form>
       </div>
    </div>
</template>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Fira sans', sans-serif;
    font-size: 16px;
}

.login{
background: rgb(31, 138, 60);
padding: 0;
margin: 0;
width: 100%;
height: 100vh;
}

.formLogin{
    display: flex;
    align-items: center;
    width: 22.8em;
    height: 55em;
    margin: 0 auto 0 auto;
    overflow: hidden;
}

input{
    background: rgb(240, 132, 132);
    background-position: 0.5em 0.6em;
    border: none;
    color: rgb(255, 255, 255);
    padding: 0 0 0 4rem;
    margin: 0 0 1em 0;
    width: 20em;
    height: 2.8em;
    outline: none;
    transition: background-color 0.4s;

}

input:hover{
background-color: rgb(230, 247, 227);
color: black;
}
input:focus{
background-color: rgb(213, 228, 210);
}

.submit{
color: black;
background: rgb(110, 230, 110);
padding: 0;
margin: 2.4em 0 0 5em;
width: 10em;
text-transform: uppercase;
cursor: pointer;
transition: background-color 0.4s;
}
.submit:hover{
    background: rgb(5, 247, 5);
    color: white;
}
.submit:focus{
    background: rgb(5, 247, 5);
    color: white;
}
.text-danger{
    color: red;
font-size: 16px;
}
</style>
