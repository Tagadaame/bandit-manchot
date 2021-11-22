<template>
  <div v-if="userCity"  id="container" :class="isWinner === 'true' ? 'bgWin' : 'bgLoose'">
    <header class="intro z2 txt-white">
      <span>TENTEZ VOTRE CHANCE JUSQU'AU 3 DÉCELMBRE 2021</span>
      <p>Pour une fois que <strong>c'est vous le client,</strong>
      <br />Vous auriez tord de ne pas en profiter !</p>
    </header>
    <div id="wrapper">
      <div id="enseigne" class="z2 txt-white txt-cursive">
      Grand Jeu
      <img src="./../assets/img/logo.png" />
      </div>
      <p id="intro" class="z2">
        <strong>Tentez de gagner un bon d’achat Promocash<br />
        d’une valeur de 50€</strong> en jouant dès à présent.<br />
        Ferez-vous partie des 100 gagnants du jour ?
        <span>Bonne chance !</span>
      </p>
      <div id="anim" class="z2">
        <div id="manchot"></div>
      </div>
      <button id="play" class="btn txt-white z2"><span>Je joue !</span></button>
      <!-- <button @click="$refs.modalResult.openModal()">button</button> -->
      <div id="bottom" class="z2 txt-white">
        <label class="checkbox">
          <input type="checkbox" id="acceptCkeck" />
          <svg viewBox="0 0 21 18">
              <symbol id="tick-path" viewBox="0 0 21 18" xmlns="http://www.w3.org/2000/svg">
                  <path d="M5.22003 7.26C5.72003 7.76 7.57 9.7 8.67 11.45C12.2 6.05 15.65 3.5 19.19 1.69" fill="none" stroke-width="2.25" stroke-linecap="round" stroke-linejoin="round" />
              </symbol>
              <defs>
                  <mask id="tick">
                      <use class="tick mask" href="#tick-path" />
                  </mask>
              </defs>
              <use class="tick" href="#tick-path" stroke="currentColor" />
              <path fill="white" mask="url(#tick)" d="M18 9C18 10.4464 17.9036 11.8929 17.7589 13.1464C17.5179 15.6054 15.6054 17.5179 13.1625 17.7589C11.8929 17.9036 10.4464 18 9 18C7.55357 18 6.10714 17.9036 4.85357 17.7589C2.39464 17.5179 0.498214 15.6054 0.241071 13.1464C0.0964286 11.8929 0 10.4464 0 9C0 7.55357 0.0964286 6.10714 0.241071 4.8375C0.498214 2.39464 2.39464 0.482143 4.85357 0.241071C6.10714 0.0964286 7.55357 0 9 0C10.4464 0 11.8929 0.0964286 13.1625 0.241071C15.6054 0.482143 17.5179 2.39464 17.7589 4.8375C17.9036 6.10714 18 7.55357 18 9Z" />
          </svg>
          <svg class="lines" viewBox="0 0 11 11">
              <path d="M5.88086 5.89441L9.53504 4.26746" />
              <path d="M5.5274 8.78838L9.45391 9.55161" />
              <path d="M3.49371 4.22065L5.55387 0.79198" />
          </svg>
        </label>
        <span class="z2 txt-white">J’accepte <a href="/jeu/instant-gagnant/reglement" target="_blank">le réglement</a> du jeu.</span>
      </div>
    </div>
    <footer>
      <img src="./../assets/img/footer.png" />
    </footer>
  </div>

  <Modal v-if="isWinner === 'true'" ref="modalResult">
    <template v-slot:header>
      <h2>Félicitations !</h2>
    </template>

    <template v-slot:body>
      <div class="boncado"></div>
      <h3>Vous avez gagné un bon d’achat d’une valeur de 50€ </h3>
      <p><strong>Ce bon d’achat est à utiliser dans votre magasin Promocash {{ userCity }} </strong></p>
      <p>Le bon d’achat sera disponible dès demain 
        en caisse et jusqu’au 31 décembre 2021, 
        utilisable à partir de 250€ d’achats. 
        Un email de confirmation vient de vous être envoyé.
      </p>
      <p>À bientôt !</p>
    </template>

    <template v-slot:footer>
      <!--<div class="d-flex align-items-center justify-content-between">
        <button class="btn btn--primary" @click="$refs.modalResult.closeModal()">J'ai compris</button>
      </div>-->
    </template>
  </Modal>

  <Modal v-else ref="modalResult">
    <template v-slot:header>
      <h2>Perdu !</h2>
    </template>

    <template v-slot:body>
      <h3>Malheureusement, vous n’avez pas gagné mais tout peut encore arriver !</h3>
      <p>Si vous réalisez à nouveau un chariot de 150€ minimum dans votre magasin {{ userCity }} 
        d’ici le 30 novembre, vous recevrez un deuxième email pour participer une seconde fois au tirage au sort et tenter de gagner un bon d’achat.
        Nous vous remercions de votre fidélité ! 
      </p>
      <p>À très bientôt</p>
    </template>

    <template v-slot:footer>
      <!--<div class="d-flex align-items-center justify-content-between">
        <button class="btn btn--primary" @click="$refs.modalResult.closeModal()">J'ai compris</button>
      </div>-->
    </template>
  </Modal>

  <div id="alert">
    <img src="./../assets/img/icon-info.png" />
    <p>Veuillez accepter le règlement du Jeu 
    pour participer</p>
  </div>

</template>


<script>

import Modal from "./Modal";
import lottieWeb from 'lottie-web';
import axios from 'axios';

export default {
  name: "BanditManchot",
  components: {
    Modal
  },
  data() {
    return {
      path: '',
      isWinner:null,
      userCity:null,
      csrf:'',
      url:''
    }
  },
  methods: {
    getPath() {
      if (this.isWinner === 'true') {
         //DECOMMENT FOR local this.path = 'http://www.kb-style.fr/PMC/bandit_manchot_win.json';
         this.path = '/templates/banditManchot/lottiefiles/bandit_manchot_win.json';
      } else {
        //DECOMMENT FOR local this.path = 'http://www.kb-style.fr/PMC/bandit_manchot_lose.json';
         this.path = '/templates/banditManchot/lottiefiles/bandit_manchot_lose.json';
      }
    }
  },

  mounted() {
    this.userCity = document.getElementById("app").getAttribute("usercity");
    this.isWinner = document.getElementById("app").getAttribute("iswinner");
    this.csrf = document.getElementById("app").getAttribute("csrf");
    this.url = document.getElementById("app").getAttribute("url");


    window.addEventListener("DOMContentLoaded", () => {
   
      var that = this    
      setTimeout(function() {

        that.userCity = document.getElementById("app").getAttribute("usercity");
        that.isWinner = document.getElementById("app").getAttribute("iswinner");
        that.csrf = document.getElementById("app").getAttribute("csrf");
        that.url = document.getElementById("app").getAttribute("url");

    
        that.getPath();

        var container = document.getElementById('manchot');
        var btnPlay = document.getElementById('play');
        var acceptCkeck = document.getElementById("acceptCkeck");
        var alert = document.getElementById("alert");
        var bottom = document.getElementById("bottom");

        var animation = lottieWeb.loadAnimation({
          container: container,
          path: that.path,
          renderer: 'svg',
          loop: false,
          autoplay: false,
          name: "Bandit Manchot",
        });

        animation.goToAndStop(0, true); 

        animation.onComplete = function() {
          that.$refs.modalResult.openModal();
          bottom.style.opacity = "0";
        }

        var count = 0;

        btnPlay.addEventListener('click', () => {
          if (count < 1)  { 
            if (acceptCkeck.checked == true) {
              count++;
              animation.play();
              btnPlay.style.visibility = "hidden";
              document.querySelector("body").classList.add("finish")
              alert.style.opacity = "0";
              alert.style.height = "0";
              bottom.classList.remove("wizz");
              axios.post(that.url, {'csrf':that.csrf});

            } else {
              alert.style.opacity = "1";
              alert.style.height = "auto";
              bottom.classList.add("wizz")
            }
          } 
        });

        acceptCkeck.addEventListener('click',() => {
          if (acceptCkeck.checked == true) {
            console.log('checked');
            alert.style.opacity = "0";
            alert.style.height = "0";
            bottom.classList.remove("wizz")
          }
        });
      }, 1500) 
      
    });

  }};

</script>

<style lang="scss">

@font-face {
    font-family: 'tahuregular';
    src: url('./../assets/fonts/tahu-webfont.woff2') format('woff2'),
         url('./../assets/fonts/tahu-webfont.woff') format('woff');
    font-weight: normal;
    font-style: normal;
}

  html, body {
    margin: 0px;
    max-height: 100vh;
  }

  body {
    background: rgb(201,36,57);
  }

  .txt-white, .txt-white p, .txt-white span, 
  .txt-white label, .txt-white a {
    color: #ffffff;
  }

  .z2 {
    z-index: 2;
    position: relative;
  }

  #app {
    font-family: Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #000000;
    #container {
      margin-top: 60px;
      background:radial-gradient(circle, rgba(201,36,57,1) 0%, rgba(155,10,28,1) 100%) no-repeat center center, url('./../assets/img/bg.png') no-repeat center center;
      background-size: cover;
      background-blend-mode: screen;
      margin: 0px;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      transition: all 0.2s easeIn;
       &:before {
        z-index: 1;
        content: ' ';
        top: 15vh;
        position: absolute;
        background: url('./../assets/img/etoile.png') no-repeat center transparent;
        left: 0;
        right: 0;
        height: 275px;
        width: 325px;
        margin-left: -162px;
        left: 50%;
      }   
      &.finish.bgWin {
        background:radial-gradient(circle, rgba(201,36,57,1) 80%, rgba(155,10,28,1) 100%) no-repeat center center, url('./../assets/img/bg.png') no-repeat center center;
        background-size: cover;
      }
    }
  }
  header {
    padding: 1rem;
    span {
      text-transform: uppercase;
      font-size: 0.8rem;
      margin: 0.5rem 0;
      display: block;
    } 
    p {
      margin: 0.5rem 0;
      font-size: 0.95rem;
    }
  }
  #wrapper {
    margin: auto;
    position: relative;
    max-width: 600px;
    width: 98%;
    @media screen and (min-width: 490px) {
      width: 70%;
    }
    &:before {
      z-index: 1;
      background: #FFAE2C;
      content:' ';
      border-radius: 1rem;
      box-shadow: 0px 0px 25px -2px rgba(0,0,0,0.65);
      top: 17%;
      left: 5%;
      right: 5%;
      position: absolute;
      height: 440px;
      @media screen and (min-width: 700px) {
        height: 500px;
      }
    }
    #enseigne {
      background-image: url('./../assets/img/enseigne-bg-full.png');
      background-position: center;
      background-repeat: no-repeat;
      background-size: 100%;
      font-weight: 500;
      font-size: 1.25rem;
      position: relative;
      width: 263px;
      height: 205px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      margin: auto;
      max-width: 100%; 
      &:after {
        z-index: 1;
        content:' ';
        top: 0%;
        left: 0%;
        right: 0%;
        bottom: 0%;
        position: absolute;
        background: url('./../assets/img/enseigne-bg-only-light.png') no-repeat center center transparent;
        background-size: 100%;
        mix-blend-mode: lighten;
        animation: blink 700ms infinite;
      }
      img {
        max-width: 100%;
        height: auto;
      }
    }
    #intro {
      margin: 0 9% 0.5rem 9%;
      font-size: 0.85rem;
      span {
        display: block;
        font-size: 1.5rem;
        margin-top: 0.3rem;
        font-family: 'tahuregular';
      }
    }
    #anim {
      width: 100%;
      height: auto;
      aspect-ratio: 2 / 1;
      overflow: hidden;
      > div {
        width: inherit;
        height: 100%;
      }
      &:hover ~ #play {
        transform: scale(1.2);
      }
    }
    #play {
      background-image: url('./../assets/img/bg-btn-jouer.png');
      background-position: center;
      background-repeat: no-repeat;
      background-size: 100%;
      background-color: transparent;
      border: 0px solid;
      box-shadow: none;
      font-weight: 500;
      font-size: 1.25rem;
      position: relative;
      width: 170px;
      height: 76px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      margin:0 auto 0.5rem;
      max-width: 100%;
      cursor: pointer;
      transition: all 25ms ease;
      &:hover {
        transform: scale(1.2);
      }
      span {
        text-transform: uppercase;
        font-weight: bold;
        color: #ffffff;
      }
      &:after {
        z-index: 1;
        content:' ';
        top: 0%;
        left: 0%;
        right: 0%;
        bottom: 0%;
        position: absolute;
        background: url('./../assets/img/bg-btn-jouer-light.png') no-repeat center center transparent;
        background-size: 100%;
        mix-blend-mode: lighten;
        animation: blink 700ms infinite;
      }
    }
    #bottom {
      padding: 1rem .5rem;
    }
  }
  footer {
    background: #ffffff;
    padding: 0.2rem 0.5rem;
  }

  #alert {
    background: red;
    color: #ffffff;
    padding: 1rem 1rem 0rem 1rem;
    text-align: center;
    position: fixed;
    top: 0px;
    left: 0px;
    right: 0px;
    z-index: 8;
    opacity: 0;
    transition: all 25ms ease;
    height: 0;
  }

  .modal__header {
    h2 {
      font-family: 'tahuregular';
      color: #C92439;
      text-align: center;
      font-size: 2.75rem;
      font-weight: normal;
      margin: 1.5rem auto 1rem;
      &:after {
        content: '';
        width: 2px;
        height: 15px;
        display: block;
        background: #C92439; 
        margin: 5px auto 0px;
      }
    }
    h3 {
      font-size: 16px;
    }
  }

  .modal__body {
    text-align: center;
    .boncado {
      width: 183px;
      height: 84px;
      margin: 0px auto;
      background: url('./../assets/img/bon-cadeau50.jpg') no-repeat center center transparent;
      background-size: contain;
    }
  }

  #reglement {
    .modal__dialog {
      width: 700px;
      max-width:100%;
      .modal__header {
        justify-content: space-between;
      }
    }
    button {
      background-color: #72c923;
      color: #ffffff;
      border: 0px solid;
      box-shadow: none;
      padding: .25rem .5rem;
      text-transform: uppercase;
      font-weight: bold;
      font-size: 1.1rem;
      cursor: pointer;
      &:hover {
         background-color: #579d09;
      }
    }
  }

  .wizz {
    animation: 1s wizz .25s ease 1 backwards;
  }

  .checkbox {
    --border-default: #ffffff;
    --border-hover: #ffffff;
    --active: #72c923;
    --active-tick: #ffffff;
    --background: #ffffff;
    display: block;
    width: 18px;
    height: 18px;
    cursor: pointer;
    position: relative;
    -webkit-tap-highlight-color: transparent;
    margin-right: 7px;
    display: inline-block;
    vertical-align: text-top;
    svg {
      display: block;
      position: absolute;
    }
    input {
      display: block;
      outline: none;
      border: none;
      padding: 0;
      margin: 0;
      -webkit-appearance: none;
      width: 18px;
      height: 18px;
      border-radius: 36% / 36%;
      box-shadow: inset 0 0 0 1.5px var(--border, var(--border-default));
      background: var(--background, transparent);
      transition: background .25s linear, box-shadow .25s linear;
      & + svg {
        width: 21px;
        height: 18px;
        left: 0;
        top: 0;
        color: var(--active);
        .tick {
          stroke-dasharray: 20;
          stroke-dashoffset: var(--stroke-dashoffset, 20);
          transition: stroke-dashoffset .2s;
          &.mask { 
            stroke: var(--active-tick);
          }
        }
        & + svg {
          width: 11px;
          height: 11px;
          fill: none;
          stroke: var(--active);
          stroke-width: 1.25;
          stroke-linecap: round;
          top: -6px;
          right: -10px;
          stroke-dasharray: 4.5px;
          stroke-dashoffset: 13.5px;
          pointer-events: none;
          animation: var(--animation, none) .2s ease .175s;
        }
      }
      &:checked {
        --background: var(--active);
        --border: var(--active);
        & + svg {
          --stroke-dashoffset: 0;
          & + svg {
            --animation: check;
          }
        }
      }
    }
    &:hover {
      input {
        &:not(:checked) {
          --border: var(--border-hover);
        }
      }
    }
  }

  @keyframes blink {
    0%    { opacity: 0; }
    20%   { opacity: 1; }
    80%   { opacity: 0; }
    100%  { opacity: 1; }
  }

  @keyframes wizz {
    0% {
      transform: translateX(-7px) rotate(-2deg);
    }
    15% {
      transform: translateX(7px) rotate(0deg);
    }
    30% {
      transform: translateX(-7px) rotate(2deg);
    }
    45% {
      transform: translateX(7px) rotate(0deg);
    }
    60% {
      transform: translateX(-7px) rotate(-2deg);
    }
    75% {
      transform: translateX(7px) rotate(0deg);
    }
    90% {
      transform: translateX(-7px) rotate(2deg);
    }
    100% {
      transform: translateX(0) rotate(0deg);
    }
  }

  @keyframes check {
    100% {
      stroke-dashoffset: 4.5px;
    }
  }

</style>