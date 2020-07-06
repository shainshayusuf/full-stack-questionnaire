<template>
    <div>
        <p>Total number of submissions:{{emails.length}}</p>
        <table >
            <thead>
				<tr>
					<th> User</th>
					<th v-for="(answer,aIndex) in answers[0]" :key="aIndex">Question {{aIndex+1}}</th>
				</tr>
			</thead>
            <tr v-for="(email,eIndex) in emails" :key="eIndex">
                <td>{{email}}</td>
                <td v-for="(answer,aIndex) in answers[eIndex]" :key="aIndex">
                    {{answer}}
                </td>
            </tr>
        </table>
        
    </div>
</template>


<script>
export default {
    layout:'admin',
     data(){
      return {
          emails:[],
          answers:[],
          arrayLength:null
      }
  },
    methods:{
 onSuccess(response) {
    var array = response;
  //  var arraytobe = response;
  this.arrayLength = Object.keys(array).length 
   this.answers = Object.values(array).map(e=>e.answers);
    this.emails = Object.values(array).map(e=>e.email);
    console.log(this.answers)
   

}
    },
     async created(){
        await this.$axios.$get('/api/guest').then((response)=>{
            this.onSuccess(response)
        })
    }
}
</script>

<style  scoped>
table, th, td {
  border: 1px solid black;
}
</style>