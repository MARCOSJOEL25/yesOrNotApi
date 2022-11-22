<template>
  <div class="container">
    <span v-if="img">
      <img :src="img" alt="bg" />
    </span>
    <div class="bg-dark"></div>

    <div class="indecision-container">
      <input type="text" placeholder="hazme una pregunta" v-model="question"/>
      <p>recuerda terminar con un signo de interrogacion (?)</p>
    </div>
    <div class="answer" v-if="isValid">
      <h2>{{question}}</h2>
      <h1>{{answer }}</h1>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      question: null,
      answer: null,
      img:null,
      isValid: null
    }
  },
  methods: {
    getYesOrNot(){
      fetch("https://yesno.wtf/api")
        .then(response => response.json())
        .then((data) => {
          this.answer = data.answer === 'yes'? 'Si!' : 'No!'
          this.img = data.image
          console.log(data)
        } )
    }
  },
  watch:{
    question(value, oldValue){
      this.answer = "...pensando"
      this.isValid = false
      if(!value.includes("?")) return
      this.isValid = true
      this.getYesOrNot()
    }
  }
}
</script>

<style>
img,
.bg-dark {
  height: 100vh;
  left: 0px;
  max-height: 100%;
  max-width: 100%;
  position: fixed;
  top: 0px;
  width: 100vw;
}

.bg-dark {
  background-color: rgba(0, 0, 0, 0.4);
}

.indecision-container {
  position: absolute;
  z-index: 99;
  top: 0;
  left: 0;
  right: 0;
  bottom: 150px;
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 450px;
}

.answer{
  position: absolute;
  z-index: 98;
  top: 0;
  left: 0;
  right: 0;
  bottom: 50px;
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.indecision-container .answer{
  text-align: center;
}

input {
  width: 250px;
  padding: 10px 15px;
  border-radius: 5px;
  border: none;
}
input:focus {
  outline: none;
}

p {
  color: white;
  font-size: 20px;
  margin-top: 0px;
}

h1,
h2 {
  color: white;
}

h2 {
  margin-top: 150px;
}
</style>
