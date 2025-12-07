# AutoTrader Pro - Crypto Trading Platform

## Overview

AutoTrader Pro is a professional auto-trading platform designed for cryptocurrency trading with EMA (Exponential Moving Average) crossover strategies. The platform provides real-time market data monitoring, automated strategy execution, portfolio management, and backtesting capabilities. It supports both paper trading (simulation) and live trading modes, with integration capabilities for exchanges like Binance and Alpaca.

The application simulates a complete trading ecosystem with market data generation, position management, order execution, and comprehensive risk management tools. Users can create, configure, and monitor multiple trading strategies while tracking performance metrics and trade history.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Framework**: React with TypeScript, using Vite as the build tool and development server.

**UI Design System**: Implements shadcn/ui components (Radix UI primitives) with a custom design system inspired by Material Design and modern trading platforms (Robinhood's clarity + TradingView's data density). The design emphasizes professional data dashboard patterns prioritizing information hierarchy and rapid scanning for financial decisions.

**Styling**: Tailwind CSS with custom theme configuration supporting dark/light modes. Typography uses Inter/DM Sans for readability and JetBrains Mono for numerical data. Color scheme uses HSL-based CSS variables for themeable components.

**State Management**: React Query (@tanstack/react-query) for server state management with automatic refetching intervals for real-time data (positions/portfolio: 5s, market data: 2s, trades: 10s). No global client state management library is used; component-level state with hooks suffices.

**Routing**: Wouter for lightweight client-side routing with routes for Dashboard, Strategies, Positions, History, Backtesting, and Settings.

**Real-time Updates**: WebSocket connection to `/ws` endpoint for live market data, position updates, order status, and trading signals.

**Key Pages**:
- Dashboard: Overview with portfolio summary, active positions, recent orders, market charts
- Strategies: Create/manage trading strategies with EMA crossover parameters
- Positions: Monitor open positions with real-time P&L
- History: Trade history with performance analytics
- Backtest: Test strategies against historical data
- Settings: API key management and notification preferences

### Backend Architecture

**Framework**: Express.js with TypeScript running on Node.js.

**API Design**: RESTful endpoints for CRUD operations on strategies, positions, orders, trades, portfolio data, and API keys. WebSocket server for real-time bidirectional communication.

**Trading Engine**: Custom in-memory trading simulation engine (`TradingEngine` class) that:
- Generates simulated market data for 5 crypto pairs (BTC, ETH, BNB, SOL, XRP)
- Calculates EMA indicators (fast/slow periods)
- Generates trading signals based on EMA crossovers
- Executes simulated orders and manages positions
- Calculates P&L and portfolio metrics
- Broadcasts updates to connected WebSocket clients

**Data Storage**: Currently uses in-memory storage (`IStorage` interface) with mock implementation. The schema is defined with Drizzle ORM and prepared for PostgreSQL migration, but database integration is not yet active.

**Session Management**: Prepared for express-session with potential PostgreSQL session store (connect-pg-simple).

**Build Process**: Custom build script that bundles server code with esbuild (bundling select dependencies for faster cold starts) and client code with Vite, outputting to `dist/` directory.

### Data Architecture

**Database Schema** (Drizzle ORM with PostgreSQL dialect, defined but not actively used):
- Users table: Basic authentication (username/password)
- In-memory types for: Positions, Orders, Trades, Strategies, Portfolio, Market Data, API Keys, Signals, Backtest Results

**Data Models**:
- **Position**: Symbol, side (long/short), quantity, entry price, current price, unrealized P&L
- **Order**: Symbol, side (buy/sell), type (market/limit), quantity, price, status
- **Trade**: Completed trade with entry/exit prices, P&L, timestamps
- **StrategyConfig**: Name, broker, symbols, timeframe, EMA parameters (fast/slow periods), risk management rules
- **Portfolio**: Total balance, available cash, total P&L, number of positions
- **MarketData**: Real-time price, 24h change, volume, bid/ask
- **Signal**: Strategy-generated buy/sell signals with reasoning

**Risk Management**: Configurable per-strategy:
- Position size as percentage of portfolio
- Stop loss percentage
- Maximum daily loss limit
- Maximum concurrent positions

### Authentication & Authorization

Basic authentication structure is defined with user schema, but not currently implemented in routes. Prepared for passport.js local strategy.

### Code Organization

```
client/
  src/
    components/     # Reusable UI components
    pages/          # Route components
    lib/            # Utilities (queryClient, utils)
    hooks/          # Custom React hooks
server/
  index.ts          # Express server setup
  routes.ts         # API routes and WebSocket handler
  storage.ts        # Storage interface (in-memory)
  trading-engine.ts # Trading simulation engine
  static.ts         # Static file serving
  vite.ts           # Vite dev server integration
shared/
  schema.ts         # Shared types and Drizzle schema
```

## External Dependencies

### UI Component Libraries
- **@radix-ui/***: Comprehensive set of unstyled, accessible UI primitives (dialogs, dropdowns, menus, tooltips, etc.)
- **shadcn/ui**: Component pattern library built on Radix UI
- **class-variance-authority**: Type-safe variant styles
- **tailwindcss**: Utility-first CSS framework

### Data & State Management
- **@tanstack/react-query**: Server state management with caching and automatic refetching
- **drizzle-orm**: TypeScript ORM for PostgreSQL (schema defined, not yet connected)
- **drizzle-zod**: Zod schema generation from Drizzle schemas
- **zod**: Runtime type validation

### Backend Services
- **express**: Web server framework
- **ws**: WebSocket implementation for real-time updates
- **connect-pg-simple**: PostgreSQL session store (prepared but not active)
- **pg**: PostgreSQL client (prepared but not active)

### Charts & Visualization
- **recharts**: Chart library for market data visualization and backtest results

### Development Tools
- **vite**: Fast build tool and dev server with HMR
- **esbuild**: JavaScript bundler for server code
- **tsx**: TypeScript execution for development
- **@replit/vite-plugin-***: Replit-specific development plugins

### Utilities
- **date-fns**: Date formatting and manipulation
- **nanoid/uuid**: Unique ID generation
- **wouter**: Lightweight routing library

### Font Resources
- Google Fonts: DM Sans, Fira Code, JetBrains Mono, Geist Mono

### Future Integration Points
Platform is prepared for integration with:
- **Binance API**: Cryptocurrency exchange (API key management UI ready)
- **Alpaca API**: Stock/crypto trading (broker option available)
- **PostgreSQL Database**: Schema defined with Drizzle ORM, migration tools configured