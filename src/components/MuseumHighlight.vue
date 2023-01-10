<template>
  <div class="museum-highlight">    
    <article class="card">
      <slot name="badge" />         
      
      <div v-if="item.image">
        <img 
          :src="item.image"
          class="card__img"                    
        />
      </div>
      <div v-else>
        <img 
          src="@/assets/asteroids.png"  
          class="card__img"                    
        />
      </div>
      
      <div
        :class="{
          'branding-partner': item.isPartner,
          'branding-museum': !item.isPartner
        }"
      >
        {{ item.isPartner ? "Partner" : "Museum" }}
      </div>

      <div class="card__info">
        <h3
          v-if="item.name"
          class="card__title"
        >
          {{ item.name }}
        </h3>

        <div
          v-if="item.description"
          class="card__category"
        >
          {{ item.description }}
        </div>
        <div
          v-if="item.news"
          class="news-block"
        >
          <span
            v-if="item.news.title"
            class="news-title"
          >
            {{ item.news.title }}
          </span>
          <span
            v-if="item.news.date"
            class="news-date"
          >
            {{ item.news.date }}
          </span>
        </div>      

        <slot name="special" />
      </div>

      <div
        v-if="item.date"
        class="card-meta"
      >
        {{ item.date }}		        
      </div>
    </article>
  </div>
</template>

<script>

export default {
  name: 'MuseumHighlight',

  props: {
    item: {
      type: Object,
      required: true
    }
  }
}
</script>

<style lang="scss" scoped>
.card__img {
  width: 100%;
  height: 185px;
}

.card {
  position: relative;
  transition: all .4s cubic-bezier(0.175, 0.885, 0, 1);
  background-color: #fff;
  width: 335px;
  border-radius: 12px;
  height: 450px;
  overflow: hidden;
  box-shadow: 0px 13px 10px -7px rgba(0, 0, 0,0.1);
}

.card-meta {
	color: rgb(182, 179, 179);
	font-size: .78em;
  visibility: hidden;
}

.card:hover {
  box-shadow: 0px 30px 18px -8px rgba(0, 0, 0,0.1);
  transform: scale(1.10, 1.10);

  .card-meta{
    visibility: visible;    
  }
 }

.card__info {
  z-index: 2;
  background-color: #fff;
  padding: 0px 15px 15px 15px;
}

.card__category {
  text-transform: uppercase;
  font-size: 13px;
  font-weight: 500;
  color: #868686;
}

.card__title {
  margin: 0px 0px 5px 0px;
}

.branding-partner{
  background: rgb(226, 177, 102);
  margin-bottom: 5px;
}

.branding-museum{
  background: rgb(106, 106, 219);
  margin-bottom: 5px;
}

.news-block{
  margin: 15px;

  .news-title{
    font-size: 13px;
  }

  .news-date{
    font-size: 10px;
    margin: 10px;
    color: grey;
  }
}
</style>