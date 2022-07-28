<template>
    <q-page>
        <div class="q-py-lg q-px-md row items-end q-col-gutter-md">
            <div class="col">
                <q-input
                    bottom-slots
                    v-model="newQweetContent"
                    class="new-qweet"
                    placeholder="What's happening?"
                    counter
                    maxlength="280"
                    :dense="dense"
                    autogrow
                >
                    <template v-slot:before>
                        <q-avatar size="xl">
                            <img src="https://cdn.quasar.dev/img/avatar5.jpg" />
                        </q-avatar>
                    </template>

                    <template v-slot:hint> Field hint </template>
                </q-input>
            </div>
            <div class="col col-shrink">
                <q-btn
                    @click="addNewQweet"
                    :disable="!newQweetContent"
                    class="q-mb-md"
                    no-caps
                    unelevated
                    rounded
                    color="primary"
                    label="Qweet"
                />
            </div>
        </div>
        <q-separator size="10px" color="grey-2" class="divider" />

        <q-list separator>
            <q-item v-for="qweet in qweets" :key="qweet.date" class="q-py-md">
                <q-item-section avatar>
                    <q-avatar>
                        <img src="https://cdn.quasar.dev/img/avatar2.jpg" />
                    </q-avatar>
                </q-item-section>

                <q-item-section>
                    <q-item-label>
                        <strong>Lisa</strong>
                        <span class="text-grey-7">@lisa111</span>
                    </q-item-label>
                    <q-item-label class="qweet-content text-body1">
                        {{ qweet.content }}
                    </q-item-label>
                    <div class="qweet-icons row justify-between q-mt-sm">
                        <q-btn
                            flat
                            round
                            color="grey"
                            icon="far fa-comment"
                            size="sm"
                        /><q-btn
                            flat
                            round
                            color="grey"
                            icon="fas fa-retweet"
                            size="sm"
                        /><q-btn
                            flat
                            round
                            color="grey"
                            icon="far fa-heart"
                            size="sm"
                        /><q-btn
                            flat
                            round
                            color="grey"
                            icon="fas fa-trash"
                            size="sm"
                            @click="deleteQweet(qweet)"
                        />
                    </div>
                </q-item-section>

                <q-item-section side top>
                    {{ qweet.date | relativeDate }}
                </q-item-section>
            </q-item>

            <q-separator inset="item" />
        </q-list>
    </q-page>
</template>

<script>
import { formatDistance } from "date-fns";

import { defineComponent } from "vue";
// import db from "src/boot/firebase";
export default defineComponent({
    name: "PageHome",
    data() {
        return {
            newQweetContent: "",
            qweets: [
                {
                    content: `1lorem lorem ipsum dolor sit amet, consectetur adipisicing
                        elit, sed do eiusmod temporary   doloribus `,
                    date: "1658744461122",
                },
                {
                    content:
                        "2lorem lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod temporary   doloribus ",
                    date: 1658744461135,
                },
            ],
        };
    },

    filters: {
        relativeDate(value) {
            return formatDistance(value, new Date(), { addSuffix: true });
        },
    },
    methods: {
        addNewQweet() {
            let newQweet = {
                content: this.newQweetContent,
                date: Date.now(),
            };
            this.qweets.unshift(newQweet);
            this.newQweetContent = "";
        },
        deleteQweet(qweet) {
            let dateToDelete = qweet.date;
            let index = this.qweets.findIndex(
                (qweet) => qweet.date === dateToDelete
            );
            this.qweets.splice(index, 1);
        },
    },
    // mounted() {
    //     db.collection("qweets").onSnapshot((snapshot) => {
    //         snapshot.docChanges().forEach((change) => {
    //             if (change.type === "added") {
    //                 console.log("New qweet: ", change.doc.data());
    //             }
    //             if (change.type === "modified") {
    //                 console.log("Modified qweet: ", change.doc.data());
    //             }
    //             if (change.type === "removed") {
    //                 console.log("Removed qweet: ", change.doc.data());
    //             }
    //         });
    //     });
    // },
});
</script>
<style lang="sass">
.new-qweet
    textarea
        font-size: 19px
        line-height: 1.4 !important
.divider
    border-top: 1px solid
    border-bottom: 1px solid
    border-color: $grey-3

.qweet-content
    white-space: pre-line

.qweet-icons
    margin-left: -5px
</style>
