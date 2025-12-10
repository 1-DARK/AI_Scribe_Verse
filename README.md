# 📊 AI Scribe Verse (AutoInsight)

**AI Scribe Verse** is a powerful, full-stack data analysis platform that automatically analyzes CSV and Excel files using AI-powered insights. Upload your dataset and receive comprehensive statistical analysis, beautiful visualizations, and AI-generated summaries—all through an intuitive web interface.

## ✨ What Makes It Special?

- **🔄 Dual Analysis Engines**: Separate specialized backends for numerical and categorical data
- **📈 Automatic Insights**: AI-generated summaries and statistical analysis
- **📊 Rich Visualizations**: Correlation matrices, histograms, boxplots, bar charts, and more
- **🎨 Modern UI**: Beautiful, responsive interface built with React and shadcn/ui
- **🔐 Secure & Private**: User authentication and data isolation via Supabase
- **☁️ Cloud-Ready**: Deployment-ready configuration for Railway and other platforms

---

## 🚀 Key Features

### 📊 Data Analysis
- **Numerical Analysis**
  - Summary statistics (mean, median, std deviation, quartiles)
  - Correlation matrix with heatmap visualization
  - Outlier detection using IQR method
  - Distribution plots (histograms with KDE)
  - Box plots for each numerical column
  - Missing value analysis
  
- **Categorical Analysis**
  - Value count distributions
  - Frequency analysis for each category
  - Rare category detection
  - Bar chart visualizations
  - Missing value tracking

### 🤖 AI-Powered Summaries
- Automatic interpretation of statistical results
- Natural language insights about your data
- Key findings and patterns highlighted
- Data quality assessment

### 💾 Data Management
- **File Upload**: Support for CSV and Excel (.xlsx) files
- **Dataset Preview**: View first rows of your data
- **Column Detection**: Automatic identification of data types
- **Error Handling**: Clear feedback for invalid files or formats

### 🎨 User Experience
- **Beautiful Landing Page**: Clear introduction to platform capabilities
- **Dark Theme**: Modern, professional interface design
- **Responsive Layout**: Works seamlessly on desktop and mobile
- **Toast Notifications**: Real-time feedback for user actions
- **Loading States**: Smooth transitions during analysis

### 🔐 Authentication & Security
- **Supabase Auth**: Secure user authentication system
- **Protected Routes**: Authenticated access to analysis features
- **Session Management**: Persistent login sessions
- **User Isolation**: Each user's data and analysis history is private

---

## 🛠️ Tech Stack

### Frontend
- **React 18.3** - Modern UI library with hooks
- **TypeScript 5.8** - Type-safe development
- **Vite 7.1** - Lightning-fast build tool and dev server
- **React Router v6** - Client-side routing
- **TanStack Query** - Powerful data fetching and caching
- **Zustand** - Lightweight state management

### UI & Styling
- **Tailwind CSS 3.4** - Utility-first CSS framework
- **shadcn/ui** - Beautiful, accessible component library
- **Radix UI** - Unstyled, accessible UI primitives
- **Lucide React** - Modern icon set
- **next-themes** - Dark mode support
- **Sonner** - Toast notifications

### Backend & Database
- **Supabase** - Backend-as-a-Service
  - PostgreSQL database
  - Real-time subscriptions
  - User authentication
  - Row-level security (RLS)
  - Database migrations

### Data Analysis Backend
- **FastAPI** - Modern, fast Python web framework
- **Pandas** - Data manipulation and analysis
- **NumPy** - Numerical computing
- **Matplotlib & Seaborn** - Data visualization
- **scikit-learn** - Machine learning utilities
- **PyTorch & Transformers** - AI model support
- **Python 3.9+** - Backend runtime

### Development Tools
- **ESLint** - Code linting
- **PostCSS & Autoprefixer** - CSS processing
- **React Hook Form** - Form management
- **Zod** - Schema validation
- **date-fns** - Date utilities

---

## 📥 Installation

### Prerequisites
- **Node.js** (v18 or higher)
- **npm** or **bun** package manager
- **Python 3.9+** (for analysis backend services)
- **Supabase Account** (for authentication and database)

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/AI_Scribe_Verse.git
cd AI_Scribe_Verse
```

### 2. Install Frontend Dependencies
```bash
# Using npm
npm install

# Or using bun (faster)
bun install
```

### 3. Install Python Dependencies

**For Numerical Analysis Service:**
```bash
cd services/numerical
pip install -r ../../requirements_numerical.txt
```

**For Categorical Analysis Service:**
```bash
cd services/categorical
pip install -r ../../requirements_categorical.txt
```

### 4. Environment Setup

Copy the example environment file and configure it:
```bash
cp .env.example .env
```

Edit `.env` with your credentials:
```env
# Frontend Environment Variables
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_PUBLISHABLE_KEY=your_supabase_anon_key
VITE_NUMERICAL_API_URL=http://localhost:8001
VITE_CATEGORICAL_API_URL=http://localhost:8002

# Backend Environment Variables (for deployment)
PYTHON_VERSION=3.11
ALLOWED_ORIGINS=http://localhost:5173
```

### 5. Supabase Setup

1. **Create a new Supabase project** at [supabase.com](https://supabase.com)
2. **Run the database migrations**:
   - Navigate to your Supabase project dashboard
   - Go to SQL Editor
   - Execute the SQL files in `supabase/migrations/` in order
3. **Copy your project credentials**:
   - Project URL → `VITE_SUPABASE_URL`
   - Anon/Public key → `VITE_SUPABASE_PUBLISHABLE_KEY`
4. **Enable Authentication**:
   - Configure email authentication in Supabase Auth settings

---

## 🏃 Running the Application

### Development Mode

You need to run three services concurrently:

**1. Start the Frontend**
```bash
npm run dev
# or
bun run dev
```
🌐 Frontend available at: `http://localhost:5173`

**2. Start the Numerical Analysis API**
```bash
cd services/numerical
uvicorn AutoInsight_numerical:app --reload --port 8001
```
🔢 Numerical API available at: `http://localhost:8001`

**3. Start the Categorical Analysis API**
```bash
cd services/categorical
uvicorn AutoInsight_categorical:app --reload --port 8002
```
📊 Categorical API available at: `http://localhost:8002`

### Production Build

**Build the Frontend:**
```bash
npm run build
# or
bun run build
```

**Preview Production Build:**
```bash
npm run preview
```

**Run Python Services in Production:**
```bash
# Numerical service
uvicorn services.numerical.AutoInsight_numerical:app --host 0.0.0.0 --port 8001

# Categorical service
uvicorn services.categorical.AutoInsight_categorical:app --host 0.0.0.0 --port 8002
```


## 📁 Project Structure

```
AI_Scribe_Verse/
├── src/
│   ├── components/
│   │   ├── ui/                    # shadcn/ui component library
│   │   └── [feature-components]   # Feature-specific components
│   ├── pages/
│   │   ├── Landing.tsx            # Landing/home page
│   │   ├── Auth.tsx               # Authentication page
│   │   ├── Chat.tsx               # Main analysis interface
│   │   └── NotFound.tsx           # 404 error page
│   ├── hooks/                     # Custom React hooks
│   ├── store/                     # Zustand state management
│   ├── integrations/
│   │   └── supabase/              # Supabase client and types
│   ├── lib/                       # Utility functions
│   ├── App.tsx                    # Root application component
│   └── main.tsx                   # Application entry point
│
├── services/
│   ├── numerical/
│   │   └── AutoInsight_numerical.py    # Numerical data analysis API
│   └── categorical/
│       └── AutoInsight_categorical.py  # Categorical data analysis API
│
├── supabase/
│   ├── migrations/                # Database migration SQL files
│   └── config.toml                # Supabase project configuration
│
├── public/                        # Static assets (images, etc.)
├── Dataset/                       # Sample datasets for testing
│
├── requirements_numerical.txt     # Python deps for numerical service
├── requirements_categorical.txt   # Python deps for categorical service
├── package.json                   # Node.js dependencies
├── tsconfig.json                  # TypeScript configuration
├── tailwind.config.ts             # Tailwind CSS configuration
├── vite.config.ts                 # Vite build configuration
├── railway.json                   # Railway deployment config
├── Procfile                       # Process file for deployment
└── README.md                      # This file
```

---




## 📖 Usage

### Getting Started

1. **Sign Up / Sign In**
   - Navigate to the Auth page
   - Create a new account or sign in with existing credentials
   - You'll be redirected to the analysis interface

2. **Upload Your Dataset**
   - Click the upload button or drag and drop your file
   - Supported formats: CSV, XLSX
   - Maximum file size and row limits may apply

3. **Choose Analysis Type**
   - **Numerical Analysis**: For datasets with numeric data (prices, measurements, counts, etc.)
   - **Categorical Analysis**: For datasets with text/category data (names, categories, labels, etc.)
   - The system will auto-detect column types

4. **Review Results**
   - **Dataset Preview**: See the first rows of your data
   - **Statistical Analysis**: Review summary statistics and metrics
   - **Visualizations**: Explore interactive charts and plots
   - **AI Summary**: Read natural language insights about your data

5. **Download or Save**
   - Export visualizations as images
   - Save analysis results for later reference
   - Share insights with your team

### Example Datasets

Sample datasets are included in the `Dataset/` folder for testing:
- Numerical data examples (sales, measurements, etc.)
- Categorical data examples (customer segments, product categories, etc.)

---

## 📜 Available Scripts

| Command | Description |
|---------|-------------|
| `npm run dev` | Start Vite development server (port 5173) |
| `npm run build` | Build optimized production bundle |
| `npm run build:dev` | Build in development mode |
| `npm run preview` | Preview production build locally |
| `npm run lint` | Run ESLint code linting |

---

## ⚙️ Configuration

### Frontend Configuration

**Tailwind CSS** (`tailwind.config.ts`)
- Theme customization (colors, fonts, spacing)
- Plugin configuration
- Content paths for purging

**TypeScript** (`tsconfig.*.json`)
- `tsconfig.json` - Base TypeScript configuration
- `tsconfig.app.json` - Application-specific settings
- `tsconfig.node.json` - Node.js environment settings

**Vite** (`vite.config.ts`)
- Build optimization settings
- Dev server configuration
- Plugin setup (React SWC)
- Path aliases

**ESLint** (`eslint.config.js`)
- Code linting rules
- React-specific rules
- TypeScript linting

### Backend Configuration

**CORS Settings**
- Both Python services support the `ALLOWED_ORIGINS` environment variable
- Default: `*` (all origins) for development
- Production: Set to your frontend domain

**Port Configuration**
- Numerical API: Port 8001 (default)
- Categorical API: Port 8002 (default)
- Frontend: Port 5173 (Vite default)

---

## 🔐 Environment Variables

### Frontend Variables

| Variable | Description | Required | Example |
|----------|-------------|----------|----------|
| `VITE_SUPABASE_URL` | Supabase project URL | Yes | `https://xxx.supabase.co` |
| `VITE_SUPABASE_PUBLISHABLE_KEY` | Supabase anon/public key | Yes | `eyJhbGc...` |
| `VITE_NUMERICAL_API_URL` | Numerical analysis API endpoint | Yes | `http://localhost:8001` |
| `VITE_CATEGORICAL_API_URL` | Categorical analysis API endpoint | Yes | `http://localhost:8002` |

### Backend Variables

| Variable | Description | Required | Default |
|----------|-------------|----------|----------|
| `PYTHON_VERSION` | Python runtime version | No | `3.11` |
| `ALLOWED_ORIGINS` | CORS allowed origins | No | `*` |

### Getting Supabase Credentials

1. Go to [supabase.com](https://supabase.com) and sign in
2. Select your project
3. Go to **Settings** → **API**
4. Copy the **Project URL** and **anon/public key**

---

## 🚀 Deployment

### Railway Deployment

This project includes configuration files for easy deployment to Railway:

1. **Create a Railway Account** at [railway.app](https://railway.app)

2. **Deploy Frontend**
   - Create a new project from GitHub repo
   - Railway will auto-detect Vite configuration
   - Add environment variables in Railway dashboard
   - Deploy will happen automatically

3. **Deploy Python Services**
   - Create two separate services in Railway
   - Service 1: Set root directory to `services/numerical/`
   - Service 2: Set root directory to `services/categorical/`
   - Add environment variables for each service
   - Railway will use `requirements_*.txt` files

4. **Update Frontend Environment**
   - Update `VITE_NUMERICAL_API_URL` with Railway numerical service URL
   - Update `VITE_CATEGORICAL_API_URL` with Railway categorical service URL
   - Redeploy frontend

### Other Platforms

**Vercel/Netlify** (Frontend)
- Build command: `npm run build`
- Output directory: `dist`
- Environment variables: Add all `VITE_*` variables

**Heroku/Render** (Python Services)
- Create two separate services
- Use `requirements_*.txt` files
- Set Python version to 3.11+
- Configure start commands:
  - Numerical: `uvicorn services.numerical.AutoInsight_numerical:app --host 0.0.0.0 --port $PORT`
  - Categorical: `uvicorn services.categorical.AutoInsight_categorical:app --host 0.0.0.0 --port $PORT`

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. **Fork** the repository
2. **Create** a feature branch
   ```bash
   git checkout -b feature/AmazingFeature
   ```
3. **Commit** your changes
   ```bash
   git commit -m 'Add some AmazingFeature'
   ```
4. **Push** to the branch
   ```bash
   git push origin feature/AmazingFeature
   ```
5. **Open** a Pull Request

### Development Guidelines
- Follow existing code style and conventions
- Write meaningful commit messages
- Add comments for complex logic
- Test your changes thoroughly
- Update documentation as needed

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

**Copyright (c) 2025 Harsh Vardhan Chauhan**

---



