# 🏗️ Architecture Documentation - AlumniChain

Dokumentasi arsitektur teknis platform AlumniChain.

## 📐 System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         CLIENT LAYER                             │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌───────────┐│
│  │  Next.js   │  │ OnchainKit │  │ SpacetimeDB│  │ Farcaster ││
│  │ Frontend   │  │   Wallet   │  │   Client   │  │    SDK    ││
│  └────────────┘  └────────────┘  └────────────┘  └───────────┘│
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                      APPLICATION LAYER                           │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌───────────┐│
│  │   Pages    │  │ Components │  │   Hooks    │  │   Utils   ││
│  │  (Routes)  │  │    (UI)    │  │  (Logic)   │  │ (Helpers) ││
│  └────────────┘  └────────────┘  └────────────┘  └───────────┘│
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                         DATA LAYER                               │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌───────────┐│
│  │ SpacetimeDB│  │    Base    │  │ Real-time  │  │   Cache   ││
│  │  (Tables)  │  │(Blockchain)│  │Subscriptions│ │  (Local)  ││
│  └────────────┘  └────────────┘  └────────────┘  └───────────┘│
└─────────────────────────────────────────────────────────────────┘
```

## 🔄 Data Flow

### 1. User Registration Flow
```
User → Connect Wallet → OnchainKit → Wallet Address
                                          ↓
                              SpacetimeDB Reducer (register_alumni)
                                          ↓
                              Create Alumni Record → Subscribe to Alumni Table
                                          ↓
                              Return Alumni Data → Update UI
```

### 2. Job Application Flow
```
User → Browse Jobs → SpacetimeDB Query (get_active_jobs)
                            ↓
                Display Jobs → User Applies → apply_to_job Reducer
                                                      ↓
                          Create Application Record → Notify Employer
                                                      ↓
                          Subscribe to Application → Real-time Status Updates
```

### 3. Real-time Updates Flow
```
Reducer Called → SpacetimeDB Server → Push to All Subscribed Clients
                                              ↓
                              Client Receives Update → React State Update
                                              ↓
                              UI Auto-updates (No Refresh Needed)
```

## 🧩 Component Architecture

### Page Structure
```
src/app/
├── page.tsx                    # Landing page
├── layout.tsx                  # Root layout (with Farcaster wrapper)
├── providers.tsx               # Context providers
├── dashboard/
│   └── page.tsx               # Dashboard with tabs navigation
├── [feature]/
│   └── page.tsx               # Feature-specific pages
```

### Component Hierarchy
```
App Layout
├── Header (Navigation)
├── NotificationCenter
├── AchievementSystem
├── Page Content
│   ├── Feature-specific components
│   │   ├── Dialogs (Create/Edit)
│   │   ├── Tables/Lists
│   │   ├── Cards
│   │   └── Forms
│   └── UI Components (shadcn/ui)
└── Footer
```

### Shared Components Pattern
```typescript
// Dialog Pattern
<DialogComponent>
  <DialogTrigger>
    <Button>Open</Button>
  </DialogTrigger>
  <DialogContent>
    <Form>
      <FormFields />
      <SubmitButton />
    </Form>
  </DialogContent>
</DialogComponent>

// Table Pattern
<DataTable>
  <TableHeader>
    <TableColumns />
  </TableHeader>
  <TableBody>
    {data.map(item => (
      <TableRow key={item.id}>
        <TableCells data={item} />
        <ActionButtons />
      </TableRow>
    ))}
  </TableBody>
</DataTable>
```

## 🗃️ Database Architecture

### SpacetimeDB Schema Design

#### Entity Relationship
```
Alumni (1) ──→ (N) Application ──→ (1) Job
   │                                    │
   │ (1)                           (1) │
   ↓                                    ↓
   Alumni_Skill (N) ──→ (1) Skill      Posted by Alumni
   │
   │ (1)
   ↓
   Endorsement (N)
   │
   │ (1)
   ↓
   Alumni (Endorser)
```

#### Table Relationships
```sql
-- One-to-Many
Alumni → Jobs (poster_id)
Alumni → Applications (alumni_id)
Alumni → Posts (author_id)
Alumni → Events (organizer_id)

-- Many-to-Many (through junction tables)
Alumni ←→ Skills (via Alumni_Skill)
Alumni ←→ Alumni (via Connection)
Alumni ←→ Events (via Event_Registration)

-- One-to-One
Alumni → Achievement (via Alumni_Achievement)
```

### Reducer Pattern
```rust
// SpacetimeDB Reducer Structure
#[spacetimedb::reducer]
pub fn reducer_name(
    ctx: &ReducerContext,
    param1: Type1,
    param2: Type2,
) -> Result<(), String> {
    // 1. Validate input
    // 2. Check permissions
    // 3. Perform database operations
    // 4. Return result
}
```

## 🔐 Authentication & Authorization

### Wallet-based Auth Flow
```
1. User clicks "Connect Wallet"
2. OnchainKit opens wallet selector
3. User approves connection
4. Wallet address stored in state
5. Check if alumni exists in SpacetimeDB
6. If not, show registration dialog
7. If yes, load user profile
```

### Permission Levels
```typescript
enum Role {
  Alumni = "alumni",        // Standard user
  Admin = "admin",          // University admin
  SuperAdmin = "super_admin" // Platform admin
}

// Permission checks in reducers
if (ctx.sender != alumni.wallet_address) {
  return Err("Unauthorized".into());
}
```

## 🎨 UI/UX Architecture

### Design System
```
Theme
├── Colors
│   ├── Primary (Blue/Purple gradient)
│   ├── Secondary (Pink/Rose)
│   ├── Success (Green)
│   ├── Warning (Amber)
│   └── Error (Red)
├── Typography
│   ├── Headings (font-bold)
│   ├── Body (text-base)
│   └── Caption (text-sm)
├── Spacing (Tailwind scale)
│   ├── px-4, py-2 (Small)
│   ├── px-6, py-4 (Medium)
│   └── px-8, py-6 (Large)
└── Animations
    ├── Hover effects
    ├── Transitions
    └── Loading states
```

### Responsive Breakpoints
```css
/* Mobile First Approach */
sm: 640px   /* Tablets */
md: 768px   /* Small laptops */
lg: 1024px  /* Desktops */
xl: 1280px  /* Large screens */
2xl: 1536px /* Ultra-wide */
```

## 🌐 API Architecture

### API Route Structure
```
/api/
├── proxy/              # External API proxy
│   └── route.ts       # POST handler
├── health/            # Health check
│   └── route.ts       # GET handler
└── logger/            # Logging endpoint
    └── route.ts       # POST handler
```

### Proxy Pattern
```typescript
// Client-side request
fetch('/api/proxy', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    protocol: 'https',
    origin: 'api.example.com',
    path: '/endpoint',
    method: 'GET',
    headers: {},
  }),
});

// Proxy forwards to external API
// Returns response to client
```

## 🚀 Performance Optimization

### Code Splitting
```typescript
// Dynamic imports for heavy components
const HeavyChart = dynamic(() => import('@/components/HeavyChart'), {
  loading: () => <Skeleton className="h-64" />,
  ssr: false, // Client-only rendering
});
```

### Data Fetching Strategy
```typescript
// SpacetimeDB subscriptions (real-time)
useEffect(() => {
  const unsubscribe = Alumni.onInsert(handleNewAlumni);
  return () => unsubscribe();
}, []);

// One-time queries (on-demand)
const jobs = await Job.filter((job) => job.active);
```

### Caching Strategy
```typescript
// React state for local cache
const [cachedData, setCachedData] = useState<Data[]>([]);

// SpacetimeDB handles server-side caching
// Real-time updates invalidate cache automatically
```

## 🔄 State Management

### State Architecture
```
Global State (SpacetimeDB)
├── Alumni Data
├── Jobs Data
├── Applications
├── Social Feed
└── Events

Local State (React)
├── UI State (modals, tabs)
├── Form State (inputs)
├── Loading States
└── Error States

Context (React Context)
├── Auth Context (wallet)
└── Theme Context (dark mode)
```

### State Flow
```typescript
// SpacetimeDB → React State
useEffect(() => {
  const subscription = Table.onUpdate((data) => {
    setLocalState(data); // Sync to React state
  });
  return () => subscription();
}, []);

// User Action → SpacetimeDB → All Clients
const handleSubmit = async () => {
  await reducer.call(data);
  // SpacetimeDB auto-updates all subscribed clients
};
```

## 🧪 Testing Architecture

### Testing Pyramid
```
               ┌─────────────┐
               │  E2E Tests  │
               │  (Minimal)  │
               └─────────────┘
          ┌────────────────────┐
          │ Integration Tests  │
          │    (Moderate)      │
          └────────────────────┘
     ┌───────────────────────────┐
     │     Unit Tests            │
     │     (Majority)            │
     └───────────────────────────┘
```

### Test Coverage Strategy
- **Unit Tests:** Utility functions, helpers
- **Integration Tests:** Component interactions
- **E2E Tests:** Critical user flows

## 🔧 Development Workflow

```
Developer
    ↓
  Edit Code
    ↓
Hot Reload (Next.js Dev Server)
    ↓
SpacetimeDB Connection
    ↓
Test Locally
    ↓
Git Commit
    ↓
Push to GitHub
    ↓
Vercel Auto-Deploy
    ↓
Production
```

## 📦 Deployment Architecture

```
GitHub Repository
    ↓
Vercel Build Pipeline
    ↓
    ├─→ Install Dependencies (npm install)
    ├─→ Type Check (TypeScript)
    ├─→ Build (next build)
    └─→ Deploy to Edge Network
         ↓
    Live Application
         ↓
    SpacetimeDB Connection (WebSocket)
         ↓
    Real-time Data Sync
```

## 🌍 Infrastructure

### Hosting
- **Frontend:** Vercel Edge Network
- **Database:** SpacetimeDB (managed)
- **Blockchain:** Base Network
- **CDN:** Vercel Edge (automatic)

### Scalability
- **Horizontal:** Vercel auto-scales
- **Database:** SpacetimeDB handles scaling
- **Real-time:** WebSocket connections managed by SpacetimeDB

## 🔍 Monitoring & Observability

### Metrics Tracked
```
Application Metrics
├── Page Load Time
├── API Response Time
├── Error Rate
└── User Engagement

Infrastructure Metrics
├── Vercel Build Time
├── Deployment Success Rate
└── SpacetimeDB Connection Status

Business Metrics
├── Active Users
├── Job Applications
├── Skill Endorsements
└── Event Registrations
```

## 🛡️ Security Architecture

### Security Layers
```
1. Transport Layer: HTTPS/WSS (TLS)
2. Authentication: Wallet signatures
3. Authorization: SpacetimeDB reducers
4. Data Validation: Zod schemas
5. API Protection: Proxy endpoint
6. Rate Limiting: Vercel edge functions
```

---

This architecture is designed for:
- ✅ Scalability
- ✅ Real-time performance
- ✅ Developer experience
- ✅ Maintainability
- ✅ Security

For questions about architecture decisions, open a discussion on GitHub.
