// Ultra-compact Fitness Checklist Widget - Fixed Version
(function(){
  // Create and append styles
  const style = document.createElement('style');
  style.textContent = `
    .fl-cw * {margin:0;padding:0;box-sizing:border-box;font-family:system-ui,-apple-system,sans-serif}
    .fl-cw {position:fixed;bottom:20px;right:20px;z-index:99999;font-size:16px;color:#fff}
    .fl-min {background:#000;border:2px solid #D7FB00;border-radius:24px;box-shadow:0 0 10px rgba(215,251,0,.5);padding:10px 16px;cursor:pointer;display:flex;align-items:center;justify-content:space-between;max-width:280px}
    .fl-min:hover {box-shadow:0 0 20px rgba(215,251,0,.7)}
    .fl-title {font-weight:700;color:#D7FB00;margin-right:8px}
    .fl-badge {background:#D7FB00;color:#000;font-weight:700;padding:4px 8px;border-radius:50px;font-size:14px}
    .fl-exp {position:fixed;top:10%;left:50%;transform:translateX(-50%);width:90%;max-width:500px;max-height:80vh;background:#000;border-radius:24px;border:2px solid #D7FB00;box-shadow:0 0 20px rgba(215,251,0,.5);overflow-y:auto;z-index:100000}
    .fl-head {display:flex;justify-content:space-between;align-items:center;padding:16px;border-bottom:1px solid #333}
    .fl-wtitle {font-size:22px;font-weight:900}
    .fl-hilight {color:#D7FB00}
    .fl-circle {background:#D7FB00;color:#000;font-size:18px;font-weight:700;width:48px;height:48px;border-radius:50%;display:flex;align-items:center;justify-content:center;box-shadow:0 0 15px rgba(215,251,0,.5)}
    .fl-box {background:#0E0E0E;border-radius:16px;margin:16px;overflow:hidden}
    .fl-boxhead {display:flex;justify-content:space-between;padding:10px 16px;background:#111;border-bottom:1px solid #222}
    .fl-sectitle {font-size:14px;color:#999}
    .fl-minbtn {cursor:pointer;padding:6px}
    .fl-items {display:flex;flex-direction:column}
    .fl-item {display:flex;align-items:center;padding:12px;border-bottom:1px solid #222}
    .fl-item:hover {background:#1A1A1A}
    .fl-check {cursor:pointer;margin-right:12px;color:#555}
    .fl-check.fl-active {color:#D7FB00}
    .fl-emoji {margin-right:8px}
    .fl-text {flex-grow:1}
    .fl-done {text-decoration:line-through;color:#555}
    .fl-remove {opacity:0;color:#555;cursor:pointer}
    .fl-item:hover .fl-remove {opacity:1}
    .fl-remove:hover {color:#ff4d4d}
    .fl-add {margin:16px}
    .fl-addbtn {width:100%;background:#111;color:#999;border:1px solid #222;border-radius:16px;padding:10px;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:8px}
    .fl-plus {color:#D7FB00}
    .fl-form {background:#111;border-radius:16px;padding:16px;margin:16px}
    .fl-emojis {display:flex;overflow-x:auto;gap:8px;margin-bottom:12px}
    .fl-emoji-opt {width:32px;height:32px;display:flex;align-items:center;justify-content:center;border-radius:50%;cursor:pointer}
    .fl-emoji-sel {background:rgba(215,251,0,.2)}
    .fl-row {display:flex;align-items:center;gap:8px}
    .fl-sel-em {font-size:20px}
    .fl-input {flex-grow:1;background:#222;border:none;border-radius:8px;padding:10px;color:#fff}
    .fl-input:focus {outline:none;box-shadow:0 0 0 2px rgba(215,251,0,.3)}
    .fl-submit {background:#D7FB00;color:#000;font-weight:500;padding:8px 14px;border:none;border-radius:8px;cursor:pointer}
    .fl-submit:disabled {opacity:.5;cursor:not-allowed}
    .fl-footer {text-align:center;padding:12px;color:#555;font-size:14px}
  `;
  document.head.appendChild(style);

  // Default items
  const defaultItems = [
    {id: "1", text: "Хидратация", emoji: "💧", done: false},
    {id: "2", text: "Пълноценна храна богата на нутриенти", emoji: "🥩", done: false},
    {id: "3", text: "Прием на добавки", emoji: "💊", done: false},
    {id: "4", text: "Слънце и чист въздух", emoji: "🌤️", done: false},
    {id: "5", text: "Тренировка за деня", emoji: "🏋️", done: false},
    {id: "6", text: "Проследяване на прогреса", emoji: "📈", done: false},
    {id: "7", text: "Медитация и молитва", emoji: "🧘‍♂️", done: false},
    {id: "8", text: "7-9 часа сън", emoji: "🛌", done: false},
    {id: "9", text: "Споделяне на успехите в Telegram", emoji: "📘", done: false}
  ];

  // Emoji options
  const emojis = "🔥 💪 🏆 🎯 ⚡ 🧠 ❤️ 🥗 🍎 🚶 🧘‍♀️ 🏃 🚴 🏊 🥤 🍚".split(" ");

  // Helper functions for localStorage
  const getItem = (key) => localStorage.getItem(key);
  const setItem = (key, value) => localStorage.setItem(key, value);
  const today = () => new Date().toDateString();

  // Get checklist items from localStorage
  function getStoredItems() {
    const stored = getItem("flItems");
    const storedDate = getItem("flDate");
    const currentDate = today();
    
    // Reset if it's a new day
    if (storedDate !== currentDate) {
      setItem("flDate", currentDate);
      return defaultItems;
    }
    
    return stored ? JSON.parse(stored) : defaultItems;
  }

  // State management
  const state = {
    items: getStoredItems(),
    minimized: true,
    adding: false,
    newText: "",
    newEmoji: "🔥"
  };

  // Save items to localStorage
  function saveItems() {
    setItem("flItems", JSON.stringify(state.items));
    
    // Set current date for daily reset check
    if (getItem("flDate") !== today()) {
      setItem("flDate", today());
    }
  }

  // Calculate completion percentage
  function getPercentage() {
    return Math.round((state.items.filter(item => item.done).length / state.items.length) * 100) || 0;
  }

  // Toggle item completion
  function toggleItem(id) {
    state.items = state.items.map(item => 
      item.id === id ? {...item, done: !item.done} : item
    );
    saveItems();
    renderWidget();
  }

  // Remove item
  function removeItem(id) {
    state.items = state.items.filter(item => item.id !== id);
    saveItems();
    renderWidget();
  }

  // Add new custom item
  function addItem() {
    if (state.newText.trim()) {
      const newItem = {
        id: Date.now().toString(),
        text: state.newText.trim(),
        emoji: state.newEmoji,
        done: false
      };
      state.items = [...state.items, newItem];
      state.newText = "";
      state.adding = false;
      saveItems();
      renderWidget();
    }
  }

  // Create and render the widget
  function renderWidget() {
    // Remove existing widget if any
    const existingWidget = document.querySelector('.fl-cw');
    if (existingWidget) {
      existingWidget.remove();
    }
    
    // Create new container
    const container = document.createElement('div');
    container.className = 'fl-cw';
    
    if (state.minimized) {
      // Minimized view
      const percent = getPercentage();
      container.innerHTML = `
        <div class="fl-min">
          <div class="fl-title">Лист с задачи</div>
          <div class="fl-badge">${percent}%</div>
        </div>
      `;
      
      // Add event listener
      container.querySelector('.fl-min').addEventListener('click', function() {
        state.minimized = false;
        renderWidget();
      });
    } else {
      // Expanded view
      const percent = getPercentage();
      
      // Generate items HTML
      const itemsHTML = state.items.map(item => `
        <div class="fl-item">
          <div class="fl-check ${item.done ? 'fl-active' : ''}" data-id="${item.id}">
            ${item.done ? '✓' : '○'}
          </div>
          <div class="fl-emoji">${item.emoji}</div>
          <div class="fl-text ${item.done ? 'fl-done' : ''}">${item.text}</div>
          <div class="fl-remove" data-rmid="${item.id}">×</div>
        </div>
      `).join('');
      
      // Generate emoji selector HTML if adding
      const emojiHTML = state.adding ? emojis.map(emoji => `
        <div class="fl-emoji-opt ${state.newEmoji === emoji ? 'fl-emoji-sel' : ''}" data-emoji="${emoji}">
          ${emoji}
        </div>
      `).join('') : '';
      
      // Complete expanded view HTML
      container.innerHTML = `
        <div class="fl-exp">
          <div class="fl-head">
            <div class="fl-wtitle"><span class="fl-hilight">FL</span> CHECKLIST</div>
            <div class="fl-circle">${percent}%</div>
          </div>
          
          <div class="fl-box">
            <div class="fl-boxhead">
              <div class="fl-sectitle">ЕЖЕДНЕВНИ ЗАДАЧИ</div>
              <div class="fl-minbtn">▼</div>
            </div>
            
            <div class="fl-items">
              ${itemsHTML}
            </div>
          </div>
          
          ${state.adding ? `
            <div class="fl-form">
              <div class="fl-emojis">
                ${emojiHTML}
              </div>
              <div class="fl-row">
                <div class="fl-sel-em">${state.newEmoji}</div>
                <input 
                  type="text" 
                  class="fl-input" 
                  placeholder="Въведете нова задача..." 
                  value="${state.newText}"
                />
                <button 
                  class="fl-submit" 
                  ${!state.newText.trim() ? 'disabled' : ''}
                >
                  Добави
                </button>
              </div>
            </div>
          ` : `
            <div class="fl-add">
              <button class="fl-addbtn">
                <span class="fl-plus">+</span>
                <span>Добави своя задача</span>
              </button>
            </div>
          `}
          
          <div class="fl-footer">
            <p>Списъкът ще се нулира в полунощ.</p>
          </div>
        </div>
      `;
      
      // Add event listeners
      // Minimize button
      container.querySelector('.fl-minbtn').addEventListener('click', function() {
        state.minimized = true;
        renderWidget();
      });
      
      // Toggle checkboxes
      container.querySelectorAll('.fl-check').forEach(el => {
        el.addEventListener('click', function() {
          toggleItem(this.getAttribute('data-id'));
        });
      });
      
      // Remove buttons
      container.querySelectorAll('.fl-remove').forEach(el => {
        el.addEventListener('click', function() {
          removeItem(this.getAttribute('data-rmid'));
        });
      });
      
      // Add item form or button
      if (state.adding) {
        // Emoji options
        container.querySelectorAll('.fl-emoji-opt').forEach(el => {
          el.addEventListener('click', function() {
            state.newEmoji = this.getAttribute('data-emoji');
            renderWidget();
          });
        });
        
        // Input field
        const input = container.querySelector('.fl-input');
        input.addEventListener('input', function() {
          state.newText = this.value;
        });
        input.addEventListener('keydown', function(e) {
          if (e.key === 'Enter') addItem();
        });
        
        // Focus input after rendering
        setTimeout(() => input.focus(), 0);
        
        // Add button
        container.querySelector('.fl-submit').addEventListener('click', addItem);
      } else {
        // Show add form button
        container.querySelector('.fl-addbtn').addEventListener('click', function() {
          state.adding = true;
          renderWidget();
        });
      }
    }
    
    // Add to page
    document.body.appendChild(container);
  }

  // Initialize
  renderWidget();
})();
