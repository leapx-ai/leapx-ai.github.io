---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
---

<div class="about-hero">
  <div class="hero-content">
    <h1 class="hero-title">咏歌与凯旋</h1>
    <p class="hero-subtitle">编程、AI与系统思考</p>
  </div>
</div>

<div class="about-container">
  <div class="about-section">
    <h2 class="section-title">关于作者</h2>
    <p class="section-text">
      你好，我是 <strong>Liu Zhao</strong>，一名热爱技术与思考的开发者。
    </p>
    <p class="section-text">
      这个博客记录我在编程实践、人工智能探索以及系统思维方面的学习心得与思考沉淀。
      我相信技术的价值不仅在于解决问题，更在于培养一种清晰、理性的思维方式。
    </p>
  </div>

  <div class="about-section">
    <h2 class="section-title">关注公众号</h2>
    <p class="section-text">
      欢迎扫码关注我的微信公众号，第一时间获取最新文章推送，一起交流技术、AI与成长。
    </p>
    <div class="qrcode-wrapper">
      <img
        src="/assets/img/IMG_5269.JPG"
        alt="微信公众号二维码"
        class="qrcode-img"
      />
      <p class="qrcode-caption">扫码关注「咏歌与凯旋」</p>
    </div>
  </div>

  <div class="about-section">
    <h2 class="section-title">联系与互动</h2>
    <p class="section-text">
      如果你有任何想法、建议或合作意向，欢迎通过以下方式与我联系：
    </p>
    <ul class="contact-list">
      <li>
        <i class="fab fa-github"></i>
        <a href="https://github.com/leapx-ai" target="_blank" rel="noopener">GitHub: @leapx-ai</a>
      </li>
      <li>
        <i class="fas fa-envelope"></i>
        <span>Email: 请通过 GitHub 或公众号留言联系</span>
      </li>
    </ul>
  </div>
</div>

<style>
  .about-hero {
    text-align: center;
    padding: 3rem 1rem 2.5rem;
    margin-bottom: 2rem;
    border-bottom: 1px solid var(--main-border-color);
    color: var(--heading-color);
  }

  .hero-title {
    font-family: 'Noto Serif SC', 'Lato', 'Microsoft Yahei', serif;
    font-size: 2.5rem;
    font-weight: 700;
    margin-bottom: 0.5rem;
    letter-spacing: 0.05em;
    color: var(--heading-color);
  }

  .hero-subtitle {
    font-size: 1.15rem;
    color: var(--text-muted-color);
    font-weight: 300;
  }

  .about-container {
    max-width: 720px;
    margin: 0 auto;
  }

  .about-section {
    margin-bottom: 2.5rem;
  }

  .section-title {
    font-family: 'Noto Serif SC', 'Lato', 'Microsoft Yahei', serif;
    font-size: 1.4rem;
    font-weight: 600;
    margin-bottom: 1rem;
    padding-bottom: 0.5rem;
    border-bottom: 1px solid var(--main-border-color);
    color: var(--heading-color);
  }

  .section-text {
    font-size: 1rem;
    line-height: 1.85;
    color: var(--text-color);
    margin-bottom: 1rem;
  }

  .qrcode-wrapper {
    text-align: center;
    margin: 1.5rem 0;
  }

  .qrcode-img {
    width: 220px;
    height: 220px;
    border: 1px solid var(--main-border-color);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }

  .qrcode-img:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
  }

  .qrcode-caption {
    margin-top: 0.75rem;
    font-size: 0.95rem;
    color: var(--text-muted-color);
  }

  .contact-list {
    list-style: none;
    padding: 0;
    margin: 1rem 0 0;
  }

  .contact-list li {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    padding: 0.6rem 0;
    font-size: 1rem;
    color: var(--text-color);
  }

  .contact-list li i {
    width: 1.5rem;
    text-align: center;
    color: var(--text-muted-color);
  }

  .contact-list a {
    color: var(--link-color);
    text-decoration: none;
    transition: color 0.2s;
  }

  .contact-list a:hover {
    color: var(--link-color);
    text-decoration: underline;
  }

  @media (max-width: 576px) {
    .hero-title {
      font-size: 2rem;
    }

    .hero-subtitle {
      font-size: 1rem;
    }

    .qrcode-img {
      width: 180px;
      height: 180px;
    }
  }
</style>
