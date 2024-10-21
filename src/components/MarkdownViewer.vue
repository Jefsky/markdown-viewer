<template>
    <div class="markdown-container">
        <div class="sidebar">
            <button @click="triggerFileUpload" class="upload-button">Choose File</button>
            <span v-if="fileName" class="file-name">{{ fileName }}</span> <!-- 显示文件名 -->
            <input type="file" ref="fileInput" @change="handleFileUpload" accept=".md" class="file-input" />
            <h3>目录</h3>
            <ul>
                <li v-for="item in toc" :key="item.id" :class="{ active: currentActive === item.id }">
                    <a href="#" @click.prevent="scrollTo(item.id)">
                        {{ item.text }}
                    </a>
                </li>
            </ul>
        </div>
        <div class="content">
            <div v-html="htmlContent" class="markdown-content"></div>
        </div>
        <button v-if="showScrollButton" @click="scrollToTop" class="scroll-to-top">
            Back to Top
        </button>
    </div>
</template>

<script>
import { marked } from 'marked'; // 引入 marked
import VueScrollTo from 'vue-scrollto'; // 引入 vue-scrollto

class Slugger {
    constructor() {
        this.seen = new Set();
    }

    slug(value) {
        let slug = value
            .toLowerCase()
            .replace(/[^\w]+/g, '-')
            .replace(/^-|-$/g, '');
        let uniqueSlug = slug;
        let counter = 1;

        while (this.seen.has(uniqueSlug)) {
            uniqueSlug = `${slug}-${counter++}`;
        }

        this.seen.add(uniqueSlug);
        return uniqueSlug;
    }
}

export default {
    data() {
        return {
            markdownContent: '',
            htmlContent: '',
            toc: [],
            showScrollButton: false,
            fileName: '', // 新增属性，用于存储文件名
            currentActive: null, // 用于跟踪当前激活的项
        };
    },
    mounted() {
        window.addEventListener('scroll', this.handleScroll);
    },
    beforeUnmount() {
        window.removeEventListener('scroll', this.handleScroll);
    },
    methods: {
        triggerFileUpload() {
            this.$refs.fileInput.click(); // 点击隐藏的文件输入框
        },
        handleFileUpload(event) {
            const file = event.target.files[0];
            // 检查文件扩展名而不是 MIME 类型
            if (file && file.name.endsWith('.md')) {
                this.fileName = file.name; // 设置文件名
                const reader = new FileReader();
                reader.onload = (e) => {
                    this.markdownContent = e.target.result;
                    this.generateHtml();
                };
                reader.readAsText(file);
            } else {
                alert('请上传正确的Markdown文件');
                this.fileName = ''; // 清空文件名
            }
        },
        generateHtml() {
            const toc = [];
            const slugger = new Slugger();
            const renderer = new marked.Renderer();

            renderer.heading = (text, level) => {
                if (typeof text !== 'string') {
                    console.error('Invalid heading text:', text);
                    text = text.text || JSON.stringify(text);
                }

                const id = slugger.slug(text.trim());
                toc.push({ id, text, level });
                return `<h${level} id="${id}">${text}</h${level}>`;
            };

            this.htmlContent = marked(this.markdownContent, { renderer });
            this.toc = toc;
        },
        scrollTo(id) {
            const el = document.getElementById(id);
            if (el) {
                VueScrollTo.scrollTo(el, 500);
                this.currentActive = id; // 更新当前激活的项
            }
        },
        scrollToTop() {
            window.scrollTo({ top: 0, behavior: 'smooth' });
        },
        handleScroll() {
            this.showScrollButton = window.scrollY > 300;
        },
    },
};
</script>

<style scoped>
.markdown-container {
    display: flex;
    padding: 20px;
    font-family: Arial, sans-serif;
}

.sidebar {
    width: 250px;
    /* 可以适当调整宽度 */
    margin-right: 20px;
    background-color: #f4f4f9;
    /* 背景色 */
    padding: 20px;
    /* 增加内边距 */
    border-radius: 8px;
    /* 圆角 */
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    /* 阴影效果 */
    position: sticky;
    /* 固定在视口中 */
    top: 20px;
    /* 固定的顶部距离 */
    height: calc(100vh - 40px);
    /* 使侧边栏高度与视口高度一致 */
    overflow-y: auto;
    /* 允许垂直滚动 */
}

h3 {
    margin-bottom: 15px;
    /* 标题与下方内容的间距 */
    font-size: 1.5rem;
    /* 字体大小 */
    color: #333;
    /* 字体颜色 */
}

.upload-button {
    padding: 10px 15px;
    /* 按钮内边距 */
    background-color: #007bff;
    /* 按钮背景色 */
    color: white;
    /* 按钮文字颜色 */
    border: none;
    /* 去掉边框 */
    border-radius: 5px;
    /* 圆角按钮 */
    cursor: pointer;
    /* 鼠标悬停效果 */
    margin-bottom: 10px;
    /* 按钮与文件名的间距 */
    transition: background-color 0.3s;
    /* 平滑过渡效果 */
}

.upload-button:hover {
    background-color: #0056b3;
    /* 悬停时的背景色 */
}

.file-name {
    display: block;
    /* 显示为块元素 */
    margin-bottom: 10px;
    /* 文件名与目录的间距 */
    color: #555;
    /* 文件名颜色 */
    font-size: 0.9rem;
    /* 字体大小 */
}

ul {
    list-style-type: none;
    /* 去掉列表样式 */
    padding: 0;
    /* 去掉内边距 */
    margin: 0;
    /* 去掉外边距 */
}

li {
    margin-bottom: 8px;
    /* 列表项的间距，减少到8px */
    text-align: left;
    /* 确保文本左对齐 */
    transition: background-color 0.3s, box-shadow 0.3s;
    /* 平滑过渡效果 */
    padding: 10px;
    /* 减少内边距 */
    line-height: 1.2;
    /* 调整行高 */
    border-radius: 5px;
    /* 圆角效果 */
}

li:hover {
    background-color: #e7f1ff;
    /* 鼠标悬停时的背景色 */
}

li.active {
    background-color: #007bff;
    /* 选中状态的背景色 */
    color: white;
    /* 选中状态的字体颜色 */
    box-shadow: 0 0 10px rgba(0, 123, 255, 0.5);
    /* 选中状态的阴影效果 */
}

li.active a {
    color: white;
    /* 确保选中状态下的链接颜色为白色 */
}

li a {
    display: block;
    /* 使链接占满整个 li 的区域 */
    padding: 10px;
    /* 增加内边距，使链接区域更大 */
    text-decoration: none;
    /* 去掉链接下划线 */
    color: #007bff;
    /* 链接颜色 */
    transition: color 0.3s;
    /* 平滑过渡效果 */
}

li a:hover {
    color: #0056b3;
    /* 链接悬停时的颜色 */
}

.content {
    margin-left: 240px;
    /* 给内容区留出侧边栏的空间 */
    flex: 1;
    overflow: auto;
}

.markdown-content {
    text-align: left;
    padding: 20px;
    background-color: #ffffff;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.scroll-to-top {
    position: fixed;
    bottom: 50px;
    right: 30px;
    display: inline-block;
    padding: 10px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.file-input {
    margin-bottom: 10px;
    /* 调整文件输入框的底部间距 */
}

.upload-button {
    padding: 10px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    margin-bottom: 10px;
    /* 添加底部间距 */
}

.upload-button:hover {
    background-color: #0056b3;
    /* 悬停时的背景色 */
}

.file-name {
    display: block;
    /* 使文件名显示为块元素 */
    margin-bottom: 10px;
    /* 添加底部间距 */
    color: #555;
    /* 颜色可以根据需要调整 */
}

.file-input {
    display: none;
    /* 隐藏原始文件输入框 */
}
</style>
