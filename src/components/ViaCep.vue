<template>
    <div class="w-full flex flex-col items-center">
        <h1 class="my-10 font-bold text-2xl text-green-500">API ViaCep</h1>
        <div class="w-[300px] mt-2 flex justify-between">
            <input class="w-full border px-1" type="text" name="cep" id="cep" v-model="cep">
            <button class="ml-2 border px-2 bg-blue-600 text-white" @click="search">Buscar</button>
        </div>

        <div v-if="ok" class="w-[300px] mt-5 border p-2">
            <div v-if="uf !== ''">
                <span class="font-bold">Estado: </span> {{ uf }}
            </div>

            <div v-if="localidade !== ''">
                <span class="font-bold">Cidade: </span> {{ localidade }}
            </div>

            <div v-if="logradouro !== ''">
                <span class="font-bold">Endereço: </span> {{ logradouro }}
            </div>

            <div v-if="complemento !== ''">
                <span class="font-bold">Complemento: </span> {{ complemento }}
            </div>

            <div v-if="bairro !== ''">
                <span class="font-bold">Bairro: </span> {{ bairro }}
            </div>
        </div>

        <div v-if="msgError !== ''" class="w-[300px] mt-5 p-1 bg-red-200 border border-red-400 text-red-800">
            {{ msgError }}
        </div>
    </div>
</template>

<script setup>
import axios from 'axios'
import { ref } from 'vue'

const cep = ref('')
const localidade = ref('')
const logradouro = ref('')
const complemento = ref('')
const bairro = ref('')
const uf = ref('')
const ok = ref(false)
const msgError = ref('')

function search() {
    axios
    .get(`https://viacep.com.br/ws/${cep.value}/json/`)
    .then(response => {
        logradouro.value = response.data.logradouro
        localidade.value = response.data.localidade
        uf.value = response.data.uf
        complemento.value = response.data.complemento
        bairro.value = response.data.bairro
        cep.value = response.data.cep

        ok.value = true
        msgError.value = ''
    })
    .catch(error => {
        ok.value = false
        msgError.value = error.message
    })
}
</script>