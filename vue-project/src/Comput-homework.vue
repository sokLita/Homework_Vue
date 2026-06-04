<template>
    <div>
       <h1>Student Score Tracking</h1>
       <p>Average of student: {{ average }}</p>
       <p>Highest of student:{{ highestScore }}</p>
       <p>Lowest of student:{{ lowestScore }}</p>
       <p>Student's Grade: {{ grade }}</p>
       <hr>
       <p>Summary:  ​{{ studentName }} | {{ average }} | {{ grade }} | {{ isPassing }}</p>
    </div>
</template>

<script setup>
import { ref, computed} from 'vue'
const studentName = ref('Lita')
const scores = ref([
    {student: 'English', score: 50},
    {student: 'PL', score: 75},
    {student: 'Vue.Js', score: 80},
    {student: 'Laravel', score: 90}
   
])
const passMark = ref(50);

const average = computed(()=>{
    let total = 0;
    scores.value.forEach(student =>{
        total += student.score 
    })
    return total /scores.value.length
}) 
const highestScore = computed(()=>{
    let highest = scores.value[0].score
    scores.value.forEach(subject=>{
        if (subject.score > highest){
            highest = subject.score;
        }
           
    })
    return highest
})
const lowestScore = computed(()=>{
    let lowest = scores.value[0].score
    scores.value.forEach(subject=>{
        if (subject.score < lowest){
            lowest = subject.score;
        }
           
    })
    return lowest
})
const grade = computed(() => {
  let totals = 0
  scores.value.forEach(student => {
    totals += student.score
  })

  if (totals >= 90)      return 'A'
  else if (totals >= 80) return 'B'
  else if (totals >= 70) return 'C'
  else if (totals >= 60) return 'D'
  else if (totals >= 50) return 'E'
  else                   return 'F'
})
const isPassing = computed (() =>{
    if ( average.value > passMark.value){
        return "Passing"
     }else {
        return "Failing"
     }
})
</script>

<style lang="scss" scoped>

</style>