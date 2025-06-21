---
title: 'Winter Break Countdown'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---
<style>
.strawberry-container {
  display: grid;
  gap: 2px;
  justify-content: center;
  margin: 1rem 5rem;
}
</style>

<div id="countdown" style="font-size: 32px;"></div>
<div id="completion" style="font-size: 32px;"></div>
<div id="strawberry-grid" class="strawberry-container"></div>
<figcaption>Each strawberry represents one hour. Fully realized strawberries represent hours that have passed.</figcaption>

<script>
// Set the date we're counting down to
const countDownDate = new Date("Aug 22, 2025 17:00:00 EST").getTime();
const startDate = new Date("May 15, 2025 04:30:00 EST").getTime();
const totalDuration = countDownDate - startDate;

// Function to update percentage
const updatePercentage = () => {
  const now = new Date().getTime();
  const elapsed = now - startDate;
  const percentComplete = Math.min(100, Math.max(0, (elapsed / totalDuration) * 100));
  document.getElementById("completion").innerHTML = "and is " + 
    percentComplete.toFixed(5) + "% over.";
};

// Function to update countdown
const updateCountdown = () => {
  const now = new Date().getTime();
  const distance = countDownDate - now;
  
  // Time calculations
  const days = Math.floor(distance / (1000 * 60 * 60 * 24));
  const hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
  const minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
  const seconds = Math.floor((distance % (1000 * 60)) / 1000);

  if (distance < 0) {
    clearInterval(countdownInterval);
    clearInterval(percentInterval);
    document.getElementById("countdown").innerHTML = "Summer break is over.";
    document.getElementById("completion").innerHTML = "";
  } else {
    document.getElementById("countdown").innerHTML = "Summer break ends in " + days + "d " + hours + "h "
    + minutes + "m " + seconds + "s";
  }
};

const totalHours = Math.floor(totalDuration / (3600000));

function updateGridLayout() {
  const container = document.getElementById('strawberry-grid');
  const strawberrySize = 12; // 10px image + 2px gap
  const maxWidth = Math.min(window.innerWidth - 40, 1200); // 20px padding on each side
  const columns = Math.floor(maxWidth / strawberrySize);
  container.style.gridTemplateColumns = `repeat(${columns}, 10px)`;
}

function updateStrawberryGrid() {
  const now = new Date().getTime();
  const hoursPassed = Math.floor((now - startDate) / 3600000);
  let html = "";
  
  for (let i = 0; i < totalHours; i++) {
    if (i < hoursPassed) {
      html += `<img src="/countdown/strawberry.svg" width="10" style="opacity:1;" />`;
    } else {
      html += `<img src="/countdown/strawberry.svg" width="10" style="opacity:0.3;" />`;
    }
  }
  document.getElementById("strawberry-grid").innerHTML = html;
}

// Add resize listener
window.addEventListener('resize', updateGridLayout);

// Initialize values immediately
updatePercentage();
updateCountdown();
updateGridLayout();
updateStrawberryGrid();

// Set up intervals for updates
const percentInterval = setInterval(updatePercentage, 100);
const countdownInterval = setInterval(updateCountdown, 1000);
const gridInterval = setInterval(updateStrawberryGrid, 60000);
</script>