# 🚀 Deployment Guide - AlumniChain

Panduan lengkap untuk deploy AlumniChain ke production environment.

## 📋 Table of Contents
1. [Prerequisites](#prerequisites)
2. [SpacetimeDB Setup](#spacetimedb-setup)
3. [Vercel Deployment](#vercel-deployment)
4. [Environment Configuration](#environment-configuration)
5. [Post-Deployment](#post-deployment)
6. [Troubleshooting](#troubleshooting)

## Prerequisites

### Required Accounts
- ✅ [Vercel Account](https://vercel.com) (for frontend hosting)
- ✅ [SpacetimeDB Account](https://spacetimedb.com) (for database)
- ✅ [Base Wallet](https://base.org) (for blockchain testing)
- ✅ GitHub Account (for repository hosting)

### Local Setup
```bash
# Node.js 18+
node --version

# SpacetimeDB CLI
spacetime version

# Vercel CLI (optional but recommended)
npm i -g vercel
```

## 🗄️ SpacetimeDB Setup

### 1. Install SpacetimeDB CLI
```bash
curl --proto '=https' --tlsv1.2 -sSf https://install.spacetimedb.com | sh
```

### 2. Login to SpacetimeDB
```bash
spacetime login
```

### 3. Publish Server Module

#### Development
```bash
cd spacetime-server
spacetime publish --project-path . alumnichain-dev
```

#### Production
```bash
cd spacetime-server
spacetime publish --project-path . alumnichain-prod
```

### 4. Get Module URL
```bash
# List your published modules
spacetime list

# Note the module URL (e.g., wss://testnet.spacetimedb.com/alumnichain-prod)
```

### 5. Generate Client Bindings
```bash
# From project root
spacetime generate --lang typescript --out-dir ./src/spacetime_module_bindings alumnichain-prod
```

### 6. Initialize Database (Optional)
```bash
# You can pre-populate data using reducers
spacetime call alumnichain-prod register_alumni '{"name": "Admin", ...}'
```

## 🌐 Vercel Deployment

### Option 1: Deploy via Vercel CLI

```bash
# Login to Vercel
vercel login

# Deploy (from project root)
vercel

# Follow prompts:
# - Set up and deploy? Yes
# - Which scope? Your account
# - Link to existing project? No
# - Project name? alumnichain
# - Directory? ./
# - Override settings? No

# Deploy to production
vercel --prod
```

### Option 2: Deploy via GitHub Integration

1. **Push to GitHub**
```bash
git add .
git commit -m "Ready for deployment"
git push origin main
```

2. **Import in Vercel**
- Go to [Vercel Dashboard](https://vercel.com/dashboard)
- Click "Add New" → "Project"
- Import your GitHub repository
- Configure project settings:
  - Framework Preset: **Next.js**
  - Build Command: `npm run build`
  - Output Directory: `.next`
  - Install Command: `npm install`

3. **Deploy**
- Click "Deploy"
- Wait for build to complete (~2-3 minutes)

### Option 3: Deploy Button

Add to your GitHub README:
```markdown
[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/yourusername/alumnichain)
```

## ⚙️ Environment Configuration

### Vercel Environment Variables

Go to Vercel Dashboard → Project → Settings → Environment Variables

#### Required Variables
```env
# SpacetimeDB Configuration
NEXT_PUBLIC_SPACETIMEDB_URL=wss://spacetimedb.com
NEXT_PUBLIC_SPACETIMEDB_MODULE=alumnichain-prod

# Node Environment
NODE_ENV=production
```

#### Optional Variables
```env
# Analytics (if using PostHog)
NEXT_PUBLIC_POSTHOG_KEY=your_posthog_project_key
NEXT_PUBLIC_POSTHOG_HOST=https://app.posthog.com

# Custom Domain
NEXT_PUBLIC_BASE_URL=https://alumnichain.yourdomain.com

# Feature Flags
NEXT_PUBLIC_ENABLE_AI_FEATURES=true
NEXT_PUBLIC_ENABLE_NFT_MINTING=true
```

### Setting Variables in Vercel

#### Via Dashboard
1. Go to Project Settings → Environment Variables
2. Add each variable with appropriate scope:
   - **Production** - For production deployments
   - **Preview** - For branch deployments
   - **Development** - For local development

#### Via Vercel CLI
```bash
vercel env add NEXT_PUBLIC_SPACETIMEDB_URL production
# Enter value when prompted: wss://spacetimedb.com

vercel env add NEXT_PUBLIC_SPACETIMEDB_MODULE production
# Enter value: alumnichain-prod
```

## 🔧 Post-Deployment

### 1. Verify Deployment
```bash
# Check if site is live
curl -I https://your-project.vercel.app

# Should return 200 OK
```

### 2. Test Core Features
- ✅ Homepage loads
- ✅ Wallet connection works
- ✅ SpacetimeDB connection establishes
- ✅ Job board displays
- ✅ Profile registration works
- ✅ Real-time updates function

### 3. Monitor Performance

#### Vercel Analytics
- Go to Project → Analytics
- Check:
  - Page load times
  - Core Web Vitals
  - Traffic patterns

#### SpacetimeDB Metrics
```bash
# Check module status
spacetime status alumnichain-prod

# View logs
spacetime logs alumnichain-prod --tail
```

### 4. Setup Custom Domain (Optional)

#### In Vercel
1. Go to Project → Settings → Domains
2. Add domain: `alumni.yourdomain.com`
3. Configure DNS:

```dns
Type: CNAME
Name: alumni
Value: cname.vercel-dns.com
```

4. Wait for SSL certificate (automatic)

### 5. Configure Farcaster (if applicable)

Update `public/.well-known/farcaster.json`:
```json
{
  "accountAssociation": {
    "header": "...",
    "payload": "...",
    "signature": "..."
  },
  "frame": {
    "version": "1",
    "name": "AlumniChain",
    "iconUrl": "https://your-domain.com/icon.png",
    "homeUrl": "https://your-domain.com",
    "webhookUrl": "https://your-domain.com/api/webhook"
  }
}
```

## 🔍 Troubleshooting

### Build Failures

#### Issue: TypeScript Errors
```bash
# Run locally first
npm run build

# Fix any type errors before deploying
```

#### Issue: Missing Dependencies
```bash
# Ensure package.json is committed
git add package.json
git commit -m "Update dependencies"
git push
```

### SpacetimeDB Connection Issues

#### Issue: Connection Timeout
```typescript
// Check if URL is correct in src/lib/spacetime.tsx
const SPACETIMEDB_URL = process.env.NEXT_PUBLIC_SPACETIMEDB_URL || 'wss://spacetimedb.com';
const MODULE_NAME = process.env.NEXT_PUBLIC_SPACETIMEDB_MODULE || 'alumnichain-prod';
```

#### Issue: Module Not Found
```bash
# Verify module exists
spacetime list

# Re-publish if needed
cd spacetime-server
spacetime publish --project-path . alumnichain-prod
```

### Performance Issues

#### Large Bundle Size
```bash
# Analyze bundle
npm run build

# Check output for large chunks
# Consider code splitting for heavy components
```

#### Slow Page Loads
```typescript
// Use dynamic imports for heavy components
import dynamic from 'next/dynamic';

const HeavyComponent = dynamic(() => import('@/components/HeavyComponent'), {
  loading: () => <Skeleton />,
});
```

### Wallet Connection Issues

#### Issue: WalletConnect Not Working
- Ensure correct RPC URLs for Base network
- Check OnchainKit version compatibility
- Verify wallet provider is supported

## 🔐 Security Checklist

- [ ] Environment variables are set correctly
- [ ] Sensitive data not committed to git
- [ ] API routes have rate limiting
- [ ] CORS configured properly
- [ ] SpacetimeDB access controlled
- [ ] SSL certificate active (Vercel handles automatically)
- [ ] Security headers configured

### Security Headers (Optional)

Create `next.config.js`:
```javascript
module.exports = {
  async headers() {
    return [
      {
        source: '/(.*)',
        headers: [
          {
            key: 'X-Frame-Options',
            value: 'DENY',
          },
          {
            key: 'X-Content-Type-Options',
            value: 'nosniff',
          },
          {
            key: 'Referrer-Policy',
            value: 'origin-when-cross-origin',
          },
        ],
      },
    ];
  },
};
```

## 📊 Monitoring & Maintenance

### Daily Checks
- ✅ Site uptime (use UptimeRobot or similar)
- ✅ Error rates in Vercel logs
- ✅ SpacetimeDB connection status

### Weekly Maintenance
- 🔄 Check for dependency updates
- 🔄 Review performance metrics
- 🔄 Analyze user feedback
- 🔄 Database backup (SpacetimeDB handles this)

### Monthly Tasks
- 📈 Review analytics data
- 📈 Update documentation
- 📈 Security audit
- 📈 Cost analysis

## 🆘 Rollback Procedure

### Vercel Rollback
```bash
# Via Dashboard
# Go to Deployments → Select previous deployment → Promote to Production

# Via CLI
vercel rollback
```

### SpacetimeDB Rollback
```bash
# Re-publish previous version
cd spacetime-server
git checkout <previous-commit>
spacetime publish --project-path . alumnichain-prod
```

## 📞 Support Resources

- **Vercel Support:** https://vercel.com/support
- **SpacetimeDB Discord:** https://discord.gg/spacetimedb
- **Next.js Docs:** https://nextjs.org/docs
- **OnchainKit Docs:** https://onchainkit.xyz

## 🎉 Success!

Your AlumniChain platform is now live! 🚀

**Next Steps:**
1. Share the URL with beta testers
2. Monitor initial usage
3. Gather feedback
4. Iterate and improve

---

**Need Help?** Open an issue on GitHub or contact the development team.
