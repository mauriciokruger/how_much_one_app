<template>
  <div class="text-center">
    <div v-if="!started">
      <v-container class="mt-6 pt-6 text-left">
        <div class="text-h4 mt-6">
          Você já teve aquela ideia em que um aplicativo resolveria o problema? <br>
          Mas a dúvida que não quer calar: quanto custa em média para fazer um aplicativo? 🤔 <br>
        </div>
        <div class="text-h5 mt-10">
          Este questionário foi projetado para ajudar na definição dos requisitos do seu aplicativo, possibilitando uma avaliação mais precisa e personalizada 🚀. <br><br>
          Responda cada pergunta cuidadosamente, escolhendo a opção que melhor se alinha com os objetivos e características desejados para o seu aplicativo. <br><br>
          <!-- Ao final do questionário, você receberá uma pontuação total, e o valor correspondente será calculado com base nas suas escolhas. <br><br> -->
          Sinta-se à vontade para voltar e revisar suas respostas usando os botões <b>"Voltar"</b> e <b>"Próxima Pergunta"</b>.🤓 <br><br>
          Agradecemos por utilizar nosso questionário e esperamos contribuir para o sucesso do seu futuro aplicativo! 🌟
        </div>
        <div class="mt-4">
          <a class="text-success text-h4" href="https://wa.me/5549999259394?text=Olá, gostaria de saber quanto custa um aplicativo :)" target="_blank">
            Fale conosco 😀
          </a>
        </div>
      </v-container>
      <div class="text-center">
        <v-btn size="x-large" color="primary" class="mt-10 mb-3" @click="started = true">
          Iniciar 🧐
        </v-btn>
      </div>
    </div>
    <div v-else>
      <v-container class="mt-15">
        <div v-if="currentQuestionIndex < questions.length">
          <h1 class="text-h4 text-center text-md-left">{{ questions[currentQuestionIndex].text }}</h1>
          <div class="center-radio">
            <v-radio-group hide-details v-model="selectedOption" v-for="(option, index) in questions[currentQuestionIndex].options" :key="index" class="mb-5">
              <v-radio color="success" :label="option.text" :value="option.points"></v-radio>
              <!-- :name="`question_${currentQuestionIndex}`" :id="index" -->
            </v-radio-group>
          </div>
          <v-btn class="mr-5" color="text" @click="previousQuestion" :disabled="currentQuestionIndex === 0">Voltar</v-btn>
          <v-btn color="success" size="x-large" @click="nextQuestion">Próxima Pergunta</v-btn>
          <div class="duvidas text-left mt-15">
            <a class="text-white" href="https://wa.me/5549999259394?text=Olá, gostaria de saber quanto custa um aplicativo :)" target="_blank">
              Dúvidas sobre o que responder? <br>
              Chame nossa equipe aqui
            </a>
          </div>
        </div>
        <div v-else>
          <h1 class="text-h2">O valor do seu aplicativo será de aproximadamente</h1>
          <h2 class="text-h1 my-15 text-success">
            <b>
              {{ formatCurrency(totalPoints * 22200) }}
            </b>
          </h2>
          <h3 class="text-h6 mb-6">Pontuação: {{ totalPoints }}</h3>
          <div class="my-15">
            <a class="text-success text-h3" href="https://wa.me/5549999259394?text=Olá, gostaria de saber quanto custa um aplicativo :)" target="_blank">
              Vamos iniciar os trabalhos? 😀
            </a>
          </div>
          <v-btn size="small" color="success" @click="restart">Voltar ao início</v-btn>
        </div>
      </v-container>
    </div>
    <div class="bg" :style="'background-image: url('+ prev +')'"></div>
    <div class="versao">v 1.2</div>
  </div>
</template>
<script setup>
  import prev from '@/assets/bg.png'
</script>
<script>
export default {
  data() {
    return {
      started: false,
      questions: [
        {
          text: "Que nível de qualidade você está procurando?",
          options: [
            { text: "Ótima qualidade, certamente.", points: 60 },
            { text: "Boa qualidade mas com preço mais acessível.", points: 40 },
            { text: "Não preciso de muita qualidade.", points: 20 }
          ]
        },
        { 
          text: "Qual tipo de aplicativo vocês está buscando?",
          options: [
            { text: "Para celulares com Android, Ex: Samsung, Xiaomi…", points: 60 },
            { text: "Para celulares da Apple.", points: 80 },
            { text: "Para ambos.", points: 100 }
          ]
        },
        {
          text: "Qual é o estilo de design que você gostaria que seu aplicativo apresentasse?", 
          options: [
            { text: "Mais simples.", points: 40 },
            { text: "Personalizada, quero algo exclusivo.", points: 120 },
            { text: "Que pareça com um site que já vi.", points: 80 },
            { text: "Sem preferência para isso.", points: 20 }
          ]
        },
        { 
          text: "Deseja ganhar dinheiro com o aplicativo?",
          options: [
            { text: "Sim, vou por publicidade nele.", points: 30 },
            { text: "Sim, vou cobrar para as pessoas baixarem ele.", points: 15 },
            { text: "Sim, vou vender produtos no aplicativo.", points: 40 },
            { text: "Sim, mas ainda não sei como.", points: 10 },
            { text: "Não tenho objetivo de lucrar com ele.", points: 1 }
          ]
        },
        {
          text: "As pessoas precisam fazer um cadastro para usar ele?",
          options: [
            { text: "Sim, com redes sociais e e-mail.", points: 40 },
            { text: "Sim, apenas e-mail.", points: 25 },
            { text: "Não é necessário.", points: 5 },
            { text: "Não sei ainda se vai ter cadastro.", points: 20 }
          ]
        },
        {
          text: "O aplicativo vai precisar buscar informações de algum site ou sistema automaticamente?",
          options: [
            { text: "Sim, quero integrar com meu site.", points: 40 },
            { text: "Sim, quero integrar com meu sistema.", points: 60 },
            { text: "Não precisa.", points: 10 },
            { text: "Não sei se será necessário.", points: 20 }
          ]
        },
        {
          text: "As pessoas que usarem seu aplicativo, podem ter um perfil personalizado para elas?",
          options: [
            { text: "Sim, tipo uma rede social.", points: 60 },
            { text: "Sim, algo mais simples com dados de contato.", points: 40 },
            { text: "Não será necessário.", points: 10 },
            { text: "Não sei ainda.", points: 20 }
          ]
        },
        { 
          text: "As informações que terá no seu aplicativo, serão fixas ou você deseja mudar com o tempo?",
          options: [
            { text: "Desejo alterar a qualquer momento, publicar notícias, produtos e afins.", points: 40 },
            { text: "Podem ser todas fixas, não será necessário alterar.", points: 10 },
            { text: "Não sei ainda.", points: 20 }
          ]
        },
        {
          text: "Seu aplicativo será apenas em português?",
          options: [
            { text: "Sim, apenas português.", points: 10 },
            { text: "Não, terá mais um idioma.", points: 40 },
            { text: "Não, terá vários idiomas.", points: 50 },
            { text: "Não sei ainda.", points: 20 }
          ]
        },
        {
          text: "Qual o nível de informações que você já tem do projeto/aplicativo?",
          options: [
            { text: "Tenho apenas a ideia.", points: 30 },
            { text: "Tenho um rascunho abrangente do aplicativo.", points: 20 },
            { text: "Já iniciei o desenvolvimento dele.", points: 40 },
            { text: "Tenho ele pronto, preciso fazer manutenção apenas.", points: 60 }
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
  align-items: start;
  display: flex;
  flex-direction: column;
  margin: 50px 0 0 0;
  .v-label {
    font-size: 30px;
    @media (max-width: 767px){
      font-size: 18px!important;
      text-align: left;
    }
  }
}
.versao {
  bottom: 5px;
  font-size: 10px;
  left: 10px;
  position: absolute;
}
.bg {
  background-size: cover;
  height: 100%;
  position: absolute;
  right: 0;
  top: 0;
  width: 480px;
  z-index: 5;
  @media (max-width: 767px){
    display: none;
  }
}
.v-container {
  position: relative;
  z-index: 10;
}
.duvidas {
  a {
    text-decoration: none;
  }
}
.text-h1 {
  font-weight: 900!important;
}
@media (max-width: 767px){
  .text-h1 {
    font-size: 24px!important;
    line-height: normal;
  }
  .text-h2 {
    font-size: 23px!important;
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
  .mt-10 {
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

@media (min-width: 1920px) {
  .v-container {
    max-width: 1200px;
  }
}

</style>