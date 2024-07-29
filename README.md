# Gemini Demo

This repository demonstrates a basic implementation of using Gemini's API within a Vue 3 application written in Typescript. It's designed to serve as a starting point for developers who want to explore integrating Gemini functionality into their web projects.

Note: This is a sample project focused on showcasing the core functionalities of the Gemini API. It does not encompass all aspects of a full-fledged web application.

## Contents

Sample implementations of Gemini API for NodeJS can be found in `./src/components` directory.

- **Text-only input**: [`./src/components/TextOnly.vue`](/src/components/TextOnly.vue)
    - This shows the most basic example on how to use and send prompts to Gemini API.
- **Text-and-image input (Multimodal)**: [`./src/components/TextAndImage.vue`](/src/components/TextAndImage.vue)
    - This example shows how you can pass on files for the model to analyze along with the prompt.
- **Multi-turn conversations** [`./src/components/MultiTurn.vue`](/src/components/MultiTurn.vue)
    - This example shows how you can build a chat UX with Gemini.
- **With System Instructions** [`./src/components/SystemInstructions.vue`](/src/components/SystemInstructions.vue)
    - This example shows how you can add system instructions to tailor the model according to your needs.

## Get API key in Google AI Studio

In order to run/build this repo:

1. You need to get your own API key from Google AI Studio:
    - https://aistudio.google.com/app/apikey
1. Rename `.env.example` to `.env`
1. Change the value of `VITE_GEMINI_API_KEY` to the API key you generated.

---

For questions, you can reach me via the following:

- Email: [anecbook@gmail.com](mailto:anecbook@gmail.com)
- Facebook: [m.me/jhonandrewbaes](https://m.me/jhonandrewbaes)
- Linkedin: [https://linkedin.com/in/jhon-andrew](https://linkedin.com/in/jhon-andrew)