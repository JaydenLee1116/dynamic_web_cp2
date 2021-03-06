# 개인 포트폴리오 동적 웹 구현 프로젝트

## 배경 및 목적 : HTML, CSS, Javascript에 대한 학습 및 동적으로 만들어진 개인 포트폴리오 웹사이트 구현

## 프로젝트 설명

링크 : [Jayden's Portfolio](https://jaydenlee1116.github.io/dynamic_web_cp2/)

<img width="1423" alt="Screen Shot 2022-06-22 at 10 17 40" src="https://user-images.githubusercontent.com/86241737/174922869-0e500acb-d657-4f1e-9e7b-010acbf83968.png">

## 코드 설명

- ### 페이지 상단 메뉴바 Top에선 투명 -> 스크롤 시 배경색 추가
   - 메뉴바의 높이보다 스크롤 길이가 커지면, .navbar--dark 추가
  
```css
#navbar {
  position: fixed;
  width: 100%;
  display: flex;
  justify-content: space-between;
  background-color: transparent;
  align-items: center;
  color: var(--color-white);
  padding: 4px;
  transition: all var(--animation-duration) ease-in-out;
}

.navbar--dark#navbar {
  background-color: var(--color-skyblue);
  padding: 4px;
  transition: all var(--animation-duration) ease-in-out;
  z-index: 1;
}
```

```javascript
const navbar = document.querySelector('#navbar');
const navbarHeight = navbar.getBoundingClientRect().height;

document.addEventListener('scroll', () => {
  if (window.scrollY > navbarHeight) {
    navbar.classList.add('navbar--dark');
  } else {
    navbar.classList.remove('navbar--dark');
  }
});
```

- ### 메뉴바 메뉴 클릭 시 해당 위치로 스크롤링

```javascript
const navbarMenu = document.querySelector('.navbar__menu');
navbarMenu.addEventListener('click', (event) => {
  const target = event.target;
  const link = target.dataset.link;
  if (link == null) {
    return;
  }
  navbarMenu.classList.remove('open');
  scrollIntoView(link);
});

function scrollIntoView(selector) {
  const scrollTo = document.querySelector(selector);
  scrollTo.scrollIntoView({ behavior: 'smooth' });
  selectNavItem(navItems[sectionIds.indexOf(selector)]);
}

function selectNavItem(selected) {
  selectedNavItem.classList.remove('active');
  selectedNavItem = selected;
  selectedNavItem.classList.add('active');
}
```

- ### 메인 홈의 'CONTACT ME' 버튼 클릭 시 contact me 페이지로 스크롤링

```javascript
const homecontactBtn = document.querySelector('.home__contact');
homecontactBtn.addEventListener('click', () => {
  scrollIntoView('#contact');
});
```

- ### 작은 화면(모바일 화면)에서 오른쪽 위 메뉴 토글 버튼 클릭 시 메뉴들 보이게(토글)

```javascript
const navbarToggleBtn = document.querySelector('.navbar__toggle-btn');
navbarToggleBtn.addEventListener('click', () => {
  navbarMenu.classList.toggle('open');
});
```

- ### 홈에서 스크롤 다운 시, 홈 화면 투명해지게

```javascript
const home = document.querySelector('.home__container');
const homeHeight = home.getBoundingClientRect().height;
document.addEventListener('scroll', () => {
  home.style.opacity = (homeHeight - window.scrollY) / homeHeight;
});
```

- ### 홈 화면 크기의 1/2 스크롤 다운 시 오른쪽 하단에 스크롤업 버튼 추가 

```javascript
const arrowUp = document.querySelector('.arrow-up');
document.addEventListener('scroll', () => {
  if (window.scrollY >= homeHeight / 2) {
    arrowUp.classList.add('visible');
  } else {
    arrowUp.classList.remove('visible');
  }
});

arrowUp.addEventListener('click', () => {
  scrollIntoView('#home');
});
```

- ### 프로젝트 파트, 프로젝트 종류 선택시 해당 프로젝트 목록만 표출

```javascript
const workBtnContainer = document.querySelector('.work__categories');
const projectContainer = document.querySelector('.work__projects');
const projects = document.querySelectorAll('.project');
workBtnContainer.addEventListener('click', (e) => {
  const filter = e.target.dataset.filter || e.target.parentNode.dataset.filter;
  if (filter === null) {
    return;
  }

  const active = document.querySelector('.category__btn.selected');
  active.classList.remove('selected');
  const target =
    e.target.nodeName === 'BUTTON' ? e.target : e.target.parentNode;
  target.classList.add('selected');

  projectContainer.classList.add('anim-out');
  setTimeout(() => {
    projects.forEach((project) => {
      if (filter === '*' || filter === project.dataset.type) {
        project.classList.remove('invisible');
      } else {
        project.classList.add('invisible');
      }
    });
    projectContainer.classList.remove('anim-out');
  }, 300);
});
```

- ### 특정 페이지로 이동 시, 메뉴바에서 해당 페이지의 메뉴를 표시

```javascript
const sectionIds = [
  '#home',
  '#about',
  '#skills',
  '#work',
  '#testinomials',
  '#contact',
];

const sections = sectionIds.map((id) => document.querySelector(id));
const navItems = sectionIds.map((id) =>
  document.querySelector(`[data-link="${id}"]`)
);

let selectedNavIndex = 0;
let selectedNavItem = navItems[0];

const observerOptions = {
  root: null,
  rootMargin: '0px',
  threshold: 0.3,
};

const observerCallback = (entries, observer) => {
  entries.forEach((entry) => {
    if (!entry.isIntersecting && entry.intersectionRatio > 0) {
      const index = sectionIds.indexOf(`#${entry.target.id}`);

      // 스크롤링이 아래로 되어서 페이지가 올라옴
      if (entry.boundingClientRect.y < 0) {
        selectedNavIndex = index + 1;
      } else {
        selectedNavIndex = index - 1;
      }
    }
  });
};

const observer = new IntersectionObserver(observerCallback, observerOptions);
sections.forEach((section) => observer.observe(section));

window.addEventListener('wheel', () => {
  if (window.scrollY === 0) {
    selectedNavIndex = 0;
  } else if (
    Math.round(window.scrollY + window.innerHeight) ===
    document.body.clientHeight
  ) {
    selectedNavIndex = navItems.length - 1;
  }
  selectNavItem(navItems[selectedNavIndex]);
});
```

## 느낀점 및 추후 보완사항

### 느낀점
그동안 html 및 css를 이용해서 정적인 웹을 혼자 구현해보기만 했었다. 또, javascript의 개념과 문법에 대한 학습을 진행하면서 '그래서 js로 어떻게 웹을 동적으로 할 수 있다는거지?'라는 생각을 했다.
이번 프로젝트를 통해 js로 어떻게 html 요소를 가져오고, DOM 조작을 할 수 있는지 등을 학습할 수 있었다.

### 추후 보완사항
웹의 기능적인 면에서는 많은 걸 구현했지만, 실제 포트폴리오 안에 있는 내용에 있어서는 아직 더 채워야할 것들이 많다. 더 많은 프로젝트를 통해 학습하고 꾸준히 포트폴리오를 업데이트하자! :)
