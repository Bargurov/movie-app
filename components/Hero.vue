<template>
  <div class="hero">
    <img src="../assets/imgs/movieHero.jpg" alt="" />
    <div class="text-container">
      <div class="text">
        <span class="mini-heading">Now Streming</span>
        <h1><span>movie</span> Zone</h1>
        <!-- <a href="#movie-grid" class="button">View Movies</a> -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
 name:"hero",
}
</script>


<style lang="scss" scoped>
.hero {
  height: 40rem;
  position: relative;
  @media (min-width: 75rem) {
    height: 50rem;
  }
  &::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.6);
  }
  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  .text-container {
    z-index: 99;
    position: absolute;
    top: 0;
    margin: 0;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    .text {
      padding: 0 16px;
      width: 100%;
      max-width: 140rem;
      margin: 0 auto;
    }
    .mini-heading {
      font-weight: 600;
      font-size: 1.8rem;
      text-transform: uppercase;
      color: #c92502;
      margin-bottom: .8rem;
      @media (min-width: 75rem) {
        font-size: 2.2rem;
      }
    }
    h1 {
      color: #fff;
      font-size: 6.5rem;
      font-weight: 200;
      margin-bottom: 8px;

      @media (min-width: 750px) {
        font-size: 8.5rem;
      }

      span {
          font-size: 8rem;
        font-weight: 500;
      }
    }
    .button {
      font-size: 6rem;
      align-self: flex-start;
    }
  }
}
</style>