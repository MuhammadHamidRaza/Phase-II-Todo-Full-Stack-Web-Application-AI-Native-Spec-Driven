# Quickstart: Todo Full-Stack Web Application

## Prerequisites

- Node.js 18+ for frontend
- Python 3.10+ for backend
- PostgreSQL (Neon Serverless account)
- Docker (optional, for containerized setup)

## Environment Setup

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd <repository-name>
   ```

2. **Create environment files**
   ```bash
   cp .env.example .env
   ```

3. **Configure environment variables**
   ```bash
   # In .env file:
   # Backend
   DATABASE_URL="your_neon_postgres_connection_string"
   BETTER_AUTH_SECRET="your_auth_secret_key"

   # Frontend (same BETTER_AUTH_SECRET as backend)
   NEXT_PUBLIC_BETTER_AUTH_URL="http://localhost:3000"
   NEXT_PUBLIC_BETTER_AUTH_SECRET="your_auth_secret_key"
   ```

## Backend Setup (FastAPI)

1. **Navigate to backend directory**
   ```bash
   cd backend
   ```

2. **Create virtual environment and install dependencies**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   pip install fastapi uvicorn sqlmodel python-jose[cryptography] passlib[bcrypt] python-multipart python-dotenv
   ```

3. **Run database migrations**
   ```bash
   # This will be implemented as part of the setup
   ```

4. **Start the backend server**
   ```bash
   uvicorn main:app --reload --port 8000
   ```

## Frontend Setup (Next.js)

1. **Navigate to frontend directory**
   ```bash
   cd frontend
   ```

2. **Install dependencies**
   ```bash
   npm install
   # or
   yarn install
   ```

3. **Start the development server**
   ```bash
   npm run dev
   # or
   yarn dev
   ```

4. **Open the application**
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:8000
   - Backend docs: http://localhost:8000/docs

## Docker Setup (Alternative)

1. **Build and start services**
   ```bash
   docker-compose up --build
   ```

2. **Run database migrations in container**
   ```bash
   docker-compose exec backend python -c "from database.connection import create_db_and_tables; create_db_and_tables()"
   ```

## Initial Configuration

1. **Database Setup**
   - The application will automatically create required tables on first run
   - Ensure your Neon PostgreSQL connection string is properly configured

2. **Authentication Setup**
   - Better Auth will handle user registration/login
   - JWT tokens will be issued upon successful authentication
   - Backend will validate JWT tokens for all protected routes

3. **Environment Variables**
   - Ensure both frontend and backend use the same BETTER_AUTH_SECRET
   - Configure CORS settings appropriately for your deployment

## Testing the Setup

1. **Verify backend is running**
   - Visit http://localhost:8000/docs to see FastAPI documentation
   - Test the /api/health endpoint

2. **Verify frontend is running**
   - Visit http://localhost:3000
   - Try registering a new account
   - Verify you can create and manage tasks

3. **API Integration Test**
   - Register a user through the frontend
   - Verify tasks can be created, read, updated, and deleted
   - Verify user isolation (one user can't see another's tasks)