# Agent Forge 🤖

**Agent Forge** is an autonomous AI-powered software development agent that transforms high-level goals into fully functional code. Built with Next.js, TypeScript, and Google's Genkit AI framework, it intelligently decomposes tasks, generates code, executes it, analyzes errors, and iteratively fixes issues until successful execution.

---

## 🌟 What is Agent Forge?

Agent Forge is an **intelligent coding assistant** that automates the entire software development lifecycle—from planning to implementation to debugging. Simply describe what you want to build, and the agent will:

1. **Break down your goal** into actionable subtasks
2. **Generate code** for each subtask using AI
3. **Detect and analyze errors** automatically
4. **Fix issues iteratively** until the code works
5. **Generate a live preview** of your application

Think of it as having an AI software engineer that works alongside you, handling repetitive coding tasks while you focus on high-level design and architecture decisions.

---

## 🎯 Core Features

### 1. **Task Planner (Task Decomposition)**
- Uses AI to break down high-level software development goals into a series of smaller, manageable subtasks
- Analyzes existing code context (package.json, source files) to understand the current application state
- Creates intelligent task plans that account for dependencies and library requirements
- Automatically adds subtasks to update package.json when new libraries are needed

### 2. **Code Generator**
- Employs Google's Gemini AI to generate code snippets for each subtask
- Leverages modern frameworks: Next.js, React, TypeScript, and Node.js
- Maintains context across all generated files to avoid redundant definitions
- Intelligently updates existing files (like package.json) rather than recreating them
- Generates complete, production-ready code with proper imports and structure

### 3. **Error Analyzer**
- Automatically detects errors in generated code
- Uses AI to analyze error messages and identify root causes
- Provides detailed explanations of what went wrong
- Suggests specific fixes based on error context

### 4. **Fix Loop (Self-Healing Code)**
- Implements an AI-powered iterative fix loop
- Automatically applies fixes and re-evaluates code
- Retries up to 10 times with progressively refined solutions
- Handles API rate limits with exponential backoff
- Only moves to the next task when code executes successfully

### 5. **Context-Aware Development**
- Upload existing code files to provide context
- Maintains memory of all previously generated files
- Builds upon existing code rather than starting from scratch
- Supports GitHub repository imports (coming soon)

### 6. **Live Preview Generator**
- Automatically creates a `preview.html` file as the final step
- Uses TailwindCSS Play CDN for instant styling
- Displays a live, interactive preview of your application
- Opens in a sandboxed iframe for security

### 7. **Real-Time Activity Feed**
- Visual representation of the agent's thought process
- Shows task plans, generated code, and error analyses
- Color-coded status indicators (info, success, error, analysis)
- Live streaming updates as the agent works

---

## 🏗️ Architecture & How It Works

### System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        USER INPUT                            │
│              (High-level development goal)                   │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                   AGENT WORKSPACE (UI)                       │
│  • Goal Input Form                                           │
│  • Context Upload (files/GitHub)                             │
│  • Real-time Activity Feed                                   │
│  • File Viewer & Live Preview                                │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                    SERVER ACTIONS                            │
│  • runTaskDecomposition()                                    │
│  • runCodeGeneration()                                       │
│  • runErrorAnalysis()                                        │
│  • runFixLoop()                                              │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                    AI FLOWS (Genkit)                         │
│                                                               │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  1. TASK DECOMPOSITION FLOW                          │   │
│  │     • Analyzes goal + context                        │   │
│  │     • Outputs JSON array of subtasks                 │   │
│  │     • Ensures preview.html is last task              │   │
│  └─────────────────────────────────────────────────────┘   │
│                       │                                      │
│                       ▼                                      │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  2. CODE GENERATION FLOW                             │   │
│  │     • Receives task description + context            │   │
│  │     • Generates code + filename                      │   │
│  │     • Uses relevant files to maintain coherence      │   │
│  └─────────────────────────────────────────────────────┘   │
│                       │                                      │
│                       ▼                                      │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  3. ERROR ANALYSIS FLOW                              │   │
│  │     • Analyzes error messages                        │   │
│  │     • Identifies root cause                          │   │
│  │     • Suggests specific fixes                        │   │
│  └─────────────────────────────────────────────────────┘   │
│                       │                                      │
│                       ▼                                      │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  4. FIX LOOP FLOW                                    │   │
│  │     • Applies suggested fixes                        │   │
│  │     • Regenerates corrected code                     │   │
│  │     • Returns fixed code for re-evaluation           │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                               │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│              GOOGLE GEMINI 2.0 FLASH (AI Model)              │
│  • Processes prompts                                         │
│  • Returns structured outputs (JSON schemas via Zod)         │
│  • Powered by @genkit-ai/googleai                            │
└─────────────────────────────────────────────────────────────┘
```

### Data Flow (Step-by-Step)

1. **User submits a goal** (e.g., "Build a weather dashboard with charts")
2. **Context gathering**: Uploads existing files or provides repo context
3. **Task Decomposition Flow**:
   - AI analyzes the goal and context
   - Returns structured JSON with subtasks:
     ```json
     [
       { "title": "Update package.json", "description": "Add recharts library" },
       { "title": "Create WeatherCard component", "description": "..." },
       { "title": "Create Dashboard page", "description": "..." },
       { "title": "Generate preview.html", "description": "..." }
     ]
     ```
4. **For each subtask**:
   - **Code Generation Flow** generates code + filename
   - **Execution attempt** (simulated via validation)
   - **If error occurs**:
     - **Error Analysis Flow** diagnoses the issue
     - **Fix Loop Flow** generates corrected code
     - Retry (up to 10 attempts)
   - **If success**: Store file and move to next task
5. **File Management**:
   - Updates existing files (like package.json)
   - Accumulates new files in memory
   - Builds context string for subsequent tasks
6. **Preview Generation**:
   - Final task creates preview.html
   - Embeds TailwindCSS CDN
   - Renders in iframe with sandboxing

---

## 🛠️ Tech Stack

### Frontend
- **[Next.js 15.3.3](https://nextjs.org/)** - React framework with App Router
- **[React 18.3.1](https://react.dev/)** - UI library
- **[TypeScript 5](https://www.typescriptlang.org/)** - Type-safe JavaScript
- **[Tailwind CSS 3.4](https://tailwindcss.com/)** - Utility-first CSS framework
- **[Radix UI](https://www.radix-ui.com/)** - Accessible component primitives
- **[Lucide React](https://lucide.dev/)** - Beautiful icon library
- **[shadcn/ui](https://ui.shadcn.com/)** - Re-usable component collection

### AI & Backend
- **[Genkit 1.13.0](https://firebase.google.com/docs/genkit)** - Firebase AI framework
- **[@genkit-ai/googleai](https://firebase.google.com/docs/genkit/plugins/google-ai)** - Google AI plugin
- **[Google Gemini 2.0 Flash](https://deepmind.google/technologies/gemini/)** - Large language model
- **[Zod 3.24.2](https://zod.dev/)** - Schema validation for AI outputs

### Form & Validation
- **[React Hook Form 7.54.2](https://react-hook-form.com/)** - Performant forms
- **[@hookform/resolvers](https://github.com/react-hook-form/resolvers)** - Schema validation integration

### Additional Libraries
- **[date-fns](https://date-fns.org/)** - Date utility library
- **[recharts](https://recharts.org/)** - Charting library
- **[embla-carousel](https://www.embla-carousel.com/)** - Carousel component

### Development Tools
- **[Turbopack](https://turbo.build/pack)** - Fast bundler (via `--turbopack` flag)
- **[genkit-cli](https://firebase.google.com/docs/genkit/devtools)** - Genkit development tools
- **[PostCSS](https://postcss.org/)** - CSS processor

### Deployment
- **[Firebase App Hosting](https://firebase.google.com/docs/app-hosting)** - Serverless hosting platform

---

## 📦 Installation & Setup

### Prerequisites
- **Node.js** (v20 or later recommended)
- **npm** or **yarn**
- **Google AI API Key** (for Gemini access)

### Step 1: Clone the Repository
```bash
git clone <repository-url>
cd Full-AI-Agent--main
```

### Step 2: Install Dependencies
```bash
npm install
```

### Step 3: Configure Environment Variables
Create a `.env.local` file in the root directory:

```env
# Google AI API Key (required)
GOOGLE_GENAI_API_KEY=your_api_key_here

# Optional: Firebase configuration
NEXT_PUBLIC_FIREBASE_API_KEY=your_firebase_api_key
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
```

To get a Google AI API key:
1. Visit [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Create a new API key
3. Copy and paste it into your `.env.local` file

### Step 4: Run Development Server
```bash
npm run dev
```

The application will be available at **http://localhost:9002**

### Step 5: (Optional) Run Genkit Dev Tools
For AI flow debugging and inspection:
```bash
npm run genkit:dev
```

This opens the Genkit developer UI for monitoring AI flows.

---

## 🚀 Usage

### Basic Workflow

1. **Open the application** at http://localhost:9002
2. **Enter your goal** in the input field (e.g., "Create a todo list app with drag-and-drop")
3. **(Optional) Upload context files** by clicking the attachment icon
4. **Click "Start"** to initiate the agent
5. **Watch the agent work** in the activity feed:
   - Task decomposition
   - Code generation for each subtask
   - Error detection and fixing (if needed)
   - Preview generation
6. **View generated files** in the "Files" tab
7. **See the live preview** in the "Preview" tab
8. **Continue with new goals** to iterate on the application

### Example Goals

#### Simple Examples
- "Create a landing page with a hero section and call-to-action button"
- "Build a contact form with validation"
- "Design a pricing table with three tiers"

#### Complex Examples
- "Build a dashboard with charts showing sales data, user growth, and revenue trends"
- "Create an e-commerce product catalog with filtering and search"
- "Develop a blog with markdown support, syntax highlighting, and dark mode"

### Tips for Best Results

1. **Be specific**: Include details about styling, functionality, and layout
2. **Provide context**: Upload existing files if you're modifying an app
3. **Start small**: Begin with simple features, then iterate
4. **Review generated code**: Check the Files tab to understand what was created
5. **Iterate**: Use the "Continue" feature to refine or add features

---

## 📁 Project Structure

```
Full-AI-Agent--main/
├── src/
│   ├── ai/                          # AI-related functionality
│   │   ├── genkit.ts                # Genkit AI instance configuration
│   │   ├── dev.ts                   # Development entry point for Genkit
│   │   └── flows/                   # AI workflows
│   │       ├── task-decomposition.ts   # Task planning AI flow
│   │       ├── code-generation.ts      # Code creation AI flow
│   │       ├── error-analysis.ts       # Error diagnosis AI flow
│   │       └── fix-loop.ts             # Code fixing AI flow
│   │
│   ├── app/                         # Next.js App Router
│   │   ├── layout.tsx               # Root layout component
│   │   ├── page.tsx                 # Home page
│   │   └── globals.css              # Global styles
│   │
│   ├── components/                  # React components
│   │   ├── agent/                   # Agent-specific components
│   │   │   ├── agent-workspace.tsx     # Main workspace UI
│   │   │   ├── goal-input-form.tsx     # Goal submission form
│   │   │   ├── code-block.tsx          # Code display component
│   │   │   ├── file-viewer.tsx         # File explorer
│   │   │   └── context-source-dialog.tsx  # Context upload modal
│   │   ├── layout/                  # Layout components
│   │   │   └── header.tsx              # App header
│   │   ├── ui/                      # shadcn/ui components
│   │   │   ├── button.tsx, dialog.tsx, tabs.tsx, etc.
│   │   └── icons.tsx                # Icon components
│   │
│   ├── hooks/                       # Custom React hooks
│   │   └── use-toast.ts             # Toast notification hook
│   │
│   └── lib/                         # Utilities and types
│       ├── actions.ts               # Server actions (API layer)
│       ├── types.ts                 # TypeScript type definitions
│       └── utils.ts                 # Utility functions
│
├── docs/
│   └── blueprint.md                 # Project design document
│
├── Configuration Files
├── next.config.ts                   # Next.js configuration
├── tsconfig.json                    # TypeScript configuration
├── tailwind.config.ts               # Tailwind CSS configuration
├── postcss.config.mjs               # PostCSS configuration
├── components.json                  # shadcn/ui configuration
├── apphosting.yaml                  # Firebase App Hosting config
├── package.json                     # Dependencies and scripts
└── .env.local                       # Environment variables (not in repo)
```

---

## 🎨 Design System

### Color Palette
- **Primary**: Midnight Blue (#191970) - Intelligence and stability
- **Background**: Dark Gray (#282828) - Professional focused environment
- **Accent**: Electric Blue (#7DF9FF) - Interactive elements and highlights
- **Success**: Green (#10B981) - Successful operations
- **Error**: Red (#EF4444) - Errors and failures
- **Warning**: Yellow (#F59E0B) - Analysis and warnings

### Typography
- **Font**: Inter (Grotesque sans-serif) - Modern, machined aesthetic
- Clean, minimalist design
- Structured layout with clear visual hierarchy

---

## 🔧 Available Scripts

```bash
# Start development server (port 9002, with Turbopack)
npm run dev

# Start Genkit development tools
npm run genkit:dev

# Start Genkit with file watching
npm run genkit:watch

# Build for production
npm run build

# Start production server
npm start

# Run ESLint
npm run lint

# Type-check without emitting files
npm run typecheck
```

---

## 🧪 How the AI Flows Work

### 1. Task Decomposition Flow
```typescript
// Input: High-level goal + optional context
{
  goal: "Build a weather app",
  context: "// existing code files..."
}

// Output: Array of subtasks
[
  { title: "Update package.json", description: "..." },
  { title: "Create WeatherCard component", description: "..." },
  { title: "Generate preview.html", description: "..." }
]
```

**Key Features**:
- Uses structured prompts with Zod schema validation
- Ensures preview.html is always the final task
- Adds dependency management subtasks automatically
- Context-aware (understands existing code)

### 2. Code Generation Flow
```typescript
// Input: Task description + relevant files
{
  taskDescription: "Create a WeatherCard component",
  relevantFiles: "// package.json + existing components..."
}

// Output: Code + filename
{
  code: "import React from 'react'...",
  fileName: "src/components/WeatherCard.tsx"
}
```

**Key Features**:
- Generates production-ready code
- Avoids redefining existing components
- Uses proper imports and typing
- Updates files intelligently (e.g., package.json merges)

### 3. Error Analysis Flow
```typescript
// Input: Code + error message
{
  code: "const x = undefined; x.map(...)",
  error: "TypeError: Cannot read property 'map' of undefined"
}

// Output: Diagnosis + fix suggestion
{
  rootCause: "Variable x is undefined when map is called",
  suggestedFix: "Add null check: x?.map(...) or initialize x as []"
}
```

### 4. Fix Loop Flow
```typescript
// Input: Broken code + error + context
{
  code: "// broken code...",
  errorMessage: "TypeError...",
  taskDescription: "Create WeatherCard",
  relevantFiles: "// context..."
}

// Output: Fixed code
{
  fixedCode: "// corrected code..."
}
```

**Key Features**:
- Iterative refinement (up to 10 attempts)
- Exponential backoff for rate limits
- Uses error analysis insights
- Maintains context across fix attempts

---

## 🚢 Deployment

### Firebase App Hosting

The project includes `apphosting.yaml` for Firebase App Hosting:

```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login to Firebase
firebase login

# Deploy
firebase deploy --only hosting
```

### Other Platforms

The application can be deployed to any platform that supports Next.js:

- **Vercel**: `vercel deploy`
- **Netlify**: Connect your repo and deploy
- **Docker**: Create a Dockerfile with `FROM node:20` and `npm run build && npm start`

**Environment Variables**: Don't forget to set `GOOGLE_GENAI_API_KEY` in your hosting platform's environment settings.

---

## 🐛 Troubleshooting

### API Rate Limit Errors
**Problem**: "503: Model is overloaded"
**Solution**: The agent automatically retries with exponential backoff. Wait a few seconds.

### Missing API Key
**Problem**: "Error: GOOGLE_GENAI_API_KEY not found"
**Solution**: Create `.env.local` with your API key (see Installation section)

### Port Already in Use
**Problem**: "Port 9002 is already in use"
**Solution**: Kill the process or change the port in `package.json` (`-p 9002`)

### TypeScript Errors
**Problem**: Type errors during development
**Solution**: Run `npm run typecheck` to identify issues

### Preview Not Loading
**Problem**: Preview tab shows blank screen
**Solution**: Ensure preview.html was generated (check Files tab). Browser may be blocking iframe.

---

## 🔐 Security Considerations

- **Sandboxed Preview**: The preview.html runs in a sandboxed iframe (`sandbox="allow-scripts"`)
- **No Code Execution**: Generated code is NOT executed on the server (only validated via AI)
- **API Key Security**: Store API keys in `.env.local` (never commit to Git)
- **Content Security**: Review generated code before deploying to production

---

## 🤝 Contributing

Contributions are welcome! Areas for improvement:

1. **GitHub Integration**: Implement repository import functionality
2. **Additional AI Providers**: Support OpenAI, Anthropic, etc.
3. **Real Code Execution**: Add sandboxed execution environment
4. **Export Functionality**: Download generated files as a .zip
5. **Project Templates**: Pre-built starting points (e-commerce, blog, etc.)
6. **Multi-file Editing**: Handle complex file dependencies better
7. **Testing Integration**: Generate unit tests automatically

---

## 📄 License

[Specify your license here - MIT, Apache 2.0, etc.]

---

## 🙏 Acknowledgments

- **Google Gemini** for powerful AI capabilities
- **Firebase Genkit** for the AI framework
- **shadcn/ui** for beautiful UI components
- **Radix UI** for accessible primitives
- **Next.js** for the amazing framework

---

## 📞 Support

For questions, issues, or feature requests:
- Open an issue on GitHub
- Check the [blueprint.md](docs/blueprint.md) for design decisions
- Review Genkit documentation: https://firebase.google.com/docs/genkit

---

## 🎓 Learning Resources

- [Next.js Documentation](https://nextjs.org/docs)
- [Firebase Genkit Guide](https://firebase.google.com/docs/genkit)
- [Google AI Documentation](https://ai.google.dev/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Tailwind CSS Docs](https://tailwindcss.com/docs)

---

**Built with ❤️ using AI and modern web technologies**
