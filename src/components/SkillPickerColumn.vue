<template>
  <div class="column-div">
    <h3>Choose Your Skills:</h3>
    <SkillChoiceModal @skill-chosen="showChosen" />
    <ul>
      <li v-for="(skill, index) in chosenSkills" :key="`${skill.name}${index}`">
        <span class="skill-name-span">{{ skill.name }}</span
        ><font-awesome-icon
          id="minus"
          icon="minus-square"
          @click="removeSkill(index)"
        />
      </li>
    </ul>
  </div>
</template>

<script setup>
import SkillChoiceModal from './modals/SkillChoiceModal'

const chosenSkills = ref([])

const showChosen = (chosenSkill) => {
  if (
    chosenSkills.value.filter((skill) => skill.name === chosenSkill).length ===
    0
  ) {
    chosenSkills.value.push({ name: chosenSkill })
  }
  emit('skill-added', chosenSkills.value)
}

const removeSkill = (skillToRemove) => {
  chosenSkills.value.splice(skillToRemove, 1)
  emit('skill-added', chosenSkills.value)
}
</script>

<style scoped>
#minus {
  font-size: 20px;
  padding-left: 25px;
}

#minus:hover {
  color: darkred;
  cursor: pointer;
}
.skill-name-span {
  font-size: 24px;
}
.skill-name-span:hover {
  text-decoration: 2px black underline;
  cursor: pointer;
}
.column-div {
  display: flex;
  flex-direction: column;
}
ul {
  list-style-type: none;
  padding-left: 0;
}

li {
  padding-top: 10px;
}
</style>
