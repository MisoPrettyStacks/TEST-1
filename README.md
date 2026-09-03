🎨 Open AI Generator
A privacy-first, free-to-use AI image generation platform that allows you to "lock" visual aesthetics and generate consistent content across different prompts.
Built with JavaScript, Pollinations.ai, and the Flux model.
🌟 Features
Zero Cost: No API keys, no subscriptions, no credits.
Privacy-First: No trackers, no analytics, and no accounts.
Aesthetic Lock: Upload a reference image (like a book cover or a specific art style), and the AI will extract the "vibe" to apply to all your future generations.
Flux Model: Powered by the state-of-the-art Flux image model for high-fidelity, coherent results.
Local Storage: Your saved styles stay on your device—no database required.
🛠️ How It Works
This tool uses a unique "Lock Prompt" workflow:
Analyze: When you upload a reference image, the app uses a vision-language model to describe the textures, lighting, and medium of the image without focusing on the subject.
Lock: This description is saved as a "Style."
Wrap: When you generate a new image, your prompt is automatically "wrapped" in that aesthetic description.
Example: If your style is "Neon Cyberpunk" and your prompt is "A cat," the AI receives: "An image with vibrant neon lighting and high-contrast synthwave aesthetic, depicting a cat."
🚀 Deployment (GitHub Pages)
Since this is a single-file application, you can go live in seconds:
Create a new repository on GitHub.
Upload index.html to the main branch.
Go to Settings > Pages.
Under Branch, select main and click Save.
Your site will be live at https://your-username.github.io/your-repo-name/
📖 Usage Guide
1. Creating a Style
Go to the "Create Lock Prompt" section.
Upload an image that has a "look" you love.
Give it a name (e.g., "VintageSketch" or "OilPainting").
Click Lock Aesthetic. The AI will analyze the style and save it to your dropdown menu.
2. Generating Images
Select your saved style from the dropdown.
Type what you want to see in the prompt box.
Click Create Image.
To go back to standard AI, select "Standard (No Lock)".
📡 Backend Providers
This project stays free and open by utilizing these amazing services:
Generation: Pollinations.ai (Flux Model)
Vision Analysis: Pollinations.ai (Text/Vision API)
Temporary Hosting: Catbox.moe (Used only during style analysis)
📜 License
MIT License - Feel free to fork, modify, and use this for your own projects!
