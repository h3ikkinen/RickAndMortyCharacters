<template>
    <div class="wrapper">
        <div class="wrapper-content wrapper-content--fixed">
                <div class="container">
                    <h1>Home page </h1>
                    <br>
                    <hr>
                    <p>This app it is intended for viewing information about the characters of the animated series "Rick and Morty"</p>
                    <br>
                    <p>Information about the characters can be found <router-link class="link" :to="links[1].url">here</router-link></p>
                </div>
        </div>
    </div>
</template> 

<script>
export default {
    data () {
        return {
        links: [
            {
                title: 'Home',
                url: '/'
            },
            {
                title: 'Charecters',
                url: '/Users'
            },
        ]
        }
    }
    }
</script>

<style lang="scss" scoped>
    .link {
        color: peru;
        transition: .2s ease-in;
        &:hover {
            color: blue ;
        }
    }
</style>