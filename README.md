# Campus Ride — Distributed Transportation Platform 🚗 🌱

[cite_start]Campus Ride is a real-time, green transportation sharing platform built for a university campus environment[cite: 17]. [cite_start]This application demonstrates a production-ready, three-tier distributed architecture running across containers, managed by Kubernetes orchestration, and deployed to a live cloud environment[cite: 18, 19].

---

## 1. System Architecture Blueprint
[cite_start]The platform implements a classic three-tier distributed systems architecture to fully separate presentation layers, business routing logic, and data persistence models[cite: 22, 23]:

```text
  ┌────────────────────────────────────────────────────────┐
  │               Presentation Tier (T1)                   │
  │  Next.js 16 + TypeScript + Tailwind CSS + Leaflet Maps │
  └───────────────────────────┬────────────────────────────┘
                              │
                              │ Stateless HTTP / JSON REST / WebSockets 
                              ▼
  ┌────────────────────────────────────────────────────────┐
  │                Application Tier (T2)                   │
  │          FastAPI (Python) + Uvicorn ASGI               │
  └───────────────────────────┬────────────────────────────┘
                              │
                              │ TCP Wire Protocol / asyncpg Database Connection [cite: 2, 29]
                              ▼
  ┌────────────────────────────────────────────────────────┐
  │                   Data Tier (T3)                       │
  │         PostgreSQL (Local) / Supabase BaaS             │
  └────────────────────────────────────────────────────────┘
Presentation Tier (T1): Client-side interface managing stateless user view layers and real-time geospatial Leaflet map socket updates.
Application Tier (T2): Microservice architecture handling the core business routing rules, security middleware pipelines, and CORS request processing asynchronously.
Data Tier (T3): Persistent data layer implementing database federation, Row-Level Security (RLS), and real-time database event streams.

2. Infrastructure Setup & Tools Reference

All tools must meet or exceed the following version baselines for successful system deployment:

| Tool Name      | Version Target | Purpose within the System                         | Installation References |
| Node.js        | v18.0.0        | Frontend Next.js compilation engine               | Node.js Official        |
| Python         | v3.11.0        | Backend FastAPI application runtime               | Python Downloads        |
| Docker Desktop | v4.25.0        | Local container isolation and image orchestration | Docker Desktop          |
| Kubectl Core   | v1.28.0        | Native Kubernetes cluster CLI manager             | Kubernetes Tools        |
| Railway CLI    | Latest         | Cloud environment production deployment interface | Railway.app Portal      |
| Wireshark      | Latest         | Live OSI packet capture network analysis          | Wireshark Go            |
| Artillery.io   | Latest         | High-throughput horizontal load stress engine     | Artillery Docs          |

3. Step-by-Step Environment Quickstart

Follow these operational blocks in exact order to launch the microservice network infrastructure:

Phase 1: Local Development Standup

  1. Clone the project repository context and explore the markdown configurations:

  Bash

  git clone [https://github.com/your-org/dist_sys_app.git](https://github.com/your-org/dist_sys_app.git)
  cd dist_sys_app

  2. Standardize your local FastAPI python environment to launch your backend server loop:

  Bash

  cd backend
  python -m venv .venv
  source .venv/bin/activate  # On Windows use: .venv\Scripts\activate
  pip install -r requirements.txt
  python server.py

  3. Initialize the Next.js runtime environment packages in a parallel terminal view block:

  Bash

  cd frontend
  npm install
  npm run dev

Phase 2: Docker Container Orchestration

  1. Compile isolated application images and launch the infrastructure network stack using Docker Compose:

  Bash

  docker-compose build
  docker-compose up -d

  2. Verify all local virtual node container groups are up and error-free:

  Bash

  docker ps
  docker-compose logs -f

Phase 3: Kubernetes Local Deployment

  1. Apply the cluster configurations, secret credentials, pod specifications, and service mappings:

  Bash

  kubectl apply -f k8s/

  2. Verify that your local Horizontal Pod Autoscaler target settings are operating properly:

  Bash

  kubectl get pods -w
  kubectl get svc,ingress,hpa

Phase 4: Production Cloud Infrastructure

  1. Bind your repository to the cloud deployment stream via the command suite interface:

  Bash

  railway login
  railway init

  2. Push your production configurations up to live remote nodes:

  Bash

  railway up

4. Environment Variables Reference Matrix

The application handles configuration states by splitting parameters between plain-text ConfigMaps and Base64-encrypted Secrets:

| Target Configuration Key | Scoping Type | Structural Purpose                                      | Source Value Provider                                 |
| NODE_ENV                 | ConfigMap    | Sets the runtime compilation optimization layer         | Set explicitly to "production"                        |
| BACKEND_HOST             | ConfigMap    | Targets the internal DNS routing key string for the API | Point to "campus-backend-service"                     |
| SUPABASE_URL             | Secret       | Endpoint path pointing to your auth service             | Your Supabase Project Settings API Tab                |
| SUPABASE_ANON_KEY        | Secret       | Public anonymous web API routing validation token       | Your Supabase Project Settings API Tab                |
| DATABASE_URL             | Secret       | Full direct pooling engine database connection string   | Your live Railway / Supabase DB connection parameters |

5. Kubernetes Manifest Inventory

The core cluster resource components stored inside the /k8s directory directory block are defined as follows:

-configmap.yaml: Manages non-sensitive, environment-specific key-value variables globally across target pods.
-secret.yaml: Securely encrypts and provisions critical access keys and database authorization credentials.
-postgres.yaml: Provisions the stateful database layer along with its isolated internal ClusterIP route address.
-backend.yaml: Launches two stateless backend API pods, configuring their resource constraints and health probes.
-frontend.yaml: Launches two frontend user interface layout pods bounded by a cluster-internal runtime service mapping.
-ingress.yaml: Implements an NGINX layer routing engine mapping root web requests straight to the frontend and /api to the backend.
-hpa.yaml: Evaluates real-time horizontal container scaling limits between 2 and 10 active pod copies matching a 70% CPU limit. 

6. Wireshark Network Capture Reference Filter Index

The following specific analysis filters are used to isolate network frames across various layers of the OSI model during transaction tracking:

| Applied Filter Query Code     | Inspected Target                 | Observed Distributed System Concept                                             |
| tcp.port == 8000              | Local HTTP API transactions      | Layer 4 TCP connection state tracking and handshake sequences                   |
| http.request.method == "POST" | Inbound structured requests      | Layer 6 Presentation encoding via JSON serialization                            |
| tcp.port == 5432              | Inter-container database traffic | Layer 3 Network internal bridge routing and Docker DNS lookup translations      |
| tls                           | Secure remote production traffic | Layer 7 Application traffic encryption established via a TLS handshake sequence |  

7. Production Deployment Details

-Live Production URL: https://campus-ride-frontend.up.railway.app
-Interactive Swagger Documentation Endpoint: https://campus-ride-backend.up.railway.app/docs
-Core Health Assessment Route: /health

8. Architectural Troubleshooting Manifest

Issue 1: Outbound API request connection blocked via browser with network CORS exceptions.
-Resolution: Open backend/app/main.py and ensure your live production Railway frontend domain is added to the allow_origins array whitelist.

Issue 2: Backend container instances fail initialization, outputting database driver timeout errors.
-Resolution: Verify that your DATABASE_URL uses the asynchronous schema string postgresql+asyncpg:// rather than the synchronous psycopg2 driver.

Issue 3: Real-time map location updates fail to stream between drivers and students.
-Resolution: Ensure that the positions table inside your Supabase project dashboard has the Realtime replication checkbox explicitly enabled.  

9. Team Project Allocation Matrix

The construction and maintenance duties for this distributed architecture deployment were allocated as follows:

| Student Name                            | Matric Number | Project Technical Contributions |
| Mohamad Hakim Bin Ahmad                 | 304953        | Part 5, 6, 7, 8                 |
| Muhammad Firdaus Bin Abdul Munir        | 304815        | Part 1 & 2                      |
| Muhammad Aqil Farhan Bin Saiful Huzairi | 306933        | Part 3 & 4                      |
