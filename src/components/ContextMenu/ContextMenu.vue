<template>
  <div class="tw-w-44 tw-h-72 tw-absolute tw-rounded-xl tw-overflow-hidden tw-border tw-border-gray-400 tw-select-none" style="z-index:9999;backdrop-filter: blur(35px);box-shadow: 0 0 10px rgba(16,16,16,.3)" @click="fullclicked" :style="{'left':context_menu_x+'px', 'top':context_menu_y +'px'}">
    <div class="tw-absolute tw-w-full tw-h-full tw-py-2 tw-flex tw-flex-col" style="background-color:rgba(235,235,235,.7)">
      <ContextMenuButton disabled further_menu :text="'新建'"/>
      <ContextMenuButton disabled further_menu :text="'查看'"/>
      <ContextMenuButton disabled further_menu :text="'排列方式'"/>
      <div class="tw-relative" @click.stop>
        <ContextMenuButton 
          :text="'重新拉取系统镜像'" 
          :disabled="isReloading"
          @click.native="reload_clicked"
        />
        <div v-if="isReloading" class="tw-absolute tw-bottom-0 tw-left-0 tw-w-full tw-h-1 tw-bg-gray-200">
          <div 
            class="tw-h-full tw-bg-blue-500 tw-transition-all tw-duration-300" 
            :style="{ width: progress + '%' }"
          />
        </div>
      </div>
      <ContextMenuButton disabled :text="'粘贴'"/>
      <ContextMenuButton :text="'在终端打开'" @click.native="terminal_clicked"/>
      <div class="tw-w-full" style="height:1.5px;background-color:rgba(188,188,188,1)"/>
      <ContextMenuButton disabled :text="'个性化'"/>
      <ContextMenuButton :text="'系统设置'" @click.native="settings_clicked"/>
      <ContextMenuButton :text="'返回传统个人网站'" @click.native="old_website"/>
    </div>
  </div>
</template>

<script>
import ContextMenuButton from './ContextMenuButton.vue'

export default {
  name: 'ContextMenu',
  components: {
    ContextMenuButton
  },
  data() {
    return {
      isReloading: false,
      progress: 0,
      progressInterval: null
    }
  },
  methods: {
    reload_clicked() {
      if (this.isReloading) return;
      
      this.isReloading = true;
      this.progress = 0;
      
      this.progressInterval = setInterval(() => {
        this.progress += Math.random() * 15 + 5;
        
        if (this.progress >= 100) {
          this.progress = 100;
          clearInterval(this.progressInterval);
          
          setTimeout(() => {
            window.location.reload();
          }, 1000);
        }
      }, 500);
    },
    
    fullclicked() {
      this.$store.commit('clear_context_menu');
    },
    
    terminal_clicked() {
      if (this.has_terminal) {
        this.$store.commit('switch_global_window_show_status', {'type':'terminal'});
      } else {
        window.open('/terminal');
      }
      this.$store.commit('clear_context_menu');
    },
    
    settings_clicked() {
      if (this.has_settings) {
        this.$store.commit('switch_global_window_show_status', {'type':'settings'});
      } else {
        window.open('/settings');
      }
      this.$store.commit('clear_context_menu');
    },
    
    old_website() {
      window.location.href = "https://old.rainy.me";
    }
  },
  
  beforeDestroy() {
    if (this.progressInterval) {
      clearInterval(this.progressInterval);
    }
  }
}
</script>
