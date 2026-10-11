# HackFest 2026 - Team 4C.5: Project Cost Estimator Pipeline

An automated, intelligent multi-agent pipeline designed to parse software & hardware project requirements and architectures from diagram images, predict development effort, estimate hardware and software component pricing, and generate structured project cost estimates.

---

## 🏛 System Architecture

```text
       Input Architecture Diagram Image
                     |
                     v
             [Gemini Agent]            <-- Image Analysis (Nvidea/image.py)
                     |
        +------------+------------+
        |                         |
        v                         v
 [Hardware Agent]         [Software Agent]    <-- Parallel Execution
 - Bill of Materials      - Effort Models (China dataset, COCOMO)
 - Live Price Search      - Role & Function Point Heuristics
        |                         |
        +------------+------------+
                     |
                     v
             [Nemotron LLM]            <-- Aggregator & Reasoning
             (nvidia/llama-3.1-nemotron-70b / nemotron-3-ultra)
                     |
                     v
             [Interactive UI]          <-- Step-by-step Status Cards &
             (pipeline_app)                Summary Breakdown (INR)
```

---

## 📂 Project Structure

```text
Team-4C.5/
├── Hardware agent/           # Hardware cost estimation & pricing search
│   ├── app.py
│   ├── requirements.txt
│   └── templates/
├── Models/                   # Trained ML effort models & benchmark datasets
│   ├── dataset/              # China, COCOMO, Maxwell, NASA datasets
│   ├── models/               # Serialized models (.joblib) & registry
│   └── SE_Cost_Estimation_Training_Evaluation.ipynb
├── Nvidea/                   # Visual diagram parser using vision models
│   ├── image.py
│   └── parts_preview.png
├── pipeline_app/             # Core Flask application and pipeline coordinator
│   ├── app.py
│   ├── local_model.py
│   ├── static/
│   └── templates/
├── software agent/           # Software role-hours & function-point heuristics
│   ├── app.py
│   ├── role_hours.json
│   └── work_items_fp.json
└── TEST images/              # Benchmark architecture diagrams for testing
```

---

## 🚀 Getting Started

### 1. Prerequisites
- Python 3.10+
- Node.js (for TinyFish live search tool, optional)

### 2. Environment Setup
Install the required dependencies:
```bash
pip install flask openai joblib numpy pydantic pillow
```

If utilizing live price searching via TinyFish:
```bash
npm install -g tinyfish
```

### 3. API Keys Configuration
- Copy `Hardware agent/.env.example` to `Hardware agent/.env` and provide your `SERPAPI_KEY`.
- Copy `Nvidea/key.txt.example` to `Nvidea/key.txt` and provide your NVIDIA API key, or set the environment variable:
  ```bash
  export GEMINI_API_KEY="your-key"   # Linux/macOS
  setx GEMINI_API_KEY "your-key"     # Windows
  ```

### 4. Running the Pipeline Application
```bash
cd pipeline_app
python app.py
```
Open your browser at `http://127.0.0.1:5002` to interact with the web interface.
