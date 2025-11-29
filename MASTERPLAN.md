# VORDLE - Application Master Plan

## 1. Executive Summary

**Vordle** is a competitive 1v1 Wordle-style battle game played in real-time on the web. Players compete head-to-head to solve five-letter words, with live updates showing opponent progress. The game combines the familiar simplicity of Wordle with the excitement of competitive matchmaking, Elo-based ratings, and instant real-time gameplay.

**Core Value Proposition:** The fastest, most engaging real-time Wordle battle experience with fair competitive matches, completely free with no ads or purchases.

---

## 2. Project Objectives

- Create an engaging real-time Wordle battle experience with server-authoritative gameplay
- Provide fair competitive matches using Elo-based matchmaking
- Build a scalable foundation capable of handling 500-5,000 concurrent players
- Maintain a clean, familiar Wordle-inspired UI with competitive elements
- Keep the product completely free with no monetization
- Ensure long-term competitive integrity through accurate Elo ratings and match history
- Launch V1 within 3 months as a solo developer project

---

## 3. Target Audience

- **Casual Wordle players** seeking a competitive twist on the classic game
- **Competitive gamers** who enjoy chess.com-style matchmaking and ratings
- **Friends** who want quick, shareable battles
- **Puzzle enthusiasts** who enjoy timed word challenges
- **Leaderboard climbers** motivated by rankings and improvement

---

## 4. Platform & Accessibility

### 4.1 Platform Strategy

**V1 (Launch):**
- Fully web-based application
- Responsive design optimized for both desktop and mobile browsers
- No native apps required

**Future Considerations:**
- Progressive Web App (PWA) for offline practice mode
- Potential native mobile apps (iOS/Android) based on user demand

### 4.2 Device Support

**Desktop:**
- Keyboard input for guesses
- Optimized layout for larger screens
- Support for all modern browsers (Chrome, Firefox, Safari, Edge)

**Mobile:**
- Native mobile keyboard input
- Touch-optimized UI
- Responsive grid layout
- Same feature set as desktop

---

## 5. Core Features

### 5.1 Gameplay Mechanics

**Classic Wordle Logic:**
- Five-letter word puzzles
- Color-coded feedback system:
  - 🟩 Green: Correct letter in correct position
  - 🟨 Yellow: Correct letter in wrong position
  - ⬜ Gray: Letter not in word
- Proper handling of repeated letters
- Same target word for both players in each match

**Real-Time Competition:**
- Live opponent progress display (colors only, no letters shown)
- High-frequency updates (100-200ms sync rate)
- Server authority for all validation and evaluation
- Synchronized match state across both players

**Server Authority (Critical for Fair Play):**
- Target word selection on server
- Guess validation on server
- Tile pattern generation on server
- Rule enforcement for repeated letters
- Match end detection
- Timeout handling
- Elo rating calculations

### 5.2 Game Modes

**V1 - Unlimited Mode (Launch Priority):**
- No time limit
- Fewest attempts to solve wins
- Maximum 6 attempts per player
- Tie-breaking: if both use same attempts, fastest time wins

**V2 - Timed Mode (Future):**
- 10-minute countdown timer
- First player to solve correctly wins
- Timeout results in rating loss
- High-pressure, fast-paced gameplay

**V3 - Limited Mode (Future):**
- Exactly 6 attempts maximum
- Fewest attempts wins
- Tie-break on time if attempts are equal
- Balanced mode combining strategy and speed

### 5.3 Matchmaking System

**Auto-Match (Ranked):**
- Elo-based matchmaking algorithm
- Initial search window: ±50 Elo points
- Window expansion: +50 Elo every 10 seconds
- Maximum wait time: 2 minutes
- If no match found: "No matches found, try again later"
- Ranked matches affect player ratings

**Friend Challenge:**
- Generate shareable private URL
- Challenger selects game mode
- URL expires after first use OR 60 minutes (whichever comes first)
- Option for ranked or unranked matches
- Ranked friend matches affect Elo ratings

**Rematch System:**
- Post-match rematch option
- Requires acceptance from both players
- Maintains same mode
- Generates new target word

### 5.4 Authentication & User Management

**Login System:**
- Google OAuth authentication only
- No anonymous/guest play
- JWT stored in httpOnly cookie for security
- WebSocket authentication using JWT validation

**User Profile:**
- Google-provided username and avatar
- Per-mode Elo rating display
- Match history access
- Statistics dashboard

### 5.5 Rating System

**Elo Implementation:**
- Separate Elo rating for each game mode
- Standard Elo calculation (K-factor based on rating stability)
- Rating updates applied immediately after match
- Ratings stored with match results for historical tracking

**Rating Integrity:**
- Server-side calculation only
- Penalties for timeouts/disconnections
- Protected against client manipulation
- Future consideration: new player calibration period

### 5.6 Leaderboards

**Global Leaderboards:**
- Separate leaderboard per game mode
- Top 100 players displayed
- Real-time ranking updates
- Sortable by rating
- Display: Rank, Username, Rating, Wins, Losses

### 5.7 Match Results & History

**Post-Match Summary Screen:**
- Winner/Loser declaration
- Elo rating change (+/-XX points)
- Attempts comparison
- Time taken by each player
- Rematch request option

**Match History (V1.1+):**
- Complete game archive per user
- For each match:
  - Opponent name
  - Win/Loss result
  - Mode played
  - Rating change
  - Full guess history
  - Tile patterns
  - Attempts used
  - Time taken
  - Timestamps
  - Outcome type

---

## 6. User Interface & Experience

### 6.1 Visual Design Philosophy

**Hybrid Design Approach:**
- Clean, Wordle-inspired grid for primary gameplay
- Dark competitive HUD overlay for match information
- Minimalist, distraction-free interface
- Subtle animations for polish without distraction

**Color Scheme:**
- Familiar Wordle green/yellow/gray for tiles
- Dark mode aesthetic for competitive feel
- High contrast for readability
- Accessibility-friendly color choices

### 6.2 Screen Layouts

**Home Screen:**
- Mode selector (V1: Unlimited only)
- Current Elo rating display
- "Find Match" button (auto-matchmaking)
- "Challenge Friend" button (generates shareable link)
- "Leaderboard" access
- "Match History" access (V1.1+)
- Simple, uncluttered navigation

**Match Screen:**
- **Player's Grid:** 6-row Wordle board with letter input
- **Opponent's Grid:** Color-only display (no letters visible)
- **Player Timer:** Elapsed time display
- **Opponent Timer:** Opponent's elapsed time
- **Attempts Counter:** Current attempts used (X/6)
- **Mode Indicator:** Current game mode badge
- **Rating Badges:** Both players' Elo ratings
- **Match Status:** Real-time status updates

**Post-Match Screen:**
- Match result (Win/Loss/Draw)
- Rating change visualization
- Performance comparison table
- Rematch button
- Return to home button

### 6.3 Animations & Feedback

**Subtle Animations:**
- Tile flip animations on guess submission
- Color reveal animations (like Wordle)
- Real-time opponent progress updates
- Victory/defeat animations
- Rating change number animations

**User Feedback:**
- Invalid word shake animation
- Keyboard highlighting for used letters
- Connection status indicator
- Match start countdown
- Opponent action notifications (subtle)

---

## 7. Technical Architecture

### 7.1 High-Level Architecture

**Monolithic Backend Structure:**
The backend is a single Node.js application containing:
- WebSocket server for real-time communication
- RESTful API for authentication, leaderboards, and match history
- Matchmaking engine
- Game logic and validation
- Rating calculation system
- Data persistence layer
- Analytics tracking

**Why Monolithic for V1:**
- Simpler to develop and deploy as solo developer
- Easier debugging and testing
- Lower operational complexity
- Sufficient for target scale (500-5,000 concurrent players)
- Can migrate to microservices later if needed

### 7.2 Technology Stack

**Backend:**
- **Runtime:** Node.js (v18+)
- **Framework:** Express.js
- **WebSocket:** Socket.IO (auto-reconnection, room support, event-based)
- **Language:** JavaScript/TypeScript (recommend TypeScript for better type safety)

**Database:**
- **Primary Database:** PostgreSQL
  - User accounts and profiles
  - Match results and history
  - Elo ratings per mode
  - Leaderboard data
  - Analytics data

**Caching & Session Management (V2+):**
- **Redis:** (Add when scaling to multiple servers)
  - Matchmaking queues
  - Active match state (when multi-server)
  - Cross-server pub/sub coordination
  - Rate limiting

**Frontend:**
- **Framework:** React
- **State Management:** React Context API or Zustand (lightweight)
- **Styling:** Tailwind CSS or CSS Modules
- **WebSocket Client:** Socket.IO client library
- **Build Tool:** Vite (fast, modern development)

**Authentication:**
- Google OAuth 2.0 for user authentication
- JWT (JSON Web Tokens) stored in httpOnly cookies
- Token-based WebSocket authentication

**Deployment (When Ready):**
- **Hosting Platform:** Railway.app or Render.com (simple, affordable, great for MVP)
- **Database Hosting:** Railway/Render managed PostgreSQL
- **Domain:** TBD (purchase when ready for beta testing)
- **SSL:** Automatic via hosting platform

### 7.3 Real-Time Communication Layer

**WebSocket Architecture (V1 - Single Server):**
- Single Socket.IO server instance
- Active match state stored in server memory
- Fast, low-latency communication
- Simple connection management

**Event-Based Communication:**
```
Client → Server Events:
- authenticate (JWT validation)
- join_queue (start matchmaking)
- leave_queue (cancel matchmaking)
- submit_guess (player guess submission)
- request_rematch (post-match)
- accept_rematch (post-match)
- disconnect (connection lost)

Server → Client Events:
- authenticated (connection successful)
- match_found (matchmaking success, opponent info)
- match_start (game begins, timer starts)
- opponent_guess (opponent submitted guess, color update)
- guess_result (server validation of player's guess)
- match_end (winner declared, ratings updated)
- opponent_disconnected (grace period started)
- opponent_reconnected (opponent back online)
- rematch_requested (opponent wants rematch)
- error (validation errors, connection issues)
```

**Update Frequency:**
- High-frequency updates: 100-200ms for critical events
- Debounced updates for non-critical UI elements
- Efficient payload sizes (JSON, minimal data)

**Scaling Strategy (V2 - Multi-Server):**
When concurrent players exceed single-server capacity:
- Deploy multiple Socket.IO server instances
- Add Redis for cross-server pub/sub
- Implement sticky sessions or consistent hashing
- Use Redis to store active match state
- Coordinate match events across servers via Redis pub/sub

### 7.4 Matchmaking Engine

**Queue Management:**
- In-memory queue for V1 (single server)
- Players grouped by mode
- FIFO queue with Elo-based pairing

**Matching Algorithm:**
1. Player joins queue with current Elo rating
2. Initial search window: ±50 Elo points
3. Search for opponent within window
4. If no match: expand window by +50 every 10 seconds
5. Maximum wait: 2 minutes
6. If no match found: notify player, remove from queue

**Friend Challenge:**
- Generate unique challenge token (UUID)
- Store challenge in memory/database with:
  - Creator user ID
  - Mode selected
  - Ranked/unranked flag
  - Expiry timestamp (60 minutes)
  - Used flag (expires on first use)
- Validate token on challenge acceptance
- Create match directly (bypass queue)

### 7.5 Server Authority & Game Logic

**Critical: All Game Logic on Server**

**Server Responsibilities:**
- Select random target word from curated 2,300-word list (same as original Wordle)
- Validate all guess submissions:
  - Check if word exists in valid word dictionary (~12,000 words)
  - Check for correct letter count
  - Enforce attempt limits
- Generate tile patterns (green/yellow/gray) for each guess
- Handle repeated letter logic correctly
- Track match state (attempts, time, guesses)
- Detect match end conditions:
  - Player solved correctly
  - Maximum attempts reached
  - Timeout (in timed mode, V2)
  - Opponent disconnected beyond grace period
- Calculate Elo rating changes
- Persist match results to database

**Client Responsibilities:**
- Render UI based on server messages
- Capture user input (keyboard/touch)
- Send guess submissions to server
- Display opponent's color-only board
- Show timers and attempt counters
- Handle animations and transitions
- **Client NEVER validates guesses or knows the target word**

### 7.6 Disconnection & Reconnection Handling

**30-Second Grace Period:**
1. Player loses connection (detected by Socket.IO)
2. Server pauses match timer (if applicable)
3. Notify opponent: "Opponent disconnected, waiting..."
4. Start 30-second countdown
5. If player reconnects within 30 seconds:
   - Resume match from exact state
   - Restore timer, attempts, board state
   - Notify opponent: "Opponent reconnected"
6. If 30 seconds expire:
   - Declare disconnected player as loser
   - Award win to connected player
   - Apply Elo rating changes (loss for disconnected player)
   - Save match result

**Socket.IO Auto-Reconnection:**
- Automatic reconnection attempts on network interruption
- Client maintains match context during reconnection
- Server validates reconnecting user's identity (JWT)
- Seamless state restoration on successful reconnection

### 7.7 Word Lists & Dictionary

**Answer Word List:**
- Use the official Wordle answer list (~2,300 five-letter words)
- Same word pool for all game modes
- Random selection per match (server-side)
- No word repetition tracking in V1 (acceptable given large pool)

**Valid Guess Dictionary:**
- Use the official Wordle valid guess dictionary (~12,000+ five-letter words)
- Server validates all guesses against this dictionary
- Includes answer words + additional valid English words
- Reject invalid/non-existent words with error message

**Word List Management:**
- Store word lists as JSON/text files in backend
- Load into memory on server startup for fast lookups
- No profanity filtering in V1 (Wordle's official list is already clean)
- Future: Could add difficulty tiers or themed word lists

---

## 8. Data Model

### 8.1 Database Schema (PostgreSQL)

**Users Table:**
```
users
- id (UUID, primary key)
- google_id (string, unique, indexed)
- email (string, unique)
- username (string, from Google profile)
- avatar_url (string, from Google profile)
- created_at (timestamp)
- last_login (timestamp)
```

**Ratings Table:**
```
ratings
- id (UUID, primary key)
- user_id (UUID, foreign key → users.id)
- mode (enum: 'unlimited', 'timed', 'limited')
- rating (integer, default: 1200)
- wins (integer, default: 0)
- losses (integer, default: 0)
- draws (integer, default: 0)
- updated_at (timestamp)
- UNIQUE(user_id, mode)
```

**Matches Table:**
```
matches
- id (UUID, primary key)
- mode (enum: 'unlimited', 'timed', 'limited')
- player1_id (UUID, foreign key → users.id)
- player2_id (UUID, foreign key → users.id)
- winner_id (UUID, foreign key → users.id, nullable for draws)
- target_word (string, 5 characters)
- player1_attempts (integer)
- player2_attempts (integer)
- player1_time_ms (bigint, milliseconds)
- player2_time_ms (bigint, milliseconds)
- rating_change_p1 (integer, can be negative)
- rating_change_p2 (integer, can be negative)
- outcome_type (enum: 'win', 'loss', 'draw', 'timeout', 'disconnect')
- started_at (timestamp)
- ended_at (timestamp)
- created_at (timestamp)
```

**Match Guesses Table (for history):**
```
match_guesses
- id (UUID, primary key)
- match_id (UUID, foreign key → matches.id)
- player_id (UUID, foreign key → users.id)
- attempt_number (integer, 1-6)
- guess_word (string, 5 characters)
- tile_pattern (string, e.g., "GYGGG" for green/yellow/gray)
- timestamp (timestamp, relative to match start)
- created_at (timestamp)
```

**Challenge Links Table:**
```
challenge_links
- id (UUID, primary key)
- token (string, unique, indexed)
- creator_id (UUID, foreign key → users.id)
- mode (enum: 'unlimited', 'timed', 'limited')
- ranked (boolean)
- used (boolean, default: false)
- expires_at (timestamp)
- created_at (timestamp)
```

### 8.2 Indexes for Performance

**Critical Indexes:**
- `users.google_id` (unique index for fast OAuth lookup)
- `ratings.user_id, ratings.mode` (composite unique index)
- `matches.player1_id, matches.player2_id` (for user match history)
- `matches.ended_at` (for recent matches queries)
- `challenge_links.token` (for fast challenge validation)
- `leaderboard query`: Index on `ratings.mode, ratings.rating DESC`

### 8.3 In-Memory State (V1)

**Active Match State (Server Memory):**
```javascript
{
  matchId: "uuid",
  mode: "unlimited",
  player1: { userId, socketId, rating },
  player2: { userId, socketId, rating },
  targetWord: "crane",
  player1State: {
    guesses: ["arose", "alert"],
    patterns: ["YGGGG", "GYGGG"],
    attempts: 2,
    startTime: timestamp,
    solved: false
  },
  player2State: { ... },
  status: "active" | "paused" | "ended"
}
```

This in-memory state allows instant access during gameplay without database queries.

---

## 9. Security Considerations

### 9.1 Authentication & Authorization

- Google OAuth 2.0 for trusted identity verification
- JWT stored in httpOnly cookies to prevent XSS attacks
- Short JWT expiry (1 hour) with refresh token mechanism
- WebSocket connections require valid JWT before accepting events
- No sensitive data in JWT payload (only user ID)

### 9.2 Server Authority

- **Target word NEVER sent to client** until match ends
- All guess validation on server
- All Elo calculations on server
- Client cannot manipulate match state
- Client cannot see opponent's letters, only colors

### 9.3 Anti-Cheating Measures

- Rate limiting on guess submissions (prevent spam)
- Server-side timing validation (prevent timer manipulation)
- WebSocket authentication on every connection
- Validate all client events on server
- Log suspicious activity (multiple rapid guesses, timing anomalies)

### 9.4 Data Protection

- All connections over HTTPS/WSS (encrypted in transit)
- Environment variables for sensitive credentials
- No passwords stored (OAuth only)
- Database connection pooling with encrypted connections
- Regular security updates for dependencies

### 9.5 Rate Limiting

- Guess submission: max 1 per second per user
- Matchmaking requests: max 1 per 5 seconds
- API endpoints: standard rate limiting (100 requests/minute per IP)
- Challenge link generation: max 5 per hour per user

### 9.6 Timeout & Abuse Prevention

- 30-second disconnect grace period (prevents abuse of "pulling plug")
- Elo penalty for excessive disconnections (future)
- Report system for unsportsmanlike conduct (future)
- IP-based rate limiting for API abuse

---

## 10. Development Phases

### Phase 1: Foundation & Core Gameplay (Weeks 1-3)

**Goal:** Build the fundamental Wordle game logic and basic UI

**Backend:**
- Set up Node.js + Express project structure
- Implement word validation engine
- Create target word selection logic
- Build tile pattern generation (green/yellow/gray logic)
- Handle repeated letter edge cases correctly
- Test game logic thoroughly with unit tests

**Frontend:**
- Set up React project with Vite
- Build basic Wordle grid component (6 rows, 5 columns)
- Implement keyboard input handling
- Create tile flip animations
- Build simple local version (no multiplayer yet)

**Database:**
- Set up PostgreSQL locally
- Create initial schema (users, ratings, matches)
- Set up database migrations

**Deliverable:** Working single-player Wordle game with server validation

---

### Phase 2: Real-Time Infrastructure (Weeks 4-5)

**Goal:** Implement WebSocket communication and basic multiplayer

**Backend:**
- Integrate Socket.IO into Express server
- Implement WebSocket event handlers
- Build match state management (in-memory)
- Create real-time guess synchronization
- Implement opponent board updates (colors only)
- Add timer tracking

**Frontend:**
- Integrate Socket.IO client
- Build opponent board display component
- Implement real-time event listeners
- Add timer displays for both players
- Create connection status indicator

**Testing:**
- Test with 2 local browser tabs (1 incognito, 1 signed in)
- Verify real-time synchronization
- Test disconnection/reconnection scenarios

**Deliverable:** Two players can play against each other in real-time locally

---

### Phase 3: Authentication & User Management (Week 6)

**Goal:** Implement Google login and user profiles

**Backend:**
- Set up Google OAuth 2.0 integration
- Implement JWT generation and validation
- Create authentication middleware
- Build user profile endpoints
- Add WebSocket authentication

**Frontend:**
- Create login page with Google sign-in button
- Build user profile display
- Implement JWT storage (httpOnly cookie)
- Add logout functionality
- Create protected routes

**Database:**
- Seed initial user data for testing
- Create rating records for new users (default 1200 Elo)

**Deliverable:** Users can sign in with Google and have persistent profiles

---

### Phase 4: Matchmaking System (Week 7)

**Goal:** Build automatic matchmaking and friend challenges

**Backend:**
- Implement in-memory matchmaking queue
- Build Elo-based pairing algorithm
- Create expanding search window logic (±50 Elo, +50 every 10s)
- Add 2-minute timeout with "no matches" response
- Implement friend challenge link generation
- Build challenge validation and expiry logic

**Frontend:**
- Create home screen with "Find Match" button
- Build matchmaking queue UI (searching... status)
- Add "Challenge Friend" button
- Display shareable challenge link
- Show queue position/waiting time

**Database:**
- Add challenge_links table
- Create indexes for matchmaking queries

**Testing:**
- Test matchmaking with multiple test accounts
- Verify Elo-based pairing works correctly
- Test friend challenge flow end-to-end

**Deliverable:** Players can find matches automatically or challenge friends

---

### Phase 5: Elo Ratings & Leaderboard (Week 8)

**Goal:** Implement competitive rating system and rankings

**Backend:**
- Implement Elo calculation algorithm
- Update ratings after each match
- Create leaderboard query (top 100 per mode)
- Build leaderboard API endpoint
- Store rating changes with match results

**Frontend:**
- Display current Elo rating on home screen
- Show rating change after match (+/- points)
- Build leaderboard page
- Create rank, username, rating display
- Add leaderboard refresh

**Database:**
- Optimize leaderboard queries with indexes
- Ensure atomic rating updates (transactions)

**Testing:**
- Verify Elo calculations are correct
- Test rating updates for wins/losses/draws
- Ensure leaderboard updates in real-time

**Deliverable:** Fully functional competitive rating system with leaderboard

---

### Phase 6: Post-Match & Polish (Week 9)

**Goal:** Complete match flow and improve user experience

**Backend:**
- Implement rematch system
- Store complete match results to database
- Add match end event handling
- Build disconnection penalty logic

**Frontend:**
- Create post-match summary screen
- Display winner/loser, rating change, stats
- Add rematch request UI
- Build rematch acceptance flow
- Polish animations and transitions
- Improve error handling and user feedback

**UI/UX:**
- Refine visual design
- Add sound effects (optional)
- Improve mobile responsiveness
- Add loading states
- Create better error messages

**Deliverable:** Polished end-to-end match experience with rematch capability

---

### Phase 7: Testing & Bug Fixes (Week 10)

**Goal:** Comprehensive testing and stability improvements

**Tasks:**
- Extensive local testing with multiple scenarios
- Test edge cases (disconnections, simultaneous solves, etc.)
- Performance testing (simulate concurrent matches)
- Mobile browser testing (iOS Safari, Android Chrome)
- Fix identified bugs
- Optimize database queries
- Reduce payload sizes
- Add error logging and monitoring

**Deliverable:** Stable, well-tested application ready for beta

---

### Phase 8: Deployment & Beta Testing (Weeks 11-12)

**Goal:** Deploy to production and gather feedback

**Deployment:**
- Set up Railway.app or Render.com account
- Configure PostgreSQL database (managed instance)
- Dockerize application (optional, but recommended)
- Deploy backend + frontend
- Configure environment variables
- Set up SSL certificates (automatic on Railway/Render)
- Purchase domain name (optional for beta)

**Beta Testing:**
- Invite 10-20 friends to test
- Collect feedback via Google Form or Discord
- Monitor server performance and errors
- Track match completion rates
- Identify and fix critical issues
- Iterate on UX based on feedback

**Analytics:**
- Add basic analytics tracking:
  - Daily active users
  - Matches played per day
  - Average match duration
  - Match completion vs disconnection rate
  - Most active time periods

**Deliverable:** Live beta version with real users and feedback loop

---

### Phase 9: Launch Preparation (Week 12)

**Goal:** Final polish and public launch

**Tasks:**
- Implement final feedback from beta testers
- Create basic admin dashboard for monitoring
- Write simple user guide/FAQ
- Prepare social media presence (optional)
- Final performance optimizations
- Set up basic error monitoring (e.g., Sentry free tier)
- Create launch announcement

**Marketing (Optional):**
- Post on Reddit (r/wordle, r/websitereview)
- Share on Twitter/X
- Post on Hacker News Show HN
- Share in relevant Discord communities

**Deliverable:** Public V1 launch of Vordle!

---

### Post-Launch: V1.1+ (Month 4+)

**Match History Feature:**
- Add match history page
- Display past matches with details
- Allow filtering by mode, date, opponent
- Show guess-by-guess replay (optional)

**Analytics Dashboard:**
- Build simple admin panel
- Display key metrics (DAU, matches, completion rate)
- Track performance over time
- Monitor server health

**V2 - Timed Mode:**
- Implement 10-minute countdown timer
- Add timer UI and server logic
- Test thoroughly for timing edge cases
- Launch as second mode

**V3 - Limited Mode:**
- Implement 6-attempt limited mode
- Add tie-breaking logic
- Launch as third mode

---

## 11. Potential Challenges & Solutions

### 11.1 Real-Time Scalability

**Challenge:** As concurrent players grow, single server may struggle with WebSocket connections.

**Solution:**
- **V1:** Single server can handle 500-2,000 concurrent connections easily
- **V2:** When scaling needed:
  - Deploy multiple Socket.IO server instances behind load balancer
  - Add Redis for cross-server pub/sub and match state
  - Implement sticky sessions (users stay on same server)
  - Use HAProxy or Nginx for WebSocket load balancing
- **Monitoring:** Track connection counts and server CPU/memory

### 11.2 Cheating & Client Manipulation

**Challenge:** Players might try to cheat by inspecting network traffic or manipulating client code.

**Solution:**
- **Server Authority:** All validation happens on server, client is "dumb"
- **Target word never sent to client** until match ends
- **Encrypted WebSocket** connections (WSS)
- **Rate limiting** on guess submissions
- **Anomaly detection:** Log suspicious patterns (impossible solve times, etc.)
- **Community reporting** system (future)

### 11.3 Rating System Integrity

**Challenge:** Rating inflation, smurfing (new accounts to farm low-rated players), or exploitation.

**Solution:**
- **Elo K-factor tuning** to reduce volatility
- **New player calibration:** Higher K-factor for first 20 matches
- **Rating floors:** Prevent intentional rating tanking
- **Detect smurf patterns:** Multiple new accounts from same IP
- **Friend match abuse:** Limit ranked friend matches per day (future)
- **Regular rating resets** (seasonal, optional)

### 11.4 Matchmaking Wait Times

**Challenge:** Low player population during off-peak hours leads to long wait times or no matches.

**Solution:**
- **Expanding Elo window:** Already built in (±50 → +50 every 10s)
- **Play vs AI** during low-traffic times (future)
- **Incentivize peak times:** Daily challenges, events (future)
- **Show active player count** to set expectations
- **Cross-mode matchmaking** if populations are very low (future)

### 11.5 Word Familiarity Imbalance

**Challenge:** Some players may be more familiar with certain words, creating unfair advantage.

**Solution:**
- **Curated word list:** Use Wordle's balanced 2,300-word answer set
- **Random selection:** Ensures variety, no patterns
- **Both players get same word:** Fair comparison of skill
- **No word repetition tracking needed** due to large pool (1 in 2,300 chance)
- **Daily challenges** with same word for all players (future feature)

### 11.6 Network Latency & Disconnections

**Challenge:** Players on slow connections or unstable networks may disconnect frequently.

**Solution:**
- **30-second grace period:** Already planned, fair for brief outages
- **Socket.IO auto-reconnection:** Handles most transient issues
- **Pause timer during disconnect:** Don't penalize for network issues
- **Connection quality indicator:** Warn players of poor connection
- **Disconnect tracking:** Identify chronic disconnectors, potential penalties (future)

### 11.7 Mobile Experience

**Challenge:** Mobile screens are smaller, touch input is different, keyboards vary.

**Solution:**
- **Responsive grid layout:** Optimized for small screens
- **Native mobile keyboard:** Uses device's built-in keyboard
- **Large touch targets:** Buttons and tiles easy to tap
- **Landscape mode support:** For better grid visibility
- **Test on real devices:** iOS Safari, Android Chrome

### 11.8 Sudden Traffic Spikes

**Challenge:** Viral growth or social media mention could cause sudden surge in players.

**Solution:**
- **Cloud hosting:** Railway/Render can auto-scale (within limits)
- **Horizontal scaling plan:** Pre-configured multi-server setup ready to deploy
- **Database connection pooling:** Prevent database overload
- **Rate limiting:** Protect against abuse during spikes
- **CDN for static assets:** Offload frontend serving (Vercel, Netlify)
- **Monitoring alerts:** Get notified of unusual traffic

### 11.9 Development Timeline Risk (Solo Developer)

**Challenge:** 3-month timeline is ambitious for solo developer with full feature set.

**Solution:**
- **Phased approach:** V1 focuses on Unlimited mode only
- **Cut scope aggressively:** Delay nice-to-have features (match history → V1.1)
- **Time buffers:** Each phase has buffer for unexpected issues
- **MVP mindset:** Launch with core features, iterate based on feedback
- **Use existing tools:** Don't reinvent wheel (Socket.IO, Tailwind, etc.)
- **Set realistic expectations:** 3 months for MVP, not perfect product

---

## 12. Future Expansion Possibilities

### 12.1 Additional Game Modes (V2+)

- **Timed Mode (V2):** 10-minute countdown, first to solve wins
- **Limited Mode (V3):** 6 attempts maximum, fewest wins
- **Daily Challenge:** Same word for all players, leaderboard for that day
- **Blitz Mode:** 3 minutes, ultra-fast-paced
- **Hard Mode:** Requires using revealed hints (like Wordle hard mode)

### 12.2 Social Features

- **Friends List:** Add friends, see their online status
- **Private Lobbies:** Create custom games with specific rules
- **Spectator Mode:** Watch high-rated matches live
- **Chat System:** Pre-match or post-match chat (text only, moderated)
- **Achievements/Badges:** Reward milestones (100 wins, 5-win streak, etc.)

### 12.3 Competitive Features

- **Tournaments:** Bracket-style competitions
- **Seasonal Leagues:** Monthly or quarterly competitive seasons
- **Team Battles:** 2v2 or team-based Vordle
- **Clan System:** Form teams, compete for clan rankings
- **Replays:** Watch past matches, learn from top players

### 12.4 Customization & Cosmetics

- **Themes:** Different color schemes (maintain accessibility)
- **Tile Styles:** Custom tile designs (unlockable)
- **Emotes:** Express reactions during/after matches
- **Profile Customization:** Banners, titles, profile icons
- **Note:** All cosmetic only, never pay-to-win

### 12.5 Platform Expansion

- **Progressive Web App (PWA):** Offline practice mode, install on mobile
- **Native Mobile Apps:** iOS and Android apps for better performance
- **Desktop App:** Electron-based desktop client (optional)
- **Browser Extension:** Quick-access Vordle (low priority)

### 12.6 Analytics & Insights

- **Personal Statistics:** Win rate, average attempts, fastest solve, etc.
- **Performance Graphs:** Rating over time, improvement tracking
- **Word Statistics:** Success rate by starting word, letter frequency analysis
- **Comparison Tool:** Compare stats with friends or top players

### 12.7 Educational Features

- **Practice Mode:** Play solo against AI or past puzzles
- **Hints System:** Optional hints for learning (unranked)
- **Strategy Guides:** Built-in tips for improving Wordle strategy
- **Word Lists:** Show valid words, learn new vocabulary

### 12.8 Accessibility

- **Colorblind Modes:** Alternative color schemes
- **Screen Reader Support:** ARIA labels, keyboard navigation
- **Adjustable Font Sizes:** Better readability
- **Reduced Motion:** Option to disable animations

### 12.9 Moderation & Community

- **Report System:** Report abusive usernames, behavior
- **Moderation Dashboard:** Admin tools for managing users
- **Banned Word Filter:** Prevent offensive usernames
- **Fair Play Enforcement:** Detect and penalize cheaters

---

## 13. Success Metrics & Goals

### 13.1 Launch Metrics (First Month)

- **User Acquisition:**
  - Target: 100-500 registered users
  - Measure: Daily/weekly signups
  
- **Engagement:**
  - Target: 50-200 daily active users
  - Target: Average 3-5 matches per user per session
  - Measure: DAU, matches per user
  
- **Match Quality:**
  - Target: >85% match completion rate (not abandoned)
  - Target: <15% disconnect rate
  - Measure: Completed matches vs disconnects
  
- **Performance:**
  - Target: <200ms average WebSocket latency
  - Target: 99% uptime
  - Measure: Latency monitoring, server uptime

### 13.2 3-Month Post-Launch Metrics

- **User Growth:**
  - Target: 500-2,000 registered users
  - Target: 10-20% monthly growth rate
  
- **Retention:**
  - Target: 30% Day-7 retention
  - Target: 15% Day-30 retention
  
- **Competitive Health:**
  - Target: Elo distribution follows bell curve (healthy competition)
  - Target: Active leaderboard with top 100 players
  
- **Community:**
  - Target: 20+ active friend challenge users
  - Target: Positive user feedback (surveys, social media)

### 13.3 Qualitative Success Indicators

- Users report the game is "fun and addictive"
- Fair matchmaking (not too many blowouts)
- Positive word-of-mouth growth
- Community engagement (Discord, Reddit discussions)
- Minimal complaints about bugs or performance

---

## 14. Risk Assessment & Mitigation

### 14.1 Technical Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| WebSocket scaling issues | Medium | High | Horizontal scaling plan ready, Redis integration path clear |
| Database performance bottleneck | Low | Medium | Proper indexing, connection pooling, query optimization |
| Security vulnerability | Medium | High | Regular dependency updates, security audits, rate limiting |
| Critical bug in Elo calculation | Low | High | Thorough testing, unit tests, peer review |

### 14.2 Product Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Low user adoption | Medium | High | Soft launch, gather feedback, iterate quickly |
| Cheating/exploitation | Medium | Medium | Server authority, monitoring, community reporting |
| Poor matchmaking experience | Medium | Medium | Flexible Elo windows, show wait times, iterate algorithm |
| User churn due to difficulty | Low | Medium | Balanced Elo system, practice modes (future) |

### 14.3 Timeline Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| 3-month timeline slips | Medium | Low | Phased launch, MVP focus, cut non-essential features |
| Unexpected technical complexity | Medium | Medium | Time buffers in each phase, prioritize ruthlessly |
| Solo developer burnout | Low | Medium | Realistic scope, breaks, community support |

---

## 15. Conclusion

Vordle is a well-conceived competitive Wordle battle game with a clear vision, solid technical foundation, and realistic development plan. By focusing on V1 with Unlimited mode, server-authoritative gameplay, and a phased 3-month development timeline, the project is achievable as a solo developer effort.

**Key Strengths:**
- Clear product vision and target audience
- Strong technical architecture with scalability path
- Server authority ensures competitive integrity
- Phased development reduces risk
- Free, no-ads model removes monetization pressure

**Critical Success Factors:**
- Delivering smooth, low-latency real-time gameplay
- Fair and engaging Elo-based matchmaking
- Stable, bug-free launch experience
- Gathering and acting on beta tester feedback
- Maintaining focus on MVP scope

**Next Steps:**
1. Review and approve this masterplan
2. Set up development environment (Node.js, PostgreSQL, React)
3. Begin Phase 1: Core gameplay logic
4. Establish weekly development milestones
5. Track progress against 3-month timeline

With disciplined execution and a focus on delivering a polished core experience, Vordle has excellent potential to become a beloved competitive word game that brings the joy of Wordle to a real-time, head-to-head format.

**Good luck, and happy building! 🚀**

---

*This masterplan is a living document. As development progresses and you gather user feedback, feel free to adjust priorities, timelines, and features. The goal is to ship a great product, not to follow the plan perfectly.*