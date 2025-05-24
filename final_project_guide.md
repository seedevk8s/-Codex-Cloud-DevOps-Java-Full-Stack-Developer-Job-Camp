# Lab 5: 최종 프로젝트 - 반응형 포트폴리오 웹사이트

## 🎯 프로젝트 개요

### 목표
10주차에서 학습한 모든 기술을 종합하여 개인 포트폴리오 웹사이트를 완성합니다.

### 기간
8시간 (1일 집중 개발)

### 핵심 기술 스택
- **HTML5**: 시맨틱 마크업, 접근성
- **CSS3**: Grid, Flexbox, 애니메이션, 반응형
- **JavaScript ES6+**: 모듈, 클래스, async/await
- **TypeScript**: 타입 안정성 (선택사항)

---

## 📋 프로젝트 요구사항

### 필수 기능 (Must Have)
1. **완전 반응형 디자인** - 모바일, 태블릿, 데스크톱 지원
2. **다크/라이트 테마 토글** - 사용자 선호도 저장
3. **스크롤 애니메이션** - Intersection Observer 활용
4. **프로젝트 필터링** - 카테고리별 동적 필터링
5. **연락처 폼** - 유효성 검증 및 실제 이메일 전송
6. **성능 최적화** - 이미지 lazy loading, 코드 압축

### 권장 기능 (Should Have)
1. **PWA 기능** - 서비스 워커, 오프라인 지원
2. **SEO 최적화** - 메타 태그, 구조화된 데이터
3. **다국어 지원** - 한국어/영어 전환
4. **타이핑 애니메이션** - 동적 텍스트 효과
5. **스킬 차트** - 프로그레스 바 또는 차트 시각화

### 추가 기능 (Could Have)
1. **블로그 섹션** - 마크다운 렌더링
2. **방문자 통계** - 간단한 analytics
3. **실시간 채팅** - WebSocket 연동
4. **3D 요소** - CSS 3D transforms

---

## 🏗️ 프로젝트 구조

```
portfolio-website/
├── src/
│   ├── index.html              # 메인 HTML
│   ├── styles/
│   │   ├── main.css           # 메인 스타일
│   │   ├── components.css     # 컴포넌트 스타일
│   │   ├── themes.css         # 테마 시스템
│   │   └── responsive.css     # 반응형 스타일
│   ├── scripts/
│   │   ├── main.js           # 메인 로직
│   │   ├── components/       # 컴포넌트별 스크립트
│   │   │   ├── header.js
│   │   │   ├── portfolio.js
│   │   │   └── contact.js
│   │   ├── utils/            # 유틸리티 함수
│   │   │   ├── animations.js
│   │   │   ├── theme.js
│   │   │   └── storage.js
│   │   └── sw.js            # 서비스 워커
│   ├── assets/
│   │   ├── images/          # 이미지 파일
│   │   ├── icons/           # 아이콘 파일
│   │   └── fonts/           # 웹폰트
│   └── data/
│       ├── projects.json    # 프로젝트 데이터
│       └── skills.json      # 스킬 데이터
├── dist/                    # 빌드 결과물
├── package.json            # 의존성 관리
├── webpack.config.js       # 번들링 설정
├── .gitignore             # Git 제외 파일
└── README.md              # 프로젝트 문서
```

---

## 🎨 디자인 가이드라인

### 색상 팔레트
```css
:root {
  /* 라이트 테마 */
  --primary: #3498db;
  --secondary: #2c3e50;
  --accent: #e74c3c;
  --success: #27ae60;
  --warning: #f39c12;
  --light: #f8f9fa;
  --dark: #343a40;
  
  /* 다크 테마 */
  --dark-primary: #5dade2;
  --dark-secondary: #85929e;
  --dark-background: #1a1a1a;
  --dark-surface: #2d2d2d;
  --dark-text: #ffffff;
}
```

### 타이포그래피
- **헤딩**: Inter, Poppins, 또는 Montserrat
- **본문**: 'Segoe UI', system fonts
- **코드**: 'Fira Code', 'Consolas', monospace

### 간격 시스템
```css
:root {
  --space-xs: 0.5rem;   /* 8px */
  --space-sm: 1rem;     /* 16px */
  --space-md: 1.5rem;   /* 24px */
  --space-lg: 2rem;     /* 32px */
  --space-xl: 3rem;     /* 48px */
  --space-xxl: 4rem;    /* 64px */
}
```

---

## 📱 반응형 브레이크포인트

```css
/* 모바일 우선 접근법 */
:root {
  --mobile: 0px;        /* 0px ~ 767px */
  --tablet: 768px;      /* 768px ~ 1023px */
  --desktop: 1024px;    /* 1024px ~ 1439px */
  --wide: 1440px;       /* 1440px+ */
}

@media (min-width: 768px) {
  /* 태블릿 스타일 */
}

@media (min-width: 1024px) {
  /* 데스크톱 스타일 */
}

@media (min-width: 1440px) {
  /* 와이드 스크린 스타일 */
}
```

---

## 🧩 주요 컴포넌트 구조

### 1. 헤더 & 네비게이션
```javascript
class Navigation {
  constructor() {
    this.header = document.querySelector('header');
    this.nav = document.querySelector('nav');
    this.menuToggle = document.querySelector('.menu-toggle');
    this.navLinks = document.querySelectorAll('.nav-link');
    
    this.init();
  }
  
  init() {
    this.bindEvents();
    this.handleScroll();
  }
  
  bindEvents() {
    // 모바일 메뉴 토글
    this.menuToggle?.addEventListener('click', this.toggleMenu.bind(this));
    
    // 스무스 스크롤
    this.navLinks.forEach(link => {
      link.addEventListener('click', this.smoothScroll.bind(this));
    });
    
    // 스크롤 시 헤더 스타일 변경
    window.addEventListener('scroll', this.handleScroll.bind(this));
  }
  
  toggleMenu() {
    this.nav.classList.toggle('nav-open');
    this.menuToggle.classList.toggle('menu-open');
  }
  
  smoothScroll(e) {
    e.preventDefault();
    const target = document.querySelector(e.target.getAttribute('href'));
    target?.scrollIntoView({ behavior: 'smooth' });
  }
  
  handleScroll() {
    const scrolled = window.scrollY > 100;
    this.header.classList.toggle('header-scrolled', scrolled);
  }
}
```

### 2. 포트폴리오 필터링
```javascript
class PortfolioFilter {
  constructor() {
    this.filterButtons = document.querySelectorAll('.filter-btn');
    this.projectItems = document.querySelectorAll('.project-item');
    this.projectsData = [];
    
    this.init();
  }
  
  async init() {
    await this.loadProjects();
    this.bindEvents();
    this.renderProjects();
  }
  
  async loadProjects() {
    try {
      const response = await fetch('./data/projects.json');
      this.projectsData = await response.json();
    } catch (error) {
      console.error('프로젝트 데이터 로드 실패:', error);
    }
  }
  
  bindEvents() {
    this.filterButtons.forEach(btn => {
      btn.addEventListener('click', this.filterProjects.bind(this));
    });
  }
  
  filterProjects(e) {
    const filter = e.target.dataset.filter;
    
    // 활성 버튼 업데이트
    this.filterButtons.forEach(btn => btn.classList.remove('active'));
    e.target.classList.add('active');
    
    // 프로젝트 필터링 및 애니메이션
    this.projectItems.forEach(item => {
      const shouldShow = filter === 'all' || item.dataset.category === filter;
      
      if (shouldShow) {
        item.style.display = 'block';
        setTimeout(() => item.classList.add('fade-in'), 10);
      } else {
        item.classList.remove('fade-in');
        setTimeout(() => item.style.display = 'none', 300);
      }
    });
  }
  
  renderProjects() {
    const container = document.querySelector('.projects-grid');
    container.innerHTML = this.projectsData.map(project => `
      <div class="project-item" data-category="${project.category}">
        <div class="project-image">
          <img src="${project.image}" alt="${project.title}" loading="lazy">
          <div class="project-overlay">
            <a href="${project.demo}" class="btn btn-primary">데모 보기</a>
            <a href="${project.github}" class="btn btn-secondary">GitHub</a>
          </div>
        </div>
        <div class="project-info">
          <h3>${project.title}</h3>
          <p>${project.description}</p>
          <div class="project-tech">
            ${project.technologies.map(tech => `<span class="tech-tag">${tech}</span>`).join('')}
          </div>
        </div>
      </div>
    `).join('');
  }
}
```

### 3. 테마 시스템
```javascript
class ThemeManager {
  constructor() {
    this.themeToggle = document.querySelector('.theme-toggle');
    this.currentTheme = localStorage.getItem('theme') || 'light';
    
    this.init();
  }
  
  init() {
    this.applyTheme(this.currentTheme);
    this.bindEvents();
  }
  
  bindEvents() {
    this.themeToggle?.addEventListener('click', this.toggleTheme.bind(this));
    
    // 시스템 테마 변경 감지
    window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', (e) => {
      if (!localStorage.getItem('theme')) {
        this.applyTheme(e.matches ? 'dark' : 'light');
      }
    });
  }
  
  toggleTheme() {
    this.currentTheme = this.currentTheme === 'light' ? 'dark' : 'light';
    this.applyTheme(this.currentTheme);
    localStorage.setItem('theme', this.currentTheme);
  }
  
  applyTheme(theme) {
    document.documentElement.setAttribute('data-theme', theme);
    this.updateThemeIcon(theme);
  }
  
  updateThemeIcon(theme) {
    const icon = this.themeToggle?.querySelector('i');
    if (icon) {
      icon.className = theme === 'light' ? 'fas fa-moon' : 'fas fa-sun';
    }
  }
}
```

### 4. 스크롤 애니메이션
```javascript
class ScrollAnimations {
  constructor() {
    this.observerOptions = {
      threshold: 0.1,
      rootMargin: '0px 0px -50px 0px'
    };
    
    this.init();
  }
  
  init() {
    this.createObserver();
    this.observeElements();
    this.initProgressBar();
  }
  
  createObserver() {
    this.observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add('animate-in');
          
          // 스킬 바 애니메이션
          if (entry.target.classList.contains('skill-item')) {
            this.animateSkillBar(entry.target);
          }
          
          // 카운터 애니메이션
          if (entry.target.classList.contains('counter')) {
            this.animateCounter(entry.target);
          }
        }
      });
    }, this.observerOptions);
  }
  
  observeElements() {
    const elements = document.querySelectorAll('.fade-up, .fade-left, .fade-right, .skill-item, .counter');
    elements.forEach(el => this.observer.observe(el));
  }
  
  animateSkillBar(skillItem) {
    const progressBar = skillItem.querySelector('.skill-progress');
    const percentage = progressBar.dataset.percentage;
    
    progressBar.style.width = percentage + '%';
  }
  
  animateCounter(counter) {
    const target = parseInt(counter.dataset.target);
    const duration = 2000;
    const startTime = performance.now();
    
    const updateCounter = (currentTime) => {
      const elapsed = currentTime - startTime;
      const progress = Math.min(elapsed / duration, 1);
      
      const currentValue = Math.floor(target * this.easeOutQuart(progress));
      counter.textContent = currentValue.toLocaleString();
      
      if (progress < 1) {
        requestAnimationFrame(updateCounter);
      }
    };
    
    requestAnimationFrame(updateCounter);
  }
  
  easeOutQuart(t) {
    return 1 - Math.pow(1 - t, 4);
  }
  
  initProgressBar() {
    const progressBar = document.querySelector('.scroll-progress');
    
    window.addEventListener('scroll', () => {
      const windowHeight = document.documentElement.scrollHeight - window.innerHeight;
      const scrolled = (window.scrollY / windowHeight) * 100;
      
      progressBar.style.width = scrolled + '%';
    });
  }
}
```

### 5. 연락처 폼
```javascript
class ContactForm {
  constructor() {
    this.form = document.querySelector('.contact-form');
    this.submitBtn = document.querySelector('.submit-btn');
    this.fields = {
      name: this.form?.querySelector('[name="name"]'),
      email: this.form?.querySelector('[name="email"]'),
      message: this.form?.querySelector('[name="message"]')
    };
    
    this.init();
  }
  
  init() {
    this.bindEvents();
    this.initValidation();
  }
  
  bindEvents() {
    this.form?.addEventListener('submit', this.handleSubmit.bind(this));
    
    // 실시간 유효성 검증
    Object.values(this.fields).forEach(field => {
      field?.addEventListener('input', this.validateField.bind(this));
      field?.addEventListener('blur', this.validateField.bind(this));
    });
  }
  
  validateField(e) {
    const field = e.target;
    const value = field.value.trim();
    let isValid = true;
    let message = '';
    
    switch (field.name) {
      case 'name':
        isValid = value.length >= 2;
        message = isValid ? '' : '이름은 2자 이상 입력해주세요.';
        break;
        
      case 'email':
        const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        isValid = emailRegex.test(value);
        message = isValid ? '' : '유효한 이메일 주소를 입력해주세요.';
        break;
        
      case 'message':
        isValid = value.length >= 10;
        message = isValid ? '' : '메시지는 10자 이상 입력해주세요.';
        break;
    }
    
    this.showFieldValidation(field, isValid, message);
    this.updateSubmitButton();
  }
  
  showFieldValidation(field, isValid, message) {
    const errorElement = field.parentNode.querySelector('.error-message');
    
    field.classList.toggle('invalid', !isValid);
    field.classList.toggle('valid', isValid && field.value.trim());
    
    if (errorElement) {
      errorElement.textContent = message;
      errorElement.style.display = message ? 'block' : 'none';
    }
  }
  
  updateSubmitButton() {
    const allValid = Object.values(this.fields).every(field => {
      return field?.classList.contains('valid');
    });
    
    this.submitBtn.disabled = !allValid;
  }
  
  async handleSubmit(e) {
    e.preventDefault();
    
    if (!this.validateForm()) return;
    
    this.setLoading(true);
    
    try {
      const formData = new FormData(this.form);
      const response = await this.submitForm(formData);
      
      if (response.success) {
        this.showSuccess('메시지가 성공적으로 전송되었습니다!');
        this.form.reset();
        this.updateSubmitButton();
      } else {
        throw new Error(response.message || '전송에 실패했습니다.');
      }
    } catch (error) {
      this.showError(error.message);
    } finally {
      this.setLoading(false);
    }
  }
  
  async submitForm(formData) {
    // 실제 환경에서는 백엔드 API 호출
    return new Promise((resolve) => {
      setTimeout(() => {
        resolve({ success: true });
      }, 1000);
    });
  }
  
  setLoading(loading) {
    this.submitBtn.disabled = loading;
    this.submitBtn.innerHTML = loading ? 
      '<i class="fas fa-spinner fa-spin"></i> 전송 중...' : 
      '<i class="fas fa-paper-plane"></i> 메시지 보내기';
  }
  
  showSuccess(message) {
    this.showNotification(message, 'success');
  }
  
  showError(message) {
    this.showNotification(message, 'error');
  }
  
  showNotification(message, type) {
    const notification = document.createElement('div');
    notification.className = `notification notification-${type}`;
    notification.innerHTML = `
      <i class="fas fa-${type === 'success' ? 'check-circle' : 'exclamation-circle'}"></i>
      <span>${message}</span>
      <button class="notification-close">&times;</button>
    `;
    
    document.body.appendChild(notification);
    
    // 자동 제거
    setTimeout(() => notification.remove(), 5000);
    
    // 수동 제거
    notification.querySelector('.notification-close').addEventListener('click', () => {
      notification.remove();
    });
  }
}
```

---

## 🚀 Codex 활용 전략

### 1단계: 프로젝트 구조 생성
```
프롬프트:
"개발자 포트폴리오 웹사이트의 완전한 HTML 구조를 만들어줘:
- 시맨틱 HTML5 마크업
- 접근성을 고려한 ARIA 속성
- SEO 최적화된 메타 태그
- 섹션: Hero, About, Skills, Portfolio, Contact
- 다국어 지원 준비 (data-i18n 속성)"
```

### 2단계: CSS 시스템 구축
```
프롬프트:
"모던한 CSS 시스템을 구축해줘:
- CSS 커스텀 속성 기반 디자인 시스템
- 다크/라이트 테마 지원
- CSS Grid와 Flexbox를 활용한 반응형 레이아웃
- 애니메이션과 트랜지션
- 컴포넌트 기반 스타일 구조"
```

### 3단계: JavaScript 기능 구현
```
프롬프트:
"포트폴리오 웹사이트의 고급 JavaScript 기능을 구현해줘:
- ES6+ 클래스 기반 모듈 구조
- Intersection Observer 스크롤 애니메이션
- 동적 프로젝트 필터링
- 테마 시스템 (localStorage 연동)
- PWA 서비스 워커
- 성능 최적화 (이미지 lazy loading, 디바운싱)"
```

### 4단계: 데이터 구조 설계
```
프롬프트:
"포트폴리오 데이터 구조를 JSON으로 설계해줘:
- 프로젝트 정보 (제목, 설명, 기술스택, 링크, 이미지)
- 스킬 정보 (카테고리별 기술, 숙련도)
- 경력 정보 (회사, 역할, 기간, 주요 성과)
- 다국어 텍스트 리소스
- 타입스크립트 인터페이스 정의"
```

### 5단계: 성능 최적화
```
프롬프트:
"웹사이트 성능 최적화를 위한 코드를 작성해줘:
- 이미지 최적화 (WebP, lazy loading, responsive images)
- CSS/JS 코드 스플리팅
- Critical CSS 인라인화
- 서비스 워커 캐싱 전략
- Lighthouse 점수 개선 방안"
```

---

## 📊 평가 기준

### 기술 구현 (40점)
- **HTML5 시맨틱 마크업** (10점)
  - 적절한 시맨틱 태그 사용
  - 접근성 준수 (ARIA, 키보드 네비게이션)
  - SEO 최적화

- **CSS3 고급 기법** (15점)
  - Grid와 Flexbox 활용
  - 반응형 디자인 완성도
  - 애니메이션 품질
  - 코드 구조와 재사용성

- **JavaScript ES6+** (15점)
  - 모듈화된 코드 구조
  - 비동기 처리
  - 이벤트 처리 및 DOM 조작
  - 에러 처리 및 예외 상황 대응

### 디자인 및 UX (25점)
- **시각적 디자인** (10점)
  - 일관된 디자인 시스템
  - 색상 및 타이포그래피 조화
  - 레이아웃 균형감

- **사용자 경험** (15점)
  - 직관적인 네비게이션
  - 로딩 시간 및 성능
  - 모바일 사용성
  - 접근성 고려

### 기능 완성도 (20점)
- **핵심 기능 구현** (15점)
  - 필수 기능 모두 동작
  - 에러 없는 안정적 동작
  - 브라우저 호환성

- **추가 기능** (5점)
  - 창의적인 기능 추가
  - PWA, 다국어 등 고급 기능

### Codex 활용도 (15점)
- **효과적인 프롬프트 작성** (7점)
  - 명확하고 구체적인 요구사항
  - 단계적 접근법
  - 컨텍스트 제공

- **결과물 개선 및 최적화** (8점)
  - Codex 생성 코드의 이해와 개선
  - 버그 수정 및 기능 확장
  - 코드 품질 향상

---

## 🛠️ 개발 도구 및 리소스

### 필수 도구
- **코드 에디터**: VS Code + 확장 프로그램
- **브라우저**: Chrome DevTools
- **디자인**: Figma (와이어프레임)
- **이미지**: Unsplash, Pexels
- **아이콘**: Font Awesome, Heroicons

### 추천 라이브러리
```json
{
  "devDependencies": {
    "webpack": "^5.0.0",
    "webpack-cli": "^4.0.0",
    "webpack-dev-server": "^4.0.0",
    "html-webpack-plugin": "^5.0.0",
    "mini-css-extract-plugin": "^2.0.0",
    "css-loader": "^6.0.0",
    "sass-loader": "^12.0.0",
    "postcss-loader": "^6.0.0",
    "autoprefixer": "^10.0.0",
    "babel-loader": "^8.0.0",
    "@babel/core": "^7.0.0",
    "@babel/preset-env": "^7.0.0"
  }
}
```

### 유용한 온라인 도구
- **CSS Grid Generator**: https://grid.layoutit.com/
- **Flexbox Guide**: https://css-tricks.com/snippets/css/a-guide-to-flexbox/
- **Color Palette**: https://coolors.co/
- **Font Pairing**: https://fontpair.co/
- **Image Optimization**: https://tinypng.com/
- **PWA Manifest**: https://www.pwabuilder.com/

---

## 📅 일정 가이드

### 시간별 계획 (8시간)

**09:00 - 10:00 (1시간): 프로젝트 기획**
- 와이어프레임 스케치
- 컬러 팔레트 및 폰트 선정
- 콘텐츠 준비 (텍스트, 이미지)

**10:00 - 12:00 (2시간): HTML 구조 및 기본 CSS**
- Codex로 HTML 구조 생성
- 기본 CSS 프레임워크 구축
- 반응형 그리드 시스템 설정

**13:00 - 15:00 (2시간): 스타일링 및 애니메이션**
- 컴포넌트 스타일링
- CSS 애니메이션 구현
- 다크/라이트 테마 시스템

**15:00 - 17:00 (2시간): JavaScript 기능 구현**
- 네비게이션 및 스크롤 효과
- 포트폴리오 필터링
- 연락처 폼 및 유효성 검증

**17:00 - 18:00 (1시간): 최적화 및 배포**
- 성능 최적화
- 브라우저 테스트
- GitHub Pages 배포

---

## 🎉 제출 가이드

### 제출물
1. **GitHub 저장소 링크**
   - 완성된 소스 코드
   - README.md 문서
   - GitHub Pages 배포 링크

2. **프로젝트 보고서**
   - 사용된 기술 스택 설명
   - 주요 기능 구현 과정
   - Codex 활용 사례
   - 어려웠던 점과 해결 방법

3. **발표 자료 (선택사항)**
   - 5분 내외 프레젠테이션
   - 주요 기능 데모
   - 기술적 도전과 성과

### 평가 포인트
- **완성도**: 모든 필수 기능 구현
- **코드 품질**: 깔끔하고 유지보수 가능한 코드
- **창의성**: 독창적인 아이디어와 구현
- **사용자 경험**: 직관적이고 편리한 인터페이스
- **기술 활용**: 학습한 기술의 효과적 활용

---

## 💡 추가 학습 리소스

### 고급 주제
- **웹 성능 최적화**: Core Web Vitals, Lighthouse
- **접근성**: WCAG 가이드라인, 스크린 리더 테스트
- **SEO**: 구조화된 데이터, 메타 태그 최적화
- **보안**: CSP, HTTPS, XSS 방지

### 참고 사이트
- **포트폴리오 영감**: https://awwwards.com/
- **CSS 기법**: https://css-tricks.com/
- **JavaScript 패턴**: https://javascript.info/
- **성능 최적화**: https://web.dev/

이 가이드를 따라 진행하면 전문적이고 완성도 높은 포트폴리오 웹사이트를 완성할 수 있습니다. Codex를 적극 활용하되, 생성된 코드를 이해하고 개선하는 것이 중요합니다!