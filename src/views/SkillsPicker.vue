<template>
  <div class="double-columns-container">
    <form class="skill-picker-form" @submit.prevent="submit">
      <div class="columns-component-div">
        <SkillPickerColumn @skill-added="updateWantedSkills($event)" />
      </div>

      <CalculateCombosButton @click="calculateCombos"
        >Calculate Combos</CalculateCombosButton
      >
    </form>
    <PossibleCombosList :combosList="possibleCombos" />
  </div>
</template>

<script setup>
import SkillPickerColumn from '../components/SkillPickerColumn'
import CalculateCombosButton from '../components/CalculateCombosButton'
import PossibleCombosList from '../components/PossibleCombosList'
import {
  raceObjects,
  playableRaces,
  playableClasses,
  skillsList,
  allCombinations
} from '../tables/skills.js'

const name = 'NewSkillsPicker'
const wantedSkills = ref([])
const allCombos = ref([])
const possibleCombos = ref([])

const updateWantedSkills = (newSkillsList) => {
  wantedSkills.value = newSkillsList
}

const calculateCombos = () => {
  findValidCombos(wantedSkills.value)
}

const skillSelected = (event = {}) => {
  const skillIndex = wantedSkills.value.indexOf(event.target.value)
  if (skillIndex === -1) wantedSkills.value.push(event.target.value)
  else {
    wantedSkills.value.splice(skillIndex, 1)
  }

  possibleCombos.value = findValidCombos(wantedSkills.value)
}

const findValidCombos = (desiredSkills = []) => {
  try {
    const comboPossibilities = []
    allCombos.value.forEach((combination) => {
      if (
        desiredSkills.every(
          (skill) =>
            combination.skills.includes(skill.name) ||
            combination.doubleSkills.includes(skill.name)
        )
      ) {
        comboPossibilities.push(combination)
      }
    })
    possibleCombos.value = comboPossibilities
  } catch (error) {
    console.log('error: ', error)
  }
}

const removeDuplicateStrings = (array) => {
  return array.filter((a, b) => array.indexOf(a) === b)
}

onMounted(() => {
  allCombos.value = allCombinations()
})
</script>

<style scoped>
.double-columns-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.columns-component-div {
  display: flex;
  justify-content: center;
}

.skill-picker-form {
  display: flex;
  flex-direction: column;
  width: 80vw;
  max-width: 400px;
  justify-content: space-between;
}
</style>
