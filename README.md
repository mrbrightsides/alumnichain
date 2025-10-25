# 🎓 AlumniChain - University Alumni Management Platform

[![Next.js](https://img.shields.io/badge/Next.js-15.3.4-black)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.8.3-blue)](https://www.typescriptlang.org/)
[![Base](https://img.shields.io/badge/Blockchain-Base-0052FF)](https://base.org/)
[![SpacetimeDB](https://img.shields.io/badge/Database-SpacetimeDB-purple)](https://spacetimedb.com/)
[![OnchainKit](https://img.shields.io/badge/OnchainKit-0.38.17-0052FF)](https://onchainkit.xyz/)

> Platform manajemen alumni modern dengan teknologi blockchain untuk Universitas Bina Darma. Menggabungkan verifikasi kredensial NFT, tracer study real-time, job board, skill-sharing marketplace, dan AI-powered career development.

## 🌟 Overview

AlumniChain adalah platform komprehensif yang menghubungkan alumni, universitas, dan industri melalui teknologi Web3. Platform ini dirancang selaras dengan prinsip **Society 5.0** dan **Industry 5.0** untuk menciptakan ekosistem pembelajaran berkelanjutan dan talent pipeline yang sustainable.

### 🎯 Key Features

#### 🏆 Core Features
- **NFT Credentials** - Ijazah digital terverifikasi di Base blockchain
- **Job Board** - Lowongan kerja eksklusif dengan sistem referral alumni
- **Skill-Sharing** - Marketplace pembelajaran peer-to-peer
- **Mentorship** - AI-powered mentor-mentee matching
- **Tracer Study** - Real-time analytics untuk akreditasi BAN-PT
- **Alumni Network** - Community engagement & networking

#### 🤖 Advanced Features
- **AI Job Matching** - Smart job recommendations based on skills & experience
- **Interview Practice** - AI mock interviews dengan feedback real-time
- **Skill Assessment** - Technical skill testing & certification
- **Gamification** - XP system, achievements, dan leaderboards
- **Real-time Notifications** - Live updates untuk aplikasi, endorsements, dan events

#### 👥 Social Features
- **Alumni Feed** - Share updates, achievements, dan job opportunities
- **Direct Messaging** - Private conversations antar alumni
- **Events Management** - Virtual & physical events dengan RSVP
- **Networking** - Connect dengan alumni berdasarkan industry & interests

#### 📊 Admin Panel
- **Analytics Dashboard** - Comprehensive insights & metrics
- **Bulk Import** - CSV/Excel import untuk data alumni
- **Announcements** - Push notifications ke seluruh alumni
- **Compliance Reports** - Auto-generate laporan akreditasi

## 🏗️ Tech Stack

### Frontend
- **Framework:** Next.js 15.3.4 with App Router
- **Language:** TypeScript 5.8.3
- **UI Library:** shadcn/ui + Radix UI
- **Styling:** Tailwind CSS with custom animations
- **State Management:** React Hooks + SpacetimeDB subscriptions
- **Forms:** React Hook Form + Zod validation

### Blockchain
- **Network:** Base (Ethereum L2)
- **SDK:** OnchainKit 0.38.17
- **Wallet:** Wagmi 2.15.5 + Viem 2.38.4
- **Smart Contracts:** NFT credentials minting

### Database
- **Database:** SpacetimeDB 1.6.1 (real-time)
- **Server Module:** Rust-based reducers
- **Client Bindings:** Auto-generated TypeScript

### Additional Tools
- **Mini-App:** Farcaster SDK integration
- **Analytics:** PostHog
- **Notifications:** Sonner toast system
- **Icons:** Lucide React
- **Date Handling:** date-fns
- **Charts:** Recharts

## 📦 Installation

### Prerequisites
```bash
# Required
- Node.js 18+ or 20+
- npm or yarn or pnpm
- Git

# For SpacetimeDB development
- Rust 1.70+
- SpacetimeDB CLI
```

### 1. Clone Repository
```bash
git clone https://github.com/mrbrightsides/alumnichain.git
cd alumnichain
```

### 2. Install Dependencies
```bash
npm install
# or
yarn install
# or
pnpm install
```

### 3. Environment Setup
Create a `.env.local` file (optional - most config is in code):
```env
# SpacetimeDB Configuration
NEXT_PUBLIC_SPACETIMEDB_URL=wss://testnet.spacetimedb.com
NEXT_PUBLIC_SPACETIMEDB_MODULE=your_module_name

# Optional: Analytics
NEXT_PUBLIC_POSTHOG_KEY=your_posthog_key
NEXT_PUBLIC_POSTHOG_HOST=https://app.posthog.com
```

### 4. SpacetimeDB Setup

#### Install SpacetimeDB CLI
```bash
curl --proto '=https' --tlsv1.2 -sSf https://install.spacetimedb.com | sh
```

#### Publish Server Module
```bash
cd spacetime-server
spacetime publish --project-path . your_module_name
```

#### Generate Client Bindings
```bash
spacetime generate --lang typescript --out-dir ../src/spacetime_module_bindings your_module_name
```

### 5. Run Development Server
```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

## 📂 Project Structure

```
alumnichain/
├── src/
│   ├── app/                      # Next.js App Router
│   │   ├── page.tsx              # Landing page
│   │   ├── layout.tsx            # Root layout
│   │   ├── dashboard/            # Alumni dashboard
│   │   ├── credentials/          # NFT credentials
│   │   ├── jobs/                 # Job board
│   │   ├── skills/               # Skill marketplace
│   │   ├── mentorship/           # Mentorship program
│   │   ├── tracer/               # Tracer study analytics
│   │   ├── feed/                 # Social feed
│   │   ├── messages/             # Direct messaging
│   │   ├── events/               # Event management
│   │   ├── network/              # Alumni networking
│   │   ├── ai-matching/          # AI job matching
│   │   ├── interview-practice/   # Mock interviews
│   │   ├── assessment/           # Skill assessments
│   │   ├── fundraising/          # Fundraising campaigns
│   │   ├── admin/                # Admin panel
│   │   └── api/                  # API routes
│   │       ├── proxy/            # External API proxy
│   │       ├── health/           # Health check
│   │       └── logger/           # Logging endpoint
│   ├── components/               # React components
│   │   ├── ui/                   # shadcn/ui components
│   │   ├── AchievementSystem.tsx # Gamification
│   │   ├── NotificationCenter.tsx# Notifications
│   │   ├── OnboardingWizard.tsx  # User onboarding
│   │   └── ...                   # Dialog components
│   ├── hooks/                    # Custom React hooks
│   ├── lib/                      # Utility libraries
│   │   ├── spacetime.tsx         # SpacetimeDB client
│   │   ├── utils.ts              # Helper functions
│   │   └── logger.ts             # Logging utility
│   ├── spacetime_module_bindings/# Generated TypeScript bindings
│   └── utils/                    # Utility functions
├── spacetime-server/             # SpacetimeDB Rust module
│   └── src/
│       └── lib.rs                # Server-side logic
├── public/                       # Static assets
│   └── .well-known/
│       └── farcaster.json        # Farcaster config
├── README.md                     # This file
└── package.json                  # Dependencies
```

## 🗄️ Database Schema (SpacetimeDB)

### Core Tables

#### `alumni`
```rust
- id: u64 (primary key)
- wallet_address: String (unique)
- name: String
- email: String
- graduation_year: u32
- major: String
- current_company: String
- current_position: String
- location: String
- bio: String
- xp: u32
- level: u32
- created_at: u64
```

#### `job`
```rust
- id: u64 (primary key)
- company: String
- title: String
- description: String
- requirements: String
- salary_min: u64
- salary_max: u64
- location: String
- job_type: JobType (Enum)
- posted_by: u64 (alumni_id)
- created_at: u64
```

#### `application`
```rust
- id: u64 (primary key)
- job_id: u64
- alumni_id: u64
- cover_letter: String
- status: ApplicationStatus (Enum)
- applied_at: u64
```

#### `skill`
```rust
- id: u64 (primary key)
- name: String
- category: SkillCategory (Enum)
- description: String
```

#### `endorsement`
```rust
- id: u64 (primary key)
- alumni_id: u64
- skill_id: u64
- endorser_id: u64
- status: EndorsementStatus (Enum)
- message: String
- created_at: u64
```

### Social Tables

#### `post`
```rust
- id: u64 (primary key)
- author_id: u64
- content: String
- post_type: PostType (Enum)
- created_at: u64
```

#### `event`
```rust
- id: u64 (primary key)
- title: String
- description: String
- event_type: EventType (Enum)
- location: String
- start_time: u64
- end_time: u64
- capacity: u32
- organizer_id: u64
```

#### `message`
```rust
- id: u64 (primary key)
- sender_id: u64
- receiver_id: u64
- content: String
- read: bool
- sent_at: u64
```

### Gamification Tables

#### `achievement`
```rust
- id: u64 (primary key)
- name: String
- description: String
- icon: String
- rarity: Rarity (Enum)
- xp_reward: u32
```

## 🔌 API Endpoints

### Internal API Routes

#### Health Check
```
GET /api/health
Response: { status: "ok", timestamp: number }
```

#### Proxy (for external APIs)
```
POST /api/proxy
Body: {
  protocol: string,
  origin: string,
  path: string,
  method: string,
  headers: object,
  body?: any
}
```

#### Logger
```
POST /api/logger
Body: {
  level: string,
  message: string,
  metadata?: object
}
```

### SpacetimeDB Reducers

All data mutations go through SpacetimeDB reducers:

- `register_alumni` - Create new alumni profile
- `update_alumni_profile` - Update profile information
- `create_job` - Post new job listing
- `apply_to_job` - Submit job application
- `update_application_status` - Update application (admin)
- `add_skill_to_alumni` - Add skill to profile
- `request_endorsement` - Request skill endorsement
- `approve_endorsement` - Approve endorsement request
- `create_post` - Create social feed post
- `like_post` / `unlike_post` - Like/unlike posts
- `comment_on_post` - Comment on posts
- `send_message` - Send direct message
- `create_event` - Create alumni event
- `register_for_event` - RSVP to event
- `send_connection_request` - Send connection request
- `accept_connection` - Accept connection
- `unlock_achievement` - Award achievement
- `create_notification` - Send notification

## 🎮 Gamification System

### XP & Levels
- **Complete Profile:** +50 XP
- **First Job Application:** +25 XP
- **Get Skill Endorsed:** +30 XP
- **Help Others (Endorsement):** +20 XP
- **Post on Feed:** +10 XP
- **Attend Event:** +15 XP

### Achievements
- 🌟 **Welcome Aboard** - Complete your profile (Common)
- 💼 **Job Hunter** - Apply to 5 jobs (Common)
- 🎯 **Skill Master** - Get 10 endorsements (Rare)
- 🤝 **Connector** - Make 20 connections (Rare)
- 👑 **Community Leader** - Reach Level 10 (Epic)
- 🏆 **Legend** - Unlock all achievements (Legendary)

## 🔐 Security & Privacy

- **Wallet Authentication** - Connect wallet required
- **Data Encryption** - SpacetimeDB handles encryption
- **NFT Verification** - Blockchain-based credential verification
- **Access Control** - Role-based admin permissions
- **API Rate Limiting** - Proxy endpoint protects external APIs

## 🚀 Deployment

### Deploy to Vercel (Recommended)

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel

# Set environment variables in Vercel dashboard
```

### SpacetimeDB Production

```bash
# Publish to production
spacetime publish --project-path spacetime-server production_module_name

# Update client bindings
spacetime generate --lang typescript --out-dir ./src/spacetime_module_bindings production_module_name
```

### Environment Variables (Production)
```env
NEXT_PUBLIC_SPACETIMEDB_URL=wss://spacetimedb.com
NEXT_PUBLIC_SPACETIMEDB_MODULE=production_module_name
```

## 📱 Farcaster Integration

Platform ini fully compatible dengan Farcaster Mini Apps:
- Manifest signing otomatis
- Toast notifications untuk status
- Mobile-optimized UI
- Deep linking support

## 🧪 Testing

```bash
# Run type checking
npm run build

# Lint code
npm run lint
```

## 🤝 Contributing

1. Fork the repository
2. Create feature branch: `git checkout -b feature/AmazingFeature`
3. Commit changes: `git commit -m 'Add AmazingFeature'`
4. Push to branch: `git push origin feature/AmazingFeature`
5. Open Pull Request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 👨‍💻 Credits

**Developer:** [mrbrightsides](https://github.com/mrbrightsides)  
**Website:** [rantai.elpeef.com](https://rantai.elpeef.com)  
**Institution:** Universitas Bina Darma

Built with:
- [Next.js](https://nextjs.org/)
- [OnchainKit](https://onchainkit.xyz/) by Coinbase
- [SpacetimeDB](https://spacetimedb.com/)
- [Base](https://base.org/) - Ethereum L2
- [shadcn/ui](https://ui.shadcn.com/)

## 🔗 Links

- **Live Demo:** [Your deployed URL]
- **Documentation:** [This README]
- **SpacetimeDB Docs:** https://spacetimedb.com/docs
- **OnchainKit Docs:** https://onchainkit.xyz/getting-started
- **Base Network:** https://base.org/

## 📞 Support

For support, email: [support@elpeef.com] or join our Discord server.

## 🎯 Roadmap

### Phase 1 (Completed) ✅
- ✅ Core platform setup
- ✅ Alumni registration & profiles
- ✅ Job board & applications
- ✅ Skill-sharing marketplace
- ✅ NFT credentials (Basic)

### Phase 2 (Completed) ✅
- ✅ Social features (Feed, Messages, Events)
- ✅ Networking & connections
- ✅ Gamification system
- ✅ Notification center

### Phase 3 (Completed) ✅
- ✅ AI job matching
- ✅ Interview practice
- ✅ Skill assessments
- ✅ Admin panel
- ✅ Analytics dashboard

### Phase 4 (Upcoming) 🚧
- 🔜 Mobile app (React Native)
- 🔜 Advanced NFT features
- 🔜 DAO governance
- 🔜 Token incentives
- 🔜 Multi-university support

---

**Made with ❤️ for Universitas Bina Darma Alumni**

*Empowering alumni through Web3 technology - Building the future of career development*
