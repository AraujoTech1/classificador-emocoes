<template>
  <div class="app">
    <h1>Classificador de Emoções</h1>
    <p class="subtitle">Escreva o que você está sentindo no momento ou o que fez hoje</p>

    <textarea v-model="texto" placeholder="Digite aqui..."></textarea>

    <div class="buttons">
      <button @click="analisarTexto">Analisar</button>
      <button class="clear" @click="limparTexto">Limpar</button>
    </div>

    <p v-if="emocao" class="resultado">{{ emojiEmocao }} {{ emocao }}</p>
  </div>
</template>

<script>
import * as tf from '@tensorflow/tfjs';
import * as toxicity from '@tensorflow-models/toxicity';

export default {
  data() {
    return {
      texto: "",
      emocao: "",
      modelo: null,
    };
  },
  computed: {
    emojiEmocao() {
      const emojis = {
        "Alegria": "😃",
        "Tristeza": "😢",
        "Raiva": "😡",
        "Neutro": "😐"
      };
      return emojis[this.emocao] || "";
    }
  },
  methods: {
    async carregarModelo() {
      this.modelo = await toxicity.load(0.9);
    },
    async analisarTexto() {
      if (!this.texto.trim()) {
        this.emocao = ""; // Se o campo estiver vazio, não faz nada
        return;
      }

      try {
        // Simulação de análise com base em palavras-chave (até treinarmos um modelo real)
        const textoLower = this.texto.toLowerCase();
        if (textoLower.includes("feliz") || textoLower.includes("alegre")) {
          this.emocao = "Alegria";
        } else if (textoLower.includes("triste") || textoLower.includes("chorar")) {
          this.emocao = "Tristeza";
        } else if (textoLower.includes("raiva") || textoLower.includes("bravo")) {
          this.emocao = "Raiva";
        } else {
          this.emocao = "Neutro";
        }
      } catch (error) {
        console.error("Erro ao analisar:", error);
        this.emocao = "Erro ao analisar o texto.";
      }
    },
    limparTexto() {
      this.texto = "";
      this.emocao = "";
    }
  },
  mounted() {
    this.carregarModelo();
  }
};
</script>

<style scoped>
/* styles.css */
.app {
  background: #121212;
  color: white;
  text-align: center;
  padding: 20px;
  font-family: Arial, sans-serif;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.subtitle {
  font-size: 14px;
  color: #b0b0b0;
  margin-top: -10px;
  margin-bottom: 15px;
}

textarea {
  width: 300px;
  height: 100px;
  background: #1e1e1e;
  color: white;
  border: none;
  padding: 10px;
  margin: 10px;
  border-radius: 5px;
}

.buttons {
  margin-top: 10px;
}

button {
  background: linear-gradient(45deg, #00c853, #b2ff59);
  border: none;
  padding: 10px 20px;
  color: black;
  font-weight: bold;
  cursor: pointer;
  margin: 5px;
  border-radius: 5px;
}

button.clear {
  background: linear-gradient(45deg, #ff5252, #ff1744);
  color: white;
}

button:hover {
  opacity: 0.8;
}

.resultado {
  margin-top: 15px;
  font-size: 18px;
  font-weight: bold;
}
</style>
