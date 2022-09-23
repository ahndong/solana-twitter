<script setup>
import { ref, watchEffect } from 'vue'
// 페이지네이션 설정 시, 삭제
// import { fetchTweets, authorFilter } from '@/api'
import { paginateTweets, authorFilter } from '@/api'
import TweetForm from '@/components/TweetForm'
import TweetList from '@/components/TweetList'
import { useWorkspace } from '@/composables'

const tweets = ref([])
// 페이지네이션 설정 시, 삭제
// const loading = ref(true)
const { wallet } = useWorkspace()
const filters = ref([])

const onNewPage = newTweets => tweets.value.push(...newTweets)
const { prefetch, hasNextPage, getNextPage, loading } = paginateTweets(filters, 10, onNewPage)

watchEffect(() => {
    if (! wallet.value) return
// 페이지네이션 설정 시, 삭제
    // fetchTweets([authorFilter(wallet.value.publicKey.toBase58())])
    //     .then(fetchedTweets => tweets.value = fetchedTweets)
    //     .finally(() => loading.value = false)
    tweets.value = []
    filters.value = [authorFilter(wallet.value.publicKey.toBase58())]
    prefetch().then(getNextPage)
})

const addTweet = tweet => tweets.value.push(tweet)
</script>

<template>
    <!-- TODO: Check connected wallet
    <div v-if="true" class="border-b px-8 py-4 bg-gray-50">
        B1AfN7AgpMyctfFbjmvRAvE1yziZFDb9XCwydBjJwtRN
    </div> -->
    <div v-if="wallet" class="border-b px-8 py-4 bg-gray-50 break-all">
        {{ wallet.publicKey.toBase58() }}
    </div>
    <tweet-form @added="addTweet"></tweet-form>
    <!-- 페이지네이션 설정 시, 삭제 -->
    <!-- <tweet-list v-model:tweets="tweets" :loading="loading"></tweet-list> -->
    <tweet-list v-model:tweets="tweets" :loading="loading" :has-more="hasNextPage" @more="getNextPage"></tweet-list>
</template>
