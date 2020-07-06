<template>
<div>
    <div  v-for="(question,qIndex) in questions" :key="question.identifier" >
        <div class="quiz-container" v-if="question.question_type=='multiple-choice'" >
            <span>{{qIndex+1}}).</span><label class="block">{{ question.headline }}</label>
            <div v-for="(choice, aIndex) in question.choices" :key="choice[aIndex]">
                <input type="radio" :id="choice[aIndex]" :name="question.identifier+'-'+qIndex" v-model="answers[qIndex]"
                       :value="choice.value">
                <label :for="choice[aIndex]" >{{ choice.label }}</label>
            </div>
        </div>
        <div class="quiz-container"  v-else>
            <span>{{qIndex+1}}).</span><label class="block">{{ question.headline }}</label>
            <div v-if="question.multiline=='true'">
                <textarea v-model="answers[qIndex]" ></textarea>
            </div>
            <div v-else>
                <input v-model="answers[qIndex]" >
            </div>
        </div>
    </div>
    <AppButton
          type="button"
          btn-style="inverted"
          style="text-align:center; border: 2px solid black;border-radius: 8px;"
          @click="onSubmit"> Submit</AppButton>
   
 </div>

</template>


<script>
import AppButton from '@/components/UI/AppButton'

    const getAuthors = () => import('~/assets/questionnaire.json').then(m => m.default || m)

export default {
    middleware: 'authenticated',
  async asyncData ({ req }) {
    const authors = await getAuthors()
    const questions=authors.questionnaire.questions
     let user=window.localStorage.getItem("email")
    // console.log(authors.questionnaire.questions)
    return { questions,user }
  },
  data(){
      return {
          
       answers:[]
      }
  },
  components:{
AppButton
  },
  methods:{
onSubmit(){
    console.log(this.user)
    this.$axios.$post('/api/submit',{userName:this.user,answer:this.answers}).then((result)=>{
        console.log(result)
    }).catch((e)=>{
        console.log(e)
    })
},

  },
  

}
</script>

<style  scoped>

.quiz-container {
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 2px 2px #ccc;
  width: 500px;
  margin: auto;
  padding: 10px;
  box-sizing: border-box;
}
</style>