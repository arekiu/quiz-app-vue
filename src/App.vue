<script setup>
    import {ref,computed} from "vue"

    const questions= ref([ 
        {    
          question: "What is the capital city of Moldova?",
          answer: 1,    
          options: [      
            "Chisinau",
            "Tiraspol",
            "Balti"    
          ],
        selected: null
      },
      {
        question: "What is the capital city of Kyrgyzstan?",
        answer: 2,
        options: [
          "Osh",
          "Bishkek",
          "Naryn"
        ],
        selected: null
      },
      {
        question: "What is the capital city of Montenegro?",
        answer: 0,
        options: [
          "Podgorica",
          "Budva",
          "Tivat"
        ],
        selected: null
      },
      {
        question: "What is the capital city of Burkina Faso?",
        answer: 2,
        options: [
          "Bobo-Dioulasso",
          "Ouagadougou",
          "Koudougou"
        ],
        selected: null
      },
      {
        question: "What is the capital city of Bhutan?",
        answer: 1,
        options: [
          "Punakha",
          "Thimphu",
          "Paro"
        ],
        selected: null
      },
      {
        question: "What is the capital city of Laos?",
        answer: 0,
        options: [
          "Vientiane",
          "Luang Prabang",
          "Savannakhet"
        ],
        selected: null
      },
      {
        question: "What is the capital city of Suriname?",
        answer: 2,
        options: [
          "Paramaribo",
          "Lelydorp",
          "Nieuw Nickerie"
        ],
        selected: null
      },
      {
        question: "What is the capital city of Eritrea?",
        answer: 1,
        options: [
          "Asmara",
          "Massawa",
          "Keren"
        ],
        selected: null
      },
      {
        question: "What is the capital city of Gambia?",
        answer: 2,
        options: [
          "Brikama",
          "Serrekunda",
          "Banjul"
        ],
        selected: null
      },
      {
        question: "What is the capital city of Cape Verde?",
        answer: 0,
        options: [
          "Praia",
          "Mindelo",
          "Santa Maria"
        ],
        selected: null
      }
  ])


    const quizCompleted = ref(false)

    const currentQuestion = ref(0)

    const score = computed(()=> {
      let value = 0
      questions.value.map(q =>{
        if(q.selected == q.answer){
          value++
        }
      })
      return value
    })

    const getCurrentQuestion = computed(()=>{
  
      let question = questions.value[currentQuestion.value]
      question.index = currentQuestion.value
      return question
    })

    
    const SetAnswer = event => {
      questions.value[currentQuestion.value].selected = event.target.value
      event.target.value = null
    }

    const NextQuestion = () => {
      if(currentQuestion.value < questions.value.length - 1){
        currentQuestion.value++
      } else{
        quizCompleted.value = true
      }
    }

</script>

<template>
  <main class="app">
		<h1>The Quiz</h1>
		
		<section class="quiz" v-if="!quizCompleted">
			<div class="quiz-info">
				<span class="question">{{ getCurrentQuestion.question }}</span>
				<span class="score">Score {{ score }}/{{ questions.length }}</span>
			</div>
			
			<div class="options">
				<label 
					v-for="(option, index) in getCurrentQuestion.options" 
					:for="'option' + index" 
					:class="`option ${
						getCurrentQuestion.selected == index 
							? index == getCurrentQuestion.answer 
								? 'correct' 
								: 'wrong'
							: ''
					} ${
						getCurrentQuestion.selected != null &&
						index != getCurrentQuestion.selected
							? 'disabled'
							: ''
					}`">
					<input 
						type="radio" 
						:id="'option' + index" 
						:name="getCurrentQuestion.index" 
						:value="index" 
						v-model="getCurrentQuestion.selected" 
						:disabled="getCurrentQuestion.selected"
						@change="SetAnswer" 
					/>
					<span>{{ option }}</span>
				</label>
			</div>
			
			<button 
				@click="NextQuestion" 
				:disabled="!getCurrentQuestion.selected">
				{{ 
					getCurrentQuestion.index == questions.length - 1 
						? 'Finish' 
						: getCurrentQuestion.selected == null
							? 'Select an option'
							: 'Next question'
				}}
			</button>
		</section>

		<section v-else>
			<h2>You have finished the quiz!</h2>
			<p>Your score is {{ score }}/{{ questions.length }}</p>
		</section>
	</main>
</template>

<style>
  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Monserrat', sans-serif;
  }

  body{
    background-color: #271C36;
    color: #FFF
  }
</style>
