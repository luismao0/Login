<script>
export default {
    data() {
      return {
        userInput: '',
        captcha: '',
        respuestaTexto: '',
        showModal: false
      };
    },
    mounted() {
      this.dibujarTextoEnCanvas();
    },
    methods: {
      generarTextoAleatorio() {
        var caracteres = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
        var textoAleatorio = '';
  
        for (var i = 0; i < 5; i++) {
          var indiceAleatorio = Math.floor(Math.random() * caracteres.length);
          textoAleatorio += caracteres.charAt(indiceAleatorio);
        }
  
        return textoAleatorio;
      },
      dibujarTextoEnCanvas() {
        var canvas = this.$refs.canvas;
        var contexto = canvas.getContext('2d');
  
        var textoAleatorio = this.generarTextoAleatorio();
        var textoX = canvas.width / 2; 
        var textoY = canvas.height / 2; 
  
        contexto.clearRect(0, 0, canvas.width, canvas.height);
        contexto.fillStyle = '#681400'; 
        contexto.fillRect(0, 0, canvas.width, canvas.height);
  
        contexto.font = '30px Lucida Handwriting';
        contexto.textAlign = 'center';
        contexto.textBaseline = 'middle';
        contexto.fillStyle = 'white'; 
  
        for (var i = 0; i < textoAleatorio.length; i++) {
          contexto.save();
  
          // Aplicar transformaciones aleatorias
          var rotation = Math.random() * 0.7 - 0.5; 
          var scaleX = Math.random() * 0.4 + 0.8; 
          var scaleY = Math.random() * 0.4 + 0.8; 
  
          var letterX = textoX + (i - Math.floor(textoAleatorio.length / 2)) * 24; 
          var letterY = textoY; 
  
          contexto.translate(letterX, letterY);
          contexto.rotate(rotation);
          contexto.scale(scaleX, scaleY);
  
          contexto.fillText(textoAleatorio[i], 0, 0);
  
          contexto.restore();
        }
  
        this.captcha = textoAleatorio;
  
        this.dibujarCurva(0, textoY, canvas.width, textoY, 2, 'black');
        this.dibujarCurva(0, textoY + 5, canvas.width, textoY + 5, 2, 'black');
      },
      dibujarCurva(startX, startY, endX, endY, lineWidth, strokeStyle) {
        var canvas = this.$refs.canvas;
        var contexto = canvas.getContext('2d');
  
        contexto.beginPath();
        contexto.moveTo(startX, startY);
        contexto.quadraticCurveTo(Math.random() * canvas.width, Math.random() * canvas.height, endX, endY);
        contexto.lineWidth = lineWidth;
        contexto.strokeStyle = strokeStyle;
        contexto.stroke();
      },
      verificarTexto() {
        if (this.userInput === this.captcha) {
          console.log("¡Correcto");
          this.respuestaTexto = '¡Correcto! El texto coincide.';
        } else {
          console.log("¡INCorrecto");
          this.respuestaTexto = 'El texto no coincide. Intenta de nuevo.';
        }
  
        this.showModal = true;
      },
      cerrarModal() {
        this.showModal = false;
  
        location.reload();
      }
    }
  };
</script>

<template>
  <div class="container flex items-center justify-center">
    <h1 class="text-3xl text-yellow-500 font-pt-sans mt-32">Iniciar Sesión</h1>
  </div>
  <div class="entradas mt-12 space-y-5">
    <div class="codigo relative flex items-center justify-center">
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" class="w-7 h-7 absolute mr-64 text-yellow-500">
        <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 6a3.75 3.75 0 11-7.5 0 3.75 3.75 0 017.5 0zM4.501 20.118a7.5 7.5 0 0114.998 0A17.933 17.933 0 0112 21.75c-2.676 0-5.216-.584-7.499-1.632z" />
      </svg>
      <input id="codigo" type="text" placeholder="Código" class="pl-12 pr-16 py-2 bg-transparent rounded-xl border-2 border-white-300 focus:border-yellow-500 focus:outline-none text-white font-semibold">
    </div>
    <div class="contrasenia relative flex items-center justify-center">
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-7 h-7 absolute mr-64 text-yellow-500">
        <path stroke-linecap="round" stroke-linejoin="round" d="M16.5 10.5V6.75a4.5 4.5 0 10-9 0v3.75m-.75 11.25h10.5a2.25 2.25 0 002.25-2.25v-6.75a2.25 2.25 0 00-2.25-2.25H6.75a2.25 2.25 0 00-2.25 2.25v6.75a2.25 2.25 0 002.25 2.25z" />
      </svg>
      <input id="contrasenia" type="password" placeholder="Contraseña" class="pl-12 pr-16 py-2 bg-transparent rounded-xl border-2 border-white-300 focus:border-yellow-500 focus:outline-none text-white font-semibold">
    </div>
    <div class="captcha relative flex items-center justify-center">
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-7 h-7 absolute mr-64 text-yellow-500">
        <path stroke-linecap="round" stroke-linejoin="round" d="M9 12.75L11.25 15 15 9.75m-3-7.036A11.959 11.959 0 013.598 6 11.99 11.99 0 003 9.749c0 5.592 3.824 10.29 9 11.623 5.176-1.332 9-6.03 9-11.622 0-1.31-.21-2.571-.598-3.751h-.152c-3.196 0-6.1-1.248-8.25-3.285z" />
      </svg>
      <input id="captcha" type="text" v-model="userInput" placeholder="Captcha" class="pl-12 pr-16 py-2 bg-transparent rounded-xl border-2 border-white-300 focus:border-yellow-500 focus:outline-none text-white font-semibold">
    </div>

    <div class="flex flex-col items-center justify-center">
      <canvas class = "pt-4 pb-4" ref="canvas" width="200" height="50"></canvas>
      <br>

      <button @click="verificarTexto" class="w-80 bg-yellow-500 hover:bg-yellow-600 text-black font-bold py-2 px-4 rounded-xl">
        Iniciar Sesión
      </button>
    </div>

    <!-- Modal de respuesta -->
    <div class="modal" v-if="showModal">
      <div class="modal-content">
        <span class="close" @click="cerrarModal">&times;</span>
        <p id="respuestaTexto">{{ respuestaTexto }}</p>
      </div>
    </div>

  </div>
</template>


<style>
@import url('https://fonts.googleapis.com/css2?family=PT+Sans&display=swap');

.font-pt-sans {
  font-family: 'PT Sans', sans-serif;
  font-size: 2rem;
}


canvas {
    border: none;
  }
  
  .modal {
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgba(0, 0, 0, 0.4);
  }
  
  .modal-content {
    background-color: #fefefe;
    margin: 15% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 80%;
  }
  
  .close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
    cursor: pointer;
  }
  
  .close:hover,
  .close:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
  }

</style>
