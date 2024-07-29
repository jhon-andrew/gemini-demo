<script setup lang="ts">
import { Ref, ref } from 'vue'
import markdownit from 'markdown-it'
import { GoogleGenerativeAI } from '@google/generative-ai'
import { ElMessageBox } from 'element-plus'

const title: Ref<string> = ref('Generate text from text-and-image input (multimodal)')
const md = markdownit()
const loading = ref(false)
const prompt = ref(null)
const selectedFile: Ref<any> = ref(null)
const output: Ref<any> = ref(null)

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
        if (!selectedFile.value) {
            ElMessageBox.alert('This demo requires a file to be selected.')
            return
        }

        loading.value = true

        // Use the model to generate content based from the prompt
        const result = await model.generateContent([
            // The usual text prompt
            prompt.value!,
            // An object with the image data
            {
                inlineData: {
                    /**
                     * Only get the actual base64 data
                     * https://ai.google.dev/gemini-api/docs/get-started/tutorial?lang=web#generate-text-from-text-and-image-input
                     */
                    data: selectedFile.value.split(',')[1],
                    // The file type of the selected file
                    mimeType: selectedFile.value.split(':')[1].split(';')[0]
                }
            }
        ])

        // Get the response from the model
        const response = await result.response

        // Get the response as text
        const text = response.text()

        // Parse markdown to HTML
        output.value = md.render(text)

        loading.value = false
        title.value = prompt.value!
        prompt.value = null
    } catch (error: any) {
        console.log(error)
        ElMessageBox.alert(error.message ?? 'An unknown error occured.', 'Error')
    }
}

// A function that opens a file upload dialog
function selectFile() {
    // Create file input
    const input = document.createElement('input')

    input.type = 'file'
    input.accept = 'image/*' // Accept only image files

    // Listen for changes in file input
    input.onchange = function () {
        const file = input.files![0]
        const reader = new FileReader()

        reader.onloadend = function () {
            // Url safe; can be used in <img /> tags
            const urlSafeBase64data = reader.result

            selectedFile.value = urlSafeBase64data
        }

        reader.readAsDataURL(file)
    }

    // Open file upload dialog
    input.click()
}
</script>

<template>
    <el-card>
        <template #header>
            <div class="card-header">
                <span>{{ title }}</span>
            </div>
        </template>

        <el-image v-if="selectedFile" :src="selectedFile"></el-image>
        <div v-html="output"></div>
        
        <template #footer>
            <el-form @submit.prevent="send">
                <el-input
                    v-model="prompt"
                    placeholder="Enter a prompt"
                    :disabled="loading"
                    clearable
                >
                    <template #prepend>
                        <el-button @click="selectFile"><i-ri-attachment-2></i-ri-attachment-2></el-button>
                    </template>
                    <template #append>
                        <el-button type="success" @click="send" :loading="loading">Send</el-button>
                    </template>
                </el-input>
            </el-form>
        </template>
    </el-card>
</template>

<style scoped>
</style>
