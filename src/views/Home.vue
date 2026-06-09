<script setup>
import { ref } from 'vue';

// 화면에 보여줄 짤 데이터를 담을 변수 (반응형 데이터)
const currentMeme = ref(null);

// 백엔드 API를 호출해서 랜덤 짤을 가져오는 함수
const fetchRandomMeme = async () => {
    try {
        // 💡 주의: 아직 백엔드 API를 안 만들어서 지금 누르면 에러가 날 거야!
        const response = await fetch('http://localhost:8080/api/memes/random');
    
        if (!response.ok) {
            throw new Error('데이터를 불러오는데 실패했습니다.');
        }
    
        const data = await response.json();
        currentMeme.value = data; // 백엔드에서 받은 데이터를 변수에 쏙 넣어줌
    
        } catch (error) {
        console.error('에러 발생:', error);
        alert('백엔드 서버 API가 아직 준비되지 않았습니다!');
        }
};
</script>

<template>
    <div class="home">
        <h1>🐱 랜덤 고양이 짤 자판기 🐾</h1>
    
        <div v-if="currentMeme" class="meme-card">
        <img :src="currentMeme.imageUrl" alt="고양이 짤" class="cat-image" />
        <p class="quote">"{{ currentMeme.quote }}"</p>
        <p class="tags">{{ currentMeme.tags }}</p>
    </div>
    
    <div v-else class="empty-state">
        <p>버튼을 눌러서 귀여운 고양이를 뽑아보세요!</p>
    </div>

    <button @click="fetchRandomMeme" class="gacha-btn">
        🎁 랜덤 짤 뽑기!
    </button>
</div>
</template>

<style scoped>
.home {
    text-align: center;
    padding: 40px 20px;
    font-family: 'Arial', sans-serif;
}

.meme-card {
    margin: 20px auto;
    padding: 20px;
    border: 3px solid #ffb6c1;
    border-radius: 20px;
    max-width: 400px;
    background-color: #fff0f5;
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

.cat-image {
    max-width: 100%;
    border-radius: 10px;
    margin-bottom: 15px;
}

.quote {
    font-size: 1.3rem;
    font-weight: bold;
    color: #333;
    margin-bottom: 5px;
}

.tags {
    color: #888;
    font-size: 0.9rem;
}

.empty-state {
    margin: 40px 0;
    font-size: 1.2rem;
    color: #666;
}

.gacha-btn {
    padding: 15px 30px;
    font-size: 1.2rem;
    font-weight: bold;
    background-color: #ff69b4;
    color: white;
    border: none;
    border-radius: 50px;
    cursor: pointer;
    transition: transform 0.2s, background-color 0.2s;
}

.gacha-btn:hover {
    background-color: #ff1493;
    transform: scale(1.05);
}
</style>