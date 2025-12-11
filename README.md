# Sleep Health and Lifestyle Analysis

A comprehensive statistical analysis of sleep health patterns and lifestyle factors using machine learning, containerized with Docker for reproducible execution.

## Prerequisites

Before running this project, ensure you have:
- **Docker Desktop** installed on your system ([Download here](https://www.docker.com/products/docker-desktop))
- At least **2GB of free disk space** for the Docker image
- Port **8888** available on your machine

## Running the Container

### Method 1: Using Docker Compose (Recommended)

This is the simplest method and automatically handles all configuration.

1. **Navigate to the project directory:**
   ```bash
   cd /path/to/315FinalProject_LKJM-main
   ```

2. **Build and start the container:**
   ```bash
   docker compose up
   ```
   
   On first run, this will:
   - Build the Python 3.10 environment
   - Install all required dependencies (pandas, scikit-learn, xgboost, etc.)
   - Start Jupyter Notebook server
   - **This may take 3-5 minutes**

3. **Access Jupyter Notebook:**
   - Look for a URL in the terminal output that looks like:
     ```
     http://127.0.0.1:8888/tree?token=abc123...
     ```
   - Copy and paste the **complete URL** (including the token) into your web browser
   - You should see the Jupyter Notebook interface with your project files

4. **Open and run the analysis:**
   - Click on `sleep_analysis.ipynb` to open the notebook
   - Run cells individually or use "Run All" from the Cell menu

5. **Stop the container:**
   - Press `Ctrl+C` in the terminal, then run:
     ```bash
     docker compose down
     ```

### Method 2: Using Docker Build and Run

If you prefer manual Docker commands:

1. **Build the Docker image:**
   ```bash
   docker build -t sleep-analysis .
   ```

2. **Run the container:**
   ```bash
   docker run -p 8888:8888 -v $(pwd):/workspace sleep-analysis
   ```
   
   This command:
   - Maps port 8888 from container to your local machine
   - Mounts the current directory so changes are saved locally

3. **Access Jupyter:**
   - Copy the URL with token from terminal output
   - Paste into your browser

4. **Stop the container:**
   ```bash
   docker ps  # Find the container ID
   docker stop <container_id>
   ```

## Troubleshooting

### Port 8888 Already in Use
If you get a "port already allocated" error:
```bash
# Find what's using port 8888
lsof -i :8888

# Or use a different port:
docker run -p 8889:8888 -v $(pwd):/workspace sleep-analysis
# Then access at http://127.0.0.1:8889
```

### Container Builds But Won't Start
```bash
# Clean up old containers and images
docker compose down
docker system prune -f

# Rebuild from scratch
docker compose up --build
```

### Changes Not Saving
Ensure you're using the volume mount (`-v $(pwd):/workspace`) when running manually, or use docker-compose which handles this automatically.

### No Token in URL
If the URL appears without a token, run this inside the container:
```bash
jupyter notebook list
```
## Project Structure

```
sleep-health-analysis/
├── data.csv                    # Dataset (374 observations)
├── sleep_analysis.ipynb        # Main analysis notebook
├── docker-compose.yml          # Docker Compose configuration
├── Dockerfile                  # Docker image specification
├── requirements.txt            # Python dependencies
└── README.md                   # Detailed project documentation
```

## Analysis Workflow

The notebook includes the following sections:

1. **Data Cleaning and Preparation** - Feature engineering, categorical variable creation
2. **Exploratory Data Analysis (EDA)** - Four comprehensive visualizations of lifestyle factors, occupation, and sleep quality predictors
3. **Predictive Modeling** - Machine learning models (Logistic Regression, Random Forest, XGBoost)
4. **Model Evaluation** - Cross-validation, hyperparameter tuning, confusion matrices, and performance metrics

---

**Authors:** Jyo Madhavarapu and Likith Kancharlapalli  