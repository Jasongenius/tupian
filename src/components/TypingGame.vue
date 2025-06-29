<template>
  <div class="app-container">
    <!-- 顶部栏 -->
    <div class="top-bar">
      <div class="logo-area">
        <img src="logo.png" alt="Logo" class="logo" />
        <span class="logo-text">TOURFLY</span>
      </div>
      <div class="control-area">
        <input v-model="searchQuery" type="text" placeholder="搜索" class="search-input" />
        <select v-model="sortOption" class="sort-select">
          <option value="created">创建时间</option>
          <option value="name">图片名称</option>
        </select>
        <button @click="toggleTags" class="tag-button">标签选择器</button>
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
        @click="showDetails(image)"
      >
        <img :src="image.src" :alt="image.name" />
      </div>
    </div>

    <!-- 图片详细信息弹窗 -->
    <div v-if="selectedImage" class="image-details-modal">
      <div class="modal-overlay" @click="closeDetails"></div>
      <div class="modal-content">
        <div class="modal-header">
          <!-- <h3>{{ selectedImage.name }}</h3> -->
          <button @click="closeDetails" class="close-button">关闭</button>
        </div>
        <div class="modal-body">
          <div class="image-preview">
            <img :src="selectedImage.src" :alt="selectedImage.name" class="preview-image" />
          </div>
          <div class="info-panel">
            <p><strong>使用模型：</strong>{{ selectedImage.model }}</p>
            <p><strong>类型：</strong>{{ selectedImage.type }}</p>
            <p><strong>尺寸：</strong>{{ selectedImage.size }}</p>
            <p><strong>种子：</strong>{{ selectedImage.seed }}</p>
            <p><strong>步数：</strong>{{ selectedImage.steps }}</p>
          </div>
        </div>
        <div class="modal-footer">
          <button @click="redrawImage">重新绘制</button>
          <button @click="downloadImage">点击下载</button>
        </div>
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
        { src: require('../assets/images/image1.jpg'), name: '幻想森林', tags: ['动植物', '插画'], created: '2023-12-01', model: 'Model A', type: '插画', size: '1920x1080', seed: '12345', steps: '50' },
        { src: require('../assets/images/image2.jpg'), name: '未来建筑', tags: ['建筑', '科幻'], created: '2023-11-15', model: 'Model B', type: '建筑', size: '1280x720', seed: '67890', steps: '30' },
        { src: require('../assets/images/image3.jpg'), name: '搞怪水果', tags: ['3D', '商品'], created: '2024-01-10', model: 'Model C', type: '3D', size: '1024x768', seed: '11223', steps: '40' },
        { src: require('../assets/images/image4.jpg'), name: '女战士', tags: ['人物', '时尚设计'], created: '2023-10-20', model: 'Model D', type: '人物', size: '1920x1080', seed: '44556', steps: '25' },
        { src: require('../assets/images/image5.jpg'), name: '动漫女孩', tags: ['卡通', '头像'], created: '2023-09-05', model: 'Model E', type: '卡通', size: '512x512', seed: '77889', steps: '35' },
        { src: require('../assets/images/image6.jpg'), name: '梦幻狐狸', tags: ['插画', '动植物'], created: '2023-12-25', model: 'Model F', type: '插画', size: '800x600', seed: '99001', steps: '45' },
      ],
      selectedImage: null
    };
  },
  computed: {
    filteredImages() {
      let result = this.images;

      if (this.selectedTags.length > 0) {
        result = result.filter(image =>
          image.tags.some(tag => this.selectedTags.includes(tag))
        );
      }

      if (this.searchQuery.trim() !== '') {
        const keyword = this.searchQuery.trim().toLowerCase();
        result = result.filter(image =>
          image.name.toLowerCase().includes(keyword)
        );
      }

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
      this.showTags = !this.showTags;
    },
    toggleTag(tag) {
      const index = this.selectedTags.indexOf(tag);
      if (index >= 0) {
        this.selectedTags.splice(index, 1);
      } else {
        this.selectedTags.push(tag);
      }
    },
    showDetails(image) {
      this.selectedImage = image;
    },
    closeDetails() {
      this.selectedImage = null;
    },
    redrawImage() {
      alert('重新绘制功能尚未实现');
    },
    downloadImage() {
      const link = document.createElement('a');
      link.href = this.selectedImage.src;
      link.download = this.selectedImage.name + '.jpg';
      link.click();
    }
  }
};
</script>

<style scoped>
.app-container {

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

/* 弹窗相关 */
.image-details-modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1000;
}

.modal-overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
}

.modal-content {
  position: relative;
  margin: 50px auto;
  background: white;
  width: 80%;
  max-width: 900px;
  border-radius: 8px;
  overflow: hidden;
  z-index: 1001;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  padding: 10px 20px;
  background-color: #007bff;
  color: white;
}

.close-button {
  background: none;
  border: none;
  color: white;
  font-size: 18px;
  cursor: pointer;
}

.modal-body {
  display: flex;
  gap: 20px;
  padding: 20px;
}

.image-preview {
  flex: 1;
  max-width: 50%;
}

.preview-image {
  width: 100%;
  height: auto;
  border-radius: 8px;
}

.info-panel {
  flex: 1;
  font-size: 14px;
  line-height: 1.8;
}

.modal-footer {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  padding: 10px 20px;
  border-top: 1px solid #ccc;
}

.modal-footer button {
  padding: 6px 12px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.modal-footer button:hover {
  background-color: #0056b3;
}
</style>
