# Vordle V1 - Step-by-Step Task Plan

This document breaks down the Vordle V1 development into actionable tasks organized by phase. Each task is designed to be completable and testable independently.

---

## 📋 PHASE 1: Foundation & Core Gameplay (Weeks 1-3)

### Backend Setup & Structure
- [ ] Initialize Node.js project with npm/yarn
- [ ] Set up TypeScript configuration
- [ ] Install Express.js and core dependencies
- [ ] Create project folder structure (routes, controllers, services, utils)
- [ ] Set up environment variables (.env file)
- [ ] Configure ESLint and Prettier for code quality

### Word Lists & Game Logic
- [ ] Download/import Wordle answer word list (~2,300 words)
- [ ] Download/import Wordle valid guess dictionary (~12,000 words)
- [ ] Create word list loader service (load into memory on startup)
- [ ] Implement random target word selection function
- [ ] Implement word validation function (check if word exists in dictionary)
- [ ] Build tile pattern generation logic (green/yellow/gray)
- [ ] Handle repeated letter edge cases correctly
- [ ] Write unit tests for tile pattern generation
- [ ] Write unit tests for edge cases (BOOBY, ABBEY, etc.)

### Database Setup
- [ ] Install PostgreSQL locally
- [ ] Set up database migration tool (e.g., node-pg-migrate or Knex)
- [ ] Create `users` table schema
- [ ] Create `ratings` table schema
- [ ] Create `matches` table schema
- [ ] Create `match_guesses` table schema
- [ ] Create `challenge_links` table schema
- [ ] Add necessary indexes for performance
- [ ] Test database connection from Node.js

### Frontend Setup
- [ ] Initialize React project with Vite
- [ ] Install Tailwind CSS or CSS framework
- [ ] Set up TypeScript configuration for frontend
- [ ] Create basic folder structure (components, pages, hooks, utils)
- [ ] Set up routing (React Router)

### Wordle UI Components
- [ ] Build Wordle grid component (6 rows × 5 columns)
- [ ] Create tile component with states (empty, filled, correct, present, absent)
- [ ] Implement keyboard input handling (physical keyboard)
- [ ] Create on-screen keyboard component
- [ ] Add tile flip animations
- [ ] Add shake animation for invalid words
- [ ] Implement letter-by-letter typing
- [ ] Add backspace and enter key handling

### Single-Player Game Flow
- [ ] Create game state management (local state or context)
- [ ] Connect frontend to backend API for word validation
- [ ] Implement guess submission flow
- [ ] Display feedback after each guess (green/yellow/gray tiles)
- [ ] Track attempt count (X/6)
- [ ] Detect win condition (word solved)
- [ ] Detect loss condition (6 attempts used, word not solved)
- [ ] Show end game screen with results

### Testing & Validation
- [ ] Test single-player game thoroughly
- [ ] Verify all edge cases work correctly
- [ ] Test with various word combinations
- [ ] Ensure animations work smoothly
- [ ] Test on different screen sizes

**Deliverable:** Working single-player Wordle game with server validation

---

## 📋 PHASE 2: Real-Time Infrastructure (Weeks 4-5)

### WebSocket Setup
- [ ] Install Socket.IO on backend
- [ ] Integrate Socket.IO with Express server
- [ ] Set up Socket.IO client on frontend
- [ ] Create WebSocket event handler structure
- [ ] Implement basic connection/disconnection events
- [ ] Test WebSocket connection in browser console

### Match State Management
- [ ] Design in-memory match state data structure
- [ ] Create match state manager service
- [ ] Implement match creation logic
- [ ] Implement match state updates
- [ ] Add player state tracking (guesses, attempts, time)
- [ ] Create match cleanup on completion

### Real-Time Events - Backend
- [ ] Implement `authenticate` event handler
- [ ] Implement `submit_guess` event handler
- [ ] Implement `disconnect` event handler
- [ ] Create `match_found` event emitter
- [ ] Create `match_start` event emitter
- [ ] Create `opponent_guess` event emitter
- [ ] Create `guess_result` event emitter
- [ ] Create `match_end` event emitter
- [ ] Add error handling for all events

### Real-Time Events - Frontend
- [ ] Implement `authenticated` event listener
- [ ] Implement `match_found` event listener
- [ ] Implement `match_start` event listener
- [ ] Implement `opponent_guess` event listener
- [ ] Implement `guess_result` event listener
- [ ] Implement `match_end` event listener
- [ ] Handle WebSocket errors gracefully

### Opponent Board Display
- [ ] Create opponent grid component (color-only, no letters)
- [ ] Update opponent board on `opponent_guess` event
- [ ] Add real-time synchronization (100-200ms updates)
- [ ] Show opponent's current attempt count

### Timer System
- [ ] Implement server-side timer tracking (match start time)
- [ ] Calculate elapsed time for each player
- [ ] Send timer updates to frontend
- [ ] Display player timer on UI
- [ ] Display opponent timer on UI
- [ ] Format time display (MM:SS or SS.ms)

### Connection Status
- [ ] Add connection status indicator component
- [ ] Show "Connected" / "Disconnected" / "Reconnecting" states
- [ ] Handle Socket.IO auto-reconnection
- [ ] Display connection quality warnings

### Testing
- [ ] Test with 2 browser tabs (regular + incognito)
- [ ] Verify real-time guess synchronization
- [ ] Test simultaneous guess submissions
- [ ] Test connection/disconnection scenarios
- [ ] Verify timer accuracy across both clients

**Deliverable:** Two players can play against each other in real-time locally

---

## 📋 PHASE 3: Authentication & User Management (Week 6)

### Google OAuth Setup
- [ ] Create Google Cloud Console project
- [ ] Configure OAuth 2.0 credentials
- [ ] Set up authorized redirect URIs
- [ ] Install Passport.js and passport-google-oauth20
- [ ] Configure Google OAuth strategy

### Backend Authentication
- [ ] Create authentication routes (/auth/google, /auth/google/callback)
- [ ] Implement JWT generation function
- [ ] Implement JWT validation middleware
- [ ] Store JWT in httpOnly cookie
- [ ] Create user profile endpoint (/api/user/me)
- [ ] Handle new user registration (create user + default rating)
- [ ] Handle existing user login
- [ ] Add WebSocket JWT authentication

### Frontend Authentication
- [ ] Create login page with Google sign-in button
- [ ] Implement Google OAuth redirect flow
- [ ] Store authentication state in React context
- [ ] Create logout functionality
- [ ] Implement protected route wrapper
- [ ] Redirect unauthenticated users to login

### User Profile
- [ ] Create user profile component
- [ ] Display Google username and avatar
- [ ] Show current Elo rating per mode
- [ ] Add profile navigation

### Database User Management
- [ ] Seed test user data for development
- [ ] Create default rating records for new users (1200 Elo)
- [ ] Test user creation on first login
- [ ] Verify rating records are created

### Testing
- [ ] Test Google login flow end-to-end
- [ ] Verify JWT is stored correctly
- [ ] Test logout and re-login
- [ ] Test protected routes (redirect when not authenticated)
- [ ] Test WebSocket authentication with JWT

**Deliverable:** Users can sign in with Google and have persistent profiles

---

## 📋 PHASE 4: Matchmaking System (Week 7)

### Backend Matchmaking Queue
- [ ] Create in-memory matchmaking queue data structure
- [ ] Implement queue join logic
- [ ] Implement queue leave logic
- [ ] Track player Elo rating in queue
- [ ] Track queue join timestamp

### Elo-Based Pairing Algorithm
- [ ] Implement initial Elo window search (±50 points)
- [ ] Implement search window expansion (+50 every 10 seconds)
- [ ] Create matching function (find compatible opponent)
- [ ] Handle match creation when pair found
- [ ] Remove matched players from queue

### Queue Timeout & No Match
- [ ] Implement 2-minute maximum wait timer
- [ ] Send "no matches found" event on timeout
- [ ] Remove player from queue on timeout
- [ ] Allow player to re-queue after timeout

### Friend Challenge System
- [ ] Create challenge link generation endpoint
- [ ] Generate unique challenge tokens (UUID)
- [ ] Store challenge in database (creator, mode, expiry, ranked flag)
- [ ] Set 60-minute expiry for challenges
- [ ] Create challenge validation endpoint
- [ ] Implement challenge acceptance flow
- [ ] Mark challenge as used after first acceptance
- [ ] Create match directly from challenge (bypass queue)

### Frontend Matchmaking UI
- [ ] Create home screen layout
- [ ] Add "Find Match" button
- [ ] Show matchmaking queue status (Searching... X seconds)
- [ ] Display current Elo rating on home screen
- [ ] Show "No matches found" message on timeout
- [ ] Add "Cancel Search" button

### Frontend Friend Challenge UI
- [ ] Add "Challenge Friend" button on home screen
- [ ] Create challenge link modal
- [ ] Display shareable challenge URL
- [ ] Add "Copy Link" button
- [ ] Show challenge expiry time
- [ ] Handle challenge acceptance page
- [ ] Show challenge creator info before accepting

### WebSocket Integration
- [ ] Implement `join_queue` event handler
- [ ] Implement `leave_queue` event handler
- [ ] Send `match_found` event with opponent info
- [ ] Handle queue position updates
- [ ] Handle queue timeout events

### Testing
- [ ] Test matchmaking with multiple test accounts
- [ ] Verify Elo-based pairing works correctly
- [ ] Test search window expansion
- [ ] Test 2-minute timeout
- [ ] Test friend challenge generation
- [ ] Test friend challenge acceptance
- [ ] Test challenge expiry (60 minutes)
- [ ] Test challenge single-use enforcement

**Deliverable:** Players can find matches automatically or challenge friends

---

## 📋 PHASE 5: Elo Ratings & Leaderboard (Week 8)

### Elo Calculation System
- [ ] Implement standard Elo calculation function
- [ ] Set K-factor based on rating stability
- [ ] Calculate expected score for both players
- [ ] Determine actual score based on match outcome
- [ ] Calculate rating changes (+/- points)
- [ ] Handle draw scenarios (same attempts, same time)

### Rating Updates
- [ ] Create rating update service
- [ ] Update player ratings after match completion
- [ ] Store rating changes in match results
- [ ] Use database transactions for atomic updates
- [ ] Handle errors gracefully (rollback on failure)

### Leaderboard Backend
- [ ] Create leaderboard query (top 100 per mode)
- [ ] Sort by rating descending
- [ ] Include rank, username, rating, wins, losses
- [ ] Optimize query with proper indexes
- [ ] Create leaderboard API endpoint (/api/leaderboard/:mode)
- [ ] Add caching for leaderboard (optional, 1-minute cache)

### Leaderboard Frontend
- [ ] Create leaderboard page component
- [ ] Display mode selector (Unlimited only for V1)
- [ ] Render leaderboard table (rank, username, rating, W/L)
- [ ] Add loading state while fetching
- [ ] Add refresh button
- [ ] Style leaderboard with competitive aesthetic

### Rating Display
- [ ] Show current Elo rating on home screen
- [ ] Display both players' ratings during match
- [ ] Show rating change on post-match screen (+/- XX points)
- [ ] Animate rating change (count-up/count-down effect)
- [ ] Color-code rating change (green for gain, red for loss)

### Testing
- [ ] Test Elo calculations manually (known scenarios)
- [ ] Verify rating updates are atomic (no partial updates)
- [ ] Test leaderboard query performance
- [ ] Ensure leaderboard updates after matches
- [ ] Test rating display across all screens
- [ ] Verify win/loss/draw counts update correctly

**Deliverable:** Fully functional competitive rating system with leaderboard

---

## 📋 PHASE 6: Post-Match & Polish (Week 9)

### Match Results Storage
- [ ] Implement match result saving to database
- [ ] Store all match data (players, attempts, times, winner, rating changes)
- [ ] Store guess history in match_guesses table
- [ ] Include outcome type (win/loss/draw/timeout/disconnect)
- [ ] Add timestamps (started_at, ended_at)

### Post-Match Screen
- [ ] Create post-match summary component
- [ ] Display winner/loser declaration
- [ ] Show rating change for both players
- [ ] Display attempts comparison (Player 1: 3/6, Player 2: 4/6)
- [ ] Show time taken by each player
- [ ] Add visual styling for victory/defeat

### Rematch System
- [ ] Create rematch request logic
- [ ] Implement `request_rematch` event handler
- [ ] Implement `accept_rematch` event handler
- [ ] Create rematch acceptance flow
- [ ] Generate new target word for rematch
- [ ] Reset match state while maintaining mode
- [ ] Add rematch timeout (30 seconds to accept)

### Rematch UI
- [ ] Add "Request Rematch" button on post-match screen
- [ ] Show "Waiting for opponent..." when request sent
- [ ] Show "Opponent requested rematch" with Accept/Decline buttons
- [ ] Handle rematch acceptance (start new match)
- [ ] Handle rematch decline (return to home)
- [ ] Add "Return to Home" button

### Disconnection Handling
- [ ] Implement 30-second grace period on disconnect
- [ ] Pause match timer during disconnection
- [ ] Send `opponent_disconnected` event to connected player
- [ ] Start grace period countdown on frontend
- [ ] Handle reconnection within grace period
- [ ] Send `opponent_reconnected` event
- [ ] Restore match state on reconnection
- [ ] Declare loser if grace period expires
- [ ] Apply Elo penalty for disconnection loss

### UI/UX Polish
- [ ] Refine visual design across all screens
- [ ] Ensure consistent color scheme
- [ ] Polish animations (smooth transitions)
- [ ] Add loading states for all async operations
- [ ] Improve error messages (user-friendly)
- [ ] Add sound effects (optional: win, loss, guess submission)
- [ ] Test mobile responsiveness (all screens)
- [ ] Optimize for touch input on mobile

### Error Handling
- [ ] Handle invalid guess submissions gracefully
- [ ] Display user-friendly error messages
- [ ] Handle WebSocket disconnections
- [ ] Recover from temporary server issues
- [ ] Add retry logic for failed API calls

**Deliverable:** Polished end-to-end match experience with rematch capability

---

## 📋 PHASE 7: Testing & Bug Fixes (Week 10)

### Functional Testing
- [ ] Test complete match flow (queue → match → result)
- [ ] Test friend challenge flow end-to-end
- [ ] Test rematch acceptance/decline
- [ ] Test disconnection and reconnection scenarios
- [ ] Test grace period expiry
- [ ] Test simultaneous actions (both players submit at once)
- [ ] Test edge cases (win on last attempt, both solve in 1 guess)

### Cross-Browser Testing
- [ ] Test on Chrome (desktop)
- [ ] Test on Firefox (desktop)
- [ ] Test on Safari (desktop)
- [ ] Test on Edge (desktop)
- [ ] Test on iOS Safari (mobile)
- [ ] Test on Android Chrome (mobile)
- [ ] Fix any browser-specific issues

### Mobile Responsiveness
- [ ] Test all screens on mobile devices
- [ ] Verify touch input works correctly
- [ ] Test landscape and portrait modes
- [ ] Ensure on-screen keyboard doesn't obscure game
- [ ] Test on small screens (iPhone SE, etc.)
- [ ] Test on large screens (iPad, etc.)

### Performance Testing
- [ ] Simulate 10 concurrent matches
- [ ] Simulate 50 concurrent matches
- [ ] Simulate 100 concurrent matches
- [ ] Monitor server CPU and memory usage
- [ ] Monitor database query performance
- [ ] Identify and fix bottlenecks
- [ ] Optimize slow queries

### Security Testing
- [ ] Test JWT validation (invalid tokens)
- [ ] Test rate limiting on API endpoints
- [ ] Test WebSocket authentication
- [ ] Verify server authority (can't cheat by modifying client)
- [ ] Test SQL injection protection
- [ ] Test XSS protection

### Bug Fixes
- [ ] Create bug tracking system (GitHub Issues or similar)
- [ ] Prioritize bugs by severity
- [ ] Fix critical bugs (blocking launch)
- [ ] Fix high-priority bugs (major issues)
- [ ] Fix medium-priority bugs (minor issues)
- [ ] Document known low-priority bugs for post-launch

### Code Quality
- [ ] Run ESLint and fix linting errors
- [ ] Review code for potential improvements
- [ ] Add comments to complex logic
- [ ] Remove console.logs and debug code
- [ ] Ensure consistent code style

**Deliverable:** Stable, well-tested application ready for beta

---

## 📋 PHASE 8: Deployment & Beta Testing (Weeks 11-12)

### Deployment Preparation
- [ ] Choose hosting platform (Railway.app or Render.com)
- [ ] Create production environment variables
- [ ] Set up managed PostgreSQL database
- [ ] Configure database connection pooling
- [ ] Set up SSL certificates (automatic via platform)

### Dockerization (Optional but Recommended)
- [ ] Create Dockerfile for backend
- [ ] Create Dockerfile for frontend
- [ ] Create docker-compose.yml for local testing
- [ ] Test Docker builds locally
- [ ] Optimize Docker image sizes

### Backend Deployment
- [ ] Create Railway/Render account
- [ ] Connect GitHub repository
- [ ] Configure build settings
- [ ] Set environment variables
- [ ] Deploy backend to production
- [ ] Verify deployment successful
- [ ] Test API endpoints in production

### Database Setup
- [ ] Create production database instance
- [ ] Run database migrations
- [ ] Verify tables created correctly
- [ ] Test database connection from deployed backend
- [ ] Set up database backups

### Frontend Deployment
- [ ] Build optimized production frontend
- [ ] Deploy frontend (same platform or CDN)
- [ ] Configure environment variables (API URL, etc.)
- [ ] Test frontend in production
- [ ] Verify WebSocket connection works

### Domain Setup (Optional for Beta)
- [ ] Purchase domain name (e.g., vordle.io)
- [ ] Configure DNS settings
- [ ] Point domain to hosting platform
- [ ] Verify domain works with HTTPS

### Monitoring & Logging
- [ ] Set up error logging (e.g., Sentry free tier)
- [ ] Add basic analytics tracking
- [ ] Monitor server uptime
- [ ] Set up alerts for critical errors
- [ ] Create admin dashboard access

### Beta Testing
- [ ] Invite 10-20 friends to beta test
- [ ] Create feedback form (Google Form)
- [ ] Set up Discord or Slack for feedback
- [ ] Monitor beta testers' matches
- [ ] Track match completion rates
- [ ] Track disconnect rates
- [ ] Identify common issues

### Analytics Tracking
- [ ] Track daily active users
- [ ] Track matches played per day
- [ ] Track average match duration
- [ ] Track match completion vs disconnection rate
- [ ] Track most active time periods
- [ ] Create analytics dashboard

### Iterate on Feedback
- [ ] Collect beta tester feedback
- [ ] Prioritize feedback items
- [ ] Fix critical issues immediately
- [ ] Implement high-value improvements
- [ ] Test fixes with beta testers
- [ ] Prepare for public launch

**Deliverable:** Live beta version with real users and feedback loop

---

## 📋 PHASE 9: Launch Preparation (Week 12)

### Final Polish
- [ ] Implement final beta tester feedback
- [ ] Fix remaining bugs from beta testing
- [ ] Optimize performance based on production data
- [ ] Review all UI/UX for consistency
- [ ] Final cross-browser and mobile testing

### Documentation
- [ ] Write user guide / How to Play
- [ ] Create FAQ page
- [ ] Document basic troubleshooting
- [ ] Add about page with project info
- [ ] Create privacy policy (basic)
- [ ] Create terms of service (basic)

### Admin Tools
- [ ] Create basic admin dashboard
- [ ] Add user management tools
- [ ] Add match monitoring tools
- [ ] Add ability to view system stats
- [ ] Create tools for handling abuse reports (future use)

### Monitoring & Alerts
- [ ] Set up uptime monitoring
- [ ] Configure error alerts (email/Slack)
- [ ] Set up performance monitoring
- [ ] Create dashboard for key metrics
- [ ] Test alert system

### Final Testing
- [ ] Complete end-to-end test of entire application
- [ ] Test with fresh user accounts
- [ ] Verify all features work in production
- [ ] Test under simulated load
- [ ] Final security check

### Launch Preparation
- [ ] Write launch announcement
- [ ] Prepare social media posts
- [ ] Create screenshots/GIFs for sharing
- [ ] Set up social media accounts (optional)
- [ ] Prepare Reddit/HN posts

### Marketing (Optional)
- [ ] Post on r/wordle
- [ ] Post on r/websitereview
- [ ] Post on Hacker News (Show HN)
- [ ] Share on Twitter/X
- [ ] Share in relevant Discord communities
- [ ] Send to friends and family

### Launch Day
- [ ] Final deployment check
- [ ] Monitor server closely
- [ ] Be ready to respond to issues
- [ ] Engage with early users
- [ ] Collect initial feedback
- [ ] Celebrate! 🎉

**Deliverable:** Public V1 launch of Vordle!

---

## 🎯 Post-V1: Future Enhancements (V1.1+)

### Match History (V1.1)
- [ ] Create match history page
- [ ] Display past matches with details
- [ ] Add filtering (by mode, date, opponent)
- [ ] Show guess-by-guess details
- [ ] Add pagination for long history

### Timed Mode (V2)
- [ ] Implement 10-minute countdown timer
- [ ] Add timer UI with countdown
- [ ] Implement timeout logic
- [ ] Test timer accuracy
- [ ] Launch as second mode

### Limited Mode (V3)
- [ ] Implement 6-attempt limited mode
- [ ] Add tie-breaking logic
- [ ] Test thoroughly
- [ ] Launch as third mode

---

## 📊 Success Metrics to Track

### Launch Metrics (First Month)
- [ ] 100-500 registered users
- [ ] 50-200 daily active users
- [ ] Average 3-5 matches per user per session
- [ ] >85% match completion rate
- [ ] <15% disconnect rate
- [ ] <200ms average WebSocket latency
- [ ] 99% uptime

### 3-Month Post-Launch Metrics
- [ ] 500-2,000 registered users
- [ ] 10-20% monthly growth rate
- [ ] 30% Day-7 retention
- [ ] 15% Day-30 retention
- [ ] Healthy Elo distribution (bell curve)
- [ ] Active leaderboard with top 100 players
- [ ] Positive user feedback

---

## ✅ Critical Success Factors

- [ ] Smooth, low-latency real-time gameplay (<200ms)
- [ ] Fair and engaging Elo-based matchmaking
- [ ] Stable, bug-free launch experience
- [ ] Positive beta tester feedback
- [ ] Maintained focus on MVP scope (no feature creep)
- [ ] Server authority ensuring fair play
- [ ] Graceful handling of disconnections
- [ ] Mobile-friendly responsive design
- [ ] Clear, helpful error messages
- [ ] Engaging UI/UX that feels polished

---

## 🚀 Getting Started

1. **Phase 1:** Set up development environment and build core Wordle game
2. **Phase 2:** Add real-time multiplayer capabilities
3. **Phase 3:** Implement Google authentication
4. **Phase 4:** Build matchmaking system
5. **Phase 5:** Add Elo ratings and leaderboard
6. **Phase 6:** Polish UI/UX and add rematch
7. **Phase 7:** Comprehensive testing and bug fixes
8. **Phase 8:** Deploy and beta test
9. **Phase 9:** Launch preparation and public release

**Remember:** This is an ambitious but achievable 3-month plan. Stay focused on the MVP, cut scope aggressively if needed, and prioritize a polished core experience over extra features.

Good luck building Vordle! 🚀
