<script setup>
import { ref, computed } from 'vue'

const questions = ref([
  {
	question: 'Que signifie HTML?',
	answer: 0,
	options: [
		'HyperText Markup Language',
		'Home Tool Markup Language',
		'Hyperlinks and Text Markup Language'
	],
	selected: null
  },
  {
	question: 'Quel élément HTML est utilisé pour créer un lien hypertexte?',
	answer: 2,
	options: [
		'<link>',
		'<href>',
		'<a>'
	],
	selected: null
  },
  {
	question: 'Lequel de ces éléments HTML est un conteneur de bloc?',
	answer: 1,
	options: [
		'<span>',
		'<div>',
		'<p>'
	],
	selected: null
  },
  {
	question: 'Comment centre-t-on un élément horizontalement avec CSS?',
	answer: 2,
	options: [
		'float: center',
		'text-align: center',
		'margin: 0 auto'
	],
	selected: null
  },

  {
	question: "Quelle propriété CSS est utilisée pour changer la couleur de fond d'un élément?",
	answer: 0,
	options: [
		'background-color',
		'color',
		'background'
	],
	selected: null
  },

  {
	question: "Quel sélecteur CSS est utilisé pour cibler un élément avec l'ID header?",
	answer: 1,
	options: [
		'.header',
		'#header',
		'header'
	],
	selected: null
  },

  {
	question: 'Comment déclare-t-on une variable en JavaScript (ES6)?',
	answer: 2,
	options: [
		'var',
		'declare',
		'const'
	],
	selected: null
  },

  {
	question: 'Quelle méthode est utilisée pour sélectionner un élément par son ID en JavaScript?',
	answer: 1,
	options: [
		'getElementByClassName()',
		'getElementById()',
		'querySelector()'
	],
	selected: null
  },

  {
	question: 'Lequel de ces opérateurs est utilisé pour comparer des valeurs strictement égales en JavaScript?',
	answer: 1,
	options: [
		'==',
		'===',
		'!=='
	],
	selected: null
  },
 
  {
	question: 'Quel symbole est utilisé pour déclarer une variable en PHP?',
	answer: 0,
	options: [
		'$',
		'@',
		'#'
	],
	selected: null
  },

  {
	question: 'Quelle fonction est utilisée pour afficher du texte en PHP?',
	answer: 2,
	options: [
		'display',
		'print',
		'echo'
	],
	selected: null
  },

  {
	question: 'Quel type de langage est PHP?',
	answer: 1,
	options: [
		'Langage côté client',
		'Langage côté serveur',
		'Langage de style'
	],
	selected: null
  },
 
  
])

const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
	let value = 0
	questions.value.map(q => {
		if (q.selected != null && q.answer == q.selected) {
			console.log('correct');
			value++
		}
	})
	return value
})

const getCurrentQuestion = computed(() => {
	let question = questions.value[currentQuestion.value]
	question.index = currentQuestion.value
	return question
})

const setAnswer = (e) => {
	questions.value[currentQuestion.value].selected = e.target.value
	e.target.value = null
}

const nextQuestion = () => {
	if (currentQuestion.value < questions.value.length - 1) {
		currentQuestion.value++
		return
	}
	
	quizCompleted.value = true
}
</script>

<template>
	<main class="app">
		<h1>The programming language quiz</h1>
		
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
						@change="setAnswer" 
					/>
					<span>{{ option }}</span>
				</label>
			</div>
			
			<button 
				@click="nextQuestion" 
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
  <p>Your score is {{ score }}/{{ questions.length }}!</p>
  <p class="mt-10 p-5 text-2xl border-2 border-emerald-600" v-if="score < 6">
  You really need to visit this : 
  <span class="underline"><a href="https://openclassrooms.com/fr/" target="_blank">OpenClassrooms</a></span>
</p>
<p class="mt-10 p-5 text-2xl border-2 border-emerald-600" v-else-if="score >= 6 && score <= 8">
  Well...
</p>
<p class="mt-10 p-5 text-2xl border-2 border-emerald-600" v-else-if="score >= 9 && score <= 11">
  You're good but you can do better!
</p>
<p class="mt-10 p-5 text-2xl border-2 border-emerald-600" v-else>
  You're incredible!
</p>

</section>
	</main>
</template>

<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font-family: 'Poppins', sans-serif;
}

body {
	background-color: #061826;
	color: #FFF;
}

.app {
	display: flex;
	flex-direction: column;
	align-items: center;
	padding: 2rem;
	height: 100vh;
}

h1 {
	font-size: 2.25rem;
	margin-bottom: 2rem;
  font-weight: 600;
}

.quiz {
	background-color: #2D848A;
	padding: 1.5rem;
	width: 100%;
	max-width: 680px;
  border-radius: 15px;
}

.quiz-info {
	display: flex;
	justify-content: space-between;
	margin-bottom: 1rem;
}

.quiz-info .question {
	color: #FFF;
	font-size: 1.2rem;
  font-weight: 600;
  max-width: 80%;
}

.quiz-info .score {
	color: #FFF;
	font-size: 1.2rem;
	font-weight: 600;
}

.options {
	margin-bottom: 1rem;
}

.option {
	padding: 1rem;
	display: block;
	background-color: #061826;
	margin-bottom: 0.5rem;
	border-radius: 0.5rem;
	cursor: pointer;
}

.option:hover {
	background-color: #09273d;
}

.option.correct {
	background-color: #2cce7d;
}

.option.wrong {
	background-color: #ff5a5f;
}

.option:last-of-type {
	margin-bottom: 0;
}

.option.disabled {
	opacity: 0.5;
	cursor: not-allowed;
	background-color: #061826;


}

.option input {
	display: none;
}

button {
	appearance: none;
	outline: none;
	border: none;
	cursor: pointer;
	padding: 0.5rem 1rem;
	background-color: #2cce7d;
	color: #061826;
	font-weight: 700;
	text-transform: uppercase;
	font-size: 1.2rem;
	border-radius: 0.5rem;
}

button:disabled {
	opacity: 0.5;
	cursor: not-allowed;
}

h2 {
	font-size: 2rem;
	margin-bottom: 2rem;
	text-align: center;
}

p {
	color: #fff;
	font-size: 1.5rem;
	text-align: center;
}
</style>