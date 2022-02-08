<template>
  <q-page dark class="">
    <div class="fit row justify-center">
      <div class="q-my-xs col-xs-12 col-md-7 column justify-center">
        <q-card dark flat square class="fit q-pa-none q-ma-none">
          <q-card-section class="q-pa-none q-ma-none">
            <div class="row justify-center">
              <div class="col-12">
                <div class="bb-a q-ma-xs q-px-xs text-h6">
                  <q-input
                    dark
                    dense
                    :readonly="workoutLoaded"
                    borderless
                    class="text-h6"
                    prefix="User:"
                    v-model="user"
                    type="tel"
                    inputmode="numeric"
                    mask="(###) ### - ####"
                    unmasked-value
                    placeholder="(###) ### - ####"
                  />
                </div>
              </div>
            </div>
          </q-card-section>
          <q-card-section class="q-pa-none q-ma-none">
            <div class="row justify-center">
              <div class="col-6">
                <div class="bb-a q-ma-xs q-px-xs text-h6">
                  Date: <a class="text-h6 text-weight-regular">{{todaysDate}}</a>
                </div>
              </div>
              <div class="col-6">
                <div class="bb-a q-ma-xs q-px-xs text-h6">
                  Start: <a class="text-h6 text-weight-regular">{{startTime}}</a>
              </div>
              </div>
            </div>
          </q-card-section>
          <q-card-section class="q-pa-none q-ma-none">
            <div class="row justify-center">
              <div class="col-12">
                <div class="bb-a q-ma-xs q-px-xs text-h6">
                  Target Muscle: <a class="text-h6 text-weight-regular">{{workout.target_muscle}}</a>
              </div>
              </div>
            </div>
          </q-card-section>
           <q-card-section v-show="!workoutLoaded" class="q-pa-none q-ma-none">
            <div class="row justify-center">
              <div class="col-12">
                <div class="bg-positive pb-a q-ma-xs">
                  <q-btn
                    flat
                    no-caps
                    dense
                    class="q-pa-none text-h8 text-center fit"
                    :loading="loadingWorkout"
                    label="Start Workout"
                    @click="loadWorkout"
                  />
                </div>
              </div>
            </div>
          </q-card-section>
        </q-card>
      </div>
      <div v-show="workoutLoaded" class="q-my-md col-xs-12 col-md-7 column justify-center">
        <q-card dark flat square class="fit q-pa-none q-ma-none">
          <q-card-section class="q-pa-none q-ma-none">
            <Exercise :exercise="currentExercise" @updateExercise="updateExercise"/>
          </q-card-section>
          <q-card-section class="q-pa-none q-ma-none" style="max-height: 225px;overflow: auto !important">
            <div
              v-for="(set, idx) in currentExercise.sets"
              :key="idx"
            >
              <Set :set="set" :idx="idx" :currentSet="set_idx === idx" @updateSet="updateSet"/>
            </div>
          </q-card-section>
          <q-card-section class="q-pa-none q-ma-none">
            <div class="row justify-center">
              <div class="col-6">
                <div class="bg-secondary rb-a q-ma-xs">
                  <q-btn
                    flat
                    no-caps
                    dense
                    text-color="dark"
                    class="q-pa-none text-h8 text-center fit"
                    icon="remove"
                    @click="deleteSet"
                  />
                </div>
              </div>
              <div class="col-6">
                <div class="bg-secondary rb-a q-ma-xs">
                  <q-btn
                    flat
                    no-caps
                    dense
                    text-color="dark"
                    class="q-pa-none text-h8 text-center fit"
                    icon="add"
                    @click="addSet"
                  />
                </div>
              </div>
            </div>
          </q-card-section>
        </q-card>
      </div>
      <div v-show="workoutLoaded" class="q-my-xs col-xs-12 col-md-7 column justify-center"
        style="position: fixed;bottom: 0px;"
      >
        <q-card dark flat square class="fit q-pa-none q-ma-none">
          <q-card-section class="q-pa-none q-ma-none">
            <div class="row justify-center">
              <div
                v-for="(set, idx) in workout.exercises"
                :key="idx"
              >
                <div class="col">
                  <div :class="`bg-primary ${idx === exercise_idx ? 'bb-a' : 'ob-a'} q-ma-xs`">
                    <q-btn
                      flat
                      no-caps
                      dense
                      class="q-px-sm text-caption text-center fit"
                      :label="idx + 1"
                      @click="updateIdx(idx)"
                    />
                  </div>
                </div>
              </div>
            </div>
            <div class="row justify-center">
              <div class="col-6">
                <div class="bg-secondary rb-a q-ma-xs">
                  <q-btn
                    flat
                    no-caps
                    dense
                    class="q-pa-none text-h8 text-center fit"
                    label="Delete EX"
                    @click="deleteExcercise"
                  />
                </div>
              </div>
              <div class="col-6">
                <div class="bg-secondary rb-a q-ma-xs">
                  <q-btn
                    flat
                    no-caps
                    dense
                    class="q-pa-none text-h8 text-center fit"
                    label="Add EX"
                    @click="addExcercise"
                  />
                </div>
              </div>
            </div>
            <div class="row justify-center">
              <div class="col-12">
                <div class="bg-negative nb-a q-ma-xs">
                  <q-btn
                    flat
                    no-caps
                    dense
                    class="q-pa-none text-h8 text-center fit"
                    label="End Workout"
                  />
                </div>
              </div>
            </div>
          </q-card-section>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script>
import { date } from 'quasar'
import Set from 'components/Set'
import Exercise from 'components/Exercise'
export default ({
  name: 'PageIndex',
  data () {
    return {
      apiURL: 'https://xfvqik0d56.execute-api.us-east-1.amazonaws.com/Prod',
      user: '',
      loadingWorkout: false,
      workoutLoaded: false,
      exercise_idx: 0,
      set_idx: 0,
      weightOptions: [5, 10, 15, 20, 25, 30, 35, 40, 45, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95, 100],
      repOptions: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20],
      schedule: [
        'Legs',
        'Shoulders',
        'Back',
        'Chest',
        'Arms',
        'Core',
        'REST'
      ],
      workout: {
        date: Date.now(),
        start_time: null,
        end_time: null,
        target_muscle: null,
        exercises: [
          {
            name: null,
            sets: []
          }
        ]
      }
    }
  },
  components: {
    Exercise,
    Set
  },
  methods: {
    updateIdx (idx) {
      this.exercise_idx = idx
      this.set_idx = (this.workout.exercises[this.exercise_idx].sets.length) - 1
    },
    addSet () {
      const exerciseName = this.workout.exercises[this.exercise_idx].name
      const exerciseSets = this.workout.exercises[this.exercise_idx].sets
      if (exerciseName && (exerciseSets.length === 0 || exerciseSets[this.set_idx].reps)) {
        this.workout.exercises[this.exercise_idx].sets.push({ reps: 0, weight: 0 })
        this.set_idx = (this.workout.exercises[this.exercise_idx].sets.length) - 1
      }
    },
    deleteSet () {
      if (this.workout.exercises[this.exercise_idx].sets.length > 1) {
        this.workout.exercises[this.exercise_idx].sets.pop()
        this.set_idx = (this.workout.exercises[this.exercise_idx].sets.length) - 1
        this.updateLog()
      }
    },
    updateSet (update) {
      if (update.type === 'weight') {
        this.workout.exercises[this.exercise_idx].sets[this.set_idx].weight = update.weight
        this.updateLog()
      } else if (update.type === 'reps') {
        this.workout.exercises[this.exercise_idx].sets[this.set_idx].reps = update.reps
        this.updateLog()
      }
    },
    addExcercise () {
      const exercise = {
        name: null,
        sets: []
      }
      if (this.workout.exercises[this.exercise_idx].sets.length >= 1) {
        this.workout.exercises.splice(this.exercise_idx + 1, 0, exercise)
        this.exercise_idx = this.exercise_idx + 1
        this.updateLog()
      }
    },
    deleteExcercise () {
      if (this.workout.exercises.length > 1) {
        this.workout.exercises.splice(this.exercise_idx, 1)
        this.exercise_idx = (this.workout.exercises.length) - 1
        this.updateLog()
      }
    },
    updateExercise (update) {
      this.workout.exercises[this.exercise_idx].name = update.name
    },
    updateLog () {
      const body = {
        phone: this.user,
        timestamp: this.workout.date,
        exercises: this.workout.exercises
      }
      return this.$axios.post(`${this.apiURL}/workout-log/update`, body)
        .then((response) => {
          if (response.status === 200) {
            console.log(response)
          }
        })
    },
    createLog () {
      this.workout.date = Date.now()
      this.workout.start_time = Date.now()
      const body = {
        phone: this.user,
        timestamp: this.workout.date,
        log: this.workout
      }
      return this.$axios.post(`${this.apiURL}/workout-log/create`, body)
        .then((response) => {
          if (response.status === 200) {
            // if (response.data.payload.log) {
            //   this.workout = { ...response.data.payload.log }
            //   this.exercise_idx = 0
            //   this.set_idx = (this.workout.exercises[this.exercise_idx].sets.length) - 1
            // }
            return response.data.payload.log
          }
        })
    },
    getLog () {
      const body = {
        phone: this.user,
        timestamp: this.workout.date
      }
      return this.$axios.post(`${this.apiURL}/workout-log/get`, body)
        .then((response) => {
          if (response.status === 200) {
            // if (response.data.payload.log) {
            //   this.workout = { ...response.data.payload.log }
            //   this.exercise_idx = 0
            //   this.set_idx = (this.workout.exercises[this.exercise_idx].sets.length) - 1
            // }
            return response.data.payload.log
          }
        })
    },
    loadWorkout () {
      this.loadingWorkout = true
      this.getLog().then((log) => {
        if (log) {
          this.workout = { ...log }
          this.exercise_idx = 0
          this.set_idx = (this.workout.exercises[this.exercise_idx].sets.length) - 1
          this.loadingWorkout = false
          this.workoutLoaded = true
        } else {
          this.createLog().then((log) => {
            this.workout = { ...log }
            this.exercise_idx = 0
            this.set_idx = (this.workout.exercises[this.exercise_idx].sets.length) - 1
            this.loadingWorkout = false
            this.workoutLoaded = true
          })
        }
      })
    }
  },
  computed: {
    todaysDate () {
      const dateString = date.formatDate(this.workout.date, 'MM/DD/YY')
      return dateString
    },
    startTime () {
      if (this.workout.start_time) {
        const timeString = date.formatDate(this.workout.start_time, 'hh:mm A')
        return timeString
      } else {
        return '00:00'
      }
    },
    currentExercise () {
      return this.workout.exercises[this.exercise_idx]
    }
  },
  mounted () {
    const dayOfWeek = parseInt(date.formatDate(this.workout.date, 'd'))
    this.workout.target_muscle = this.schedule[dayOfWeek]
  }
})
</script>
