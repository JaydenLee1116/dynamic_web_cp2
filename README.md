# dynamic_web_cp2

## 배경 및 목적 : HTML, CSS, Javascript에 대한 학습 및 동적으로 만들어진 개인 포트폴리오 웹사이트 구현

## 프로젝트 설명(Only 동적 구현 파트)

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

