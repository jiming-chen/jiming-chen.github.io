---
title: 'Winter Break Countdown'
cover:
    image: ""
    alt: ""
    relative: false # when usng page bundles set this to true
---
<div id="countdown" style="font-size: 40px;"></div>
<div id="completion" style="font-size: 32px;"></div>

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
  document.getElementById("completion").innerHTML = "Winter break is " + 
    percentComplete.toFixed(5) + "% over.";
};

// Update percentage every 100ms
const percentInterval = setInterval(updatePercentage, 100);

// Update the countdown every 1 second
const x = setInterval(function() {
  // Get today's date and time
  const now = new Date().getTime();

  // Find the distance between now and the count down date
  const distance = countDownDate - now;
  
  // Time calculations
  const days = Math.floor(distance / (1000 * 60 * 60 * 24));
  const hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
  const minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
  const seconds = Math.floor((distance % (1000 * 60)) / 1000);

  // Display the result
  document.getElementById("countdown").innerHTML = "Winter break ends in " + days + "d " + hours + "h "
  + minutes + "m " + seconds + "s.";

  // If the countdown is finished, display expired
  if (distance < 0) {
    clearInterval(x);
    clearInterval(percentInterval);
    document.getElementById("countdown").innerHTML = "Winter break is over.";
    document.getElementById("completion").innerHTML = "Winter break is 100% over.";
  }
}, 1000);
</script>