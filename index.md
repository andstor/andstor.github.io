---
layout: home
---

I am a PhD candidate at the Norwegian University of Science and Technology (NTNU) with a large passion for new technology.
Under the supervision of Professor Jingyue Li, my research focuses on code generation using large language models.

In my spare time, I love to contribute to open-source projects.

<script type="module">
import { Client } from "https://cdn.jsdelivr.net/npm/@gradio/client/dist/index.min.js";

async function initFlorence() {
  console.log("Florence result22");
  //const preloader = document.getElementById("preloader");

  // Show preloader
  //preloader.style.display = "flex";

  const img = document.getElementById("profile-pic");
  const cacheKey = "florence_results";

  // Check if cached
  let data;
  const cached = sessionStorage.getItem(cacheKey);
  if (cached) {
    console.log("Using cached detection results");
    data = JSON.parse(cached);
  } else {
    const app = await Client.connect("andstor/Florence-2");

    // Load image
    const response_0 = await fetch("/assets/images/profile_pic_gemini.jpeg");
    const exampleImage = await response_0.blob();

    const task = "Caption to Phrase Grounding";
    //const task = "Object Detection";
    // Get Florence detection
    const result = await app.predict("/process_image", { 
      image: exampleImage,
      task_prompt: task,
      text_input: "human face",
      model_id: "microsoft/Florence-2-base",
    });

    console.log(result.data);
    data = JSON.parse(result.data[0])["<CAPTION_TO_PHRASE_GROUNDING>"];
    //data = JSON.parse(result.data[0])["<OD>"];

    // Save to sessionStorage
    sessionStorage.setItem(cacheKey, JSON.stringify(data));
  }

  //preloader.style.display = "none";


  function drawOverlayDivs() {
    const container = document.getElementById("overlay-container");

    // Remove old boxes
    container.querySelectorAll(".bbox").forEach(el => el.remove());

    const scaleX = img.clientWidth / img.naturalWidth;
    const scaleY = img.clientHeight / img.naturalHeight;

    data.bboxes.forEach((box, i) => {
      const [x1, y1, x2, y2] = box;
      const left = x1 * scaleX;
      const top = y1 * scaleY;
      const width = (x2 - x1) * scaleX;
      const height = (y2 - y1) * scaleY;

      // Box
      const boxDiv = document.createElement("div");
      boxDiv.classList.add("bbox");
      boxDiv.style.left = `${left}px`;
      boxDiv.style.top = `${top}px`;
      boxDiv.style.width = `${width}px`;
      boxDiv.style.height = `${height}px`;

      // Label
      const labelDiv = document.createElement("div");
      labelDiv.classList.add("bbox-label");
      labelDiv.textContent = data.labels[i];

      boxDiv.appendChild(labelDiv);
      container.appendChild(boxDiv);
    });
  }

  // Draw initially once image is loaded
  if (img.complete) {
    drawOverlayDivs();
  } else {
    img.addEventListener("load", drawOverlayDivs);
  }

  // Watch for image resizing
  const resizeObserver = new ResizeObserver(drawOverlayDivs);
  resizeObserver.observe(img);
}

initFlorence();
</script>


<style>

#overlay-container {
  position: relative;
  display: inline-block;
}

#profile-pic {
  display: block;
  max-width: 100%;
  height: auto;
}

.bbox {
  position: absolute;
  border: 2px solid #8B8DB5;
  box-sizing: border-box;
  pointer-events: none;
}

.bbox-label {
  position: absolute;
  top: -18px;
  left: 0;
  background: #8B8DB5;
  color: white;
  font-size: 12px;
  font-family: sans-serif;
  padding: 1px 4px;
  border-radius: 3px;
  white-space: nowrap;
}
/* Wave dots preloader */
.preloader {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 99%;
  left: 50%;
  transform: translate(-50%, -50%);
  height: 40px;
  z-index: 20;
}

.preloader span {
  width: 8px;
  height: 8px;
  margin: 0 3px;
  background: lightslategray;
  border-radius: 50%;
  display: inline-block;
  animation: wave 1.2s infinite ease-in-out;
}

.preloader span:nth-child(1) { animation-delay: 0s; }
.preloader span:nth-child(2) { animation-delay: 0.2s; }
.preloader span:nth-child(3) { animation-delay: 0.4s; }

@keyframes wave {
  0%, 60%, 100% { transform: translateY(0); }
  30% { transform: translateY(-8px); }
}
</style>



