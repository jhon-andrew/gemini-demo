<script setup lang="ts">
import { ref } from 'vue'
import TextOnly from './components/TextOnly.vue'
import TextAndImage from './components/TextAndImage.vue'
import MultiTurn from './components/MultiTurn.vue'
import SystemInstructions from './components/SystemInstructions.vue'

const selectedMenu = ref('text-only')

function selectMenu(key: string) {
  selectedMenu.value = key
}
</script>

<template>
  <el-container>
    <el-header>
      <h1 size="large">Google I/O Extended ZamPen 2024! 🚀</h1>
      <el-text size="small">#GoogleIOExtended #GoogleforDevelopers #GDGPhilippines #ioextended #GoogleIO #GDGZamboanga #WTMZamboanga</el-text>
    </el-header>
    <el-main>
      <el-row>
        <el-col :sm="9" :md="6">
          <el-menu @select="selectMenu" :default-active="selectedMenu">
          <el-menu-item index="text-only">
            Text-only input
          </el-menu-item>
          <el-menu-item index="text-and-image">
            Text-and-image input (Multimodal)
          </el-menu-item>
          <el-menu-item index="multi-turn">
            Multi-turn conversations
          </el-menu-item>
          <el-menu-item index="system-instructions">
            With System Instructions
          </el-menu-item>
        </el-menu>
        </el-col>
        <el-col class="main-content" :sm="15" :md="18">
          <text-only v-if="selectedMenu === 'text-only'" />
          <text-and-image v-else-if="selectedMenu === 'text-and-image'" />
          <multi-turn v-else-if="selectedMenu === 'multi-turn'" />
          <system-instructions v-else-if="selectedMenu === 'system-instructions'" />
          <template v-else>{{ selectedMenu }}</template>
        </el-col>
      </el-row>
    </el-main>
  </el-container>
</template>

<style scoped>
.main-content {
  padding-left: 20px;
}
</style>
