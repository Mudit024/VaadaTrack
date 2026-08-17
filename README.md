# 🏛️ VaadaTrack — Political Manifesto & Promise Tracker (MERN + AI)

<div align="center">

[![Live Demo](https://img.shields.io/badge/Live_Demo-VaadaTrack-00A651?style=for-the-badge&logo=render&logoColor=white)](https://vaadatrack-frontend.onrender.com/)
[![React](https://img.shields.io/badge/React_18-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)](https://reactjs.org/)
[![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![Express.js](https://img.shields.io/badge/Express.js-000000?style=for-the-badge&logo=express&logoColor=white)](https://expressjs.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB_Atlas-47A248?style=for-the-badge&logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![Groq AI](https://img.shields.io/badge/Groq_SDK-F05032?style=for-the-badge&logo=fastapi&logoColor=white)](https://groq.com/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2D1?style=for-the-badge&logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)

<p align="center">
  <strong>An intelligent, full-stack platform to track, verify, analyze, and compare political party manifestos and election promises in India using Retrieval-Augmented Generation (RAG) and ultra-fast LLM inference.</strong>
</p>

[🌐 **Visit Live Application**](https://vaadatrack-frontend.onrender.com/) • [✨ Features](#-key-features) • [🏗️ Architecture](#-system-architecture--rag-pipeline) • [🛠️ Tech Stack](#-tech-stack) • [🚀 Getting Started](#-getting-started) • [📡 API Reference](#-api-endpoints)

---

</div>

## 📌 Overview

During elections, political parties release extensive 100+ page manifestos containing thousands of promises. After elections, tracking what was actually accomplished is notoriously difficult for the general public.

**VaadaTrack** bridges this accountability gap by combining:
1. **Multi-Party Promise Tracking:** Transparently categorizing and tracking fulfillment status (*Fulfilled*, *In Progress*, *Broken*, *Pending*).
2. **AI-Powered Manifesto Analysis (RAG):** Allowing citizens to ask natural questions directly to official party manifestos and receive evidence-grounded answers.
3. **Cross-Party Policy Comparison:** Side-by-side comparative policy breakdowns across key national issues (Economy, Agriculture, Healthcare, Governance).
4. **AI Fact-Checking & Evidence Verification:** An admin suite that analyzes uploaded ground reports and factual evidence to calculate promise fulfillment scores.

---

## ✨ Key Features

### 👥 Public Citizen Portal
* **📊 Interactive Analytics Dashboard:** Real-time visual progress breakdowns using Chart.js, summarizing national fulfillment rates across political parties.
* **🔍 Search & Category Filters:** Filter promises by political party, election year, and key sector (*Agriculture, Health, Economy, Infrastructure, Defence, Social Welfare*).
* **🗳️ Community Voting & Sentiment:** Citizens can vote on promises they consider crucial or unfulfilled.
* **🤖 "Ask Manifesto" AI Chatbot:** Query specific election manifestos in plain language with citations grounded directly in the source text.
* **⚖️ Cross-Party Comparison Engine:** Select any two parties (e.g., BJP vs. INC) and a topic to generate an instant side-by-side policy matrix.

### 🛡️ Protected Admin & Fact-Checking Suite
* **📄 Automated PDF Manifesto Ingestion:** Upload official manifesto PDFs (`pdf-parse`) with automatic text chunking and vector embedding generation.
* **⚡ AI Promise Extraction:** Automatically scan hundreds of pages to extract actionable, measurable promises into structured database records.
* **🔍 Evidence-Based Promise Verification:** Submit factual news reports or government audits to let the AI score and classify promise fulfillment (*Fulfilled*, *Partially Fulfilled*, *Broken*).
* **🔐 JWT Role-Based Access Control:** Secure routes ensuring only verified administrators can manage parties and verify promises.

---

## 🏗️ System Architecture & RAG Pipeline

VaadaTrack uses a multi-threaded Retrieval-Augmented Generation (RAG) architecture to provide accurate, hallucination-free answers:

```mermaid
flowchart TD
    A[Manifesto PDF / Text] --> B[PDF-Parse & Chunker]
    B --> C[Node.js Worker Thread]
    C -->|@xenova/transformers| D[Vector Embeddings: all-MiniLM-L6-v2]
    D --> E[(MongoDB Atlas Database)]
    
    F[User Question] --> G[Query Vectorizer]
    G --> H[Cosine Similarity Search]
    E --> H
    H -->|Top Relevant Chunks| I[Context Assembler]
    I --> J[Groq SDK / High-Speed LLM]
    J --> K[Grounded, Cited AI Answer]
```

1. **Document Processing:** PDF text is parsed, cleaned, and split into semantic overlapping chunks (~1,500 characters).
2. **Multi-Threaded Vectorization:** Chunks are passed to a background Node.js `worker_thread` running `@xenova/transformers` (`Xenova/all-MiniLM-L6-v2`) to generate 384-dimensional embeddings without blocking the Express event loop.
3. **Similarity Search:** When a user queries a manifesto, cosine similarity locates the most relevant chunks.
4. **LLM Synthesis:** The retrieved chunks are fed with the question into Groq's high-speed LLM (`openai/gpt-oss-120b` / `llama-3.3-70b-versatile`) to generate a concise, neutral response.

---

## 🛠️ Tech Stack

| Domain | Technologies |
| :--- | :--- |
| **Frontend** | React 18, React Router v6, Tailwind CSS, Chart.js, React-Chartjs-2, Axios, React Markdown |
| **Backend** | Node.js, Express.js, Express Async Handler, CORS, Dotenv, JWT, bcryptjs |
| **Database** | MongoDB Atlas, Mongoose ODM |
| **AI & ML** | Groq Cloud SDK, Xenova Transformers (`all-MiniLM-L6-v2`), Node.js `worker_threads` |
| **Data Parsing** | Multer, PDF-Parse |
| **DevOps & Hosting** | Render, Docker, Docker Compose, Nginx |

---

## 📁 Repository Structure

```text
VaadaTrack/
├── backend/
│   ├── config/             # MongoDB database connection & DNS configuration
│   ├── controllers/        # Route controllers (Auth, Parties, Promises, Manifestos, AI)
│   ├── middleware/         # JWT Authentication & Multer upload middleware
│   ├── models/             # Mongoose schemas (User, Party, Promise, Manifesto)
│   ├── routes/             # Express API route declarations
│   ├── services/           # AI services (Groq LLM) & RAG semantic search engine
│   ├── utils/              # Database seeding scripts (parties, promises, manifestos)
│   ├── workers/            # Multi-threaded embedding generation worker
│   ├── Dockerfile          # Backend container specification
│   ├── package.json        # Backend dependencies & scripts
│   └── server.js           # Express app entrypoint
├── frontend/
│   ├── public/             # Static assets & index.html
│   ├── src/
│   │   ├── components/     # Reusable UI components (Navbar, Footer, Modals, Cards)
│   │   ├── pages/          # Application views (Home, Parties, Promises, Manifestos, Compare, Admin, Login)
│   │   ├── services/       # Axios API client services
│   │   ├── App.js          # Root routing & layout
│   │   └── index.css       # Tailwind CSS directives
│   ├── Dockerfile          # Multi-stage Nginx container specification
│   ├── nginx.conf          # Nginx reverse proxy configuration
│   └── package.json        # Frontend dependencies & scripts
├── docker-compose.yml      # Multi-container orchestration
├── package.json            # Root scripts (concurrent development launcher)
└── README.md               # Project documentation
```

---

## 🚀 Getting Started

### Prerequisites
* **Node.js:** `v18.x` or higher
* **npm:** `v9.x` or higher
* **MongoDB:** Free cluster from [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) or local instance
* **Groq API Key:** Free key from [Groq Console](https://console.groq.com/keys)

---

### 1. Clone the Repository
```bash
git clone https://github.com/Mudit024/VaadaTrack.git
cd VaadaTrack
```

### 2. Install Dependencies
Install dependencies for root, backend, and frontend with a single command:
```bash
npm run install-all
```

### 3. Setup Environment Variables
Create a file named `.env` inside the `backend/` directory:
```env
PORT=8000
MONGODB_URI=your_mongodb_atlas_connection_string
JWT_SECRET=your_super_secret_jwt_key
GROQ_API_KEY=your_groq_api_key
FRONTEND_URL=http://localhost:3000
```

### 4. Seed Database (Parties, Promises & Manifestos)
Run the automated seeders to populate initial data and the admin user:
```bash
cd backend
node utils/seed.js
node utils/seedManifestos.js
cd ..
```

> **Default Seed Admin Credentials:**
> - **Email:** `admin@vaadatrack.com`
> - **Password:** `admin123`

### 5. Run the Application
Launch both backend and frontend concurrently:
```bash
npm run dev
```

* **Frontend:** Open [http://localhost:3000](http://localhost:3000) in your browser.
* **Backend API:** Live at [http://localhost:8000](http://localhost:8000).

---

## 🐳 Running with Docker

You can build and spin up the entire application stack using Docker Compose:

```bash
docker-compose up --build
```
* **Frontend:** [http://localhost:3000](http://localhost:3000) (Served via Nginx)
* **Backend:** [http://localhost:8000](http://localhost:8000)

---

## 📡 API Endpoints

### 🔐 Authentication (`/api/auth`)
| Method | Endpoint | Description | Access |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/auth/register` | Register a new user | Public |
| `POST` | `/api/auth/login` | Authenticate and obtain JWT token | Public |
| `GET` | `/api/auth/me` | Fetch authenticated user profile | Private |

### 🏛️ Parties (`/api/parties`)
| Method | Endpoint | Description | Access |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/parties` | Retrieve all political parties | Public |
| `GET` | `/api/parties/:id` | Get details and promises for a party | Public |
| `GET` | `/api/parties/compare` | Compare party statistics | Public |
| `POST` | `/api/parties` | Create new political party | Admin |
| `PUT` | `/api/parties/:id` | Update political party info | Admin |
| `DELETE` | `/api/parties/:id` | Remove political party | Admin |

### 🎯 Promises (`/api/promises`)
| Method | Endpoint | Description | Access |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/promises` | List promises with filters (party, category, status) | Public |
| `GET` | `/api/promises/stats/overview`| Aggregate fulfillment statistics & charts | Public |
| `POST` | `/api/promises/:id/vote` | Cast community upvote/downvote | Public |
| `POST` | `/api/promises` | Add tracked promise | Admin |
| `PUT` | `/api/promises/:id` | Update promise status and details | Admin |
| `DELETE` | `/api/promises/:id` | Delete promise record | Admin |

### 🤖 AI & RAG (`/api/ai`)
| Method | Endpoint | Description | Access |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/ai/chat` | General Indian politics chatbot | Public |
| `POST` | `/api/ai/ask-manifesto` | RAG-based Q&A over specific manifesto | Public |
| `POST` | `/api/ai/compare-manifestos` | Compare two manifestos on a specific topic | Public |
| `POST` | `/api/ai/extract-promises` | AI promise extraction from manifesto text | Admin |
| `POST` | `/api/ai/analyze-promise` | Fact-check and verify promise against evidence | Admin |

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:
1. Fork the repository.
2. Create your feature branch (`git checkout -b feature/AmazingFeature`).
3. Commit your changes (`git commit -m 'feat: Add AmazingFeature'`).
4. Push to the branch (`git push origin feature/AmazingFeature`).
5. Open a Pull Request.

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

<div align="center">
  <sub>Built with ❤️ for democratic transparency and citizen empowerment.</sub>
</div>
