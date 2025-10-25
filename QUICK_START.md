# ⚡ Quick Start Guide - AlumniChain

Panduan cepat untuk development dan deployment AlumniChain.

## 🚀 Development Setup (5 menit)

```bash
# 1. Clone repo
git clone https://github.com/mrbrightsides/alumnichain.git
cd alumnichain

# 2. Install dependencies
npm install

# 3. Run development server
npm run dev

# 4. Open browser
# http://localhost:3000
```

That's it! 🎉

## 📤 Push ke GitHub

```bash
# 1. Create new repo di GitHub
# Nama: alumnichain
# Private/Public: Your choice

# 2. Add remote
git remote add origin https://github.com/mrbrightsides/alumnichain.git

# 3. Push all branches & tags
git push -u origin main
```

## 🌐 Deploy ke Production

### Option 1: Vercel (Recommended - 2 menit)

```bash
# Install Vercel CLI
npm i -g vercel

# Login
vercel login

# Deploy
vercel --prod

# Done! You'll get a URL like: https://alumnichain.vercel.app
```

### Option 2: Vercel via GitHub (Automatic)

1. Go to [vercel.com](https://vercel.com)
2. Click "New Project"
3. Import GitHub repo
4. Click "Deploy"
5. Done!

## 🗄️ SpacetimeDB Setup

```bash
# 1. Install SpacetimeDB CLI
curl --proto '=https' --tlsv1.2 -sSf https://install.spacetimedb.com | sh

# 2. Login
spacetime login

# 3. Publish server module
cd spacetime-server
spacetime publish --project-path . alumnichain-prod

# 4. Generate client bindings
spacetime generate --lang typescript --out-dir ../src/spacetime_module_bindings alumnichain-prod

# 5. Update SpacetimeDB connection in Vercel
# Environment Variables:
# NEXT_PUBLIC_SPACETIMEDB_URL=wss://spacetimedb.com
# NEXT_PUBLIC_SPACETIMEDB_MODULE=alumnichain-prod
```

## 🎯 Common Commands

```bash
# Development
npm run dev          # Start dev server
npm run build        # Build for production
npm run start        # Start production server

# Git
git status           # Check changes
git add .            # Stage all changes
git commit -m "msg"  # Commit with message
git push             # Push to GitHub

# Vercel
vercel               # Deploy preview
vercel --prod        # Deploy production
vercel logs          # View logs
vercel domains add   # Add custom domain

# SpacetimeDB
spacetime list                    # List modules
spacetime logs alumnichain-prod   # View logs
spacetime status alumnichain-prod # Check status
```

## 📁 Project Structure

```
alumnichain/
├── src/                    # Source code
│   ├── app/               # Pages & routes
│   ├── components/        # React components
│   ├── lib/              # Utilities
│   └── spacetime_module_bindings/ # Generated types
├── spacetime-server/      # Database server
├── public/               # Static files
├── README.md            # Main docs
├── DEPLOYMENT.md        # Deployment guide
├── ARCHITECTURE.md      # Technical docs
└── CONTRIBUTING.md      # Contributing guide
```

## 🔑 Key Features

- ✅ **NFT Credentials** - `/credentials`
- ✅ **Job Board** - `/jobs`
- ✅ **Skills Marketplace** - `/skills`
- ✅ **Mentorship** - `/mentorship`
- ✅ **Tracer Study** - `/tracer`
- ✅ **Social Feed** - `/feed`
- ✅ **Events** - `/events`
- ✅ **Admin Panel** - `/admin`

## 🛠️ Tech Stack

- **Frontend:** Next.js 15 + TypeScript
- **UI:** shadcn/ui + Tailwind CSS
- **Database:** SpacetimeDB (real-time)
- **Blockchain:** Base (Ethereum L2)
- **Wallet:** OnchainKit
- **Hosting:** Vercel

## 📞 Need Help?

- 📖 **Full docs:** [README.md](./README.md)
- 🚀 **Deployment:** [DEPLOYMENT.md](./DEPLOYMENT.md)
- 🏗️ **Architecture:** [ARCHITECTURE.md](./ARCHITECTURE.md)
- 🤝 **Contributing:** [CONTRIBUTING.md](./CONTRIBUTING.md)
- 🐛 **Issues:** Open GitHub Issue
- 💬 **Questions:** Open GitHub Discussion

## ✅ Pre-Launch Checklist

Before going public:

- [ ] All features tested locally
- [ ] Build successful (`npm run build`)
- [ ] SpacetimeDB module published
- [ ] Environment variables configured
- [ ] Custom domain added (optional)
- [ ] Documentation reviewed
- [ ] GitHub repo created
- [ ] Vercel deployment done
- [ ] Test production URL
- [ ] Share with team! 🎉

## 🎉 You're Ready!

Your AlumniChain platform is production-ready. Time to go live! 🚀

```bash
# Final check
npm run build  # ✅ Should pass

# Push to GitHub
git push

# Deploy to Vercel
vercel --prod

# Share your URL!
# https://your-alumnichain.vercel.app
```

---

**Made with ❤️ for Universitas Bina Darma**

*Building the future of alumni management with Web3*
