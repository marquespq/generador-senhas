<template>
  <div class="password-generator">
    <div class="container">
      <h2 class="title">Gerador de Senhas</h2>
      <div class="inputs">
        <label for="passwordLength">Comprimento da senha</label>
        <input
          v-model.number="passwordLength"
          id="passwordLength"
          type="number"
          :min="4"
          :max="200"
          class="number-input"
        />
        <label for="includeUppercase">Primeiro caractere maiúsculo</label>
        <select v-model="includeUppercase" id="includeUppercase">
          <option value="true">Sim</option>
          <option value="false">Não</option>
        </select>
        <label for="includeSpecialCharacters">Caracteres especiais</label>
        <select
          v-model="includeSpecialCharacters"
          id="includeSpecialCharacters"
        >
          <option value="true">Sim</option>
          <option value="false">Não</option>
        </select>
      </div>
      <button @click="generatePassword">Gerar Senha </button>
      <div class="password" ref="password" @click="copyPassword">{{ generatedPassword }}  <span class="tooltip">Clique para copiar a senha</span></div>
      <div class="password-strength" v-if="generatedPassword">
        <p>Nível da senha:</p>
        <div
          class="strength-bar"
          :style="{
            width: passwordStrengthPercentage + '%',
            backgroundColor: passwordStrengthColor,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      passwordLength: 8,
      includeUppercase: true,
      includeSpecialCharacters: false,
      generatedPassword: "",
    };
  },

  computed: {
    passwordStrengthPercentage() {
      let strengthPercentage = 0;
      if (this.passwordLength >= 8) {
        strengthPercentage += 33.33;
      }
      if (this.includeUppercase) {
        strengthPercentage += 33.33;
      }
      if (this.includeSpecialCharacters) {
        strengthPercentage += 33.33;
      }
      return strengthPercentage;
    },
    passwordStrengthColor() {
      if (this.generatedPassword.length < 6) {
        return "red";
      } else if (
        this.generatedPassword.length >= 6 &&
        this.generatedPassword.length <= 8
      ) {
        return "orange";
      } else {
        return "green";
      }
    },
  },
  methods: {
    copyPassword() {
      const passwordElement = this.$refs.password;
      const textarea = document.createElement("textarea");
      textarea.value = passwordElement.innerText;
      document.body.appendChild(textarea);
      textarea.select();
      document.execCommand("copy");
      document.body.removeChild(textarea);

      // Exibir notificação de que a senha foi copiada
      this.showNotification("Senha copiada!");
    },
    showNotification(message) {
      // Utilize um componente de notificação ou implemente sua própria lógica para exibir a notificação
      alert(message);
    },
    generatePassword() {
      const lowercaseLetters = "abcdefghijklmnopqrstuvwxyz";
      const uppercaseLetters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
      const specialCharacters = "!@#$%^&*+~|}{[]?><,./-=";

      let validCharacters = lowercaseLetters;
      if (this.includeUppercase) {
        validCharacters += uppercaseLetters;
      }
      if (this.includeUppercase) {
        validCharacters += uppercaseLetters;
      }
      if (this.includeSpecialCharacters) {
        validCharacters += specialCharacters;
      }

      let generatedPassword = "";
      for (let i = 0; i < this.passwordLength; i++) {
        const randomIndex = Math.floor(Math.random() * validCharacters.length);
        generatedPassword += validCharacters[randomIndex];
      }

      this.generatedPassword = generatedPassword;
    },
  },
};
</script>

<style scoped>
.tooltip {
  position: absolute;
  bottom: -20px;
  left: 50%;
  transform: translateX(-50%);
  background-color: #000;
  color: #fff;
  padding: 5px 10px;
  border-radius: 4px;
  opacity: 0;
  transition: opacity 0.3s;
  pointer-events: none;
}

.v-btn:hover .tooltip {
  opacity: 1;
}
.password-generator {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: #f2f2f2;
}

.container {
  width: 400px;
  padding: 20px;
  background-color: #fff;
  border-radius: 8px;
}

.title {
  font-size: 24px;
  transition: color 0.3s;
}

.title:hover {
  color: #4caf50;
}

.inputs {
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 5px;
}

input,
select {
  margin-bottom: 10px;
  width: 100%;
  padding: 10px;
  border-radius: 4px;
  border: 1px solid #ccc;
  outline: none;
}

.number-input {
  box-sizing: border-box;
  /* width: calc(100% - 10px);  */
  width: 100%; /* Subtrai a largura das bordas */
}

button {
  display: block;
  width: 100%;
  padding: 10px;
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 4px;
  font-weight: bold;
  cursor: pointer;
}

.password {
  margin-top: 10px; /* Afastando a senha gerada do próximo elemento */
  margin-bottom: 10px; /* Afastando a senha gerada do próximo elemento */
  font-weight: bold;
  word-wrap: break-word;

}

.password-strength {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

.strength-bar {
  height: 10px;
  background-color: #4caf50;
}
</style>
