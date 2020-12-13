<template>
    <div class="splash" @click="modal">
        <a href="#">
        <div class="search" >
            <i class="fa fa-search icon" v-if="!inpSearching" aria-hidden="true"></i>
            <input type="text" @keyup.enter="searchQuery" v-if="!inpSearching" placeholder="Search for a photo" class="input"  v-model="searchText">
            <div v-else-if="searched" class="text-header">Search results For <span class="query">"{{ searchText }}"</span></div>
            <div class="text-header" v-else>Searching for <span class="query">"{{ searchText }}"</span></div>
            <div v-show="!searched && !searching" class="cover"></div>
        </div>
        
        <div v-if="searching">
            <div class="loader-parent">
                <div class="grid loader">
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
        </div>
        <div v-else-if="searched">
          <div class="parent">
                <div v-for="search in searchPhoto" :key="search.id" class="grid">
                    <div class="grid-parent">
                      <div class="grid">
                        <div>
                            <a class="imgg photobox" id="images" :href="'#' + search.id">
                                <img :src="search.cover_photo.urls.regular" />
                            </a>
                            <div class="text">{{search.cover_photo.user.name}}</div>
                            <div class="text two">{{search.cover_photo.user.location }}</div> 
                                <div class="photobox-target" :id="search.id">
                                    <img :src="search.cover_photo.urls.regular"/>
                                    <a class="photobox-close" href="#"></a>
                                    <div class="modal-text">
                                        <div>{{search.cover_photo.user.name}}</div>
                                        <div>{{search.cover_photo.user.location }}</div>
                                    </div>
                                </div>
                        </div>    
                      </div>
                    </div>
                  </div>
            </div>
        </div>
        <div v-else>
            <div class="parent">
                <div v-for="photo in photos" :key="photo.id">
                    <div class="grid-parent">
                        <div class="grid">
                            <a class="imgg photobox" id="images" :href="'#' + photo.id">
                                <img :src="photo.urls.regular" />
                            </a>
                            <div class="text">{{photo.user.name}}</div>
                            <div class="text two">{{photo.user.location }}</div> 
                            <div class="photobox-target" :id="photo.id">
                                <img :src="photo.urls.regular"/>
                                <a class="photobox-close" href="#"></a>
                                <div class="modal-text">
                                    <div>{{photo.user.name}}</div>
                                    <div>{{photo.user.location }}</div>
                                </div>
                            </div>
                        </div>
                    </div>
                 </div>
            </div>
        </div>
      </a>
    </div>
</template>


<script>
import { ref } from 'vue'
import axios from 'axios'

export default {
  async setup () {
    const photos = ref(await getPhotos())
    const inpSearching = ref(false)
    const searchText = ref('')
    const searching = ref(false)
    const searchPhoto = ref(null)
    const searched = ref(false)


// While handling an api respponse, attach it to a ref variable, which is what will be used(variable) to render it
    function getPhotos () {
    return axios.get('https://api.unsplash.com/photos/?client_id=YcVBb5VZgUaFtCSVeOV-0d1-JBshgL_UwbpS13uzif0')
            .then((response) => {
                    return response.data
            })
            .catch(err => console.log(err))
    }

    const searchQuery = () => {
      inpSearching.value = true
      searching.value = true
        axios.get(`https://api.unsplash.com/search/collections/?client_id=YcVBb5VZgUaFtCSVeOV-0d1-JBshgL_UwbpS13uzif0&query=${searchText.value}`)
            .then(response => {
                searchPhoto.value = response.data.results
                searched.value = true
                searching.value = false
                console.log(response.data.results)
            })
            .catch (err => console.log(err))
    }

    const modal = (e) => {
        console.log(e)
    }
    return { photos, searchText, searching, searchQuery, searchPhoto, searched, inpSearching, modal }
  }
}
</script>

<style scoped>

    .splash {
        font-family: 'Poppins Light', Nunito, Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        margin: -8px;
    }

    .search {
        background-color: #DDE2E9;
        padding: 100px;
        display: grid;
    }

    .input {
        padding: 20px 50px;
        border: none;
        border-radius: 5px;
        font-size: inherit !important;
        font-family: 'Nunito' !important;
    }

    a {
        color: black;
        text-decoration: none
    }

    .input:focus {
        outline: none;
    }

    .search i {
        position: absolute;
    }

    .icon {
        padding: 22px; 
    } 

    .parent {
        width: 70%;
        margin: -35px 9.5em;
        column-count: 3;
        column-gap: 30px;
    }



    .grid {
        margin: 0;
        display: grid;
        grid-template-columns: auto;
        margin-bottom: 20px;
    }

    div img {
        width: 100%;
        border-radius: 10px;        
    }
   

    .imgg::after {
        display: block;
        position: relative;
        background-image: linear-gradient(to bottom, rgba(66, 66, 66, 0) 0, rgba(10, 10, 10, 0.863) 100%);
        margin-top: -106px;
        height: 100px;
        width: 100%;
        content: '';
        border-radius: 10px;
    }

    /* Original text overlay */
    .text {
        color: #fff;      
        transform: translate(0%, -300%);
        position: absolute;
        margin-left: 10px;
        width: 200px;
        font-family: Poppins;
    }

    .text.two {
        font-size: 0.8em;
        transform: translate(0%, -350%);
        margin-top: 20px;
        width: 150px;
        font-family: 'Poppins Light';
    }

    .opacity {
        opacity: 0
    }

    .text-header {
        font-weight: 700;
        font-size: 2em;
        color: #042046d0;
        z-index: 1;
    }

    .query {
        color: #234d86a1
    }


    /*Eliminates padding, centers the thumbnail */

    body, html {
        padding: 0;
        margin: 0;
        text-align: center;
    }


    /* Styles the photobox, removes it from sight and adds the fade-in transition */

    .photobox-target {
        position: fixed;
        top: 0%;
        left: 0%;
        width: 100%;
        background: rgba(0,0,0,.7);
        width: 100%;
        opacity: 0;
        z-index: 1;
        -webkit-transition: opacity .5s ease-in-out;
        -moz-transition: opacity .5s ease-in-out;
        -o-transition: opacity .5s ease-in-out;
        transition: opacity .5s ease-in-out;
        overflow: hidden;
    }

    /* Styles the lightbox image, centers it vertically and horizontally, adds the zoom-in transition and makes it responsive using a combination of margin and absolute positioning */

    .photobox-target img {
        margin: auto;
        position: absolute;
        left:0;
        right:0;
        bottom: 20px;
        height: 80%;
        width: 50%;
        box-shadow: 0px 0px 8px rgba(0,0,0,.3);
        -webkit-transition: .5s ease-in-out;
        -moz-transition: .5s ease-in-out;
        -o-transition: .5s ease-in-out;
        transition: .5s ease-in-out;
    }

    .photobox-target .modal-text {
        position: fixed;
        left: 30%;
        bottom: 0;
        width: 40%;
        padding: 20px 0;
        background: #eee;
        border-bottom-left-radius: 10px;
        border-bottom-right-radius: 10px;
    }

    .photobox-target .modal-text div {
        padding-left: 10px
    }


    .photobox-target .modal-text div:first-child {
        font-size: 1.2em;
        font-weight: 700
    }

    .photobox-target .modal-text div:last-child {
        font-size: 0.75em;
        color: rgb(139, 138, 138);
        font-weight: 700
    }

    /* Styles the close link, adds the slide down transition */

    a.photobox-close {
        display: block;
        width:50px;
        height:50px;
        box-sizing: border-box;
        text-decoration: none;
        position: absolute;
        top: 20px;
        right: 20px;
        -webkit-transition: .5s ease-in-out;
        -moz-transition: .5s ease-in-out;
        -o-transition: .5s ease-in-out;
        transition: .5s ease-in-out;
    }

    /* Provides part of the "X" to eliminate an image from the close link */

    a.photobox-close:before {
        content: "";
        display: block;
        height: 30px;
        width: 1px;
        background: #eee;
        position: absolute;
        left: 26px;
        top:10px;
        -webkit-transform:rotate(45deg);
        -moz-transform:rotate(45deg);
        -o-transform:rotate(45deg);
        transform:rotate(45deg);
    }

    /* Provides part of the "X" to eliminate an image from the close link */

    a.photobox-close:after {
        content: "";
        display: block;
        height: 30px;
        width: 1px;
        background: #eee;
        position: absolute;
        left: 26px;
        top:10px;
        -webkit-transform:rotate(-45deg);
        -moz-transform:rotate(-45deg);
        -o-transform:rotate(-45deg);
        transform:rotate(-45deg);
    }

    /* Uses the :target pseudo-class to perform the animations upon clicking the .lightbox-target anchor */

    .photobox-target:target {
        opacity: 1;
        top: 0;
        bottom: 0;
    }

    .photobox-target:target img {
        height: auto;
        width: 40%;
    }

    .photobox-target:target a.photobox-close {
        top: 0px;
    }

    @keyframes pulse-bg {
        0% { background-color: #ddd; }
        50% { background-color: #d0d0d0; }
        100% { background-color: #ddd; }
    }

    .grid.loader {
        display: grid;
        grid-template-columns: auto;
        grid-gap: 30px;
        column-gap: 10px
        
    }

    .loader-parent {
        column-count: 3
    }

    div .loader-image {
        width: 100%;
        border-radius: 10px;
        height: 15em;
        background: #f5f5f5;            
    }

    .loader-parent {
        width: 70%;
        margin: -50px 10em;
    }

    .loader-text {
        transform: translate(0%, -350%);
        position: absolute;
        margin-left: 10px;
        width: 150px;
        height: 15px;
        animation: pulse-bg 1s infinite;
    }

    .loader-text.two {
        font-size: 0.8em;
        transform: translate(0%, -350%);
        margin-top: 20px;
        width: 100px;
    }

    .cover {
        position: absolute;
        top: 10.15em;
        left: 2em;
        height: 50px;
        width:85%;
        background: #DDE2E9;
        z-index: 1;
    }

    @media(max-width: 996px) {
        .parent {
            column-count: 2;
            column-gap: 30px;
        }

        .loader-parent {
            column-count: 2;
            width: 70%;
            margin: -50px 10em;
        }

        .photobox-target .modal-text {
            position: fixed;
            left: 27.5%;
            bottom: 0;
            width: 45%;
            padding: 20px 0;
            background: #eee;
            border-bottom-left-radius: 10px;
            border-bottom-right-radius: 10px;
        }

        .photobox-target:target img {
            height: auto;
            width: 45%;
        }
    }

    @media(max-width: 760px) {
        .parent {
            width: 60%;
            column-count: 1;
            column-gap: 20px;
        }

        .loader-parent {
            column-count: 1;
            margin: -50px 50px;
        }

        
    }

    @media(max-width: 730px) {
        .parent {
            width: 57%;
        }

        .photobox-target .modal-text {
            position: fixed;
            left: 20%;
            bottom: 0;
            width: 60%;
            padding: 20px 0;
            background: #eee;
            border-bottom-left-radius: 10px;
            border-bottom-right-radius: 10px;
        }

        .photobox-target:target img {
            height: auto;
            width: 60%;
        }
    }

    @media(max-width: 573px) {
        .parent {
            width: 80%;
            margin: -35px 50px;
        }

         .search {
            padding: 100px 50px;
        }

        .photobox-target .modal-text {
            position: fixed;
            left: 10%;
            bottom: 0;
            width: 80%;
            padding: 20px 0;
            background: #eee;
            border-bottom-left-radius: 10px;
            border-bottom-right-radius: 10px;
        }

        .photobox-target:target img {
            height: auto;
            width: 80%;
        }

    }

    @media(max-width: 500px) {
        .parent {
            width: 77%;
        }
    }

    @media(max-width: 400px) {
        .parent {
            width: 75%;
        }
    }

    @media(max-width: 300px) {
        .input {
            width: 50%;
            font-size: 13px !important;
        }

        .parent {
            margin: -35px 30px;
        }

        .search {
            padding: 100px 30px;
        }
    }

</style>