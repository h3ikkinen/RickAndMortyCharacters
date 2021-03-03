<template>
    <modal
        @close="onClose" 
        :id="user.id"   
    >
        <template v-slot:body>
            <div class="user_container">
                <div class="left">
                    <img :src="user.image">
                </div>
                <div class="right">
                    <h2>{{user.name}}</h2>
                    <div class="descr">
                        <p >
                            Gender: {{user.gender}}
                        </p>
                        <p v-if="user.location != null">
                            Location: {{user.location.name}}
                        </p>
                        <p v-if="user.origin != null">
                            Origin: {{user.origin.name}}
                        </p>
                        <p>
                            Status: {{user.status}}
                        </p>
                        <p v-if="user.episode != null">
                            <span @click="showEpisodesInfo" class="episodeHeader">Episodes: </span>
                            <transition name="fade">
                                <div v-if="episodesShow == true">
                                    <episodItem 
                                        v-for="(episod, id) in user.episode" 
                                        :key="id"
                                        v-bind:episod="episod"
                                    />
                                </div>
                            </transition>
                        </p>
                    </div>
                </div>
            </div>
        </template>
    </modal>
</template>

<script>
import modal from './UI/modal';
import EpisodItem from './Episod/EpisodItem';
export default {
    data() {
        return {
            episodes: [],
            episodesShow: false,
        }
    },
    components: {
        modal, EpisodItem
    },
    props: {
        user: {
            type: Object,
            required: true,
        }
    },
    methods: {
        onClose () {
            this.episodesShow = false;
            this.$emit('close')
        },
        showEpisodesInfo () {
            this.episodesShow = !this.episodesShow;
        }
    },

}
</script>

<style lang="scss" scoped>
    .fade-enter-active, .fade-leave-active {
        transition: .5s;
    }
    .fade-enter, .fade-leave-to /* .fade-leave-active до версии 2.1.8 */ {
        opacity: 0;
    }
    .user_container {
        width: 100%;
        display: flex;
        align-items: start;
        justify-content: space-around;
        .left {
            width: 15%;
            img {
                width: 150px;
                height: auto;
                border-radius: 100%;
                position: sticky;
                left: 0;
                top: 0;
            }
        }
        .right {
            width: 70%;
            max-width: 70%;
            // padding-left: 2rem;
        }
    }
    h2 {
        font-size: 35px;
        font-weight: bold;
    }
    p {
        margin-top: 1rem;
        font-weight: bold;
    }
    .episodeHeader {
        transition: .2s ease-in-out;
        cursor: pointer;
        font-size: 20px;
        &:hover {
            color: orange;
        }
    }
</style>