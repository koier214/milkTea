<script setup>
import { ref, onMounted } from 'vue'

// 用户名数组，页面加载时随机选一个
const usernames = ["苏菲","高田穗本","水稚青信","水至清信","上岛夜芊子","宫崎病","笨蛋"]
const username = ref('')

// 奶茶门店和饮品映射
const teaDoorList = [
            "库迪","瑞幸Lucky","蜜雪冰城","茶百道","茉莉奶白"
             ,"古茗","喜茶","霸王茶姬","书亦烧仙草","豆吉",
             "沪上阿姨"
        ];
const teaMap = {
            "茶百道": [
                "竹香系列：鲜奶竹香乌龙",
                "竹香系列：竹香酒酿",
                "竹香系列：竹香猕猴桃",
                "超级蔬食：轻畅羽衣甘蓝",
                "超级蔬食：闪充·清体小麦草",
                "真鲜奶茶：鲜奶茉莉奶绿",
                "真鲜奶茶：鲜奶桂花金萱",
                "真鲜奶茶：鲜奶奇香乌龙",
                "招牌奶茶：手炒黑糖珍珠鲜奶",
                "招牌奶茶：黑糖珍珠奶茶pro",
                "招牌奶茶：超厚芋泥奶绿",
                "招牌奶茶：超厚芋泥奶茶",
                "招牌奶茶：抹茶奶布丁",
                "招牌奶茶：椰椰桂花乌龙",
                "招牌奶茶：铁观音奶冻",
                "招牌奶茶：招聘芋圆奶茶",
                "招牌奶茶：豆乳玉麒麟",
                "招牌奶茶：豆乳米麻薯",
                "招牌奶茶：茉莉奶绿",
                "招牌奶茶：奥利奥奶茶",
                "清爽果茶：草莓酒酿",
                "清爽果茶：草莓爆柠",
                "清爽果茶：草莓奶冻",
                "清爽果茶：耙耙杠上花",
                "清爽果茶：蜀实耙耙柑",
                "清爽果茶：百香凤梨",
                "清爽果茶：杨枝甘露",
                "清爽果茶：多芒杨枝甘露",
                "清爽果茶：青提茉莉",
                "清爽果茶：西瓜啵啵",
                "清爽果茶：超级杯水果茶",
                "千亿活菌酸奶昔：活菌·酸奶昔紫米露",
                "轻松现泡茶：金萱乌龙",
                "轻松现泡茶：绿茶",
                "轻松现泡茶：岩茶"
            ],
            "喜茶": ["芝芝桃桃", "满杯红柚", "多肉葡萄"],
            "蜜雪冰城": ["冰鲜柠檬水", "芝芝葡萄", "草莓啵啵"],
            
        };

const selectedTeaDoor = ref(null)          // 用户选择的门店
const selectedMilkTea = ref('')            // 抽中的奶茶
const showMilkTeaGrid = ref(false)         // 是否显示奶茶
const showConfirm = ref(false)             // 是否显示确认/取消
const milkTeaCount = ref(0)                // 每周奶茶数

onMounted(() => {
  username.value = usernames[Math.floor(Math.random() * usernames.length)]
})

// 用户点击门店进行选择
const selectTeaDoor = (door) => {
  selectedTeaDoor.value = door
  selectedMilkTea.value = ''
  showMilkTeaGrid.value = false
  showConfirm.value = false
}

// 点击开始抽奶茶
const startRandom = () => {
  if (!selectedTeaDoor.value) {
    alert("请先选择一个奶茶门店！");
    return;
  }

  showMilkTeaGrid.value = true
  showConfirm.value = false
  selectedMilkTea.value = ''

  const milkTeas = teaMap[selectedTeaDoor.value]

  let count = 0
  const interval = setInterval(() => {
    selectedMilkTea.value = milkTeas[Math.floor(Math.random() * milkTeas.length)]
    count++
    if (count > 15) {
      clearInterval(interval)
      showConfirm.value = true
    }
  }, 100)
}

const confirmMilkTea = () => {
  milkTeaCount.value++
  showMilkTeaGrid.value = false
  showConfirm.value = false
  selectedMilkTea.value = ''
}

const cancelSelection = () => {
  selectedMilkTea.value = ''
  showConfirm.value = false
}
</script>



<template>
  <div class="container">
    <div class="title">
      <p>Hi，{{ username }}！</p>
      <div class="info">本周奶茶数：{{ milkTeaCount }}</div>
    </div>

    <div class="content">
        <div class="chooseBtn">
            <!-- 显示当前抽中的门店 -->
            <!-- 门店选择区域 -->
            <div class="teaDoor">
            <div
                class="teaDoorItem"
                v-for="(item, index) in teaDoorList"
                :key="index"
                :class="{ selected: item === selectedTeaDoor }"
                @click="selectTeaDoor(item)"
            >
                {{ item }}
            </div>
            </div>

            <!-- 显示当前选择的门店 -->
            <div v-if="selectedTeaDoor">
            <p>当前选择门店：<strong>{{ selectedTeaDoor }}</strong></p>
            </div>


            <!-- 抽奖按钮 -->
            <button @click="startRandom">开始随机~</button>

            <!-- 抽中的奶茶 -->
            <div class="milkTea" v-if="showMilkTeaGrid">
            <p>你抽到了：<strong>{{ selectedMilkTea }}</strong></p>
            </div>

            <!-- 动画文字 -->
            <div class="startBtn" v-if="!showConfirm && showMilkTeaGrid">
            <p>抽选中...</p>
            </div>

            <!-- 确认取消 -->
            <div class="actions" v-if="showConfirm">
            <button class="confirm" @click="confirmMilkTea">确认</button>
            <button class="cancel" @click="cancelSelection">取消</button>
            </div>
        </div>
        </div>


  </div>
</template>

<style scoped>
.container {
  padding: 20px;
  max-width: 400px;
  margin: auto;
  font-family: sans-serif;
  text-align: center;
  background-color: #fff8f0;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

.title {
  margin-bottom: 20px;
}
.info {
  font-size: 14px;
  color: #555;
  margin-top: 5px;
}

.chooseBtn button {
  margin: 10px;
  padding: 10px 20px;
  background-color: #ffce99;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
}

.milkTea {
  margin: 15px 0;
  font-size: 18px;
  color: #d2691e;
}

.actions button {
  margin: 5px;
  padding: 8px 16px;
  border-radius: 8px;
  border: none;
}

.confirm {
  background-color: #99cc99;
}
.cancel {
  background-color: #ff9999;
}
.teaDoor {
  display: flex;
  justify-content: center;
  margin-bottom: 10px;
  flex-wrap: wrap;
  gap: 10px;
}

.teaDoorItem {
  padding: 10px 16px;
  margin: 0 5px;
  border-radius: 10px;
  background-color: #f0e68c;
  cursor: pointer;
  font-weight: bold;
  transition: 0.2s;
}

.teaDoorItem.selected {
  background-color: #ffa07a;
  color: white;
  box-shadow: 0 0 8px #ffb347;
}

</style>
