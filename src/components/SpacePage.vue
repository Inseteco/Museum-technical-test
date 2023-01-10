<template>
  <div class="space-page">
    <h1 class="space-page__title">
      Space
    </h1>   

    <div class="cards">

      <div v-if="loading">
        Loading...
      </div>

      <MuseumHighlight
        v-else
        v-for="(item, index) in cards"
        :item="item"
        :key="index"
      >
        <template v-slot:badge>
          <img 
            src="@/assets/icons/star.png"
            class="card_badge"       
          /> 
        </template>

        <!-- Slots for unique content -->
        <template v-slot:special>
          <div v-if="item.quiz">
            Take a quiz
            <a
              v-text="item.quiz"
              :src="item.quiz"
            />
          </div>
        </template>
      </MuseumHighlight>
    </div>   
  </div>
</template>

<script>
import { sortBy } from 'lodash'

import MuseumHighlight from '@/components/MuseumHighlight.vue'

export default {
  name: 'SpacePage',

  components: {
    MuseumHighlight
  },

  data: () => ({
    loading: true,

    spaceHighlights: [
      {
        date: '2020-04-20 12:20:00',
        description: 'Asteroids are minor planets, especially of the inner Solar System. Larger asteroids have also been called planetoids.',
        id: 1,
        image: '',
        name: 'Asteroids'
      },
      {
        date: '2020-05-20 15:50:00',
        description: 'A comet is an icy, small Solar System body that, when passing close to the Sun, warms and begins to release gases, a process called outgassing.',
        id: 9,
        image: '',
        name: 'Comets'
      },
      {
        date: '2020-05-01 9:22:00',
        description: 'The term planet is ancient, with ties to history, astrology, science, mythology, and religion.',
        id: 7,
        image: '',
        name: 'Planets',
        news: {
          date: '2020-08-18 18:00:00',
          title: 'Attend our talk about Jupiter with Dr. Hogarth'
        },
        quiz: 'https://planetquiz.space'
      },
      {
        date: '2020-07-05 4:10:00',
        description: 'A meteor shower is a celestial event in which a number of meteors are observed to radiate, or originate, from one point in the night sky.',
        id: 12,
        image: '',
        name: 'Meteor showers',
        news: {
          title: 'The Lyrids will peak at on April 21-22 2021, at night'
        }
      }
    ],
    spacePartners: {
      observatory: {
        createdAt: '2020-06-01 11:45:00',
        info: 'The Mauna Kea Observatories (MKO) are a number of independent astronomical research facilities and large telescope observatories that are located at the summit of Mauna Kea on the Big Island of Hawaiʻi, United States.',
        image: '',
        name: 'Mauna Kea Observatories'
      }
    }
  }),

  computed: {
    cards() {
      // Basic data
      let data = this.spaceHighlights.map(e => {
        e.isPartner = false
        return e
      })
      
      // Partners data
      Object.entries(this.spacePartners).forEach(e => {
        let partner = e[1]

        let fromattedPartner = {
          date: partner.createdAt,
          description: partner.info,
          image: partner.image,
          name: partner.name,
          isPartner: true
        }

        if (partner.news) {
          fromattedPartner.news = partner.news
        }

        if (partner.quiz) {
          fromattedPartner.quiz = partner.quiz
        }

        data.push(fromattedPartner)
      })

      // Sorting from new to old
      data = sortBy(data, [(e) => -new Date(e.date).getTime()])

      return data
    }
  },

  mounted() {
    // Pretend the loading from backend

    this.loading = true

    setTimeout(() => {
      this.loading = false
    }, 700)
  }
}
</script>

<style lang="scss" scoped>
.space-page {
  &__title {
    color: #2c3791;
    font-size: 24px;
    font-weight: 600;
  }
}

.cards {
  padding: 30px;
  display: grid;
  gap: 30px;
  grid-template-columns: 30em 30em 30em;
}

.card_badge{
  position: absolute;
  height: 50px;
  width: 50px;
  top: 0;
  right: 0;
}
</style>
