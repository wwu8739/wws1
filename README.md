# wws1
CIS 376 SPRING 2026
# The Foodie Web App – Responsive Menu & Cart Explorer

**Authorship & Attribution**  
Created by Wansheng Wu  for CIS 376 Front-End Web Development project.  
Technologies & resources:  
- Bootstrap 5.3 (via CDN – grid, cards, navbar, responsive components)  
- Vanilla JavaScript (fetch, DOM manipulation, events)  
- Font Awesome icons (free CDN)  
- Internal JSON data (`data/json` – sample food items)  
- food delivery images from Unsplash (free for commercial use)

**"Browse mouth-watering dishes, search & sort effortlessly, add to your cart in real-time – all front-end, no backend needed!"**

**User Story**  
As a food enthusiast resident, I want to browse, search, sort, and add items to a dynamic cart so that I can quickly plan and review my potential order.

**Links**  
- Repository:https://github.com/wwu8739/wws1
- Live Demo (GitHub Pages):   https://github.com/wwu8739/wws1
- Design inspiration evidence:   Ubere![25576](https://github.com/user-attachments/assets/aec61a48-4d06-4082-a191-4327829c1d82)
ats.com
- Screenshot Issue: [#1 – Inspiration from Swiggy Mobile Menu](https://github.com/wwu8739/wws1)

**Design: pic(s) with short description of another site that inspired your design**  
Heavily inspired by Swiggy's mobile menu interface – featuring a top search bar, clean responsive food cards (image + name + price + add button), and a floating cart summary.  
See the screenshot in the Wiki page and Issue #1 linked above.  
I adopted the card grid layout and search prominence for intuitive browsing, but customized it with warmer colors, added sort options, and integrated a session-based cart.

**Model/Inspiration evidence**  
I was inspired by the responsive card design (top image, bottom details & action button) and search bar positioning. I fixed potential clutter by using Bootstrap spacing utilities for better readability, and improved it by implementing real-time search/filter, price/name sort, fake front-end login (with console feedback), and a sessionStorage-powered cart that updates dynamically and can be cleared (features not present in the static Swiggy demo pages).

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
