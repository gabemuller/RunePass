<template>
  <div class="container">
    <h1>RunePass</h1>
    <div class="checkmarks">
      <div>
        <input
          v-model="options.Maiusculo"
          class="check"
          type="checkbox"
          id="Upper-Case"
          @change="updateLength"
        />
        <label for="Upper-Case">Letras Maiúsculas (A - Z)</label>
      </div>
      <div>
        <input
          v-model="options.Minusculo"
          class="check"
          type="checkbox"
          id="Lower-Case"
          @change="updateLength"
        />
        <label for="Lower-Case">Letras Minúsculas (a - z)</label>
      </div>
      <div>
        <input
          v-model="options.Digitos"
          class="check"
          type="checkbox"
          id="Digitos"
          @change="updateLength"
        />
        <label for="Digitos">Dígitos (0 - 9)</label>
      </div>
      <div>
        <input
          v-model="options.Simbolos"
          class="check"
          type="checkbox"
          id="Simbolos"
          @change="updateLength"
        />
        <label for="Simbolos">Símbolos (@, %, /, #, & etc...)</label>
      </div>
    </div>
    <div class="comprimento">
      <label for="Comprimento">Comprimento: </label>
      <div class="custom-number">
        <button
          @click="diminuirLength"
          @mousedown="startDecrement"
          @mouseup="stopInterval"
          @mouseleave="stopInterval"
        >
          −
        </button>
        <input
          type="number"
          v-model.number="options.length"
          id="Comprimento"
          min="1"
          max="99"
        />
        <button
          @click="aumentarLength"
          @mousedown="startIncrement"
          @mouseup="stopInterval"
          @mouseleave="stopInterval"
        >
          +
        </button>
      </div>
    </div>
    <button class="pushable" @click="generatePassword">
      <span class="front">Gerar Senha</span>
    </button>
    <div id="passwordResult" class="password-result" v-if="password">
      <i class="fa-solid fa-copy copy-icon" @click="copyPassword"></i>
      <span>{{ password }}</span>
    </div>
  </div>
  <footer class="footer">
    <a href="https://github.com/gabemuller" target="_blank">
      <i class="fa-brands fa-github"></i>
    </a>
    <a
      href="https://www.linkedin.com/in/gabriel-matusiak-642332211/"
      target="_blank"
    >
      <i class="fa-brands fa-linkedin"></i>
    </a>
  </footer>
</template>

<script>
export default {
  data() {
    return {
      options: {
        Maiusculo: false,
        Minusculo: false,
        Digitos: false,
        Simbolos: false,
        length: 0,
      },
      password: "",
      errorMessage: "",
      interval: null,
    };
  },
  watch: {
    "options.length": function (newLength) {
      if (newLength < 1) this.options.length = 1;
      else if (newLength > 99) this.options.length = 99;
    },
  },
  methods: {
    aumentarLength() {
      if (this.options.length < 99) {
        this.options.length++;
      }
    },
    diminuirLength() {
      if (this.options.length > 1) {
        this.options.length--;
      }
    },
    startIncrement() {
      this.interval = setInterval(() => {
        if (this.options.length < 99) this.options.length++;
      }, 100);
    },
    startDecrement() {
      this.interval = setInterval(() => {
        if (this.options.length > 1) this.options.length--;
      }, 100);
    },
    stopInterval() {
      clearInterval(this.interval);
    },
    updateLength(event) {
      if (event.target.checked) this.options.length++;
      else if (this.options.length > 1) this.options.length--;
    },
    generatePassword() {
      const { Maiusculo, Minusculo, Digitos, Simbolos, length } = this.options;
      const caractMaiusculo = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
      const caractMinusculo = "abcdefghijklmnopqrstuvwxyz";
      const caractDigitos = "0123456789";
      const caractSimbolos = "@%/#!&*?$()_-,;|{}][><";

      let todosCaract = "";
      if (Maiusculo) todosCaract += caractMaiusculo;
      if (Minusculo) todosCaract += caractMinusculo;
      if (Digitos) todosCaract += caractDigitos;
      if (Simbolos) todosCaract += caractSimbolos;

      this.password = Array.from({ length }, () =>
        todosCaract.charAt(Math.floor(Math.random() * todosCaract.length))
      ).join("");
    },
    copyPassword() {
      if (!this.password) return;
      navigator.clipboard
        .writeText(this.password)
        .then(() => {
          alert("Senha copiada!");
        })
        .catch((err) => console.error("Erro ao copiar:", err));
    },
  },
};
</script>

<style scoped>
@import "@/assets/styles.css";
</style>
