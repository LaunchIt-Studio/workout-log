<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <div class="fit row justify-center">
          <div class="col column justify-center">
            <div class="text-center text-h4 text-weight-regular">Workout Log</div>
          </div>
        </div>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default ({
  name: 'MainLayout'
})
</script>
