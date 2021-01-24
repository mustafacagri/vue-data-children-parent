<template>
  <v-col cols="12" sm="12" md="3">
    <v-card :loading="loading" class="mx-auto mt-5" max-width="374">
      <template slot="progress">
        <v-progress-linear color="deep-purple" height="10" indeterminate></v-progress-linear>
      </template>
      <v-img height="250" :src="restaurant.img"><div class="fill-height repeating-gradient"></div></v-img>
      <v-card-title>{{ restaurant.title }}</v-card-title>
      <v-card-text>
        <v-row align="center" class="mx-0">
          <v-rating :value="restaurant.rating.star" color="amber" dense half-increments readonly size="14"></v-rating>
          <div class="grey--text ml-4">{{ restaurant.rating.star }} ({{ restaurant.rating.total }})</div>
        </v-row>
        <div class="mt-4">
          {{
            restaurant.description.length > 40
              ? restaurant.description.substring(0, 40) + '...'
              : restaurant.description
          }}
        </div>
      </v-card-text>
      <v-card-actions>
        <v-btn color="success" text @click="select">
          Select
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-col>
</template>

<script>
import { defineComponent, reactive, toRefs } from '@vue/composition-api'

export default defineComponent({
  name: 'Restaurant',
  props: ['restaurant'],
  emits: ['select'],
  setup(props, { emit }) {
    const state = reactive({
      loading: false
    })

    const select = () => {
      emit('selected', props.restaurant)
    }

    return { ...toRefs(state), select }
  }
})
</script>

<style lang="scss" scoped>
.repeating-gradient {
  background-image: repeating-linear-gradient(
    -45deg,
    rgba(255, 0, 0, 0.15),
    rgba(255, 0, 0, 0.15) 5px,
    rgba(0, 0, 255, 0.15) 5px,
    rgba(0, 0, 255, 0.15) 10px
  );
}
</style>
