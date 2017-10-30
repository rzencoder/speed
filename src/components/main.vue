<template>
  <div class="hello">
    <div :class="{'overlay': true, show: !startModal}">
      <div class="modal start-modal" id="start-modal">
        <h2>SPEED Typing</h2>
        <p>Keep your typing speed above the selected WPM or the bomb will explode</p>
        <p>Select WPM Target</p>
        <div>
          <button @click="setLevel(20)">20</button>
          <button @click="setLevel(40)">40</button>
          <button @click="setLevel(60)">60</button>
        </div>
      </div>
    </div>
    <div :class="{'overlay': true, show: !modal}">
      <div class="modal result-modal">
        <p>{{msg}}</p>
        <button @click="restart">Restart</button>
      </div>
    </div>
    <div class="info">
      <h2 class="errors">{{errors}} Errors</h2>
      <h1>SPEED</h1>
      <h2 class="wpm">{{wpm}} WPM</h2>
    </div>  
    <div :class="{scene: true, play: !stop }">
      <h2 :class="{armed: true, show: !armed}">Bomb Armed</h2>
      <div :class="{bus: true, show: detonated }" :style="{'left': busPos + 'px'}">
        <div class="wheel back"></div>
        <div class="wheel front"></div>
      </div>
      <div :class="{explosion: true, show: detonated}"></div>     
    </div>
    <div class="text">{{text}}</div>
    <div class="attempt"></div>
  </div>
</template>

<script>

export default {
  name: 'Main',
  data () {
    return {
      text: lyrics[0],
      wpm: 0,
      level: 0,
      startTime: Date.now() /1000,
      busPos: 260,
      correct: 0,
      errors: 0,
      msg: '',
      armed: false,
      detonated: false,
      stop: true,
      startModal: true,
      modal: false
    }
  },

  methods: {

    endGame () {
      this.stop = true;
      clearInterval(interval);
      const attemptNode = document.querySelector('.attempt');
      while(attemptNode.firstChild) {
        attemptNode.removeChild(attemptNode.firstChild);
      }
      this.modal = true;
    },

    restart () {
      this.modal = false;
      this.startModal = true;
    },

    reset () {
      this.startModal = false;
      this.text = okay;
      this.wpm = 0;
      this.startTime = Date.now() /1000;
      this.busPos = 0;
      this.correct = 1;
      this.errors = 0;
      this.armed = false;
      this.detonated = false;
      this.stop = false;
    },

    init () {
      this.stop = false;
      setTimeout(() => {
      interval = setInterval(() =>{
        let dateInSecs = Date.now() / 1000;
        let time =  Math.floor(dateInSecs - this.startTime);
        this.wpm = Math.ceil((this.correct / 5) / (time / 60));
        this.busPos = (this.wpm - this.level) * 10 + 240;
        if (this.wpm >= this.level + 5){
          this.armed = true;
        }
        if (this.wpm < this.level && this.armed) {
          this.msg = "The Bomb was Detonated!";
          this.detonated = true;
          setTimeout(()=> {
            this.endGame();
          }, 1000)
          
        }
        if (!this.text) {
          this.msg = "You defused the Bomb!";
          this.endGame();
        }
      }, 50);
      }, 1000)
    },

    setLevel (num) {
      this.level = num;
      this.reset();
      this.init();
    }

  

  },

  created () {
    
    window.addEventListener("keydown", (e) => {
      
      if (e.key !== 'Shift' && e.key !== 'CapsLock' && e.key !== 'Backspace') {
        const attemptNode = document.querySelector('.attempt');
        const div = document.createElement('div');
        div.style.height = '25px';
        div.style.width = '12px';
        div.style.verticalAlign = 'top';
        div.style.display = 'inline-block';
        if (this.text[0] === e.key){       
          this.text = this.text.substring(1);
          div.style.background = 'green';
          this.correct++;
        } else {
          div.style.background = 'red';
          this.errors++;
        }      
        div.innerHTML = e.key;
        attemptNode.appendChild(div);
      }
    })
      
  }
  
}

import lyrics from '../assets/lyrics.js'

let interval;
const templateText = '"We believe that we can';
const okay = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa";

function randomLyrics (list, level) {
  if(level === 20){
    return list.easy[Math.floor(Math.random() * list.easy.length)];
  } else if (level === 40){
    return list.medium[Math.floor(Math.random() * list.medium.length)];
  } else {
    return list.hard[Math.floor(Math.random() * list.hard.length)];
  }
}

</script>

<style lang="scss" scoped>

.hello {
  text-align: center;
}

.text {
  font-weight: normal;
  font-size: 30px;
  padding: 5px;
  max-width: 800px;
  min-height:30px;
  width:800px;
  display:inline-block;
  position:relative;
  white-space:pre;
  text-align: left;
  overflow: hidden;
  
  &:before {
    content: '';
    position: absolute;
    bottom: 0px;
    width: 20px;
    height: 3px;
    background: lightblue;
    animation: flash 1s ease-in infinite;
  }
}

@keyframes flash {
  0% { opacity: 0.3; }
  100% { opacity: 1; }
}


.correct {
  background: green;
}

.wrong {
  background: red;
}



.scene {
  position: relative;
  height: 350px;
  width: 1200px;
  overflow: hidden;
  background: url('../assets/landscape.png') repeat 0 0;
  animation: travel 8s linear infinite;
  margin: 0 auto;
  animation-play-state: paused;
  &.play {
    animation-play-state: running;
  }
}

.bus {
  display: block;
  position: absolute;
  top: 190px;
  left: 340px;
  height: 90px;
  width: 330px;
  background: url('../assets/bus.png') 0 0;
  background-size: cover;
  &.show {
    display: none;
  }
}


@keyframes travel {
  from { background-position: 0, 0; }
    to { background-position: -3000px, 0; }
}

.attempt {
  height: 170px;
  background: #555;
  width: 800px;
  max-width: 800px;
  margin: 0 auto;
  text-align: left;
  line-height: 2;
}

.explosion {
  position: absolute;
  background: url('../assets/explosion.png');
  top: 200px;
  left: 340px;
  height: 100px;
  width: 200px;
  background-size: contain;
  display: none;
  animation: explode 2s ease-out;
  transform: scale(0);
  &.show {
    display: block;
  }
}

@keyframes explode {
  0% { transform: scale(0) };
  50% { transform: scale(2) }
  100% { transform: scale(0) }
}

.info {
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.modal {
  background: #222;
  width: 60%;
  border: 3px solid red;
  border-radius: 10px;
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;

}

.start-modal {
  text-align: center;
  p {
    width: 80%;
    font-size: 20px;
    margin: 0;
    padding: 10px;
  }
  button {
    font-size: 26px;
    font-weight: 900;
    padding: 6px;
    background: white;
    color: #222;
    border-radius: 50%;
    border: 6px solid red;
    cursor: pointer;
    margin: 0 5px 10px;
  }

}

.result-modal {
  font-size: 30px;
  color: white;
  font-weight: 700;
  margin-bottom: 250px;
  button {
    background: red;
    border: 3px solid red;
    border-radius: 5px;
    color: white;
    font-size: 25px;
    font-weight: 700;
    padding: 3px 10px;
    margin-bottom: 10px;
    cursor: pointer;
  }
}

.overlay  {
  background: rgba(0,0,0,0.5);
  position: absolute;
  width: 100%;
  height: 100vh;
  top: 0;
  left: 0;
  z-index: 3;
  display: flex;
  align-items: center;
  justify-content: center;
  &.show {
    display: none;
  }

}

.wpm {
  font-size: 30px;
  width: 200px;
}

.errors {
  font-size: 30px;
  width: 200px;
}

.wheel {
  position: absolute;
  background: #bbb;
  width: 15px;
  height: 15px;
  border-radius: 50%;
  animation: wheels 0.5s linear infinite;
  &:before {
    content: '';
    width: 3px;
    height: 3px;
    position: absolute;
    background: #222;
    top: 2px;
    left: 6px;
    border-radius: 50%;
    box-shadow: 0px 8px #222,
                3px 4px #222,
                -3px 4px #222;
  }
  &.front {
    top: 68px;
    left: 264px;
  }
  &.back {
    top: 68px;
    left: 68px;
  }
}

@keyframes wheels {
  from { transform: rotate(0deg) }
    to { transform: rotate(360deg) }
}

.armed {
  text-shadow: 2px 2px #222;
  margin-top: 50px;
  &.show {
    display: none;
  }
}

</style>
