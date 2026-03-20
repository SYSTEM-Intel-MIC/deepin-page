<template>
  <Window :uuid='uuid' :startpos_x="startpos_x" :startpos_y="startpos_y" :zindex="zindex" :default_width="900" :minimized="minimized" :blacktheme="true">
    <template v-slot:header>
      <div class="tw-flex tw-items-center tw-select-none" style="pointer-events:none;">
        <div class="tw-w-8 tw-h-8 tw-ml-4 tw-flex tw-items-center tw-justify-center" style="background-color:#5D7C15;border-radius:4px;">
          <span class="tw-text-white tw-font-bold tw-text-lg">M</span>
        </div>
        <div class="tw-ml-3 tw-font-bold tw-tracking-wider tw-text-white" style="pointer-events:auto;font-family: 'Microsoft YaHei', sans-serif;"> Minecraft 启动器 - SimpFun服务器 </div>
      </div>
    </template>
    <template v-slot:content>
      <div class="tw-w-full tw-h-full tw-overflow-hidden tw-rounded-b-2xl tw-flex tw-flex-col" style="background: linear-gradient(180deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);" @contextmenu.prevent="mr_clicked">

        <!-- 顶部导航栏 -->
        <div class="tw-w-full tw-h-14 tw-flex tw-items-center tw-px-6" style="background-color: rgba(0,0,0,0.3);border-bottom: 2px solid #5D7C15;">
          <div class="tw-flex tw-gap-1">
            <div v-for="tab in tabs" :key="tab.id"
                 @click="activeTab = tab.id"
                 class="tw-px-5 tw-py-2 tw-cursor-pointer tw-transition-all tw-duration-300 tw-rounded-t-lg"
                 :class="activeTab === tab.id ? 'tw-text-white' : 'tw-text-gray-400 hover:tw-text-gray-200'"
                 :style="activeTab === tab.id ? 'background-color: #5D7C15;' : ''">
              <v-icon small class="tw-mr-2" :color="activeTab === tab.id ? 'white' : 'gray'">{{ tab.icon }}</v-icon>
              <span class="tw-font-medium">{{ tab.name }}</span>
            </div>
          </div>
        </div>

        <!-- 主内容区域 -->
        <div class="tw-flex-1 tw-overflow-y-auto tw-p-6">

          <!-- 首页/服务器信息 -->
          <div v-if="activeTab === 'home'" class="tw-space-y-6">
            <!-- 服务器状态卡片 -->
            <div class="tw-rounded-xl tw-p-6" style="background: linear-gradient(135deg, #5D7C15 0%, #4a6310 100%);box-shadow: 0 8px 32px rgba(93,124,21,0.4);">
              <div class="tw-flex tw-items-center tw-justify-between">
                <div>
                  <h2 class="tw-text-3xl tw-font-bold tw-text-white tw-mb-2">🎮 SimpFun MC服务器</h2>
                  <p class="tw-text-green-100 tw-text-lg">状态: <span class="tw-font-bold tw-text-white">{{ serverStatus }}</span></p>
                  <p class="tw-text-green-100">在线玩家: <span class="tw-font-bold tw-text-white">{{ onlinePlayers }}/{{ maxPlayers }}</span></p>
                </div>
                <div class="tw-text-right">
                  <div class="tw-text-5xl tw-mb-2">⛏️</div>
                  <p class="tw-text-green-100">版本: 1.20.1</p>
                </div>
              </div>
            </div>

            <!-- 服务器地址 -->
            <div class="tw-bg-gray-800 tw-rounded-xl tw-p-5 tw-border tw-border-gray-700">
              <h3 class="tw-text-xl tw-font-bold tw-text-white tw-mb-4">📡 服务器地址</h3>
              <div class="tw-flex tw-gap-3">
                <div class="tw-flex-1 tw-bg-gray-900 tw-rounded-lg tw-px-4 tw-py-3 tw-flex tw-items-center tw-border tw-border-gray-600">
                  <v-icon small color="#5D7C15" class="tw-mr-3">mdi-server</v-icon>
                  <span class="tw-text-green-400 tw-font-mono tw-text-lg">{{ serverAddress }}</span>
                </div>
                <button @click="copyAddress" class="tw-px-6 tw-py-3 tw-rounded-lg tw-font-bold tw-text-white tw-transition-all hover:tw-scale-105" style="background-color: #5D7C15;">
                  <v-icon small color="white" class="tw-mr-2">mdi-content-copy</v-icon>
                  复制
                </button>
              </div>
              <p class="tw-text-gray-400 tw-mt-3 tw-text-sm">💡 提示: 点击复制按钮，然后在游戏中直接粘贴使用</p>
            </div>

            <!-- 快捷操作 -->
            <div class="tw-grid tw-grid-cols-2 tw-gap-4">
              <div @click="activeTab = 'mods'" class="tw-bg-gray-800 tw-rounded-xl tw-p-5 tw-border tw-border-gray-700 tw-cursor-pointer hover:tw-border-green-500 tw-transition-all hover:tw-transform hover:tw-scale-[1.02]">
                <div class="tw-flex tw-items-center tw-mb-3">
                  <div class="tw-w-12 tw-h-12 tw-rounded-lg tw-flex tw-items-center tw-justify-center" style="background-color: #5D7C15;">
                    <v-icon color="white">mdi-puzzle</v-icon>
                  </div>
                  <div class="tw-ml-4">
                    <h4 class="tw-text-white tw-font-bold">安装模组</h4>
                    <p class="tw-text-gray-400 tw-text-sm">下载服务器所需模组</p>
                  </div>
                </div>
              </div>

              <div @click="activeTab = 'guide'" class="tw-bg-gray-800 tw-rounded-xl tw-p-5 tw-border tw-border-gray-700 tw-cursor-pointer hover:tw-border-green-500 tw-transition-all hover:tw-transform hover:tw-scale-[1.02]">
                <div class="tw-flex tw-items-center tw-mb-3">
                  <div class="tw-w-12 tw-h-12 tw-rounded-lg tw-flex tw-items-center tw-justify-center" style="background-color: #5D7C15;">
                    <v-icon color="white">mdi-book-open</v-icon>
                  </div>
                  <div class="tw-ml-4">
                    <h4 class="tw-text-white tw-font-bold">新手指南</h4>
                    <p class="tw-text-gray-400 tw-text-sm">查看游戏教程</p>
                  </div>
                </div>
              </div>
            </div>

            <!-- 服务器特色 -->
            <div class="tw-bg-gray-800 tw-rounded-xl tw-p-5 tw-border tw-border-gray-700">
              <h3 class="tw-text-xl tw-font-bold tw-text-white tw-mb-4">✨ 服务器特色</h3>
              <div class="tw-grid tw-grid-cols-3 tw-gap-4">
                <div v-for="feature in features" :key="feature.name" class="tw-text-center tw-p-4 tw-rounded-lg" style="background-color: rgba(93,124,21,0.2);">
                  <div class="tw-text-3xl tw-mb-2">{{ feature.icon }}</div>
                  <h4 class="tw-text-white tw-font-bold tw-mb-1">{{ feature.name }}</h4>
                  <p class="tw-text-gray-400 tw-text-sm">{{ feature.desc }}</p>
                </div>
              </div>
            </div>
          </div>

          <!-- 模组下载页面 -->
          <div v-if="activeTab === 'mods'" class="tw-space-y-6">
            <div class="tw-flex tw-items-center tw-mb-6">
              <button @click="activeTab = 'home'" class="tw-mr-4 tw-text-gray-400 hover:tw-text-white">
                <v-icon>mdi-arrow-left</v-icon>
              </button>
              <h2 class="tw-text-2xl tw-font-bold tw-text-white">📦 模组下载</h2>
            </div>

            <div class="tw-bg-gray-800 tw-rounded-xl tw-p-5 tw-border tw-border-gray-700">
              <div class="tw-flex tw-items-center tw-justify-between tw-mb-4">
                <h3 class="tw-text-lg tw-font-bold tw-text-white">必需模组列表</h3>
                <a :href="modDownloadUrl" target="_blank" class="tw-px-4 tw-py-2 tw-rounded-lg tw-font-bold tw-text-white tw-transition-all hover:tw-scale-105" style="background-color: #5D7C15;">
                  <v-icon small color="white" class="tw-mr-2">mdi-download</v-icon>
                  下载全部模组
                </a>
              </div>

              <div class="tw-space-y-3">
                <div v-for="mod in modList" :key="mod.name" class="tw-bg-gray-900 tw-rounded-lg tw-p-4 tw-flex tw-items-center tw-justify-between tw-border tw-border-gray-700">
                  <div class="tw-flex tw-items-center">
                    <div class="tw-w-10 tw-h-10 tw-rounded-lg tw-flex tw-items-center tw-justify-center" style="background-color: rgba(93,124,21,0.3);">
                      <v-icon color="#5D7C15">mdi-puzzle-outline</v-icon>
                    </div>
                    <div class="tw-ml-4">
                      <h4 class="tw-text-white tw-font-bold">{{ mod.name }}</h4>
                      <p class="tw-text-gray-400 tw-text-sm">{{ mod.version }} | {{ mod.size }}</p>
                    </div>
                  </div>
                  <span class="tw-px-3 tw-py-1 tw-rounded-full tw-text-xs tw-font-bold" :class="mod.required ? 'tw-bg-red-500 tw-text-white' : 'tw-bg-gray-600 tw-text-gray-300'">
                    {{ mod.required ? '必需' : '可选' }}
                  </span>
                </div>
              </div>

              <div class="tw-mt-6 tw-p-4 tw-rounded-lg" style="background-color: rgba(93,124,21,0.2);border-left: 4px solid #5D7C15;">
                <h4 class="tw-text-white tw-font-bold tw-mb-2">📋 安装说明</h4>
                <ol class="tw-text-gray-300 tw-space-y-1 tw-list-decimal tw-list-inside">
                  <li>点击上方按钮下载模组压缩包</li>
                  <li>解压文件到 .minecraft/mods 文件夹</li>
                  <li>确保已安装 Forge 1.20.1</li>
                  <li>启动游戏，选择对应版本进入服务器</li>
                </ol>
              </div>
            </div>
          </div>

          <!-- 教程页面 -->
          <div v-if="activeTab === 'guide'" class="tw-space-y-6">
            <div class="tw-flex tw-items-center tw-mb-6">
              <button @click="activeTab = 'home'" class="tw-mr-4 tw-text-gray-400 hover:tw-text-white">
                <v-icon>mdi-arrow-left</v-icon>
              </button>
              <h2 class="tw-text-2xl tw-font-bold tw-text-white">📚 游戏教程</h2>
            </div>

            <div class="tw-space-y-4">
              <div v-for="(guide, index) in guides" :key="index" class="tw-bg-gray-800 tw-rounded-xl tw-p-5 tw-border tw-border-gray-700">
                <div class="tw-flex tw-items-start">
                  <div class="tw-w-12 tw-h-12 tw-rounded-lg tw-flex tw-items-center tw-justify-center tw-flex-shrink-0" style="background-color: #5D7C15;">
                    <span class="tw-text-white tw-font-bold tw-text-xl">{{ index + 1 }}</span>
                  </div>
                  <div class="tw-ml-4 tw-flex-1">
                    <h3 class="tw-text-lg tw-font-bold tw-text-white tw-mb-2">{{ guide.title }}</h3>
                    <p class="tw-text-gray-400 tw-mb-3">{{ guide.desc }}</p>
                    <div v-if="guide.steps" class="tw-space-y-2">
                      <div v-for="(step, sIndex) in guide.steps" :key="sIndex" class="tw-flex tw-items-center tw-text-gray-300">
                        <v-icon x-small color="#5D7C15" class="tw-mr-2">mdi-check-circle</v-icon>
                        <span class="tw-text-sm">{{ step }}</span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- 关于页面 -->
          <div v-if="activeTab === 'about'" class="tw-space-y-6">
            <div class="tw-flex tw-items-center tw-mb-6">
              <button @click="activeTab = 'home'" class="tw-mr-4 tw-text-gray-400 hover:tw-text-white">
                <v-icon>mdi-arrow-left</v-icon>
              </button>
              <h2 class="tw-text-2xl tw-font-bold tw-text-white">ℹ️ 关于服务器</h2>
            </div>

            <div class="tw-bg-gray-800 tw-rounded-xl tw-p-6 tw-border tw-border-gray-700 tw-text-center">
              <div class="tw-w-24 tw-h-24 tw-mx-auto tw-mb-4 tw-rounded-2xl tw-flex tw-items-center tw-justify-center" style="background: linear-gradient(135deg, #5D7C15 0%, #4a6310 100%);">
                <span class="tw-text-5xl">⛏️</span>
              </div>
              <h3 class="tw-text-2xl tw-font-bold tw-text-white tw-mb-2">SimpFun MC服务器</h3>
              <p class="tw-text-gray-400 tw-mb-4">一个有趣的 Minecraft 生存服务器</p>

              <div class="tw-grid tw-grid-cols-2 tw-gap-4 tw-max-w-md tw-mx-auto tw-mb-6">
                <div class="tw-bg-gray-900 tw-rounded-lg tw-p-3">
                  <p class="tw-text-gray-400 tw-text-sm">开服时间</p>
                  <p class="tw-text-white tw-font-bold">2024年</p>
                </div>
                <div class="tw-bg-gray-900 tw-rounded-lg tw-p-3">
                  <p class="tw-text-gray-400 tw-text-sm">服务器版本</p>
                  <p class="tw-text-white tw-font-bold">1.20.1</p>
                </div>
              </div>

              <div class="tw-space-y-3">
                <a :href="modDownloadUrl" target="_blank" class="tw-inline-block tw-px-6 tw-py-3 tw-rounded-lg tw-font-bold tw-text-white tw-transition-all hover:tw-scale-105" style="background-color: #5D7C15;">
                  <v-icon small color="white" class="tw-mr-2">mdi-web</v-icon>
                  访问模组网站
                </a>
              </div>

              <div class="tw-mt-6 tw-pt-6 tw-border-t tw-border-gray-700">
                <p class="tw-text-gray-500 tw-text-sm">Made with ❤️ for Minecraft Players</p>
                <p class="tw-text-gray-600 tw-text-xs tw-mt-1">服务器地址: {{ serverAddress }}</p>
              </div>
            </div>
          </div>

        </div>

        <!-- 底部状态栏 -->
        <div class="tw-w-full tw-h-10 tw-flex tw-items-center tw-px-4 tw-justify-between" style="background-color: rgba(0,0,0,0.5);border-top: 1px solid #333;">
          <div class="tw-flex tw-items-center tw-gap-4">
            <span class="tw-text-gray-400 tw-text-sm">
              <v-icon x-small color="#5D7C15" class="tw-mr-1">mdi-circle</v-icon>
              已连接
            </span>
            <span class="tw-text-gray-500 tw-text-sm">|</span>
            <span class="tw-text-gray-400 tw-text-sm">版本: 1.20.1</span>
          </div>
          <div class="tw-text-gray-500 tw-text-xs">
            SimpFun Server Launcher v1.0
          </div>
        </div>

      </div>
    </template>
  </Window>
</template>

<script>
import Window from '../WindowBasic/Window.vue'

export default {
  name: 'WindowMinecraft',
  components: {
    Window,
  },
  data() {
    return {
      activeTab: 'home',
      serverAddress: 'play.simpfun.cn:30856',
      modDownloadUrl: 'https://mc.mod.com',
      serverStatus: '在线',
      onlinePlayers: 12,
      maxPlayers: 100,
      tabs: [
        { id: 'home', name: '首页', icon: 'mdi-home' },
        { id: 'mods', name: '模组', icon: 'mdi-puzzle' },
        { id: 'guide', name: '教程', icon: 'mdi-book-open' },
        { id: 'about', name: '关于', icon: 'mdi-information' },
      ],
      features: [
        { icon: '🏠', name: '生存模式', desc: '经典生存玩法' },
        { icon: '🤝', name: '友好社区', desc: '和谐玩家群体' },
        { icon: '📦', name: '丰富模组', desc: '趣味模组扩展' },
      ],
      modList: [
        { name: 'Forge', version: '1.20.1-47.2.0', size: '5.2 MB', required: true },
        { name: 'JourneyMap', version: '5.9.18', size: '3.1 MB', required: false },
        { name: 'JEI', version: '15.2.0.27', size: '1.8 MB', required: false },
        { name: "Xaero's Minimap", version: '23.9.3', size: '2.4 MB', required: false },
      ],
      guides: [
        {
          title: '如何加入服务器',
          desc: '按照以下步骤快速加入我们的服务器',
          steps: [
            '下载并安装 Minecraft 1.20.1 版本',
            '安装 Forge 模组加载器',
            '下载服务器模组并放入 mods 文件夹',
            '启动游戏，选择多人游戏，添加服务器地址'
          ]
        },
        {
          title: '新手生存指南',
          desc: '初入服务器的生存技巧',
          steps: [
            '出生点有新手礼包可以领取',
            '使用 /spawn 返回出生点',
            '使用 /sethome 设置家，/home 回家',
            '与其他玩家友好相处，共同建设'
          ]
        },
        {
          title: '常用指令',
          desc: '服务器常用指令大全',
          steps: [
            '/spawn - 返回出生点',
            '/sethome [名称] - 设置家',
            '/home [名称] - 回家',
            '/tpa [玩家名] - 请求传送到玩家'
          ]
        }
      ]
    }
  },
  props: {
    uuid: String,
    startpos_x: {
      default: 60,
    },
    startpos_y: {
      default: 60
    },
    zindex: {
      type: Number,
      default: 999,
    },
    minimized: {
      type: Boolean,
      default: false,
    }
  },
  created() {
    // 模拟获取服务器状态
    this.fetchServerStatus()
  },
  methods: {
    copyAddress() {
      navigator.clipboard.writeText(this.serverAddress).then(() => {
        alert('服务器地址已复制到剪贴板！')
      }).catch(() => {
        // 降级方案
        const input = document.createElement('input')
        input.value = this.serverAddress
        document.body.appendChild(input)
        input.select()
        document.execCommand('copy')
        document.body.removeChild(input)
        alert('服务器地址已复制到剪贴板！')
      })
    },
    fetchServerStatus() {
      // 这里可以添加真实的服务器状态 API 调用
      // 目前使用模拟数据
    },
    mr_clicked() {
      this.$store.commit('show_context_menu')
    }
  }
}
</script>

<style scoped>
/* 自定义滚动条 */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: #1a1a2e;
}

::-webkit-scrollbar-thumb {
  background: #5D7C15;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: #4a6310;
}
</style>
