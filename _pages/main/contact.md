---
title: "Contact"
permalink: /contact/
layout: full-width
---
<style>
/* --- contact.md Page Specific Styles --- */
.contact-section {
  max-width: 900px;
  margin: 0 auto;
  padding: 3rem 2rem 5rem; /* 상단 패딩을 3rem으로 줄였습니다. */
  text-align: center;
}
.contact-section h2 {
  font-size: 3em;
  font-weight: 700;
  margin-top: 0; /* h2의 기본 margin-top 제거 */
  margin-bottom: 2.5rem; /* 제목 아래 간격 조정 */
}
.contact-section > p {
  font-size: 1.2em;
  line-height: 1.6;
  color: #555;
  max-width: 650px;
  margin: 0 auto 3.5rem;
}

.contact-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
}

.contact-card {
  background-color: #f8f8f8;
  border-radius: 20px;
  padding: 3rem 2rem;
  text-decoration: none;
  color: #1d1d1f;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  display: flex; /* 내부 요소 정렬을 위해 flexbox 사용 */
  flex-direction: column; /* 세로 방향 정렬 */
  align-items: center; /* 수평 중앙 정렬 */
  text-align: center; /* 텍스트 중앙 정렬 */
}
.contact-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 8px 25px rgba(0,0,0,0.1);
}

.contact-card .icon {
  font-size: 3em;
  color: #333;
  margin-bottom: 1rem; /* 아이콘과 제목 사이 여백 줄임 */
}
.contact-card h3 {
  font-size: 1.8em;
  margin-bottom: 0.5rem; /* 제목과 설명 사이 여백 줄임 */
  margin-top: 0; /* h3의 기본 margin-top 제거 */
}
.contact-card .description {
  font-size: 1.1em;
  color: #666;
  margin-bottom: 1.5rem; /* 설명과 주소 사이 여백 조정 */
  flex-grow: 1; /* 내용을 위로 밀고, 주소가 아래에 붙도록 */
}
.contact-card .address {
  font-size: 1.2em;
  font-weight: 600;
  color: #111;
  word-break: break-all;
  margin-top: auto; /* 주소를 카드 하단에 붙이도록 */
}

/* --- Responsive Adjustments --- */
@media (max-width: 768px) {
  .contact-grid {
    grid-template-columns: 1fr;
  }
}
</style>

<div class="contact-section">
  <h2>Get in Touch</h2>
  <p>
    I welcome any questions or opportunities for collaboration. Please choose the channel that best fits the nature of your inquiry.
  </p>
  
  <div class="contact-grid">

    <a href="mailto:yshajae@postech.ac.kr" class="contact-card">
      <div class="icon"><i class="fas fa-envelope"></i></div>
      <h3>Email</h3>
      <p class="description">
        For professional inquiries, research collaborations, or academic questions.
      </p>
      <div class="address">yshajae@postech.ac.kr</div>
    </a>

    <a href="https://www.instagram.com/daily_hajae/" target="_blank" rel="noopener noreferrer" class="contact-card">
      <div class="icon"><i class="fab fa-instagram"></i></div>
      <h3>Instagram DM</h3>
      <p class="description">
        For casual conversations, or questions about my personal posts and pursuits.
      </p>
      <div class="address">@daily_hajae</div>
    </a>

  </div>
</div>