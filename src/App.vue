<script setup>
import { ref } from 'vue';
const numero = ref(4);
const senha = ref("");
const forca = ref(4);
const msgForca = ref("senha fraca");
const numeros = ref(false);
const maiusculas = ref(false);
const especiais = ref(false);
const color = ref('orangered');

const algarismos = ['1', '2', '3', '4', '5', '6', '7', '8', '9', '0'];
const letrasMin = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o',
'p', 'q', 'r', 's', 't','u', 'v', 'w', 'x', 'y', 'z'];
const letrasMai = letrasMin.map((el) => {
  return el.toUpperCase();
})
const caracteresEsp = ['!', '@', '#', '$', '%', '&','-', '_', '+', '=', '|', '/', '?', ';', ':' ];


function gerarForca() {
  forca.value = numero.value;
  if(numeros.value) {
    forca.value++;
  }
  if (maiusculas.value) {
    forca.value++;
  }
  if(especiais.value) {
    forca.value++;
  }

  if(forca.value <= 7) {
      msgForca.value = "senha fraca";
      color.value = 'orangered';
  } else if(forca.value <= 14) {
    msgForca.value = "senha moderada";
    color.value = 'yellow';
  } else {
    msgForca.value = "senha forte";
    color.value = '#00ff82';
  }
}


function plusNumero() {
    if (numero.value < 30) {
      numero.value++;
      gerarForca();
    }

}
function minusNumero() {
  if(numero.value > 4) {
    numero.value--;
    gerarForca();
  }
}

function shuffleString(string) {
  let charArray = string.split('');
  for (let i = charArray.length - 1; i > 0; i--) {
    let j = Math.floor(Math.random() * (i + 1));
    let temp = charArray[i];
    charArray[i] = charArray[j];
    charArray[j] = temp;
  }
  let shuffledString = charArray.join('');
  return shuffledString;
}

function gerarSenha() {
  let senhaTemp = "";
  let i = 0;
  while(true) {
    let num = Math.floor(Math.random() * letrasMin.length);
    let char = letrasMin[num];
    senhaTemp += char;
    i++;
    if(i == numero.value) break;

    if(numeros.value) {
      let num = Math.floor(Math.random() * algarismos.length);
      let char = algarismos[num];
      senhaTemp += char;
      i++;
      if(i == numero.value) break;
    }
    if (maiusculas.value) {
      let num = Math.floor(Math.random() * letrasMai.length);
      let char = letrasMai[num];
      senhaTemp += char;
      i++;
      if(i == numero.value) break;
    }
    if(especiais.value) {
      let num = Math.floor(Math.random() * caracteresEsp.length);
      let char = caracteresEsp[num];
      senhaTemp += char;
      i++;
      if(i == numero.value) break;
    }
  }
  const novaSenha = shuffleString(senhaTemp);
  senha.value = novaSenha;
}

function copiarSenha() {
  navigator.clipboard.writeText(senha.value);
}
</script>

<template>
  <main class="card">
    <h1 class="titulo">Gerador de Senha</h1>
    <hr>
    <h3>Quantos Caracteres?</h3>
    <p class="escolha">*escolha entre 4 e 30 caracteres</p>
    <div class="display">
      <button @click="plusNumero"  class="mudanca">+</button>
      <div class="numero"><p>{{ numero }}</p></div>
      <button @click="minusNumero" class="mudanca">-</button>
    </div>
    <div class="input">
      <input type="checkbox" name="numeros" v-model="numeros" @change="gerarForca">
      <label for="numero">Números</label>
    </div>
    <div class="input">
      <input type="checkbox" name="maiusculas" v-model="maiusculas" @change="gerarForca">
      <label for="maiusculas">Letras Maiúsculas</label>
    </div>
    <div class="input">
      <input type="checkbox" name="especiais" v-model="especiais" @change="gerarForca">
      <label for="especiais">Caracteres Especiais</label>
    </div>
    <p class="status" :style="{color: `${color}`}">{{ msgForca }}</p>
    <div class="status-bar" :style="{backgroundColor: `${color}`}"></div>
    <button class="gerar" @click="gerarSenha">Gerar Senha</button>
    <div class="resultado">
      <p class="senha">{{ senha }}</p>
      <button class="copiar" @click="copiarSenha">Copiar</button>
    </div>
    <p class="autor">Feito por Lucas Herlon com VueJs</p>
  </main>
</template>

<style scoped>
.card {
  min-height: 600px;
  min-width: 400px;
  background-color: var(--preto-massa);
  padding: 20px;
  border-radius: 12px;
  box-shadow: 3px 3px 3px rgba(0, 0, 0, 0.5);
}

.titulo {
  color: var(--verde-foda);
  text-align: center;
  margin: 15px;
}

h3 {
    color: var(--branco-estiloso);
    margin: 20px;
}

.numero {
    color: var(--branco-estiloso);
}

.escolha {
  margin-left: 20px;
  margin-top: -10px;
  margin-bottom: 18px;
}
.display {
    min-width: 200px;
    display: flex;
    justify-content: space-around;
}
.mudanca {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    font-size: 40px;
    font-weight: 600;
    display: flex;
    align-items: center;
    justify-content: center;
    border-style: none;
    background-color: var(--verde-foda);
    color: var(--preto-massa);
}

.mudanca:hover {
  cursor: pointer;
  transform: scale(1.1);
  transition: transform 0.5s ease;
}

p {
  color: var(--branco-estiloso);
}

.numero > p {
    font-size: 40px;
}

.input {
  width: 300px;
  margin: 15px;
  color: var(--branco-estiloso);
}

label {
  margin: 20px;
}

.status {
  text-align: right;
  margin-right: 25px;
}
.status-bar {
  width: 350px;
  height: 17px;
  background-color: var(--branco-estiloso);
  border-radius: 17px;
  margin: 10px;
}

.gerar {
  width: 350px;
  height: 30px;
  border-style: none;
  margin-left: 10px;
  margin-bottom: 25px;
  background-color: var(--verde-foda);
  color: var(--preto-massa);
  font-size: 18px;
  font-weight: 600;
}

.gerar:hover {
  cursor: pointer;
  transform: scale(1.05);
  transition: transform 0.5s ease;
}
.resultado {
  width: 350px;
  height: 40px;
  background-color: var(--branco-estiloso);
  margin-left: 10px;
  padding: 10px;
  display: flex;
  justify-content: flex-end;
}

.copiar {
  height: 40px;
  width: 60px;
  background-color: var(--verde-foda);
  color: var(--preto-massa);
  border-style: none;
  margin-top: -10px;
  margin-right: -10px;
  margin-left: 10px;
  font-size: 18px;
}

.copiar:hover {
  cursor: pointer;
  transform: scale(1.05);
  transition: transform 0.5s ease;
}

.copiar:active {
  color: var(--branco-estiloso);
}

.senha {
  color: var(--preto-massa);
  text-align: right;
  font-size: 16px;
}

input[type="checkbox"] {
  width: 100px;
  transform: scale(1.5);
}
input[type="checkbox"]:hover {
  background-color: var(--verde-foda);
}

.autor {
  margin-top: 10px;
  margin-left: 10px;
}
</style>
