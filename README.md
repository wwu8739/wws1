# The Foodie Web App – Responsive Menu & Cart Explorer

**Authorship & Attribution**  
Wansheng Wu - For CIS 376 Front-End Web Development project.  
Technologies & resources:  
- Bootstrap 5.3 (via CDN – grid, cards, navbar, responsive components)  
- Vanilla JavaScript (fetch, DOM manipulation, events)  
- Font Awesome icons (free CDN)  
- Internal JSON data (`data/menu.json` – sample food items)  
- Placeholder images from Unsplash (free for commercial use)

**"Browse mouth-watering dishes, search & sort effortlessly, add to your cart in real-time – all front-end, no backend needed!"**

**User Story**  
As a food enthusiast, I want to browse, search, sort, and add items to a dynamic cart so that I can quickly plan and review my potential order.

**Links**  
- Repository: https://github.com/wwu8739/wws1 
- Screenshot Issue: [#1 – Inspiration from Swiggy Mobile Menu

**Design: pic(s) with short description of another site that inspired your design**  
Heavily inspired by Swiggy's mobile menu interface – featuring a top search bar, clean responsive food cards (image + name + price + add button), and a floating cart summary. 
I adopted the card grid layout and search prominence for intuitive browsing, but customized it with warmer colors, added sort options, and integrated a session-based cart.

**Model/Inspiration evidence**  
[Screenshot of Swiggy mobile menu & cart

I fixed potential clutter by using Bootstrap spacing utilities for better readability, and improved it by implementing real-time search/filter, price/name sort, fake front-end login (with console feedback), and a sessionStorage-powered cart that updates dynamically and can be cleared (features not present in the static Swiggy demo pages).

**Code Block + Explanation**  
```javascript
// scripts/main.js – excerpt showing DOM → script → DOM/data flow (\~lines 35–70)

document.addEventListener('DOMContentLoaded', () => {
  const menuContainer = document.getElementById('menu-container');
  const searchInput = document.getElementById('search-input');
  const sortSelect = document.getElementById('sort-select');
  let menuData = [];

  fetch('data/menu.json')
    .then(response => response.json())
    .then(items => {
      menuData = items;
      renderMenu(menuData);
      updateCartDisplay(); // Show any existing session cart
    });

  searchInput.addEventListener('input', filterAndRender);
  sortSelect.addEventListener('change', filterAndRender);

  function filterAndRender() {
    let filtered = menuData;
    const query = searchInput.value.toLowerCase().trim();
    if (query) {
      filtered = filtered.filter(item =>
        item.name.toLowerCase().includes(query) ||
        item.category.toLowerCase().includes(query)
      );
    }
    const sortValue = sortSelect.value;
    if (sortValue) {
      filtered = [...filtered].sort((a, b) => {
        if (sortValue === 'price-asc') return a.price - b.price;
        if (sortValue === 'price-desc') return b.price - a.price;
        if (sortValue === 'name') return a.name.localeCompare(b.name);
        return 0;
      });
    }
    renderMenu(filtered);
  }

  function renderMenu(items) {
    menuContainer.innerHTML = '';  // Clear existing DOM cards
    items.forEach(item => {
      const card = document.createElement('div');
      card.className = 'col-md-4 mb-4';
      card.innerHTML = `
        <div class="card h-100">
          <img src="\( {item.image}" class="card-img-top" alt=" \){item.name}">
          <div class="card-body">
            <h5 class="card-title">${item.name}</h5>
            <p class="card-text">${item.category} • $${item.price.toFixed(2)}</p>
            <button class="btn btn-primary btn-sm add-to-cart" data-id="${item.id}">
              Add to Cart
            </button>
          </div>
        </div>`;
      menuContainer.appendChild(card);
    });
    // Re-attach event listeners to new buttons
    document.querySelectorAll('.add-to-cart').forEach(btn => {
      btn.addEventListener('click', addToCart);
    });
  }

  // ... (addToCart, updateCartDisplay, sessionStorage logic below)
});# The Foodie Web App – Responsive Menu & Cart Explorer

**Authorship & Attribution**  
Created by Rashan L (@l_rashan, Nairobi, Kenya) for CIS 376 Front-End Web Development project.  
Technologies & resources:  
- Bootstrap 5.3 (via CDN – grid, cards, navbar, responsive components)  
- Vanilla JavaScript (fetch, DOM manipulation, events)  
- Font Awesome icons (free CDN)  
- Internal JSON data (`data/menu.json` – sample food items)  
- Placeholder images from Unsplash (free for commercial use)

**"Browse mouth-watering dishes, search & sort effortlessly, add to your cart in real-time – all front-end, no backend needed!"**

**User Story**  
As a food enthusiast or busy Nairobi resident, I want to browse, search, sort, and add items to a dynamic cart so that I can quickly plan and review my potential order.

**Links**  
- Repository: https://github.com/l-rashan/the-foodie-web-app  
- Live Demo (GitHub Pages): https://l-rashan.github.io/the-foodie-web-app  
- Design inspiration evidence: [Wiki – Inspiration & Screenshots](https://github.com/l-rashan/the-foodie-web-app/wiki/Design-Inspiration)  
- Screenshot Issue: [#1 – Inspiration from Swiggy Mobile Menu](https://github.com/l-rashan/the-foodie-web-app/issues/1)

**Design: pic(s) with short description of another site that inspired your design**  
Heavily inspired by Swiggy's mobile menu interface – featuring a top search bar, clean responsive food cards (image + name + price + add button), and a floating cart summary.  
See the screenshot in the Wiki page and Issue #1 linked above.  
I adopted the card grid layout and search prominence for intuitive browsing, but customized it with warmer colors, added sort options, and integrated a session-based cart.

**Model/Inspiration evidence**  
[Screenshot of Swiggy mobile menu & cart (uploaded in Issue #1)](https://github.com/l-rashan/the-foodie-web-app/issues/1)  
I copied the responsive card design (top image, bottom details & action button) and search bar positioning. I fixed potential clutter by using Bootstrap spacing utilities for better readability, and improved it by implementing real-time search/filter, price/name sort, fake front-end login (with console feedback), and a sessionStorage-powered cart that updates dynamically and can be cleared (features not present in the static Swiggy demo pages).

**Code Block + Explanation**  
```javascript
// scripts/main.js – excerpt showing DOM → script → DOM/data flow (\~lines 35–70)

document.addEventListener('DOMContentLoaded', () => {
  const menuContainer = document.getElementById('menu-container');
  const searchInput = document.getElementById('search-input');
  const sortSelect = document.getElementById('sort-select');
  let menuData = [];

  fetch('data/menu.json')
    .then(response => response.json())
    .then(items => {
      menuData = items;
      renderMenu(menuData);
      updateCartDisplay(); // Show any existing session cart
    });

  searchInput.addEventListener('input', filterAndRender);
  sortSelect.addEventListener('change', filterAndRender);

  function filterAndRender() {
    let filtered = menuData;
    const query = searchInput.value.toLowerCase().trim();
    if (query) {
      filtered = filtered.filter(item =>
        item.name.toLowerCase().includes(query) ||
        item.category.toLowerCase().includes(query)
      );
    }
    const sortValue = sortSelect.value;
    if (sortValue) {
      filtered = [...filtered].sort((a, b) => {
        if (sortValue === 'price-asc') return a.price - b.price;
        if (sortValue === 'price-desc') return b.price - a.price;
        if (sortValue === 'name') return a.name.localeCompare(b.name);
        return 0;
      });
    }
    renderMenu(filtered);
  }

  function renderMenu(items) {
    menuContainer.innerHTML = '';  // Clear existing DOM cards
    items.forEach(item => {
      const card = document.createElement('div');
      card.className = 'col-md-4 mb-4';
      card.innerHTML = `
        <div class="card h-100">
          <img src="\( {item.image}" class="card-img-top" alt=" \){item.name}">
          <div class="card-body">
            <h5 class="card-title">${item.name}</h5>
            <p class="card-text">${item.category} • $${item.price.toFixed(2)}</p>
            <button class="btn btn-primary btn-sm add-to-cart" data-id="${item.id}">
              Add to Cart
            </button>
          </div>
        </div>`;
      menuContainer.appendChild(card);
    });
    // Re-attach event listeners to new buttons
    document.querySelectorAll('.add-to-cart').forEach(btn => {
      btn.addEventListener('click', addToCart);
    });
  }

  // ... (addToCart, updateCartDisplay, sessionStorage logic below)
});
