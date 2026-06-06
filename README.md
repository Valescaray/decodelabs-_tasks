# Job Processor Dashboard

A full-stack distributed job processing application built with a Node.js/Express frontend, a FastAPI backend, a Redis queue/store, and a Python background worker. The application allows users to submit jobs via a dashboard, queue them in Redis, and track their processing status in real time as they are handled by the background worker.

---

## Architecture Overview

- **Frontend**: Node.js & Express server (located in `frontend`) serving the client dashboard and proxying API requests
- **Backend API**: FastAPI server (located in `api`) handling job submission and job status lookup
- **Worker**: Python background service (located in `worker`) popping jobs from Redis and simulating task execution
- **Database/Cache**: Redis (configured via environment variables or docker-compose) used as a message broker and state store

---

## Installation

Follow these step-by-step commands to get the application set up locally:

### 1. Prerequisites
Ensure you have the following installed on your machine:
- [Docker](https://www.docker.com/) and Docker Compose (recommended setup)
- **OR** for manual setup:
  - [Node.js](https://nodejs.org/) (v16.x or higher recommended) and [npm](https://www.npmjs.com/)
  - [Python](https://www.python.org/) (v3.10 or higher recommended)
  - A running [Redis](https://redis.io/) server (or access to a remote instance)

### 2. Environment Setup
1. Create a `.env` file in the root directory by copying the `.env.example` file:
   ```bash
   cp .env.example .env
   ```
2. Populate the `.env` file with your Redis credentials and port mappings:
   ```env
   REDIS_HOST=redis
   REDIS_PORT=6379
   REDIS_PASSWORD=supersecret
   APP_ENV=production
   API_URL=http://api:8000
   FRONTEND_PORT=3000
   ```

### 3. Service-Specific Setup (For Non-Docker Setup)

If you are running the services individually without Docker Compose, set up each component:

#### Backend API Setup
1. Open a terminal and navigate to the `api` directory:
   ```bash
   cd api
   ```
2. Install the backend Python dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Create a `.env` file in the `api` directory if needed (or export the variables):
   ```env
   REDIS_HOST=localhost
   REDIS_PORT=6379
   REDIS_PASSWORD=supersecret
   ```

#### Worker Setup
1. Open a terminal and navigate to the `worker` directory:
   ```bash
   cd worker
   ```
2. Install the worker Python dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Create a `.env` file in the `worker` directory if needed (or export the variables):
   ```env
   REDIS_HOST=localhost
   REDIS_PORT=6379
   REDIS_PASSWORD=supersecret
   ```

#### Frontend Setup
1. Navigate to the `frontend` directory:
   ```bash
   cd frontend
   ```
2. Install the client dependencies:
   ```bash
   npm install
   ```
3. Configure the backend API URL in the `frontend/.env` file:
   ```env
   API_URL=http://localhost:8000
   PORT=3000
   ```

---

## Usage

### Running Locally

To run the application locally, you can use Docker Compose or run the services individually.

#### Option A: Running with Docker Compose (Recommended)
From the root directory, run:
```bash
docker compose up --build
```
The application dashboard will start running in your browser at [http://localhost:3000](http://localhost:3000).

#### Option B: Running Services Manually
Ensure your local Redis server is started, then:

##### 1. Start the FastAPI Server
From the `api` directory, run:
```bash
uvicorn main:app --reload --port 8000
```
The API will start running at [http://localhost:8000](http://localhost:8000).

##### 2. Start the Background Worker
From the `worker` directory, run:
```bash
python worker.py
```

##### 3. Start the Express Frontend
From the `frontend` directory, run:
```bash
npm start
```
The frontend dashboard will start running at [http://localhost:3000](http://localhost:3000).

---

## License

This project is licensed under the [MIT License](LICENSE).
