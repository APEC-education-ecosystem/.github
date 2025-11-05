# APEC OnChain Academy

A blockchain-based learning certificate management and issuance system built on Solana. This platform enables educational institutions to create, issue, and manage verifiable, secure, and tamper-proof digital certificates.

## 🎯 Overview

APEC OnChain Academy is a Web3 platform that combines Solana blockchain technology with traditional learning management systems. The platform serves three main user groups:

- **Educational Institutions (Providers)**: Create and manage courses, issue certificates
- **Students**: Receive and authenticate certificates as NFTs on the blockchain
- **Verifiers**: Validate certificate authenticity through blockchain verification

## ✨ Key Features

### 1. Provider Management (Educational Institutions)

- Register and manage organization information on-chain
- Each provider has a unique identifier (shortName) stored on the blockchain
- Supports detailed information: full name, country, address
- Immutable record of all provider activities

### 2. Course Management

- Create and manage courses under each provider
- Every course is recorded on-chain with:
  - Unique course ID (on-chain)
  - Course name and description
  - Issuing provider reference
  - Transaction hash for verification
  - Course metadata stored off-chain

### 3. NFT Certificate System

- **Certificate Issuance**: Create certificate proof (merkle tree) for each course
- **Certificate Claiming**: Students can claim certificates as NFTs
- **Verification**: Certificates are permanently stored on blockchain, tamper-proof
- **QR Code**: Generate and share certificates via QR code
- **Metadata**: Rich certificate data stored on Supabase

### 4. Authentication & Security

- Privy integration for user authentication
- Solana wallet connection support
- Merkle tree verification for certificate claiming rights
- Metadata storage on Supabase
- Server-side validation with Next.js Server Actions

## 🔄 Application Workflows

### Flow 1: Educational Institution Setup

```
1. Provider Registration
   ├─> User logs in via Privy
   ├─> Connect Solana wallet
   ├─> Create provider on-chain (Solana program)
   ├─> Save information to PostgreSQL
   └─> Receive unique provider ID

2. Course Creation
   ├─> Select existing provider
   ├─> Enter course information
   ├─> Submit transaction to Solana
   └─> Store course ID and metadata
```

### Flow 2: Certificate Issuance

```
1. Certificate Proof Preparation
   ├─> Admin creates list of eligible students
   ├─> Build Merkle tree from the list
   ├─> Upload metadata to Supabase
   └─> Create cert_proof account on-chain

2. Certificate NFT Minting
   ├─> Student accesses the system
   ├─> Authenticate via Privy
   ├─> Verify claiming rights (merkle proof)
   ├─> Execute claim transaction
   ├─> Mint NFT certificate to wallet
   └─> Store certificate information in DB
```

### Flow 3: Certificate Verification

```
1. Certificate Sharing
   ├─> Student generates QR code from NFT mint address
   └─> Share QR code or mint address

2. Verification
   ├─> Verifier scans QR code
   ├─> Query blockchain with mint address
   ├─> Check metadata and provider
   └─> Confirm authenticity
```

## 🏗️ System Architecture

### Technology Stack

**Frontend & Framework:**

- **Next.js 16** - React framework with App Router
- **TypeScript** - Type safety and enhanced DX
- **TailwindCSS v4** - Modern utility-first CSS
- **shadcn/ui** - Beautiful UI component library
- **React Query** - Powerful data fetching and caching

**Backend & API:**

- **tRPC** - End-to-end type-safe APIs
- **Drizzle ORM** - Type-safe database queries
- **PostgreSQL** - Reliable relational database
- **Supabase** - Storage and authentication backend

**Blockchain:**

- **Solana** - High-performance Layer 1 blockchain
- **Anchor** - Solana program development framework
- **@solana/kit** - Solana Web3 interactions
- **Metaplex** - NFT standard implementation

**Authentication:**

- **Privy** - Web3 authentication and wallet management
- **Wallet adapters** - Multi-wallet support

**Development Tools:**

- **Turborepo** - High-performance monorepo build system
- **Biome** - Fast linting and formatting
- **Bun** - Lightning-fast package manager and runtime

### Project Structure

```
apec-learning/
├── apps/
│   └── web/                    # Next.js Application
│       ├── src/
│       │   ├── app/           # App router pages
│       │   │   ├── (authenticated)/  # Protected routes
│       │   │   │   └── app/
│       │   │   │       ├── provider/       # Provider management
│       │   │   │       ├── course/         # Course management
│       │   │   │       ├── certificates/   # Certificate management
│       │   │   │       └── profile/        # User profile
│       │   │   ├── api/       # API routes
│       │   │   ├── login/     # Authentication
│       │   │   └── public/    # Public certificate view
│       │   ├── components/    # React components
│       │   ├── hooks/         # Custom React hooks
│       │   ├── lib/           # Utility functions
│       │   └── server/        # Server actions
│       └── package.json
│
├── packages/
│   ├── api/                   # tRPC API Layer
│   │   └── src/
│   │       └── routers/       # API route handlers
│   │
│   ├── db/                    # Database Layer
│   │   └── src/
│   │       ├── schema/        # Drizzle schemas
│   │       │   ├── provider.ts
│   │       │   ├── course.ts
│   │       │   └── certificate.ts
│   │       ├── provider/      # Provider queries
│   │       ├── course/        # Course queries
│   │       └── certificate/   # Certificate queries
│   │
│   └── program-sdk/           # Solana Program SDK
│       ├── idls/
│       │   └── apec-cert.json # Program IDL
│       └── src/
│           ├── accounts/      # Account type definitions
│           ├── instructions/  # Instruction builders
│           └── programs/      # Program clients
```

## 🚀 Getting Started

### Prerequisites

- **Bun** >= 1.3.1
- **Node.js** >= 20.x
- **PostgreSQL** >= 14.x
- **Solana CLI** (optional, for local development)

### Installation

1. **Clone the repository:**

```bash
git clone https://github.com/APEC-education-ecosystem/apec-certificate-web
cd apec-learning
```

2. **Install dependencies:**

```bash
bun install
```

3. **Configure Database:**

Create a `.env` file in `apps/web/`:

```env
# Database
DATABASE_URL="postgresql://user:password@localhost:5432/apec_learning"

# Supabase
NEXT_PUBLIC_SUPABASE_URL="your-supabase-url"
NEXT_PUBLIC_SUPABASE_ANON_KEY="your-supabase-key"
SUPABASE_SERVICE_ROLE_KEY="your-service-role-key"

# Privy Authentication
NEXT_PUBLIC_PRIVY_APP_ID="your-privy-app-id"
PRIVY_APP_SECRET="your-privy-secret"

# Solana Configuration
NEXT_PUBLIC_SOLANA_RPC_URL="https://api.devnet.solana.com"
NEXT_PUBLIC_SOLANA_NETWORK="devnet"
NEXT_PUBLIC_APEC_PROGRAM_ID="CAxe8JydEaRrtF3DVdPATw9XwAgYZUnCJ4wr5ZbvUFMp"
```

4. **Initialize database:**

```bash
bun db:push
```

5. **Start the development server:**

```bash
bun dev
```

6. **Access the application:**
   Open [http://localhost:3001](http://localhost:3001) in your browser

## 📝 Available Scripts

- `bun dev` - Start all applications in development mode
- `bun dev:web` - Run only the web application
- `bun build` - Build all applications for production
- `bun check-types` - Type-check all TypeScript files
- `bun db:push` - Push schema changes to database
- `bun db:studio` - Open Drizzle Studio for database management
- `bun db:generate` - Generate migrations from schema
- `bun db:migrate` - Run database migrations
- `bun check` - Run Biome linting and formatting

## 🗄️ Database Schema

### Provider (Educational Institution)

- `id`: Unique identifier (on-chain)
- `fullName`: Full organization name
- `shortName`: Abbreviated name (unique)
- `country`, `address`: Geographic information
- `creator`: Wallet address of creator
- `txHash`: On-chain transaction hash

### Course

- `id`: Unique identifier (on-chain)
- `name`: Course name
- `shortName`: Abbreviated name
- `providerId`: Reference to provider
- `description`: Course description
- `creator`: Wallet address of creator
- `txHash`: On-chain transaction hash

### Certificate

- `id`: Auto-increment ID
- `wallet`: Recipient wallet address
- `name`: Recipient name
- `nftMint`: NFT mint address (unique)
- `courseId`: Reference to course
- `creator`: Issuer wallet address
- `txHash`: Claim transaction hash

## 🔐 Security

- User authentication via Privy with multi-chain wallet support
- Merkle tree verification for certificate claiming rights
- On-chain validation for all critical operations
- Immutable certificate records on Solana blockchain
- Server-side validation with Next.js Server Actions
- Type-safe APIs with tRPC
- Environment variable validation with Zod

## 🎨 Features in Detail

### Certificate Claiming Process

1. Admin creates a merkle tree of eligible students
2. Merkle root is stored on-chain in cert_proof account
3. Students receive merkle proofs off-chain
4. Students submit claim transaction with their proof
5. Smart contract verifies the proof against the root
6. If valid, NFT certificate is minted to student's wallet

### QR Code Generation

- Each certificate NFT has a unique mint address
- QR codes encode the mint address for easy sharing
- Anyone can scan the QR code to verify certificate authenticity
- Verification queries the Solana blockchain directly

## 📚 Documentation & Resources

- [Solana Documentation](https://docs.solana.com/)
- [Anchor Framework](https://www.anchor-lang.com/)
- [Next.js Documentation](https://nextjs.org/docs)
- [Privy Documentation](https://docs.privy.io/)
- [tRPC Documentation](https://trpc.io/docs)
- [Drizzle ORM](https://orm.drizzle.team/)

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the [issues page](https://github.com/APEC-education-ecosystem/apec-certificate-web/issues).

### Development Workflow

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👨‍� Author

**Brolab Team**

- Project Link: [https://github.com/APEC-education-ecosystem/apec-certificate-web](https://github.com/APEC-education-ecosystem/apec-certificate-web)

## 🙏 Acknowledgments

- Built with [Better-T-Stack](https://github.com/AmanVarshney01/create-better-t-stack)
- Powered by Solana blockchain
- UI components from [shadcn/ui](https://ui.shadcn.com/)

---

<p align="center">Made with ❤️ for the future of verifiable credentials</p>
