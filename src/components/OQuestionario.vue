<template>
  <div class="text-center">
    <div v-if="!started">
      <v-container class="mt-6 pt-6">
        <div class="text-h4 mt-6 pt-6">
          Você já teve aquela ideia em que um aplicativo resolveria o problema? <br>
          Mas a dúvida que não quer calar: quanto custa em média para fazer um aplicativo? 🤔 <br>
        </div>
        <div class="text-h5 mt-15">
          Este questionário foi projetado para ajudar na definição dos requisitos do seu aplicativo, possibilitando uma avaliação mais precisa e personalizada 🚀. <br><br>
          Responda cada pergunta cuidadosamente, escolhendo a opção que melhor se alinha com os objetivos e características desejados para o seu aplicativo. <br><br>
          Ao final do questionário, você receberá uma pontuação total, e o valor correspondente será calculado com base nas suas escolhas. <br><br>
          Sinta-se à vontade para voltar e revisar suas respostas usando os botões <b>"Voltar"</b> e <b>"Próxima Pergunta"</b>.🤓 <br><br><br>
          Agradecemos por utilizar nosso questionário e esperamos contribuir para o sucesso do seu futuro aplicativo! 🌟
        </div>
        <div class="mt-4">
          <a class="text-success text-h4" href="https://wa.me/5549999259394?text=Olá, gostaria de saber quanto custa um aplicativo :)" target="_blank">
            Fale conosco 😀
          </a>
        </div>
      </v-container>
      <v-btn size="x-large" color="primary" class="mt-15" @click="started = true">
        Iniciar 🧐
      </v-btn>
    </div>
    <div v-else>
      <v-container class="mt-15">
        <div v-if="currentQuestionIndex < questions.length">
          <h1 class="text-h3">{{ questions[currentQuestionIndex].text }}</h1>
          <div class="center-radio">
            <v-radio-group v-model="selectedOption" v-for="(option, index) in questions[currentQuestionIndex].options" :key="index" class="mb-5">
              <v-radio color="success" :label="option.text" :value="option.points"></v-radio>
              <!-- :name="`question_${currentQuestionIndex}`" :id="index" -->
            </v-radio-group>
          </div>
          <v-btn class="mr-5" color="text" @click="previousQuestion" :disabled="currentQuestionIndex === 0">Voltar</v-btn>
          <v-btn color="success" size="x-large" @click="nextQuestion">Próxima Pergunta</v-btn>
        </div>
        <div v-else>
          <h1 class="text-h1">O valor do seu aplicativo será de aproximadamente:</h1>
          <h2 class="text-h1 my-15 text-success">
            <b>
              {{ formatCurrency(totalPoints * 22200) }}
            </b>
          </h2>
          <h3 class="text-h6 mb-6">Pontuação: {{ totalPoints }}</h3>
          <div class="my-15">
            <a class="text-success text-h4" href="https://wa.me/5549999259394?text=Olá, gostaria de saber quanto custa um aplicativo :)" target="_blank">
              Dúvidas? Fale conosco 😀
            </a>
          </div>
          <v-btn size="large" color="success" @click="restart">Voltar ao início</v-btn>
        </div>
      </v-container>
    </div>
    <div class="versao">v1.1</div>
    <img class="logo" src="../assets/logo_white.png" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      started: false,
      questions: [
        {
          text: "Que nível de qualidade você está procurando?",
          options: [
            { text: "Ótima qualidade", points: 60 },
            { text: "Boa relação de qualidade e preço", points: 40 },
            { text: "Não estou interessado em muita qualidade", points: 20 }
          ]
        },
        { 
          text: "Qual tipo de aplicativo você precisa?",
          options: [
            { text: "Aplicativo Android", points: 60 },
            { text: "Aplicativo iPhone", points: 80 },
            { text: "Aplicativo Android e iPhone", points: 100 }
          ]
        },
        {
          text: "Qual design você deseja que o seu aplicativo tenha?", 
          options: [
            { text: "Interface simples", points: 40 },
            { text: "Interface personalizada", points: 120 },
            { text: "Interface semelhante à Web", points: 80 },
            { text: "Sem design", points: 20 }
          ]
        },
        { 
          text: "Qual retorno financeiro você deseja ter?",
          options: [
            { text: "Aplicativo gratuito com publicidade", points: 30 },
            { text: "Aplicativo pago", points: 15 },
            { text: "Compras dentro do aplicativo", points: 40 },
            { text: "Outros / Ainda não sei", points: 10 },
            { text: "Não desejo retorno", points: 1 }
          ]
        },
        {
          text: "O seu aplicativo precisa de um sistema de login?",
          options: [
            { text: "Sim, com redes sociais e e-mail", points: 40 },
            { text: "Sim, apenas com e-mail", points: 25 },
            { text: "Não é necessário login", points: 5 },
            { text: "Ainda não sei", points: 20 }
          ]
        },
        {
          text: "Seu aplicativo deve ser integrado a um website?",
          options: [
            { text: "Sim", points: 40 },
            { text: "Não", points: 20 },
            { text: "Ainda não sei", points: 30 }
          ]
        },
        {
          text: "Os usuários devem ter seu perfil próprio?",
          options: [
            { text: "Sim", points: 40 },
            { text: "Não", points: 10 },
            { text: "Ainda não sei", points: 20 }
          ]
        },
        { 
          text: "Seu aplicativo necessita de um painel administrativo?",
          options: [
            { text: "Sim", points: 40 },
            { text: "Não", points: 10 },
            { text: "Ainda não sei", points: 20 }
          ]
        },
        {
          text: "Quantas línguas você deseja que seu aplicativo tenha?",
          options: [
            { text: "Uma única língua", points: 10 },
            { text: "Bilíngue", points: 30 },
            { text: "Multilíngue", points: 50 }
          ]
        },
        {
          text: "Em que estágio se encontra o seu projeto?",
          options: [
            { text: "É apenas um projeto", points: 30 },
            { text: "Esboço já preparado", points: 20 },
            { text: "Aplicativo em desenvolvimento", points: 40 },
            { text: "Aplicativo já está pronto", points: 60 }
          ]
        },
        {
          text: "Em quanto tempo deseja ter o aplicativo pronto?",
          options: [
            { text: "2 meses", points: 45 },
            { text: "5 meses", points: 20 },
            { text: "1 ano", points: 10 },
            { text: "Para ontem", points: 60 },
            { text: "Sem pressa", points: 15 }
          ]
        },
      ],
      currentQuestionIndex: 0,
      selectedOption: null,
      totalPoints: 0,
    }
  },
  methods: {
    restart () {
      this.started = false
      this.currentQuestionIndex = 0
      this.selectedOption = null
      this.totalPoints = 0
    },
    formatCurrency(value) {
      const formattedValue = new Intl.NumberFormat('pt-BR', {
        style: 'currency',
        currency: 'BRL',
      }).format(value / 100)
      return formattedValue
    },
    nextQuestion() {
      if (this.selectedOption !== null) {
        this.totalPoints += this.selectedOption
        this.selectedOption = null
        this.currentQuestionIndex++
      } else {
        alert("Por favor, selecione uma opção antes de prosseguir.")
      }
    },
    previousQuestion() {
      if (this.currentQuestionIndex > 0) {
        this.currentQuestionIndex--
      }
    }
  }
}
</script>

<style lang="scss">
.center-radio {
  align-items: center;
  display: flex;
  flex-direction: column;
  margin: 50px 0 0 0;
  .v-label {
    font-size: 30px;
    @media (max-width: 767px){
      font-size: 20px!important;
    }
  }
}
.versao {
  bottom: 5px;
  font-size: 10px;
  left: 10px;
  position: absolute;
}
.logo {
  bottom: 10px;
  max-width: 200px;
  position: absolute;
  right: 10px;
  @media (max-width: 767px){
    margin-top: 40px;
    position: relative;
    right: inherit;
  }
}
@media (max-width: 767px){
  .text-h1 {
    font-size: 24px!important;
    line-height: normal;
  }
  .text-h3 {
    font-size: 22px!important;
    line-height: normal;
  }
  .text-h4 {
    font-size: 20px!important;
    line-height: normal;
  }
  .text-h5 {
    font-size: 18px!important;
    line-height: normal;
  }
  .pt-6 {
    padding-top: 15px!important;
  }
  .mt-6 {
    margin-top: 15px!important;
  }
  .mt-15 {
    margin-top: 20px!important;
  }
  .mb-5 {
    margin-bottom: 10px!important;
  }
  .mr-5 {
    margin-bottom: 25px;
    margin-right: 0px!important;
    width: 50%;
  }
}
</style>