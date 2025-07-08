---
layout: page
title: 我的相册
---

<style>
  .album-categories { margin-bottom: 20px; }
  .category-link { margin-right: 15px; color: #ffaa33; text-decoration: none; }
  .category-link:hover { text-decoration: underline; }
  .album-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 15px; margin-top: 20px; }
  .grid-item { cursor: pointer; }
  .grid-item img { width: 100%; height: auto; border-radius: 4px; transition: transform 0.2s; }
  .grid-item img:hover { transform: scale(1.05); }
  .hidden { display: none; }
  .modal { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); display: none; justify-content: center; align-items: center; z-index: 1000; }
  .modal-content { max-width: 90%; max-height: 90%; }
  .modal img { max-width: 100%; max-height: 90vh; }
  .close { position: absolute; top: 20px; right: 30px; color: white; font-size: 30px; cursor: pointer; }
</style>

<section>
  <h3>我的相册</h3>
  <div class="album-categories">
    <a href="#" class="category-link" data-category="category1">示例照片 1</a>
    <a href="#" class="category-link" data-category="category2">示例照片 2</a>
    <a href="#" class="category-link" data-category="category3">算法可视化</a>
    <a href="#" class="category-link" data-category="category4">项目结果展示</a>
  </div>

  <!-- 示例照片 1 分类 -->
  <div id="category1" class="album-grid">
    <div class="grid-item"><img src="{{ '/assets/Clipboard-1.png' | prepend: site.baseurl | replace: '//', '/' }}" alt="示例图片 1" onclick="openModal(this)"></div>
    <div class="grid-item"><img src="{{ '/assets/Clipboard-2.png' | prepend: site.baseurl | replace: '//', '/' }}" alt="示例图片 2" onclick="openModal(this)"></div>
  </div>

  <!-- 示例照片 2 分类 -->
  <div id="category2" class="album-grid hidden">
    <div class="grid-item"><img src="{{ '/assets/Clipboard-3.png' | prepend: site.baseurl | replace: '//', '/' }}" alt="示例图片 3" onclick="openModal(this)"></div>
    <div class="grid-item"><img src="{{ '/assets/Clipboard-4.png' | prepend: site.baseurl | replace: '//', '/' }}" alt="示例图片 4" onclick="openModal(this)"></div>
  </div>

  <!-- 算法可视化分类 -->
  <div id="category3" class="album-grid hidden">
    <div class="grid-item"><img src="{{ '/assets/array_meet.png' | prepend: site.baseurl | replace: '//', '/' }}" alt="算法示意图" onclick="openModal(this)"></div>
    <div class="grid-item"><img src="{{ '/assets/list.png' | prepend: site.baseurl | replace: '//', '/' }}" alt="列表示意图" onclick="openModal(this)"></div>
  </div>

  <!-- 项目结果展示分类 -->
  <div id="category4" class="album-grid hidden">
    <div class="grid-item"><img src="{{ '/assets/results.png' | prepend: site.baseurl | replace: '//', '/' }}" alt="项目结果" onclick="openModal(this)"></div>
    <div class="grid-item"><img src="{{ '/assets/PR_curve.png' | prepend: site.baseurl | replace: '//', '/' }}" alt="PR曲线" onclick="openModal(this)"></div>
  </div>
</section>

<!-- 图片放大模态框 -->
<div id="imageModal" class="modal">
  <span class="close" onclick="closeModal()">&times;</span>
  <div class="modal-content">
    <img id="modalImage" src="" alt="放大图片">
  </div>
</div>

<script>
  // 显示选中分类的图片网格
  document.querySelectorAll('.category-link').forEach(link => {
    link.addEventListener('click', function(e) {
      e.preventDefault();
      const category = this.getAttribute('data-category');
      
      // 隐藏所有网格
      document.querySelectorAll('.album-grid').forEach(grid => {
        grid.classList.add('hidden');
      });
      
      // 显示选中分类的网格
      document.getElementById(category).classList.remove('hidden');
    });
  });

  // 打开图片模态框
  function openModal(img) {
    const modal = document.getElementById('imageModal');
    const modalImg = document.getElementById('modalImage');
    modal.style.display = 'flex';
    modalImg.src = img.src;
  }

  // 关闭图片模态框
  function closeModal() {
    document.getElementById('imageModal').style.display = 'none';
  }

  // 点击模态框外部关闭
  window.onclick = function(e) {
    const modal = document.getElementById('imageModal');
    if (e.target == modal) {
      closeModal();
    }
  }
</script>