<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Open AI Gen: Automated Cinematic Pipeline</title>
    <style>
        :root { --gold: #c5a059; --bg: #0d0d0d; --card: #1a1a1a; --text: #e0e0e0; }
        body { font-family: 'Inter', sans-serif; background: var(--bg); color: var(--text); margin: 0; padding: 20px; display: flex; flex-direction: column; align-items: center; }
        .container { max-width: 800px; width: 100%; }
        .card { background: var(--card); padding: 25px; border-radius: 12px; border: 1px solid #333; margin-bottom: 20px; box-shadow: 0 10px 30px rgba(0,0,0,0.5); }
        h1 { color: var(--gold); text-transform: uppercase; letter-spacing: 2px; text-align: center; border-bottom: 1px solid var(--gold); padding-bottom: 10px; margin-top: 0; }
        label { color: var(--gold); font-size: 10px; font-weight: 800; text-transform: uppercase; display: block; margin-bottom: 8px; }
        input, select, textarea { width: 100%; padding: 12px; margin-bottom: 15px; background: #000; border: 1px solid #444; color: #fff; border-radius: 6px; font-size: 14px; }
        button { width: 100%; padding: 16px; background: var(--gold); border: none; border-radius: 6px; color: #000; font-weight: bold; cursor: pointer; text-transform: uppercase; transition: 0.3s; }
        button:hover { background: #fff; transform: translateY(-2px); }
        .status-box { font-size: 12px; color: #888; margin-top: 10px; padding: 10px; background: #111; border-radius: 4px; min-height: 20px; }
        .lock-preview { background: #000; padding: 15px; border: 1px dashed var(--gold); font-size: 12px; color: #aaa; margin-top: 15px; display: none; line-height: 1.5; }
        #resultArea img { width: 100%; border-radius: 10px; margin-top: 25px; border: 1px solid var(--gold); box-shadow: 0 0 20px rgba(197, 160, 89, 0.2); }
        .fix-tag { font-size: 11px; color: #00ff88; margin-top: 5px; opacity: 0.8; }
    </style>
</head>
<body>

<div class="container">
    <h1>Cinematic Pipeline</h1>

    <!-- PIPELINE STEP 1: VISION + BRAIN -->
    <div class="card">
        <label>1. Automated Style Interrogator</label>
        <p style="font-size: 12px; color: #888; margin-bottom: 15px;">Pipeline: [Vision AI] → [Brain AI] → [Framework Lock]</p>
        
        <input type="file" id="styleImage" accept="image/*">
        <input type="text" id="lockName" placeholder="Name this Framework (e.g. '80s-SciFi')">
        
        <button onclick="runFreePipeline()">Run Automated Analysis</button>
        
        <div id="status" class="status-box">Ready.</div>
        <div id="lockPreview" class="lock-preview"></div>
    </div>

    <!-- PIPELINE STEP 2: GENERATION -->
    <div class="card">
        <label>2. Subject + Action</label>
        <select id="styleSelect" onchange="updatePreview()"></select>
        <textarea id="userPrompt" rows="3" placeholder="What is the character doing?"></textarea>
        
        <button onclick="generateImage()">Generate Master Shot</button>
        <div class="fix-tag">✓ Scribble-Fix Active | ✓ Text-Block Active | ✓ Free Pipeline</div>
    </div>

    <div id="resultArea"></div>
</div>

<script>
    let savedLocks = JSON.parse(localStorage.getItem('freeCinematicLocks') || '{}');
    updateDropdown();

    function updateDropdown() {
        const select = document.getElementById('styleSelect');
        select.innerHTML = '<option value="">Standard (No Lock)</option>';
        for (let name in savedLocks) {
            let opt = document.createElement('option');
            opt.value = name;
            opt.innerText = `FRAMEWORK: ${name}`;
            select.appendChild(opt);
        }
    }

    function updatePreview() {
        const name = document.getElementById('styleSelect').value;
        const preview = document.getElementById('lockPreview');
        if (name && savedLocks[name]) {
            preview.style.display = "block";
            preview.innerText = savedFrameworkData(name);
        } else {
            preview.style.display = "none";
        }
    }

    function savedFrameworkData(name) {
        return savedLocks[name];
    }

    /**
     * THE PIPELINE:
     * 1. Hugging Face (Vision) - Truly free image captioning.
     * 2. Pollinations (Brain) - Anonymous text expansion into the Cinematic Framework.
     */
    async function runFreePipeline() {
        const file = document.getElementById('styleImage').files[0];
        const name = document.getElementById('lockName').value;
        const status = document.getElementById('status');

        if (!file || !name) return alert("Select an image and name your style!");

        status.innerText = "[1/2] Vision AI: Seeing the image...";
        
        try {
            // STEP A: Vision Analysis (using Hugging Face BLIP - No Key required for basic use)
            const imageData = await file.arrayBuffer();
            const visionRes = await fetch("https://api-inference.huggingface.co/models/Salesforce/blip-image-captioning-large", {
                method: "POST",
                body: imageData,
            });
            const visionResult = await visionRes.json();
            const baseCaption = visionResult[0].generated_text;

            // STEP B: Framework Expansion (using Anonymous Pollinations Text)
            status.innerText = "[2/2] Brain AI: Constructing Cinematic Framework...";
            
            const frameworkInstruction = `Turn this description into a Cinematic Framework: "${baseCaption}". 
            Structure it using these tags: [VISUAL STYLE], [LIGHTING], [CAMERA], [TEXTURE]. 
            Focus on artistic medium and film tech. No subject description. Max 25 words. 
            Add 'Negative Constraints: no text, no scribbles'.`;

            const brainRes = await fetch(`https://text.pollinations.ai/${encodeURIComponent(frameworkInstruction)}`);
            const frameworkLock = await brainRes.text();

            // STEP C: Save
            savedLocks[name] = frameworkLock.replace(/["']/g, "");
            localStorage.setItem('freeCinematicLocks', JSON.stringify(savedLocks));
            
            status.innerText = "✅ Pipeline Complete. Framework Locked.";
            updateDropdown();
            updatePreview();
        } catch (e) {
            status.innerText = "Error in Pipeline. Ensure image is under 4MB.";
            console.error(e);
        }
    }

    async function generateImage() {
        const prompt = document.getElementById('userPrompt').value;
        const lockName = document.getElementById('styleSelect').value;
        const resultArea = document.getElementById('resultArea');

        if (!prompt) return alert("Enter your story!");

        resultArea.innerHTML = "<p style='text-align:center;'>AI is mastering the cinematic shot...</p>";

        // The Scribble Fixer (The Infographic Negatives)
        const negativeGuards = "no text, no letters, no numbers, no scribbles, no watermarks, no signatures, no warped objects, clean high-end photography";

        let finalPrompt = "";
        if (lockName && savedLocks[lockName]) {
            // Apply the structure: Story + Locked Framework + Negatives
            finalPrompt = `[STORY]: ${prompt}. [CINEMATIC FRAMEWORK]: ${savedLocks[lockName]}. [GUARDRAILS]: ${negativeGuards}.`;
        } else {
            finalPrompt = `${prompt}. [GUARDRAILS]: ${negativeGuards}.`;
        }

        const seed = Math.floor(Math.random() * 999999);
        const url = `https://image.pollinations.ai/prompt/${encodeURIComponent(finalPrompt)}?model=flux&width=1280&height=720&nologo=true&seed=${seed}`;

        const img = new Image();
        img.src = url;
        img.onload = () => {
            resultArea.innerHTML = `<img src="${url}">`;
        };
    }
</script>

</body>
</html>
