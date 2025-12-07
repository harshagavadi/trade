# Auto-Trading Platform Design Guidelines

## Design Approach

**Hybrid Approach**: Material Design principles + Modern Trading Platform patterns (inspired by Robinhood's clarity + TradingView's data density)

**Core Philosophy**: Professional data dashboard prioritizing information hierarchy, rapid scanning, and safe interaction patterns for financial decisions.

## Typography System

**Font Stack**:
- Primary: Inter or DM Sans (Google Fonts) - excellent for data readability
- Monospace: JetBrains Mono - for numbers, prices, percentages

**Hierarchy**:
- Dashboard Title: text-2xl font-bold
- Section Headers: text-lg font-semibold
- Data Labels: text-sm font-medium uppercase tracking-wide
- Primary Numbers (P&L, Prices): text-3xl font-bold (monospace)
- Secondary Data: text-base (monospace for numbers)
- Supporting Text: text-sm
- Micro Labels: text-xs

## Layout System

**Spacing Primitives**: Use Tailwind units of 3, 4, 6, 8
- Component padding: p-4 or p-6
- Section gaps: gap-6 or gap-8
- Tight groups: gap-3
- Generous sections: p-8

**Grid Structure**:
- Main layout: Sidebar (w-64) + Content area (flex-1)
- Dashboard cards: Grid (grid-cols-1 lg:grid-cols-2 xl:grid-cols-3)
- Stat cards: Always display 3-4 key metrics in row (grid-cols-2 lg:grid-cols-4)

## Component Library

### Navigation
**Left Sidebar (Fixed, w-64)**:
- Platform logo/name at top (p-6)
- Main navigation links with icons (Dashboard, Strategies, Positions, History, Settings)
- Account selector dropdown
- Trading mode indicator badge (Paper/Live)
- Logout at bottom

### Dashboard Layout
**Top Bar (Sticky)**:
- Real-time clock/market status
- Quick stats bar (Total P&L, Today's P&L, Active Positions, Win Rate)
- Emergency stop button (prominent, top-right)

**Main Content Grid**:
1. **Strategy Control Panel** (full-width card at top):
   - Strategy selector dropdown
   - Start/Stop/Pause buttons (large, clear states)
   - Configuration quick-access (EMA periods, symbols, timeframe)
   - Real-time strategy status indicator

2. **Market Chart Area** (2/3 width, left):
   - TradingView-style candlestick chart
   - Symbol selector and timeframe controls above chart
   - Technical indicators overlay (EMA lines)
   - Volume bars below

3. **Real-time Positions** (1/3 width, right):
   - Scrollable card list
   - Each position shows: Symbol, Quantity, Entry Price, Current Price, P&L (with +/- indicator)

4. **Open Orders** (below positions):
   - Table format: Time, Symbol, Type, Quantity, Price, Status
   - Cancel action per order

5. **Trade History** (full-width, bottom):
   - Paginated table: Date, Symbol, Side (Buy/Sell), Quantity, Price, P&L
   - Filter controls above

6. **Performance Metrics** (3-card row):
   - Total Trades, Win Rate %, Average Profit
   - Small sparkline charts in each

### Data Display Components

**Stat Cards**:
- Elevated cards (shadow-md)
- Label at top (text-sm, muted)
- Large number (text-3xl, bold, monospace)
- Percentage change indicator with up/down icon
- Small trend sparkline (optional)

**Price/P&L Display**:
- Always use monospace font
- Show sign explicitly (+/- for P&L)
- Include percentage change in parentheses

**Tables**:
- Sticky headers
- Zebra striping (subtle)
- Hover states on rows
- Action buttons aligned right
- Compact spacing (py-3)

**Action Buttons**:
- Primary actions (Start Trading): Solid, large (px-8 py-3)
- Danger actions (Stop/Emergency): High contrast treatment
- Secondary actions: Outline style
- Icon + label for clarity

### Real-time Elements

**Status Indicators**:
- Dot indicator (h-2 w-2 rounded-full) + label
- Pulsing animation for active states
- Clear states: Active (green dot), Paused (yellow), Stopped (gray)

**Live Data Updates**:
- Subtle flash/highlight on number change
- No distracting animations
- Smooth transitions for chart updates

### Risk Management UI

**Controls Section**:
- Clearly labeled inputs with help text
- Position size slider with current/max display
- Stop-loss percentage input
- Max daily loss limit input
- Save/Apply changes button

**Warnings/Alerts**:
- Toast notifications (top-right)
- Inline warnings for risk thresholds
- Modal confirmations for critical actions (going live, stopping strategy)

## Icons

**Library**: Heroicons (via CDN)
- Navigation: outline style
- Actions: solid style when active
- Status indicators: custom dot + Heroicon for trends (arrow-up, arrow-down)

## Responsive Behavior

**Desktop (lg+)**: Full sidebar + multi-column grid
**Tablet (md)**: Collapsible sidebar + 2-column max
**Mobile**: Hidden sidebar (hamburger menu) + single column, chart remains scrollable

## Accessibility

- All interactive elements have proper focus states (ring-2)
- Clear labels for all form inputs
- ARIA labels for icon-only buttons
- Sufficient contrast for all text
- Keyboard navigation support throughout

## Images

No hero images needed for this application dashboard. All visual content is data-driven (charts, graphs, numbers). Use icon illustrations for empty states (e.g., "No active positions").