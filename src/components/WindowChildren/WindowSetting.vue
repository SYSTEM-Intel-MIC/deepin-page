<template>
  <Window :uuid='uuid' :startpos_x="startpos_x" :startpos_y="startpos_y" :zindex="zindex"  :minimized="minimized">
    <template v-slot:header>
      <div class="tw-flex tw-items-center tw-select-none" style="pointer-events:none;"> 
        <img src="../../assets/images/icons/settings.png" alt="" style="pointer-events:auto;" class=" tw-w-7 tw-h-7 tw-ml-4">
        <div class=" tw-ml-4 tw-font-bold tw-tracking-wider" style="pointer-events:auto;"> 系统设置 </div>
      </div>
    </template>
    <template v-slot:content>
      <div class=" tw-w-full  tw-h-full tw-rounded-b-2xl tw-overflow-hidden tw-flex
       tw-select-none" @contextmenu.prevent="mr_clicked">
        <div class=" tw-w-52 tw-bg-white tw-h-full tw-flex tw-flex-col tw-px-3 tw-py-2 tw-flex-none" >
          <WindowSettingIcon :tag="'关于'" :img="'profile'" :selected_tag="selected_tag" @click.native="{selected_tag = '关于';selected_tag_2 = '系统消息'}"/>
          <WindowSettingIcon :tag="'Skills'" :img="'skills'" :selected_tag="selected_tag"  @click.native="{selected_tag = 'Skills';selected_tag_2 = 'And Some Else'}"/>
          <WindowSettingIcon :tag="'Resume'" :img="'paint'" :selected_tag="selected_tag"  @click.native="selected_tag = 'Resume'"/>
        </div>
        <div class=" tw-flex-grow tw-h-full  tw-p-2">
          <div class="tw-w-full tw-h-full  tw-flex">
            <div class="tw-w-48 tw-flex-none tw-rounded-xl tw-bg-white tw-p-2">
              <div class="tw-w-full tw-h-full" v-if="selected_tag === '关于'">
                <WindowSettingIcon :tag="'系统信息'" :mdi="'beaker-question'" :selected_tag="selected_tag_2"  @click.native="selected_tag_2 = 'up主'"/>
                <WindowSettingIcon :tag="'开源声明'" :mdi="'card-account-details-star'" :selected_tag="selected_tag_2"  @click.native="selected_tag_2 = '开源声明'"/>
              </div>
              <div class="tw-w-full tw-h-full" v-if="selected_tag === 'Skills'">
                <WindowSettingIcon :tag="'And Some Else'" :mdi="'card-account-details-star'" :selected_tag="selected_tag_2"  @click.native="selected_tag_2 = 'And Some Else'"/>
              </div>
            </div>
            <div class="vl"></div>
            <div class="tw-flex-grow tw-bg-white tw-h-full">
              <div class=" tw-w-full tw-h-full" v-if="selected_tag === '关于'">
                <div ref="overall_page" class="tw-w-full tw-h-full tw-items-center tw-flex tw-flex-col" style="text-align:center" v-if="selected_tag_2 ==='系统消息'">
                  <div class=" tw-w-20 tw-h-20 tw-rounded-full tw-overflow-hidden tw-mt-16">
                    <img src="../../assets/images/deepin_2.png" alt="" class="tw-bg-red-500">
                  </div>
                  <div class="tw-mt-3 tw-text-gray-400">deepin</div>
                  <div class="tw-text-xl tw-mt-2 tw-tracking-wide"> 网页模拟深度操作系统</div>
                  <div class="tw-text-lg tw-mt-2 tw-tracking-wide"> 一个有趣的网站，可惜目前只适配电脑端 </div>
                </div>
                <div ref="github_page" class="tw-w-full tw-h-full tw-items-center tw-flex tw-flex-col tw-justify-center" style="text-align:center" v-if="selected_tag_2 ==='开源声明'">
                  <div class="tw-text-xl tw-mt-2 tw-tracking-wide">开源声明</div>
                  <div class="tw-px-4 tw-py-6 tw-max-w-lg">
                    <p class="tw-leading-relaxed">
                      本项目遵循 AGPL-3.0 协议开源，不用于商业用途。
                    </p>
                    <p class="tw-text-sm tw-text-gray-600 tw-mt-4">
                      原项目作者：GoodManWEN
                    </p>
                  </div>
                </div>
              </div>
              <div class="tw-w-full tw-h-full" v-if="selected_tag === 'Skills'">
                <div ref="github_page" class="tw-w-full tw-h-full tw-items-center tw-flex tw-flex-col tw-justify-center tw-mb-16" style="text-align:center" v-if="selected_tag_2 ==='And Some Else'">
                  <img src="../../assets/images/holo.gif" alt="" class="">
                  <div class="tw-text-lg tw-mt-2 tw-tracking-wide tw-mb-10"> Thanks for watching this demo, hope you enjoy it. </div>
                </div> 
              </div>
              <div class="tw-w-full tw-h-full" v-if="selected_tag === 'Resume'">
                <div ref="overall_page" class="tw-w-full tw-h-full tw-items-center tw-flex tw-flex-col tw-justify-center" style="text-align:center">
                  <div class="tw-text-4xl tw-mt-2 tw-tracking-wider"> Coming Soon </div>
                </div>
              </div>
            </div>
            
          </div>
        </div>
      </div>
    </template>
  </Window>
</template>

<script>
import Window from '../WindowBasic/Window.vue'
import WindowSettingIcon from './WindowSettingIcon.vue'

export default {
  name: 'WindowSettings',
  components: {
    Window,
    WindowSettingIcon,
  },
  data(){
    return {
      selected_tag:"关于",
      selected_tag_2:"系统信息",
    }
  },
  props:{
    uuid:String,
    startpos_x:{
      default:60,
    },
    startpos_y:{
      default:60
    },
    zindex:{
      type:Number,
      default:999,
    },
    minimized:{
      type:Boolean,
      default:false,
    },
    default_width:{
      type:Number,
      default:680
    }
  },
  created(){
  },
  mounted(){
    
  },
  watch:{
  },
  computed:{
  },
  methods:{
    mr_clicked(){
      this.$store.commit('show_context_menu')
    }
  }
}
</script>

<style scoped>
.vl {
  border-left: 1.5px solid rgba(244,244,244);
  height: 100%;
  left: 50%;
}
</style>
