---
title: 'Winter Break Countdown'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---
<div id="countdown" style="font-size: 32px;"></div>
<div id="completion" style="font-size: 32px;"></div>
<div id="strawberry-grid" style="display: grid; grid-template-columns: repeat(50, 10px); gap: 2px; justify-content: center;"></div>
<figcaption>Each strawberry represents one hour. Fully realized strawberries represent hours that have passed.</figcaption>

<script>
// Set the date we're counting down to
const countDownDate = new Date("Jan 20, 2025 16:45:00 EST").getTime();
const startDate = new Date("Dec 21, 2024 15:00:00 EST").getTime();
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
    document.getElementById("countdown").innerHTML = "Winter break is over.";
    document.getElementById("completion").innerHTML = "";
  } else {
    document.getElementById("countdown").innerHTML = "Winter break ends in " + days + "d " + hours + "h "
    + minutes + "m " + seconds + "s";
  }
};

const totalHours = Math.floor(totalDuration / (3600000));

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

// Initialize values immediately
updatePercentage();
updateCountdown();
updateStrawberryGrid();

// Set up intervals for updates
const percentInterval = setInterval(updatePercentage, 100);
const countdownInterval = setInterval(updateCountdown, 1000);
const gridInterval = setInterval(updateStrawberryGrid, 60000);
</script>