<template>
  <div class="yes-no">
    <h1>Ask me something...</h1>
    <form @submit.prevent="getRandom">
      <input type="text" placeholder="What is your question?" v-model="curr">
      <button>Send</button>
    </form>

    <div v-if="yn.image !== ''">
      <div class="answer" >
        <h3 :class="yn.answer"> The answer is 
          <span>{{ yn.answer }}</span></h3>
      
      <figure>
        <img :src="yn.image" :alt="yn.answer" class="img">
      </figure>
      </div>
      <div>
        <h3>History:</h3>
        <div class="history">
            <div 
            class="hist-item" 
            :class="q.ans"
            :style="{backgroundImage: `url(${q.img})`}"
            :key="q.id" v-for="q in quest">
              <span v-html="q.text"></span>
              <!-- <span>{{q.text}}</span> -->
            </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import { v4 as uuidv4 } from 'uuid';

export default {
  name: 'YesNo',
  data() {
    return {
      yn: {
        answer: '',
        forced: '',
        image: ''
      },
      curr: '',
      quest: []
    }
  },
  methods: {
    getRandom() {
      if (this.curr !== '') {
        axios.get('https://yesno.wtf/api')
        .then(res => {
          this.yn = res.data;
          const newq = {
            id: uuidv4(),
            text: this.curr + " &rarr; " + res.data.answer,
            ans: res.data.answer,
            img: res.data.image
          }
          this.quest = [newq, ...this.quest];
          this.curr='';
          console.log(this.quest);
          })
        .catch(err => console.log(err))
      }
    }
  }

};
</script>

<style>

img {
  width: 300px;
}

h3 {
  display:inline-block;
  padding: 5px 10px;
}
.yes {
  background: green;
  color: white;
}
.no {
  background: red;
  color: white;
}

.history {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 1rem;
}

.hist-item {
  border: 1px solid #111;
  padding: 1rem;
  border-radius: 15px;
  text-align: center;
  position: relative;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center center;
  min-height: 150px;
  }

.hist-item span{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: start;
    font-size: 2em;
    font-weight: 700;
  }
</style>