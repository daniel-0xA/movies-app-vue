<template>
  <div class="movie">
    <img :src="posterPath" :alt="title">
    <div class="movie-info">
      <h3>{{title}}</h3>
      <span :class="`tag ` + voteClass(vote_average)">{{vote_average}}</span>
    </div>
    <div class="movie-overview">
      <h3>{{title}}</h3>
      <p>{{overview}}</p>
    </div>
  </div>
</template>

<script>
const IMG_API = "https://image.tmdb.org/t/p/w1280";
const IMG_REPLACEMENT = "https://images.unsplash.com/photo-1440404653325-ab127d49abc1"
  export default {
    name: "Movie",
    props:{
      title:String,
      overview: String,
      poster_path: String,
      vote_average: Number
    },
    setup(props){
      const posterPath = props.poster_path ? IMG_API + props.poster_path : IMG_REPLACEMENT
      const voteClass = (vote) => {
        if(vote >= 8)
          return "green"
        else if(vote >= 6)
          return "orange"
        else return "red"
      }
      return{posterPath, voteClass}
    }
  }
</script>

<style scoped>
.movie {
  width: 250px;
  margin: 0.3rem;
  background-color: orange;
  border-radius: 5px;
  overflow: hidden;
  position: relative;
}
.movie img {
  width: 100%;
  height: 350px;
  object-fit: cover;
}
.movie-info {
  padding: 0.5rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.movie-info h3 {
  margin: 0;
}
.movie-overview {
  background-color: white;
  color: orangered;
  padding: 0.5rem;
  position: absolute;
  bottom: 0;
  right: 0;
  left: 0;
  transform: translateY(103%);
  transition: transform 0.3s ease-in-out;
}
.movie:hover .movie-overview {
  transform: translateY(0);
}
.tag {
  background-color: black;
  padding: 0.3rem;
  border-radius: 5px;
  font-weight: bold;
}
.tag.green {
  color: green;
}
.tag.orange {
  color: orange;
}
.tag.red {
  color: red;
}
</style>