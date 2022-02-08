<template>
  <div class="row justify-center">
    <div class="col-4">
      <div :class="`${currentSet ? 'ob-a' : 'bb-a'} q-ma-xs`">
        <q-select
          dense
          dark
          readonly
          borderless
          class="fit q-px-none q-ma-none text-h6"
          :bottom-slots="false"
          :hide-bottom-space="true"
          :hide-dropdown-icon="true"
          :no-error-icon="true"
          :hide-hint="true"
          prefix="Set"
          v-model="setNumber"
        >
          <template v-slot:prepend>
            <q-icon class="q-px-xs" name="drive_file_rename_outline" />
          </template>
        </q-select>
    </div>
    </div>
    <div class="col-4">
      <div :class="`${currentSet ? 'ob-a' : 'bb-a'} q-ma-xs`">
        <q-select
          dense
          dark
          :readonly="!currentSet"
          borderless
          class="fit q-px-none q-ma-none text-h6"
          :bottom-slots="false"
          :hide-bottom-space="true"
          :hide-dropdown-icon="true"
          :no-error-icon="true"
          :hide-hint="true"
          v-model="setWeight"
          @update:model-value="updateWeight(setWeight)"
          :options="weightOptions"
        >
          <template v-slot:prepend>
            <q-icon class="q-px-xs" name="fitness_center" />
          </template>
        </q-select>
    </div>
    </div>
    <div class="col-4">
      <div :class="`${currentSet ? 'ob-a' : 'bb-a'} q-ma-xs`">
        <q-select
          dense
          dark
          :readonly="!currentSet"
          borderless
          class="fit q-px-none q-ma-none text-h6"
          :bottom-slots="false"
          :hide-bottom-space="true"
          :hide-dropdown-icon="true"
          :no-error-icon="true"
          :hide-hint="true"
          v-model="setReps"
          @update:model-value="updateReps(setReps)"
          :options="repOptions"
        >
          <template v-slot:prepend>
            <q-icon class="q-px-xs" name="autorenew" />
          </template>
        </q-select>
      </div>
    </div>
  </div>
</template>

<script>
export default ({
  name: 'Set',
  data () {
    return {
      setWeight: 0,
      setReps: 0,
      weightOptions: [0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95, 100],
      repOptions: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20]
    }
  },
  props: {
    currentSet: Boolean,
    idx: Number,
    set: Object
  },
  methods: {
    updateWeight (value) {
      this.$emit('updateSet', { type: 'weight', weight: value })
    },
    updateReps (value) {
      this.$emit('updateSet', { type: 'reps', reps: value })
    }
  },
  mounted () {
    if (this.set && this.set.reps) {
      this.setReps = this.set.reps
    }
    if (this.set && this.set.weight) {
      this.setWeight = this.set.weight
    }
  },
  updated () {
    if (this.set && this.set.reps) {
      this.setReps = this.set.reps
    }
    if (this.set && this.set.weight) {
      this.setWeight = this.set.weight
    }
  },
  computed: {
    setNumber () {
      return this.idx + 1
    }
  }
})
</script>
