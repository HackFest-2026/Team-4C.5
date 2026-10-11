# KLE Fest 2 - Project Cost Estimator Pipeline

## Architecture
`
Image Upload
    |
    v
[Gemini Agent]  <-- image.py from Nvidea/
    |
    +----------+----------+
    |                     |
    v                     v
[HW Agent]           [SW Agent]    <-- run IN PARALLEL
Price search         Effort model (china_model.joblib)
via TinyFish         + TinyFish licence pricing
    |                     |
    +----------+----------+
               |
               v
       [Nemotron LLM]
    nvidia/llama-3.1-nemotron-70b
    Final cost prediction (INR)
               |
               v
        [Frontend UI]
    Step-by-step status cards
    Cost banner + tables
    Chatbot (Nemotron + cache)
`

## Setup

1. Install Python deps:
   `
   pip install flask openai joblib numpy pydantic pillow
   `

2. Make sure TinyFish CLI is installed (for live price searches):
   `
   npm install -g tinyfish
   `

3. Set your Gemini API key:
   `
   setx GEMINI_API_KEY "your-key"
   `
   (or it reads from Nvidea/key.txt automatically)

4. Run:
   `
   cd pipeline_app
   python app.py
   `
   Open: http://127.0.0.1:5002

## Files used from existing project
- Nvidea/image.py        -- Gemini image analysis
- Nvidea/key.txt         -- NVIDIA/Gemini API keys
- models/china_model.joblib -- trained effort prediction model
- models/china_meta.json    -- model metadata
- software agent/role_hours.json   -- role-specific hour adjustments
- software agent/work_items_fp.json -- function-point heuristics
- Hardware agent/app.py  -- TinyFish search logic (reimported inline)
