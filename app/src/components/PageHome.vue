<script setup>
import { ref } from 'vue'
// 페이지네이션 설정 시, 삭제
// import { fetchTweets } from '@/api'
import { paginateTweets } from '@/api'
import TweetForm from '@/components/TweetForm'
import TweetList from '@/components/TweetList'

const tweets = ref([])
// 페이지네이션 설정 시, 삭제
// const loading = ref(true)
// fetchTweets()
//     .then(fetchedTweets => tweets.value = fetchedTweets)
//     .finally(() => loading.value = false)
const onNewPage = newTweets => tweets.value.push(...newTweets)
const { prefetch, hasNextPage, getNextPage, loading } = paginateTweets([], 10, onNewPage)
prefetch().then(getNextPage)
    
const addTweet = tweet => tweets.value.push(tweet)
</script>

<template>
    <tweet-form @added="addTweet"></tweet-form>
    <!-- 페이지네이션 설정 시, 삭제 -->
    <!-- <tweet-list v-model:tweets="tweets" :loading="loading"></tweet-list> -->
    <tweet-list v-model:tweets="tweets" :loading="loading" :has-more="hasNextPage" @more="getNextPage"></tweet-list>
</template>