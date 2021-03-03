<template>
    <div v-if="episodeInfo != null">
        <hr>
        <h2 v-if="episodeInfo.name != null">{{episodeInfo.name}}</h2>
        <p  v-if="episodeInfo.episode != null">Episod: {{episodeInfo.episode}}</p>
        <p  v-if="episodeInfo.air_date != null">Release date: {{episodeInfo.air_date}}</p>
        <p>
            <span @click="generateLink" class="hrefLink">
                View episode in Russian Language
            </span>
        </p>
        <hr>
    </div>
</template>
<script>
    import axios from 'axios';
    export default {
        data() {
            return {
                episodeInfo: null,
                linkToView: ''
            }
        },
        props: {
            episod: {
                required: true,
                type: String,
            }
        },
        methods: {
            generateLink() {
                let el = this.episodeInfo.episode;
                let x = parseInt(el.replace(/[^\d]/g, ''))+'';
                let linkArr = x.split('');
                let sQs;
                if (linkArr[1] == '0') {
                    sQs = linkArr.filter((c) => c != '0');
                } else {
                    sQs = linkArr;
                }
                
                if(sQs.length == 3) {
                    let sezon = sQs[0];
                    let seria = sQs[1] + sQs[2];
                    this.linkToView = `https://rick-i-morty.online/episodes/${sezon}sez-${seria}seriya112/`;
                } else {
                    this.linkToView = `https://rick-i-morty.online/episodes/${sQs[0]}sez-${sQs[1]}seriya112/`;
                }
                window.open(this.linkToView, '_blank');
            }
        },
        created() {
            

            if(this.episod != null) {

                
                let episodArr = this.episod.split('/');
                let episodId = episodArr[episodArr.length - 1];
                axios
                    .get(`https://rickandmortyapi.com/api/episode/${episodId}`)
                        .then(response => {
                            if (response.data != null) {
                                this.episodeInfo = response.data;
                            }
                        })
                        .catch(error => {
                            console.log(error)
                        });
                
            }
        }
    }
</script>

<style lang="scss" scoped>
    h2 {
        font-weight: normal;
    }
    p {
        font-weight: 400;
    }
    .hrefLink {
        font-size: 20px;
        color:orchid;
        transition: .2s ease-out;
        cursor: pointer;
        &:hover {
            color: blue;
        }
    }
</style>