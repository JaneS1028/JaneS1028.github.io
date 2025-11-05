---
layout: archives
title: Life Archives
icon: fas fa-archive
order: 3

filters: ["all", "travel", "daily", "people", "food"]

photos:
  - src: /assets/img/pic.jpg
    title: "Spring 2024 🌸"
    desc: "벚꽃이 만개했던 도쿄 여행의 하루"
    tags: ["travel"]
    date: 2024-04-05

  - src: /assets/img/pic.jpg
    title: "Summer Days ☀️"
    desc: "친구들과 해운대에서"
    tags: ["daily", "people"]
    date: 2024-08-12

  - src: /assets/img/pic.jpg
    title: "Latte Break"
    desc: "비 오는 날의 라떼 한 잔"
    tags: ["daily", "food"]
    date: 2025-02-11
---

<!-- Lightbox CSS/JS -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/lightgallery@2.7.2/css/lightgallery-bundle.min.css">
<script src="https://cdn.jsdelivr.net/npm/lightgallery@2.7.2/lightgallery.min.js" defer></script>
<script src="https://cdn.jsdelivr.net/npm/lightgallery@2.7.2/plugins/zoom/lg-zoom.min.js" defer></script>
<script src="https://cdn.jsdelivr.net/npm/lightgallery@2.7.2/plugins/thumbnail/lg-thumbnail.min.js" defer></script>

<style>
  .life-archive { padding: 0.5rem 0; }
  .filter-bar {
    display: flex; flex-wrap: wrap; gap: .5rem; margin: 0 0 1rem 0;
  }
  .filter-btn {
    border: 1px solid #e5e7eb;
    background: #fff;
    border-radius: 999px;
    padding: .4rem .9rem;
    font-size: .9rem;
    cursor: pointer;
    transition: transform .15s ease, background .15s ease, color .15s ease;
  }
  .filter-btn[aria-pressed="true"] {
    background: #111827;
    color: #fff;
  }
  .filter-btn:focus-visible {
    outline: 2px solid #3b82f6;
    outline-offset: 2px;
  }

  .photo-grid {
    display: grid;
    gap: 1rem;
    grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  }

  .photo-card {
    display: flex;
    flex-direction: column;
    background: #fff;
    border-radius: 14px;
    overflow: hidden;
    box-shadow: 0 6px 18px rgba(0,0,0,.06);
    transition: transform .18s ease, box-shadow .18s ease;
  }

  .photo-card:hover {
    transform: translateY(-2px);
    box-shadow: 0 10px 24px rgba(0,0,0,.10);
  }

  .photo-img-wrap {
    aspect-ratio: 4/3;
    overflow: hidden;
    background: #f3f4f6;
  }

  .photo-img-wrap img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
  }

  .photo-meta {
    padding: .8rem .9rem 1rem;
  }

  .photo-title {
    margin: .2rem 0 .35rem;
    font-weight: 700;
    line-height: 1.25;
  }

  .photo-desc {
    margin: 0;
    color: #6b7280;
    font-size: .92rem;
  }

  .photo-tags {
    margin-top: .6rem;
    display: flex;
    gap: .4rem;
    flex-wrap: wrap;
  }

  .tag-chip {
    background: #f3f4f6;
    color: #374151;
    font-size: .75rem;
    padding: .2rem .5rem;
    border-radius: 999px;
  }

  .photo-date {
    margin-left: auto;
    font-size: .75rem;
    color: #9ca3af;
  }

  [data-hidden="true"] { display: none !important; }
</style>

<div class="life-archive">
  <!-- 필터 바 -->
  <div class="filter-bar" role="toolbar" aria-label="Life gallery filters">
    {% assign _filters = page.filters | default: "all" %}
    {% for f in _filters %}
      <button class="filter-btn" type="button"
              data-filter="{{ f | downcase }}"
              aria-pressed="{{ f == 'all' | ternary: 'true','false' }}">
        {{ f | capitalize }}
      </button>
    {% endfor %}
  </div>

  <!-- 갤러리 -->
  <div id="life-gallery" class="photo-grid" aria-live="polite">
    {% assign photos = page.photos | sort: "date" | reverse %}
    {% for p in photos %}
      {% assign thumb = p.thumb | default: p.src %}
      {% assign taglist = p.tags | join: ' ' | downcase %}
      <article class="photo-card" data-tags="{{ taglist | escape }}">
        <a href="{{ p.src }}" class="photo-img-wrap" data-lg-size="1400-933" data-sub-html="#meta-{{ forloop.index }}">
          <img src="{{ thumb }}" alt="{{ p.title | default: 'photo' }}" loading="lazy" decoding="async">
        </a>
        <div class="photo-meta" id="meta-{{ forloop.index }}">
          <h3 class="photo-title">{{ p.title }}</h3>
          {% if p.desc %}<p class="photo-desc">{{ p.desc }}</p>{% endif %}
          <div class="photo-tags">
            {% if p.tags %}
              {% for t in p.tags %}
                <span class="tag-chip">#{{ t }}</span>
              {% endfor %}
            {% endif %}
            {% if p.date %}<span class="photo-date">{{ p.date | date: "%Y-%m-%d" }}</span>{% endif %}
          </div>
        </div>
      </article>
    {% endfor %}
  </div>

  <noscript>
    <p>자바스크립트가 비활성화되어 있어 필터/확대 기능이 제한됩니다. 이미지는 그대로 보실 수 있어요.</p>
  </noscript>
</div>

<script>
  window.addEventListener('DOMContentLoaded', function () {
    if (window.lightGallery) {
      lightGallery(document.getElementById('life-gallery'), {
        selector: '.photo-img-wrap',
        plugins: [lgZoom, lgThumbnail],
        download: false
      });
    }

    const buttons = document.querySelectorAll('.filter-btn');
    const cards = document.querySelectorAll('.photo-card');

    function setPressed(target) {
      buttons.forEach(b => b.setAttribute('aria-pressed', b === target ? 'true' : 'false'));
    }

    function applyFilter(name) {
      const tag = (name || 'all').toLowerCase();
      cards.forEach(card => {
        const tags = card.getAttribute('data-tags') || '';
        const show = tag === 'all' || tags.split(/\s+/).includes(tag);
        card.toggleAttribute('data-hidden', !show);
      });
    }

    buttons.forEach(btn => {
      btn.addEventListener('click', () => {
        setPressed(btn);
        applyFilter(btn.dataset.filter);
      });
    });
  });
</script>
