# Front-end-project-// 
index.html

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Advanced Dynamic Image Slider</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="slider-container" id="slider-container">
    <div class="slider" id="slider">
      <!-- Images inserted dynamically -->
    </div>

    <!-- Navigation -->
    <button class="prev" id="prev">&#10094;</button>
    <button class="next" id="next">&#10095;</button>

    <!-- Caption -->
    <div class="caption" id="caption"></div>

    <!-- Dots / Thumbnails -->
    <div class="dots" id="dots"></div>
  </div>

  <script src="script.js"></script>
</body>
</html>


---

style.css

body {
  font-family: Arial, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
  background: #f0f0f0;
}

.slider-container {
  position: relative;
  width: 90%;
  max-width: 900px;
  overflow: hidden;
  border-radius: 12px;
  box-shadow: 0 6px 20px rgba(0,0,0,0.3);
  background: #000;
}

.slider img {
  width: 100%;
  display: none;
  position: absolute;
  top: 0;
  left: 0;
  transition: opacity 1s ease-in-out;
}

.slider img.active {
  display: block;
  opacity: 1;
}

.prev, .next {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background-color: rgba(0,0,0,0.5);
  color: white;
  border: none;
  padding: 12px;
  cursor: pointer;
  border-radius: 50%;
  font-size: 20px;
  z-index: 10;
}

.prev:hover, .next:hover {
  background-color: rgba(0,0,0,0.8);
}

.prev { left: 15px; }
.next { right: 15px; }

.caption {
  position: absolute;
  bottom: 10px;
  width: 100%;
  text-align: center;
  font-size: 18px;
  color: white;
  background: rgba(0,0,0,0.3);
  padding: 10px 0;
  border-radius: 5px;
}

.dots {
  text-align: center;
  padding: 12px;
  background: rgba(0,0,0,0.1);
}

.dot {
  height: 14px;
  width: 14px;
  margin: 0 5px;
  display: inline-block;
  background-color: #bbb;
  border-radius: 50%;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.dot.active {
  background-color: #fff;
}

/* Responsive */
@media (max-width: 600px) {
  .prev, .next {
    font-size: 16px;
    padding: 8px;
  }
  .caption {
    font-size: 14px;
  }
}


---

script.js

// Advanced Dynamic Image Slider
const images = [
  { src: 'images/image1.jpg', caption: 'Beautiful Landscape 1' },
  { src: 'images/image2.jpg', caption: 'Serene Mountain View' },
  { src: 'images/image3.jpg', caption: 'Calm Beach Sunset' },
  { src: 'images/image4.jpg', caption: 'Forest Trail Adventure' }
];

const slider = document.getElementById('slider');
const dotsContainer = document.getElementById('dots');
const caption = document.getElementById('caption');
let currentSlide = 0;
let slideInterval;

// Create images and dots dynamically
images.forEach((imgObj, index) => {
  const img = document.createElement('img');
  img.src = imgObj.src;
  img.alt = imgObj.caption;
  if (index === 0) img.classList.add('active');
  slider.appendChild(img);

  const dot = document.createElement('span');
  dot.classList.add('dot');
  if (index === 0) dot.classList.add('active');
  dot.addEventListener('click', () => goToSlide(index));
  dotsContainer.appendChild(dot);
});

const slides = slider.querySelectorAll('img');
const dots = dotsContainer.querySelectorAll('.dot');
const totalSlides = images.length;

// Display the slide
function showSlide(index) {
  slides.forEach(slide => slide.classList.remove('active'));
  dots.forEach(dot => dot.classList.remove('active'));

  slides[index].classList.add('active');
  dots[index].classList.add('active');
  caption.innerText = images[index].caption;
  currentSlide = index;
}

// Navigation
function nextSlide() { showSlide((currentSlide + 1) % totalSlides); }
function prevSlide() { showSlide((currentSlide - 1 + totalSlides) % totalSlides); }
function goToSlide(index) { showSlide(index); }

// Auto-slide
function startSlide() { slideInterval = setInterval(nextSlide, 4000); }
function stopSlide() { clearInterval(slideInterval); }

// Pause on hover
const sliderContainer = document.getElementById('slider-container');
sliderContainer.addEventListener('mouseover', stopSlide);
sliderContainer.addEventListener('mouseout', startSlide);

// Keyboard navigation
document.addEventListener('keydown', e => {
  if (e.key === 'ArrowLeft') prevSlide();
  if (e.key === 'ArrowRight') nextSlide();
});

// Swipe support for mobile
let touchStartX = 0;
let touchEndX = 0;

sliderContainer.addEventListener('touchstart', e => {
  touchStartX = e.changedTouches[0].screenX;
});

sliderContainer.addEventListener('touchend', e => {
  touchEndX = e.changedTouches[0].screenX;
  handleGesture();
});

function handleGesture() {
  if (touchEndX < touchStartX - 50) nextSlide(); // swipe left
  if (touchEndX > touchStartX + 50) prevSlide(); // swipe right
}

// Initialize slider
showSlide(currentSlide);
startSlide();

// Buttons
document.getElementById('prev').addEventListener('click', prevSlide);
document.getElementById('next').addEventListener('click', nextSlide);


