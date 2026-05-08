# Team Task Manager

A full-stack task management application built with modern web technologies.

## Tech Stack

### Frontend
- **React 18** - UI framework
- **Vite** - Fast build tool
- **TailwindCSS** - Utility-first CSS framework
- **React Router** - Client-side routing
- **Axios** - HTTP client
- **Context API** - State management
- **Zod** - Schema validation

### Backend
- **Node.js** - Runtime environment
- **Express** - Web framework
- **PostgreSQL** - Database
- **Prisma** - ORM
- **JWT** - Authentication
- **Zod** - Data validation

## Project Structure

```
team-task-manager/
├── client/                 # Frontend (React + Vite)
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── hooks/
│   │   ├── context/
│   │   ├── services/
│   │   ├── utils/
│   │   ├── styles/
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── index.html
│   ├── package.json
│   ├── vite.config.js
│   ├── tailwind.config.js
│   ├── postcss.config.js
│   └── .env.example
│
├── server/                 # Backend (Node.js + Express)
│   ├── src/
│   │   ├── middleware/
│   │   ├── routes/
│   │   ├── controllers/
│   │   ├── models/
│   │   ├── services/
│   │   ├── utils/
│   │   ├── validations/
│   │   ├── config/
│   │   └── index.js
│   ├── prisma/
│   │   └── schema.prisma
│   ├── package.json
│   ├── .env.example
│   └── nodemon.json
│
├── .gitignore
└── README.md
```

## Getting Started

### Prerequisites
- Node.js (v16+)
- PostgreSQL (v12+)
- npm or yarn

### Installation

#### Backend Setup
```bash
cd server
npm install
cp .env.example .env.local
npx prisma migrate dev
npm run dev
```

#### Frontend Setup
```bash
cd client
npm install
cp .env.example .env
npm run dev
```

## Available Scripts

### Backend
- `npm run dev` - Start development server with hot reload
- `npm run build` - Build for production
- `npm start` - Start production server
- `npm run prisma:migrate` - Run database migrations

### Frontend
- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build
- `npm run lint` - Run ESLint

## License

MIT