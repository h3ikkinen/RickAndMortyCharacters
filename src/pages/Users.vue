<template>
    <div class="wrapper">
        <div class="wrapper-content wrapper-content--fixed">
            <div class="container">
                <h1>Charecters page</h1>
                <hr />
                <p>
                    Debag:
                    <br />
                    sort: <span class="debagBlue">{{ currentSort }}</span
                    >,
                    <br />
                    dir: <span class="debagBlue">{{ currentSortDir }}</span>
                </p>
                <!-- table users -->
                <table>
                    <!-- head -->
                    <thead>
                    <tr>
                        <th
                            v-for="{ text, value } in headers"
                            :key="value"
                            @click="sort(value)"
                            v-bind:class="{ active: currentSort === value }"
                        >
                            {{ text }}
                        </th>
                    </tr>
                    </thead>

                    <!-- body -->
                    <tbody>
                    <tr
                        @click="characterHandler(user.id)"
                        v-for="user in usersSort"
                        :key="user.id"
                    >
                        <td>
                            <div class="img">
                                <img :src="user.image" alt="user" />
                                <span>
                                {{ user.name }}
                                </span>
                            </div>
                        </td>
                        <td>
                            {{ user.status }}
                        </td>
                        <td>
                            {{ user.gender }}
                        </td>
                    </tr>
                    </tbody>
                </table>
            </div>
            <user
                v-show="userShow"
                @close="userShow = false"
                v-bind:user="userModalContent"
            >
            </user>
        </div>
        <!-- btns -->
        <div>
            <div class="container">
                <div class="button-list">
                    <div @click="prevPage" class="btn btnPrimary">&#8592;</div>
                    <div @click="nextPage" class="btn btnPrimary">&#8594;</div>
                </div>
            </div>
        </div>
    </div>
</template> 

<script>
import axios from "axios";
import user from "@/components/UserModal.vue";

export default {
    data() {
        return {
            headers: [
            { text: "Имя", value: "name" },
            { text: "Статус", value: "status" },
            { text: "Пол", value: "gender" },
            ],
            users: [],
            usersSort: null,
            userShow: false,
            userModalContent: {},
            currentSort: null,
            currentSortDir: "asc",
            page: {
                current: 1,
                length: 5,
            }
        };
    },

    components: {
        user,
    },

    created() {
        axios
            .get("https://rickandmortyapi.com/api/character")
            .then((response) => {
                this.users = response.data.results;
                this.sortUsers();
            })
            .catch((error) => {
                console.log(error);
            });
    },

    methods: {
        characterHandler(id) {
            let user = this.users.filter((u) => u.id === id);
            if (user) {
                this.userShow = true;
                this.userModalContent = user[0];
            }
        },
        sort(sort) {
            if (sort === this.currentSort) {
                this.switchSortDirection();
            } else {
                this.currentSort = sort;
            }

            this.sortUsers();
        },
        switchSortDirection() {
            this.currentSortDir = this.currentSortDir === "asc" ? "desc" : "asc";
        },
        sortUsers() {
            this.usersSort = this.users.sort((a, b) => {
                let mod = this.currentSortDir === "asc" ? 1 : -1;

                if (a[this.currentSort] < b[this.currentSort]) return -1 * mod;
                if (a[this.currentSort] > b[this.currentSort]) return mod;
                
                return 0;
            }).filter((row, index) => {
                let start = (this.page.current-1)*this.page.length;
                let end = this.page.current * this.page.length;

                if (index >= start && index < end) return true
            })
        },
        prevPage () {
            if (this.page.current > 1) this.page.current-=1
            this.sortUsers()
        },
        nextPage () {
            if ( (this.page.current * this.page.length ) < this.users.length) this.page.current+=1
            this.sortUsers()
        }
    },
};
</script>

<style lang="scss" scoped>
    .button-list {
        width: 100%;
        text-align: center;
        margin: 1rem 0;
        .btn {
            border-radius: 60px;
            margin: 0 20px;
        }
    }
    .img {
        img {
            width: 100px;
            height: 100px;
            border-radius: 100%;
        }
        span {
            margin-left: 1rem;
        }
    }
    th {
        transition: 0.2s ease-in-out;
        &:hover {
            color: peru;
        }
        &.active {
            color: blue;
        }
    }
    .debagBlue {
        color: blue;
    }
</style>
