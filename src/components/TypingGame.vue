<template>
  <div class="app-container">
    <!-- 顶部栏 -->
    <div class="top-bar">
      <!-- Logo -->
      <div class="logo-area">
        <img src="logo.png" alt="Logo" class="logo" />
        <span class="logo-text">TOURFLY</span>
      </div>

      <!-- 搜索+排序+标签 -->
      <div class="control-area">
        <input
          v-model="searchQuery"
          type="text"
          placeholder="搜索"
          class="search-input"
        />
        <select v-model="sortOption" class="sort-select">
          <option value="created">创建时间</option>
          <option value="name">图片名称</option>
        </select>
        <button @click="toggleTags" class="tag-button">
          标签选择器
        </button>
      </div>
    </div>

    <!-- 标签列表 -->
    <div v-if="showTags" class="tag-list">
      <button
        v-for="tag in tags"
        :key="tag"
        @click="toggleTag(tag)"
        :class="['tag', { selected: selectedTags.includes(tag) }]"
      >
        {{ tag }}
      </button>
    </div>

    <!-- 图片展示区 -->
    <div class="image-grid">
      <div
        v-for="(image, index) in filteredImages.slice(0, 12)"
        :key="index"
        class="image-item"
      >
        <img :src="image.src" :alt="image.name" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: '',
      sortOption: 'created',
      showTags: true,
      selectedTags: [],
      tags: [
        '动植物', '印花', 'T恤图案', '人物', 'Logo', '时尚设计', '纹样', '摄影',
        '风景', '头像', '商品', '海报', '游戏', '3D', '科幻', '艺术',
        '建筑', '卡通', '插画', '漫画'
      ],
      images: [
        { src: require('../assets/images/image1.jpg'), name: '幻想森林', tags: ['动植物', '插画'], created: '2023-12-01' },
        { src:  require('../assets/images/image2.jpg'), name: '未来建筑', tags: ['建筑', '科幻'], created: '2023-11-15' },
        { src:  require('../assets/images/image3.jpg'), name: '搞怪水果', tags: ['3D', '商品'], created: '2024-01-10' },
        { src: require('../assets/images/image4.jpg'), name: '女战士', tags: ['人物', '时尚设计'], created: '2023-10-20' },
        { src: require('../assets/images/image5.jpg'), name: '动漫女孩', tags: ['卡通', '头像'], created: '2023-09-05' },
        { src: require('../assets/images/image6.jpg'), name: '梦幻狐狸', tags: ['插画', '动植物'], created: '2023-12-25' },
        // { src: 'img7.jpg', name: '线条艺术', tags: ['艺术', '印花'], created: '2024-02-18' },
        // { src: 'img8.jpg', name: '机械昆虫', tags: ['科幻', '3D'], created: '2024-01-02' },
        // { src: 'img9.jpg', name: '时尚女郎', tags: ['人物', '时尚设计'], created: '2024-03-08' },
        // { src: 'img10.jpg', name: '未来大厅', tags: ['建筑'], created: '2023-08-11' },
        // { src: 'img11.jpg', name: '发光动物', tags: ['动植物'], created: '2023-07-22' },
        // { src: 'img12.jpg', name: '动态海报', tags: ['海报', '艺术'], created: '2023-06-30' },
      ]
    }
  },
  computed: {
  filteredImages() {
  let result = this.images;

  // ✅ 标签过滤（包含任意一个选中的标签）
  if (this.selectedTags.length > 0) {
    result = result.filter(image =>
      image.tags.some(tag => this.selectedTags.includes(tag))
    );
  }

  // 搜索过滤
  if (this.searchQuery.trim() !== '') {
    const keyword = this.searchQuery.trim().toLowerCase();
    result = result.filter(image =>
      image.name.toLowerCase().includes(keyword)
    );
  }

  // 排序
  if (this.sortOption === 'created') {
    result = result.sort((a, b) => new Date(b.created) - new Date(a.created));
  } else if (this.sortOption === 'name') {
    result = result.sort((a, b) => a.name.localeCompare(b.name));
  }

  return result;
}
  },
  methods: {
    toggleTags() {
      this.showTags = !this.showTags
    },
    toggleTag(tag) {
      const index = this.selectedTags.indexOf(tag)
      if (index >= 0) {
        this.selectedTags.splice(index, 1)
      } else {
        this.selectedTags.push(tag)
      }
    }
  }
}
</script>

<style scoped>
.app-container {
  padding: 20px;
  font-family: Arial, sans-serif;
}

.top-bar {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.logo-area {
  display: flex;
  align-items: center;
}

.logo {
  height: 32px;
  margin-right: 10px;
}

.logo-text {
  font-size: 20px;
  color: #007bff;
  font-weight: bold;
}

.control-area {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: 10px;
}

.search-input {
  padding: 5px 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.sort-select {
  padding: 5px;
  border-radius: 4px;
}

.tag-button {
  background-color: #f60;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
}

.tag-button:hover {
  background-color: #e55200;
}

.tag-list {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-bottom: 20px;
}

.tag {
  padding: 6px 12px;
  border: 1px solid #007bff;
  border-radius: 4px;
  background-color: white;
  color: #007bff;
  cursor: pointer;
}

.tag.selected {
  background-color: #007bff;
  color: white;
}

.image-grid {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 10px;
}

.image-item {
  width: 100%;
  height: 120px;
  overflow: hidden;
  border-radius: 4px;
  background-color: #f0f0f0;
}

.image-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style>
