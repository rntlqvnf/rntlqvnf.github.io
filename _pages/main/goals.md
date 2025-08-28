---
title: "My Goals"
permalink: /goals/
layout: full-width
---

<style>
/* --- goals.md Final Page Styles --- */
.goals-page-wrapper {
  min-height: 100vh;
  width: 100%;
  padding: 2rem 2rem 5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column; /* 자식 요소들을 세로로 쌓도록 변경 */
}
.page-main-title {
  font-size: 4em; /* 제목 폰트 크기 */
  font-weight: 600; /* 폰트 굵기 */
  margin-top: 0;
  margin-bottom: 3rem; /* 카드와의 간격 */
  text-align: center;
}
.goals-grid {
  display: grid;
  grid-template-columns: 1fr 1fr; /* 2-column grid */
  gap: 2rem;
  width: 100%;
  max-width: 1280px;
}
.goal-card {
  border-radius: 20px;
  padding: 3rem;
  min-height: 450px;
}

/* --- Profession Card Style --- */
.profession-card {
  background-color: #f5f5f7;
}
.goal-card .category {
  font-size: 1.1em;
  font-weight: 600;
  color: #888;
  margin-bottom: 2rem;
}
.goal-card h3 {
  font-size: 3em;
  font-weight: 700;
  margin: 0;
  line-height: 1.2;
}
.goal-card .desc {
  font-size: 1.3em;
  color: #555;
  margin-top: 1rem;
  max-width: 450px;
}

/* --- Hobby Card Style --- */
.hobby-goal-card {
  background-color: #fff;
  border: 1px solid #ebebeb;
}
.hobby-list {
  list-style: none;
  padding: 0;
  margin-top: 1.5rem;
  display: flex;
  flex-wrap: wrap;
  gap: 0.8rem;
}
.hobby-item {
  background-color: #f5f5f7;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  font-size: 1.1em;
  font-weight: 600;
}

/* --- Responsive Adjustments --- */
@media (max-width: 900px) {
  .goals-grid {
    grid-template-columns: 1fr;
  }
  .goal-card {
    min-height: auto;
  }
  .page-main-title {
    font-size: 3em;
  }
}
</style>

<div class="goals-page-wrapper">

  <h2 class="page-main-title">Goals</h2>

  <div class="goals-grid">

    <div class="goal-card profession-card">
      <p class="category">In Profession</p>
      <h3>Longevity at the Top Tier</h3>
      <p class="desc">
        To consistently perform at a high level, maintaining relevance for the entire duration of a career.
      </p>
    </div>

    <div class="goal-card hobby-goal-card">
      <p class="category">In Pursuits</p>
      <h3>The Top-Level Amateur</h3>
      <ul class="hobby-list">
        <li class="hobby-item">Gastronomy</li>
        <li class="hobby-item">Culinary</li>
        <li class="hobby-item">Bodybuilding</li>
        <li class="hobby-item">Badminton</li>
        <li class="hobby-item">Novels</li>
        <li class="hobby-item">Investment</li>
      </ul>
    </div>

  </div>
</div>