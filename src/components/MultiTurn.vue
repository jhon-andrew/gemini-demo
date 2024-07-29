<script setup lang="ts">
import { Ref, ref } from 'vue'
import markdownit from 'markdown-it'
import { GoogleGenerativeAI } from '@google/generative-ai'
import { ElMessageBox } from 'element-plus'

const title = ref('Build multi-turn conversations (chat)')
const md = markdownit()
const loading = ref(false)
const prompt = ref(null)
const chatSession: Ref<any> = ref(null)
// const output = ref(null)

/**
 * You can get your API key from the Gemini API Documentation
 * https://ai.google.dev/gemini-api/docs/api-key
 */
const apiKey = import.meta.env.VITE_GEMINI_API_KEY

/**
 * Step 1: Initialize the GoogleGenerativeAI class
 * https://ai.google.dev/gemini-api/docs/quickstart?lang=node#initialize-model
 */
const genAi = new GoogleGenerativeAI(apiKey)

/**
 * Step 2: Get a generative model
 */
const model = genAi.getGenerativeModel({
    /**
     * See link below on what this model can do and what other variants you can use:
     * https://ai.google.dev/gemini-api/docs/models/gemini#model-variations
     */
    model: 'gemini-1.5-flash'
})

/**
 * Step 3: Send a prompt to the model and get a response
 */
async function send() {
    try {
        loading.value = true

        // Check if there is no existing chat session
        if (!chatSession.value) {
            /**
             * See link below to know more on how to start a chat session
             * https://ai.google.dev/gemini-api/docs/get-started/tutorial?lang=web#multi-turn-conversations-chat
             */
            chatSession.value = model.startChat({
                history: [],
                generationConfig: {
                    maxOutputTokens: 100,
                },
            })
        }

        // Get current chat session
        const chat = chatSession.value

        // Send a message to current chat session
        const result = await chat.sendMessage(prompt.value)

        // Get the response from the model
        const response = await result.response

        // Get the response as text
        const text = response.text()

        loading.value = false
        prompt.value = null

        console.log('RESPONSE:', { text })
    } catch (error: any) {
        console.log(error)
        ElMessageBox.alert(error.message ?? 'An unknown error occured.', 'Error')
    }
}
</script>

<template>
    <el-card>
        <template #header>
            <div class="card-header">
                <span>{{ title }}</span>
            </div>
        </template>

        <!-- <div v-html="output"></div> -->
        <el-timeline v-if="chatSession">
            <el-timeline-item
                v-for="(chat, index) of chatSession._history"
                :key="index"
                :type="chat.role === 'model' ? 'primary' : 'info'"
                :timestamp="chat.parts[0].timestamp"
            >
                <div v-html="md.render(chat.parts[0].text)"></div>
            </el-timeline-item>
        </el-timeline>
        
        <template #footer>
            <el-form @submit.prevent="send">
                <el-input
                    v-model="prompt"
                    placeholder="Enter a prompt"
                    :disabled="loading"
                    clearable
                >
                    <template #append>
                        <el-button @click="send" :loading="loading">Send</el-button>
                    </template>
                </el-input>
            </el-form>
        </template>
    </el-card>
</template>

<style scoped>
</style>
