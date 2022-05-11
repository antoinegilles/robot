<template>
  <div class="coque">
    <div class="robot">
      <div class="oreille1">
        <div class="base_antenne1"></div>
        <div class="antenne_antenne1"></div>
        <div class="boule_antenne1"></div>
      </div>
      <div class="oreille2">
        <div class="base_antenne2"></div>
        <div class="antenne_antenne2"></div>
        <div class="boule_antenne2"></div>
      </div>

      <div class="oeil1"></div>
      <div class="oeil2"></div>
      <div v-if="recherche" class="boucheMouvement"></div>
    </div>
    <div class="cou"></div>
  </div>

  <div class="saisie content">
    <div class="input1">
      <input
        v-model="message"
        type="text"
        name=""
        id=""
        class="saisie_input"
      />
      <label class="label_input">Posez votre question</label>
    </div>
<!-- Test -->
    <div @click="sendMessage" class="btn_parent">
      <div class="parent">
        <div class="background"></div>
        <div class="arrow1"></div>
        <div class="arrow2"></div>
        <span class="text-send">Envoyer</span>
        <!-- <button class="btn_send" s @click="sendMessage">></button> -->
      </div>
    </div>

    <div class="reponse">
      {{ reponse }}
    </div>
  </div>
</template>

<script>
import { Configuration, OpenAIApi } from "openai";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      message: "",
      reponse: "",
      recherche: false,
    };
  },
  methods: {
    sleep(ms) {
      return new Promise((resolve) => setTimeout(resolve, ms));
    },
    async sendMessage() {
      console.log(process.env.VUE_APP_OPENAI_API_TOKEN)
      const configuration = new Configuration({
        // organization: "org-kZeQvc757DgeZi3xuxeyUlYJ",
        apiKey: process.env.VUE_APP_OPENAI_API_TOKEN,
      });
      const openai = new OpenAIApi(configuration);

      const response = await openai.createCompletion("text-davinci-002", {
        prompt: this.message,
        max_tokens: 6,
        temperature: 0,
      });
      this.reponse = response.data.choices[0].text;
      console.log(response)
      this.recherche = true;
      this.message = "" 

      for (let i = 0; i < 3; i++) {
        await this.sleep(i * 1000);
      }
      this.recherche = false;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped>
.reponse {
  margin-top: 3em;
  color: white;
  font-family: "Roboto", sans-serif;
}
.input1 {
  display: inline-block;
  position: relative;
}
.btn_parent {
  display: inline-block;
  margin-left: 1em;
  transform: translateY(64%);
}
.text-send {
  color: white;
  font-family: "Roboto", sans-serif;
  font-weight: bold;
  position: absolute;
  top: 0em;
  left: 4.2em;
}
.btn_send {
  border-radius: 2em;
  width: 3em;
  background-color: black;
  /* transform: translateY(-50%); */
  color: white;
  border: none !important;
  height: 3em;
  outline: none;
  transition: width 100ms ease-in;
}
.parent {
  position: relative;
  width: 3em;
  height: 3em;
}
.background {
  position: absolute;
  top: 0.5em;
  transform: translateY(-50%);
  width: 4em;
  height: 4em;
  background: black;
  border-radius: 80px;
  transition: width 200ms ease-in;
}
.parent:hover .background {
  width: 10em;
}
.parent:hover .arrow1 {
  left: 2em;
}
.parent:hover .arrow2 {
  /* display: unset; */
  background: white;
  left: 1.4em;
}
.arrow1 {
  color: white;
  position: absolute;
  font-weight: bold;
  left: 1.2em;
  top: 0em;
  width: 0.8em;
  height: 0.8em;
  border-top: 0.2em solid white;
  border-right: 0.2em solid white;
  transform: rotate(45deg);
  transition: 150ms cubic-bezier(0.4, 0, 0.2, 1);
}
.arrow2 {
  /* display: none; */
  position: absolute;
  font-weight: bold;
  left: 0.1em;
  top: 0.4em;
  width: 1em;
  height: 0.2em;
  background: black;
  transition: 550ms cubic-bezier(0.4, 0, 0.2, 1);
}
.saisie {
  margin-top: 3em;
  position: relative;
  margin-left: 3em;
  cursor: pointer;
}
.saisie_input {
  background-color: #282b2c;
  height: 3em;
  width: 17em;
  border-radius: 1.2em;
  border: 0.2em solid black;
  color: white;
  outline: none !important;
  padding: 0.3em;
  padding-left: 0.5em;
}

.saisie_input:focus ~ .label_input {
  transform: translateY(-150%) translateX(-40%) scale(0.7);
  padding: 0 0.2em;
}
.label_input {
  position: absolute;
  top: 1em;
  left: 2.7em;
  font-family: "Roboto", sans-serif;
  font-weight: bold;
  pointer-events: none;
  transition: 150ms cubic-bezier(0.4, 0, 0.2, 1);
  background-color: #282b2c;
  height: 1em;
  width: 10em;
  color: white;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.cou {
  width: 11em;
  height: 1.5em;
  background-color: #000000;
  position: absolute;
  top: 17em;
  left: 4em;
  border-radius: 0em 0em 0.6em 0.6em;
}
.coque {
  position: relative;
  background-color: rgb(255, 255, 255);
  width: 19em;
  height: 17em;
  border-radius: 8em 8em 0em 0em;
  margin: auto;
  box-shadow: 0em 0em 17px 1px black;
  border: 0.2em solid black;
}
.robot {
  background-color: #000000;
  width: 13em;
  height: 13em;
  border-radius: 5em 5em 0em 0em;
  position: absolute;
  top: 3em;
  left: 3em;
}
.oeil1 {
  background-color: #ffd849;
  width: 1em;
  height: 3em;
  border-radius: 1em;
  position: absolute;
  left: 3em;
  top: 5em;
}
.oeil2 {
  background-color: #ffd849;
  width: 1em;
  height: 3em;
  border-radius: 1em;
  position: absolute;
  left: 9em;
  top: 5em;
}

.boucheMouvement {
  width: 4em;
  height: 3em;
  border-radius: 2em;
  background-color: #ffd849;
  position: absolute;
  top: 9em;
  left: 3.3em;
  animation-name: example;
  animation-duration: 0.4s;
  animation-iteration-count: infinite;
  /* animation-direction: alternate; */
}
@keyframes example {
  0% {
    width: 4em;
    height: 3em;
  }
  25% {
    width: 3em;
    height: 2em;
  }
  50% {
    width: 3em;
    height: 2em;
  }
  100% {
    width: 4em;
    height: 3em;
  }
}

/* Oreille gauche ( vue de l'écran) */
.base_antenne1 {
  width: 3em;
  height: 7em;
  background-color: white;
  position: absolute;
  right: 15em;
  border-radius: 1em 0em 0em 1em;
  top: 4em;
  border: 0.2em solid black;
  z-index: -1;
}
.antenne_antenne1 {
  width: 0.8em;
  height: 7em;
  background-color: white;
  position: absolute;
  right: 16.2em;
  bottom: 7em;
  border: 0.2em solid black;
  z-index: -2;
}
.boule_antenne1 {
  width: 3em;
  height: 3em;
  position: absolute;
  background-color: white;
  bottom: 13em;
  border-radius: 3em;
  right: 15em;
  border: 0.2em solid black;
  z-index: -2;
}
/* Oreille droite ( vue de l'écran ) */
.base_antenne2 {
  width: 3em;
  height: 7em;
  background-color: white;
  position: absolute;
  left: 15em;
  border-radius: 0em 1em 1em 0em;
  top: 4em;
  border: 0.2em solid black;
  z-index: -1;
}
.antenne_antenne2 {
  width: 0.8em;
  height: 7em;
  background-color: white;
  position: absolute;
  left: 16.2em;
  bottom: 7em;
  border: 0.2em solid black;
  z-index: -2;
}
.boule_antenne2 {
  width: 3em;
  height: 3em;
  position: absolute;
  background-color: white;
  bottom: 13em;
  border-radius: 3em;
  left: 15em;
  border: 0.2em solid black;
  z-index: -1;
}

/* @media (max-width: 347px) { */
@media (max-width: 600px) {
  .input1 {
    margin: auto;
  }
  .btn_parent {
    margin-left: 30%;
    margin-top: 1em;
    display: block;
  }
}
</style>
