<template>
  <v-container fluid>
    <v-btn color="success" @click="openForm" v-if="!form">Add a new restaurant</v-btn>
    <v-row justify="center" v-else>
      <v-col md="4" sm="12">
        <v-card :loading="false" outlined shaped>
          <div class="pa-5">
            <v-form ref="form" v-model="valid" lazy-validation>
              <v-text-field
                v-model="formData.title"
                :counter="20"
                :rules="rules.title"
                label="Restaurant Name"
                required
              />
              <v-text-field v-model="formData.img" label="Restaurant Photo" :rules="rules.img" required />
              <v-row>
                <v-col cols="12" sm="12" md="5">
                  <v-select
                    v-model="formData.rating.star"
                    :rules="rules.rating.star"
                    :items="[0, 1, 2, 3, 4, 5]"
                    label="Restaurant Star"
                    required
                  />
                </v-col>
                <v-col cols="12" sm="12" md="7">
                  <v-text-field
                    v-model="formData.rating.total"
                    :counter="5"
                    :rules="rules.rating.total"
                    label="Total number of comments"
                    required
                  />
                </v-col>
              </v-row>
              <v-textarea
                clearable
                clear-icon="mdi-close-circle"
                label="Description"
                counter
                v-model="formData.description"
              />
              <v-btn class="mt-5" color="success" @click="addNew">Submit Your Restaurant</v-btn>
            </v-form>
          </div>
        </v-card>
      </v-col>
    </v-row>
    <v-row v-if="restaurant" class="selectedRestaurant mt-5">
      <v-col cols="12" sm="12" md="12">
        <v-toolbar color="indigo" dark>
          <v-toolbar-title>Selected Restaurant: {{ restaurant.title }}</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn icon>
            <v-icon @click="clear">mdi-close</v-icon>
          </v-btn>
        </v-toolbar>
        <v-card class="mx-auto my-5" max-width="374">
          <v-img height="250" :src="restaurant.img"></v-img>
          <v-card-title>{{ restaurant.title }}</v-card-title>
          <v-card-text>
            <v-row align="center" class="mx-0">
              <v-rating
                :value="restaurant.rating.star"
                color="amber"
                dense
                half-increments
                readonly
                size="14"
              ></v-rating>
              <div class="grey--text ml-4">{{ restaurant.rating.star }} ({{ restaurant.rating.total }})</div>
            </v-row>
            <div class="my-4">
              {{ restaurant.description }}
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <template v-for="restaurant in restaurants">
        <Restaurant :restaurant="restaurant" :key="restaurant.unique" @selected="selected" />
      </template>
    </v-row>
  </v-container>
</template>

<script>
import { defineComponent, toRefs, reactive } from '@vue/composition-api'
import Restaurant from './Restaurant'

export default defineComponent({
  name: 'Home',
  components: {
    Restaurant
  },
  setup() {
    const state = reactive({
      // if a restaurant is selected, we store its info here
      restaurant: null,
      
      form: false,
      formData: {
        title: null,
        img: null,
        rating: {
          star: null,
          total: null
        },
        description: 'Introduction letter about the restaurant'
      },

      valid: true,
      rules: {
        title: [
          v => !!v || 'Restaurant name is required',
          v => (v && v.length <= 20) || 'Restaurant name must be less than 20 characters'
        ],
        img: [v => !!v || 'Restaurant photo is required'],
        rating: {
          star: [v => !!v || 'Star is required', v => (v && v <= 5 && v >= 0) || 'Star must be selected']
        }
      },

      restaurants: [
        {
          id: 1,
          title: "Çağrı's Patisserie",
          img: 'https://cdn.vuetifyjs.com/images/cards/cooking.png',
          rating: {
            star: 4.9,
            total: 547
          },
          description: 'Lorem Ipsum is simply dummy text of the printing and typesetting industry.'
        },
        {
          id: 2,
          title: 'Starbucks',
          img: 'https://images.pexels.com/photos/597933/pexels-photo-597933.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500',
          rating: {
            star: 3.7,
            total: 987
          },
          description:
            "Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book."
        },
        {
          id: 3,
          title: 'Nusr-et',
          img: 'https://media-cdn.tripadvisor.com/media/photo-s/05/a8/bc/42/getlstd-property-photo.jpg',
          rating: {
            star: 4.12,
            total: 2034
          },
          description:
            'Really enjoyed our meal great steak and the serving was so special, really cool presentation. The taste was amazing, have not enjoyed a steak this much. Great desert as well the ice-cream between the baqlawa. Loved it all.'
        },
        {
          id: 4,
          title: "McDonald's",
          img: 'https://media-cdn.tripadvisor.com/media/photo-s/1a/83/3e/33/img-20200103-221903-mh15782683.jpg',
          rating: {
            star: 2.8,
            total: 32
          },
          description:
            'definitely not up to scratch for a McDonalds branch which will also be serving international visitors, staff can barely speak english let alone understand you.'
        }
      ]
    })

    const selected = restaurant => {
      state.restaurant = restaurant
      state.form = false
      setTimeout(() => {
        document.querySelector('.selectedRestaurant').scrollIntoView()
      }, 0)
    }

    const clear = () => {
      state.restaurant = null
    }
    const clearForm = () => {
      state.formData = {
        title: null,
        rating: {
          star: null,
          total: null
        },
        img: null,
        description: null
      }
    }

    const openForm = () => {
      state.form = true
    }

    const addNew = () => {
      //
      // there will be a validation here ASAP!
      state.valid = false
      const { title, img, rating, description } = state.formData
      if (title && img && description) {
        state.valid = true
      }
      if (state.valid) {
        let index = state.restaurants[state.restaurants.length - 1].id + 1
        state.restaurants.unshift({ index, img, title, rating, description })
        state.form = false
        clearForm()
      }
    }

    return { ...toRefs(state), selected, clear, openForm, addNew, clearForm }
  }
})
</script>

<style lang="scss" scoped>
html {
  scroll-behavior: smooth;
}
</style>
