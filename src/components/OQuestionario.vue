<template>
  <div class="text- text-md-left">
    <div v-if="!started">
      <v-container class="mt-15 text-left">
        <div>
          <a href="https://d3t.com.br/" target="_blank">
            <img class="logo" src="../assets/logo_white.png" />
          </a>
        </div>
        <div class="text-h5 font-thin mt-6">
          Você já teve <b>aquela ideia</b> em que um <b>aplicativo</b> resolveria o problema? <br><br>
          Mas a dúvida que não quer calar: <b>quanto custa</b> em média para fazer um aplicativo? 🤔 <br>
        </div>
        <v-row class="mt-md-15">
          <v-col cols="12" md="4">
            <v-btn size="x-large" color="success" block class="mt-10 mb-3 button-large" @click="started = true">
              Iniciar <v-icon>mdi-chevron-right</v-icon><v-icon class="button-large-icon">mdi-chevron-right</v-icon>
            </v-btn>
          </v-col>
        </v-row>
      </v-container>
    </div>
    <div v-else>
      <v-container class="mt-15">
        <div class="text-md-left text-center">
          <a href="https://d3t.com.br/" target="_blank">
            <img class="logo" src="../assets/logo_white.png" />
          </a>
        </div>
        <div v-if="currentQuestionIndex < questions.length">
          <h1 class="text-h4 text-center text-md-left">{{ questions[currentQuestionIndex].text }}</h1>
          <div class="center-radio">
            <v-radio-group hide-details v-model="selectedOption" v-for="(option, index) in questions[currentQuestionIndex].options" :key="index" class="mb-5">
              <v-radio color="success" :label="option.text" :value="option.points"></v-radio>
              <!-- :name="`question_${currentQuestionIndex}`" :id="index" -->
            </v-radio-group>
          </div>
          <v-row align="center" class="mt-6">
            <v-col cols="12" md="2">
              <v-btn color="text" @click="previousQuestion" :disabled="currentQuestionIndex === 0">Voltar</v-btn>
            </v-col>
            <v-col cols="12" md="4">
              <v-btn size="x-large" color="success" block class="button-large" @click="nextQuestion">
                Próxima <v-icon>mdi-chevron-right</v-icon><v-icon class="button-large-icon">mdi-chevron-right</v-icon>
              </v-btn>
            </v-col>
          </v-row>
          <div class="duvidas text-left mt-15">
            <a class="text-white" href="https://bit.ly/3Tt54iH" target="_blank">
              Dúvidas sobre o que responder? <br>
              Chame nossa equipe aqui
            </a>
          </div>
        </div>
        <div v-else class="mw-div">
          <div class="text-center">
            <div v-if="enviarForm">
              <h1 class="text-h4 font-thin">Preencha o formulário abaixo para saber qual <br> será o valor do seu aplicativo 😀</h1>
              <v-row align="center" justify="center" class="mt-6">
                <v-col cols="12" md="4">
                  <v-form ref="form" @submit.prevent="submit" v-model="validaForm">
                    <v-text-field 
                      autofocus 
                      label="Nome*"
                      variant="outlined"
                      v-model="form.name"
                      :rules="[rules.required]"
                      :disabled="salvando"
                      :loading="salvando"
                    ></v-text-field>
                    <v-text-field 
                      label="E-mail*"
                      variant="outlined"
                      v-model="form.email"
                      :rules="[rules.required, rules.email]"
                      :disabled="salvando"
                      :loading="salvando"
                      type="email"
                    ></v-text-field>
                    <div class="d-none">
                      <input
                        v-model="form.phone"
                        v-maska
                        data-maska="['(##) ####-####', '(##) #####-####']"
                      >
                    </div>
                    <v-text-field 
                      :rules="[rules.required]"
                      :disabled="salvando"
                      :loading="salvando"
                      v-model="form.phone"
                      variant="outlined"
                      label="Telefone*">
                    </v-text-field>
                    <!--  prepend-icon="mdi-send" -->
                    <v-btn :loading="salvando" type="submit" size="large" color="success" block class="mt-2">Ver valor</v-btn>
                  </v-form>
                </v-col>
              </v-row>
            </div>
            <div v-else>
              <h1 class="text-h4 font-thin">O valor do seu aplicativo será de <b>aproximadamente</b></h1>
              <h2 class="text-h1 mt-md-15 mt-10 text-success">
                <b>
                  {{ formatCurrency(userResponses.reduce((total, response) => total + response.value, 0) * 22200) }}
                </b>
              </h2>
              <div class="text-center font-thin">
                Pontuação: <b>{{ userResponses.reduce((total, response) => total + response.value, 0) }}</b>
              </div>
            </div>
            <v-row align="center" class="mt-10 mt-md-15" justify="center">
              <v-col cols="12" md="2">
                <v-btn color="text" @click="restart">Iniciar novamente</v-btn>
              </v-col>
              <v-col cols="12" md="4">
                <a class="text-success text-h3" href="https://bit.ly/3Tt54iH" target="_blank">
                  <v-btn size="x-large" color="success" block class="button-large" @click="nextQuestion">
                    Falar com a D3T <v-icon>mdi-chevron-right</v-icon><v-icon class="button-large-icon">mdi-chevron-right</v-icon>
                  </v-btn>
                </a>
              </v-col>
            </v-row>
          </div>
          <div class="mt-10 mt-md-15">
            <div class="text-h6 font-thin mb-4">
              <b>
                Como chegamos neste valor?
              </b>
              <br>
              O valor do aplicativo é baseado em uma estimativa conforme as respostas que selecionou no formulário, cada resposta tem um grau de complexibilidade. Esta complexibilidade é denifida com base em diversos projetos que a D3T já criou.
            </div>
            <div class="text-h6 font-thin mb-4">
              <b>
                Este valor pode mudar?
              </b>
              <br>
              Certamente, este valor é apenas uma estimativa do projeto, mas em convesa com nossa equipe haverá detalhamento do projeto, onde o valor poderá mudar para mais ou para menos.
            </div>
            <div class="text-h6 font-thin mb-4">
              <b>
                O que está incluído no valor total?
              </b>
              <br>
              Horas da nossa equipe de desenvolvimento. Valores extras devem ser analisados, como: Disponibilidade na loja, servidores de hospedagem, manutenção...
            </div>
            <a class="link_none" href="https://bit.ly/3Tt54iH" target="_blank">
              <div class="text-h6 font-thin mb-4">
                <b>
                  Chame nossa equipe, lhe ajudamos a entender seu projeto e tirar a ideia do papel :)
                </b>
              </div>
            </a>
          </div>
        </div>
      </v-container>
    </div>
    <v-snackbar
      v-model="sendAlert.show"
      :timeout="sendAlert.time"
    >
      {{sendAlert.text}}
      <template v-slot:actions>
        <v-btn
          :color="sendAlert.color"
          variant="elevated"
          @click="sendAlert.show = false"
        >
          Fechar
        </v-btn>
      </template>
    </v-snackbar>
    <div class="bg" :style="'background-image: url('+ prev +')'"></div>
    <div class="versao">v 1.3</div>
  </div>
</template>
<script setup>
  import { vMaska } from "maska"
  import prev from '@/assets/bg.png'
</script>
<script>
import axios from 'axios'
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
      salvando: false,
      form: {
        name: '',
        email: '',
        phone: ''
      },
      sendAlert: {
        show: false,
        time: '',
        text: '',
        color: ''
      },
      currentQuestionIndex: 0,
      selectedOption: null,
      totalPoints: 0,
      validaForm: false,
      enviarForm: true,
      userResponses: [],
      rules: {
        required: v => !!v || 'Obrigatório.',
        email: v => !v || /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'Digite um e-mail.'
      }
    }
  },
  methods: {
    restart () {
      this.started = false
      this.currentQuestionIndex = 0
      this.selectedOption = null
      this.totalPoints = 0
      this.enviarForm = true
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
        this.userResponses.push({
          question: this.questions[this.currentQuestionIndex].text,
          value: this.selectedOption,
        })
        // this.userResponses[this.userResponses.length - 1].totalPoints = this.totalPoints
        this.selectedOption = null
        this.currentQuestionIndex++
      } else {
        alert("Por favor, selecione uma opção antes de prosseguir.")
      }
    },
    previousQuestion() {
      if (this.currentQuestionIndex > 0) {
        this.userResponses.pop()
        this.currentQuestionIndex--
      }
    },
    async submit () {
      if (!this.validaForm) {
        this.$refs.form.validate()
        this.sendAlert = {
          show: true,
          time: '5000',
          text: 'Verifique os campos em destaque!',
          color: 'error'
        }
        return false
      }
      this.form.questions = this.userResponses

      let dataForm = {
        "cards": {
          "sections": [
            {
              "header": this.form.name + ' - ' + this.userResponses.reduce((total, response) => total + response.value, 0) + ' pontos' + ' - ' + this.formatCurrency(this.userResponses.reduce((total, response) => total + response.value, 0) * 22200),
              "widgets": [
                {
                  "keyValue": {
                    "topLabel": this.form.email,
                    "content": this.form.phone
                  }
                },
                {
                  "textParagraph": {
                    "text": JSON.stringify(this.form.questions)
                  }
                }
              ]
            }
          ]
        }
      }
      this.salvando = true
      await axios.post('https://chat.googleapis.com/v1/spaces/AAAAFVPa8VQ/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=ukj5b8lcp2B76_HM6U6VqRvkso8X2KE_9i-T9hIpY70', dataForm)
        .then(response => {
          console.log(response.data)
          this.enviarForm = false
          // this.sendAlert = {
          //   show: true,
          //   time: '5000',
          //   text: 'Enviado!',
          //   color: 'success'
          // }
          this.salvando = false
        })
        .catch(err => {
          console.log(err)
          this.sendAlert = {
            show: true,
            time: '5000',
            text: 'Ocorreu um erro ao enviar!',
            color: 'error'
          }
          this.salvando = false
        })
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
  b {
    font-weight: 900!important;
  }
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

// @media (min-width: 1900px) {
//   .v-container {
//     max-width: 1200px;
//   }
// }
.font-thin {
  font-weight: 300;
  b {
    font-weight: 900;
  }
}
.button-large {
  font-size: 26px!important;
  font-weight: 900!important;
  line-height: normal!important;
  text-align: right!important;
  .v-btn__content {
    justify-content: flex-end;
    width: 100%;
    @media (max-width: 767px){
      justify-content: center;
    }
  }
  @media (max-width: 767px){
    font-size: 20px!important;
  }
  i {
    padding-top: 5px;
  }
  &-icon {
    margin-left: -25px;
  }
}
.logo {
  max-width: 300px;
  margin-bottom: 20px;
  @media (max-width: 767px){
    max-width: 180px;
    padding: 0 0 5px 0;
  }
}
.mw-div {
  margin: 0 auto;
  max-width: 1200px;
  width: 100%;
}
.link_none {
  color: white!important;
  text-decoration: none;
}
</style>