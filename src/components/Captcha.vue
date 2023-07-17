<template>
    <div>
      <canvas ref="canvas" width="200" height="50"></canvas>
      <br>
      <input type="text" v-model="userInput" placeholder="Ingresa el captcha">

      <button @click="verificarTexto" class="bg-yellow-500 hover:bg-yellow-600 text-black font-bold py-2 px-4 rounded-xl">
        Iniciar Sesión
      </button>
  
      <!-- Modal de respuesta -->
      <div class="modal" v-if="showModal">
        <div class="modal-content">
          <span class="close" @click="cerrarModal">&times;</span>
          <p id="respuestaTexto">{{ respuestaTexto }}</p>
        </div>
      </div>
    </div>
  </template>
  
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
        var textoX = canvas.width / 2; // Posición x centrada
        var textoY = canvas.height / 2; // Posición y centrada
  
        contexto.clearRect(0, 0, canvas.width, canvas.height);
        contexto.fillStyle = '#681400'; // Establecer el fondo rojo
        contexto.fillRect(0, 0, canvas.width, canvas.height);
  
        contexto.font = '30px Lucida Handwriting';
        contexto.textAlign = 'center';
        contexto.textBaseline = 'middle';
        contexto.fillStyle = 'white'; // Establecer letras blancas
  
        // Dibujar cada letra individualmente con transformaciones aleatorias
        for (var i = 0; i < textoAleatorio.length; i++) {
          contexto.save();
  
          // Aplicar transformaciones aleatorias
          var rotation = Math.random() * 0.7 - 0.5; // Rotación aleatoria entre -0.1 y 0.1 radianes
          var scaleX = Math.random() * 0.4 + 0.8; // Escalado aleatorio entre 0.8 y 1.2 en el eje x
          var scaleY = Math.random() * 0.4 + 0.8; // Escalado aleatorio entre 0.8 y 1.2 en el eje y
  
          var letterX = textoX + (i - Math.floor(textoAleatorio.length / 2)) * 24; // Posición x de la letra
          var letterY = textoY; // Posición y de la letra
  
          contexto.translate(letterX, letterY);
          contexto.rotate(rotation);
          contexto.scale(scaleX, scaleY);
  
          // Dibujar la letra
          contexto.fillText(textoAleatorio[i], 0, 0);
  
          contexto.restore();
        }
  
        // Almacenar el texto aleatorio en la propiedad captcha
        this.captcha = textoAleatorio;
  
        // Dibujar dos curvas por encima del texto
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
  
        // Mostrar el modal de respuesta
        this.showModal = true;
      },
      cerrarModal() {
        // Ocultar el modal de respuesta
        this.showModal = false;
  
        // Recargar la página
        location.reload();
      }
    }
  };
  </script>
  
  <style scoped>
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
  