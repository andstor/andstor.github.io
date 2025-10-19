---
layout: home
---

I am a PhD candidate at the Norwegian University of Science and Technology (NTNU) with a large passion for new technology.
Under the supervision of Professor Jingyue Li, my research focuses on code generation using large language models.

In my spare time, I love to contribute to open-source projects.

<script type="module">
import { Client } from "https://cdn.jsdelivr.net/npm/@gradio/client/dist/index.min.js";

async function runFlorence(taskText = "human face") {
  console.log("Running Florence with text_input:", taskText);
  const preloader = document.getElementById("preloader");
  const img = document.getElementById("profile-pic");
  const detectButton = document.getElementById("submit-prompt");
  const cacheKey = "florence_results_" + taskText;

  // Show preloader and disable button
  preloader.style.display = "flex";
  detectButton.disabled = true;
  detectButton.classList.add("disabled");
  const originalText = detectButton.textContent;
  detectButton.textContent = "Detecting…";

  // Remember last prompt
  sessionStorage.setItem("florence_last_prompt", taskText);

  // Check cache
  let data;
  const cached = sessionStorage.getItem(cacheKey);
  if (cached) {
    console.log("Using cached detection results for:", taskText);
    data = JSON.parse(cached);
  } else {
    const app = await Client.connect("andstor/Florence-2");

    // Load image
    const response_0 = await fetch("/assets/images/profile_pic_gemini.jpeg");
    const exampleImage = await response_0.blob();

    const task = "Caption to Phrase Grounding";
    // Get Florence detection
    const result = await app.predict("/process_image", {
      image: exampleImage,
      task_prompt: task,
      text_input: taskText,
      model_id: "microsoft/Florence-2-base-ft",
    });

    console.log(result.data);
    data = JSON.parse(result.data[0])["<CAPTION_TO_PHRASE_GROUNDING>"];
    sessionStorage.setItem(cacheKey, JSON.stringify(data));
  }

  // Hide preloader and re-enable button
  preloader.style.display = "none";
  detectButton.disabled = false;
  detectButton.classList.remove("disabled");
  detectButton.textContent = originalText;

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

      const boxDiv = document.createElement("div");
      boxDiv.classList.add("bbox");
      boxDiv.style.left = `${left}px`;
      boxDiv.style.top = `${top}px`;
      boxDiv.style.width = `${width}px`;
      boxDiv.style.height = `${height}px`;

      const labelDiv = document.createElement("div");
      labelDiv.classList.add("bbox-label");
      labelDiv.textContent = data.labels[i];
      boxDiv.appendChild(labelDiv);
      container.appendChild(boxDiv);
    });
  }

  // Draw once image is loaded
  if (img.complete) drawOverlayDivs();
  else img.addEventListener("load", drawOverlayDivs);

  // Watch for resizing
  const resizeObserver = new ResizeObserver(drawOverlayDivs);
  resizeObserver.observe(img);
}

// 🔹 Restore last cached value or default
const lastPrompt = sessionStorage.getItem("florence_last_prompt") || "human face";
document.getElementById("custom-text").value = lastPrompt;
runFlorence(lastPrompt);

// 🔹 Submit button handler
document.getElementById("submit-prompt").addEventListener("click", () => {
  const textInput = document.getElementById("custom-text").value.trim();
  if (textInput.length === 0) return;
  runFlorence(textInput);
});

// 🔹 Submit on Enter key
document.getElementById("custom-text").addEventListener("keypress", (e) => {
  if (e.key === "Enter") {
    e.preventDefault();
    document.getElementById("submit-prompt").click();
  }
});
</script>

<style>
/* 🔸 Disabled detect button styling */
#submit-prompt.disabled {
  background-color: #9ca3af; /* Gray out */
  cursor: not-allowed;
  opacity: 0.7;
  transition: background-color 0.2s ease, opacity 0.2s ease;
}

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
  /* display: flex; */
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: calc(100% - 37px);
  right: 5%;
  z-index: 20;
}


</style>



<style>
.loader svg {
  width: 80px;
  height: 80px;
}

.loader svg path{
  fill: var(--loader-color, #FF7C00);
}

.loader.margin {
  margin: 16px;
}
.loader.margin p {
  margin-top: 10px;
}
.top, .bottom {
  animation-duration: 4s; /* 4 hops × 1s each */
  animation-iteration-count: infinite;
  animation-timing-function: cubic-bezier(0.9, 0, 0.1, 1); /* fast move, slow pause */
}

/* top group */
.top {
  animation-name: top-step;
}

/* bottom group */
.bottom {
  animation-name: bottom-step;
}

@keyframes top-step {
  0%   { transform: translate(125px, 140px); }
  25%  { transform: translate(-125px, 140px); }
  50%  { transform: translate(-125px, 0); }
  75%  { transform: translate(125px, 0); }
  100% { transform: translate(125px, 140px); } /* loop back */
}

@keyframes bottom-step {
  0%   { transform: translate(-125px, -140px); }
  25%  { transform: translate(125px, -140px); }
  50%  { transform: translate(125px, 0); }
  75%  { transform: translate(-125px, 0); }
  100% { transform: translate(-125px, -140px); } /* loop back */
}




  
  .detect-input {
    padding: 8px;
    width: 240px;
    border: 1px solid #ccc;
    border-radius: 6px;
    font-size: 14px;
  }
  .detect-button {
    padding: 8px 14px;
    margin-left: 8px;
    background-color: #FF7C00;
    color: white;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    transition: background-color 0.2s, opacity 0.2s;
    font-size: 14px;
  }

  .detect-button:hover:not(:disabled) {
    background-color: #e56f00;
  }

  .detect-button:disabled {
    background-color: #ccc;
    color: #777;
    cursor: not-allowed;
    opacity: 0.7;
  }

  .detect-input-container {
    padding-top: 40px;
  }

</style>

