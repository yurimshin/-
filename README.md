<div class="container">
    <h1>관람자 입력 화면</h1>
    <div id="output" class="output"></div>
</div>

<style>
body {
    background-color: #8B4513; /* 갈색 배경 */
    color: white; /* 텍스트 색상 */
    font-family: Arial, sans-serif; /* 글꼴 설정 */
}

.container {
    text-align: center; /* 중앙 정렬 */
    margin-top: 50px; /* 상단 여백 */
}

.output {
    position: relative; /* 절대 위치 지정 자식 요소를 위한 기준 */
    height: 500px; /* 출력 영역의 높이 */
    width: 400px; /* 출력 영역의 너비 */
    overflow: hidden; /* 넘치는 내용 숨기기 */
    border: 2px solid white; /* 경계선 */
    background-color: rgba(255, 255, 255, 0.2); /* 네모 칸 배경 (반투명) */
    margin: 0 auto; /* 중앙 정렬 */
}

.word {
    position: absolute; /* 절대 위치 */
    width: 120px; /* 단어의 너비 */
    height: 40px; /* 이름표의 높이 */
    display: flex; /* Flexbox 사용 */
    justify-content: center; /* 가운데 정렬 */
    align-items: center; /* 수직 가운데 정렬 */
    font-size: 24px; /* 글자 크기 */
    border: 2px solid white; /* 이름표 경계선 */
    background-color: rgba(255, 255, 255, 0.8); /* 반투명 배경 */
    opacity: 0; /* 초기에는 보이지 않음 */
}
</style>

<script>
document.addEventListener('keydown', function(event) {
    // Enter 키가 눌렸을 때만 단어를 추가
    if (event.key === 'Enter') {
        // 기본 동작 방지
        event.preventDefault();

        const input = prompt("단어를 입력하세요:");
        const outputDiv = document.getElementById('output');

        if (input && input.trim() !== "") {
            const wordDiv = document.createElement('div');
            wordDiv.className = 'word';
            wordDiv.textContent = input;

            outputDiv.appendChild(wordDiv);

            // 애니메이션 설정
            const randomX = Math.random() * (outputDiv.clientWidth - 120); // 랜덤 X 위치
            const randomY = Math.random() * (outputDiv.clientHeight - 40); // 랜덤 Y 위치
            wordDiv.style.left = `${randomX}px`; // 랜덤 위치
            wordDiv.style.top = `${randomY}px`; // 랜덤 위치
            wordDiv.style.opacity = '1'; // 보이게 설정

            // 자유롭게 움직이도록 설정
            let dx = (Math.random() * 2 - 1) * 2; // X축 이동 속도
            let dy = (Math.random() * 2 - 1) * 2; // Y축 이동 속도

            const moveInterval = setInterval(() => {
                let left = parseFloat(wordDiv.style.left);
                let top = parseFloat(wordDiv.style.top);

                // 이동
                wordDiv.style.left = (left + dx) + 'px';
                wordDiv.style.top = (top + dy) + 'px';

                // 경계 체크
                if (parseFloat(wordDiv.style.left) <= 0 || parseFloat(wordDiv.style.left) >= outputDiv.clientWidth - 120) {
                    dx *= -1; // 방향 반전
                }
                if (parseFloat(wordDiv.style.top) <= 0 || parseFloat(wordDiv.style.top) >= outputDiv.clientHeight - 40) {
                    dy *= -1; // 방향 반전
                }

                // 단어 간 부딪힘 감지
                const words = document.querySelectorAll('.word');
                words.forEach(otherWord => {
                    if (otherWord !== wordDiv) {
                        const otherLeft = parseFloat(otherWord.style.left);
                        const otherTop = parseFloat(otherWord.style.top);

                        if (Math.abs(left - otherLeft) < 120 && Math.abs(top - otherTop) < 40) {
                            // 부딪힐 때 위치 반전
                            dx *= -1;
                            dy *= -1;
                        }
                    }
                });

                // 출력 영역의 50%가 차면 이전 단어 삭제
                const totalWords = words.length;
                const occupiedSpace = Array.from(words).reduce((acc, word) => {
                    const wordTop = parseFloat(word.style.top);
                    if (wordTop < outputDiv.clientHeight) {
                        return acc + 1;
                    }
                    return acc;
                }, 0);
                const occupiedPercentage = (occupiedSpace / totalWords) * 100;

                if (occupiedPercentage > 50) {
                    words.forEach(w => {
                        if (w !== wordDiv) {
                            w.remove(); // 이전 단어 삭제
                        }
                    });
                }
            }, 100); // 이동 속도
        }
    }
});
</script>
