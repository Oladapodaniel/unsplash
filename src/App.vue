<template>
<Suspense>
      <template #default>
        <Unsplash />
      </template>
      <template #fallback>
        <div class="loader-search-bg">
          <div class="loader-search"></div>
        </div>
        <div class="parent">
          <div class="grid">
          <div>
            <div class="loader-image"></div>
            <div class="loader-text"></div>
            <div class="loader-text two"></div>
          </div>
          <div>
            <div class="loader-image"></div>
            <div class="loader-text"></div>
            <div class="loader-text two"></div>
          </div>
          <div>
            <div class="loader-image"></div>
            <div class="loader-text"></div>
            <div class="loader-text two"></div>
          </div>
          <div>
            <div class="loader-image"></div>
            <div class="loader-text"></div>
            <div class="loader-text two"></div>
          </div>
          <div>
            <div class="loader-image"></div>
            <div class="loader-text"></div>
            <div class="loader-text two"></div>
          </div>
          <div>
            <div class="loader-image"></div>
            <div class="loader-text"></div>
            <div class="loader-text two"></div>
          </div>
          </div>
        </div>
      </template>
</Suspense>
</template>




<script>
import Unsplash from './components/Unsplash.vue'

export default {
  name: 'App',
  components: {
    Unsplash,    
  }
}
</script>

<style scoped>


        .grid {
            display: grid;
            grid-template-columns: auto auto auto;
            grid-gap: 30px;
        }

        .grid > div {
          border: none
        
        }

         @keyframes pulse-bg {
            0% { background-color: #ddd; }
            50% { background-color: #d0d0d0; }
            100% { background-color: #ddd; }
          }


        div .loader-image {
            width: 100%;
            border-radius: 10px;
            height: 15em;
            background: #f5f5f5;
            
        }
        .parent {
            width: 70%;
            margin: 13.5em 10em;
        }

        .loader-text {
            transform: translate(0%, -350%);
            position: absolute;
            margin-left: 10px;
            width: 200px;
            height: 15px;
            background: #e6e6e6e6;
            animation: pulse-bg 1s infinite;
        }

         .loader-text.two {
            font-size: 0.8em;
            transform: translate(0%, -350%);
            margin-top: 20px;
            width: 150px;
        }

        .parent .grid > div:nth-child(2) > div:first-child {
          height: 130%
        }

        .parent .grid > div:nth-child(5) > div:first-child{
          margin-top: 4.5em;
        }

       
        
         .loader-search-bg {
            border: 2px solid green;
            width: 100%;
            padding: 100px;
            background-color: #DDE2E9;
        }

        .loader-search {
            width: 70%;
            height: 30px;
            border: 2px solid pink;
            animation: pulse-bg 1s infinite;
        }
</style>
