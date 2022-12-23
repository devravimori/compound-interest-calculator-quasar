<template>
  <q-page class="q-mb-lg">
    <div class="flex justify-center">
      <div class="q-pa-md col-xs-12">
        <h1 class="text-center">About Us</h1>
        <div class="q-mx-auto" style="width: 75%;">
         <q-card flat bordered class="my-card col-xs-12">
          <q-card-section>
            <!-- <div class="text-h6">About Us</div> -->
          </q-card-section>

          <q-card-section class="q-pt-none">
            <p>
              We are <a href="https://optimumitapps.com">OptimumItapps</a> from India, we are the team of developers well experienced in Laravel, Vue, React, Angular and Quasar.
            </p>

            <p>
              We like to develop the Useful websites in our favourite programming languages. we belive in the inclusive and open minded environment and culture for programmers.
            </p>
          </q-card-section>

          <q-separator inset />
        </q-card>
        </div>
      </div>
    </div>

  </q-page>
</template>

<script setup>
import { useQuasar } from 'quasar'
import { ref, reactive } from 'vue'
import { computed } from '@vue/reactivity';


const $q = useQuasar()

</script>
<style script>
h1{
  font-size: xxx-large;
}
.w-50 {
  width: 50%;
}
</style>
