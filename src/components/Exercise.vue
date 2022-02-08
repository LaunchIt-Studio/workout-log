<template>
  <div class="row justify-center">
    <div class="col-12">
      <div class="ob-a q-ma-xs">
        <q-select
          dense
          dark
          borderless
          class="fit q-px-none q-ma-none text-h6"
          :bottom-slots="false"
          :hide-bottom-space="true"
          :hide-dropdown-icon="true"
          :no-error-icon="true"
          :hide-hint="true"
          label="Exercise"
          v-model="selectedExercise"
          @update:model-value="updateExercise(selectedExercise)"
          option-label="name"
          :options="exercisesForTargetMuscle"
        >
          <template v-slot:prepend>
            <q-icon class="q-px-xs" name="directions_run" />
          </template>
        </q-select>
      </div>
    </div>
  </div>
</template>

<script>
import { date } from 'quasar'
import exercises from 'src/assets/exercises.json'
export default ({
  name: 'Exercise',
  data () {
    return {
      selectedExercise: null,
      exercises: exercises.exercises
    }
  },
  props: {
    exercise: Object
  },
  computed: {
    targerMuscle () {
      const dayOfWeek = parseInt(date.formatDate(Date.now(), 'd'))
      if (dayOfWeek === 0) {
        return ['hamstrings', 'quadriceps', 'calves', 'glutes']
      } else if (dayOfWeek === 1) {
        return ['shoulders', 'neck', 'traps']
      } else if (dayOfWeek === 2) {
        return ['lats', 'lower back', 'middle back']
      } else if (dayOfWeek === 3) {
        return ['chest']
      } else if (dayOfWeek === 4) {
        return ['triceps', 'biceps', 'forearms']
      } else if (dayOfWeek === 5) {
        return ['abdominals', 'adductors', 'abductors']
      } else if (dayOfWeek === 6) {
        return ['']
      } else {
        return ['']
      }
    },
    exercisesForTargetMuscle () {
      const list = this.exercises.filter(ex => this.targerMuscle.includes(ex.primaryMuscles[0]))
      return list
    }
  },
  methods: {
    updateExercise (selectedExercise) {
      this.$emit('updateExercise', selectedExercise)
    }
  },
  updated () {
    if (this.exercise && this.exercise.name) {
      this.selectedExercise = this.exercise.name
    } else {
      this.selectedExercise = null
    }
  }
})
</script>
