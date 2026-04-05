# Finance-Dashboard-Project

A clean, interactive personal finance dashboard built with React, Recharts, and Context API. Designed as a frontend internship assignment for Zorvyn.

---

## Live Preview

> Deploy via [CodeSandbox](https://codesandbox.io), [StackBlitz](https://stackblitz.com), or run locally.

---

## Tech Stack

| Layer         | Choice                          |
|---------------|---------------------------------|
| Framework     | React 18 (functional components + hooks) |
| Styling       | Inline styles with design tokens (no Tailwind dependency) |
| Charts        | Recharts (Area, Bar, Pie)       |
| State         | Context API + `useReducer`      |
| Persistence   | `localStorage`                  |
| Data          | Mock API (simulated `fetch` with `setTimeout`) |

---

## Getting Started

### Prerequisites
- Node.js 18+
- npm or yarn

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/your-username/finflow-dashboard.git
cd finflow-dashboard

# 2. Install dependencies
npm install

# 3. Start the dev server
npm run dev
```

The app will be available at `http://localhost:5173`

### Using with Create React App
```bash
npx create-react-app finflow
cd finflow
npm install recharts
# Replace src/App.js with finance-dashboard.jsx content
npm start
```

### Using with Vite (recommended)
```bash
npm create vite@latest finflow -- --template react
cd finflow
npm install recharts
npm run dev
```

---

## Project Structure

```
src/
├── App.jsx                  # Root entry — AppProvider wraps AppShell
│
├── context/
│   └── AppContext.jsx        # Context + useReducer (state, dispatch, addToast)
│
├── mockApi/
│   └── transactions.js       # Simulated API (setTimeout-based fetch)
│
├── data/
│   └── mockData.js           # 30 transactions across Jan–Mar 2025
│
├── components/
│   ├── layout/
│   │   ├── Sidebar.jsx       # Nav, role toggle, dark/light mode toggle
│   │   └── Topbar.jsx        # Breadcrumb, role badge, hamburger (mobile)
│   │
│   ├── ui/
│   │   ├── StatCard.jsx      # Summary metric cards with hover animation
│   │   ├── TxModal.jsx       # Add/Edit transaction modal
│   │   ├── TxRow.jsx         # Single transaction table row
│   │   ├── Toasts.jsx        # Toast notification container
│   │   ├── Skeleton.jsx      # Loading skeleton blocks
│   │   └── ApiLoader.jsx     # Spinner shown during mock API fetch
│   │
│   └── charts/
│       └── ChartTooltip.jsx  # Custom styled Recharts tooltip
│
├── pages/
│   ├── Dashboard.jsx         # Overview: stats, area chart, pie, bar chart
│   ├── Transactions.jsx      # Table with search, filter, sort, CSV export
│   └── Insights.jsx          # Category breakdown, monthly comparison, savings
│
└── utils/
    ├── format.js             # fmt(), fmtDate(), calcPct() helpers
    └── theme.js              # DARK / LIGHT token objects + useTheme hook
```

> **Note:** The submission is a single-file artifact (`finance-dashboard.jsx`) for easy evaluation. The structure above shows how it would be split in a real project.

---

## Features

### 1. Dashboard Overview
- **Total Balance** card with month-over-month delta
- **Total Income** card with trend indicator
- **Total Expenses** card
- **6-Month Area Chart** — balance trend over time
- **Donut Pie Chart** — spending by category
- **Grouped Bar Chart** — monthly income vs expenses comparison

### 2. Transactions
- Full table: Date · Description · Category · Type · Amount
- **Search** — live text search on description
- **Category filter** — dropdown across all 11 categories
- **Type filter** — All / Income / Expense
- **Sort** — by Date or Amount, ascending or descending
- **CSV Export** — exports currently filtered view
- Color-coded category and type badges

### 3. Role-Based UI (Frontend Simulation)
- **Admin** — can Add, Edit, and Delete transactions
- **Viewer** — read-only; all edit controls are hidden
- Role toggle in sidebar; active role shown in topbar badge
- Role preference persisted to `localStorage`

### 4. Insights
- **Top Spending Category** with total amount
- **Total Savings** with savings rate percentage
- **Monthly Avg Spend** across the 3-month window
- **Transaction count** breakdown (income vs expense)
- **Category Progress Bars** — visual spend % per category
- **Monthly Expense Bar Chart** with Jan→Feb and Feb→Mar % change
- **Savings Health Gauge** — gradient bar with 20% target marker
- Smart tip shown when savings rate is below target

### 5. State Management
All state flows through a single Context + `useReducer` setup:

| Action          | Effect                                  |
|-----------------|-----------------------------------------|
| `SET_TX`        | Populate from mock API on load          |
| `ADD_TX`        | Prepend new transaction                 |
| `EDIT_TX`       | Update transaction by ID                |
| `DELETE_TX`     | Remove transaction by ID                |
| `SET_ROLE`      | Switch between admin / viewer           |
| `TOGGLE_DARK`   | Toggle dark / light mode                |
| `TOGGLE_SIDEBAR`| Open/close mobile sidebar               |
| `SET_FILTER`    | Update any filter key                   |
| `ADD_TOAST`     | Show notification (auto-dismisses)      |

### 6. Mock API Integration
On app mount, `mockApi.fetchTransactions()` is called — a `Promise` that resolves after 1.2 seconds, simulating a real network request. During this time:
- A **spinner** with `"Fetching data… /api/transactions"` is shown on each page
- Once resolved, data populates and `localStorage` is checked for any saved changes

### 7. UI / UX
- **Dark / Light mode** toggle in sidebar (persisted)
- **Mobile responsive** — sidebar collapses to overlay with hamburger toggle; all grids use `auto-fit` / `minmax`; table horizontally scrollable
- **Loading skeletons** — card and chart skeletons on initial render
- **Empty state** — friendly message when no transactions match filters
- **Toast notifications** — success / error toasts for Add, Edit, Delete, Export
- **Hover animations** — stat cards lift on hover, table rows highlight
- **Page transitions** — `fadeUp` animation on page change

---

## Design Decisions

**Typography:** IBM Plex Mono for all data/numbers (terminal precision) paired with Syne for headings (geometric authority). This pairing is uncommon in dashboards and immediately distinguishes the design.

**Color Palette:**
- `#22d3a0` — Income / positive (acid green)
- `#f43f5e` — Expense / negative (rose red)
- `#6366f1` — Accent / interactive (indigo)
- `#0f172a` / `#f8fafc` — Dark / Light background

**Theme Architecture:** Two token objects (`DARK` and `LIGHT`) are merged with shared brand colors. The `useTheme()` hook reads `state.darkMode` and returns the appropriate object, making every component automatically theme-aware without prop drilling.

---

## Optional Features Implemented

| Feature              | Status  |
|----------------------|---------|
| Dark mode toggle     | ✅ Done  |
| LocalStorage persist | ✅ Done  |
| Mock API integration | ✅ Done  |
| Animations           | ✅ Done  |
| CSV export           | ✅ Done  |
| Sidebar navigation   | ✅ Done  |
| Toast notifications  | ✅ Done  |

---

## Assumptions Made

1. "Monthly comparison" refers to expense trend across Jan, Feb, Mar 2025 — the 3 months of mock data available.
2. Savings is calculated as `Total Income − Total Expenses` across all transactions (not month-specific).
3. The mock API delay (1.2s) is intentional to demonstrate the loading state — not a real network call.
4. Role switching is frontend-only for demonstration; no authentication layer is implemented.

---

## Author

**Pooja Shekharagouda Doddagoudar**  
Frontend Developer Intern Applicant  
poojasdoddagoudar@gmail.com

---

*Built for Zorvyn Assignment Portal — Finance Dashboard UI*
